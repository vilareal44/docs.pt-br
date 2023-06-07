---
title: Passando argumentos por posição e nome
ms.date: 02/01/2018
helpviewer_keywords:
- arguments [Visual Basic], passing by name
- procedures [Visual Basic], arguments
- := passing arguments
- procedure arguments
- Visual Basic code, procedures
- named arguments [Visual Basic], passing arguments
- arguments [Visual Basic], passing by position
- Function procedures [Visual Basic], passing arguments
- named parameters [Visual Basic], passing arguments
- named arguments
- passing parameters [Visual Basic], named parameter arguments
- passing parameters [Visual Basic], by position
- procedures [Visual Basic], calling
- named parameters
- Sub procedures [Visual Basic], passing arguments
- argument passing
- passing parameters [Visual Basic], by name
- argument passing [Visual Basic], by position
- arguments [Visual Basic], listing by name
ms.assetid: 1ad7358f-1da9-48da-a95b-f3c7ed41eff3
ms.openlocfilehash: 686b64977f086c8128e56298a0ed8c5aa0c51efa
ms.sourcegitcommit: f8c270376ed905f6a8896ce0fe25b4f4b38ff498
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/04/2020
ms.locfileid: "84364026"
---
# <a name="passing-arguments-by-position-and-by-name-visual-basic"></a>Passando argumentos por posição e nome (Visual Basic)

Ao chamar um `Sub` procedimento ou `Function` , você pode passar argumentos *por posição* — na ordem em que eles aparecem na definição do procedimento — ou você pode passá-los *por nome*, sem considerar a posição.

Quando você passa um argumento por nome, especifica o nome declarado do argumento seguido por dois-pontos e um sinal de igual ( `:=` ), seguido pelo valor do argumento. Você pode fornecer argumentos nomeados em qualquer ordem.

Por exemplo, o procedimento a seguir `Sub` usa três argumentos:

[!code-vb[SampleProcedure](../../../../../samples/snippets/visualbasic/programming-guide/language-features/passing-named-arguments/module1.vb#1)]

Ao chamar esse procedimento, você pode fornecer os argumentos por posição, por nome ou usando uma combinação de ambos.

## <a name="passing-arguments-by-position"></a>Passando argumentos por posição

Você pode chamar o `Display` método com seus argumentos passados pela posição e delimitados por vírgulas, conforme mostrado no exemplo a seguir:

[!code-vb[ByPosition](../../../../../samples/snippets/visualbasic/programming-guide/language-features/passing-named-arguments/module1.vb#2)]

Se você omitir um argumento opcional em uma lista de argumentos posicionais, deverá manter seu lugar com uma vírgula. O exemplo a seguir chama o `Display` método sem o `age` argumento:

[!code-vb[ByPositionWithOptionalArgument](../../../../../samples/snippets/visualbasic/programming-guide/language-features/passing-named-arguments/module1.vb#3)]

## <a name="passing-arguments-by-name"></a>Passando argumentos por nome

Como alternativa, você pode chamar `Display` com os argumentos passados pelo nome, também delimitados por vírgulas, conforme mostrado no exemplo a seguir:

[!code-vb[ByName](../../../../../samples/snippets/visualbasic/programming-guide/language-features/passing-named-arguments/module1.vb#4)]

Passar argumentos por nome dessa maneira é especialmente útil quando você chama um procedimento que tem mais de um argumento opcional. Se você fornecer argumentos por nome, não precisará usar vírgulas consecutivas para denotar argumentos posicionais ausentes. A passagem de argumentos por nome também facilita o controle de quais argumentos você está passando e quais estão sendo omitidos.

## <a name="mixing-arguments-by-position-and-by-name"></a>Misturando argumentos por posição e por nome

Você pode fornecer argumentos tanto por posição quanto por nome em uma única chamada de procedimento, conforme mostrado no exemplo a seguir:

[!code-vb[ByNameAndPosition](../../../../../samples/snippets/visualbasic/programming-guide/language-features/passing-named-arguments/module1.vb#5)]

No exemplo anterior, nenhuma vírgula extra é necessária para manter o lugar do argumento omitido `age` , já que `birth` é passado pelo nome.

Em versões do Visual Basic antes de 15,5, quando você fornece argumentos por uma mistura de posição e nome, os argumentos posicionais devem ser todos apresentados primeiro. Depois de fornecer um argumento por nome, todos os argumentos restantes devem ser passados pelo nome.  Por exemplo, a chamada a seguir para o `Display` método exibe o erro do compilador [BC30241: argumento nomeado esperado](../../../misc/bc30241.md).

[!code-vb[ByNameAndPosition](../../../../../samples/snippets/visualbasic/programming-guide/language-features/passing-named-arguments/module1.vb#6)]

A partir do Visual Basic 15,5, argumentos posicionais podem seguir argumentos nomeados se os argumentos posicionais finais estiverem na posição correta. Se for compilado em Visual Basic 15,5, a chamada anterior para o `Display` método será compilada com êxito e não gerará mais o erro [BC30241](../../../misc/bc30241.md)do compilador.

Essa capacidade de misturar e corresponder argumentos nomeados e posicionais em qualquer ordem é particularmente útil quando você deseja usar um argumento nomeado para tornar seu código mais legível. Por exemplo, o construtor de classe a seguir `Person` requer dois argumentos do tipo `Person` , ambos podem ser `Nothing` .

[!code-vb[ByNameAndPosition](../../../../../samples/snippets/visualbasic/programming-guide/language-features/passing-named-arguments/module1.vb#7)]

Usar argumentos nomeados e posicionais mistos ajuda a tornar a intenção do código claro quando o valor dos `father` `mother` argumentos e é `Nothing` :

[!code-vb[ByNameAndPosition](../../../../../samples/snippets/visualbasic/programming-guide/language-features/passing-named-arguments/module1.vb#8)]

Para seguir argumentos posicionais com argumentos nomeados, você deve adicionar o seguinte elemento ao seu arquivo de Visual Basic projeto ( \* . vbproj):

```xml
<PropertyGroup>
  <LangVersion>15.5</LangVersion>
</PropertyGroup>
```

Para obter mais informações, consulte [definindo a versão do idioma do Visual Basic](../../../language-reference/configure-language-version.md).

## <a name="restrictions-on-supplying-arguments-by-name"></a>Restrições de fornecimento de argumentos por nome

Não é possível passar argumentos por nome para evitar a inserção de argumentos necessários. Você pode omitir apenas os argumentos opcionais.

Não é possível passar uma matriz de parâmetros por nome. Isso ocorre porque, ao chamar o procedimento, você fornece um número indefinido de argumentos separados por vírgulas para a matriz de parâmetros e o compilador não pode associar mais de um argumento a um único nome.

## <a name="see-also"></a>Confira também

- [Procedimentos](./index.md)
- [Parâmetros e Argumentos de Procedimento](./procedure-parameters-and-arguments.md)
- [Como passar argumentos para um procedimento](./how-to-pass-arguments-to-a-procedure.md)
- [Passar argumentos por valor e por referência](./passing-arguments-by-value-and-by-reference.md)
- [Parâmetros Opcionais](./optional-parameters.md)
- [Matrizes de parâmetros](./parameter-arrays.md)
- [Opcional](../../../language-reference/modifiers/optional.md)
- [ParamArray](../../../language-reference/modifiers/paramarray.md)
