---
title: MDA reportAvOnComRelease
description: Examine o MDA (Assistente de depuração gerenciada) reportAvOnComRelease, que pode ser ativado devido a violações de acesso e corrupção de memória no .NET.
ms.date: 03/30/2017
helpviewer_keywords:
- MDAs (managed debugging assistants), reference counting errors
- exceptions, reference counting errors
- ReportAvOnComRelease MDA
- COM release access violations
- user reference counting errors
- managed debugging assistants (MDAs), reference counting errors
- report access violation on Com release
- reference counting errors
ms.assetid: a2b86b63-08b2-4943-b344-3c2cf46ccd31
ms.openlocfilehash: f9ba343060cb4d16de5909a5b619353546aca8ca
ms.sourcegitcommit: c23d9666ec75b91741da43ee3d91c317d68c7327
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/01/2020
ms.locfileid: "85803602"
---
# <a name="reportavoncomrelease-mda"></a>MDA reportAvOnComRelease
O MDA (Assistente para Depuração Gerenciada) do `reportAvOnComRelease` é ativado quando as exceções são lançadas por causa da contagem de erros pela referência do usuário durante a realização da interoperabilidade COM e do uso do método <xref:System.Runtime.InteropServices.Marshal.Release%2A> ou <xref:System.Runtime.InteropServices.Marshal.ReleaseComObject%2A> combinado com chamadas COM brutas.  
  
## <a name="symptoms"></a>Sintomas  
 Violações de acesso e corrupção de memória.  
  
## <a name="cause"></a>Causa  
 Às vezes, uma exceção é lançada por causa da contagem de erros pela referência do usuário durante a realização da interoperabilidade COM e do uso do método <xref:System.Runtime.InteropServices.Marshal.Release%2A> ou <xref:System.Runtime.InteropServices.Marshal.ReleaseComObject%2A> combinado com chamadas COM brutas. Normalmente, essa exceção é descartada porque deixar de fazer isso causaria uma violação de acesso no CLR, desativando-o. Quando esse assistente é habilitado, essas exceções podem ser detectadas e relatadas em vez de ser simplesmente descartadas.  
  
## <a name="resolution"></a>Resolução  
 Examine o código de contagem de referência e procure erros, também examine os clientes nativos do objeto em busca de erros de contagem de referência.  
  
## <a name="effect-on-the-runtime"></a>Efeito sobre o runtime  
 Há dois modos disponíveis. Se o atributo `allowAv` for `true`, o assistente evitará que o runtime descarte a violação de acesso. Se `allowAv` for `false` (o padrão), o runtime descartará a violação de acesso, mas uma mensagem de aviso será relatada ao usuário para indicar que uma exceção foi lançada e descartada.  
  
## <a name="output"></a>Saída  
 Se possível, a saída contém a vtable original do ponteiro da interface COM. Do contrário, é exibida uma mensagem informativa.  
  
## <a name="configuration"></a>Configuração  
  
```xml  
<mdaConfig>  
  <assistants>  
    <reportAvOnComRelease />  
  </assistants>  
</mdaConfig>  
```  
  
## <a name="see-also"></a>Consulte também

- <xref:System.Runtime.InteropServices.MarshalAsAttribute>
- [Diagnosticando erros com assistentes para depuração gerenciada](diagnosing-errors-with-managed-debugging-assistants.md)
- [Realizando marshaling de interoperabilidade](../interop/interop-marshaling.md)
