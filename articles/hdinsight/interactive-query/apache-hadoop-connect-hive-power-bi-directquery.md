---
title: Visualización de datos de Interactive Query Hive con Power BI en Azure HDInsight
description: Uso de Microsoft Power BI para visualizar datos de Interactive Query Hive desde Azure HDInsight
services: hdinsight
ms.service: hdinsight
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: jasonh
ms.custom: hdinsightactive
ms.topic: conceptual
ms.date: 11/06/2018
ms.openlocfilehash: 68f2314b995eb0f2f67307b44cbfd177f5c5a796
ms.sourcegitcommit: a08d1236f737915817815da299984461cc2ab07e
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/26/2018
ms.locfileid: "52309277"
---
# <a name="visualize-interactive-query-apache-hive-data-with-microsoft-power-bi-using-direct-query-in-azure-hdinsight"></a>Visualización de datos de Interactive Query Apache Hive con Microsoft Power BI mediante DirectQuery en Azure HDInsight

En este artículo se describe cómo conectar Microsoft Power BI a clústeres de Interactive Query de Azure HDInsight y cómo visualizar los datos de Apache Hive mediante DirectQuery. En el ejemplo proporcionado se cargan los datos de una tabla de Hive (hivesampletable) en Power BI. La tabla de Hive hivesampletable contiene algunos datos de uso de teléfonos móviles. Luego se trazan los datos de uso en un mapamundi:

![Informe de mapa de Power BI en HDInsight](./media/apache-hadoop-connect-hive-power-bi-directquery/hdinsight-power-bi-visualization.png)

Puede aprovechar el [controlador ODBC de Apache Hive](../hadoop/apache-hadoop-connect-hive-power-bi.md) para realizar la importación a través del conector ODBC genérico en Power BI Desktop. Sin embargo, no se recomienda para las cargas de trabajo de BI dada la naturaleza no interactiva del motor de consulta de Hive. El [conector de HDInsight Interactive Query](./apache-hadoop-connect-hive-power-bi-directquery.md) y el [conector de HDInsight Apache Spark](https://docs.microsoft.com/power-bi/spark-on-hdinsight-with-direct-connect) son las mejores opciones para su rendimiento.

## <a name="prerequisites"></a>Requisitos previos
Antes de seguir los pasos de este artículo, debe disponer de los siguientes elementos:

* **Clúster de HDInsight**. El clúster puede ser un clúster de HDInsight con Apache Hive o un clúster de Interactive Query recién lanzado. Para crear clústeres, vea [Crear clúster](../hadoop/apache-hadoop-linux-tutorial-get-started.md#create-cluster).
* **[Microsoft Power BI Desktop](https://powerbi.microsoft.com/desktop/)**. Puede descargar una copia desde el [Centro de descarga de Microsoft](https://www.microsoft.com/download/details.aspx?id=45331).

## <a name="load-data-from-hdinsight"></a>Carga de datos desde HDInsight

La tabla de Hive hivesampletable incluye todos los clústeres de HDInsight.

1. Inicie sesión en Power BI Desktop.

2. Haga clic en la pestaña **Inicio**, en **Obtener datos** en la cinta **Datos externos** y luego seleccione **Más...**

    ![Datos abiertos de Power BI en HDInsight](./media/apache-hadoop-connect-hive-power-bi-directquery/hdinsight-power-bi-open-odbc.png)
    
3. Desde el panel **Obtener datos**, escriba **hdinsight** en el cuadro de búsqueda. Si no ve **HDInsight Interactive Query (Beta)**, debe actualizar Power BI Desktop a la versión más reciente.

4. Seleccione **HDInsight Interactive Query (Beta)** y, después, **Conectar**.

5. Seleccione **Continuar** para cerrar el cuadro de diálogo de advertencia **Conector de vista previa**.

6. En **HDInsight Interactive Query**, seleccione o escriba la información siguiente:

    - **Servidor**: escriba el nombre de clúster de Interactive Query (por ejemplo, *myiqcluster.azurehdinsight.net*).

    - **Base de datos**: para este tutorial, escriba **default**.
    
    - **Modo de conectividad de datos**: para este tutorial, seleccione **DirectQuery**.

    ![Conexión de HDInsight Interactive Query y Power BI con DirectQuery](./media/apache-hadoop-connect-hive-power-bi-directquery/hdinsight-interactive-query-power-bi-connect.png)

7. Haga clic en **OK**.

8. Escriba las credenciales de usuario HTTP y, a continuación, haga clic en **Aceptar**. El nombre de usuario predeterminado es **admin**.

9. En el panel izquierdo, seleccione **hivesampletale** y, a continuación, haga clic en **Cargar**.

    ![HDInsight Interactive Query y Power BI con hivesampletable](./media/apache-hadoop-connect-hive-power-bi-directquery/hdinsight-interactive-query-power-bi-hivesampletable.png)

## <a name="visualize-data-on-a-map"></a>Visualización de datos en un mapa

Continúe a partir del último procedimiento.

1. En el panel Visualizaciones, seleccione **Mapa**.  Es un icono de globo terráqueo.

    ![HDInsight Power BI personaliza el informe](./media/apache-hadoop-connect-hive-power-bi-directquery/hdinsight-power-bi-customize.png)
    
2. En el panel Campos, seleccione **country** y **devicemake**. Puede ver los datos trazados en el mapa.

3. Expanda el mapa.

## <a name="next-steps"></a>Pasos siguientes
En este artículo ha aprendido a visualizar datos de HDInsight mediante Power BI.  Para más información sobre la visualización de datos, vea los siguientes artículos:

* [Visualización de los datos de Apache Hive con Microsoft Power BI mediante ODBC en Azure HDInsight](../hadoop/apache-hadoop-connect-hive-power-bi.md). 
* [Uso de Apache Zeppelin para ejecutar consultas de Apache Hive en Azure HDInsight](./../hdinsight-connect-hive-zeppelin.md).
* [Conexión de Excel a Hadoop en Azure HDInsight con Microsoft Hive ODBC Driver](../hadoop/apache-hadoop-connect-excel-hive-odbc-driver.md).
* [Conexión de Excel a Apache Hadoop con Power Query](../hadoop/apache-hadoop-connect-excel-power-query.md).
* [Conectarse a Azure HDInsight y ejecutar consultas de Apache Hive con Herramientas de Data Lake para Visual Studio](../hadoop/apache-hadoop-visual-studio-tools-get-started.md).
* [Uso de Herramientas de Azure HDInsight para Visual Studio Code](../hdinsight-for-vscode.md).
* [Carga de datos en HDInsight](./../hdinsight-upload-data.md).
