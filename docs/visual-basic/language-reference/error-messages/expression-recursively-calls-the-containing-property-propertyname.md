---
title: A expressão chama recursivamente a propriedade que contém '<propertyname>'
ms.date: 07/20/2015
f1_keywords:
- vbc42026
- BC42026
helpviewer_keywords:
- BC42026
ms.assetid: 4fde9db6-3bf3-48dc-8e05-981bf08969da
ms.openlocfilehash: e3a9f4cf2f4105d2c449813bf0c593860df7d1f0
ms.sourcegitcommit: f8c270376ed905f6a8896ce0fe25b4f4b38ff498
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/04/2020
ms.locfileid: "84409498"
---
# <a name="expression-recursively-calls-the-containing-property-propertyname"></a>A expressão chama recursivamente a propriedade que contém '\<propertyname>'
Uma instrução no `Set` procedimento de uma definição de propriedade armazena um valor no nome da propriedade.  
  
 A abordagem recomendada para manter o valor de uma propriedade é definir uma `Private` variável no contêiner da propriedade e usá-la nos `Get` `Set` procedimentos e. O `Set` procedimento deve armazenar o valor de entrada nessa `Private` variável.  
  
 O `Get` procedimento se comporta como um `Function` procedimento, portanto, ele pode atribuir um valor ao nome da propriedade e ao controle de retorno ao encontrar a `End Get` instrução. No entanto, a abordagem recomendada é incluir a `Private` variável como o valor em uma [instrução return](../statements/return-statement.md).  
  
 O `Set` procedimento se comporta como um `Sub` procedimento, que não retorna um valor. Portanto, o nome do procedimento ou da propriedade não tem um significado especial dentro de um `Set` procedimento e você não pode armazenar um valor nele.  
  
 O exemplo a seguir ilustra a abordagem que pode causar esse erro, seguida da abordagem recomendada.  
  
```vb  
Public Class illustrateProperties  
' The code in the following property causes this error.  
    Public Property badProp() As Char  
        Get  
            Dim charValue As Char  
            ' Insert code to update charValue.  
            badProp = charValue  
        End Get  
        Set(ByVal Value As Char)  
            ' The following statement causes this error.  
            badProp = Value  
            ' The value stored in the local variable badProp  
            ' is not used by the Get procedure in this property.  
        End Set  
    End Property  
' The following code uses the recommended approach.  
    Private propValue As Char  
    Public Property goodProp() As Char  
        Get  
            ' Insert code to update propValue.  
            Return propValue  
        End Get  
        Set(ByVal Value As Char)  
            propValue = Value  
        End Set  
    End Property  
End Class  
```  
  
 Por padrão, esta mensagem é um aviso. Para obter mais informações sobre como ocultar avisos ou tratar avisos como erros, consulte [Configurando avisos no Visual Basic](/visualstudio/ide/configuring-warnings-in-visual-basic).  
  
 **ID do erro:** BC42026  
  
## <a name="to-correct-this-error"></a>Para corrigir este erro  
  
- Reescreva a definição de propriedade para usar a abordagem recomendada, conforme ilustrado no exemplo anterior.  
  
## <a name="see-also"></a>Confira também

- [Procedimentos de propriedade](../../programming-guide/language-features/procedures/property-procedures.md)
- [Instrução Property](../statements/property-statement.md)
- [Instrução SET](../statements/set-statement.md)
