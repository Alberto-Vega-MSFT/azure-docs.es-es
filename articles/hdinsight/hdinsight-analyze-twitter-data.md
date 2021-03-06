---
title: Análisis de datos de Twitter con Apache Hadoop en HDInsight (Azure)
description: Vea cómo utilizar Hive para analizar datos de Twitter con Apache Hadoop en HDInsight para saber la frecuencia de uso de una palabra determinada.
services: hdinsight
author: hrasheed-msft
ms.reviewer: jasonh
ms.service: hdinsight
ms.topic: conceptual
ms.date: 05/25/2017
ms.author: hrasheed
ROBOTS: NOINDEX
ms.openlocfilehash: b8ab4acd24a53267711fde4408bb9fa8f52c35f3
ms.sourcegitcommit: c94cf3840db42f099b4dc858cd0c77c4e3e4c436
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/19/2018
ms.locfileid: "53635585"
---
# <a name="analyze-twitter-data-using-apache-hive-in-hdinsight"></a>Análisis de datos de Twitter con Apache Hive en HDInsight
Los sitios web de las redes sociales constituyen una de las principales fuerzas motrices para la adopción de Big Data. Las API públicas proporcionadas por sitios como Twitter constituyen un origen de datos muy útil para analizar y comprender las tendencias populares.
En este tutorial, obtendrá tweets mediante la API de streaming de Twitter y, a continuación, usará [Apache Hive](https://hive.apache.org/) en HDInsight de Azure para obtener una lista de usuarios de Twitter que envían más tweets que contienen una palabra determinada.

> [!IMPORTANT]  
> Los pasos de este documento requieren un clúster de HDInsight basado en Windows. Linux es el único sistema operativo que se usa en la versión 3.4 de HDInsight, o en las superiores. Consulte la información sobre la [retirada de HDInsight en Windows](hdinsight-component-versioning.md#hdinsight-windows-retirement). Para ver los pasos específicos para un clúster basado en Linux, consulte [Análisis de datos de Twitter con Apache Hive en HDInsight (Linux)](hdinsight-analyze-twitter-data-linux.md).

## <a name="prerequisites"></a>Requisitos previos
Antes de empezar este tutorial, debe contar con lo siguiente:

* **Una estación de trabajo** con Powershell de Azure instalado y configurado.

    Para ejecutar scripts de Windows PowerShell, debe ejecutar PowerShell de Azure como administrador y establecer la directiva de ejecución en *RemoteSigned*. Consulte [Ejecución de scripts de Windows PowerShell][powershell-script].

    Antes de ejecutar scripts de Windows PowerShell, asegúrese de estar conectado a su suscripción de Azure mediante el siguiente cmdlet:

    ```powershell
    Connect-AzureRmAccount
    ```

    Si tiene varias suscripciones a Azure, utilice el siguiente cmdlet para definir la suscripción actual:

    ```powershell
    Select-AzureRmSubscription -SubscriptionID <Azure Subscription ID>
    ```

    > [!IMPORTANT]  
    > La compatibilidad con Azure PowerShell para administrar recursos de HDInsight mediante Azure Service Manager está **en desuso** y dejó de estar disponible por completo el 1 de enero de 2017. En los pasos descritos en este documento, se usan los nuevos cmdlets de HDInsight que funcionan con Azure Resource Manager.
    >
    > Para instalar la versión más reciente, siga los pasos descritos en [Cómo instalar y configurar Azure PowerShell](/powershell/azureps-cmdlets-docs) . Si tiene scripts que se deben modificar para usar los nuevos cmdlets que funcionan con Azure Resource Manager, consulte [Migrating to Azure Resource Manager-based development tools for HDInsight clusters](hdinsight-hadoop-development-using-azure-resource-manager.md) (Migración a herramientas de desarrollo basadas en Azure Resource Manager para clústeres de HDInsight) para más información.

* **Un clúster de HDInsight de Azure**. Para obtener instrucciones sobre el aprovisionamiento del clúster, consulte [Introducción al uso de HDInsight][hdinsight-get-started] o [Aprovisionamiento de clústeres de HDInsight][hdinsight-provision]. Necesitará el nombre del clúster más adelante en el tutorial.

La siguiente tabla enumera los archivos utilizados en este tutorial:

| Archivos | DESCRIPCIÓN |
| --- | --- |
| /tutorials/twitter/data/tweets.txt |Los datos de origen para el trabajo de Hive. |
| /tutorials/twitter/output |La carpeta de salida para el trabajo de Hive. El nombre predeterminado del archivo de salida del trabajo de Hive es **000000_0**. |
| tutorials/twitter/twitter.hql |El archivo de script de HiveQL. |
| /tutorials/twitter/jobstatus |El estado del trabajo de Hadoop. |

## <a name="get-twitter-feed"></a>Obtención de una fuente de Twitter
En este tutorial, verá las [API de streaming de Twitter][twitter-streaming-api]. La API de streaming de Twitter específica que usará es [statuses/filter][twitter-statuses-filter].

> [!NOTE]  
> Un archivo que contiene 10 000 tweets y el archivo de script de Hive (que se describe en la sección siguiente) se han cargado en un contenedor de Blob público. Puede omitir esta sección si va a utilizar los archivos cargados.

Los datos de tweets se almacenan en formato de notación de objetos JavaScript (JSON) que contiene una compleja estructura anidada. En lugar de escribir varias líneas de código con un lenguaje de programación convencional, puede transformar esta estructura anidada en una tabla de Hive para que se puedan realizar consultas a través de un lenguaje similar al Lenguaje de consulta estructurado (SQL) llamado HiveQL.

Twitter utiliza OAuth para brindar acceso autorizado a su API. OAuth es un protocolo de autenticación mediante el que los usuarios pueden permitir que la aplicación actúe en su nombre sin compartir la contraseña. Puede encontrar más información en [oauth.net](https://oauth.net/) o en la excelente [Guía para principiantes de OAuth](https://hueniverse.com/oauth/) de Hueniverse.

El primer paso es utilizar OAuth para crear una aplicación nueva en el sitio de desarrolladores de Twitter.

**Para crear una aplicación de Twitter**

1. Inicie sesión en [https://apps.twitter.com/](https://apps.twitter.com/). Haga clic en el vínculo **Registrase ahora** si no tiene una cuenta de Twitter.
2. Haga clic en **Crear nueva aplicación**.
3. Especifique los campos **Name** (Nombre), **Description** (Descripción), **Website** (Sitio web). Puede conformar una dirección URL para el campo **Website** (Sitio web). La siguiente tabla muestra algunos valores de ejemplo para utilizar:

   | Campo | Valor |
   | --- | --- |
   |  NOMBRE |MyHDInsightApp |
   |  DESCRIPCIÓN |MyHDInsightApp |
   |  Website |https://www.myhdinsightapp.com |
4. Active **Yes, I agree** (Acepto) y, a continuación, haga clic en **Create your Twitter application** (Crear la aplicación de Twitter).
5. Haga clic en la pestaña **Permissions** (Permisos). El permiso predeterminado es **Read only**(Solo lectura). Esto es suficiente para este tutorial.
6. Haga clic en la pestaña **Keys and Access Tokens** (Claves y tokens de acceso).
7. Haga clic en **Create my access token**(Crear mi token de acceso).
8. Haga clic en **Prueba de OAuth** en la esquina superior derecha de la página.
9. Rellene los campos **consumer key** (clave del consumidor), **Consumer secret** (Secreto del consumidor), **Access token** (Token de acceso) y **Access token secret** (Secreto del token de acceso). Los necesitará más adelante en el tutorial.

En este tutorial, usará Windows PowerShell para realizar una llamada de servicio web. La otra herramienta popular para realizar llamadas de servicios web es [*Curl*][curl]. Puede descargar Curl [aquí][curl-download].

> [!NOTE]  
> Cuando utilice el comando Curl en Windows, use comillas dobles en lugar de comillas simples para los valores de opción.

**Para obtener tweets**

1. Abra el entorno de scripting integrado (ISE) de Windows PowerShell. En la pantalla Inicio de Windows 8, escriba **PowerShell_ISE** y haga clic en **Windows PowerShell ISE**. Consulte [Inicio de Windows PowerShell en Windows 8 y Windows](https://docs.microsoft.com/powershell/scripting/setup/starting-windows-powershell?view=powershell-6)
2. Copie el siguiente script en el panel de scripts:

    ```powershell
    #region - variables and constants
    $clusterName = "<HDInsightClusterName>" # Enter the HDInsight cluster name

    # Enter the OAuth information for your Twitter application
    $oauth_consumer_key = "<TwitterAppConsumerKey>";
    $oauth_consumer_secret = "<TwitterAppConsumerSecret>";
    $oauth_token = "<TwitterAppAccessToken>";
    $oauth_token_secret = "<TwitterAppAccessTokenSecret>";

    $destBlobName = "tutorials/twitter/data/tweets.txt" # This script saves the tweets into this blob.

    $trackString = "Azure, Cloud, HDInsight" # This script gets the tweets containing these keywords.
    $track = [System.Uri]::EscapeDataString($trackString);
    $lineMax = 10000  # The script will get this number of tweets. It is about 3 minutes every 100 lines.
    #endregion

    #region - Connect to Azure subscription
    Write-Host "`nConnecting to your Azure subscription ..." -ForegroundColor Green
    Connect-AzureRmAccount
    #endregion

    #region - Create a block blob object for writing tweets into Blob storage
    Write-Host "Get the default storage account name and Blob container name using the cluster name ..." -ForegroundColor Green
    $myCluster = Get-AzureRmHDInsightCluster -Name $clusterName
    $resourceGroupName = $myCluster.ResourceGroup
    $storageAccountName = $myCluster.DefaultStorageAccount.Replace(".blob.core.windows.net", "")
    $containerName = $myCluster.DefaultStorageContainer
    Write-Host "`tThe storage account name is $storageAccountName." -ForegroundColor Yellow
    Write-Host "`tThe blob container name is $containerName." -ForegroundColor Yellow

    Write-Host "Define the Azure storage connection string ..." -ForegroundColor Green
    $storageAccountKey = (Get-AzureRmStorageAccountKey -ResourceGroupName $resourceGroupName -Name $storageAccountName)[0].Value
    $storageConnectionString = "DefaultEndpointsProtocol=https;AccountName=$storageAccountName;AccountKey=$storageAccountKey"
    Write-Host "`tThe connection string is $storageConnectionString." -ForegroundColor Yellow

    Write-Host "Create block blob object ..." -ForegroundColor Green
    $storageAccount = [Microsoft.WindowsAzure.Storage.CloudStorageAccount]::Parse($storageConnectionString)
    $storageClient = $storageAccount.CreateCloudBlobClient();
    $storageContainer = $storageClient.GetContainerReference($containerName)
    $destBlob = $storageContainer.GetBlockBlobReference($destBlobName)
    #end region

    # region - Format OAuth strings
    Write-Host "Format oauth strings ..." -ForegroundColor Green
    $oauth_nonce = [System.Convert]::ToBase64String([System.Text.Encoding]::ASCII.GetBytes([System.DateTime]::Now.Ticks.ToString()));
    $ts = [System.DateTime]::UtcNow - [System.DateTime]::ParseExact("01/01/1970", "dd/MM/yyyy", $null)
    $oauth_timestamp = [System.Convert]::ToInt64($ts.TotalSeconds).ToString();

    $signature = "POST&";
    $signature += [System.Uri]::EscapeDataString("https://stream.twitter.com/1.1/statuses/filter.json") + "&";
    $signature += [System.Uri]::EscapeDataString("oauth_consumer_key=" + $oauth_consumer_key + "&");
    $signature += [System.Uri]::EscapeDataString("oauth_nonce=" + $oauth_nonce + "&");
    $signature += [System.Uri]::EscapeDataString("oauth_signature_method=HMAC-SHA1&");
    $signature += [System.Uri]::EscapeDataString("oauth_timestamp=" + $oauth_timestamp + "&");
    $signature += [System.Uri]::EscapeDataString("oauth_token=" + $oauth_token + "&");
    $signature += [System.Uri]::EscapeDataString("oauth_version=1.0&");
    $signature += [System.Uri]::EscapeDataString("track=" + $track);

    $signature_key = [System.Uri]::EscapeDataString($oauth_consumer_secret) + "&" + [System.Uri]::EscapeDataString($oauth_token_secret);

    $hmacsha1 = new-object System.Security.Cryptography.HMACSHA1;
    $hmacsha1.Key = [System.Text.Encoding]::ASCII.GetBytes($signature_key);
    $oauth_signature = [System.Convert]::ToBase64String($hmacsha1.ComputeHash([System.Text.Encoding]::ASCII.GetBytes($signature)));

    $oauth_authorization = 'OAuth ';
    $oauth_authorization += 'oauth_consumer_key="' + [System.Uri]::EscapeDataString($oauth_consumer_key) + '",';
    $oauth_authorization += 'oauth_nonce="' + [System.Uri]::EscapeDataString($oauth_nonce) + '",';
    $oauth_authorization += 'oauth_signature="' + [System.Uri]::EscapeDataString($oauth_signature) + '",';
    $oauth_authorization += 'oauth_signature_method="HMAC-SHA1",'
    $oauth_authorization += 'oauth_timestamp="' + [System.Uri]::EscapeDataString($oauth_timestamp) + '",'
    $oauth_authorization += 'oauth_token="' + [System.Uri]::EscapeDataString($oauth_token) + '",';
    $oauth_authorization += 'oauth_version="1.0"';

    $post_body = [System.Text.Encoding]::ASCII.GetBytes("track=" + $track);
    #endregion

    #region - Read tweets
    Write-Host "Create HTTP web request ..." -ForegroundColor Green
    [System.Net.HttpWebRequest] $request = [System.Net.WebRequest]::Create("https://stream.twitter.com/1.1/statuses/filter.json");
    $request.Method = "POST";
    $request.Headers.Add("Authorization", $oauth_authorization);
    $request.ContentType = "application/x-www-form-urlencoded";
    $body = $request.GetRequestStream();

    $body.write($post_body, 0, $post_body.length);
    $body.flush();
    $body.close();
    $response = $request.GetResponse() ;

    Write-Host "Start stream reading ..." -ForegroundColor Green

    Write-Host "Define a MemoryStream and a StreamWriter for writing ..." -ForegroundColor Green
    $memStream = New-Object System.IO.MemoryStream
    $writeStream = New-Object System.IO.StreamWriter $memStream

    $sReader = New-Object System.IO.StreamReader($response.GetResponseStream())

    $inrec = $sReader.ReadLine()
    $count = 0
    while (($inrec -ne $null) -and ($count -le $lineMax))
    {
        if ($inrec -ne "")
        {
            Write-Host "`n`t $count tweets received." -ForegroundColor Yellow

            $writeStream.WriteLine($inrec)
            $count ++
        }

        $inrec=$sReader.ReadLine()
    }
    #endregion

    #region - Write tweets to Blob storage
    Write-Host "Write to the destination blob ..." -ForegroundColor Green
    $writeStream.Flush()
    $memStream.Seek(0, "Begin")
    $destBlob.UploadFromStream($memStream)

    $sReader.close()
    #endregion

    Write-Host "Completed!" -ForegroundColor Green
    ```

3. Establecen los primeros cinco a ocho variables en el script:

    Variable|DESCRIPCIÓN
    ---|---
    $clusterName|Este es el nombre del clúster de HDInsight donde desea ejecutar la aplicación.
    $oauth_consumer_key|Esta es la **clave de consumidor** de la aplicación de Twitter que anotó anteriormente al crear la aplicación de Twitter.
    $oauth_consumer_secret|Este es el **secreto del consumidor** de la aplicación de Twitter que anotó anteriormente.
    $oauth_token|Este es el **token de acceso** de la aplicación de Twitter que anotó anteriormente.
    $oauth_token_secret|Este es el **secreto de token de acceso** de la aplicación de Twitter que anotó anteriormente.
    $destBlobName|Este es el nombre de blob de salida. El valor predeterminado es **tutorials/twitter/data/tweets.txt**. Si cambia el valor predeterminado, deberá actualizar los scripts de Windows PowerShell como corresponda.
    $trackString|El servicio web devolverá tweets relacionados con estas palabras clave. El valor predeterminado es **Azure, Cloud, HDInsight**. Si cambia el valor predeterminado, actualizará los scripts de Windows PowerShell como corresponda.
    $lineMax|El valor determina cuántos tweets leerá el script. Tomará unos tres minutos para leer 100 tweets. Puede definir un número mayor, pero aumentará el tiempo necesario para la descarga.

1. Presione **F5** para ejecutar el script. Si encuentra problemas, como solución alternativa, seleccione todas las líneas y, a continuación, presione **F8**.
2. Verá un mensaje de que se ha completado la tarea al final de la salida. Los posibles mensajes de error aparecerán en rojo.

Como procedimiento de validación, puede revisar el archivo de salida, **/tutorials/twitter/data/tweets.txt**, en el almacenamiento de blobs de Azure con un explorador de almacenamiento de Azure o PowerShell de Azure. Para obtener un script de ejemplo de Windows PowerShell de todos los archivos enumerados, consulte [Uso de Blob Storage con HDInsight][hdinsight-storage-powershell].

## <a name="create-hiveql-script"></a>Creación de un script de HiveQL
Con Azure PowerShell, puede ejecutar varias instrucciones [HiveQL](https://cwiki.apache.org/confluence/display/Hive/LanguageManual) a la vez, o empaquetar la instrucción de HiveQL en un archivo script. En este tutorial, creará un script de HiveQL. El archivo de script debe cargarse en el almacenamiento de blobs de Azure. En la siguiente sección, ejecutará el archivo de script con PowerShell de Azure.

> [!NOTE]  
> El archivo de script de Hive y un archivo que contiene 10 000 tweets se han cargado en un contenedor de Blob público. Puede omitir esta sección si va a utilizar los archivos cargados.

El script de HiveQL realizará lo siguiente:

1. **Anular la tabla tweets_raw** en caso de que ya exista.
2. **Crear la tabla tweets_raw Hive**. La tabla estructurada temporal de Hive contiene los datos para un procesamiento adicional de extracción, transformación y carga (ETL). Para obtener información sobre las particiones, consulte el [tutorial de Hive][apache-hive-tutorial].
3. **Cargar datos** desde la carpeta de origen /tutorials/twitter/data. El gran conjunto de datos de los tweets en formato JSON anidado se ha transformado en una estructura de tabla temporal de Hive.
4. **Anular la tabla de tweets** en caso de que ya exista.
5. **Crear la tabla de tweets**. Antes de poder consultar el conjunto de datos de tweets mediante Hive, hay que ejecutar otro proceso de ETL. Este proceso ETL define un esquema de tabla más detallado para los datos que hemos almacenado en la tabla "twitter_raw".
6. **Insertar tabla de sobrescritura**. Este script de Hive complejo iniciará un gran conjunto de trabajos de MapReduce realizado por el clúster de Hadoop. Dependiendo del conjunto de datos y del tamaño del clúster, este puede tardar unos 10 minutos.
7. **Insertar directorio de sobrescritura**. Ejecute una consulta y ponga el conjunto de datos en un archivo. Esta consulta devolverá una lista de los usuarios de Twitter que envían más tweets que contienen la palabra "Azure".

**Para crear un script de Hive y cargarlo en Azure**

1. Abra Windows PowerShell ISE.
2. Copie el siguiente script en el panel de scripts:

    ```powershell
    #region - variables and constants
    $clusterName = "<Existing HDInsight Cluster Name>" # Enter your HDInsight cluster name
    $subscriptionID = "<Azure Subscription ID>"

    $sourceDataPath = "/tutorials/twitter/data"
    $outputPath = "/tutorials/twitter/output"
    $hqlScriptFile = "tutorials/twitter/twitter.hql"

    $hqlStatements = @"
    set hive.exec.dynamic.partition = true;
    set hive.exec.dynamic.partition.mode = nonstrict;

    DROP TABLE tweets_raw;
    CREATE EXTERNAL TABLE tweets_raw (
        json_response STRING
    )
    STORED AS TEXTFILE LOCATION '$sourceDataPath';

    DROP TABLE tweets;
    CREATE TABLE tweets
    (
        id BIGINT,
        created_at STRING,
        created_at_date STRING,
        created_at_year STRING,
        created_at_month STRING,
        created_at_day STRING,
        created_at_time STRING,
        in_reply_to_user_id_str STRING,
        text STRING,
        contributors STRING,
        retweeted STRING,
        truncated STRING,
        coordinates STRING,
        source STRING,
        retweet_count INT,
        url STRING,
        hashtags array<STRING>,
        user_mentions array<STRING>,
        first_hashtag STRING,
        first_user_mention STRING,
        screen_name STRING,
        name STRING,
        followers_count INT,
        listed_count INT,
        friends_count INT,
        lang STRING,
        user_location STRING,
        time_zone STRING,
        profile_image_url STRING,
        json_response STRING
    );

    FROM tweets_raw
    INSERT OVERWRITE TABLE tweets
    SELECT
        cast(get_json_object(json_response, '$.id_str') as BIGINT),
        get_json_object(json_response, '$.created_at'),
        concat(substr (get_json_object(json_response, '$.created_at'),1,10),' ',
        substr (get_json_object(json_response, '$.created_at'),27,4)),
        substr (get_json_object(json_response, '$.created_at'),27,4),
        case substr (get_json_object(json_response, '$.created_at'),5,3)
            when "Jan" then "01"
            when "Feb" then "02"
            when "Mar" then "03"
            when "Apr" then "04"
            when "May" then "05"
            when "Jun" then "06"
            when "Jul" then "07"
            when "Aug" then "08"
            when "Sep" then "09"
            when "Oct" then "10"
            when "Nov" then "11"
            when "Dec" then "12" end,
        substr (get_json_object(json_response, '$.created_at'),9,2),
        substr (get_json_object(json_response, '$.created_at'),12,8),
        get_json_object(json_response, '$.in_reply_to_user_id_str'),
        get_json_object(json_response, '$.text'),
        get_json_object(json_response, '$.contributors'),
        get_json_object(json_response, '$.retweeted'),
        get_json_object(json_response, '$.truncated'),
        get_json_object(json_response, '$.coordinates'),
        get_json_object(json_response, '$.source'),
        cast (get_json_object(json_response, '$.retweet_count') as INT),
        get_json_object(json_response, '$.entities.display_url'),
        array(
            trim(lower(get_json_object(json_response, '$.entities.hashtags[0].text'))),
            trim(lower(get_json_object(json_response, '$.entities.hashtags[1].text'))),
            trim(lower(get_json_object(json_response, '$.entities.hashtags[2].text'))),
            trim(lower(get_json_object(json_response, '$.entities.hashtags[3].text'))),
            trim(lower(get_json_object(json_response, '$.entities.hashtags[4].text')))),
        array(
            trim(lower(get_json_object(json_response, '$.entities.user_mentions[0].screen_name'))),
            trim(lower(get_json_object(json_response, '$.entities.user_mentions[1].screen_name'))),
            trim(lower(get_json_object(json_response, '$.entities.user_mentions[2].screen_name'))),
            trim(lower(get_json_object(json_response, '$.entities.user_mentions[3].screen_name'))),
            trim(lower(get_json_object(json_response, '$.entities.user_mentions[4].screen_name')))),
        trim(lower(get_json_object(json_response, '$.entities.hashtags[0].text'))),
        trim(lower(get_json_object(json_response, '$.entities.user_mentions[0].screen_name'))),
        get_json_object(json_response, '$.user.screen_name'),
        get_json_object(json_response, '$.user.name'),
        cast (get_json_object(json_response, '$.user.followers_count') as INT),
        cast (get_json_object(json_response, '$.user.listed_count') as INT),
        cast (get_json_object(json_response, '$.user.friends_count') as INT),
        get_json_object(json_response, '$.user.lang'),
        get_json_object(json_response, '$.user.location'),
        get_json_object(json_response, '$.user.time_zone'),
        get_json_object(json_response, '$.user.profile_image_url'),
        json_response
    WHERE (length(json_response) > 500);

    INSERT OVERWRITE DIRECTORY '$outputPath'
    SELECT name, screen_name, count(1) as cc
        FROM tweets
        WHERE text like "%Azure%"
        GROUP BY name,screen_name
        ORDER BY cc DESC LIMIT 10;
    "@
    #endregion

    #region - Connect to Azure subscription
    Write-Host "`nConnecting to your Azure subscription ..." -ForegroundColor Green

    Try{
        Get-AzureRmSubscription
    }
    Catch{
        Connect-AzureRmAccount
    }

    Select-AzureRmSubscription -SubscriptionId $subscriptionID

    #endregion

    #region - Create a block blob object for writing the Hive script file
    Write-Host "Get the default storage account name and container name based on the cluster name ..." -ForegroundColor Green
    $myCluster = Get-AzureRmHDInsightCluster -ClusterName $clusterName
    $resourceGroupName = $myCluster.ResourceGroup
    $defaultStorageAccountName = $myCluster.DefaultStorageAccount.Replace(".blob.core.windows.net", "")
    $defaultBlobContainerName = $myCluster.DefaultStorageContainer
    Write-Host "`tThe storage account name is $defaultStorageAccountName." -ForegroundColor Yellow
    Write-Host "`tThe blob container name is $defaultBlobContainerName." -ForegroundColor Yellow

    Write-Host "Define the connection string ..." -ForegroundColor Green
    $defaultStorageAccountKey = (Get-AzureRmStorageAccountKey -ResourceGroupName $resourceGroupName -Name $defaultStorageAccountName)[0].Value
    $storageConnectionString = "DefaultEndpointsProtocol=https;AccountName=$defaultStorageAccountName;AccountKey=$defaultStorageAccountKey"

    Write-Host "Create block blob objects referencing the hql script file" -ForegroundColor Green
    $storageAccount = [Microsoft.WindowsAzure.Storage.CloudStorageAccount]::Parse($storageConnectionString)
    $storageClient = $storageAccount.CreateCloudBlobClient();
    $storageContainer = $storageClient.GetContainerReference($defaultBlobContainerName)
    $hqlScriptBlob = $storageContainer.GetBlockBlobReference($hqlScriptFile)

    Write-Host "Define a MemoryStream and a StreamWriter for writing ... " -ForegroundColor Green
    $memStream = New-Object System.IO.MemoryStream
    $writeStream = New-Object System.IO.StreamWriter $memStream
    $writeStream.Writeline($hqlStatements)
    #endregion

    #region - Write the Hive script file to Blob storage
    Write-Host "Write to the destination blob ... " -ForegroundColor Green
    $writeStream.Flush()
    $memStream.Seek(0, "Begin")
    $hqlScriptBlob.UploadFromStream($memStream)
    #endregion

    Write-Host "Completed!" -ForegroundColor Green
    ```

3. Establezca las dos primeras variables en el script

   | Variable | DESCRIPCIÓN |
   | --- | --- |
   |  $clusterName |Escriba el nombre del clúster de HDInsight donde desea ejecutar la aplicación. |
   |  $subscriptionID |Especifique el identificador de su suscripción a Azure. |
   |  $sourceDataPath |La ubicación de almacenamiento de blobs de Azure desde donde se leerán los datos de consultas de Hive. No es necesario que cambie esta variable. |
   |  $outputPath |La ubicación de almacenamiento de blobs de Azure donde las consultas de Hive generarán los resultados. No es necesario que cambie esta variable. |
   |  $hqlScriptFile |La ubicación y el nombre de archivo del archivo de script de HiveQL. No es necesario que cambie esta variable. |
4. Presione **F5** para ejecutar el script. Si encuentra problemas, como solución alternativa, seleccione todas las líneas y, a continuación, presione **F8**.
5. Verá un mensaje de que se ha completado la tarea al final de la salida. Los posibles mensajes de error aparecerán en rojo.

Como procedimiento de validación, puede revisar el archivo de salida, **//tutorials/twitter/twitter.hql**, en el almacenamiento de blobs de Azure con un explorador de almacenamiento de Azure o PowerShell de Azure. Para obtener un script de ejemplo de Windows PowerShell de todos los archivos enumerados, consulte [Uso de Blob Storage con HDInsight][hdinsight-storage-powershell].

## <a name="process-twitter-data-by-using-hive"></a>Procesamiento de datos de Twitter mediante Hive
Ya ha terminado todo el trabajo de preparación. Ahora, puede invocar el script de Hive y comprobar los resultados.

### <a name="submit-a-hive-job"></a>Envío de un trabajo de Hive
Use el siguiente script de Windows PowerShell para ejecutar el script de Hive. Tendrá que definir la primera variable.

> [!NOTE]  
> Para usar los tweets y el script de [HiveQL](https://cwiki.apache.org/confluence/display/Hive/LanguageManual) que cargó en las dos últimas secciones, establezca $hqlScriptFile en "/tutorials/twitter/twitter.hql". Para usar los que se han cargado en un blob público automáticamente, establezca $hqlScriptFile en "wasb://twittertrend@hditutorialdata.blob.core.windows.net/twitter.hql".

```powershell
#region variables and constants
$clusterName = "<Existing Azure HDInsight Cluster Name>"
$httpUserName = "admin"
$httpUserPassword = "<HDInsight Cluster HTTP User Password>"

#use one of the following
$hqlScriptFile = "wasb://twittertrend@hditutorialdata.blob.core.windows.net/twitter.hql"
$hqlScriptFile = "/tutorials/twitter/twitter.hql"

$statusFolder = "/tutorials/twitter/jobstatus"
#endregion

$myCluster = Get-AzureRmHDInsightCluster -ClusterName $clusterName
$resourceGroupName = $myCluster.ResourceGroup
$defaultStorageAccountName = $myCluster.DefaultStorageAccount.Replace(".blob.core.windows.net", "")
$defaultStorageAccountKey = (Get-AzureRmStorageAccountKey -ResourceGroupName $resourceGroupName -Name $defaultStorageAccountName)[0].Value

$defaultBlobContainerName = $myCluster.DefaultStorageContainer

#region - Invoke Hive
Write-Host "Invoke Hive ... " -ForegroundColor Green

# Create the HDInsight cluster
$pw = ConvertTo-SecureString -String $httpUserPassword -AsPlainText -Force
$httpCredential = New-Object System.Management.Automation.PSCredential($httpUserName,$pw)

Use-AzureRmHDInsightCluster -ResourceGroupName $resourceGroupName -ClusterName $clusterName -HttpCredential $httpCredential
$response = Invoke-AzureRmHDInsightHiveJob -DefaultStorageAccountName $defaultStorageAccountName -DefaultStorageAccountKey $defaultStorageAccountKey -DefaultContainer $defaultBlobContainerName -file $hqlScriptFile -StatusFolder $statusFolder #-OutVariable $outVariable

Write-Host "Display the standard error log ... " -ForegroundColor Green
$jobID = ($response | Select-String job_ | Select-Object -First 1) -replace �\s*$� -replace �.*\s�
Get-AzureRmHDInsightJobOutput -ClusterName $clusterName -JobId $jobID -DefaultContainer $defaultBlobContainerName -DefaultStorageAccountName $defaultStorageAccountName -DefaultStorageAccountKey $defaultStorageAccountKey -HttpCredential $httpCredential
#endregion
```

### <a name="check-the-results"></a>Comprobar los resultados
Use el siguiente script de Windows PowerShell para comprobar la salida del trabajo de Hive. Tendrá que definir las dos primeras variables.

```powershell
#region variables and constants
$clusterName = "<Existing Azure HDInsight Cluster Name>"

$blob = "tutorials/twitter/output/000000_0" # The name of the blob to be downloaded.
#endregion

#region - Create an Azure storage context object
Write-Host "Get the default storage account name and container name based on the cluster name ..." -ForegroundColor Green
$myCluster = Get-AzureRmHDInsightCluster -ClusterName $clusterName
$resourceGroupName = $myCluster.ResourceGroup
$defaultStorageAccountName = $myCluster.DefaultStorageAccount.Replace(".blob.core.windows.net", "")
$defaultStorageAccountKey = (Get-AzureRmStorageAccountKey -ResourceGroupName $resourceGroupName -Name $defaultStorageAccountName)[0].Value
$defaultBlobContainerName = $myCluster.DefaultStorageContainer

Write-Host "`tThe storage account name is $defaultStorageAccountName." -ForegroundColor Yellow
Write-Host "`tThe blob container name is $defaultBlobContainerName." -ForegroundColor Yellow

Write-Host "Create a context object ... " -ForegroundColor Green
$storageContext = New-AzureStorageContext -StorageAccountName $defaultStorageAccountName -StorageAccountKey $defaultStorageAccountKey
#endregion

#region - Download blob and display blob
Write-Host "Download the blob ..." -ForegroundColor Green
cd $HOME
Get-AzureStorageBlobContent -Container $defaultBlobContainerName -Blob $blob -Context $storageContext -Force

Write-Host "Display the output ..." -ForegroundColor Green
Write-Host "==================================" -ForegroundColor Green
cat "./$blob"
Write-Host "==================================" -ForegroundColor Green
#end region
```

> [!NOTE]  
> La tabla de Hive utiliza \001 como delimitador de campo. El delimitador no es visible en la salida.

Una vez que los resultados del análisis se hayan colocado en el almacenamiento de blobs de Azure, puede exportar los datos a una Base de datos SQL de Azure o SQL Server, exportar los datos a Excel con Power Query, o bien, conectar la aplicación con los datos mediante el uso de Hive ODBC. Para obtener más información, consulte [Uso de Apache Sqoop con HDInsight][hdinsight-use-sqoop], [Análisis de la información de retraso de vuelos usando HDInsight][hdinsight-analyze-flight-delay-data], [Conexión de Excel a HDInsight con Power Query][hdinsight-power-query] y [Conexión de Excel a HDInsight con Microsoft Hive ODBC Driver][hdinsight-hive-odbc].

## <a name="next-steps"></a>Pasos siguientes
En este tutorial hemos visto cómo transformar un conjunto de datos JSON no estructurado en una tabla de Apache Hive estructurada para consultar, explorar y analizar datos desde Twitter mediante HDInsight en Azure. Para obtener más información, consulte:

* [Introducción a HDInsight][hdinsight-get-started]
* [Análisis de la información de retraso de vuelos con HDInsight][hdinsight-analyze-flight-delay-data]
* [Conexión de Excel a HDInsight con Power Query][hdinsight-power-query]
* [Conexión de Excel a HDInsight con Microsoft Hive ODBC Driver][hdinsight-hive-odbc]
* [Uso de Apache Sqoop con HDInsight][hdinsight-use-sqoop]

[curl]: https://curl.haxx.se
[curl-download]: https://curl.haxx.se/download.html

[apache-hive-tutorial]: https://cwiki.apache.org/confluence/display/Hive/Tutorial

[twitter-streaming-api]: https://developer.twitter.com/en/docs/api-reference-index
[twitter-statuses-filter]: https://developer.twitter.com/en/docs/tweets/filter-realtime/api-reference/post-statuses-filter

[powershell-start]: https://technet.microsoft.com/library/hh847889.aspx
[powershell-install]: /powershell/azureps-cmdlets-docs
[powershell-script]: https://technet.microsoft.com/library/ee176961.aspx

[hdinsight-provision]: hdinsight-hadoop-provision-linux-clusters.md
[hdinsight-get-started]:hadoop/apache-hadoop-linux-tutorial-get-started.md
[hdinsight-storage-powershell]:hdinsight-hadoop-use-blob-storage.md#access-blobs-using-azure-powershell
[hdinsight-analyze-flight-delay-data]: hdinsight-analyze-flight-delay-data.md
[hdinsight-storage]: hdinsight-hadoop-use-blob-storage.md
[hdinsight-use-sqoop]:hadoop/hdinsight-use-sqoop.md
[hdinsight-power-query]:hadoop/apache-hadoop-connect-excel-power-query.md
[hdinsight-hive-odbc]:hadoop/apache-hadoop-connect-excel-hive-odbc-driver.md
