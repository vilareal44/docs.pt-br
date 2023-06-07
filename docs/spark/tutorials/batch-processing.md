---
title: Tutorial de processamento em lote com .NET para Apache Spark
description: Saiba como fazer o processamento em lotes usando o .NET para Apache Spark.
author: mamccrea
ms.author: mamccrea
ms.date: 06/25/2020
ms.topic: tutorial
ms.openlocfilehash: dbc3ab5cc4bd7f438e9f3f8e5d36c764d785ce4b
ms.sourcegitcommit: e02d17b2cf9c1258dadda4810a5e6072a0089aee
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/01/2020
ms.locfileid: "85618279"
---
# <a name="tutorial-do-batch-processing-with-net-for-apache-spark"></a>Tutorial: fazer processamento em lote com .NET para Apache Spark

Neste tutorial, você aprenderá a fazer o processamento em lotes usando o .NET para Apache Spark. O processamento em lotes é a transformação de dados em repouso, o que significa que os dados de origem já foram carregados no armazenamento de dados.

Em geral, o processamento em lotes é executado em conjuntos de grandes volumes de valores simples, que precisam ser preparados para análise posterior. O processamento de log e o data warehousing são cenários comuns de processamento em lote. Nesse cenário, você analisa informações sobre projetos do GitHub, como o número de vezes que projetos diferentes foram bifurcados ou como os projetos foram atualizados.

Neste tutorial, você aprenderá como:

> [!div class="checklist"]
>
> * Criar e executar um aplicativo .NET para Apache Spark
> * Ler dados em um dataframe e prepará-los para análise
> * Processar os dados usando o Spark SQL

[!INCLUDE [spark-preview-note](../../../includes/spark-preview-note.md)]

## <a name="prerequisites"></a>Pré-requisitos

Se esta for a primeira vez que você usa o .NET para Apache Spark, confira o tutorial introdução ao [.net para Apache Spark](get-started.md) para saber como preparar seu ambiente e executar seu primeiro aplicativo .net para Apache Spark.

## <a name="download-the-sample-data"></a>Baixe os dados de exemplo

O [GHTorrent](http://ghtorrent.org/) monitora todos os eventos públicos do GitHub, como informações sobre projetos, confirmações e inspetores, e armazena os eventos e sua estrutura em bancos de dados. Os dados coletados em períodos de tempo diferentes estão disponíveis como arquivos mortos para download. Como os arquivos de despejo são muito grandes, este guia usa uma [versão truncada do arquivo de despejo](https://github.com/dotnet/spark/tree/master/examples/Microsoft.Spark.CSharp.Examples/Sql/Batch/projects_smaller.csv) que pode ser baixada do github.

> [!NOTE]
> O conjunto de GHTorrent é distribuído sob um esquema de licenciamento duplo ([Creative Commons +](https://wiki.creativecommons.org/wiki/CCPlus)). Para usos não comerciais (incluindo, mas não se limitando a, os usos educativos, de pesquisa ou pessoais), o conjunto de pontos é distribuído na [licença CC por SA](https://creativecommons.org/licenses/by-sa/4.0/).

## <a name="create-a-console-application"></a>Criar um aplicativo de console

1. No prompt de comando, execute os seguintes comandos para criar um novo aplicativo de console:

   ```dotnetcli
   dotnet new console -o mySparkBatchApp
   cd mySparkBatchApp
   ```

   O `dotnet` comando cria um `new` aplicativo do tipo `console` para você. O `-o` parâmetro cria um diretório chamado *mySparkBatchApp* onde seu aplicativo é armazenado e o preenche com os arquivos necessários. O `cd mySparkBatchApp` comando altera o diretório para o diretório de aplicativo que você acabou de criar.

1. Para usar o .NET para Apache Spark em um aplicativo, instale o pacote Microsoft. Spark. No console do, execute o seguinte comando:

   ```dotnetcli
   dotnet add package Microsoft.Spark
   ```

## <a name="create-a-sparksession"></a>Criar um SparkSession

1. Adicione as instruções adicionais a seguir `using` à parte superior do arquivo *Program.cs* no *mySparkBatchApp*.

   ```csharp
   using System;
   using Microsoft.Spark.Sql;
   using static Microsoft.Spark.Sql.Functions;
   ```

1. Adicione o código a seguir ao namespace do projeto. *s_referenceData* é usado posteriormente no programa para filtrar com base na data.

   ```csharp
   static readonly DateTime s_referenceDate = new DateTime(2015, 10, 20);
   ```

1. Adicione o código a seguir dentro do método Main para estabelecer um novo SparkSession. O SparkSession é o ponto de entrada para programação do Spark com a API do conjunto de e do DataSet. Ao chamar o `spark` objeto, você pode acessar a funcionalidade Spark e dataframe em todo o seu programa.

   ```csharp
   SparkSession spark = SparkSession
        .Builder()
        .AppName("GitHub and Spark Batch")
        .GetOrCreate();
   ```

## <a name="prepare-the-data"></a>Preparar os dados

1. Leia o arquivo de entrada em um `DataFrame` , que é uma coleção distribuída de dados organizados em colunas nomeadas. Você pode definir as colunas para os dados por meio de <xref:Microsoft.Spark.Sql.DataFrame.Schema%2A> . Use o <xref:Microsoft.Spark.Sql.DataFrame.Show%2A> método para exibir os dados em seu dataframe. Certifique-se de atualizar o caminho do arquivo CSV para o local dos dados do GitHub que você baixou.

   ```csharp
   DataFrame projectsDf = spark
       .Read()
       .Schema("id INT, url STRING, owner_id INT, " +
       "name STRING, descriptor STRING, language STRING, " +
       "created_at STRING, forked_from INT, deleted STRING" +
       "updated_at STRING")
       .Csv("filepath");

   projectsDf.Show();
   ```

1. Use o <xref:Microsoft.Spark.Sql.DataFrame.Na%2A> método para descartar linhas com valores de na (NULL) e o <xref:Microsoft.Spark.Sql.DataFrame.Drop%2A> método para remover determinadas colunas de seus dados. Isso ajuda a evitar erros se você tentar analisar dados nulos ou colunas que não são relevantes para sua análise final.

   ```csharp
   // Drop any rows with NA values
   DataFrameNaFunctions dropEmptyProjects = projectsDf.Na();
   DataFrame cleanedProjects = dropEmptyProjects.Drop("any");

   // Remove unnecessary columns
   cleanedProjects = cleanedProjects.Drop("id", "url", "owner_id");
   cleanedProjects.Show();
   ```

## <a name="analyze-the-data"></a>Analisar os dados

O Spark SQL permite fazer chamadas SQL em seus dados. É comum combinar funções definidas pelo usuário e o Spark SQL para aplicar uma função definida pelo usuário a todas as linhas do dataframe.

Você pode chamar especificamente `spark.Sql` para imitar chamadas SQL padrão vistas em outros tipos de aplicativos. Você também pode chamar métodos como <xref:Microsoft.Spark.Sql.DataFrame.GroupBy%2A> e <xref:Microsoft.Spark.Sql.DataFrame.Agg%2A> para combinar, filtrar e executar cálculos em seus dados especificamente.

O objetivo deste aplicativo é obter algumas informações sobre os dados de projetos do GitHub. Adicione os trechos de código a seguir ao seu programa para analisar os dados.

1. Adicionar o seguinte bloco de código localiza o número de vezes que cada idioma foi bifurcado. Primeiro, os dados são agrupados por linguagem. Em seguida, o número médio de bifurcações de cada idioma é tirado.

   ```csharp
   // Average number of times each language has been forked
   DataFrame groupedDF = cleanedProjects
       .GroupBy("language")
       .Agg(Avg(cleanedProjects["forked_from"]);
   ```

1. Adicione o seguinte bloco de código para ordenar o número médio de bifurcações em ordem decrescente para ver quais idiomas são os mais bifurcados. Ou seja, o maior número de bifurcações será exibido primeiro.

   ```csharp
   // Sort by most forked languages first
   groupedDF.OrderBy(Desc("avg(forked_from)")).Show();
   ```

1. O próximo bloco de código mostra como os projetos recentemente foram atualizados. Você registra uma nova função definida pelo usuário chamada *MyUDF* e a compara com uma data, *s_referenceDate*, que foi declarado no início do tutorial. A data de cada projeto é comparada com a data de referência. Em seguida, o Spark SQL é usado para chamar o UDF em cada linha dos dados para analisar cada projeto no conjunto de dados.

   ```csharp
   spark.Udf().Register<string, bool>(
       "MyUDF",
       (date) => DateTime.TryParse(date, out DateTime convertedDate) &&
           (convertedDate > s_referenceDate);
   cleanedProjects.CreateOrReplaceTempView("dateView");

   DataFrame dateDf = spark.Sql(
       "SELECT *, MyUDF(dateView.updated_at) AS datebefore FROM dateView");
   dateDf.Show();
   ```

1. Chame `spark.Stop()` para terminar o SparkSession.

## <a name="use-spark-submit-to-run-your-app"></a>Use o Spark-Submit para executar seu aplicativo

1. Use o seguinte comando para compilar seu aplicativo .NET:

   ```dotnetcli
   dotnet build
   ```

1. Execute seu aplicativo com `spark-submit` . Certifique-se de atualizar o comando a seguir com os caminhos reais para o arquivo JAR do Microsoft Spark.

   ```console
   spark-submit --class org.apache.spark.deploy.dotnet.DotnetRunner --master local /<path>/to/microsoft-spark-<version>.jar dotnet /<path>/to/netcoreapp<version>/GitHubProjects.dll
   ```

## <a name="get-the-code"></a>Obter o código

Você pode ver a [solução completa](https://github.com/dotnet/spark/blob/master/examples/Microsoft.Spark.CSharp.Examples/Sql/Batch/GitHubProjects.cs) no github.

## <a name="next-steps"></a>Próximas etapas

Avance para o próximo artigo para saber como processar dados de streaming com o .NET para Apache Spark.
> [!div class="nextstepaction"]
> [Tutorial: streaming estruturado com .NET para Apache Spark](streaming.md)
