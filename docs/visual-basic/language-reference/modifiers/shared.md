---
title: Compartilhado
ms.date: 07/20/2015
f1_keywords:
- vb.Shared
helpviewer_keywords:
- Shared keyword [Visual Basic]
- members [Visual Basic], shared
- shared members
- nonshared
- shared [elements VB]
- elements [Visual Basic], shared
ms.assetid: 2bf7cf2c-b0dd-485e-8749-b5d674dab4cd
ms.openlocfilehash: b51c88e1af3a720912af8ba6aaf8ae4016af9cfa
ms.sourcegitcommit: 45c8eed045779b70a47b23169897459d0323dc89
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/18/2020
ms.locfileid: "84990194"
---
# <a name="shared-visual-basic"></a>Compartilhado (Visual Basic)

Especifica que um ou mais elementos de programação declarados estão associados a uma classe ou estrutura em tamanho grande, e não com uma instância específica da classe ou estrutura.

## <a name="when-to-use-shared"></a>Quando usar compartilhado

O compartilhamento de um membro de uma classe ou estrutura torna-o disponível para cada instância, em vez de *não compartilhado*, em que cada instância mantém sua própria cópia. O compartilhamento é útil, por exemplo, se o valor de uma variável se aplicar a todo o aplicativo. Se você declarar essa variável como sendo `Shared` , todas as instâncias acessarão o mesmo local de armazenamento e, se uma instância alterar o valor da variável, todas as instâncias acessarão o valor atualizado.

O compartilhamento não altera o nível de acesso de um membro. Por exemplo, um membro de classe pode ser compartilhado e privado (acessível somente de dentro da classe), ou não compartilhado e público. Para obter mais informações, consulte [níveis de acesso em Visual Basic](../../programming-guide/language-features/declared-elements/access-levels.md).

## <a name="rules"></a>Regras

- **Contexto de declaração.** Você pode usar `Shared` somente no nível do módulo. Isso significa que o contexto de declaração de um `Shared` elemento deve ser uma classe ou estrutura e não pode ser um arquivo de origem, namespace ou procedimento.

- **Modificadores combinados.** Você não pode especificar `Shared` junto com [Overrides](overrides.md), [Overridable, NotOverridable](overridable.md), [MustOverride](mustoverride.md)ou [static](static.md) na mesma declaração. [NotOverridable](notoverridable.md)

- **Acess.** Você acessa um elemento compartilhado qualificando-o com seu nome de classe ou estrutura, não com o nome da variável de uma instância específica de sua classe ou estrutura. Você não precisa nem criar uma instância de uma classe ou estrutura para acessar seus membros compartilhados.

     O exemplo a seguir chama o procedimento compartilhado <xref:System.Double.IsNaN%2A> exposto pela <xref:System.Double> estrutura.

     ```vb
     If Double.IsNaN(result) Then Console.WriteLine("Result is mathematically undefined.")
     ```

- **Compartilhamento implícito.** Você não pode usar o `Shared` modificador em uma [instrução const](../statements/const-statement.md), mas as constantes são compartilhadas implicitamente. Da mesma forma, você não pode declarar um membro de um módulo ou uma interface para ser `Shared` , mas eles são compartilhados implicitamente.

## <a name="behavior"></a>Comportamento

- **Repositório.** Uma variável ou evento compartilhado é armazenado na memória apenas uma vez, independentemente de quantas instâncias você criar de sua classe ou estrutura. Da mesma forma, um procedimento compartilhado ou propriedade mantém apenas um conjunto de variáveis locais.

- **Acessando por meio de uma variável de instância.** É possível acessar um elemento compartilhado qualificando-o com o nome de uma variável que contém uma instância específica de sua classe ou estrutura. Embora isso normalmente funcione conforme o esperado, o compilador gera uma mensagem de aviso e faz o acesso por meio do nome da classe ou da estrutura em vez da variável.

- **Acessando por meio de uma expressão de instância.** Se você acessar um elemento compartilhado por meio de uma expressão que retorna uma instância de sua classe ou estrutura, o compilador fará o acesso por meio do nome da classe ou da estrutura em vez de avaliar a expressão. Esse acesso produz resultados inesperados se você pretendia que a expressão execute outras ações, bem como o retorno da instância. O exemplo a seguir ilustra essa situação.
  
    ```vb
    Sub Main()
        ' The following line is the preferred way to access Total.
        ShareTotal.Total = 10

        ' The following line generates a compiler warning message and
        ' accesses total through class ShareTotal instead of through
        ' the variable instanceVar. This works as expected and adds
        ' 100 to Total.
        Dim instanceVar As New ShareTotal
        instanceVar.Total += 100

        ' The following line generates a compiler warning message and
        ' accesses total through class ShareTotal instead of calling
        ' ReturnClass(). This adds 1000 to total but does not work as
        ' expected, because the WriteLine in ReturnClass() does not run.
        Console.WriteLine("Value of total is " & CStr(ShareTotal.Total))
        ReturnClass().Total += 1000
    End Sub

    Public Function ReturnClass() As ShareTotal
        Console.WriteLine("Function ReturnClass() called")
        Return New ShareTotal
    End Function

    Public Class ShareTotal
        Public Shared Property Total As Integer
    End Class
    ```

     No exemplo anterior, o compilador gera uma mensagem de aviso duas vezes o código acessa a propriedade compartilhada `Total` por meio de uma instância. Em cada caso, ele faz o acesso diretamente por meio da classe `ShareTotal` e não faz uso de nenhuma instância. No caso da chamada pretendida para o procedimento `ReturnClass` , isso significa que ele não gera uma chamada para `ReturnClass` , portanto, a ação adicional de exibir "função ReturnClass () chamada" não é executada.

O `Shared` modificador pode ser usado nesses contextos:

- [Instrução Dim](../statements/dim-statement.md)
- [Instrução Event](../statements/event-statement.md)
- [Instrução Function](../statements/function-statement.md)
- [Instrução Operator](../statements/operator-statement.md)
- [Instrução Property](../statements/property-statement.md)
- [Instrução Sub](../statements/sub-statement.md)
  
## <a name="see-also"></a>Confira também

- [Sombras](shadows.md)
- [Estático](static.md)
- [Tempo de vida no Visual Basic](../../programming-guide/language-features/declared-elements/lifetime.md)
- [Procedimentos](../../programming-guide/language-features/procedures/index.md)
- [Estruturas](../../programming-guide/language-features/data-types/structures.md)
- [Objetos e classes](../../programming-guide/language-features/objects-and-classes/index.md)
