---
title: Habilitando o rastreamento de rede
description: Saiba como habilitar o rastreamento de rede, que fornece informações sobre invocações de método e tráfego de rede para um aplicativo gerenciado no .NET Framework.
ms.date: 03/30/2017
helpviewer_keywords:
- trace destinations
- sending traces to log file
- trace listeners, network tracing
- network tracing, enabling
- CLR Debugger
- default listeners
- logs, trace
- destination for tracing output
ms.assetid: 5fff458c-51a6-4134-ba47-8a6137ddc41e
ms.openlocfilehash: 4ad0b23fc93ddcdc11cebcc556d12148df5e8ae2
ms.sourcegitcommit: da21fc5a8cce1e028575acf31974681a1bc5aeed
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/08/2020
ms.locfileid: "84502594"
---
# <a name="enabling-network-tracing"></a>Habilitando o rastreamento de rede
O rastreamento de rede fornece acesso a informações sobre invocações de método e o tráfego de rede gerados por um aplicativo gerenciado. Você deve concluir as seguintes tarefas para habilitar o rastreamento de rede no aplicativo:  
  
- Compile o código com o rastreamento habilitado. Consulte [Como compilar condicionalmente com Trace e Debug](../debug-trace-profile/how-to-compile-conditionally-with-trace-and-debug.md) para obter mais informações sobre as opções do compilador necessárias para habilitar o rastreamento.  
  
- Especifique um destino para a saída de rastreamento.  
  
- Configure o comportamento do rastreamento de rede. Consulte [Como configurar o rastreamento de rede](how-to-configure-network-tracing.md) para obter informações detalhadas.  
  
 Os destinos de rastreamento mais comuns, também conhecidos como ouvintes de rastreamento, são o ouvinte padrão e o arquivo de log.  
  
 O rastreamento usará o ouvinte padrão se você não especificar um ouvinte de rastreamento. Exiba as mensagens enviadas para o ouvinte padrão executando o código em um depurador habilitado para código gerenciado, como o Depurador CLR fornecido com o SDK do .NET Framework ou o DBwin32.exe fornecido com o SDK do Windows. Usando o Depurador CLR, as mensagens de rastreamento são exibidas na janela **Saída**.  
  
 Se preferir usar um arquivo para receber rastreamentos, especifique um arquivo de log usando as definições de configuração, conforme mostrado no exemplo a seguir. (Para obter uma discussão geral sobre arquivos de configuração, consulte [Arquivos de configuração](../configure-apps/index.md).)  
  
 Para enviar os rastreamentos para um arquivo de log, adicione o nó a seguir ao nó `<system.diagnostics>` do arquivo de configuração apropriado (aplicativo ou computador). Altere o nome do arquivo (trace.log) de acordo com suas necessidades.  
  
```xml  
<system.diagnostics>  
  <trace autoflush="true" indentsize="4">  
    <listeners>  
      <add name="file" type="System.Diagnostics.TextWriterTraceListener" initializeData="trace.log"/>  
    </listeners>
  </trace>  
</system.diagnostics>  
```  
  
## <a name="see-also"></a>Confira também

- [Interpretando o rastreamento de rede](interpreting-network-tracing.md)
- [Rastreamento de rede no .NET Framework](network-tracing.md)
- [Rastreamento e instrumentação de aplicativos](../debug-trace-profile/tracing-and-instrumenting-applications.md)
