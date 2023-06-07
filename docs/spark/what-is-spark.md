---
title: O que é Apache Spark?
description: Saiba mais sobre cenários de Apache Spark e Big Data.
ms.date: 10/15/2019
ms.topic: conceptual
ms.custom: mvc
ms.openlocfilehash: cde66c4084b7c86e1b78d89c2bad94402dbd7d60
ms.sourcegitcommit: b7a8b09828bab4e90f66af8d495ecd7024c45042
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/04/2020
ms.locfileid: "87555988"
---
# <a name="what-is-apache-spark"></a>O que é Apache Spark?

[Apache Spark](https://spark.apache.org/) é uma estrutura de processamento paralelo de software livre que dá suporte ao processamento na memória para melhorar o desempenho de aplicativos que analisam Big Data. As soluções de Big data são projetadas para lidar com os dados muito grandes ou complexos para os bancos tradicionais de dados. O Spark processa grandes quantidades de dados na memória, o que é muito mais rápido do que as alternativas baseadas em disco.

## <a name="common-big-data-scenarios"></a>Cenários comuns de Big Data

Você pode considerar uma arquitetura de Big Data se precisar armazenar e processar grandes volumes de dados, transformar dados não estruturados ou processar dados de streaming. O Spark é um mecanismo de processamento distribuído de finalidade geral que pode ser usado para vários cenários de Big Data.

### <a name="extract-transform-and-load-etl"></a>ETL (extrair, transformar e carregar)

[Extração, transformação e carregamento (ETL)](/azure/architecture/data-guide/relational-data/etl) é o processo de coleta de dados de uma ou várias fontes, modificação dos dados e movimentação dos dados para um novo armazenamento de dados. Há várias maneiras de transformar dados, incluindo:

* Filtragem
* Classificação
* Agregar
* Adição
* Limpe
* Eliminação
* Validando

### <a name="real-time-data-stream-processing"></a>Processamento de fluxo de dados em tempo real

Os dados em tempo real e de streaming são dados em movimento. A telemetria de dispositivos IoT, weblogs e cliques são exemplos de dados de streaming. Os dados em tempo real podem ser processados para fornecer informações úteis, como análise geoespacial, monitoramento remoto e detecção de anomalias. Assim como os dados relacionais, você pode filtrar, agregar e preparar dados de streaming antes de mover os dados para um coletor de saída. O Apache Spark dá suporte ao [processamento de fluxo de dados em tempo real](/azure/architecture/data-guide/big-data/real-time-processing) por meio do [streaming do Spark](https://spark.apache.org/streaming/).

### <a name="batch-processing"></a>Processamento em lotes

O [processamento em lotes](/azure/architecture/data-guide/big-data/batch-processing) é o processamento de Big data em repouso. Você pode filtrar, agregar e preparar conjuntos de grandes volumes de trabalho usando trabalhos de longa execução em paralelo.

### <a name="machine-learning-through-mllib"></a>Aprendizado de máquina por meio do MLlib

O Machine Learning é usado para problemas analíticos avançados. Seu computador pode usar dados existentes para prever ou prever comportamentos, resultados e tendências futuros. A biblioteca de Machine Learning do Apache Spark, [MLlib](https://spark.apache.org/mllib/), contém vários algoritmos e utilitários de aprendizado de máquina.

### <a name="graph-processing-through-graphx"></a>Processamento de grafo por meio de GraphX

Um grafo é uma coleção de nós conectados por bordas. Você pode usar um banco de dados de grafo se você tiver dado de hierarquia ou dados com relações interconectadas. Você pode processar esses dados usando a API [GraphX](https://spark.apache.org/graphx/) do Apache Spark.

### <a name="sql-and-structured-data-processing-with-spark-sql"></a>Processamento de dados estruturado e SQL com Spark SQL

Se você estiver trabalhando com dados estruturados (formatados), poderá usar consultas SQL em seu aplicativo Spark usando o [Spark SQL](https://spark.apache.org/sql/).

## <a name="apache-spark-architecture"></a>Arquitetura de Apache Spark

Apache Spark, que usa a arquitetura mestre/de trabalho, tem três componentes principais: o driver, executores e Gerenciador de cluster.

![Arquitetura de Apache Spark](media/spark-architecture.png)

### <a name="driver"></a>Driver

O driver consiste em seu programa, como um aplicativo de console em C# e uma sessão do Spark. A sessão do Spark leva seu programa e divide-o em tarefas menores que são manipuladas pelos executores.

### <a name="executors"></a>Executores

Cada executor, ou nó de trabalho, recebe uma tarefa do driver e executa essa tarefa. Os executores residem em uma entidade conhecida como um cluster.

### <a name="cluster-manager"></a>Gerenciador de cluster

O Gerenciador de cluster se comunica com o driver e os executores para:

* Gerenciar alocação de recursos
* Gerenciar divisão de programa
* Gerenciar a execução do programa

## <a name="language-support"></a>Suporte ao idioma

O Apache Spark dá suporte às seguintes linguagens de programação:

* Scala
* Python
* Java
* SQL
* R
* Linguagens .NET (C#/F #)

## <a name="spark-apis"></a>APIs do Spark

O Apache Spark dá suporte às seguintes APIs:

* [API de escala do Spark](https://spark.apache.org/docs/2.2.0/api/scala/index.html)
* [API do Spark Java](https://spark.apache.org/docs/2.2.0/api/java/index.html)
* [API do Python do Spark](https://spark.apache.org/docs/2.2.0/api/python/index.html)
* [API do Spark R](https://spark.apache.org/docs/2.2.0/api/R/index.html)
* [Spark SQL](https://spark.apache.org/docs/latest/api/sql/index.html), funções internas

## <a name="next-steps"></a>Próximas etapas

Saiba como você pode usar Apache Spark em seu aplicativo .NET. Com o .NET para Apache Spark, os desenvolvedores com experiência .NET e lógica de negócios podem escrever Big Data consultas em C# e F #.
> [!div class="nextstepaction"]
> [O que é o .NET para Apache Spark](what-is-apache-spark-dotnet.md)
