---
title: MDA callbackOnCollectedDelegate
description: Examine o MDA (Assistente de depuração gerenciada) do callbackOnCollectedDelegate no .NET, que é invocado se um retorno de chamada ocorrer depois que o delegado for coletado pelo lixo.
ms.date: 03/30/2017
dev_langs:
- cpp
helpviewer_keywords:
- MDAs (managed debugging assistants), garbage collection
- managed debugging assistants (MDAs), callback on collected delegates
- MDAs (managed debugging assistants), callback on collected delegates
- callback on delegate after garbage collection
- callbackOnCollectedDelegate MDA
- managed debugging assistants (MDAs), garbage collection
- call back collected delegates
- garbage collection, run-time errors
- delegates [.NET Framework], garbage collection
ms.assetid: 398b0ce0-5cc9-4518-978d-b8263aa21e5b
ms.openlocfilehash: 32f02a4e65455f11f3bfa9260caae8b4e48f494e
ms.sourcegitcommit: a2c8b19e813a52b91facbb5d7e3c062c7188b457
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/26/2020
ms.locfileid: "85416025"
---
# <a name="callbackoncollecteddelegate-mda"></a>MDA callbackOnCollectedDelegate
O MDA (assistente para depuração gerenciada) `callbackOnCollectedDelegate` é ativado se um representante tem o marshaling realizado de um código gerenciado para um código não gerenciado como um ponteiro de função e um retorno de chamada é colocado nesse ponteiro de função depois que o representante foi coletado como lixo.  
  
## <a name="symptoms"></a>Sintomas  
 Violações de acesso ocorrem ao tentar chamar o código gerenciado por meio de ponteiros de função que foram obtidos de representantes gerenciados. Essas falhas, embora não sejam bugs de CLR (Common Language Runtime), podem parecer ser bugs porque a violação de acesso ocorre no código CLR.  
  
 A falha não é consistente; às vezes, a chamada no ponteiro de função é bem-sucedida e, às vezes, ela falha. A falha pode ocorrer somente sob carga pesada ou em um número aleatório de tentativas.  
  
## <a name="cause"></a>Causa  
 O representante do qual o ponteiro de função foi criado e exposto ao código não gerenciado foi coletado como lixo. Quando o componente não gerenciado tenta chamar no ponteiro de função, ele gera uma violação de acesso.  
  
 A falha parece aleatória porque ela depende de quando ocorre a coleta de lixo. Se um representante é qualificado para a coleta, a coleta de lixo pode ocorrer após o retorno de chamada e depois que a chamada é bem-sucedida. Em outras ocasiões, a coleta de lixo ocorre antes do retorno de chamada, o retorno de chamada gera uma violação de acesso e o programa é interrompido.  
  
 A probabilidade da falha depende do tempo entre o marshaling do representante e o retorno de chamada no ponteiro de função, bem como a frequência das coletas de lixo. A falha é esporádica se o tempo entre o marshaling do representante e o retorno de chamada resultante é curto. Esse geralmente é o caso se o método não gerenciado que recebe o ponteiro de função não salva o ponteiro de função para uso posterior, mas em vez disso, realiza uma chamada novamente no ponteiro de função imediatamente para concluir sua operação antes de retornar. Da mesma forma, mais coletas de lixo ocorrem quando um sistema está sob carga pesada, o que torna mais provável que ocorra uma coleta de lixo antes do retorno de chamada.  
  
## <a name="resolution"></a>Resolução  
 Depois que um representante tem o marshaling realizado como um ponteiro de função não gerenciada, o coletor de lixo não pode acompanhar seu tempo de vida. Em vez disso, o código deve manter uma referência ao representante durante o tempo de vida do ponteiro de função não gerenciada. Porém, antes de poder fazer isso, primeiro você deve identificar qual representante foi coletado. Quando o MDA é ativado, ele fornece o nome do tipo do representante. Use esse nome para pesquisar o código em busca da invocação de plataforma ou de assinaturas COM que passam o representante para o código não gerenciado. O representante inválido é passado por meio de um desses sites de chamada. Você também pode habilitar o MDA `gcUnmanagedToManaged` a forçar uma coleta de lixo antes de cada retorno de chamada no runtime. Isso removerá a incerteza introduzida pela coleta de lixo, garantindo que uma coleta de lixo sempre ocorra antes do retorno de chamada. Depois de saber qual representante foi coletado, altere o código para manter uma referência ao representante no lado gerenciado durante o tempo de vida do ponteiro de função não gerenciada com marshaling.  
  
## <a name="effect-on-the-runtime"></a>Efeito sobre o runtime  
 Quando os representantes têm o marshaling realizado como ponteiros de função, o runtime aloca uma conversão que faz a transição do não gerenciado para o gerenciado. Essa conversão é o que o código não gerenciado, na verdade, chama antes que o representante gerenciado seja finalmente invocado. Sem o MDA `callbackOnCollectedDelegate` habilitado, o código de marshaling não gerenciado é excluído quando o representante é coletado. Com o MDA `callbackOnCollectedDelegate` habilitado, o código de marshaling não gerenciado não é excluído imediatamente quando o representante é coletado. Em vez disso, as últimas 1.000 instâncias são mantidas ativas por padrão e alteradas para ativar o MDA quando chamadas. A conversão é, no final das contas, excluída depois que 1.001 representantes com marshaling são coletados.  
  
## <a name="output"></a>Saída  
 O MDA relata o nome do tipo do representante que foi coletado antes da tentativa de um retorno de chamada em seu ponteiro de função não gerenciada.  
  
## <a name="configuration"></a>Configuração  
 O exemplo a seguir mostra as opções de configuração do aplicativo. Ele define o número de conversões que o MDA mantém ativas como 1.500. O valor padrão `listSize` é 1.000, o mínimo é 50 e o máximo é 2.000.  
  
```xml  
<mdaConfig>  
  <assistants>  
    <callbackOnCollectedDelegate listSize="1500" />  
  </assistants>  
</mdaConfig>  
```  
  
## <a name="example"></a>Exemplo  
 O seguinte exemplo demonstra uma situação que pode ativar esse MDA:  
  
```cpp
// Library.cpp : Defines the unmanaged entry point for the DLL application.  
#include "windows.h"  
#include "stdio.h"  
  
void (__stdcall *g_pfTarget)();  
  
void __stdcall Initialize(void __stdcall pfTarget())  
{  
    g_pfTarget = pfTarget;  
}  
  
void __stdcall Callback()  
{  
    g_pfTarget();  
}
```

```csharp
// C# Client  
using System;  
using System.Runtime.InteropServices;  
  
public class Entry  
{  
    public delegate void DCallback();  
  
    public static void Main()  
    {  
        new Entry();  
        Initialize(Target);  
        GC.Collect();  
        GC.WaitForPendingFinalizers();  
        Callback();  
    }  
  
    public static void Target()  
    {
    }  
  
    [DllImport("Library", CallingConvention = CallingConvention.StdCall)]  
    public static extern void Initialize(DCallback pfDelegate);  
  
    [DllImport ("Library", CallingConvention = CallingConvention.StdCall)]  
    public static extern void Callback();  
  
    ~Entry() { Console.Error.WriteLine("Entry Collected"); }  
}  
```  
  
## <a name="see-also"></a>Veja também

- <xref:System.Runtime.InteropServices.MarshalAsAttribute>
- [Diagnosticando erros com assistentes para depuração gerenciada](diagnosing-errors-with-managed-debugging-assistants.md)
- [Realizando marshaling de interoperabilidade](../interop/interop-marshaling.md)
- [gcUnmanagedToManaged](gcunmanagedtomanaged-mda.md)
