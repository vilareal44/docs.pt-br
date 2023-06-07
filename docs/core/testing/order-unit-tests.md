---
title: Solicitar testes de unidade
description: Saiba como solicitar testes de unidade com o .NET Core.
author: IEvangelist
ms.author: dapine
ms.date: 05/18/2020
zone_pivot_groups: unit-testing-framework-set-one
ms.openlocfilehash: eb426b790e0623b0cf233a763e93d2bd501b8034
ms.sourcegitcommit: 4ad2f8920251f3744240c3b42a443ffbe0a46577
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/08/2020
ms.locfileid: "86100815"
---
# <a name="order-unit-tests"></a>Solicitar testes de unidade

Ocasionalmente, talvez você queira que os testes de unidade sejam executados em uma ordem específica. O ideal é que a ordem em que os testes de unidade sejam executados _não_ seja importante, e é uma [prática recomendada](unit-testing-best-practices.md) evitar a ordenação de testes de unidade. Independentemente disso, pode ser necessário fazer isso. Nesse caso, este artigo demonstra como ordenar execuções de teste.

Se você preferir procurar o código-fonte, consulte o repositório solicitar exemplo de [testes de unidade do .NET Core](/samples/dotnet/samples/order-unit-tests-cs) .

> [!TIP]
> Além dos recursos de pedidos descritos neste artigo, considere a [criação de listas de reprodução personalizadas com o Visual Studio](/visualstudio/test/run-unit-tests-with-test-explorer?view=vs-2019#create-custom-playlists) como alternativa.

:::zone pivot="mstest"

## <a name="order-alphabetically"></a>Ordem alfabética

Com MSTest, os testes são ordenados automaticamente pelo seu nome de teste.

> [!NOTE]
> Um teste chamado `Test14` será executado antes `Test2` mesmo que o número `2` seja menor que `14` . Isso ocorre porque, o nome do teste Order usa o nome de texto do teste.

:::code language="csharp" source="~/dotnet-samples/csharp/unit-testing/MSTest.Project/ByAlphabeticalOrder.cs":::

:::zone-end
:::zone pivot="xunit"

A estrutura de teste do xUnit permite mais granularidade e controle da ordem de execução de teste. Você implementa as `ITestCaseOrderer` `ITestCollectionOrderer` interfaces e para controlar a ordem dos casos de teste para uma classe ou coleções de teste.

## <a name="order-by-test-case-alphabetically"></a>Ordenar por caso de teste em ordem alfabética

Para ordenar os casos de teste pelo nome do método, você implementa o `ITestCaseOrderer` e fornece um mecanismo de ordenação.

:::code language="csharp" source="~/dotnet-samples/csharp/unit-testing/XUnit.TestProject/Orderers/AlphabeticalOrderer.cs":::

Em seguida, em uma classe de teste, defina a ordem do caso de teste com o `TestCaseOrdererAttribute` .

:::code language="csharp" source="~/dotnet-samples/csharp/unit-testing/XUnit.TestProject/ByAlphabeticalOrder.cs":::

## <a name="order-by-collection-alphabetically"></a>Ordenar por coleção em ordem alfabética

Para solicitar coleções de teste por seu nome de exibição, você implementa o `ITestCollectionOrderer` e fornece um mecanismo de ordenação.

:::code language="csharp" source="~/dotnet-samples/csharp/unit-testing/XUnit.TestProject/Orderers/DisplayNameOrderer.cs":::

Como as coleções de testes podem ser executadas em paralelo, você deve desabilitar explicitamente a paralelização de teste das coleções com o `CollectionBehaviorAttribute` . Em seguida, especifique a implementação para o `TestCollectionOrdererAttribute` .

:::code language="csharp" source="~/dotnet-samples/csharp/unit-testing/XUnit.TestProject/ByDisplayName.cs":::

## <a name="order-by-custom-attribute"></a>Ordenar por atributo personalizado

Para solicitar testes de xUnit com atributos personalizados, primeiro você precisa de um atributo para confiar. Defina uma das `TestPriorityAttribute` seguintes maneiras:

:::code language="csharp" source="~/dotnet-samples/csharp/unit-testing/XUnit.TestProject/Attributes/TestPriorityAttribute.cs":::

Em seguida, considere a `PriorityOrderer` implementação da interface a seguir `ITestCaseOrderer` .

:::code language="csharp" source="~/dotnet-samples/csharp/unit-testing/XUnit.TestProject/Orderers/PriorityOrderer.cs":::

Em seguida, em uma classe de teste, defina o pedido de caso de teste com o `TestCaseOrdererAttribute` para `PriorityOrderer` .

:::code language="csharp" source="~/dotnet-samples/csharp/unit-testing/XUnit.TestProject/ByPriorityOrder.cs":::

:::zone-end
:::zone pivot="nunit"

## <a name="order-by-priority"></a>Ordenar por prioridade

Para ordenar os testes explicitamente, o NUnit fornece um [`OrderAttribute`](https://github.com/nunit/docs/wiki/Order-Attribute) . Os testes com este atributo são iniciados antes dos testes sem. O valor do pedido é usado para determinar a ordem de execução dos testes de unidade.

:::code language="csharp" source="~/dotnet-samples/csharp/unit-testing/NUnit.TestProject/ByOrder.cs":::

:::zone-end

## <a name="next-steps"></a>Próximas etapas

> [!div class="nextstepaction"]
> [Cobertura de código de testes de unidades](unit-testing-code-coverage.md)
