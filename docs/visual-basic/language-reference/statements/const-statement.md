---
title: Instrução Const
ms.date: 05/12/2018
f1_keywords:
- vb.Const
helpviewer_keywords:
- Const statement [Visual Basic]
ms.assetid: 495b318d-b7c5-4198-94f8-0790a541b07a
ms.openlocfilehash: 3b05d4067ef99e03df07d2c316c982051180d961
ms.sourcegitcommit: f8c270376ed905f6a8896ce0fe25b4f4b38ff498
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/04/2020
ms.locfileid: "84382101"
---
# <a name="const-statement-visual-basic"></a>Instrução Const (Visual Basic)

Declara e define uma ou mais constantes.

## <a name="syntax"></a>Sintaxe

```vb
[ <attributelist> ] [ accessmodifier ] [ Shadows ]
Const constantlist
```

## <a name="parts"></a>Partes

`attributelist`  
Opcional. Lista de atributos que se aplicam a todas as constantes declaradas nesta instrução. Consulte a [lista de atributos](attribute-list.md) entre colchetes angulares (" `<` " e " `>` ").

`accessmodifier`  
Opcional. Use isso para especificar qual código pode acessar essas constantes. Pode ser [público](../modifiers/public.md), [protegido](../modifiers/protected.md), [amigo](../modifiers/friend.md), [amigo protegido](../modifiers/protected-friend.md), [privado](../modifiers/private.md)ou [privado protegido](../modifiers/private-protected.md).

`Shadows`  
Opcional. Use isso para redeclarar e ocultar um elemento de programação em uma classe base. Consulte [Shadows](../modifiers/shadows.md).

`constantlist`  
Obrigatórios. Lista de constantes que estão sendo declaradas nesta instrução.

`constant` `[ ,` `constant` `... ]`

Cada `constant` uma tem a seguinte sintaxe e partes:

`constantname` `[ As` `datatype` `] =` `initializer`

|Parte|Descrição|
|----------|-----------------|
|`constantname`|Obrigatórios. Nome da constante. Consulte [nomes de elementos declarados](../../programming-guide/language-features/declared-elements/declared-element-names.md).|
|`datatype`|Obrigatório se `Option Strict` for `On` . Tipo de dados da constante.|
|`initializer`|Obrigatórios. Expressão que é avaliada em tempo de compilação e atribuída à constante.|

## <a name="remarks"></a>Comentários

Se você tiver um valor que nunca é alterado em seu aplicativo, poderá definir uma constante nomeada e usá-la no lugar de um valor literal. Um nome é mais fácil de lembrar do que um valor. Você pode definir a constante apenas uma vez e usá-la em vários lugares em seu código. Se, em uma versão posterior, você precisar redefinir o valor, a `Const` instrução será o único lugar necessário para fazer uma alteração.

Você pode usar `Const` somente no nível do módulo ou do procedimento. Isso significa que o *contexto de declaração* para uma variável deve ser uma classe, estrutura, módulo, procedimento ou bloco, e não pode ser um arquivo de origem, namespace ou interface. Para obter mais informações, consulte [Contextos de declaração e níveis de acesso padrão](declaration-contexts-and-default-access-levels.md).

As constantes locais (dentro de um procedimento) assumem o padrão de acesso público e você não pode usar nenhum modificador de acesso neles. As constantes de membro de classe e módulo (fora de qualquer procedimento) são padrão para acesso privado e constantes de membro de estrutura padrão para acesso público. Você pode ajustar seus níveis de acesso com os modificadores de acesso.

## <a name="rules"></a>Regras

- **Contexto de declaração.** Uma constante declarada no nível do módulo, fora de qualquer procedimento, é uma *constante membro*; é um membro da classe, da estrutura ou do módulo que o declara.

  Uma constante declarada no nível de procedimento é uma *constante local*; é local para o procedimento ou bloco que o declara.

- **Atributos.** Você pode aplicar atributos somente a constantes de membro, não a constantes locais. Um atributo contribui com informações para os metadados do assembly, o que não é significativo para armazenamento temporário, como constantes locais.

- **Modificadores.** Por padrão, todas as constantes são `Shared` , `Static` e `ReadOnly` . Você não pode usar nenhuma dessas palavras-chave ao declarar uma constante.

  No nível do procedimento, você não pode usar `Shadows` ou nenhum modificador de acesso para declarar constantes locais.

- **Várias constantes.** Você pode declarar várias constantes na mesma instrução de declaração, especificando a `constantname` parte para cada uma. Várias constantes são separadas por vírgulas.

## <a name="data-type-rules"></a>Regras de tipo de dados

- **Tipos de dados.** A `Const` instrução pode declarar o tipo de dados de uma variável. Você pode especificar qualquer tipo de dados ou o nome de uma enumeração.

- **Tipo padrão.** Se você não especificar `datatype` , a constante usará o tipo de dados de `initializer` . Se você especificar `datatype` e `initializer` , o tipo de dados de `initializer` deve ser conversível para `datatype` . Se nem `datatype` nem `initializer` estiver presente, o tipo de dados padrão será `Object` .

- **Tipos diferentes.** Você pode especificar tipos de dados diferentes para constantes diferentes usando uma `As` cláusula separada para cada variável que declarar. No entanto, você não pode declarar várias constantes para serem do mesmo tipo usando uma `As` cláusula comum.

- **Initialization.** Você deve inicializar o valor de cada constante no `constantlist` . Você usa `initializer` o para fornecer uma expressão a ser atribuída à constante. A expressão pode ser qualquer combinação de literais, outras constantes que já estão definidas e membros de enumeração que já estão definidos. Você pode usar operadores aritméticos e lógicos para combinar esses elementos.

  Você não pode usar variáveis ou funções no `initializer` . No entanto, você pode usar palavras-chave de conversão, como `CByte` e `CShort` . Você também pode usar `AscW` se chamá-lo com uma constante `String` ou um `Char` argumento, pois isso pode ser avaliado no momento da compilação.

## <a name="behavior"></a>Comportamento

- **Com.** As constantes locais são acessíveis somente de dentro de seu procedimento ou bloco. Constantes de membro podem ser acessadas de qualquer lugar dentro de sua classe, estrutura ou módulo.

- **Aprovação.** O código fora de uma classe, estrutura ou módulo deve qualificar o nome de uma constante de membro com o nome dessa classe, estrutura ou módulo. O código fora de um procedimento ou bloco não pode se referir a nenhuma constante local dentro desse procedimento ou bloco.

## <a name="example"></a>Exemplo

O exemplo a seguir usa a `Const` instrução para declarar constantes para uso em vez de valores literais.

[!code-vb[VbVbalrStatements#13](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrStatements/VB/Class1.vb#13)]

## <a name="example"></a>Exemplo

Se você definir uma constante com o tipo `Object` de dados, o compilador Visual Basic fornecerá o tipo de `initializer` , em vez de `Object` . No exemplo a seguir, a constante `naturalLogBase` tem o tipo de tempo de execução `Decimal` .

[!code-vb[VbVbalrStatements#87](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrStatements/VB/Class1.vb#87)]

O exemplo anterior usa o <xref:System.Type.ToString%2A> método no <xref:System.Type> objeto retornado pelo [Operador GetType](../operators/gettype-operator.md), porque <xref:System.Type> não pode ser convertido em `String` using `CStr` .

## <a name="see-also"></a>Confira também

- <xref:Microsoft.VisualBasic.Strings.Asc%2A>
- <xref:Microsoft.VisualBasic.Strings.AscW%2A>
- [Instrução Enum](enum-statement.md)
- [#Const diretiva](../directives/const-directive.md)
- [Instrução Dim](dim-statement.md)
- [Instrução ReDim](redim-statement.md)
- [Conversões implícitas e explícitas](../../programming-guide/language-features/data-types/implicit-and-explicit-conversions.md)
- [Constantes e enumerações](../../programming-guide/language-features/constants-enums/index.md)
- [Constantes e enumerações](../constants-and-enumerations.md)
- [Funções de conversão do tipo](../functions/type-conversion-functions.md)
