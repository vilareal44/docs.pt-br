---
title: Tutorial estruturado com .NET para Apache Spark
description: Neste tutorial, você aprenderá a usar o .NET para Apache Spark para o streaming estruturado do Spark.
author: mamccrea
ms.author: mamccrea
ms.date: 06/25/2020
ms.topic: tutorial
ms.openlocfilehash: 5420fe081db1704d7af647e8c88826c1bcf614d9
ms.sourcegitcommit: e02d17b2cf9c1258dadda4810a5e6072a0089aee
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/01/2020
ms.locfileid: "85617837"
---
# <a name="tutorial-structured-streaming-with-net-for-apache-spark"></a>Tutorial: streaming estruturado com .NET para Apache Spark

Este tutorial ensina como invocar o streaming estruturado do Spark usando o .NET para Apache Spark. O streaming estruturado do Spark é o suporte de Apache Spark para o processamento de fluxos de dados em tempo real. Processamento de fluxo significa analisar dados dinâmicos conforme eles estão sendo produzidos.

Neste tutorial, você aprenderá como:

> [!div class="checklist"]
>
> * Criar e executar um aplicativo .NET para Apache Spark
> * Usar netcat para criar um fluxo de dados
> * Usar funções definidas pelo usuário e SparkSQL para analisar dados de streaming

[!INCLUDE [spark-preview-note](../../../includes/spark-preview-note.md)]

## <a name="prerequisites"></a>Pré-requisitos

Se este for seu primeiro aplicativo .NET para Apache Spark, comece com o [tutorial de introdução](get-started.md) para se familiarizar com os conceitos básicos.

## <a name="create-a-console-application"></a>Criar um aplicativo de console

1. No prompt de comando, execute os seguintes comandos para criar um novo aplicativo de console:

   ```dotnetcli
   dotnet new console -o mySparkStreamingApp
   cd mySparkStreamingApp
   ```

   O `dotnet` comando cria um `new` aplicativo do tipo `console` para você. O `-o` parâmetro cria um diretório chamado *mySparkStreamingApp* onde seu aplicativo é armazenado e o preenche com os arquivos necessários. O `cd mySparkStreamingApp` comando altera o diretório para o diretório de aplicativo que você acabou de criar.

1. Para usar o .NET para Apache Spark em um aplicativo, instale o pacote Microsoft. Spark. No console do, execute o seguinte comando:

   ```dotnetcli
   dotnet add package Microsoft.Spark
   ```

## <a name="establish-and-connect-to-a-data-stream"></a>Estabelecer e conectar-se a um fluxo de dados

Uma maneira popular de testar o processamento de fluxos é por meio de **netcat**. Netcat (também conhecido como *NC*) permite que você leia e grave em conexões de rede. Você estabelece uma conexão de rede com o netcat por meio de uma janela de terminal.

### <a name="create-a-data-stream-with-netcat"></a>Criar um fluxo de dados com netcat

1. [Baixe o netcat](https://sourceforge.net/projects/nc110/files/). Em seguida, extraia o arquivo do download do zip e acrescente o diretório extraído à variável de ambiente "PATH".

2. Para iniciar uma nova conexão, abra um novo console e execute o comando a seguir que se conecta ao localhost na porta 9999.

   No Windows:

   ```console
   nc -vvv -l -p 9999
   ```

   No Linux:

   ```console
   nc -lk 9999
   ```

   O programa Spark escuta a entrada digitada nesse prompt de comando.

### <a name="create-a-sparksession"></a>Criar um SparkSession

1. Adicione as seguintes `using` instruções adicionais à parte superior do arquivo *Program.cs* em *mySparkStreamingApp*:

   ```csharp
   using System;
   using Microsoft.Spark.Sql;
   using Microsoft.Spark.Sql.Streaming;
   using static Microsoft.Spark.Sql.Functions;
   ```

1. Adicione o código a seguir ao `Main` método para criar um novo `SparkSession` . A sessão do Spark é o ponto de entrada para programar o Spark com o conjunto de e a API dataframe.

   ```csharp
   SparkSession spark = SparkSession
        .Builder()
        .AppName("Streaming example with a UDF")
        .GetOrCreate();
   ```

   Chamar o objeto *Spark* criado acima permite que você acesse a funcionalidade Spark e dataframe em todo o programa.

### <a name="connect-to-a-stream-with-readstream"></a>Conectar-se a um fluxo com ReadStream ()

O `ReadStream()` método retorna um `DataStreamReader` que pode ser usado para ler dados de streaming no como um `DataFrame` . Inclua as informações de host e porta para informar ao aplicativo Spark onde esperar seus dados de streaming.

```csharp
DataFrame lines = spark
    .ReadStream()
    .Format("socket")
    .Option("host", hostname)
    .Option("port", port)
    .Load();
```

## <a name="register-a-user-defined-function"></a>Registrar uma função definida pelo usuário

Você pode usar UDFs, *funções definidas pelo usuário*, em aplicativos Spark para executar cálculos e análises em seus dados.

Adicione o código a seguir ao `Main` método para registrar um UDF chamado `udfArray` .

```csharp
Func<Column, Column> udfArray =
    Udf<string, string[]>((str) => new string[] { str, $"{str} {str.Length}" });
```

Esse UDF processa cada cadeia de caracteres que recebe do terminal netcat para produzir uma matriz que inclui a cadeia de caracteres original (contida em *Str*), seguida pela cadeia de caracteres original concatenada com o comprimento da cadeia de caracteres original.

Por exemplo, a inserção de *Olá, mundo* no terminal netcat produz uma matriz em que:

* matriz \[ 0] = Olá, mundo
* matriz \[ 1] = Olá, mundo 11

## <a name="use-sparksql"></a>Usar SparkSQL

Use SparkSQL para executar várias funções nos dados armazenados em seu dataframe. É comum combinar UDFs e SparkSQL a aplicar um UDF a cada linha de um dataframe.

```csharp
DataFrame arrayDF = lines.Select(Explode(udfArray(lines["value"])));
```

Esse trecho de código aplica *udfArray* a cada valor em seu dataframe, que representa cada cadeia de caracteres lida do seu terminal netcat. Aplique o método SparkSQL <xref:Microsoft.Spark.Sql.Functions.Explode%2A> para colocar cada entrada da matriz em sua própria linha. Por fim, use <xref:Microsoft.Spark.Sql.DataFrame.Select%2A> para posicionar as colunas que você produziu no novo Dataframe *arrayDF.*

## <a name="display-your-stream"></a>Exibir seu fluxo

Use <xref:Microsoft.Spark.Sql.DataFrame.WriteStream> para estabelecer características de saída, como imprimir resultados no console e exibir apenas a saída mais recente.

```csharp
StreamingQuery query = arrayDf
    .WriteStream()
    .Format("console")
    .Start();
```

## <a name="run-your-code"></a>Executar seu código

O streaming estruturado no Spark processa dados por meio de uma série de **lotes**pequenos.  Quando você executa o programa, o prompt de comando no qual você estabelece a conexão netcat permite que você comece a digitar. Cada vez que você pressiona a tecla ENTER depois de digitar dados nesse prompt de comando, o Spark considera sua entrada em um lote e executa o UDF.

### <a name="use-spark-submit-to-run-your-app"></a>Use o Spark-Submit para executar seu aplicativo

Depois de iniciar uma nova sessão do netcat, abra um novo terminal e execute o `spark-submit` comando, semelhante ao seguinte comando:

```powershell
spark-submit --class org.apache.spark.deploy.dotnet.DotnetRunner --master local /path/to/microsoft-spark-<version>.jar Microsoft.Spark.CSharp.Examples.exe Sql.Streaming.StructuredNetworkCharacterCount localhost 9999
```

> [!NOTE]
> Certifique-se de atualizar o comando acima com o caminho real para o arquivo JAR do Microsoft Spark. O comando acima também pressupõe que o servidor netcat está em execução na porta localhost 9999.

## <a name="get-the-code"></a>Obter o código

Este tutorial usa o exemplo [StructuredNetworkCharacterCount.cs](https://github.com/dotnet/spark/blob/master/examples/Microsoft.Spark.CSharp.Examples/Sql/Streaming/StructuredNetworkCharacterCount.cs) , mas há três outros exemplos de processamento de fluxo completos no github:

* [StructuredNetworkWordCount.cs](https://github.com/dotnet/spark/blob/master/examples/Microsoft.Spark.CSharp.Examples/Sql/Streaming/StructuredNetworkWordCount.cs): contagem de palavras em dados transmitidos de qualquer fonte
* [StructuredNetworkWordCountWindowed.cs](https://github.com/dotnet/spark/blob/master/examples/Microsoft.Spark.CSharp.Examples/Sql/Streaming/StructuredNetworkWordCountWindowed.cs): contagem de palavras em dados com lógica de janela
* [StructuredKafkaWordCount.cs](https://github.com/dotnet/spark/blob/master/examples/Microsoft.Spark.CSharp.Examples/Sql/Streaming/StructuredKafkaWordCount.cs): contagem de palavras em dados transmitidos de Kafka

## <a name="next-steps"></a>Próximas etapas

Avance para o próximo artigo para saber como implantar seu .NET para Apache Spark aplicativo no databricks.
> [!div class="nextstepaction"]
> [Tutorial: implantar um aplicativo .NET para Apache Spark no databricks](databricks-deployment.md)
