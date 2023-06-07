---
title: Referências a elementos declarados
ms.date: 07/20/2015
helpviewer_keywords:
- declared elements [Visual Basic]
- references [Visual Basic], declared elements
- qualified names [Visual Basic]
ms.assetid: d6301709-f4cc-4b7a-b8ba-80898f14ab46
ms.openlocfilehash: 23bff2eb098982f67ecb1b709e59096d5259a644
ms.sourcegitcommit: f8c270376ed905f6a8896ce0fe25b4f4b38ff498
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/04/2020
ms.locfileid: "84405177"
---
# <a name="references-to-declared-elements-visual-basic"></a>Referências a elementos declarados (Visual Basic)
Quando seu código se refere a um elemento declarado, o compilador Visual Basic corresponde ao nome em sua referência à declaração apropriada desse nome. Se mais de um elemento for declarado com o mesmo nome, você poderá controlar quais desses elementos serão referenciados *qualificando* seu nome.  
  
 O compilador tenta corresponder uma referência de nome a uma declaração de nome com o *escopo mais estreito*. Isso significa que ele começa com o código que faz a referência e funciona fora por meio de níveis sucessivos de elementos continentes.  
  
 O exemplo a seguir mostra referências a duas variáveis com o mesmo nome. O exemplo declara duas variáveis, cada uma denominada `totalCount` , em níveis diferentes de escopo no módulo `container` . Quando o procedimento é `showCount` exibido `totalCount` sem qualificação, o compilador Visual Basic resolve a referência à declaração com o escopo mais estreito, ou seja, a declaração local dentro do `showCount` . Quando ele se qualifica `totalCount` com o módulo recipiente `container` , o compilador resolve a referência à declaração com o escopo mais amplo.  
  
```vb  
' Assume these two modules are both in the same assembly.  
Module container  
    Public totalCount As Integer = 1  
    Public Sub showCount()  
        Dim totalCount As Integer = 6000  
        ' The following statement displays the local totalCount (6000).  
        MsgBox("Unqualified totalCount is " & CStr(totalCount))  
        ' The following statement displays the module's totalCount (1).  
        MsgBox("container.totalCount is " & CStr(container.totalCount))  
    End Sub  
End Module  
Module callingModule  
    Public Sub displayCount()  
        container.showCount()  
        ' The following statement displays the containing module's totalCount (1).  
        MsgBox("container.totalCount is " & CStr(container.totalCount))  
    End Sub  
End Module  
```  
  
## <a name="qualifying-an-element-name"></a>Qualificando um nome de elemento  
 Se você quiser substituir esse processo de pesquisa e especificar um nome declarado em um escopo mais amplo, você deverá *qualificar* o nome com o elemento recipiente do escopo mais amplo. Em alguns casos, você também pode precisar qualificar o elemento recipiente.  
  
 Qualificar um nome significa precedendo-o em sua instrução de origem com informações que identificam onde o elemento de destino é definido. Essas informações são chamadas de *cadeia de caracteres de qualificação*. Ele pode incluir um ou mais namespaces e um módulo, classe ou estrutura.  
  
 A cadeia de caracteres de qualificação deve especificar de forma não ambígua o módulo, a classe ou a estrutura que contém o elemento de destino. O contêiner pode, por sua vez, ser localizado em outro elemento recipiente, geralmente um namespace. Talvez seja necessário incluir vários elementos continentes na cadeia de caracteres de qualificação.  
  
#### <a name="to-access-a-declared-element-by-qualifying-its-name"></a>Para acessar um elemento declarado qualificando seu nome  
  
1. Determine o local no qual o elemento foi definido. Isso pode incluir um namespace ou até mesmo uma hierarquia de namespaces. Dentro do namespace de nível mais baixo, o elemento deve estar contido em um módulo, classe ou estrutura.  
  
    ```vb  
    ' Assume the following hierarchy exists outside your code.  
    Namespace outerSpace  
        Namespace innerSpace  
            Module holdsTotals  
                Public Structure totals  
                    Public thisTotal As Integer  
                    Public Shared grandTotal As Long  
                End Structure  
            End Module  
        End Namespace  
    End Namespace  
    ```  
  
2. Determine um caminho de qualificação com base no local do elemento de destino. Comece com o namespace de nível mais alto, vá para o namespace de nível mais baixo e termine com o módulo, classe ou estrutura que contém o elemento de destino. Cada elemento no caminho deve conter o elemento que o segue.  
  
     `outerSpace`→ `innerSpace` → `holdsTotals` →`totals`  
  
3. Prepare a cadeia de caracteres de qualificação para o elemento de destino. Coloque um ponto ( `.` ) após cada elemento no caminho. Seu aplicativo deve ter acesso a todos os elementos em sua cadeia de caracteres de qualificação.  
  
    ```vb  
    outerSpace.innerSpace.holdsTotals.totals.  
    ```  
  
4. Escreva a expressão ou instrução de atribuição referindo-se ao elemento de destino da maneira normal.  
  
    ```vb  
    grandTotal = 9000  
    ```  
  
5. Preceda o nome do elemento de destino com a cadeia de caracteres de qualificação. O nome deve seguir imediatamente o período ( `.` ) que segue o módulo, a classe ou a estrutura que contém o elemento.  
  
    ```vb  
    ' Assume the following module is part of your code.  
    Module accessGrandTotal  
        Public Sub setGrandTotal()  
            outerSpace.innerSpace.holdsTotals.totals.grandTotal = 9000  
        End Sub  
    End Module  
    ```  
  
6. O compilador usa a cadeia de caracteres de qualificação para encontrar uma declaração clara e não ambígua para a qual ela pode corresponder à referência de elemento de destino.  
  
 Você também pode ter que qualificar uma referência de nome se seu aplicativo tiver acesso a mais de um elemento de programação que tenha o mesmo nome. Por exemplo, os <xref:System.Windows.Forms> <xref:System.Web.UI.WebControls> namespaces e contêm uma `Label` classe ( <xref:System.Windows.Forms.Label?displayProperty=nameWithType> e <xref:System.Web.UI.WebControls.Label?displayProperty=nameWithType> ). Se seu aplicativo usar ambos, ou se ele definir sua própria `Label` classe, você deverá distinguir os diferentes `Label` objetos. Inclua o namespace ou o alias de importação na declaração de variável. O exemplo a seguir usa o alias de importação.  
  
```vb  
' The following statement must precede all your declarations.  
Imports win = System.Windows.Forms, web = System.Web.UI.WebControls  
' The following statement references the Windows.Forms.Label class.  
Dim winLabel As New win.Label()  
```  
  
## <a name="members-of-other-containing-elements"></a>Membros de outros elementos continentes  
 Quando você usa um membro não compartilhado de outra classe ou estrutura, primeiro você deve qualificar o nome do membro com uma variável ou expressão que aponta para uma instância da classe ou estrutura. No exemplo a seguir, `demoClass` é uma instância de uma classe chamada `class1` .  
  
```vb  
Dim demoClass As class1 = New class1()  
demoClass.someSub[(argumentlist)]  
```  
  
 Você não pode usar o nome de classe em si para qualificar um membro que não é [compartilhado](../../../language-reference/modifiers/shared.md). Primeiro, você deve criar uma instância em uma variável de objeto (nesse caso `demoClass` ) e, em seguida, referenciá-la pelo nome da variável.  
  
 Se uma classe ou estrutura tiver um `Shared` membro, você poderá qualificar esse membro com o nome de classe ou estrutura ou com uma variável ou expressão que aponte para uma instância.  
  
 Um módulo não tem nenhuma instância separada, e todos os seus membros são `Shared` por padrão. Portanto, você qualifica um membro de módulo com o nome do módulo.  
  
 O exemplo a seguir mostra referências qualificadas para procedimentos de membro de módulo. O exemplo declara dois `Sub` procedimentos, ambos nomeados `perform` , em diferentes módulos em um projeto. Cada um pode ser especificado sem qualificação dentro de seu próprio módulo, mas deve ser qualificado se for referenciado de qualquer outro lugar. Como a referência final no `module3` não está qualificada `perform` , o compilador não pode resolver essa referência.  
  
```vb  
' Assume these three modules are all in the same assembly.  
Module module1  
    Public Sub perform()  
        MsgBox("module1.perform() now returning")  
    End Sub  
End Module  
Module module2  
    Public Sub perform()  
        MsgBox("module2.perform() now returning")  
    End Sub  
    Public Sub doSomething()  
        ' The following statement calls perform in module2, the active module.  
        perform()  
        ' The following statement calls perform in module1.  
        module1.perform()  
    End Sub  
End Module  
Module module3  
    Public Sub callPerform()  
        ' The following statement calls perform in module1.  
        module1.perform()  
        ' The following statement makes an unresolvable name reference  
        ' and therefore generates a COMPILER ERROR.  
        perform() ' INVALID statement  
    End Sub  
End Module  
```  
  
## <a name="references-to-projects"></a>Referências a projetos  
 Para usar elementos [públicos](../../../language-reference/modifiers/public.md) definidos em outro projeto, primeiro você deve definir uma *referência* para o assembly ou a biblioteca de tipos do projeto. Para definir uma referência, clique em **Adicionar referência** no menu **projeto** ou use a opção de compilador de linha de comando [-Reference (Visual Basic)](../../../reference/command-line-compiler/reference.md) .  
  
 Por exemplo, você pode usar o modelo de objeto XML do .NET Framework. Se você definir uma referência ao <xref:System.Xml> namespace, poderá declarar e usar qualquer uma de suas classes, como <xref:System.Xml.XmlDocument> . O exemplo a seguir usa <xref:System.Xml.XmlDocument>.  
  
```vb  
' Assume this project has a reference to System.Xml  
' The following statement creates xDoc as an XML document object.  
Dim xDoc As System.Xml.XmlDocument  
```  
  
## <a name="importing-containing-elements"></a>Importando elementos continentes  
 Você pode usar a [instrução Imports (namespace e tipo do .net)](../../../language-reference/statements/imports-statement-net-namespace-and-type.md) para *importar* os namespaces que contêm os módulos ou as classes que você deseja usar. Isso permite que você consulte os elementos definidos em um namespace importado sem qualificar totalmente seus nomes. O exemplo a seguir reescreve o exemplo anterior para importar o <xref:System.Xml> namespace.  
  
```vb  
' Assume this project has a reference to System.Xml  
' The following statement must precede all your declarations.  
Imports System.Xml  
' The following statement creates xDoc as an XML document object.  
Dim xDoc As XmlDocument  
```  
  
 Além disso, a `Imports` instrução pode definir um *alias de importação* para cada namespace importado. Isso pode tornar o código-fonte mais curto e mais fácil de ler. O exemplo a seguir reescreve o exemplo anterior para usar `xD` como um alias para o <xref:System.Xml> namespace.  
  
```vb  
' Assume this project has a reference to System.Xml  
' The following statement must precede all your declarations.  
Imports xD = System.Xml  
' The following statement creates xDoc as an XML document object.  
Dim xDoc As xD.XmlDocument  
```  
  
 A `Imports` instrução não torna elementos de outros projetos disponíveis para seu aplicativo. Ou seja, ele não assume o lugar de definir uma referência. A importação de um namespace apenas remove a necessidade de qualificar os nomes definidos nesse namespace.  
  
 Você também pode usar a `Imports` instrução para importar módulos, classes, estruturas e enumerações. Você pode usar os membros de tais elementos importados sem qualificação. No entanto, você deve sempre qualificar Membros não compartilhados de classes e estruturas com uma variável ou expressão que é avaliada como uma instância da classe ou estrutura.  
  
## <a name="naming-guidelines"></a>Diretrizes de nomenclatura  
 Quando você define dois ou mais elementos de programação que têm o mesmo nome, uma *ambiguidade de nome* pode resultar quando o compilador tenta resolver uma referência a esse nome. Se mais de uma definição estiver no escopo ou se nenhuma definição estiver no escopo, a referência será não resolvível. Para obter um exemplo, consulte "exemplo de referência qualificada" nesta página de ajuda.  
  
 Você pode evitar a ambiguidade de nome fornecendo todos os seus elementos nomes exclusivos. Em seguida, você pode fazer referência a qualquer elemento sem precisar qualificar seu nome com um namespace, módulo ou classe. Você também reduz as chances de se referir acidentalmente ao elemento errado.  
  
## <a name="shadowing"></a>Sombreamento  
 Quando dois elementos de programação compartilham o mesmo nome, um deles pode ocultar ou *sombrear*o outro. Um elemento sombreado não está disponível para referência; em vez disso, quando o código usa o nome do elemento sombreado, o compilador Visual Basic o resolve para o elemento de sombreamento. Para obter uma explicação mais detalhada com exemplos, consulte [sombreamento em Visual Basic](shadowing.md).  
  
## <a name="see-also"></a>Confira também

- [Nomes de elementos declarados](declared-element-names.md)
- [Características do Elemento Declarado](declared-element-characteristics.md)
- [Gerenciando propriedades de solução e de projeto](/visualstudio/ide/managing-project-and-solution-properties)
- [Variáveis](../variables/index.md)
- [Instrução Imports (tipo e namespace .NET)](../../../language-reference/statements/imports-statement-net-namespace-and-type.md)
- [Novo operador](../../../language-reference/operators/new-operator.md)
- [Pública](../../../language-reference/modifiers/public.md)
