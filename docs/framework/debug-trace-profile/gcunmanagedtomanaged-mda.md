---
title: MDA gcUnmanagedToManaged
description: Examine o assistente de depuração gerenciada do gcManagedToUnmanaged no .NET. Este MDA pode ser ativado devido à corrupção do heap de lixo durante a transição para o código gerenciado.
ms.date: 03/30/2017
helpviewer_keywords:
- MDAs (managed debugging assistants), garbage collection
- GC unmanaged to managed
- transitioning threads unmanaged to managed code
- GcUnmanagedToManaged MDA
- threading [.NET Framework], garbage collection
- managed debugging assistants (MDAs), garbage collection
- threading [.NET Framework], managed debugging assistants
- garbage collection, run-time errors
- unmanaged to managed garbage collection
ms.assetid: 103eb3a3-1cf0-4406-8a9a-a7798fdc22d1
ms.openlocfilehash: 320d55224e6a204d330447d6c68eabe0fa6cf892
ms.sourcegitcommit: a2c8b19e813a52b91facbb5d7e3c062c7188b457
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/26/2020
ms.locfileid: "85415895"
---
# <a name="gcunmanagedtomanaged-mda"></a>MDA gcUnmanagedToManaged
O MDA (assistente para depuração gerenciada) `gcUnmanagedToManaged` causa uma coleta de lixo sempre que um thread faz a transição de código não gerenciado para código gerenciado.  
  
## <a name="symptoms"></a>Sintomas  
 Um aplicativo que executa componentes de usuário não gerenciados usando o COM e a invocação de plataforma está causando uma violação de acesso não determinístico no CLR.  
  
## <a name="cause"></a>Causa  
 Se um aplicativo estiver executando componentes de usuário não gerenciados, esses componentes poderão ter corrompido o heap coletado como lixo. Isso causa uma violação de acesso no CLR quando o coletor de lixo tenta percorrer o gráfico do objeto.  
  
## <a name="resolution"></a>Resolução  
 A habilitação desse assistente reduz o tempo entre o período em que o componente não gerenciado corrompe o heap coletado como lixo e o período em que ocorre a violação de acesso, forçando uma coleta de lixo antes de cada transição gerenciada.  
  
## <a name="effect-on-the-runtime"></a>Efeito sobre o runtime  
 Causa uma coleta de lixo sempre que um thread faz a transição de código não gerenciado para código gerenciado.  
  
## <a name="output"></a>Saída  
 Esse MDA não produz nenhuma saída.  
  
## <a name="configuration"></a>Configuração  
  
```xml  
<mdaConfig>  
  <assistants>  
    <gcUnmanagedToManaged/>  
  </assistants>  
</mdaConfig>  
```  
  
## <a name="see-also"></a>Veja também

- <xref:System.Runtime.InteropServices.MarshalAsAttribute>
- [Diagnosticando erros com assistentes para depuração gerenciada](diagnosing-errors-with-managed-debugging-assistants.md)
- [gcManagedToUnmanaged](gcmanagedtounmanaged-mda.md)
- [Realizando marshaling de interoperabilidade](../interop/interop-marshaling.md)
