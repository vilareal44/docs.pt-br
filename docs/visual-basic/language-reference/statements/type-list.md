---
title: Lista de Tipos
ms.date: 07/20/2015
f1_keywords:
- StructureConstraint
- vb.StructureConstraint
- ClassConstraint
- vb.ClassConstraint
helpviewer_keywords:
- class constraint
- constraints, Visual Basic generic types
- generic parameters
- generics [Visual Basic], constraints
- generics [Visual Basic], type list
- structure constraint
- constraints, in type parameters
- generics [Visual Basic], generic types
- parameters [Visual Basic], type
- constraints, Structure keyword
- type parameters [Visual Basic], constraints
- types [Visual Basic], generic
- parameters [Visual Basic], generic
- generics [Visual Basic], type parameters
- type parameters
- constraints, Class keyword
ms.assetid: 56db947a-2ae8-40f2-a70a-960764e9d0db
ms.openlocfilehash: 7e22ad6e32ec13f081391e1d47a80df8b1e65063
ms.sourcegitcommit: f8c270376ed905f6a8896ce0fe25b4f4b38ff498
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/04/2020
ms.locfileid: "84412982"
---
# <a name="type-list-visual-basic"></a>Lista de tipos (Visual Basic)

Especifica os *parâmetros de tipo* para um elemento de programação *genérico* . Vários parâmetros são separados por vírgulas. A seguir está a sintaxe para um parâmetro de tipo.

## <a name="syntax"></a>Sintaxe

```vb
[genericmodifier] typename [ As constraintlist ]
```

## <a name="parts"></a>Partes

|Termo|Definição|
|---|---|
|`genericmodifier`|Opcional. Pode ser usado somente em interfaces e delegados genéricos. Você pode declarar um tipo covariant usando a palavra-chave [out](../modifiers/out-generic-modifier.md) ou contravariant usando a palavra-chave [in](../modifiers/in-generic-modifier.md) . Consulte [Covariância e contravariância](../../programming-guide/concepts/covariance-contravariance/index.md).|
|`typename`|Obrigatórios. Nome do parâmetro de tipo. Este é um espaço reservado, a ser substituído por um tipo definido fornecido pelo argumento de tipo correspondente.|
|`constraintlist`|Opcional. Lista de requisitos que restringem o tipo de dados que pode ser fornecido para o `typename` . Se você tiver várias restrições, coloque-as entre chaves ( `{ }` ) e separe-as com vírgulas. Você deve introduzir a lista de restrições com [a palavra-](as-clause.md) chave as. Você usa `As` apenas uma vez, no início da lista.|

## <a name="remarks"></a>Comentários

Cada elemento de programação genérico deve ter pelo menos um parâmetro de tipo. Um parâmetro de tipo é um espaço reservado para um tipo específico (um *elemento construído*) que o código do cliente especifica quando ele cria uma instância do tipo genérico. Você pode definir uma classe genérica, estrutura, interface, procedimento ou delegado.

Para obter mais informações sobre quando definir um tipo genérico, consulte [tipos genéricos em Visual Basic](../../programming-guide/language-features/data-types/generic-types.md). Para obter mais informações sobre nomes de parâmetro de tipo, consulte [nomes de elemento declarados](../../programming-guide/language-features/declared-elements/declared-element-names.md).

## <a name="rules"></a>Regras

- **Parênteses.** Se você fornecer uma lista de parâmetros de tipo, deverá colocá-la entre parênteses e deverá introduzir a lista com a palavra-chave [de](of-clause.md) . Você usa `Of` apenas uma vez, no início da lista.

- **Reflexiva.** Uma lista de *restrições* em um parâmetro de tipo pode incluir os seguintes itens em qualquer combinação:

  - Qualquer número de interfaces. O tipo fornecido deve implementar todas as interfaces nesta lista.

  - No máximo uma classe. O tipo fornecido deve herdar dessa classe.

  - A palavra-chave `New`. O tipo fornecido deve expor um construtor sem parâmetros que o seu tipo genérico possa acessar. Isso será útil se você restringir um parâmetro de tipo por uma ou mais interfaces. Um tipo que implementa interfaces não expõe necessariamente um construtor e, dependendo do nível de acesso de um construtor, o código dentro do tipo genérico pode não ser capaz de acessá-lo.

  - A `Class` palavra-chave ou a `Structure` palavra-chave. A `Class` palavra-chave restringe um parâmetro de tipo genérico para exigir que qualquer argumento de tipo passado para ele seja um tipo de referência, por exemplo, uma cadeia de caracteres, matriz ou delegado, ou um objeto criado a partir de uma classe. A `Structure` palavra-chave restringe um parâmetro de tipo genérico para exigir que qualquer argumento de tipo passado para ele seja um tipo de valor, por exemplo, um tipo de dados de estrutura, enumeração ou elementar. Você não pode incluir `Class` e `Structure` no mesmo `constraintlist` .

  O tipo fornecido deve atender a todos os requisitos incluídos no `constraintlist` .

  Restrições em cada parâmetro de tipo são independentes de restrições em outros parâmetros de tipo.

## <a name="behavior"></a>Comportamento

- **Substituição de tempo de compilação.** Quando você cria um tipo construído a partir de um elemento de programação genérico, você fornece um tipo definido para cada parâmetro de tipo. O compilador Visual Basic substitui esse tipo fornecido para cada ocorrência de `typename` dentro do elemento genérico.

- **Ausência de restrições.** Se você não especificar nenhuma restrição em um parâmetro de tipo, seu código será limitado às operações e aos membros com suporte do [tipo de dados Object](../data-types/object-data-type.md) para esse parâmetro de tipo.

## <a name="example"></a>Exemplo

O exemplo a seguir mostra uma definição de esqueleto de uma classe de dicionário genérica, incluindo uma função de esqueleto para adicionar uma nova entrada ao dicionário.

[!code-vb[VbVbalrStatements#3](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrStatements/VB/Class1.vb#3)]

## <a name="example"></a>Exemplo

Como `dictionary` o é genérico, o código que o usa pode criar uma variedade de objetos a partir dele, cada um com a mesma funcionalidade, mas agindo em um tipo de dados diferente. O exemplo a seguir mostra uma linha de código que cria um `dictionary` objeto com `String` entradas e `Integer` chaves.

[!code-vb[VbVbalrStatements#4](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrStatements/VB/Class1.vb#4)]

## <a name="example"></a>Exemplo

O exemplo a seguir mostra a definição esqueleto equivalente gerada pelo exemplo anterior.

[!code-vb[VbVbalrStatements#5](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrStatements/VB/Class1.vb#5)]

## <a name="see-also"></a>Confira também

- [Desse](of-clause.md)
- [Novo operador](../operators/new-operator.md)
- [Níveis de acesso no Visual Basic](../../programming-guide/language-features/declared-elements/access-levels.md)
- [Tipo de dados Object](../data-types/object-data-type.md)
- [Instrução Function](function-statement.md)
- [Instrução Structure](structure-statement.md)
- [Instrução Sub](sub-statement.md)
- [Como: Usar uma classe genérica](../../programming-guide/language-features/data-types/how-to-use-a-generic-class.md)
- [Covariância e contravariância](../../programming-guide/concepts/covariance-contravariance/index.md)
- [Em](../modifiers/in-generic-modifier.md)
- [Fora](../modifiers/out-generic-modifier.md)
