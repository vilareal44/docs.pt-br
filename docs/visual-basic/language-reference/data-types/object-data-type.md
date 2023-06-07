---
title: Tipo de dados Object
ms.date: 07/20/2015
f1_keywords:
- vb.Object
- vb.Variant
helpviewer_keywords:
- object variables [Visual Basic], Object type
- data types [Visual Basic], assigning
- Object data type
- Object data type [Visual Basic], reference
ms.assetid: 61ea4a7c-3b3d-48d4-adc4-eacfa91779b2
ms.openlocfilehash: 14770e74a0169dba3a45a04845dd32323e6201e3
ms.sourcegitcommit: f8c270376ed905f6a8896ce0fe25b4f4b38ff498
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/04/2020
ms.locfileid: "84415577"
---
# <a name="object-data-type"></a>Tipo de dados Object

Contém endereços que se referem a objetos. Você pode atribuir qualquer tipo de referência (cadeia de caracteres, matriz, classe ou interface) a uma `Object` variável. Uma `Object` variável também pode se referir a dados de qualquer tipo de valor (numérico, `Boolean` , `Char` , `Date` , estrutura ou Enumeração).

## <a name="remarks"></a>Comentários

O `Object` tipo de dados pode apontar para dados de qualquer tipo de dados, incluindo qualquer instância de objeto que seu aplicativo reconhece. Use `Object` quando você não souber em tempo de compilação a qual tipo de dados a variável pode apontar.

O valor padrão de `Object` é `Nothing` (uma referência nula).

## <a name="data-types"></a>Tipos de dados

Você pode atribuir uma variável, constante ou expressão de qualquer tipo de dados a uma `Object` variável. Para determinar o tipo de dados `Object` ao qual uma variável se refere atualmente, você pode usar o <xref:System.Type.GetTypeCode%2A> método da <xref:System.Type?displayProperty=nameWithType> classe. O exemplo a seguir ilustra isto.

```vb
Dim myObject As Object
' Suppose myObject has now had something assigned to it.
Dim datTyp As Integer
datTyp = Type.GetTypeCode(myObject.GetType())
```

O `Object` tipo de dados é um tipo de referência. No entanto, Visual Basic trata uma `Object` variável como um tipo de valor quando se refere aos dados de um tipo de valor.

## <a name="storage"></a>Armazenamento

Qualquer tipo de dados ao qual se refere, uma `Object` variável não contém o próprio valor de dados, mas sim um ponteiro para o valor. Ele sempre usa quatro bytes na memória do computador, mas isso não inclui o armazenamento para os dados que representam o valor da variável. Devido ao código que usa o ponteiro para localizar os dados, as `Object` variáveis que mantêm os tipos de valor são um pouco mais lentas para acessar do que as variáveis explicitamente digitadas.

## <a name="programming-tips"></a>Dicas de programação

- **Considerações sobre interoperabilidade.** Se você estiver fazendo a interface com componentes não escritos para o .NET Framework, por exemplo, automação ou objetos COM, tenha em mente que os tipos de ponteiro em outros ambientes não são compatíveis com o tipo de Visual Basic `Object` .

- **Desempenho.** Uma variável que você declara com o `Object` tipo é flexível o suficiente para conter uma referência a qualquer objeto. No entanto, quando você invoca um método ou propriedade em tal variável, sempre ocorre a *ligação tardia* (em tempo de execução). Para forçar a *ligação antecipada* (no momento da compilação) e melhorar o desempenho, declare a variável com um nome de classe específico ou converta-a para o tipo de dados específico.

  Ao declarar uma variável de objeto, tente usar um tipo de classe específico, por exemplo <xref:System.OperatingSystem> , em vez do tipo generalizado `Object` . Você também deve usar a classe mais específica disponível, como <xref:System.Windows.Forms.TextBox> em vez de <xref:System.Windows.Forms.Control> , para que você possa acessar suas propriedades e métodos. Normalmente, você pode usar a lista de **classes** no **pesquisador de objetos** para localizar os nomes de classe disponíveis.

- **Ampliação.** Todos os tipos de dados e todos os tipos de referência ampliam para o `Object` tipo de dados. Isso significa que você pode converter qualquer tipo para `Object` sem encontrar um <xref:System.OverflowException?displayProperty=nameWithType> erro.

  No entanto, se você converter entre tipos de valor e `Object` , Visual Basic executar operações chamadas *Boxing* e *unboxing*, o que torna a execução mais lenta.

- **Digite os caracteres.** `Object`Não tem caractere de tipo literal ou caractere de tipo de identificador.

- **Tipo de estrutura.** O tipo correspondente no .NET Framework é a <xref:System.Object?displayProperty=nameWithType> classe.

## <a name="example"></a>Exemplo

O exemplo a seguir ilustra uma `Object` variável que aponta para uma instância de objeto.

```vb
Dim objDb As Object
Dim myCollection As New Collection()
' Suppose myCollection has now been populated.
objDb = myCollection.Item(1)
```

## <a name="see-also"></a>Confira também

- <xref:System.Object>
- [Tipos de dados](index.md)
- [Funções de conversão do tipo](../functions/type-conversion-functions.md)
- [Resumo da Conversão](../keywords/conversion-summary.md)
- [Uso eficiente de tipos de dados](../../programming-guide/language-features/data-types/efficient-use-of-data-types.md)
- [Como determinar se dois objetos estão relacionados](../../programming-guide/language-features/variables/how-to-determine-whether-two-objects-are-related.md)
- [Como determinar se dois objetos são idênticos](../../programming-guide/language-features/variables/how-to-determine-whether-two-objects-are-identical.md)
