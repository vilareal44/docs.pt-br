---
title: Me, My, MyBase e MyClass
ms.date: 07/20/2015
f1_keywords:
- MyClass
- vb.Me
- MyBase
- vb.MyBase
- Me
- vb.MyClass
- vb.This
- vb.My
helpviewer_keywords:
- My object
- self-reference [Visual Basic], Me keyword
- MyClass keyword [Visual Basic], relationship to similar programming elements
- Me keyword [Visual Basic], relationship to similar programming elements
- Me keyword [Visual Basic], referring to the current instance of an object
- Me keyword [Visual Basic]
- self-reference
- current instance [Visual Basic], Me keyword
- MyBase keyword [Visual Basic], relationship to similar programming elements
ms.assetid: f8e241ae-b1ed-4886-9aa0-08c632154029
ms.openlocfilehash: b4470e5c178c0f66dc33956ea0131d4eabc51d46
ms.sourcegitcommit: f8c270376ed905f6a8896ce0fe25b4f4b38ff498
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/04/2020
ms.locfileid: "84397461"
---
# <a name="me-my-mybase-and-myclass-in-visual-basic"></a>Me, My, MyBase e MyClass no Visual Basic
`Me`, `My` , `MyBase` e `MyClass` em Visual Basic têm nomes semelhantes, mas finalidades diferentes. Este tópico descreve cada uma dessas entidades para diferenciá-las.  
  
## <a name="me"></a>Eu  
 A `Me` palavra-chave fornece uma maneira de se referir à instância específica de uma classe ou estrutura na qual o código está sendo executado no momento. `Me`comporta-se como uma variável de objeto ou uma variável de estrutura referindo-se à instância atual. O uso do `Me` é particularmente útil para passar informações sobre a instância em execução no momento de uma classe ou estrutura para um procedimento em outra classe, estrutura ou módulo.  
  
 Por exemplo, suponha que você tenha o procedimento a seguir em um módulo.  
  
```vb  
Sub ChangeFormColor(FormName As Form)  
   Randomize()  
   FormName.BackColor = Color.FromArgb(Rnd() * 256, Rnd() * 256, Rnd() * 256)  
End Sub  
```  
  
 Você pode chamar esse procedimento e passar a instância atual da <xref:System.Windows.Forms.Form> classe como um argumento usando a instrução a seguir.  
  
```vb  
ChangeFormColor(Me)  
```  
  
## <a name="my"></a>Meu  
 O `My` recurso fornece acesso fácil e intuitivo a várias classes de .NET Framework, permitindo que o usuário Visual Basic interaja com o computador, o aplicativo, as configurações, os recursos e assim por diante.  
  
## <a name="mybase"></a>MyBase  
 A `MyBase` palavra-chave se comporta como uma variável de objeto referindo-se à classe base da instância atual de uma classe. `MyBase`normalmente é usado para acessar membros da classe base que são substituídos ou sombreados em uma classe derivada. `MyBase.New`é usado para chamar explicitamente um construtor de classe base de um construtor de classe derivada.  
  
## <a name="myclass"></a>MyClass  
 A `MyClass` palavra-chave se comporta como uma variável de objeto referindo-se à instância atual de uma classe como implementada originalmente. `MyClass`é semelhante a `Me` , mas todas as chamadas de método nela são tratadas como se o método estivesse `NotOverridable` .  
  
## <a name="see-also"></a>Confira também

- [Noções básicas de herança](../language-features/objects-and-classes/inheritance-basics.md)
