---
title: Segurança e condições de corrida
'description:': Describes pitfalls to avoid around security holes exploited by race conditions, including dispose methods, constructors, cached objects, and finalizers.
ms.date: 07/15/2020
ms.technology: dotnet-standard
dev_langs:
- csharp
- vb
helpviewer_keywords:
- security [.NET], race conditions
- race conditions
- secure coding, race conditions
- code security, race conditions
ms.assetid: ea3edb80-b2e8-4e85-bfed-311b20cb59b6
ms.openlocfilehash: a667bf69ba72cbe203bd2603c4c6b7a1e58a6d43
ms.sourcegitcommit: b7a8b09828bab4e90f66af8d495ecd7024c45042
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/04/2020
ms.locfileid: "87555104"
---
# <a name="security-and-race-conditions"></a>Segurança e condições de corrida

Outra área de preocupação é o potencial de brechas de segurança exploradas por condições de corrida. Há várias maneiras pelas quais isso pode acontecer. Os subtópicos a seguir descrevem algumas das principais armadilhas que o desenvolvedor deve evitar.  
  
## <a name="race-conditions-in-the-dispose-method"></a>Condições de corrida no método Dispose  

Se o método **Dispose** de uma classe (para obter mais informações, consulte [coleta de lixo](../garbage-collection/index.md)) não estiver sincronizado, é possível que o código de limpeza dentro de **Dispose** possa ser executado mais de uma vez, conforme mostrado no exemplo a seguir.  
  
```vb  
Sub Dispose()  
    If Not (myObj Is Nothing) Then  
       Cleanup(myObj)  
       myObj = Nothing  
    End If  
End Sub  
```  
  
```csharp  
void Dispose()
{  
    if (myObj != null)
    {  
        Cleanup(myObj);  
        myObj = null;  
    }  
}  
```  
  
Como essa implementação **Dispose** não é sincronizada, é possível que seja `Cleanup` chamado pelo primeiro thread e, em seguida, um segundo thread antes que `_myObj` seja definido como **NULL**. Se essa é uma preocupação de segurança depende do que acontece quando o `Cleanup` código é executado. Um grande problema com implementações de **Dispose** não sincronizadas envolve o uso de identificadores de recursos, como arquivos. A alienação imprópria pode fazer com que o identificador errado seja usado, o que geralmente leva a vulnerabilidades de segurança.  
  
## <a name="race-conditions-in-constructors"></a>Condições de corrida em construtores

Em alguns aplicativos, pode ser possível que outros threads acessem os membros da classe antes que seus construtores de classe tenham sido executados completamente. Você deve examinar todos os construtores de classe para garantir que não haja problemas de segurança se isso ocorrer ou sincronizar threads, se necessário.  
  
## <a name="race-conditions-with-cached-objects"></a>Condições de corrida com objetos armazenados em cache  

O código que armazena em cache informações de segurança ou usa a operação de [declaração](../../framework/misc/using-the-assert-method.md) de segurança de acesso ao código também pode ser vulnerável a condições de corrida se outras partes da classe não estiverem sincronizadas adequadamente, conforme mostrado no exemplo a seguir.  
  
```vb  
Sub SomeSecureFunction()  
    If SomeDemandPasses() Then  
        fCallersOk = True  
        DoOtherWork()  
        fCallersOk = False  
    End If  
End Sub  
  
Sub DoOtherWork()  
    If fCallersOK Then  
        DoSomethingTrusted()  
    Else  
        DemandSomething()  
        DoSomethingTrusted()  
    End If  
End Sub  
```  
  
```csharp  
void SomeSecureFunction()
{  
    if (SomeDemandPasses())
    {  
        fCallersOk = true;  
        DoOtherWork();  
        fCallersOk = false;  
    }  
}  
void DoOtherWork()
{  
    if (fCallersOK)
    {  
        DoSomethingTrusted();  
    }  
    else
    {  
        DemandSomething();  
        DoSomethingTrusted();  
    }  
}  
```  
  
Se houver outros caminhos para `DoOtherWork` que possam ser chamados de outro thread com o mesmo objeto, um chamador não confiável poderá passar além de uma demanda.  
  
Se o código armazenar em cache as informações de segurança, certifique-se de analisá-las para essa vulnerabilidade.  
  
## <a name="race-conditions-in-finalizers"></a>Condições de corrida em finalizadores  

As condições de corrida também podem ocorrer em um objeto que faz referência a um recurso estático ou não gerenciado que ele libera em seu finalizador. Se vários objetos compartilharem um recurso que é manipulado no finalizador de uma classe, os objetos deverão sincronizar todo o acesso a esse recurso.  
  
## <a name="see-also"></a>Confira também

- [Diretrizes de codificação segura](secure-coding-guidelines.md)
- [Segurança de ASP.NET Core](/aspnet/core/security/)
