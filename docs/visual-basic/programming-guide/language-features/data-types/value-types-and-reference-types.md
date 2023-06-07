---
title: Tipos de valor e referência
ms.date: 07/20/2015
helpviewer_keywords:
- reference data types [Visual Basic]
- reference types [Visual Basic]
- value types [Visual Basic]
- value data types [Visual Basic]
- types [Visual Basic]
- data types [Visual Basic], value types
- data types [Visual Basic], reference types
ms.assetid: fc82ce15-5a40-4c5c-a1e1-a556830e7391
ms.openlocfilehash: 3a1de120f5f97ba93939f332f121d70cd3091216
ms.sourcegitcommit: f8c270376ed905f6a8896ce0fe25b4f4b38ff498
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/04/2020
ms.locfileid: "84392968"
---
# <a name="value-types-and-reference-types"></a>Tipos de valor e referência
Há dois tipos de tipo em Visual Basic: tipos de referência e tipos de valor. Variáveis de tipos de referência armazenam referências em seus dados (objetos) enquanto que variáveis de tipos de valor contém diretamente seus dados. Com tipos de referência, duas variáveis podem fazer referência ao mesmo objeto; portanto, operações em uma variável podem afetar o objeto referenciado pela outra variável. Com tipos de valor, cada variável tem sua própria cópia dos dados e não é possível que as operações em uma variável afetem a outra (exceto no caso do [modificador ByRef nos parâmetros](../../../language-reference/modifiers/byref.md)).
  
## <a name="value-types"></a>Tipos de valor  
 Um tipo de dados é um *tipo de valor* se ele mantém os dados dentro de sua própria alocação de memória. Os tipos de valor incluem o seguinte:  
  
- Todos os tipos de dados numéricos  
  
- `Boolean`, `Char`, e `Date`  
  
- Todas as estruturas, mesmo que seus membros sejam tipos de referência  
  
- Enumerações, já que seu tipo subjacente é sempre,,,,,, `SByte` `Short` `Integer` `Long` `Byte` `UShort` `UInteger` ou`ULong`  
  
 Cada estrutura é um tipo de valor, mesmo que contenha membros de tipo de referência. Por esse motivo, tipos de valor como `Char` e `Integer` são implementados por estruturas .NET Framework.  
  
 Você pode declarar um tipo de valor usando a palavra-chave reservada, por exemplo, `Decimal` . Você também pode usar a `New` palavra-chave para inicializar um tipo de valor. Isso é especialmente útil se o tipo tiver um construtor que aceite parâmetros. Um exemplo disso é o <xref:System.Decimal.%23ctor%28System.Int32%2CSystem.Int32%2CSystem.Int32%2CSystem.Boolean%2CSystem.Byte%29> Construtor, que cria um novo `Decimal` valor a partir das partes fornecidas.  
  
## <a name="reference-types"></a>Tipos de referência  
 Um *tipo de referência* armazena uma referência a seus dados. Os tipos de referência incluem o seguinte:  
  
- `String`  
  
- Todas as matrizes, mesmo que seus elementos sejam de tipos de valor  
  
- Tipos de classe, como<xref:System.Windows.Forms.Form>  
  
- Delegados  
  
 Uma classe é um *tipo de referência*. Observe que cada matriz é um tipo de referência, mesmo que seus membros sejam de tipos de valor.  
  
 Como cada tipo de referência representa uma classe de .NET Framework subjacente, você deve usar a palavra-chave [New Operator](../../../language-reference/operators/new-operator.md) ao inicializá-la. A instrução a seguir inicializa uma matriz.  
  
```vb  
Dim totals() As Single = New Single(8) {}  
```  
  
## <a name="elements-that-are-not-types"></a>Elementos que não são tipos  
 Os seguintes elementos de programação não se qualificam como tipos, porque você não pode especificar nenhum deles como um tipo de dados para um elemento declarado:  
  
- Namespaces  
  
- Módulos  
  
- Eventos  
  
- Propriedades e procedimentos  
  
- Variáveis, constantes e campos  
  
## <a name="working-with-the-object-data-type"></a>Trabalhando com o tipo de dados Object  
 Você pode atribuir um tipo de referência ou um tipo de valor a uma variável do `Object` tipo de dados. Uma `Object` variável sempre mantém uma referência aos dados, nunca os dados em si. No entanto, se você atribuir um tipo de valor a uma `Object` variável, ele se comparecerá como se ele mantiver seus próprios dados. Para obter mais informações, consulte [Object Data Type](../../../language-reference/data-types/object-data-type.md).  
  
 Você pode descobrir se uma `Object` variável está agindo como um tipo de referência ou um tipo de valor passando-a para o <xref:Microsoft.VisualBasic.Information.IsReference%2A> método na <xref:Microsoft.VisualBasic.Information> classe do <xref:Microsoft.VisualBasic?displayProperty=nameWithType> namespace. <xref:Microsoft.VisualBasic.Information.IsReference%2A?displayProperty=nameWithType>retorna `True` se o conteúdo da `Object` variável representa um tipo de referência.  
  
## <a name="see-also"></a>Confira também

- [Tipos de valor anulável](nullable-value-types.md)
- [Conversões de tipo no Visual Basic](type-conversions.md)
- [Instrução Structure](../../../language-reference/statements/structure-statement.md)
- [Uso eficiente de tipos de dados](efficient-use-of-data-types.md)
- [Tipo de dados Object](../../../language-reference/data-types/object-data-type.md)
- [Tipos de dados](index.md)
