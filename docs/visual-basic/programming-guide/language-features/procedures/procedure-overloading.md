---
title: Sobrecarga de procedimento
ms.date: 07/20/2015
helpviewer_keywords:
- signatures
- Overloads keyword [Visual Basic]
- hiding by signature
- Visual Basic code, procedures
- procedures [Visual Basic], signatures for
- procedures [Visual Basic], overloading
- procedures [Visual Basic], multiple versions
- parameters [Visual Basic], lists
- signatures [Visual Basic], procedure
- parameter lists [Visual Basic]
- Visual Basic code, parameter lists
- Shadows keyword [Visual Basic]
- procedure overloading
- procedures [Visual Basic], parameter lists
ms.assetid: fbc7fb18-e3b2-48b6-b554-64c00ed09d2a
ms.openlocfilehash: f8accc74fbdd9b1d8cf9bc3d8f6ddd26f73452b8
ms.sourcegitcommit: f8c270376ed905f6a8896ce0fe25b4f4b38ff498
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/04/2020
ms.locfileid: "84363870"
---
# <a name="procedure-overloading-visual-basic"></a>Sobrecarga de procedimento (Visual Basic)

*Sobrecarregar* um procedimento significa defini-lo em várias versões, usando o mesmo nome, mas listas de parâmetros diferentes. A finalidade do sobrecarregamento é definir várias versões de um procedimento bem relacionadas sem precisar diferenciá-las por nome. Você faz isso variando a lista de parâmetros.

## <a name="overloading-rules"></a>Regras de sobrecarga

Quando você sobrecarrega um procedimento, as seguintes regras se aplicam:

- **Mesmo nome**. Cada versão sobrecarregada deve usar o mesmo nome de procedimento.

- **Assinatura diferente**. Cada versão sobrecarregada deve ser diferente de todas as outras versões sobrecarregadas em pelo menos um dos seguintes aspectos:

  - Número de parâmetros

  - Ordem dos parâmetros

  - Tipos de dados dos parâmetros

  - Número de parâmetros de tipo (para um procedimento genérico)

  - Tipo de retorno (somente para um operador de conversão)

  Junto com o nome do procedimento, os itens anteriores são chamados coletivamente da *assinatura* do procedimento. Quando você chama um procedimento sobrecarregado, o compilador usa a assinatura para verificar se a chamada corresponde corretamente à definição.

- **Itens que não fazem parte da assinatura**. Não é possível sobrecarregar um procedimento sem variar a assinatura. Em particular, você não pode sobrecarregar um procedimento, variando apenas um ou mais dos seguintes itens:

  - Palavras-chave do modificador de procedimento, como `Public` , `Shared` e`Static`

  - Nomes de parâmetro de tipo ou parâmetro

  - Restrições de parâmetro de tipo (para um procedimento genérico)

  - Palavras-chave do modificador de parâmetro, como `ByRef` e`Optional`

  - Se ele retorna um valor

  - O tipo de dados do valor de retorno (exceto para um operador de conversão)

  Os itens na lista anterior não fazem parte da assinatura. Embora não seja possível usá-los para diferenciar as versões sobrecarregadas, você pode variar entre as versões sobrecarregadas que são diferenciadas corretamente por suas assinaturas.

- **Argumentos de associação tardia**. Se você pretende passar uma variável de objeto de associação tardia para uma versão sobrecarregada, você deve declarar o parâmetro apropriado como <xref:System.Object> .

## <a name="multiple-versions-of-a-procedure"></a>Várias versões de um procedimento

Suponha que você esteja escrevendo um `Sub` procedimento para lançar uma transação em relação ao saldo de um cliente e desejar consultar o cliente por nome ou por número de conta. Para acomodar isso, você pode definir dois `Sub` procedimentos diferentes, como no exemplo a seguir:

[!code-vb[VbVbcnProcedures#73](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbcnProcedures/VB/Class1.vb#73)]

### <a name="overloaded-versions"></a>Versões sobrecarregadas

Uma alternativa é sobrecarregar um único nome de procedimento. Você pode usar a palavra-chave [Overloads](../../../language-reference/modifiers/overloads.md) para definir uma versão do procedimento para cada lista de parâmetros, da seguinte maneira:

[!code-vb[VbVbcnProcedures#72](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbcnProcedures/VB/Class1.vb#72)]

#### <a name="additional-overloads"></a>Sobrecargas adicionais

Se você também quisesse aceitar um valor de transação em `Decimal` ou `Single` , poderá sobrecarregar `post` para permitir essa variação. Se você fez isso para cada uma das sobrecargas no exemplo anterior, teria quatro `Sub` procedimentos, todos com o mesmo nome, mas com quatro assinaturas diferentes.

## <a name="advantages-of-overloading"></a>Vantagens da sobrecarga

A vantagem de sobrecarregar um procedimento é a flexibilidade da chamada. Para usar o `post` procedimento declarado no exemplo anterior, o código de chamada pode obter a identificação do cliente como um `String` ou um `Integer` e, em seguida, chamar o mesmo procedimento em ambos os casos. O exemplo a seguir ilustra isso:

[!code-vb[VbVbcnProcedures#56](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbcnProcedures/VB/Class1.vb#56)]

[!code-vb[VbVbcnProcedures#57](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbcnProcedures/VB/Class1.vb#57)]

## <a name="see-also"></a>Confira também

- [Procedimentos](./index.md)
- [Como definir várias versões de um procedimento](./how-to-define-multiple-versions-of-a-procedure.md)
- [Como chamar um procedimento sobrecarregado](./how-to-call-an-overloaded-procedure.md)
- [Como sobrecarregar um procedimento que usa parâmetros opcionais](./how-to-overload-a-procedure-that-takes-optional-parameters.md)
- [Como sobrecarregar um procedimento que usa um número indefinido de parâmetros](./how-to-overload-a-procedure-that-takes-an-indefinite-number-of-parameters.md)
- [Considerações sobre procedimentos de sobrecarga](./considerations-in-overloading-procedures.md)
- [Resolução de sobrecarga](./overload-resolution.md)
- [Sobrecargas](../../../language-reference/modifiers/overloads.md)
- [Tipos genéricos no Visual Basic](../data-types/generic-types.md)
