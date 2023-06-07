---
title: Declaração de variável
ms.date: 07/20/2015
helpviewer_keywords:
- variables [Visual Basic], declaring
- member variables [Visual Basic], declarations
- Dim statement [Visual Basic], variable declaration
- declaring variables [Visual Basic]
- variables [Visual Basic], scope
- variables [Visual Basic], data types
- data types [Visual Basic], variable declarations
- lifetime [Visual Basic], variables
- variables [Visual Basic], lifetime
- access levels [Visual Basic], variables
- scope [Visual Basic], declaration statements
- variables [Visual Basic], access level
- local variables [Visual Basic], declarations
- scope [Visual Basic], variables
ms.assetid: d8f10226-92b1-480f-9f53-df377b2d7e15
ms.openlocfilehash: 587cb84faa09b686361c255c413ad852780b8971
ms.sourcegitcommit: f8c270376ed905f6a8896ce0fe25b4f4b38ff498
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/04/2020
ms.locfileid: "84410290"
---
# <a name="variable-declaration-in-visual-basic"></a>Declaração de variável no Visual Basic
Você declara uma variável para especificar seu nome e suas características. A instrução de declaração para variáveis é a [instrução Dim](../../../language-reference/statements/dim-statement.md). Sua localização e seu conteúdo determinam as características da variável.  
  
 Para regras e considerações de nomenclatura de variáveis, consulte [nomes de elementos declarados](../declared-elements/declared-element-names.md).  
  
## <a name="declaration-levels"></a>Níveis de declaração  
  
### <a name="local-and-member-variables"></a>Variáveis locais e de membro  
 Uma *variável local* é uma que é declarada dentro de um procedimento. Uma *variável de membro* é um membro de um tipo de Visual Basic; Ele é declarado em nível de módulo, dentro de uma classe, estrutura ou módulo, mas não dentro de nenhum procedimento interno a essa classe, estrutura ou módulo.  
  
### <a name="shared-and-instance-variables"></a>Variáveis de instância e compartilhadas  
 Em uma classe ou estrutura, a categoria de uma variável de membro depende se ela é compartilhada ou não. Se ela for declarada com a palavra-chave [Shared](../../../language-reference/modifiers/shared.md) , ela será uma *variável compartilhada*e ela existirá em uma única cópia compartilhada entre todas as instâncias da classe ou estrutura.  
  
 Caso contrário, é uma *variável de instância*e uma cópia separada dela é criada para cada instância da classe ou estrutura. Uma determinada cópia de uma variável de instância está disponível somente para a instância da classe ou estrutura na qual ela foi criada. Ele é independente de uma cópia da variável de instância em qualquer outra instância da classe ou estrutura.  
  
## <a name="declaring-data-type"></a>Tipo de dados declarativo  
 A [cláusula as na instrução de declaração](../../../language-reference/statements/as-clause.md) permite que você defina o tipo de dados ou o tipo de objeto da variável que está declarando. Você pode especificar qualquer um dos seguintes tipos para uma variável:  
  
- Um tipo de dados elementar, como `Boolean` , `Long` ou`Decimal`  
  
- Um tipo de dados composto, como uma matriz ou estrutura  
  
- Um tipo de objeto, ou classe, definido em seu aplicativo ou em outro aplicativo  
  
- Uma classe .NET Framework, como <xref:System.Windows.Forms.Label> ou<xref:System.Windows.Forms.TextBox>  
  
- Um tipo de interface, como <xref:System.IComparable> ou<xref:System.IDisposable>  
  
 Você pode declarar várias variáveis em uma instrução sem precisar repetir o tipo de dados. Nas instruções a seguir, as variáveis `i` , `j` , e `k` são declaradas como tipo `Integer` , `l` e `m` as `Long` `x` `y` `Single` e como:  
  
```vb  
Dim i, j, k As Integer  
' All three variables in the preceding statement are declared as Integer.  
Dim l, m As Long, x, y As Single  
' In the preceding statement, l and m are Long, x and y are Single.  
```  
  
 Para obter mais informações sobre tipos de dados, consulte [tipos de dados](../data-types/index.md). Para obter mais informações sobre objetos, consulte [objetos e classes](../objects-and-classes/index.md) e [programação com componentes](https://docs.microsoft.com/previous-versions/visualstudio/visual-studio-2013/0ffkdtkf(v=vs.120)).  
  
## <a name="local-type-inference"></a>Inferência de tipo local  
 A *inferência de tipos* é usada para determinar os tipos de dados de variáveis locais declarados sem uma `As` cláusula. O compilador infere o tipo da variável do tipo da expressão de inicialização. Isso permite que você declare variáveis sem explicitamente indicar um tipo. No exemplo a seguir, `num1` e `num2` são fortemente tipadas como inteiros.  
  
 [!code-vb[VbVbalrTypeInference#1](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrTypeInference/VB/Class1.vb#1)]  
  
 Se você quiser usar a inferência de tipo local, `Option Infer` deverá ser definido como `On` . Para obter mais informações, consulte [Inferência de tipo de variável local](local-type-inference.md) e [Instrução Option Infer](../../../language-reference/statements/option-infer-statement.md).  
  
## <a name="characteristics-of-declared-variables"></a>Características das variáveis declaradas  
 O tempo de *vida* de uma variável é o período durante o qual ela está disponível para uso. Em geral, existe uma variável, desde que o elemento que a declara (como um procedimento ou classe) continue existindo. Se a variável não precisar continuar existente além do tempo de vida do elemento que a contém, você não precisará fazer nada de especial na declaração. Se a variável precisar continuar existindo mais do que o elemento que a contém, você poderá incluir a `Static` `Shared` palavra-chave ou em sua `Dim` instrução. Para obter mais informações, consulte [tempo de vida em Visual Basic](../declared-elements/lifetime.md).  
  
 O *escopo* de uma variável é o conjunto de todos os códigos que podem se referir a ele sem qualificar seu nome. O escopo de uma variável é determinado pelo local em que ela é declarada. O código localizado em uma determinada região pode usar as variáveis definidas nessa região sem precisar qualificar seus nomes. Para obter mais informações, consulte [escopo em Visual Basic](../declared-elements/scope.md).  
  
 O nível de *acesso* de uma variável é a extensão do código que tem permissão para acessá-lo. Isso é determinado pelo modificador de acesso (como [público](../../../language-reference/modifiers/public.md) ou [privado](../../../language-reference/modifiers/private.md)) que você usa na `Dim` instrução. Para obter mais informações, consulte [níveis de acesso em Visual Basic](../declared-elements/access-levels.md).  
  
## <a name="see-also"></a>Confira também

- [Como criar uma nova variável](how-to-create-a-new-variable.md)
- [Como inserir e remover dados de uma variável](how-to-move-data-into-and-out-of-a-variable.md)
- [Tipos de dados](../../../language-reference/data-types/index.md)
- [Protected](../../../language-reference/modifiers/protected.md)
- [Público](../../../language-reference/modifiers/friend.md)
- [Estático](../../../language-reference/modifiers/static.md)
- [Características do Elemento Declarado](../declared-elements/declared-element-characteristics.md)
- [Inferência de Tipo de Variável Local](local-type-inference.md)
- [Instrução Option Infer](../../../language-reference/statements/option-infer-statement.md)
