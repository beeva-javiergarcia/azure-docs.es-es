---
title: MapReduce y Escritorio remoto con Hadoop en HDInsight | Microsoft Docs
description: Obtenga información acerca de cómo usar Escritorio remoto para conectarse a Hadoop en HDInsight y ejecutar trabajos de MapReduce.
services: hdinsight
documentationcenter: ''
author: Blackmist
manager: jhubbard
editor: cgronlun
tags: azure-portal

ms.service: hdinsight
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 09/27/2016
ms.author: larryfr

---
# Uso de MapReduce de Hadoop en HDInsight con Escritorio remoto
[!INCLUDE [mapreduce-selector](../../includes/hdinsight-selector-use-mapreduce.md)]

En este artículo, obtendrá información sobre cómo conectarse a un clúster de Hadoop en HDInsight mediante Escritorio remoto y, a continuación, ejecutar trabajos de MapReduce mediante el comando de Hadoop.

## <a id="prereq"></a>Requisitos previos
Para completar los pasos de este artículo, necesitará lo siguiente:

* Un clúster de HDInsight basado en Windows (Hadoop en HDInsight)
* Un equipo cliente con Windows 10, Windows 8 o Windows 7

## <a id="connect"></a>Conexión con el Escritorio remoto
Habilite el Escritorio remoto para el clúster de HDInsight y conéctese a él siguiendo las instrucciones dadas en [Conexión a los clústeres de HDInsight con RDP](hdinsight-administer-use-management-portal.md#rdp).

## <a id="hadoop"></a>Uso del comando Hadoop
Una vez conectado al escritorio para el clúster de HDInsight, siga estos pasos para ejecutar un trabajo de MapReduce mediante el comando de Hadoop:

1. Desde el escritorio de HDInsight, inicie la **línea de comandos de Hadoop**. Se abrirá un nuevo símbolo del sistema en el directorio **c:\\apps\\dist\\hadoop-número de versión>**.
   
   > [!NOTE]
   > El número de versión cambia cuando se actualiza Hadoop. La variable de entorno **HADOOP\_HOME** puede usarse para encontrar la ruta de acceso. Por ejemplo, `cd %HADOOP_HOME%` cambiará los directorios al directorio de Hadoop, sin que sea necesario conocer el número de versión.
   > 
   > 
2. Para usar el comando de **Hadoop** para ejecutar un trabajo de MapReduce de ejemplo, utilice el siguiente comando:
   
        hadoop jar hadoop-mapreduce-examples.jar wordcount wasbs:///example/data/gutenberg/davinci.txt wasbs:///example/data/WordCountOutput
   
    Se inicia la clase **wordcount**, incluida en el archivo **hadoop-mapreduce-examples.jar** del directorio actual. Como entrada, usa el documento **wasbs://example/data/gutenberg/davinci.txt** y la salida se almacena en: **wasbs:///example/data/WordCountOutput**.
   
   > [!NOTE]
   > Para obtener más información acerca de este trabajo de MapReduce y los datos de ejemplo, consulte <a href="hdinsight-use-mapreduce.md">Uso de MapReduce en Hadoop de HDInsight</a>.
   > 
   > 
3. El trabajo emite detalles cuando se procesa y devuelve información similar a la siguiente cuando finaliza el trabajo:
   
        File Input Format Counters
        Bytes Read=1395666
        File Output Format Counters
        Bytes Written=337623
4. Cuando finalice el trabajo, use el siguiente comando para mostrar los archivos de salida almacenados en **wasbs://example/data/WordCountOutput**:
   
        hadoop fs -ls wasbs:///example/data/WordCountOutput
   
    Se deberían mostrar dos archivos, **\_SUCCESS** y **part-r-00000**. El archivo **part-r-00000** contiene la salida de este trabajo.
   
   > [!NOTE]
   > Algunos trabajos de MapReduce pueden dividir los resultados entre varios archivos **part-r-####**. Si es así, utilice el sufijo #### para indicar el orden de los archivos.
   > 
   > 
5. Para ver la salida, use el comando siguiente:
   
        hadoop fs -cat wasbs:///example/data/WordCountOutput/part-r-00000
   
    Esto muestra una lista de las palabras que se encuentran en el archivo **wasbs://example/data/gutenberg/davinci.txt**, junto con el número de veces que apareció cada palabra. El siguiente es un ejemplo de los datos que estarán contenidos en el archivo:
   
        wreathed        3
        wreathing       1
        wreaths         1
        wrecked         3
        wrenching       1
        wretched        6
        wriggling       1

## <a id="summary"></a>Resumen
Como se puede ver, el comando de Hadoop proporciona una manera fácil de ejecutar trabajos de MapReduce en un clúster de HDInsight y, a continuación, ver la salida del trabajo.

## <a id="nextsteps"></a>Pasos siguientes
Para obtener información general sobre los trabajos de MapReduce en HDInsight:

* [Uso de MapReduce en Hadoop de HDInsight](hdinsight-use-mapreduce.md)

Para obtener información sobre otras maneras de trabajar con Hadoop en HDInsight:

* [Uso de Hive con Hadoop en HDInsight](hdinsight-use-hive.md)
* [Uso de Pig con Hadoop en HDInsight](hdinsight-use-pig.md)

<!---HONumber=AcomDC_0928_2016-->