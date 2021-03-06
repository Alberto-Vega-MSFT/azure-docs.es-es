---
title: Crear, ejecutar y realizar un seguimiento de las canalizaciones de ML
titleSuffix: Azure Machine Learning service
description: Cree y ejecute una canalización de aprendizaje automático con el SDK de Azure Machine Learning para Python.  Las canalizaciones se usan para crear y administrar flujos de trabajo que unen fases de aprendizaje automático (ML). Entre las diferentes fases de aprendizaje automático se incluyen la preparación de datos, el entrenamiento de modelos, la implementación de modelos y la inferencia.
services: machine-learning
ms.service: machine-learning
ms.component: core
ms.topic: conceptual
ms.reviewer: sgilley
ms.author: sanpil
author: sanpil
ms.date: 12/04/2018
ms.custom: seodec18
ms.openlocfilehash: 8478b6760921f4641cd214b1ff19cae9757b6d7e
ms.sourcegitcommit: 7fd404885ecab8ed0c942d81cb889f69ed69a146
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/12/2018
ms.locfileid: "53269052"
---
# <a name="create-and-run-a-machine-learning-pipeline-using-azure-machine-learning-sdk"></a>Crear y ejecutar una canalización de aprendizaje automático con el SDK de Azure Machine Learning

En este artículo aprenderá a crear, publicar, ejecutar y realizar un seguimiento de una [canalización de aprendizaje automático](concept-ml-pipelines.md) mediante el [SDK de Azure Machine Learning](https://aka.ms/aml-sdk).  Estas canalizaciones se usan para crear y administrar flujos de trabajo que unen varias fases de aprendizaje automático. Cada fase de una canalización, como la preparación de datos y el entrenamiento de modelos, puede incluir uno o más pasos.

Las canalizaciones que cree serán visibles para los miembros de su [área de trabajo](how-to-manage-workspace.md) de Azure Machine Learning Service. 

Recuerde que las canalizaciones usan destinos de proceso remotos para el cálculo y el almacenamiento de los datos intermedios y finales asociados a esa canalización.  Asimismo, las canalizaciones pueden leer y escribir datos en y desde las ubicaciones de[Azure Storage](https://docs.microsoft.com/azure/storage/).

>[!Note]
>Si no tiene una suscripción a Azure, cree una cuenta gratuita antes de empezar. Pruebe hoy mismo la [versión gratuita o de pago de Azure Machine Learning Service](http://aka.ms/AMLFree).

## <a name="prerequisites"></a>Requisitos previos

* [Configure un entorno de desarrollo](how-to-configure-environment.md) para instalar el SDK de Azure Machine Learning.

* Cree un [área de trabajo de Azure Machine Learning](how-to-configure-environment.md#workspace) que contendrá todos los recursos de la canalización. 

 ```python
 ws = Workspace.create(
     name = '<workspace-name>',
     subscription_id = '<subscription-id>',
     resource_group = '<resource-group>',
     location = '<workspace_region>',
     exist_ok = True)
 ```

## <a name="set-up-machine-learning-resources"></a>Configurar los recursos de aprendizaje automático

Cree los recursos necesarios para ejecutar una canalización:

* Configure un almacén de datos que puede usar para obtener acceso a los datos necesarios en los pasos de la canalización.

* Configure un objeto `DataReference` para que apunte a los datos que se guardan o a los que puede acceder en un almacén de datos.

* Configure los [destinos de proceso](concept-azure-machine-learning-architecture.md#compute-target) en los que se ejecutarán los pasos de su canalización.

### <a name="set-up-a-datastore"></a>Configurar un almacén de datos
Un almacén de datos almacena los datos a los que la canalización puede obtener acceso.  Cada área de trabajo tiene un almacén de datos predeterminado. Asimismo, puede registrar almacenes de datos adicionales. 

Cuando cree su área de trabajo, una instancia de [Azure File Storage ](https://docs.microsoft.com/azure/storage/files/storage-files-introduction) y una de [Blob Storage ](https://docs.microsoft.com/azure/storage/blobs/storage-blobs-introduction) se adjuntan al área de trabajo de forma predeterminada.  Azure File Storage es el "almacén de datos predeterminado" para un área de trabajo, pero también puede usar Blob Storage como un almacén de datos.  Obtenga más información sobre las [opciones de Azure Storage](https://docs.microsoft.com/azure/storage/common/storage-decide-blobs-files-disks). 

```python
# Default datastore (Azure file storage)
def_data_store = ws.get_default_datastore() 

# The above call is equivalent to this 
def_data_store = Datastore(ws, "workspacefilestore")

# Get blob storage associated with the workspace
def_blob_store = Datastore(ws, "workspaceblobstore")
```

Cargue archivos de datos o directorios en el almacén de datos para que pueda obtener acceso a ellos desde sus canalizaciones.  En este ejemplo se usa la versión de Blob Storage del almacén de datos:

```python
def_blob_store.upload_files(
    ["./data/20news.pkl"],
    target_path="20newsgroups", 
    overwrite=True)
```

Una canalización consta de uno o varios pasos.  Un paso es una unidad que se ejecuta en un destino de proceso.  Los pasos pueden consumir orígenes de datos y producir datos "intermedios". Igualmente, un paso puede crear datos como un modelo, un directorio con archivos dependientes o datos temporales.  Estos datos están disponibles para otros pasos más adelante en la canalización.

### <a name="configure-data-reference"></a>Configurar la referencia de datos

Acaba de crear un origen de datos al que se puede hacer referencia en una canalización como una entrada a un paso. Un objeto [DataReference](https://docs.microsoft.com/python/api/azureml-core/azureml.data.data_reference.datareference) se encarga de representar un origen de datos en una canalización. El objeto `DataReference` apunta a los datos que están en o que son accesibles desde un almacén de datos.

```python
blob_input_data = DataReference(
    datastore=def_blob_store,
    data_reference_name="test_data",
    path_on_datastore="20newsgroups/20news.pkl")
```

Los datos intermedios (o salida de un paso) se representan mediante un objeto [PipelineData](https://docs.microsoft.com/python/api/azureml-pipeline-core/azureml.pipeline.core.pipelinedata?view=azure-ml-py). `output_data1` se crea como la salida de un paso y se usa como la entrada de uno o más pasos futuros.  `PipelineData` introduce una dependencia de datos entre los pasos y crea un orden de ejecución implícito en la canalización.

```python
output_data1 = PipelineData(
    "output_data1",
    datastore=def_blob_store,
    output_name="output_data1")
```

### <a name="set-up-compute"></a>Configurar un proceso

En Azure Machine Learning, el proceso (o destino de proceso) se refiere a las máquinas o clústeres que realizarán los pasos del cálculo en su canal de aprendizaje automático. Por ejemplo, puede crear un proceso de Azure Machine Learning para ejecutar sus pasos.

```python
compute_name = "aml-compute"
 if compute_name in ws.compute_targets:
    compute_target = ws.compute_targets[compute_name]
    if compute_target and type(compute_target) is AmlCompute:
        print('Found compute target: ' + compute_name)
else:
    print('Creating a new compute target...')
    provisioning_config = AmlCompute.provisioning_configuration(vm_size = vm_size, # NC6 is GPU-enabled
                                                                min_nodes = 1, 
                                                                max_nodes = 4)
     # create the compute target
    compute_target = ComputeTarget.create(ws, compute_name, provisioning_config)
    
    # Can poll for a minimum number of nodes and for a specific timeout. 
    # If no min node count is provided it will use the scale settings for the cluster
    compute_target.wait_for_completion(show_output=True, min_node_count=None, timeout_in_minutes=20)
    
     # For a more detailed view of current cluster status, use the 'status' property    
    print(compute_target.status.serialize())
```

## <a name="construct-your-pipeline-steps"></a>Construir los pasos de la canalización

Ahora ya está listo para definir un paso de canalización. Hay muchos pasos integrados disponibles a través del SDK de Azure Machine Learning. El más básico de estos pasos es un `PythonScriptStep` que ejecuta un script de Python en un destino de proceso específico.

```python
trainStep = PythonScriptStep(
    script_name="train.py",
    arguments=["--input", blob_input_data, "--output", processed_data1],
    inputs=[blob_input_data],
    outputs=[processed_data1],
    compute_target=compute_target,
    source_directory=project_folder
)
```

Después de definir sus pasos, debe compilar la canalización mediante algunos o todos ellos.

>[!NOTE]
>No se carga ningún archivo o dato en Azure Machine Learning Service cuando define los pasos o compila la canalización.

```python
# list of steps to run
compareModels = [trainStep, extractStep, compareStep]

# Build the pipeline
pipeline1 = Pipeline(workspace=ws, steps=[compareModels])
```

## <a name="submit-the-pipeline"></a>Enviar la canalización

Cuando envía la canalización, se comprueban las dependencias para cada paso y se carga una instantánea de la carpeta especificada como directorio de origen en Azure Machine Learning Service.  Si no se especifica ningún directorio de origen, se carga el directorio local actual.

```python
# Submit the pipeline to be run
pipeline_run1 = Experiment(ws, 'Compare_Models_Exp').submit(pipeline1)
```

Cuando ejecute primero una canalización:

* La instantánea del proyecto se descarga en el destino de proceso de la instancia de Blob Storage asociada al área de trabajo.
* Se construye una imagen de docker correspondiente a cada paso en la canalización.
* La imagen de docker para cada paso se descarga en el destino de proceso del registro de contenedor.
* Si se especifica un objeto `DataReference` en un paso, se monta el almacén de datos. Si no se admite el montaje, los datos se copian al destino de proceso.
* El paso se ejecuta en el destino de proceso especificado en la definición del paso. 
* Se crean artefactos como registros, las propiedades stdout y stderr, métricas y resultados que especifica el paso. Estos artefactos se cargan y se guardan en el almacén de datos predeterminado del usuario.

![ejecutar un experimento como una canalización](./media/how-to-create-your-first-pipeline/run_an_experiment_as_a_pipeline.png)

## <a name="publish-a-pipeline"></a>Publicar una canalización

Puede publicar una canalización para ejecutarla con diferentes entradas más adelante. Para que el punto de conexión REST de una canalización ya publicada acepte los parámetros, la canalización debe parametrizarse antes de publicarse. 

1. Para crear un parámetro de canalización, use un objeto [PipelineParameter](https://docs.microsoft.com/python/api/azureml-pipeline-core/azureml.pipeline.core.graph.pipelineparameter?view=azure-ml-py) con un valor predeterminado.

 ```python
 pipeline_param = PipelineParameter(
     name="pipeline_arg", 
     default_value=10)
 ```

2. Agregue este objeto `PipelineParameter` como parámetro a cualquiera de los pasos de la canalización tal como se muestra a continuación:

 ```python
 compareStep = PythonScriptStep(
     script_name="compare.py",
     arguments=["--comp_data1", comp_data1, "--comp_data2", comp_data2, "--output_data", out_data3, "--param1", pipeline_param],
     inputs=[ comp_data1, comp_data2],
     outputs=[out_data3],    
     target=compute_target, 
     source_directory=project_folder)
 ```

3. Publique esta canalización para que acepte un parámetro cuando se invoque.

```python
published_pipeline1 = pipeline1.publish(
    name="My_Published_Pipeline", 
    description="My Published Pipeline Description")
```

## <a name="run-a-published-pipeline"></a>Ejecutar una canalización publicada

Todas las canalizaciones publicadas tienen un punto de conexión REST para invocar la ejecución de la canalización desde sistemas externos, como los clientes que no son de Python. Este punto de conexión proporciona una forma de "repetibilidad administrada" en los escenarios de puntuación y nuevo entrenamiento.

Para invocar el proceso de ejecución de la canalización anterior, necesitará un token de encabezado de autenticación de Azure Active Directory, tal como se describe en la [clase AzureCliAuthentication](https://docs.microsoft.com/python/api/azureml-core/azureml.core.authentication.azurecliauthentication?view=azure-ml-py).

```python
response = requests.post(published_pipeline1.endpoint, 
    headers=aad_token, 
    json={"ExperimentName": "My_Pipeline",
        "ParameterAssignments": {"pipeline_arg": 20}})
```
## <a name="view-results"></a>Visualización de los resultados

Ver la lista de todas las canalizaciones y sus detalles de ejecución:
1. Inicie sesión en el [Azure Portal](https://portal.azure.com/).  

1. [Ver el área de trabajo](how-to-manage-workspace.md#view-a-workspace) para encontrar la lista de canalizaciones.
 ![lista de canalizaciones de Machine Learning](./media/how-to-create-your-first-pipeline/list_of_pipelines.png)
 
1. Seleccione una canalización específica para ver los resultados de la ejecución.

## <a name="next-steps"></a>Pasos siguientes
- Use [estos cuadernos de Jupyter en GitHub](https://aka.ms/aml-pipeline-readme) para explorar aún más canalizaciones de Machine Learning.
- Lea la ayuda de referencia del SDK para el paquete [azureml-pipelines-core](https://docs.microsoft.com/python/api/azureml-pipeline-core/?view=azure-ml-py) y el paquete [azureml-pipelines-steps](https://docs.microsoft.com/python/api/azureml-pipeline-steps/?view=azure-ml-py).

[!INCLUDE [aml-clone-in-azure-notebook](../../../includes/aml-clone-for-examples.md)]
