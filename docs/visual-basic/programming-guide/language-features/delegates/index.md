---
title: Delegados
ms.date: 07/20/2015
helpviewer_keywords:
- delegates [Visual Basic]
- Visual Basic code, delegates
ms.assetid: 410b60dc-5e60-4ec0-bfae-426755a2ee28
ms.openlocfilehash: 1f161248fa04f8fab0e5335413e69ca565732f71
ms.sourcegitcommit: f8c270376ed905f6a8896ce0fe25b4f4b38ff498
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/04/2020
ms.locfileid: "84410677"
---
# <a name="delegates-visual-basic"></a>Delegados (Visual Basic)

Os delegados são objetos que se referem aos métodos. Às vezes, eles são descritos como *ponteiros de função fortemente tipados* porque eles são semelhante aos ponteiros de função usados em outras linguagens de programação. Mas, diferentemente dos ponteiros de função, os delegados de Visual Basic são um tipo de referência com base na classe <xref:System.Delegate?displayProperty=nameWithType> . Os delegados podem fazer referência a ambos os métodos compartilhados: os métodos que podem ser chamados sem uma instância específica de uma classe e os métodos de instância.

## <a name="delegates-and-events"></a>Representantes e eventos

Os delegados são úteis em situações em que é necessário um intermediário entre um procedimento de chamada e o procedimento sendo chamado. Por exemplo, você pode desejar que um objeto que aciona eventos possa chamar manipuladores de eventos diferentes em diferentes circunstâncias. Infelizmente, o objeto que aciona os eventos não pode saber de antemão quais manipulador de eventos estarão tratando um evento específico. Visual Basic permite associar dinamicamente manipuladores de eventos a eventos criando um delegado para você quando você usa a `AddHandler` instrução. No tempo de execução, o delegado encaminha chamadas para o manipulador de eventos apropriado.

Embora você possa criar seus próprios delegados, na maioria dos casos Visual Basic cria o delegado e cuida dos detalhes para você. Por exemplo, uma instrução `Event` define implicitamente uma classe delegada chamada `<EventName>EventHandler` como uma classe aninhada da classe que contém a instrução `Event` e com a mesma assinatura que o evento. A instrução `AddressOf` cria implicitamente uma instância de um delegado que se refere a um procedimento específico. As duas linhas de código a seguir são equivalentes. Na primeira linha, você vê a criação explícita de uma instância do `EventHandler`, com uma referência ao método `Button1_Click` enviado como o argumento. A segunda linha é uma maneira mais conveniente de fazer a mesma coisa.

[!code-vb[VbVbalrDelegates#6](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrDelegates/VB/Class1.vb#6)]

Você pode usar a forma mais simples de criar delegados em qualquer local que o compilador possa determinar o tipo do delegado pelo contexto.

## <a name="declaring-events-that-use-an-existing-delegate-type"></a>Declarando eventos que usam um tipo delegado existente

Em algumas situações, convém declarar um evento para usar um tipo delegado existente como seu delegado subjacente. A sintaxe a seguir demonstra como:

[!code-vb[VbVbalrDelegates#7](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrDelegates/VB/Class1.vb#7)]

Isso é útil quando você deseja rotear vários eventos para o mesmo manipulador.

## <a name="delegate-variables-and-parameters"></a>Variáveis e parâmetros de delegado

Você pode usar delegados para outras tarefas não relacionadas a eventos, como threading livre ou com os procedimentos que precisem chamar diferentes versões de funções no tempo de execução.

Por exemplo, suponha que você tenha um aplicativo com anúncio classificado que inclui uma caixa de listagem com os nomes de carros. Os anúncios são classificados por título, que é, normalmente, a marca do carro. Um problema que você pode enfrentar ocorre quando alguns carros incluem o ano antes do fabricante. O problema é que a funcionalidade interna de classificação da caixa de listagem classifica somente por códigos de caracteres. Ela coloca todos os anúncios começando com datas primeiro, seguidos de anúncios começando com o fabricante.

Para corrigir isso, você pode criar um procedimento de classificação em uma classe que usa a classificação alfabética padrão na maioria das caixas de listagem, mas é possível mudar no tempo de execução para o procedimento de classificação personalizada para anúncios de carro. Para fazer isso, você passa o procedimento de classificação personalizada para a classe de classificação no tempo de execução usando delegados.

## <a name="addressof-and-lambda-expressions"></a>Expressões lambda e AddressOf

Cada classe de delegado define um construtor que é passado para a especificação de um método do objeto. Um argumento para o construtor delegado deve ser uma referência a um método ou uma expressão lambda.

Para especificar uma referência a um método, use a seguinte sintaxe:

`AddressOf` [`expression`.]`methodName`

O tipo de tempo de compilação do `expression` deve ser o nome de uma classe ou uma interface que contém um método do nome especificado cuja assinatura coincide com a assinatura da classe delegada. O `methodName` pode ser um método compartilhado ou um método de instância. O `methodName` não é opcional, mesmo se você criar um delegado para o método padrão da classe.

Para especificar uma expressão lambda, use a seguinte sintaxe:

`Function` ([`parm` As `type`, `parm2` As `type2`, ...]) `expression`

O exemplo a seguir mostra as expressões lambda e `AddressOf` usadas para especificar a referência para um delegado.

[!code-vb[VbVbalrDelegates#15](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrDelegates/VB/Class2.vb#15)]

A assinatura da função deve corresponder a do tipo delegado. Para obter mais informações sobre expressões lambda, consulte [expressões lambda](../procedures/lambda-expressions.md). Para obter mais exemplos de expressão lambda e atribuições `AddressOf` aos delegados, consulte [Conversão de delegado amena](relaxed-delegate-conversion.md).

## <a name="related-topics"></a>Tópicos relacionados

|Title|Descrição|
|-----------|-----------------|
|[Como invocar um método delegado](how-to-invoke-a-delegate-method.md)|Fornece um exemplo que mostra como associar um método a um delegado e depois invoca esse método por meio do delegado.|
|[Como passar procedimentos para outro procedimento no Visual Basic](how-to-pass-procedures-to-another-procedure.md)|Demonstra como usar delegados para passar um procedimento para outro.|
|[Conversão de delegado reduzida](relaxed-delegate-conversion.md)|Descreve como você pode atribuir assinaturas e funções a delegados ou manipuladores mesmo quando as assinaturas não são idênticas|
|[Eventos](../events/index.md)|Fornece uma visão geral dos eventos no Visual Basic.|
