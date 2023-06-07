---
title: Criar um aplicativo .NET para Apache Spark no Windows
description: Saiba como criar seu .NET para Apache Spark aplicativo no Windows.
ms.date: 06/25/2020
ms.topic: conceptual
ms.custom: how-to
ms.openlocfilehash: 6d52e5be8c8e528880eece5a9b46fb08933c1eb3
ms.sourcegitcommit: e02d17b2cf9c1258dadda4810a5e6072a0089aee
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/01/2020
ms.locfileid: "85617659"
---
# <a name="learn-how-to-build-your-net-for-apache-spark-application-on-windows"></a>Saiba como criar seu .NET para Apache Spark aplicativo no Windows

Este artigo ensina como criar seu .NET para aplicativos Apache Spark no Windows.

[!INCLUDE [spark-preview-note](../../../includes/spark-preview-note.md)]

## <a name="prerequisites"></a>Pré-requisitos

Se você já tiver todos os pré-requisitos a seguir, pule para as etapas de [compilação](#build) .

  1. Baixar e instalar o **[SDK do .NET Core](https://dotnet.microsoft.com/download/dotnet-core/2.1)** -a instalação do SDK adicionará o `dotnet` ferramentas ao seu caminho. Há suporte para o .NET Core 2,1, 2,2 e 3,1.
  2. Instale o **[Visual Studio 2019](https://www.visualstudio.com/downloads/)** (versão 16,3 ou posterior). A versão da Comunidade é totalmente gratuita. Ao configurar sua instalação, inclua estes componentes no mínimo:
     * Desenvolvimento para área de trabalho com .NET
       * Todos os componentes necessários
         * Ferramentas de desenvolvimento do .NET Framework 4.6.1
     * Desenvolvimento multiplataforma com o .NET Core
       * Todos os componentes necessários
  3. Instale o **[Java 1,8](https://www.oracle.com/technetwork/java/javase/downloads/jdk8-downloads-2133151.html)**.
     - Selecione a versão apropriada para seu sistema operacional. Por exemplo, *jdk-8u201-windows-x64.exe* para o computador Windows x64.
     - Instale o usando o instalador e verifique se você consegue executar a `java` partir da linha de comando.
  4. Instale o **[Apache Maven 3.6.0 +](https://maven.apache.org/download.cgi)**.
     - Baixe o [Apache Maven 3.6.0](http://mirror.metrocast.net/apache/maven/maven-3/3.6.3/binaries/apache-maven-3.6.3-bin.zip).
     - Extraia para um diretório local. Por exemplo, * C:\bin\apache-Maven-3.6.0 \* .
     - Adicione o Apache Maven à sua [variável de ambiente PATH](https://www.java.com/en/download/help/path.xml). Por exemplo, *C:\bin\apache-Maven-3.6.0\Bin*.
     - Verifique se você pode executar a `mvn` partir de sua linha de comando.
  5. Instale o **[Apache Spark 2.3 +](https://spark.apache.org/downloads.html)**.
     - Baixe [Apache Spark 2.3 +](https://spark.apache.org/downloads.html) e extraia-o em uma pasta local (por exemplo, *C:\bin\spark-2.3.2-bin-hadoop2.7 \* ) usando [7-zip](https://www.7-zip.org/). (As versões do Spark com suporte são 2,3.*, 2.4.0, 2.4.1, 2.4.3 e 2.4.4)
     - Adicione uma [nova variável de ambiente](https://www.java.com/en/download/help/path.xml) `SPARK_HOME` . Por exemplo, * C:\bin\spark-2.3.2-bin-hadoop2.7 \* .

       ```powershell
       set SPARK_HOME=C:\bin\spark-2.3.2-bin-hadoop2.7\
       ```

     - Adicione o Apache Spark à sua [variável de ambiente PATH](https://www.java.com/en/download/help/path.xml). Por exemplo, *C:\bin\spark-2.3.2-bin-hadoop2.7\bin*.

       ```powershell
       set PATH=%SPARK_HOME%\bin;%PATH%
       ```

     - Verifique se você pode executar a `spark-shell` partir de sua linha de comando.
        Exemplo de saída do console:

        ```
        Welcome to
              ____              __
             / __/__  ___ _____/ /__
            _\ \/ _ \/ _ `/ __/  '_/
           /___/ .__/\_,_/_/ /_/\_\   version 2.3.2
              /_/

        Using Scala version 2.11.8 (Java HotSpot(TM) 64-Bit Server VM, Java 1.8.0_201)
        Type in expressions to have them evaluated.
        Type :help for more information.

        scala> sc
        res0: org.apache.spark.SparkContext = org.apache.spark.SparkContext@6eaa6b0c
        ```

        </details>

  6. Instale o **[WinUtils](https://github.com/steveloughran/winutils)**.
     - Baixe o `winutils.exe` binário do [repositório WinUtils](https://github.com/steveloughran/winutils). Você deve selecionar a versão do Hadoop com a qual a distribuição do Spark foi compilada. Para exammple, use Hadoop-2.7.1 para Spark 2.3.2.
     - Salve `winutils.exe` Binary em um diretório de sua escolha. Por exemplo, *C:\hadoop\bin*.
     - Defina `HADOOP_HOME` para refletir o diretório com winutils.exe (sem bin). Por exemplo, usando a linha de comando:

       ```powershell
       set HADOOP_HOME=C:\hadoop
       ```

     - Defina a variável de ambiente PATH como include `%HADOOP_HOME%\bin` . Por exemplo, usando a linha de comando:

       ```powershell
       set PATH=%HADOOP_HOME%\bin;%PATH%
       ```

Verifique se você pode executar `dotnet` o,, `java` `mvn` , `spark-shell` da linha de comando antes de passar para a próxima seção. Existe uma maneira melhor? [Abra um problema](https://github.com/dotnet/spark/issues) e sinta-se à vontade para contribuir.

> [!NOTE]
> Uma nova instância da linha de comando poderá ser necessária se qualquer variável de ambiente tiver sido atualizada.

## <a name="build"></a>Build

Para o restante deste guia, você precisará ter clonado o .NET para Apache Spark repositório em seu computador. Você pode escolher qualquer local para o repositório clonado. Por exemplo, * C:\github\dotnet-Spark \* .

```bash
git clone https://github.com/dotnet/spark.git C:\github\dotnet-spark
```

### <a name="build-net-for-apache-spark-scala-extensions-layer"></a>Compilar .NET para Apache Spark camada de extensões escalares

Quando você envia um aplicativo .NET, o .NET para Apache Spark tem a lógica necessária escrita em escalares que informa Apache Spark como lidar com suas solicitações (por exemplo, solicitação para criar uma nova sessão do Spark, solicitação para transferir dados do lado do .NET para o lado da JVM, etc.). Essa lógica pode ser encontrada no [código-fonte do .net para Spark escala](https://github.com/dotnet/spark/tree/master/src/scala).

Independentemente de você estar usando o .NET Framework ou o .NET Core, será necessário criar o .NET para Apache Spark camada de extensão escalabilidade:

```powershell
cd src\scala
mvn clean package
```

Você deve ver os JARs criados para as versões do Spark com suporte:

* `microsoft-spark-2.3.x\target\microsoft-spark-2.3.x-<version>.jar`
* `microsoft-spark-2.4.x\target\microsoft-spark-2.4.x-<version>.jar`

### <a name="build-the-net-for-spark-sample-applications"></a>Compilar o .NET para aplicativos de exemplo do Spark

Esta seção explica como criar os [aplicativos de exemplo](https://github.com/dotnet/spark/tree/master/examples) para .net para Apache Spark. Essas etapas ajudarão a compreender o processo de criação geral de qualquer aplicativo .NET para Spark.

#### <a name="using-visual-studio-for-net-framework"></a>Usando o Visual Studio para .NET Framework

  1. Abra `src\csharp\Microsoft.Spark.sln` no Visual Studio e compile o `Microsoft.Spark.CSharp.Examples` projeto na `examples` pasta (isso, por sua vez, criará também o projeto de associações .net). Se desejar, você pode escrever seu próprio código no `Microsoft.Spark.Examples` projeto (o ' input_file.jsem ' neste exemplo é um arquivo JSON com os dados com os quais você deseja criar o dataframe):
  
      ```csharp
        // Instantiate a session
        var spark = SparkSession
            .Builder()
            .AppName("Hello Spark!")
            .GetOrCreate();

        // Create initial DataFrame
        DataFrame df = spark.Read().Json(input_file.json);

        // Print schema
        df.PrintSchema();

        // Apply a filter and show results
        df.Filter(df["age"] > 21).Show();
      ```

     Depois que a compilação for bem-sucedida, você verá os binários apropriados produzidos no diretório de saída.
     Exemplo de saída do console:

      ```powershell
            Directory: C:\github\dotnet-spark\artifacts\bin\Microsoft.Spark.CSharp.Examples\Debug\net461


        Mode                LastWriteTime         Length Name
        ----                -------------         ------ ----
        -a----         3/6/2019  12:18 AM         125440 Apache.Arrow.dll
        -a----        3/16/2019  12:00 AM          13824 Microsoft.Spark.CSharp.Examples.exe
        -a----        3/16/2019  12:00 AM          19423 Microsoft.Spark.CSharp.Examples.exe.config
        -a----        3/16/2019  12:00 AM           2720 Microsoft.Spark.CSharp.Examples.pdb
        -a----        3/16/2019  12:00 AM         143360 Microsoft.Spark.dll
        -a----        3/16/2019  12:00 AM          63388 Microsoft.Spark.pdb
        -a----        3/16/2019  12:00 AM          34304 Microsoft.Spark.Worker.exe
        -a----        3/16/2019  12:00 AM          19423 Microsoft.Spark.Worker.exe.config
        -a----        3/16/2019  12:00 AM          11900 Microsoft.Spark.Worker.pdb
        -a----        3/16/2019  12:00 AM          23552 Microsoft.Spark.Worker.xml
        -a----        3/16/2019  12:00 AM         332363 Microsoft.Spark.xml
        ------------------------------------------- More framework files -------------------------------------
      ```

#### <a name="using-net-core-cli-for-net-core"></a>Usando o CLI do .NET Core para .NET Core

> [!NOTE]
> No momento, estamos trabalhando para automatizar as compilações do .NET Core para Spark .NET. Até lá, agradecemos sua paciência em executar algumas das etapas manualmente.

  1. Crie o trabalho:

      ```powershell
      cd C:\github\dotnet-spark\src\csharp\Microsoft.Spark.Worker\
      dotnet publish -f netcoreapp2.1 -r win10-x64
      ```

      Exemplo de saída do console:

      ```powershell
      PS C:\github\dotnet-spark\src\csharp\Microsoft.Spark.Worker> dotnet publish -f netcoreapp2.1 -r win10-x64
      Microsoft (R) Build Engine version 16.0.462+g62fb89029d for .NET Core
      Copyright (C) Microsoft Corporation. All rights reserved.

        Restore completed in 299.95 ms for C:\github\dotnet-spark\src\csharp\Microsoft.Spark\Microsoft.Spark.csproj.
        Restore completed in 306.62 ms for C:\github\dotnet-spark\src\csharp\Microsoft.Spark.Worker\Microsoft.Spark.Worker.csproj.
        Microsoft.Spark -> C:\github\dotnet-spark\artifacts\bin\Microsoft.Spark\Debug\netstandard2.0\Microsoft.Spark.dll
        Microsoft.Spark.Worker -> C:\github\dotnet-spark\artifacts\bin\Microsoft.Spark.Worker\Debug\netcoreapp2.1\win10-x64\Microsoft.Spark.Worker.dll
        Microsoft.Spark.Worker -> C:\github\dotnet-spark\artifacts\bin\Microsoft.Spark.Worker\Debug\netcoreapp2.1\win10-x64\publish\
      ```

  2. Compile os exemplos:

      ```powershell
      cd C:\github\dotnet-spark\examples\Microsoft.Spark.CSharp.Examples\
      dotnet publish -f netcoreapp2.1 -r win10-x64
      ```

      Exemplo de saída do console:

      ```powershell
      PS C:\github\dotnet-spark\examples\Microsoft.Spark.CSharp.Examples> dotnet publish -f netcoreapp2.1 -r win10-x64
      Microsoft (R) Build Engine version 16.0.462+g62fb89029d for .NET Core
      Copyright (C) Microsoft Corporation. All rights reserved.

        Restore completed in 44.22 ms for C:\github\dotnet-spark\src\csharp\Microsoft.Spark\Microsoft.Spark.csproj.
        Restore completed in 336.94 ms for C:\github\dotnet-spark\examples\Microsoft.Spark.CSharp.Examples\Microsoft.Spark.CSharp.Examples.csproj.
        Microsoft.Spark -> C:\github\dotnet-spark\artifacts\bin\Microsoft.Spark\Debug\netstandard2.0\Microsoft.Spark.dll
        Microsoft.Spark.CSharp.Examples -> C:\github\dotnet-spark\artifacts\bin\Microsoft.Spark.CSharp.Examples\Debug\netcoreapp2.1\win10-x64\Microsoft.Spark.CSharp.Examples.dll
        Microsoft.Spark.CSharp.Examples -> C:\github\dotnet-spark\artifacts\bin\Microsoft.Spark.CSharp.Examples\Debug\netcoreapp2.1\win10-x64\publish\
      ```

## <a name="run-the-net-for-spark-sample-applications"></a>Executar o .NET para aplicativos de exemplo do Spark

Depois de criar os exemplos, executá-los será por meio `spark-submit` de você, independentemente de você estar se concentrando .NET Framework ou no .NET Core. Verifique se você seguiu a seção de [pré-requisitos](#prerequisites) e instalou o Apache Spark.

  1. Defina a `DOTNET_WORKER_DIR` `PATH` variável de ambiente ou para incluir o caminho em que o `Microsoft.Spark.Worker` binário foi gerado (por exemplo, *C:\github\dotnet\spark\artifacts\bin\Microsoft.Spark.Worker\Debug\net461* para .NET Framework, *C:\github\dotnet-spark\artifacts\bin\Microsoft.Spark.Worker\Debug\netcoreapp2.1\win10-x64\publish* para .NET Core):

      ```powershell
      set DOTNET_WORKER_DIR=C:\github\dotnet-spark\artifacts\bin\Microsoft.Spark.Worker\Debug\netcoreapp2.1\win10-x64\publish
      ```
  
  2. Abra o PowerShell e vá para o diretório em que o binário do aplicativo foi gerado (por exemplo, *C:\github\dotnet\spark\artifacts\bin\Microsoft.Spark.CSharp.Examples\Debug\net461* para .NET Framework, *C:\github\dotnet-spark\artifacts\bin\Microsoft.Spark.CSharp.Examples\Debug\netcoreapp2.1\win10-x64\publish* para .NET Core):

      ```powershell
      cd C:\github\dotnet-spark\artifacts\bin\Microsoft.Spark.CSharp.Examples\Debug\netcoreapp2.1\win10-x64\publish
      ```

  3. A execução do aplicativo segue a estrutura básica:

     ```powershell
     spark-submit.cmd `
       [--jars <any-jars-your-app-is-dependent-on>] `
       --class org.apache.spark.deploy.dotnet.DotnetRunner `
       --master local `
       <path-to-microsoft-spark-jar> `
       <path-to-your-app-exe> <argument(s)-to-your-app>
     ```

     Aqui estão alguns exemplos que você pode executar:

     - **[Microsoft.Spark.Examples.Sql.Batch. Basic](https://github.com/dotnet/spark/blob/master/examples/Microsoft.Spark.CSharp.Examples/Sql/Batch/Basic.cs)**

         ```powershell
         spark-submit.cmd `
         --class org.apache.spark.deploy.dotnet.DotnetRunner `
         --master local `
         C:\github\dotnet-spark\src\scala\microsoft-spark-<version>\target\microsoft-spark-<version>.jar `
         Microsoft.Spark.CSharp.Examples.exe Sql.Batch.Basic %SPARK_HOME%\examples\src\main\resources\people.json
         ```

     - **[Microsoft. Spark. examples. Sql. streaming. StructuredNetworkWordCount](https://github.com/dotnet/spark/blob/master/examples/Microsoft.Spark.CSharp.Examples/Sql/Streaming/StructuredNetworkWordCount.cs)**

         ```powershell
         spark-submit.cmd `
         --class org.apache.spark.deploy.dotnet.DotnetRunner `
         --master local `
         C:\github\dotnet-spark\src\scala\microsoft-spark-<version>\target\microsoft-spark-<version>.jar `
         Microsoft.Spark.CSharp.Examples.exe Sql.Streaming.StructuredNetworkWordCount localhost 9999
         ```

     - **[Microsoft. Spark. examples. Sql. streaming. StructuredKafkaWordCount (com acesso ao Maven)](https://github.com/dotnet/spark/blob/master/examples/Microsoft.Spark.CSharp.Examples/Sql/Streaming/StructuredKafkaWordCount.cs)**

         ```powershell
         spark-submit.cmd `
         --packages org.apache.spark:spark-sql-kafka-0-10_2.11:2.3.2 `
         --class org.apache.spark.deploy.dotnet.DotnetRunner `
         --master local `
         C:\github\dotnet-spark\src\scala\microsoft-spark-<version>\target\microsoft-spark-<version>.jar `
         Microsoft.Spark.CSharp.Examples.exe Sql.Streaming.StructuredKafkaWordCount localhost:9092 subscribe test
         ```

     - **[Microsoft. Spark. examples. Sql. streaming. StructuredKafkaWordCount (jars fornecidos)](https://github.com/dotnet/spark/blob/master/examples/Microsoft.Spark.CSharp.Examples/Sql/Streaming/StructuredKafkaWordCount.cs)**

         ```powershell
         spark-submit.cmd
         --jars path\to\net.jpountz.lz4\lz4-1.3.0.jar,path\to\org.apache.kafka\kafka-clients-0.10.0.1.jar,path\to\org.apache.spark\spark-sql-kafka-0-10_2.11-2.3.2.jar,`path\to\org.slf4j\slf4j-api-1.7.6.jar,path\to\org.spark-project.spark\unused-1.0.0.jar,path\to\org.xerial.snappy\snappy-java-1.1.2.6.jar `
         --class org.apache.spark.deploy.dotnet.DotnetRunner `
         --master local `
         C:\github\dotnet-spark\src\scala\microsoft-spark-<version>\target\microsoft-spark-<version>.jar `
         Microsoft.Spark.CSharp.Examples.exe Sql.Streaming.StructuredKafkaWordCount localhost:9092 subscribe test
          ```
