---
title: Protegendo a manipulação de exceções
description: Veja como tornar o tratamento de exceções seguro no código .NET. Examine a ordem em que o código é executado se houver instruções Try, Except, catch e finally.
ms.date: 03/30/2017
dev_langs:
- cpp
helpviewer_keywords:
- code security, exception handling
- security [.NET Framework], exception handling
- secure coding, exception handling
- exception handling, security
ms.assetid: 1f3da743-9742-47ff-96e6-d0dd1e9e1c19
ms.openlocfilehash: c7643bb34da0cbcbd267fc90e6294bc0b565985e
ms.sourcegitcommit: c37e8d4642fef647ebab0e1c618ecc29ddfe2a0f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/06/2020
ms.locfileid: "87855771"
---
# <a name="securing-exception-handling"></a>Protegendo a manipulação de exceções

[!INCLUDE[net_security_note](../../../includes/net-security-note-md.md)]

Em Visual C++ e Visual Basic, uma expressão de filtro mais acima da pilha é executada antes de qualquer `finally` instrução. O bloco **Catch** associado a esse filtro é executado após a `finally` instrução. Para obter mais informações, consulte [usando exceções filtradas pelo usuário](../../standard/exceptions/using-user-filtered-exception-handlers.md). Esta seção examina as implicações de segurança deste pedido. Considere o seguinte exemplo de pseudocódigo que ilustra a ordem em que as instruções de filtro e as `finally` instruções são executadas.  
  
```cpp  
void Main()
{  
    try
    {  
        Sub();  
    }
    except (Filter())
    {  
        Console.WriteLine("catch");  
    }  
}  
bool Filter () {  
    Console.WriteLine("filter");  
    return true;  
}  
void Sub()
{  
    try
    {  
        Console.WriteLine("throw");  
        throw new Exception();  
    }
    finally
    {  
        Console.WriteLine("finally");  
    }  
}
```  
  
 Esse código imprime o seguinte.  
  
```output
Throw  
Filter  
Finally  
Catch  
```  
  
 O filtro é executado antes da `finally` instrução, portanto, problemas de segurança podem ser introduzidos por qualquer coisa que faça uma alteração de estado em que a execução de outro código possa tirar proveito. Por exemplo:  
  
```cpp  
try
{  
    Alter_Security_State();  
    // This means changing anything (state variables,  
    // switching unmanaged context, impersonation, and
    // so on) that could be exploited if malicious
    // code ran before state is restored.  
    Do_some_work();  
}
finally
{  
    Restore_Security_State();  
    // This simply restores the state change above.  
}  
```  
  
 Esse pseudocódigo permite que um filtro acima da pilha execute um código arbitrário. Outros exemplos de operações que teriam um efeito semelhante são a representação temporária de outra identidade, a definição de um sinalizador interno que ignora alguma verificação de segurança ou a alteração da cultura associada ao thread. A solução recomendada é introduzir um manipulador de exceção para isolar as alterações do código no estado do thread dos blocos de filtro dos chamadores. No entanto, é importante introduzir corretamente o manipulador de exceção ou esse problema não será corrigido. O exemplo a seguir alterna a cultura da interface do usuário, mas qualquer tipo de alteração de estado de thread pode ser exposta de forma semelhante.  
  
```cpp  
YourObject.YourMethod()  
{  
   CultureInfo saveCulture = Thread.CurrentThread.CurrentUICulture;  
   try {  
      Thread.CurrentThread.CurrentUICulture = new CultureInfo("de-DE");  
      // Do something that throws an exception.  
}  
   finally {  
      Thread.CurrentThread.CurrentUICulture = saveCulture;  
   }  
}  
```  
  
```vb  
Public Class UserCode  
   Public Shared Sub Main()  
      Try  
         Dim obj As YourObject = new YourObject  
         obj.YourMethod()  
      Catch e As Exception When FilterFunc  
         Console.WriteLine("An error occurred: '{0}'", e)  
         Console.WriteLine("Current Culture: {0}",
Thread.CurrentThread.CurrentUICulture)  
      End Try  
   End Sub  
  
   Public Function FilterFunc As Boolean  
      Console.WriteLine("Current Culture: {0}", Thread.CurrentThread.CurrentUICulture)  
      Return True  
   End Sub  
  
End Class  
```  
  
 A correção correta nesse caso é encapsular o bloco **try** / **finally** existente em um bloco **try** / **Catch** . Simplesmente introduzir uma cláusula **Catch-throw** no bloco **try** / **finally** existente não corrige o problema, conforme mostrado no exemplo a seguir.  
  
```cpp  
YourObject.YourMethod()  
{  
    CultureInfo saveCulture = Thread.CurrentThread.CurrentUICulture;  
  
    try
    {  
        Thread.CurrentThread.CurrentUICulture = new CultureInfo("de-DE");  
        // Do something that throws an exception.  
    }  
    catch { throw; }  
    finally
    {  
        Thread.CurrentThread.CurrentUICulture = saveCulture;  
    }  
}  
```  
  
 Isso não corrige o problema porque a `finally` instrução não foi executada antes do `FilterFunc` controle Gets.  
  
 O exemplo a seguir corrige o problema, garantindo que a `finally` cláusula tenha sido executada antes de oferecer uma exceção dos blocos de filtro de exceção dos chamadores.  
  
```cpp  
YourObject.YourMethod()  
{  
    CultureInfo saveCulture = Thread.CurrentThread.CurrentUICulture;  
    try
    {  
        try
        {  
            Thread.CurrentThread.CurrentUICulture = new CultureInfo("de-DE");  
            // Do something that throws an exception.  
        }  
        finally
        {  
            Thread.CurrentThread.CurrentUICulture = saveCulture;  
        }  
    }  
    catch { throw; }  
}  
```  
  
## <a name="see-also"></a>Veja também

- [Diretrizes de codificação segura](../../standard/security/secure-coding-guidelines.md)
