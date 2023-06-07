---
title: Instrução Namespace
ms.date: 07/20/2015
f1_keywords:
- vb.Namespace
helpviewer_keywords:
- namespaces [Visual Basic], root
- Namespace statement [Visual Basic]
- namespaces [Visual Basic], nested
- declaring namespaces [Visual Basic], syntax
- namespaces [Visual Basic], declaring
- root namespaces
- declarations [Visual Basic], namespaces
ms.assetid: a31fbd95-9ace-4c3d-bbb1-51222a2272b2
ms.openlocfilehash: 0f1ba9a038fc604b6e4ede758891832e087fc096
ms.sourcegitcommit: f8c270376ed905f6a8896ce0fe25b4f4b38ff498
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/04/2020
ms.locfileid: "84404428"
---
# <a name="namespace-statement"></a>Instrução Namespace
Declara o nome de um namespace e faz com que o código-fonte que segue a declaração seja compilado dentro desse namespace.  
  
## <a name="syntax"></a>Sintaxe  
  
```vb  
Namespace [Global.] { name | name.name }  
    [ componenttypes ]  
End Namespace  
```  
  
## <a name="parts"></a>Partes  
 Global  
 Opcional. Permite que você defina um namespace fora do namespace raiz do seu projeto. Consulte [namespaces em Visual Basic](../../programming-guide/program-structure/namespaces.md).  
  
 `name`  
 Obrigatórios. Um nome exclusivo que identifica o namespace. Deve ser um identificador de Visual Basic válido. Para obter mais informações, consulte [nomes de elementos declarados](../../programming-guide/language-features/declared-elements/declared-element-names.md).  
  
 `componenttypes`  
 Opcional. Elementos que compõem o namespace. Elas incluem, mas não se limitam a, enumerações, estruturas, interfaces, classes, módulos, delegados e outros namespaces.  
  
 `End Namespace`  
 Finaliza um `Namespace` bloco.  
  
## <a name="remarks"></a>Comentários  
 Os namespaces são usados como um sistema organizacional. Eles fornecem uma maneira de classificar e apresentar elementos de programação que são expostos a outros programas e aplicativos. Observe que um namespace não é um *tipo* no sentido de que uma classe ou estrutura é — você não pode declarar um elemento de programação para ter o tipo de dados de um namespace.  
  
 Todos os elementos de programação declarados após uma `Namespace` instrução pertencem a esse namespace. Visual Basic continua a compilar elementos no último namespace declarado até encontrar uma `End Namespace` instrução ou outra `Namespace` instrução.  
  
 Se um namespace já estiver definido, mesmo fora do seu projeto, você poderá adicionar elementos de programação a ele. Para fazer isso, você usa uma `Namespace` instrução para direcionar Visual Basic para compilar elementos nesse namespace.  
  
 Você pode usar uma `Namespace` instrução somente no nível de arquivo ou de namespace. Isso significa que o *contexto de declaração* para um namespace deve ser um arquivo de origem ou outro namespace e não pode ser uma classe, estrutura, módulo, interface ou procedimento. Para obter mais informações, consulte [Contextos de declaração e níveis de acesso padrão](declaration-contexts-and-default-access-levels.md).  
  
 Você pode declarar um namespace dentro de outro. Não há nenhum limite estrito para os níveis de aninhamento que você pode declarar, mas lembre-se de que quando outro código acessa os elementos declarados no namespace mais interno, ele deve usar uma cadeia de caracteres de qualificação que contenha todos os nomes de namespace na hierarquia de aninhamento.  
  
## <a name="access-level"></a>Nível de acesso  
 Os namespaces são tratados como se tivessem um `Public` nível de acesso. Um namespace pode ser acessado do código em qualquer lugar no mesmo projeto, de outros projetos que fazem referência ao projeto e de qualquer assembly criado a partir do projeto.  
  
 Elementos de programação declarados no nível de namespace, o que significa que em um namespace, mas não dentro de qualquer outro elemento, pode ter `Public` ou `Friend` acessar. Se não for especificado, o nível de acesso desse elemento `Friend` será usado por padrão. Os elementos que você pode declarar no nível de namespace incluem classes, estruturas, módulos, interfaces, enumerações e delegados. Para obter mais informações, consulte [Contextos de declaração e níveis de acesso padrão](declaration-contexts-and-default-access-levels.md).  
  
## <a name="root-namespace"></a>Namespace raiz  
 Todos os nomes de namespace em seu projeto são baseados em um *namespace raiz*. O Visual Studio atribui o nome do projeto como o namespace raiz padrão para todo o código em seu projeto. Por exemplo, se seu projeto for nomeado `Payroll` , seus elementos de programação pertencerão ao namespace `Payroll` . Se você declarar `Namespace funding` , o nome completo desse namespace será `Payroll.funding` .  
  
 Se você quiser especificar um namespace existente em uma `Namespace` instrução, como no exemplo de classe de lista genérica, você pode definir o namespace raiz como um valor nulo. Para fazer isso, clique em **Propriedades do projeto** no menu **projeto** e desmarque a entrada do **namespace raiz** para que a caixa fique vazia. Se você não fez isso no exemplo de classe de lista genérica, o compilador Visual Basic executaria `System.Collections.Generic` como um novo namespace no projeto `Payroll` , com o nome completo de `Payroll.System.Collections.Generic` .  
  
 Como alternativa, você pode usar a `Global` palavra-chave para se referir a elementos de namespaces definidos fora do seu projeto. Isso permite que você mantenha o nome do projeto como o namespace raiz. Isso reduz a chance de Mesclar de forma não intencional seus elementos de programação com os de namespaces existentes. Para obter mais informações, consulte a seção "palavra-chave global em nomes totalmente qualificados" em [namespaces no Visual Basic](../../programming-guide/program-structure/namespaces.md).  
  
 A `Global` palavra-chave também pode ser usada em uma instrução de namespace. Isso permite que você defina um namespace fora do namespace raiz do seu projeto. Para obter mais informações, consulte a seção "palavra-chave global em instruções de namespace" em [namespaces no Visual Basic](../../programming-guide/program-structure/namespaces.md).  
  
 **Solução.** O namespace raiz pode levar a concatenações inesperadas de nomes de namespace. Se você fizer referência a namespaces definidos fora do seu projeto, o compilador Visual Basic poderá interpretar como namespaces aninhados no namespace raiz. Nesse caso, o compilador não reconhece nenhum tipo que já tenha sido definido nos namespaces externos. Para evitar isso, defina o namespace raiz como um valor nulo, conforme descrito em "namespace raiz", ou use a `Global` palavra-chave para acessar elementos de namespaces externos.  
  
## <a name="attributes-and-modifiers"></a>Atributos e modificadores  
 Você não pode aplicar atributos a um namespace. Um atributo contribui com informações para os metadados do assembly, o que não é significativo para classificadores de origem, como namespaces.  
  
 Você não pode aplicar nenhum modificador de acesso ou de procedimento, ou qualquer outro modificador, a um namespace. Como não é um tipo, esses modificadores não são significativos.  
  
## <a name="example"></a>Exemplo  
 O exemplo a seguir declara dois namespaces, um aninhado no outro.  
  
 [!code-vb[VbVbalrStatements#43](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrStatements/VB/Class1.vb#43)]  
  
## <a name="example"></a>Exemplo  
 O exemplo a seguir declara vários namespaces aninhados em uma única linha e é equivalente ao exemplo anterior.  
  
 [!code-vb[VbVbalrStatements#41](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrStatements/VB/Class1.vb#41)]  
  
## <a name="example"></a>Exemplo  
 O exemplo a seguir acessa a classe definida nos exemplos anteriores.  
  
 [!code-vb[VbVbalrStatements#42](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrStatements/VB/Class1.vb#42)]  
  
## <a name="example"></a>Exemplo  
 O exemplo a seguir define o esqueleto de uma nova classe genérica List e a adiciona ao <xref:System.Collections.Generic?displayProperty=nameWithType> namespace.  
  
```vb  
Namespace System.Collections.Generic  
    Class specialSortedList(Of T)  
        Inherits List(Of T)  
        ' Insert code to define the special generic list class.  
    End Class  
End Namespace  
```  
  
## <a name="see-also"></a>Confira também

- [Instrução Imports (tipo e namespace .NET)](imports-statement-net-namespace-and-type.md)
- [Nomes de elementos declarados](../../programming-guide/language-features/declared-elements/declared-element-names.md)
- [Namespaces no Visual Basic](../../programming-guide/program-structure/namespaces.md)
