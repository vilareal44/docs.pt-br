---
title: 'Como: Declarar uma estrutura'
ms.date: 07/20/2015
helpviewer_keywords:
- declarations [Visual Basic], structures
- structure statements [Visual Basic]
- statements [Visual Basic], structure
- structures [Visual Basic], declaring
ms.assetid: d5e98381-eb81-47d4-af83-48cc534a2572
ms.openlocfilehash: a6b70d0973e92db90e35e61b7fed2279c5b0bac3
ms.sourcegitcommit: f8c270376ed905f6a8896ce0fe25b4f4b38ff498
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/04/2020
ms.locfileid: "84393968"
---
# <a name="how-to-declare-a-structure-visual-basic"></a>Como declarar uma estrutura (Visual Basic)
Você começa uma declaração de estrutura com a [instrução Structure](../../../language-reference/statements/structure-statement.md)e a encerra com a `End Structure` instrução. Entre essas duas instruções, você deve declarar pelo menos um *elemento*. Os elementos podem ser de qualquer tipo de dados, mas pelo menos um deve ser uma variável não compartilhada ou um evento não compartilhado e não-personalizado.  
  
 Você não pode inicializar nenhum dos elementos de estrutura na declaração de estrutura. Quando você declara uma variável para ser de um tipo de estrutura, você atribui valores aos elementos acessando-os por meio da variável.  
  
 Para obter uma discussão sobre as diferenças entre estruturas e classes, consulte [estruturas e classes](structures-and-classes.md).  
  
 Para fins de demonstração, considere uma situação em que você deseja acompanhar o nome, a extensão do telefone e o salário de um funcionário. Uma estrutura permite que você faça isso em uma única variável.  
  
### <a name="to-declare-a-structure"></a>Para declarar uma estrutura  
  
1. Crie as instruções inicial e final para a estrutura.  
  
     Você pode especificar o nível de acesso de uma estrutura usando a palavra-chave [Public](../../../language-reference/modifiers/public.md), [Protected](../../../language-reference/modifiers/protected.md), [Friend](../../../language-reference/modifiers/friend.md)ou [Private](../../../language-reference/modifiers/private.md) , ou pode deixar que ela seja padrão `Public` .  
  
    ```vb  
    Private Structure employee  
    End Structure  
    ```  
  
2. Adicione elementos ao corpo da estrutura.  
  
     Uma estrutura deve ter pelo menos um elemento. Você deve declarar todos os elementos e especificar um nível de acesso para ele. Se você usar a [instrução Dim](../../../language-reference/statements/dim-statement.md) sem nenhuma palavra-chave, a acessibilidade será padronizada para `Public` .  
  
    ```vb  
    Private Structure employee  
        Public givenName As String  
        Public familyName As String  
        Public phoneExtension As Long  
        Private salary As Decimal  
        Public Sub giveRaise(raise As Double)  
            salary *= raise  
        End Sub  
        Public Event salaryReviewTime()  
    End Structure  
    ```  
  
     O `salary` campo no exemplo anterior é `Private` , o que significa que ele está inacessível fora da estrutura, mesmo da classe que a contém. No entanto, o `giveRaise` procedimento é `Public` , portanto, pode ser chamado de fora da estrutura. Da mesma forma, você pode gerar o `salaryReviewTime` evento de fora da estrutura.  
  
     Além de variáveis, `Sub` procedimentos e eventos, você também pode definir constantes, `Function` procedimentos e propriedades em uma estrutura. Você pode designar, no máximo, uma propriedade como a *propriedade padrão*, desde que ela aceite pelo menos um argumento. Você pode manipular um evento com um procedimento [compartilhado](../../../language-reference/modifiers/shared.md) `Sub` . Para obter mais informações, consulte [como: declarar e chamar uma propriedade padrão em Visual Basic](../procedures/how-to-declare-and-call-a-default-property.md).  
  
## <a name="see-also"></a>Confira também

- [Tipos de dados](index.md)
- [Tipos de dados elementares](elementary-data-types.md)
- [Tipos de dados compostos](composite-data-types.md)
- [Tipos de valor e referência](value-types-and-reference-types.md)
- [Estruturas](structures.md)
- [Solução de problemas de tipos de dados](troubleshooting-data-types.md)
- [Variáveis de estrutura](structure-variables.md)
- [Estruturas e outros elementos de programação](structures-and-other-programming-elements.md)
- [Estruturas e classes](structures-and-classes.md)
- [Tipo de dados definido pelo usuário](../../../language-reference/data-types/user-defined-data-type.md)
