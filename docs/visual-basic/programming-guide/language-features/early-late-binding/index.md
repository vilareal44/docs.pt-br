---
title: Associação antecipada e tardia
ms.date: 07/20/2015
helpviewer_keywords:
- early binding [Visual Basic]
- objects [Visual Basic], late-bound
- objects [Visual Basic], early-bound
- objects [Visual Basic], late bound
- early binding [Visual Basic], Visual Basic compiler
- binding [Visual Basic], late and early
- objects [Visual Basic], early bound
- Visual Basic compiler, early and late binding
- late binding [Visual Basic]
- late binding [Visual Basic], Visual Basic compiler
ms.assetid: d6ff7f1e-b94f-4205-ab8d-5cfa91758724
ms.openlocfilehash: e8d87e095b7c3104e3a2d66525644d1771ae883e
ms.sourcegitcommit: f8c270376ed905f6a8896ce0fe25b4f4b38ff498
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/04/2020
ms.locfileid: "84410626"
---
# <a name="early-and-late-binding-visual-basic"></a>Associação antecipada e tardia (Visual Basic)
O compilador Visual Basic executa um processo chamado `binding` quando um objeto é atribuído a uma variável de objeto. Um objeto é *associado inicialmente* quando ele é atribuído a uma variável declarada como de um tipo de objeto específico. Os objetos de associação inicial permitem que o compilador aloque memória e execute outras otimizações antes que um aplicativo seja executado. Por exemplo, o seguinte fragmento de código declara que uma variável é do tipo <xref:System.IO.FileStream>:  
  
 [!code-vb[VbVbalrOOP#90](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrOOP/VB/OOP.vb#90)]  
  
 Uma vez que <xref:System.IO.FileStream> é um tipo de objeto específico, a instância atribuída a `FS` é associada no início.  
  
 Por outro lado, um objeto *associado tardiamente* quando ele é atribuído a uma variável declarada para ser do tipo `Object`. Objetos desse tipo podem conter referências a qualquer objeto, mas têm muitas das vantagens de objetos de associação inicial. Por exemplo, o fragmento de código a seguir declara uma variável de objeto para conter um objeto retornado pela função `CreateObject`:  
  
 [!code-vb[VbVbalrOOP#91](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrOOP/VB/LateBinding.vb#91)]  
  
## <a name="advantages-of-early-binding"></a>Vantagens da associação inicial  
 Você deve usar objetos associação inicial sempre que possível, pois eles permitem que o compilador faça otimizações importantes que resultam em aplicativos mais eficientes. Os objetos de associação inicial são significativamente mais rápidos do que objetos de associação tardia e tornam seu código mais fácil de ler e manter informando exatamente quais tipos de objetos estão sendo usados. Outra vantagem da ligação inicial é que ela permite recursos úteis, como a conclusão automática de código e a ajuda dinâmica porque o IDE (ambiente de desenvolvimento integrado) do Visual Studio pode determinar exatamente o tipo de objeto com o qual você está trabalhando enquanto edita o código. A associação inicial reduz o número e a gravidade dos erros em tempo de execução porque ela permite que o compilador relate erros quando um programa é compilado.  
  
> [!NOTE]
> A associação tardia só pode ser usada para acessar membros de tipo que são declarados como `Public`. Acessando membros declarados como `Friend` ou `Protected Friend` resulta em um erro em tempo de execução.  
  
## <a name="see-also"></a>Confira também

- <xref:Microsoft.VisualBasic.Interaction.CreateObject%2A>
- [Tempo de vida do objeto: como os objetos são criados e destruídos](../objects-and-classes/object-lifetime-how-objects-are-created-and-destroyed.md)
- [Tipo de dados Object](../../../language-reference/data-types/object-data-type.md)
