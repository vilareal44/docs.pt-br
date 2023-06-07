---
title: O que é o .NET para o Apache Spark?
description: Saiba mais sobre o .NET para Apache Spark, uma estrutura de análise de Big Data gratuita de software livre e multiplataforma que usa o Spark em qualquer lugar em que você escreva o código .NET.
author: mamccrea
ms.topic: overview
ms.date: 06/25/2020
ms.openlocfilehash: 2c04861dabe604b52df583cd5a7eecc5ff8e5481
ms.sourcegitcommit: e02d17b2cf9c1258dadda4810a5e6072a0089aee
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/01/2020
ms.locfileid: "85621854"
---
# <a name="what-is-net-for-apache-spark"></a>O que é o .NET para o Apache Spark?

[Apache Spark](what-is-spark.md) é um mecanismo de processamento distribuído de finalidade geral para análise em conjuntos de dados grandes – normalmente terabytes ou petabytes de dados. Com o .NET para Apache Spark, o suporte .NET gratuito, de software livre e multiplataforma para a estrutura de análise de Big Data de código aberto popular, agora você pode adicionar o poder de Apache Spark aos seus aplicativos de Big Data usando as linguagens que você já conhece.

[!INCLUDE [spark-preview-note](../../includes/spark-preview-note.md)]

## <a name="why-choose-net-for-apache-spark"></a>Por que escolher o .NET para Apache Spark?

O .NET para Apache Spark capacita os desenvolvedores com a experiência .NET ou com bases de código para participar do mundo da análise de Big Data. O .NET para Apache Spark fornece APIs de alto desempenho para usar o Spark do C# e do F #. Com C# e F#, você pode acessar:

* Dataframe e SparkSQL para trabalhar com dados estruturados.
* Streaming estruturado do Spark para trabalhar usando dados de streaming.
* Spark SQL para gravar consultas com sintaxe SQL.
* Integração de Machine Learning para treinamento e previsão mais rápidos (ou seja, use .NET para Apache Spark junto com [ml.net](https://dot.net/ml)).

O .NET para Apache Spark está em conformidade com o .NET Standard, uma especificação formal das APIs do .NET comuns em implementações do .NET. Isso significa que você pode usar o .NET para Apache Spark em qualquer lugar em que você escreva código .NET, permitindo que você reutilize todo o conhecimento, as habilidades, o código e as bibliotecas que você já tem como um desenvolvedor do .NET.

O .NET para Apache Spark é executado no Windows, no Linux e no macOS usando o .NET Core. Ele também é executado no Windows usando o .NET Framework. Você pode implantar seus aplicativos em todos os principais provedores de nuvem, incluindo Azure HDInsight Spark, Amazon EMR Spark, Azure Databricks e Databricks na AWS.

## <a name="net-for-apache-spark-architecture"></a>.NET para arquitetura de Apache Spark

A ligação de linguagem C#/F # para Spark é escrita em uma nova camada de interoperabilidade do Spark, que oferece extensibilidade mais fácil. Essa nova camada de interoperabilidade do Spark foi escrita usando as práticas recomendadas para extensão de linguagem e otimiza a interoperabilidade e o desempenho. A longo prazo, essa extensibilidade pode ser usada para adicionar suporte a outros idiomas no Spark.

> [!div class="mx-imgBorder"]
> ![.NET para arquitetura de Apache Spark](media/dotnet-spark-architecture.png)

Você pode aprender sobre o suporte de interoperabilidade para extensões de linguagem do Spark da [proposta](https://issues.apache.org/jira/browse/SPARK-26257).

## <a name="net-for-apache-spark-performance"></a>Desempenho do .NET para Apache Spark

Quando comparado com o Python e escalabilidade usando o parâmetro de [comparação de TPC-H](http://www.tpc.org/tpch/), o .net para Apache Spark funciona bem na maioria dos casos e é 2x mais rápido do que o Python quando o desempenho da função definida pelo usuário é crítico. Há um esforço contínuo para melhorar o desempenho do benchmark.

Para fazer seu próprio benchmark, consulte os parâmetros de comparação disponíveis no [.net para Apache Spark GitHub](https://github.com/dotnet/spark/tree/master/benchmark).

## <a name="net-for-apache-spark-roadmap"></a>Mapa do .NET para Apache Spark

Saiba mais sobre os planos de curto e longo prazo do [.net oficial para o roteiro de Apache Spark](https://github.com/dotnet/spark/blob/master/ROADMAP.md).

## <a name="net-foundation"></a>.NET Foundation

O projeto .NET para Apache Spark faz parte do [.NET Foundation.](https://www.dotnetfoundation.org/)

## <a name="contributions"></a>Contribuições

A equipe do .NET para Apache Spark incentiva contribuições, tanto em problemas do GitHub quanto em solicitações de pull. Primeiro, procure um [problema existente](https://github.com/dotnet/spark/issues). Se você não conseguir encontrar um problema existente, [abra um novo problema](https://github.com/dotnet/spark/issues?utf8=%E2%9C%93&q=is%3Aissue+is%3Aopen+).

## <a name="next-steps"></a>Próximas etapas

Experimente o .NET para Apache Spark.
> [!div class="nextstepaction"]
> [Tutorial: introdução ao .NET para Apache Spark](./tutorials/get-started.md)
