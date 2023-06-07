---
title: Árvores de expressão
description: Saiba mais sobre árvores de expressão no .NET Core e como usá-las para representar o código como estruturas que você pode examinar, modificar e executar.
ms.date: 06/20/2016
ms.technology: csharp-advanced-concepts
ms.assetid: aceb4719-0d5a-4b19-b01f-b51063bcc54f
ms.openlocfilehash: 62f5b93097ee8ad2177fc0bb484c656408f91f30
ms.sourcegitcommit: 7476c20d2f911a834a00b8a7f5e8926bae6804d9
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/11/2020
ms.locfileid: "88062503"
---
# <a name="expression-trees"></a>Árvores de expressão

Se tiver usado o LINQ, você tem experiência com uma rica biblioteca em que os tipos `Func` fazem parte do conjunto de API. (Se você não estiver familiarizado com o LINQ, provavelmente deseja ler [o tutorial do LINQ](linq/index.md) e o artigo sobre [expressões lambda](language-reference/operators/lambda-expressions.md) antes desta.) As *árvores de expressão* fornecem uma interação mais rica com os argumentos que são funções.

Você escreve argumentos de função, normalmente usando expressões lambda, quando cria consultas LINQ. Em uma consulta LINQ típica, esses argumentos de função são transformados em um delegado que o compilador cria.

Quando quiser ter uma interação mais avançada, você precisa usar *Árvores de expressão*.
Árvores de expressão representam o código como uma estrutura que você pode examinar, modificar ou executar. Essas ferramentas oferecem a capacidade de manipular o código em tempo de execução. Você pode escrever código que examina algoritmos em execução ou injeta novos recursos. Em cenários mais avançados, você pode modificar algoritmos em execução e até mesmo converter expressões C# para outro formato para execução em outro ambiente.

Provavelmente, você já escreveu código usando Árvores de expressão. APIs do LINQ do Entity Framework aceitam Árvores de expressão como os argumentos para o padrão de expressão de consulta do LINQ.
Isso permite que o [Entity Framework](/ef/) converta a consulta que você escreveu em C# em SQL, que é executado no mecanismo do banco de dados. Outro exemplo é [Moq](https://github.com/Moq/moq), que é uma estrutura de simulação popular para .NET.

As seções restantes deste tutorial explorarão o que são as árvores de expressão, examinarão as classes de estrutura que dão suporte a árvores de expressão e mostrarão como trabalhar com árvores de expressão. Você aprenderá a ler árvores de expressão, criar árvores de expressão, criar árvores de expressão modificadas e executar o código representado pelas árvores de expressão. Após a leitura, você estará pronto para usar essas estruturas para criar algoritmos adaptáveis avançados.

1. [Árvores de Expressão Explicadas](expression-trees-explained.md)

    Compreender a estrutura e os conceitos por trás das *Árvores de Expressão*.

2. [Tipos de Framework com suporte a árvores de expressão](expression-classes.md)

    Saiba mais sobre as estruturas e classes que definem e manipulam as árvores de expressão.

3. [Executando Expressões](expression-trees-execution.md)

    Saiba como converter uma árvore de expressão representada como uma expressão lambda em um delegado e como executar o delegado resultante.

4. [Interpretando Expressões](expression-trees-interpreting.md)

    Saiba como percorrer e examinar *árvores de expressão* para entender que código a árvore de expressão representa.

5. [Compilando Expressões](expression-trees-building.md)

    Saiba como construir os nós de uma árvore de expressão e compilar árvores de expressão.

6. [Traduzindo Expressões](expression-trees-translating.md)

    Saiba como compilar uma cópia modificada de uma árvore de expressão ou converter uma árvore de expressão em um formato diferente.

7. [Resumindo](expression-trees-summary.md)

    Examine informações sobre as árvores de expressão.
