---
title: Tutorial de análise de sentimentos com .NET para Apache Spark e ML.NET
description: Neste tutorial, você aprenderá a usar o ML.NET com .NET para Apache Spark para análise de sentimentos.
author: mamccrea
ms.author: mamccrea
ms.date: 06/25/2020
ms.topic: tutorial
ms.openlocfilehash: 69deb30419b98536fa309547d94f59bb266e413c
ms.sourcegitcommit: e02d17b2cf9c1258dadda4810a5e6072a0089aee
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/01/2020
ms.locfileid: "85617568"
---
# <a name="tutorial-sentiment-analysis-with-net-for-apache-spark-and-mlnet"></a>Tutorial: análise de sentimentos com .NET para Apache Spark e ML.NET

Este tutorial ensina como fazer uma análise de sentimentos das revisões online usando o ML.NET e o .NET para Apache Spark. O [ml.net](http://dot.net/ml) é uma estrutura de aprendizado de máquina livre, de plataforma cruzada e gratuita. Você pode usar o ML.NET com .NET para Apache Spark para dimensionar o treinamento e a previsão de algoritmos de aprendizado de máquina.

Neste tutorial, você aprenderá como:

> [!div class="checklist"]
>
> * Criar um modelo de análise de sentimentos usando o ML.NET Model Builder no Visual Studio.
> * Crie um .NET para Apache Spark aplicativo de console.
> * Escreva e implemente uma função definida pelo usuário.
> * Execute um .NET para Apache Spark aplicativo de console.

[!INCLUDE [spark-preview-note](../../../includes/spark-preview-note.md)]

## <a name="prerequisites"></a>Pré-requisitos

* Se você ainda não desenvolveu um aplicativo .NET para Apache Spark antes, comece com o [tutorial de introdução](get-started.md) para se familiarizar com os conceitos básicos. Conclua todos os pré-requisitos para o tutorial de Introdução antes de continuar com este tutorial.

* Este tutorial usa o ML.NET Model Builder (versão prévia), uma interface visual disponível no Visual Studio. Se você ainda não tiver o Visual Studio, poderá [baixar a versão da Comunidade do Visual Studio](https://visualstudio.microsoft.com/downloads/) gratuitamente.

* [Baixar e instalar](https://marketplace.visualstudio.com/items?itemName=MLNET.07) ML.NET Model Builder (versão prévia).

* Baixe o [yelptest.csv](https://github.com/dotnet/spark/blob/master/examples/Microsoft.Spark.CSharp.Examples/MachineLearning/Sentiment/Resources/yelptest.csv) e [yelptrain.csv](https://github.com/dotnet/spark/blob/master/examples/Microsoft.Spark.CSharp.Examples/MachineLearning/Sentiment/Resources/yelptrain.csv) os conjuntos de Yelp de revisão.

## <a name="review-the-data"></a>Examinar os dados

O conjunto de informações de revisões do Yelp contém revisões online do Yelp sobre vários serviços. Abra [yelptrain.csv](https://github.com/dotnet/spark/blob/master/examples/Microsoft.Spark.CSharp.Examples/MachineLearning/Sentiment/Resources/yelptrain.csv) e observe a estrutura dos dados. A primeira coluna contém o texto de revisão e a segunda coluna contém pontuações de sentimentos. Se a pontuação de sentimentos for 1, a revisão será positiva e, se a pontuação de sentimentos for 0, a revisão será negativa.

A tabela a seguir contém dados de exemplo:

|ReviewText|Sentimento|
|-|-|
|Wow... Adorei este lugar.|    1|
|A torrada não é boa.|    0|

## <a name="build-your-machine-learning-model"></a>Crie seu modelo de aprendizado de máquina

1. Abra o Visual Studio e crie um novo aplicativo de console em C# (.NET Core). Nomeie o projeto *MLSparkModel*.

1. Em **Gerenciador de soluções**, clique com o botão direito do mouse no projeto *MLSparkModel* . Em seguida, selecione **adicionar > Machine Learning**.

1. No construtor de modelos de ML.NET, selecione o bloco **análise de sentimento** cenário.

1. Na página **Adicionar dados** , carregue o conjunto de dados *yelptrain.csv* .

1. Escolha *sentimentos* das **colunas para prever a** lista suspensa.

1. Na página **treinar** , defina o tempo de treinamento para *60 segundos* e selecione **Iniciar treinamento**. Observe o status do treinamento em **andamento**.

1. Depois de concluir o treinamento do construtor de modelos, **avalie** os resultados do treinamento. Você pode digitar frases na caixa de texto abaixo, **testar o modelo** e selecionar **prever** para ver a saída.

1. Selecione **código** e, em seguida, selecione **adicionar projetos** para adicionar o modelo ml à solução.

1. Observe que dois projetos são adicionados às suas soluções: **MLSparkModelML. ConsoleApp** e **MLSparkModelML. Model**.

1. Clique duas vezes em seu projeto *MLSpark* C# e observe que a referência de projeto a seguir foi adicionada.

   ```xml
   <ItemGroup>
       <ProjectReference Include="..\MLSparkModelML.Model\MLSparkModelML.Model.csproj" />
   </ItemGroup>
   ```

## <a name="create-a-console-app"></a>Criar um aplicativo do console

O construtor de modelos cria um aplicativo de console para você.

1. Clique com o botão direito do mouse em **MLSparkModelML. console** em Gerenciador de soluções e selecione **gerenciar pacotes NuGet**.

1. Procure **Microsoft. Spark** e instale o pacote. O **Microsoft.ml** é instalado automaticamente para você pelo construtor de modelos.

### <a name="create-a-sparksession"></a>Criar um SparkSession

1. Abra o arquivo *Program.cs* para **MLSparkModelML. ConsoleApp**. Este arquivo foi gerado automaticamente pelo construtor de modelos. Exclua as `using` instruções, o conteúdo do método Main () e a `CreateSingleDataSample` região.

1. Adicione as seguintes `using` instruções adicionais à parte superior do *Program.cs*:

   ```csharp
   using System;
   using System.Collections.Generic;
   using Microsoft.ML;
   using Microsoft.ML.Data;
   using Microsoft.Spark.Sql;
   using MLSparkModelML.Model;
   ```

1. Altere o `DATA_FILEPATH` para o caminho do seu *yelptest.csv*.

1. Adicione o código a seguir ao `Main` método para criar um novo `SparkSession` . A sessão do Spark é o ponto de entrada para programar o Spark com o conjunto de e a API dataframe.

   ```csharp
   SparkSession spark = SparkSession
        .Builder()
        .AppName(".NET for Apache Spark Sentiment Analysis")
        .GetOrCreate();
   ```

   Chamar o objeto *Spark* criado acima permite que você acesse a funcionalidade Spark e dataframe em todo o programa.

### <a name="create-a-dataframe-and-print-to-console"></a>Criar um dataframe e imprimir no console

Leia em Yelp dados de revisão do arquivo de *yelptest.csv* como um `DataFrame` . Incluir `header` e `inferSchema` opções. A `header` opção lê a primeira linha de *yelptest.csv* como nomes de coluna em vez de dados. A `inferSchema` opção infere os tipos de coluna com base nos dados.

```csharp
DataFrame df = spark
    .ReadStream()
    .Option("header", true)
    .Option("inferSchema", true)
    .Csv(DATA_FILEPATH);

df.Show();
```

### <a name="register-a-user-defined-function"></a>Registrar uma função definida pelo usuário

Você pode usar UDFs, *funções definidas pelo usuário*, em aplicativos Spark para fazer cálculos e análises em seus dados. Neste tutorial, você usará o ML.NET com um UDF para avaliar cada revisão de Yelp.

Adicione o código a seguir ao `Main` método para registrar um UDF chamado `MLudf` .

```csharp
spark.Udf()
    .Register<string, bool>("MLudf", predict);
```

Esse UDF usa uma cadeia de caracteres de revisão Yelp como entrada e produz verdadeiro ou falso para sentimentos positivos ou negativos, respectivamente. Ele usa o método *Predict ()* que você define em uma etapa posterior.

### <a name="use-spark-sql-to-call-the-udf"></a>Usar o Spark SQL para chamar o UDF

Agora que você leu os dados e incorporou o ML, use o Spark SQL para chamar o UDF que executará a análise de sentimentos em cada linha do dataframe. Adicione o código a seguir ao método `Main`:

```csharp
// Use Spark SQL to call ML.NET UDF
// Display results of sentiment analysis on reviews
df.CreateOrReplaceTempView("Reviews");
DataFrame sqlDf = spark.Sql("SELECT ReviewText, MLudf(ReviewText) FROM Reviews");
sqlDf.Show();

// Print out first 20 rows of data
// Prevent data getting cut off by setting truncate = 0
sqlDf.Show(20, 0, false);

spark.Stop();
```

### <a name="create-predict-method"></a>Criar método Predict ()

Adicione o código a seguir antes do seu `Main()` método. Esse código é semelhante ao que é produzido pelo construtor de modelos no *ConsumeModel.cs*. Mover esse método para o console carrega o carregamento do modelo cada vez que você executa seu aplicativo.

```csharp
private static readonly PredictionEngine<ModelInput, ModelOutput> _predictionEngine;

static Program()
{
    MLContext mlContext = new MLContext();
    ITransformer model = mlContext.Model.Load("MLModel.zip", out DataViewSchema schema);
    _predictionEngine = mlContext.Model.CreatePredictionEngine<ModelInput, ModelOutput>(model);
}

static bool predict(string text)
{
    ModelInput input = new ModelInput
    {
        ReviewText = text
    };

    return _predictionEngine.Predict(input).Prediction;
}
```

## <a name="add-the-model-to-your-console-app"></a>Adicionar o modelo ao seu aplicativo de console

Em Gerenciador de Soluções, copie o arquivo de *MLModel.zip* do projeto **MLSparkModelML. Model** e cole-o no projeto **MLSparkModelML. ConsoleApp** . Uma referência é adicionada automaticamente em *MLSparkModelML. ConsoleApp. csproj*.

## <a name="run-your-code"></a>Executar seu código

Use `spark-submit` para executar seu código. Navegue até a pasta raiz do aplicativo de console usando o prompt de comando e execute os comandos a seguir.

Primeiro, limpe e publique seu aplicativo.

```dotnetcli
dotnet clean
dotnet publish
```

Em seguida, navegue até a pasta de publicação do aplicativo de console e execute o comando a seguir `spark-submit` . Lembre-se de atualizar o comando com o caminho real do arquivo JAR do Microsoft Spark.

```dotnetcli
%SPARK_HOME%\bin\spark-submit --class org.apache.spark.deploy.dotnet.DotnetRunner --master local microsoft-spark-2.4.x-0.10.0.jar dotnet MLSparkModelML.ConsoleApp.dll
```

## <a name="get-the-code"></a>Obter o código

Este tutorial é semelhante ao código do exemplo [análise de sentimento com Big data](https://github.com/dotnet/spark/tree/master/examples/Microsoft.Spark.CSharp.Examples/MachineLearning/Sentiment) .

## <a name="next-steps"></a>Próximas etapas

Avance para o próximo artigo para saber como fazer streaming estruturado com o .NET para Apache Spark.
> [!div class="nextstepaction"]
> [Tutorial: streaming estruturado com .NET para Apache Spark](streaming.md)
