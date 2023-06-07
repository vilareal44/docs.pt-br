---
title: 'Como: Usar uma classe genérica'
ms.date: 07/20/2015
helpviewer_keywords:
- type parameters [Visual Basic], defining
- data type arguments [Visual Basic], defining
- arguments [Visual Basic], data types
- Of keyword [Visual Basic], using
- generic parameters
- data type parameters
- generics [Visual Basic], about generics
- data types [Visual Basic], as parameters
- data types [Visual Basic], as arguments
- parameters [Visual Basic], type
- types [Visual Basic], generic
- parameters [Visual Basic], generic
- generics [Visual Basic], creating generic types
- data type arguments
- parameters [Visual Basic], data type
- data type parameters [Visual Basic], defining
- type arguments [Visual Basic], defining
- arguments [Visual Basic], type
ms.assetid: 242dd2a6-86c4-4ce7-83f2-f2661803f752
ms.openlocfilehash: 01f1f7ef5963feeb3fe2b5390244e4e516773bad
ms.sourcegitcommit: f8c270376ed905f6a8896ce0fe25b4f4b38ff498
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/04/2020
ms.locfileid: "84393838"
---
# <a name="how-to-use-a-generic-class-visual-basic"></a>Como usar uma classe genérica (Visual Basic)
Uma classe que usa *parâmetros de tipo* é chamada de *classe genérica*. Se você estiver usando uma classe genérica, poderá gerar uma *classe construída* a partir dela fornecendo um argumento de *tipo* para cada um desses parâmetros. Você pode então declarar uma variável do tipo de classe construída e pode criar uma instância da classe construída e atribuí-la a essa variável.  
  
 Além das classes, você também pode definir e usar estruturas genéricas, interfaces, procedimentos e delegados.  
  
 O procedimento a seguir usa uma classe genérica definida no .NET Framework e cria uma instância a partir dela.  
  
### <a name="to-use-a-class-that-takes-a-type-parameter"></a>Para usar uma classe que usa um parâmetro de tipo  
  
1. No início do arquivo de origem, inclua uma [instrução Imports (namespace e tipo do .net)](../../../language-reference/statements/imports-statement-net-namespace-and-type.md) para importar o <xref:System.Collections.Generic?displayProperty=nameWithType> namespace. Isso permite que você faça referência à <xref:System.Collections.Generic.Queue%601?displayProperty=nameWithType> classe sem precisar qualificá-la totalmente para diferenciá-la de outras classes de fila, como <xref:System.Collections.Queue?displayProperty=nameWithType> .  
  
2. Crie o objeto da maneira normal, mas adicione `(Of type)` imediatamente após o nome da classe.  
  
     O exemplo a seguir usa a mesma classe ( <xref:System.Collections.Generic.Queue%601?displayProperty=nameWithType> ) para criar dois objetos de fila que contêm itens de tipos de dados diferentes. Ele adiciona itens ao final de cada fila e, em seguida, remove e exibe itens da frente de cada fila.  
  
     [!code-vb[VbVbalrDataTypes#9](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrDataTypes/VB/Class1.vb#9)]  
  
## <a name="see-also"></a>Confira também

- [Tipos de dados](index.md)
- [Tipos genéricos no Visual Basic](generic-types.md)
- [Componentes de independência de linguagem e componentes independentes da linguagem](../../../../standard/language-independence-and-language-independent-components.md)
- [Desse](../../../language-reference/statements/of-clause.md)
- [Instrução Imports (tipo e namespace .NET)](../../../language-reference/statements/imports-statement-net-namespace-and-type.md)
- [Como: Definir uma classe capaz de fornecer uma funcionalidade idêntica em tipos de dados diferentes](how-to-define-a-class-that-can-provide-identical-functionality.md)
- [Iterators](../../concepts/iterators.md)
