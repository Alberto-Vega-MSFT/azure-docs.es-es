---
title: Implementación de modelos en FPGA
titleSuffix: Azure Machine Learning service
description: Obtenga información sobre cómo implementar un servicio web con un modelo que se ejecuta en una FPGA con Azure Machine Learning Service para obtener una inferencia de latencia ultrabaja.
services: machine-learning
ms.service: machine-learning
ms.component: core
ms.topic: conceptual
ms.reviewer: jmartens
ms.author: tedway
author: tedway
ms.date: 12/06/2018
ms.custom: seodec18
ms.openlocfilehash: 3148d4d63ad1464dbd45c361237ac9cd4ffd485a
ms.sourcegitcommit: 7fd404885ecab8ed0c942d81cb889f69ed69a146
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/12/2018
ms.locfileid: "53268247"
---
# <a name="deploy-a-model-as-a-web-service-on-an-fpga-with-azure-machine-learning-service"></a>Implementación de un modelo como servicio web en una FPGA con Azure Machine Learning Service

Puede implementar un modelo como un servicio web en [Field-programmable Gate Arrays (FPGA)](concept-accelerate-with-fpgas.md).  El uso de las FPGA brinda una inferencia de latencia muy baja, incluso con un tamaño de lote único.   

## <a name="prerequisites"></a>Requisitos previos

- Si no tiene una suscripción a Azure, cree una cuenta gratuita antes de empezar. Pruebe hoy mismo la [versión gratuita o de pago de Azure Machine Learning Service](http://aka.ms/AMLFree).

- Un área de trabajo del servicio Azure Machine Learning y el SDK de Azure Machine Learning para Python instalado. Descubra cómo obtener estos requisitos previos con el documento [How to configure a development environment](how-to-configure-environment.md) (Cómo configurar un entorno de desarrollo).
 
  - El área de trabajo debe estar en la región *Este de EE. UU. 2*.

  - Instale los extras de "contrib":

    ```shell
    pip install --upgrade azureml-sdk[contrib]
    ```  

## <a name="create-and-deploy-your-model"></a>Creación e implementación de un modelo
Cree una canalización para preprocesar la imagen de entrada, caracterizarla mediante ResNet 50 en una FPGA y, a continuación, ejecute las características a través de un clasificador entrenado en el conjunto de datos de ImageNet.

Siga las instrucciones para:

* Definir la canalización del modelo
* Implementación del modelo
* Consumo del modelo implementado
* Eliminación de los servicios implementados

> [!IMPORTANT]
> Para optimizar la latencia y el rendimiento, el cliente debe estar en la misma región de Azure que el punto de conexión.  Actualmente, las API se crean en la región de Azure Este de EE. UU.



### <a name="preprocess-image"></a>Imagen de preprocesamiento
La primera fase de la canalización es preprocesar las imágenes.

```python
import os
import tensorflow as tf

# Input images as a two-dimensional tensor containing an arbitrary number of images represented a strings
import azureml.contrib.brainwave.models.utils as utils
in_images = tf.placeholder(tf.string)
image_tensors = utils.preprocess_array(in_images)
print(image_tensors.shape)
```

### <a name="add-featurizer"></a>Incorporación de un caracterizador
Inicialice el modelo y descargue un punto de control de TensorFlow de la versión cuantificada de ResNet50 que se usará como caracterizador.

```python
from azureml.contrib.brainwave.models import QuantizedResnet50, Resnet50
model_path = os.path.expanduser('~/models')
model = QuantizedResnet50(model_path, is_frozen = True)
feature_tensor = model.import_graph_def(image_tensors)
print(model.version)
print(feature_tensor.name)
print(feature_tensor.shape)
```

### <a name="add-classifier"></a>Incorporación de un clasificador
Este clasificador se ha entrenado en el conjunto de datos de ImageNet.

```python
classifier_input, classifier_output = Resnet50.get_default_classifier(feature_tensor, model_path)
```

### <a name="create-service-definition"></a>Cree la definición de servicio
Ahora que ha definido el procesamiento previo de imágenes, el caracterizador y el clasificador que se ejecuta en el servicio, puede crear una definición de servicio. La definición de servicio es un conjunto de archivos generado a partir del modelo que se implementa en el servicio FPGA. La definición del servicio consta de una canalización. La canalización consta de una serie de fases que se ejecutan en orden.  Se admiten las fases TensorFlow, Keras y BrainWave.  Las fases se ejecutan en el orden en el servicio, con la salida de cada entrada de fase en la fase posterior.

Para crear una fase TensorFlow, especifique una sesión que contiene el gráfico (en este caso se usa el gráfico de forma predeterminada) y los tensores de entrada y salida para esta fase.  Esta información se usa para guardar el gráfico para que se pueda ejecutar en el servicio.

```python
from azureml.contrib.brainwave.pipeline import ModelDefinition, TensorflowStage, BrainWaveStage

save_path = os.path.expanduser('~/models/save')
model_def_path = os.path.join(save_path, 'service_def.zip')

model_def = ModelDefinition()
with tf.Session() as sess:
    model_def.pipeline.append(TensorflowStage(sess, in_images, image_tensors))
    model_def.pipeline.append(BrainWaveStage(sess, model))
    model_def.pipeline.append(TensorflowStage(sess, classifier_input, classifier_output))
    model_def.save(model_def_path)
    print(model_def_path)
```

### <a name="deploy-model"></a>Implementación del modelo
Cree un servicio de la definición del servicio.  El área de trabajo debe estar en la ubicación Este de EE. UU. 2.

```python
from azureml.core import Workspace

ws = Workspace.from_config()
print(ws.name, ws.resource_group, ws.location, ws.subscription_id, sep = '\n')

from azureml.core.model import Model
model_name = "resnet-50-rtai"
registered_model = Model.register(ws, model_def_path, model_name)

from azureml.core.webservice import Webservice
from azureml.exceptions import WebserviceException
from azureml.contrib.brainwave import BrainwaveWebservice, BrainwaveImage
service_name = "imagenet-infer"
service = None
try:
    service = Webservice(ws, service_name)
except WebserviceException:
    image_config = BrainwaveImage.image_configuration()
    deployment_config = BrainwaveWebservice.deploy_configuration()
    service = Webservice.deploy_from_model(ws, service_name, [registered_model], image_config, deployment_config)
    service.wait_for_deployment(true)
```

### <a name="test-the-service"></a>Probar el servicio
Para enviar una imagen a la API y probar la respuesta, agregue una asignación de identificador de la clase de salida en el nombre de clase ImageNet.

```python
import requests
classes_entries = requests.get("https://raw.githubusercontent.com/Lasagne/Recipes/master/examples/resnet50/imagenet_classes.txt").text.splitlines()
```

Llame al servicio y reemplace el nombre de archivo "your-image.jpg" a continuación con una imagen del equipo. 

```python
with open('your-image.jpg') as f:
    results = service.run(f)
# map results [class_id] => [confidence]
results = enumerate(results)
# sort results by confidence
sorted_results = sorted(results, key=lambda x: x[1], reverse=True)
# print top 5 results
for top in sorted_results[:5]:
    print(classes_entries[top[0]], 'confidence:', top[1])
``` 

### <a name="clean-up-service"></a>Limpieza del servicio
Elimina el servicio web.

```python
service.delete()
    
registered_model.delete()
```

## <a name="secure-fpga-web-services"></a>Protección de los servicios web FPGA

Los modelos de Azure Machine Learning que se ejecutan en FPGA proporcionan compatibilidad con el protocolo SSL y autenticación basada en claves. Esto le permite restringir el acceso a su servicio y proteger los datos que envían los clientes. [Obtenga información sobre cómo proteger el servicio web](how-to-secure-web-service.md).


## <a name="next-steps"></a>Pasos siguientes

Obtenga información en el artículo [Consume a ML Model deployed as a web service ](how-to-consume-web-service.md) (Consumir un modelo de ML implementado como servicio web).
