---
title: Destinos de proceso remoto de ML automatizado
titleSuffix: Azure Machine Learning service
description: Aprenda a crear modelos mediante el aprendizaje automático automatizado en un destino de proceso remoto de Data Science Virtual Machine (DSVM) con Azure Machine Learning Service
services: machine-learning
author: nacharya1
ms.author: nilesha
ms.reviewer: sgilley
ms.service: machine-learning
ms.component: core
ms.workload: data-services
ms.topic: conceptual
ms.date: 12/04/2018
ms.custom: seodec18
ms.openlocfilehash: 18b2b3df2748392b12b60517604478b120871754
ms.sourcegitcommit: 1c1f258c6f32d6280677f899c4bb90b73eac3f2e
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/11/2018
ms.locfileid: "53256067"
---
# <a name="train-models-with-automated-machine-learning-in-the-cloud"></a>Entrenamiento de modelos con aprendizaje automático automatizado en la nube

En Azure Machine Learning, puede entrenar un modelo en los diferentes tipos de recursos de proceso que administra. El destino de proceso podría ser un equipo local o un equipo en la nube.

Fácilmente, puede escalar vertical u horizontalmente el experimento de aprendizaje automático al agregar destinos de proceso adicionales. Entre las opciones de destino de proceso se incluyen Data Science Virtual Machine (DSVM) basada en Ubuntu o Proceso de Azure Machine Learning. DSVM es una imagen de máquina virtual personalizada en la nube de Azure de Microsoft diseñada específicamente para realizar la ciencia de datos. Tiene muchas ciencias de datos conocidas y otras herramientas preinstaladas y configuradas previamente.  

En este artículo, aprenderá a crear un modelo mediante ML automatizado en una máquina DSVM.

## <a name="how-does-remote-differ-from-local"></a>¿En qué se diferencia el modo remoto del local?

El tutorial "[Train a classification model with automated machine learning](tutorial-auto-train-models.md)" (Entrenamiento de un modelo de clasificación con el aprendizaje automático automatizado) le enseña a usar un equipo local para entrenar el modelo con ML automatizado.  El flujo de trabajo del entrenamiento local también se aplica a los destinos remotos. Sin embargo, con un proceso remoto, las iteraciones del experimento de ML automatizado se ejecutan de forma asincrónica. Esta funcionalidad le permite cancelar una iteración determinada, ver el estado de la ejecución y continuar trabajando en otras celdas del cuaderno de Jupyter. Para entrenar de forma remota, en primer lugar cree un destino de proceso remoto, como una máquina DSVM de Azure.  A continuación, configure el recurso remoto y envíe el código allí.

Este artículo muestra los pasos adicionales necesarios para ejecutar un experimento de ML automatizado en una máquina DSVM remota.  Aquí se usa, en todo el código, el objeto del área de trabajo, `ws` del tutorial.

```python
ws = Workspace.from_config()
```

## <a name="create-resource"></a>Crear el recurso

Cree la DSVM en el área de trabajo (`ws`) si aún no existe. Si la máquina DSVM se creó anteriormente, este código omite el proceso de creación y carga los detalles de los recursos existentes en el objeto `dsvm_compute`.  

**Tiempo estimado**: La creación de la VM tarda aproximadamente 5 minutos.

```python
from azureml.core.compute import DsvmCompute

dsvm_name = 'mydsvm' #Name your DSVM
try:
    dsvm_compute = DsvmCompute(ws, dsvm_name)
    print('found existing dsvm.')
except:
    print('creating new dsvm.')
    # Below is using a VM of SKU Standard_D2_v2 which is 2 core machine. You can check Azure virtual machines documentation for additional SKUs of VMs.
    dsvm_config = DsvmCompute.provisioning_configuration(vm_size = "Standard_D2_v2")
    dsvm_compute = DsvmCompute.create(ws, name = dsvm_name, provisioning_configuration = dsvm_config)
    dsvm_compute.wait_for_completion(show_output = True)
```

Ahora puede usar el objeto `dsvm_compute` como destino del proceso remoto.

Entre las restricciones en los nombres de máquina DSVM se incluyen:
+ La longitud debe ser inferior a 64 caracteres.  
+ No puede contener ninguno de los caracteres siguientes: {`\` ~ ! @ # $ % ^ & * ( ) = + _ [ ] { } \\\\ | ; : \' \\" , < > / ?.`

>[!Warning]
>Si la creación no puede llevarse a cabo y aparece un mensaje sobre la idoneidad de la compra de Marketplace:
>    1. Vaya a [Azure Portal](https://portal.azure.com).
>    1. Empiece a crear una máquina DSVM. 
>    1. Seleccione "Want to create programmatically" (Deseo crear mediante programación) para habilitar la creación mediante programación.
>    1. Salga sin crear realmente la máquina virtual.
>    1. Vuelva a ejecutar el código de creación.

Este código no crea un nombre de usuario ni una contraseña para la máquina DSVM que se aprovisiona. Si desea conectarse directamente a la VM, vaya a [Azure Portal](https://portal.azure.com) para crear las credenciales.  

### <a name="attach-existing-linux-dsvm"></a>Adjuntar la DSVM Linux existente

También puede adjuntar una DSVM Linux existente como destino de proceso. En este ejemplo se usa una DSVM existente, pero no se crea un nuevo recurso.

> [!NOTE]
>
> En el siguiente código se usa la clase de destino `RemoteCompute` para adjuntar una VM existente como destino de proceso.
> La clase `DsvmCompute` caerá en desuso en futuras versiones a favor de este patrón de diseño.

Ejecute el código siguiente para crear el destino de proceso desde una DSVM Linux ya existentes.

```python
from azureml.core.compute import ComputeTarget, RemoteCompute 

attach_config = RemoteCompute.attach_configuration(username='<username>',
                                                   address='<ip_adress_or_fqdn>',
                                                   ssh_port=22,
                                                   private_key_file='./.ssh/id_rsa')
compute_target = ComputeTarget.attach(workspace=ws,
                                      name='attached_vm',
                                      attach_configuration=attach_config)

compute_target.wait_for_completion(show_output=True)
```

Ahora puede usar el objeto `compute_target` como destino del proceso remoto.

## <a name="access-data-using-getdata-file"></a>Acceso a los datos con un archivo get_data

Proporcione a los recursos remotos acceso a los datos de entrenamiento. Para los experimentos de aprendizaje automático automatizado que se ejecutan en el proceso remoto, los datos deben capturarse con una función `get_data()`.  

Para proporcionar acceso, debe:
+ Crear un archivo get_data.py que contenga una función `get_data()` 
* Colocar ese archivo en un directorio accesible como ruta de acceso completa 

Puede encapsular el código para leer datos desde un almacenamiento de blobs o un disco local en el archivo get_data.py. En el siguiente ejemplo de código, los datos proceden del paquete sklearn.

>[!Warning]
>Si usa un proceso remoto, debe usar la función `get_data()` donde se realicen las transformaciones de datos. Si tiene que instalar bibliotecas adicionales para las transformaciones de datos como parte de get_data(), existen pasos adicionales que se deben seguir. Consulte el [Cuaderno de ejemplo auto-ml-dataprep](https://aka.ms/aml-auto-ml-data-prep ) para más información.


```python
# Create a project_folder if it doesn't exist
if not os.path.exists(project_folder):
    os.makedirs(project_folder)

#Write the get_data file.
%%writefile $project_folder/get_data.py

from sklearn import datasets
from scipy import sparse
import numpy as np

def get_data():
    
    digits = datasets.load_digits()
    X_digits = digits.data[10:,:]
    y_digits = digits.target[10:]

    return { "X" : X_digits, "y" : y_digits }
```

## <a name="configure-experiment"></a>Configuración del experimento

Especifique la configuración de `AutoMLConfig`.  (Consulte una [lista completa de parámetros](how-to-configure-auto-train.md#configure-experiment) y sus posibles valores).

En la configuración, `run_configuration` está establecido en el objeto `run_config`, que contiene la configuración de la máquina DSVM.  

```python
from azureml.train.automl import AutoMLConfig
import time
import logging

automl_settings = {
    "name": "AutoML_Demo_Experiment_{0}".format(time.time()),
    "iteration_timeout_minutes": 10,
    "iterations": 20,
    "n_cross_validations": 5,
    "primary_metric": 'AUC_weighted',
    "preprocess": False,
    "max_concurrent_iterations": 10,
    "verbosity": logging.INFO
}

automl_config = AutoMLConfig(task='classification',
                             debug_log='automl_errors.log',
                             path=project_folder,
                             compute_target = dsvm_compute,
                             data_script=project_folder + "/get_data.py",
                             **automl_settings,
                            )
```

### <a name="enable-model-explanations"></a>Habilitación de las explicaciones de modelo

Establezca el parámetro opcional `model_explainability` en el constructor `AutoMLConfig`. Además, se debe pasar un objeto dataframe de validación como parámetro `X_valid` para usar la característica de explicación del modelo.

```python
automl_config = AutoMLConfig(task='classification',
                             debug_log='automl_errors.log',
                             path=project_folder,
                             compute_target = dsvm_compute,
                             data_script=project_folder + "/get_data.py",
                             **automl_settings,
                             model_explainability=True,
                             X_valid = X_test
                            )
```

## <a name="submit-training-experiment"></a>Envío del experimento de entrenamiento

Ahora, envíe la configuración para seleccionar automáticamente el algoritmo y los hiperparámetros, y entrenar el modelo.

```python
from azureml.core.experiment import Experiment
experiment=Experiment(ws, 'automl_remote')
remote_run = experiment.submit(automl_config, show_output=True)
```

Verá un resultado similar al del siguiente ejemplo:

    Running on remote compute: mydsvmParent Run ID: AutoML_015ffe76-c331-406d-9bfd-0fd42d8ab7f6
    ***********************************************************************************************
    ITERATION: The iteration being evaluated.
    PIPELINE:  A summary description of the pipeline being evaluated.
    DURATION: Time taken for the current iteration.
    METRIC: The result of computing score on the fitted pipeline.
    BEST: The best observed score thus far.
    ***********************************************************************************************
    
     ITERATION     PIPELINE                               DURATION                METRIC      BEST
             2      Standardize SGD classifier            0:02:36                  0.954     0.954
             7      Normalizer DT                         0:02:22                  0.161     0.954
             0      Scale MaxAbs 1 extra trees            0:02:45                  0.936     0.954
             4      Robust Scaler SGD classifier          0:02:24                  0.867     0.954
             1      Normalizer kNN                        0:02:44                  0.984     0.984
             9      Normalizer extra trees                0:03:15                  0.834     0.984
             5      Robust Scaler DT                      0:02:18                  0.736     0.984
             8      Standardize kNN                       0:02:05                  0.981     0.984
             6      Standardize SVM                       0:02:18                  0.984     0.984
            10      Scale MaxAbs 1 DT                     0:02:18                  0.077     0.984
            11      Standardize SGD classifier            0:02:24                  0.863     0.984
             3      Standardize gradient boosting         0:03:03                  0.971     0.984
            12      Robust Scaler logistic regression     0:02:32                  0.955     0.984
            14      Scale MaxAbs 1 SVM                    0:02:15                  0.989     0.989
            13      Scale MaxAbs 1 gradient boosting      0:02:15                  0.971     0.989
            15      Robust Scaler kNN                     0:02:28                  0.904     0.989
            17      Standardize kNN                       0:02:22                  0.974     0.989
            16      Scale 0/1 gradient boosting           0:02:18                  0.968     0.989
            18      Scale 0/1 extra trees                 0:02:18                  0.828     0.989
            19      Robust Scaler kNN                     0:02:32                  0.983     0.989


## <a name="explore-results"></a>Exploración de los resultados

Puede usar el mismo widget de Jupyter que el del [tutorial de aprendizaje](tutorial-auto-train-models.md#explore-the-results) para ver un gráfico y la tabla de resultados.

```python
from azureml.widgets import RunDetails
RunDetails(remote_run).show()
```
Esta es una imagen estática del widget.  En el cuaderno, puede hacer clic en cualquier línea de la tabla para ver las propiedades de ejecución y los registros de salida para los que se ejecutan.   También puede usar la lista desplegable encima del grafo para ver un grafo de cada métrica disponible para cada iteración.

![tabla de widget](./media/how-to-auto-train-remote/table.png)
![trazado de widget](./media/how-to-auto-train-remote/plot.png)

El widget muestra una dirección URL que puede usar para ver y explorar los detalles de ejecución individuales.
 
### <a name="view-logs"></a>Ver registros

Busque los registros en la DSVM en `/tmp/azureml_run/{iterationid}/azureml-logs`.

## <a name="best-model-explanation"></a>Explicación del mejor modelo

La recuperación de los datos de explicación de modelos le permite ver información detallada sobre los modelos para aumentar la transparencia sobre lo que se ejecuta en el back-end. En este ejemplo, ejecuta explicaciones de modelo únicamente para el mejor modelo de ajuste. Si se ejecuta para todos los modelos en la canalización, el tiempo de ejecución será significativo. La información de explicación de modelos incluye:

* shap_values: Información de explicación generada por shap lib
* expected_values: Valor esperado del modelo que se aplica para el conjunto de datos X_train
* overall_summary: Valores de importancia de características a nivel del modelo en orden descendente
* overall_imp: Nombres de las características ordenadas igual que overall_summary
* per_class_summary: Valores de importancia de características a nivel de la clase en orden descendente. Solo está disponible para el caso de clasificación
* per_class_imp: Nombres de las características ordenadas igual que per_class_summary. Solo está disponible para el caso de clasificación

Use el código siguiente para seleccionar la mejor canalización de las iteraciones. El método `get_output` devuelve la mejor ejecución y el modelo ajustado de la última invocación de ajuste.

```python
best_run, fitted_model = remote_run.get_output()
```

Importe la función `retrieve_model_explanation` y ejecútela en el mejor modelo.

```python
from azureml.train.automl.automlexplainer import retrieve_model_explanation

shap_values, expected_values, overall_summary, overall_imp, per_class_summary, per_class_imp = \
    retrieve_model_explanation(best_run)
```

Imprima los resultados de las variables de explicación `best_run` que quiere ver.

```python
print(overall_summary)
print(overall_imp)
print(per_class_summary)
print(per_class_imp)
```

Imprimir las variables de resumen de explicación `best_run` genera la siguiente salida.

![Salida de la consola de explicación de modelos](./media/how-to-auto-train-remote/expl-print.png)

También puede visualizar la importancia de características mediante la UI del widget, así como la UI web en Azure Portal dentro de su área de trabajo.

![UI de explicación de modelos](./media/how-to-auto-train-remote/model-exp.png)

## <a name="example"></a>Ejemplo

En el cuaderno [how-to-use-azureml/automated-machine-learning/remote-execution/auto-ml-remote-execution.ipynb](https://github.com/Azure/MachineLearningNotebooks/blob/master/how-to-use-azureml/automated-machine-learning/remote-execution/auto-ml-remote-execution.ipynb) se muestran los conceptos de este artículo. 

[!INCLUDE [aml-clone-in-azure-notebook](../../../includes/aml-clone-for-examples.md)]

## <a name="next-steps"></a>Pasos siguientes

Más información acerca de [cómo configurar opciones para el aprendizaje automático](how-to-configure-auto-train.md).
