---
title: MDA fatalExecutionEngineError
description: Examine o MDA (Assistente de depuração gerenciada) do fatalExecutionEngineError no .NET, que pode ser ativado devido a um encerramento de processo inesperado.
ms.date: 03/30/2017
helpviewer_keywords:
- corrupted CLR
- fatal execution error
- terminated processes
- unexpected terminations
- fatal errors
- MDAs (managed debugging assistants), fatal errors
- process termination
- FatalExecutionEngineError MDA
- managed debugging assistants (MDAs), fatal errors
ms.assetid: 8b559e44-2393-4e4e-8160-7558d37a4a89
ms.openlocfilehash: 0806d2eaa1752c88bebd03304fbe5c8094416a48
ms.sourcegitcommit: a2c8b19e813a52b91facbb5d7e3c062c7188b457
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/26/2020
ms.locfileid: "85415921"
---
# <a name="fatalexecutionengineerror-mda"></a>MDA fatalExecutionEngineError
O MDA (assistente para depuração gerenciada) `fatalExecutionEngineError` é ativado quando um erro fatal no CLR (Common Language Runtime) é detectado. O processo será terminado.  
  
## <a name="symptoms"></a>Sintomas  
 Término inesperado do processo. Outros sintomas não podem ser determinados porque uma falha do CLR pode ocorrer por vários motivos.  
  
## <a name="cause"></a>Causa  
 O CLR foi fatalmente corrompido. Isso geralmente é causado por dados corrompidos, que podem ser causados por vários problemas, como chamadas a funções de invocação de plataforma malformadas e passagem de dados inválidos para o CLR.  
  
## <a name="resolution"></a>Resolução  
 A habilitação de MDAs adicionais pode ajudar a identificar o problema. Os seguintes MDAs podem ser especialmente úteis para diagnosticar o problema:  
  
- [invalidOverlappedToPinvoke](invalidoverlappedtopinvoke-mda.md)  
  
- [overlappedFreeError](overlappedfreeerror-mda.md)  
  
- [pInvokeStackImbalance](pinvokestackimbalance-mda.md)  
  
- [gcUnmanagedToManaged](gcunmanagedtomanaged-mda.md)  
  
- [gcManagedToUnmanaged](gcmanagedtounmanaged-mda.md)  
  
- [callbackOnCollectedDelegate](callbackoncollecteddelegate-mda.md)  
  
- [reportAvOnComRelease](reportavoncomrelease-mda.md)  
  
- [invalidVariant](invalidvariant-mda.md)  
  
- [invalidIUnknown](invalidiunknown-mda.md)  
  
- [raceOnRCWCleanup](raceonrcwcleanup-mda.md)  
  
- [invalidFunctionPointerInDelegate](invalidfunctionpointerindelegate-mda.md)  
  
- [invalidGCHandleCookie](invalidgchandlecookie-mda.md)  
  
## <a name="effect-on-the-runtime"></a>Efeito sobre o runtime  
 Esse MDA não tem nenhum efeito sobre o comportamento do runtime.  
  
## <a name="output"></a>Saída  
 O endereço da função CLR que causou o erro fatal, a ID do thread em que ocorreu o erro e o código de erro.  
  
## <a name="configuration"></a>Configuração  
  
```xml  
<mdaConfig>  
  <assistants>  
    <fatalExecutionEngineError />  
  </assistants>  
</mdaConfig>  
```  
  
## <a name="see-also"></a>Veja também

- <xref:System.Runtime.CompilerServices.RuntimeHelpers.PrepareMethod%2A>
- <xref:System.Runtime.ConstrainedExecution.Cer>
- [Diagnosticando erros com assistentes para depuração gerenciada](diagnosing-errors-with-managed-debugging-assistants.md)
