---
title: MDA reentrancy
description: Examine o MDA de reentrância, que poderá ser ativado se o heap de objeto estiver corrompido ou se outros erros graves ocorrerem ao fazer a transição de código nativo para o gerenciado.
ms.date: 03/30/2017
helpviewer_keywords:
- unmanaged code, debugging
- transitioning threads unmanaged to managed code
- reentrancy MDA
- reentrancy without an orderly transition
- managed debugging assistants (MDAs), reentrancy
- illegal reentrancy
- MDAs (managed debugging assistants), reentrancy
- threading [.NET Framework], managed debugging assistants
- managed code, debugging
- native debugging, MDAs
ms.assetid: 7240c3f3-7df8-4b03-bbf1-17cdce142d45
ms.openlocfilehash: f666e505b8382b0bec8dcfdb34c775850e46c429
ms.sourcegitcommit: c23d9666ec75b91741da43ee3d91c317d68c7327
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/01/2020
ms.locfileid: "85803099"
---
# <a name="reentrancy-mda"></a>MDA reentrancy
O MDA (Assistente de Depuração Gerenciado) de `reentrancy` é ativado quando é feita uma tentativa de transição de código nativo para gerenciado em casos nos quais um comutador anterior do código gerenciado para nativo não foi executado por meio de uma transição ordenada.  
  
## <a name="symptoms"></a>Sintomas  
 O heap do objeto está corrompido ou outros erros graves estão ocorrendo durante a transição de código nativo para gerenciado.  
  
 Threads que alternam entre o código nativo e o gerenciado em qualquer direção devem realizar uma transição ordenada. No entanto, certos pontos de extensibilidade de nível inferior no sistema operacional, tais como o manipulador de exceção em vetor, permitem mudanças de código gerenciado para código nativo sem realizar uma transição ordenada.  Essas opções estão sob controle do sistema operacional, em vez de sob o controle do CLR (Common Language Runtime).  Qualquer código nativo executado dentro desses pontos de extensibilidade deve evitar retornar a chamada para o código gerenciado.  
  
## <a name="cause"></a>Causa  
 Um ponto de extensibilidade do sistema de operacional de baixo nível, tal como o manipulador de exceção em vetor, foi ativado durante a execução de código gerenciado.  O código do aplicativo que é invocado por meio desse ponto de extensibilidade está tentando retornar a chamada para o código gerenciado.  
  
 Esse problema é sempre causado pelo código do aplicativo.  
  
## <a name="resolution"></a>Resolução  
 Examine o rastreamento de pilha do thread que ativou esse MDA.  O thread está tentando fazer uma chamada ilegal para o código gerenciado.  O rastreamento de pilha deve revelar o código do aplicativo usando esse ponto de extensibilidade, o código de sistema operacional que fornece esse ponto de extensibilidade e o código gerenciado que foi interrompido pelo ponto de extensibilidade.  
  
 Por exemplo, você verá o MDA ativado em uma tentativa de chamar código gerenciado de dentro de um manipulador de exceção em vetor.  Na pilha, você verá o código de tratamento de exceção do sistema operacional e algum código gerenciado disparando uma exceção, tal como uma <xref:System.DivideByZeroException> ou uma <xref:System.AccessViolationException>.  
  
 Neste exemplo, a resolução correta é implementar o manipulador de exceção em vetor completamente em código não gerenciado.  
  
## <a name="effect-on-the-runtime"></a>Efeito sobre o runtime  
 Esse MDA não tem efeito sobre o CLR.  
  
## <a name="output"></a>Saída  
 O MDA informa que está ocorrendo uma tentativa de reentrada ilegal.  Examine a pilha do thread para determinar por que isso está acontecendo e como corrigir o problema. O demonstrado a seguir é uma saída de exemplo.  
  
```output
Additional Information: Attempting to call into managed code without
transitioning out first.  Do not attempt to run managed code inside
low-level native extensibility points. Managed Debugging Assistant
'Reentrancy' has detected a problem in 'D:\ConsoleApplication1\  
ConsoleApplication1\bin\Debug\ConsoleApplication1.vshost.exe'.  
```  
  
## <a name="configuration"></a>Configuração  
  
```xml  
<mdaConfig>  
  <assistants>  
    <reentrancy />  
  </assistants>  
</mdaConfig>  
```  
  
## <a name="example"></a>Exemplo  
 O código de exemplo a seguir faz com que uma <xref:System.AccessViolationException> seja lançada.  Em versões do Windows que dão suporte à manipulação de exceção em vetor, isso fará com que o manipulador de exceção em vetor gerenciado seja chamado.  Se o MDA `reentrancy` estiver habilitado, o MDA será ativado durante a tentativa de chamada para `MyHandler` do código de suporte de manipulação de exceção em vetor do sistema operacional.  
  
```csharp
using System;  
public delegate int ExceptionHandler(IntPtr ptrExceptionInfo);  
  
public class Reenter
{  
    public static ExceptionHandler keepAlive;  
  
    [System.Runtime.InteropServices.DllImport("kernel32", ExactSpelling=true,
        CharSet=System.Runtime.InteropServices.CharSet.Auto)]  
    public static extern IntPtr AddVectoredExceptionHandler(int bFirst,
        ExceptionHandler handler);  
  
    static int MyHandler(IntPtr ptrExceptionInfo)
    {  
        // EXCEPTION_CONTINUE_SEARCH  
        return 0;  
    }  
    void Run() {}  
  
    static void Main()
    {  
        keepAlive = new ExceptionHandler(Reenter.MyHandler);  
        IntPtr ret = AddVectoredExceptionHandler(1, keepAlive);  
        try
        {  
            // Dispatch on null should AV.  
            Reenter r = null;
            r.Run();  
        }
        catch { }  
    }  
}  
```  
  
## <a name="see-also"></a>Consulte também

- [Diagnosticando erros com assistentes para depuração gerenciada](diagnosing-errors-with-managed-debugging-assistants.md)
