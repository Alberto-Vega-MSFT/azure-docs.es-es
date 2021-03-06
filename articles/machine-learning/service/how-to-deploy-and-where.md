---
title: Implementación de modelos como servicios web
titleSuffix: Azure Machine Learning service
description: 'Obtenga información sobre cómo y dónde implementar los modelos de Azure Machine Learning Service, incluidos: Azure Container Instances, Azure Kubernetes Service, Azure IoT Edge y matrices de puertas programables.'
services: machine-learning
ms.service: machine-learning
ms.component: core
ms.topic: conceptual
ms.author: aashishb
author: aashishb
ms.reviewer: larryfr
ms.date: 12/07/2018
ms.custom: seodec18
ms.openlocfilehash: 0c171ff768395540c123c4ef2a19168d926b0661
ms.sourcegitcommit: c94cf3840db42f099b4dc858cd0c77c4e3e4c436
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/19/2018
ms.locfileid: "53633834"
---
# <a name="deploy-models-with-the-azure-machine-learning-service"></a>Implementación de modelos con el servicio Azure Machine Learning

Azure Machine Learning Service ofrece varios métodos que puede utilizar para implementar el modelo entrenado mediante el SDK. En este documento, aprenderá a implementar el modelo como un servicio web en la nube de Azure o en dispositivos IoT Edge.

Puede implementar modelos en los destinos de proceso siguientes:

| Destino de proceso | Tipo de implementación | DESCRIPCIÓN |
| ----- | ----- | ----- |
| [Azure Container Instances (ACI)](#aci) | Servicio web | Implementación rápida. Se recomienda para desarrollo o pruebas. |
| [Azure Kubernetes Service (AKS)](#aks) | Servicio web | Es útil para las implementaciones de producción a gran escala. Proporciona escalado automático y tiempos de respuesta rápidos. |
| [Azure IoT Edge](#iotedge) | Módulo de IoT | Implementa modelos en dispositivos IoT. Se produce inferencia en el dispositivo. |
| [Matriz de puertas programables (FPGA)](#fpga) | Servicio web | Latencia ultrabaja para inferencia en tiempo real. |

> [!VIDEO https://www.microsoft.com/videoplayer/embed/RE2Kwk3]

## <a name="prerequisites"></a>Requisitos previos

- Un área de trabajo del servicio Azure Machine Learning y el SDK de Azure Machine Learning para Python instalado. Obtenga información sobre cómo cumplir estos requisitos previos mediante la [guía de inicio rápido de introducción de Azure Machine Learning](quickstart-get-started.md).

- Modelo entrenado en formato pickle (`.pkl`) u ONNX (`.onnx`). Si no tiene un modelo entrenado, siga los pasos del tutorial [Entrenamiento de un modelo](tutorial-train-models-with-aml.md) para entrenar y registrar uno con Azure Machine Learning Service.

- Las secciones de código suponen que `ws` hace referencia a su área de trabajo de Machine Learning. Por ejemplo, `ws = Workspace.from_config()`.

## <a name="deployment-workflow"></a>Flujo de trabajo de implementación

El proceso de implementación de un modelo es similar para todos los destinos de proceso:

1. Entrene un modelo.
1. Registre el modelo.
1. Cree la configuración de una imagen.
1. Cree la imagen.
1. Implemente la imagen en un destino de proceso.
1. Prueba de la implementación
1. (Opcional) Limpiar artefactos.

    * Al realizar la **implementación como un servicio web**, existen tres opciones de implementación:

        * [deploy](https://docs.microsoft.com/python/api/azureml-core/azureml.core.webservice(class)?view=azure-ml-py#deploy-workspace--name--model-paths--image-config--deployment-config-none--deployment-target-none-): cuando se usa este método, no es necesario registrar el modelo ni crear la imagen. Sin embargo, no puede controlar el nombre del modelo o la imagen, ni las etiquetas y descripciones asociadas.
        * [deploy_from_model](https://docs.microsoft.com/python/api/azureml-core/azureml.core.webservice(class)?view=azure-ml-py#deploy-from-model-workspace--name--models--image-config--deployment-config-none--deployment-target-none-): cuando se usa este método, no es necesario crear una imagen. No obstante, no tiene control sobre el nombre de la imagen que se crea.
        * [deploy_from_image](https://docs.microsoft.com/python/api/azureml-core/azureml.core.webservice(class)?view=azure-ml-py#deploy-from-image-workspace--name--image--deployment-config-none--deployment-target-none-): registre el modelo y cree una imagen antes de usar este método.

        Los ejemplos de este documento usan `deploy_from_image`.

    * Para la **implementación como un módulo IoT Edge**, debe registrar el modelo y crear la imagen.

## <a name="register-a-model"></a>Registrar un modelo

Solo se pueden implementar los modelos entrenados. El modelo se puede entrenar con Azure Machine Learning u otro servicio. Para registrar un modelo desde un archivo, use el código siguiente:

```python
from azureml.core.model import Model

model = Model.register(model_path = "model.pkl",
                       model_name = "Mymodel",
                       tags = {"key": "0.1"},
                       description = "test",
                       workspace = ws)
```

> [!NOTE]
> Aunque en el ejemplo se muestra el uso de un modelo almacenado como un archivo pickle, también puede usados modelos ONNX. Para obtener más información sobre el uso de modelos ONNX, consulte el documento [ONNX y Azure Machine Learning](how-to-build-deploy-onnx.md).

Para más información, consulte la documentación de referencia de la [clase Model](https://docs.microsoft.com/python/api/azureml-core/azureml.core.model.model?view=azure-ml-py).

## <a id="configureimage"></a> Creación de una configuración de imagen

Los modelos implementados se empaquetan como una imagen. La imagen contiene las dependencias necesarias para ejecutar el modelo.

Para las implementaciones de **Azure Container Instance**, **Azure Kubernetes Service** y **Azure IoT Edge**, se usa la clase `azureml.core.image.ContainerImage` para crear una configuración de imagen. La configuración de imagen se usa posteriormente para crear una nueva imagen de Docker. 

El código siguiente muestra cómo crear una nueva configuración de imagen:

```python
from azureml.core.image import ContainerImage

# Image configuration
image_config = ContainerImage.image_configuration(execution_script = "score.py",
                                                 runtime = "python",
                                                 conda_file = "myenv.yml",
                                                 description = "Image with ridge regression model",
                                                 tags = {"data": "diabetes", "type": "regression"}
                                                 )
```

Esta configuración utiliza un archivo `score.py` para pasar la solicitudes al modelo. Este archivo contiene dos funciones:

* `init()`: normalmente, esta función carga el modelo en un objeto global. Esta función solo se ejecuta una vez cuando se inicia el contenedor de Docker. 

* `run(input_data)`: esta función usa el modelo para predecir un valor basado en los datos de entrada. Los datos de entrada y salida de la ejecución suelen usan el formato JSON para la serialización y deserialización, pero se admiten otros formatos.

Para obtener un archivo `score.py` de ejemplo, consulte el [tutorial de clasificación de imágenes](tutorial-deploy-models-with-aml.md#make-script). Para obtener un ejemplo que use un modelo ONNX, consulte el documento [ONNX y Azure Machine Learning](how-to-build-deploy-onnx.md).

El parámetro `conda_file` se usa para proporcionar un archivo de entorno conda. Este archivo define el entorno conda para el modelo implementado. Para obtener más información sobre cómo crear este archivo, consulte [Creación del archivo de entorno (myenv.yml)](tutorial-deploy-models-with-aml.md#create-environment-file).

Para más información, consulte la documentación de referencia de la [clase ContainerImage](https://docs.microsoft.com/python/api/azureml-core/azureml.core.image.containerimage?view=azure-ml-py).

## <a id="createimage"></a> Creación de la imagen

Una vez haya creado la configuración de imagen, puede usarla para crear una imagen. Esta imagen se almacena en el registro de contenedor de su área de trabajo. Una vez creada, puede implementar la misma imagen en varios servicios.

```python
# Create the image from the image configuration
image = ContainerImage.create(name = "myimage", 
                              models = [model], #this is the model object
                              image_config = image_config,
                              workspace = ws
                              )
```

**Tiempo estimado**: aproximadamente 3 minutos.

Las imágenes se versionan automáticamente al registrar varias imágenes con el mismo nombre. Por ejemplo, a la primera imagen registrada como `myimage` se le asigna un identificador de `myimage:1`. La próxima vez registre una imagen como `myimage`, el identificador de la nueva imagen será `myimage:2`.

Para más información, consulte la documentación de referencia de la [clase ContainerImage](https://docs.microsoft.com/python/api/azureml-core/azureml.core.image.containerimage?view=azure-ml-py).

## <a name="deploy-the-image"></a>Implementación de la imagen

Al llegar a la implementación, el proceso es ligeramente diferente dependiendo del destino de proceso donde se realiza implementación. Use la información de las secciones siguientes para aprender a realizar la implementación en los destinos siguientes:

* [Azure Container Instances](#aci)
* [Azure Kubernetes Services](#aks)
* [Project Brainwave (matrices de puertas programables)](#fpga)
* [Dispositivos de Azure IoT Edge](#iotedge)

### <a id="aci"></a> Implementación en Azure Container Instances

Utilice Azure Container Instances para implementar los modelos como un servicio web si se dan una o varias de las siguientes condiciones:

- Debe implementar y validar rápidamente el modelo. La implementación de ACI finaliza en menos de 5 minutos.
- Está probando un modelo que está en desarrollo. Para ver la disponibilidad de cuotas y regiones de ACI, consulte el documento [Disponibilidad de cuotas y regiones en Azure Container Instances](https://docs.microsoft.com/azure/container-instances/container-instances-quotas).

Para realizar la implementación en Azure Container Instances, siga estos pasos:

1. Defina la configuración de la implementación. En el ejemplo siguiente se define una configuración que usa un núcleo de CPU y 1 GB de memoria:

    [!code-python[](~/aml-sdk-samples/ignore/doc-qa/how-to-deploy-to-aci/how-to-deploy-to-aci.py?name=configAci)]

2. Para implementar la imagen creada en la sección [Crear la imagen](#createimage) de este documento, use el código siguiente:

    [!code-python[](~/aml-sdk-samples/ignore/doc-qa/how-to-deploy-to-aci/how-to-deploy-to-aci.py?name=option3Deploy)]

    **Tiempo estimado**: aproximadamente 3 minutos.

    > [!TIP]
    > Si hay errores durante la implementación, use `service.get_logs()` para ver los registros de servicio de AKS. La información registrada puede indicar la causa del error.

Para más información, consulte la documentación de referencia de las clases [AciWebservice](https://docs.microsoft.com/python/api/azureml-core/azureml.core.webservice.aciwebservice?view=azure-ml-py) y [Webservice](https://docs.microsoft.com/python/api/azureml-core/azureml.core.webservice.webservice?view=azure-ml-py).

### <a id="aks"></a> Implementación en Azure Kubernetes Service

Para implementar el modelo como un servicio web de producción a gran escala, use Azure Kubernetes Service (AKS). Puede usar un clúster AKS existente o crear uno nuevo con el SDK de Azure Machine Learning, la CLI o Azure Portal.

Crear un clúster de AKS es un proceso único en el área de trabajo. Puede volver a usar este clúster con diferentes implementaciones. Si elimina el clúster, deberá crear uno nuevo la próxima vez que necesite implementar.

Azure Kubernetes Service proporciona las siguientes funcionalidades:

* Escalado automático
* Registro
* Recopilación de datos de modelos
* Tiempos de respuesta rápidos para los servicios web

Para implementar Azure Kubernetes Service, utilice los pasos siguientes:

1. Para crear un clúster de AKS, utilice el código siguiente:

    > [!IMPORTANT]
    > Crear el clúster de AKS es un proceso único para el área de trabajo. Una vez creado, puede volver a usar este clúster para varias implementaciones. Si elimina el clúster o el grupo de recursos que lo contiene, tendrá que crear un nuevo clúster la próxima vez que tenga que realizar una implementación.
    > Para [`provisioning_configuration()`](https://docs.microsoft.com/python/api/azureml-core/azureml.core.compute.akscompute?view=azure-ml-py), si elige valores personalizados para agent_count y vm_size, deberá asegurarse de que agent_count multiplicado por vm_size sea mayor o igual que 12 CPU virtuales. Por ejemplo, si usa un valor de vm_size de "Standard_D3_v2", que tiene 4 CPU virtuales, debe elegir un valor de agent_count de 3 o mayor.

    ```python
    from azureml.core.compute import AksCompute, ComputeTarget

    # Use the default configuration (you can also provide parameters to customize this)
    prov_config = AksCompute.provisioning_configuration()

    aks_name = 'aml-aks-1' 
    # Create the cluster
    aks_target = ComputeTarget.create(workspace = ws, 
                                        name = aks_name, 
                                        provisioning_configuration = prov_config)

    # Wait for the create process to complete
    aks_target.wait_for_completion(show_output = True)
    print(aks_target.provisioning_state)
    print(aks_target.provisioning_errors)
    ```

    **Tiempo estimado**: aproximadamente 20 minutos.

    > [!TIP]
    > Si ya tiene un clúster de AKS en su suscripción a Azure y es de la versión 1.11.* puede usarlo para implementar la imagen. El código siguiente muestra cómo conectar un clúster existente al área de trabajo:
    >
    > ```python
    > from azureml.core.compute import AksCompute, ComputeTarget
    > # Set the resource group that contains the AKS cluster and the cluster name
    > resource_group = 'myresourcegroup'
    > cluster_name = 'mycluster'
    > 
    > # Attatch the cluster to your workgroup
    > attach_config = AksCompute.attach_configuration(resource_group = resource_group,
    >                                          cluster_name = cluster_name)
    > aks_target = ComputeTarget.attach(ws, 'mycompute', attach_config)
    > 
    > # Wait for the operation to complete
    > aks_target.wait_for_completion(True)
    > ```

2. Para implementar la imagen creada en la sección [Crear la imagen](#createimage) de este documento, use el código siguiente:

    ```python
    from azureml.core.webservice import Webservice, AksWebservice

    # Set configuration and service name
    aks_config = AksWebservice.deploy_configuration()
    aks_service_name ='aks-service-1'
    # Deploy from image
    service = Webservice.deploy_from_image(workspace = ws, 
                                                name = aks_service_name,
                                                image = image,
                                                deployment_config = aks_config,
                                                deployment_target = aks_target)
    # Wait for the deployment to complete
    service.wait_for_deployment(show_output = True)
    print(service.state)
    ```

    > [!TIP]
    > Si hay errores durante la implementación, use `service.get_logs()` para ver los registros de servicio de AKS. La información registrada puede indicar la causa del error.

Para más información, consulte la documentación de referencia de las clases [AksWebservice](https://docs.microsoft.com/python/api/azureml-core/azureml.core.webservice.akswebservice?view=azure-ml-py) y [Webservice](https://docs.microsoft.com/python/api/azureml-core/azureml.core.webservice.webservice(class)?view=azure-ml-py).

### <a id="fpga"></a> Implementación en matrices de puertas programables (FPGA)

Project Brainwave permite conseguir una latencia muy baja en solicitudes de inferencia en tiempo real. Project Brainwave acelera las redes neurales profundas (DNN) implementadas en matrices de puertas programables por campo en la nube de Azure. Las DNN que se utilizan habitualmente están disponibles en forma de características para transferir conocimientos o pueden personalizarse con ponderaciones entrenadas con sus propios datos.

Para ver un tutorial sobre la implementación de un modelo con Project Brainwave, consulte el documento sobre la [implementación en una matriz FPGA](how-to-deploy-fpga-web-service.md).

### <a id="iotedge"></a> Implementación en Azure IoT Edge

Un dispositivo Azure IoT Edge es un dispositivo basado en Linux o Windows que ejecuta el entorno de ejecución de Azure IoT Edge. En estos dispositivos pueden implementarse modelos de aprendizaje automático como módulos IoT Edge. Implementar un modelo en un dispositivo IoT Edge permite que el dispositivo use el modelo directamente, en lugar de tener que enviar datos a la nube para su procesamiento. Obtendrá tiempos de respuesta más rápidos y una menor transferencia de datos.

Los módulos de Azure IoT Edge se implementan en el dispositivo desde un registro de contenedor. Cuando se crea una imagen desde el modelo, se almacena en el registro de contenedor del área de trabajo.

#### <a name="set-up-your-environment"></a>Configuración del entorno

* Un entorno de desarrollo. Para obtener más información, consulte el documento sobre [cómo configurar un entorno de desarrollo](how-to-configure-environment.md).

* Una instancia de [Azure IoT Hub](../../iot-hub/iot-hub-create-through-portal.md) en la suscripción de Azure. 

* Un modelo entrenado. Para obtener un ejemplo de entrenamiento de un modelo, consulte el documento [Train an image classification model with Azure Machine Learning](tutorial-train-models-with-aml.md) (Entrenamiento de un modelo de clasificación de imágenes con Azure Machine Learning). Hay disponible un modelo previamente entrenado en el [Kit de herramientas de AI para el repositorio de Azure IoT Edge GitHub](https://github.com/Azure/ai-toolkit-iot-edge/tree/master/IoT%20Edge%20anomaly%20detection%20tutorial).

#### <a name="prepare-the-iot-device"></a>Preparación del dispositivo IoT
Debe crear un IoT Hub y registrar un dispositivo o reutilizar uno que ya tenga con [este script](https://raw.githubusercontent.com/Azure/ai-toolkit-iot-edge/master/amliotedge/createNregister).

``` bash
ssh <yourusername>@<yourdeviceip>
sudo wget https://raw.githubusercontent.com/Azure/ai-toolkit-iot-edge/master/amliotedge/createNregister
sudo chmod +x createNregister
sudo ./createNregister <The Azure subscriptionID you want to use> <Resourcegroup to use or create for the IoT hub> <Azure location to use e.g. eastus2> <the Hub ID you want to use or create> <the device ID you want to create>
```

Guarde la cadena de conexión resultante después de "cs":"{copy this string}".

Inicialice el dispositivo mediante la descarga de [este script](https://raw.githubusercontent.com/Azure/ai-toolkit-iot-edge/master/amliotedge/installIoTEdge) en un nodo perimetral de IoT UbuntuX64 o DSVM para ejecutar los comandos siguientes:

```bash
ssh <yourusername>@<yourdeviceip>
sudo wget https://raw.githubusercontent.com/Azure/ai-toolkit-iot-edge/master/amliotedge/installIoTEdge
sudo chmod +x installIoTEdge
sudo ./installIoTEdge
```

El nodo IoT Edge está listo para recibir la cadena de conexión de IoT Hub. Busque la línea ```device_connection_string:``` y pegue la cadena de conexión anterior entre las comillas.

También puede aprender a registrar el dispositivo e instalar el runtime de IoT paso a paso. Para ello, siga las instrucciones del documento [Inicio rápido: Implementación del primer módulo de IoT Edge en un dispositivo Linux x64](../../iot-edge/quickstart-linux.md).


#### <a name="get-the-container-registry-credentials"></a>Obtención de las credenciales del registro de contenedor
Para implementar un módulo de IoT Edge en el dispositivo, Azure IoT necesita las credenciales para el registro de contenedor que almacena las imágenes de Docker en Azure Machine Learning Service.

Puede recuperar fácilmente las credenciales del registro de contenedor necesarias de dos maneras:

+ **En Azure Portal**:

  1. Inicie sesión en el [Azure Portal](https://portal.azure.com/signin/index).

  1. Vaya al área de trabajo del servicio Azure Machine Learning y seleccione __Introducción__. Para ir a la configuración del registro de contenedor, seleccione el vínculo __Registro__.

     ![Imagen de la entrada en el registro de contenedor](./media/how-to-deploy-and-where/findregisteredcontainer.png)

  1. Una vez en el registro de contenedor, seleccione **Claves de acceso** y habilite el usuario administrador.
 
     ![Imagen de la pantalla de claves de acceso](./media/how-to-deploy-and-where/findaccesskey.png)

  1. Guarde los valores de **servidor de inicio de sesión**, **nombre de usuario** y **contraseña**. 

+ **Con un script de Python**:

  1. Use el siguiente script de Python después del código ejecutado anteriormente para crear un contenedor:

     ```python
     # Getting your container details
     container_reg = ws.get_details()["containerRegistry"]
     reg_name=container_reg.split("/")[-1]
     container_url = "\"" + image.image_location + "\","
     subscription_id = ws.subscription_id
     from azure.mgmt.containerregistry import ContainerRegistryManagementClient
     from azure.mgmt import containerregistry
     client = ContainerRegistryManagementClient(ws._auth,subscription_id)
     result= client.registries.list_credentials(resource_group_name, reg_name, custom_headers=None, raw=False)
     username = result.username
     password = result.passwords[0].value
     print('ContainerURL{}'.format(image.image_location))
     print('Servername: {}'.format(reg_name))
     print('Username: {}'.format(username))
     print('Password: {}'.format(password))
     ```
  1. Guarde los valores de ContainerURL, Servername, Username y Password. 

     Estas credenciales son necesarias para proporcionar el acceso de dispositivo IoT Edge a las imágenes en su registro de contenedor privado.

#### <a name="deploy-the-model-to-the-device"></a>Implementación del modelo en el dispositivo

Para implementar fácilmente un modelo, puede ejecutar [este script](https://raw.githubusercontent.com/Azure/ai-toolkit-iot-edge/master/amliotedge/deploymodel) y proporcionar la siguiente información de los pasos anteriores: nombre del registro de contenedor, nombre de usuario, contraseña, URL de ubicación de la imagen, nombre de la implementación deseada, nombre del IoT Hub e id. del dispositivo que creó. Puede hacerlo en la máquina virtual siguiendo estos pasos: 

```bash 
wget https://raw.githubusercontent.com/Azure/ai-toolkit-iot-edge/master/amliotedge/deploymodel
sudo chmod +x deploymodel
sudo ./deploymodel <ContainerRegistryName> <username> <password> <imageLocationURL> <DeploymentID> <IoTHubname> <DeviceID>
```

También puede seguir los pasos del documento [Implementación de módulos de Azure IoT Edge desde Azure Portal](../../iot-edge/how-to-deploy-modules-portal.md) para implementar la imagen en su dispositivo. Al definir la __configuración del registro__ para el dispositivo, use los valores de __servidor de inicio de sesión__, __nombre de usuario__ y __contraseña__ del registro de contenedor del área de trabajo.

> [!NOTE]
> Si no está familiarizado con Azure IoT, consulte los siguientes documentos para obtener información sobre cómo comenzar a usar el servicio:
>
> * [Inicio rápido: Implementación del primer módulo de IoT Edge en un dispositivo Linux](../../iot-edge/quickstart-linux.md)
> * [Inicio rápido: Implementación del primer módulo de IoT Edge en un dispositivo Windows](../../iot-edge/quickstart.md)


## <a name="testing-web-service-deployments"></a>Prueba de implementaciones de servicio web

Para probar una implementación de servicio web, puede usar el método `run` del objeto Webservice. En el ejemplo siguiente, un documento JSON está establecido en un servicio web y se muestra el resultado. Los datos enviados deben coincidir con lo que espera el modelo. En este ejemplo, el formato de datos coincide con la entrada esperada por el modelo de diabetes.

```python
import json

test_sample = json.dumps({'data': [
    [1,2,3,4,5,6,7,8,9,10], 
    [10,9,8,7,6,5,4,3,2,1]
]})
test_sample = bytes(test_sample,encoding = 'utf8')

prediction = service.run(input_data = test_sample)
print(prediction)
```

## <a name="update-the-web-service"></a>Actualizar el servicio web

Para actualizar el servicio web, utilice el método `update`. El código siguiente muestra cómo actualizar el servicio web para usar una nueva imagen:

```python
from azureml.core.webservice import Webservice

service_name = 'aci-mnist-3'
# Retrieve existing service
service = Webservice(name = service_name, workspace = ws)
# Update the image used by the service
service.update(image = new-image)
print(service.state)
```

> [!NOTE]
> Cuando se actualiza una imagen, el servicio web no se actualiza automáticamente. Debe actualizar manualmente cada servicio que quiera que use la nueva imagen.

## <a name="clean-up"></a>Limpieza

Para eliminar un servicio web implementado, use `service.delete()`.

Para eliminar una imagen, utilice `image.delete()`.

Para eliminar un modelo registrado, use `model.delete()`.

## <a name="next-steps"></a>Pasos siguientes

* [Protección de los servicios web de Azure Machine Learning con SSL](how-to-secure-web-service.md)
* [Consumir un modelo de ML que está implementado como un servicio web](how-to-consume-web-service.md)
* [Cómo ejecutar predicciones por lotes](how-to-run-batch-predictions.md)
