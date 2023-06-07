---
title: Eventos ETW de informações de runtime
description: Consulte eventos de ETW de informações de tempo de execução, que registram o SKU, o número de versão, como o tempo de execução foi ativado (incluindo parâmetros de linha de comando), o GUID e muito mais.
ms.date: 03/30/2017
helpviewer_keywords:
- runtime information events [.NET Framework]
- ETW, runtime information events
ms.assetid: 68b4edbc-7f3b-45f6-ab75-4fd066d6af9a
ms.openlocfilehash: 385519229bdb76841cdf592d95e96d2288ec5e1a
ms.sourcegitcommit: cf5a800a33de64d0aad6d115ffcc935f32375164
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/20/2020
ms.locfileid: "86474222"
---
# <a name="runtime-information-etw-events"></a>Eventos ETW de informações de runtime
Esses eventos ETW registram informações sobre o runtime, incluindo a SKU, o número de versão, a maneira pela qual o runtime foi ativado, os parâmetros de linha de comando com os quais ele foi iniciado, o GUID (se aplicável) e outras informações relevantes. Se vários runtimes estiverem sendo executados dentro de um processo, as informações fornecidas por esses eventos (o ClrInstanceID) ajudarão a desfazer a ambiguidade entre os runtimes.  
  
 A tabela a seguir mostra os dois eventos de informações de runtime. Os eventos podem ser gerados sob qualquer palavra-chave ou máscara. (Para obter mais informações, consulte [Palavras-chaves e níveis CLR ETW](clr-etw-keywords-and-levels.md).)  
  
|Evento|ID do evento|Provedor|Descrição|  
|-----------|--------------|--------------|-----------------|  
|`RuntimeInformationEvent`|187|CLRRuntime|Gerado quando um runtime é carregado.|  
|`RuntimeInformationDCStart`|187|CLRRundown|Enumera os runtimes que são carregados.|  
  
 A tabela a seguir mostra dados do evento.  
  
|Nome do campo|Tipo de dados|Descrição|  
|----------------|---------------|-----------------|  
|ClrInstanceID|win:UInt16|ID exclusiva da instância do CLR ou do CoreCLR.|  
|Sku|win:UInt16|1 – CLR de Área de Trabalho.<br /><br /> 2 – CoreCLR.|  
|BclVersion – versão principal|win:UInt16|Versão principal de mscorlib.dll.|  
|BclVersion – versão secundária|win:UInt16|Número de versão secundária de mscorlib.dll.|  
|BclVersion – número de build|win:UInt16|Número de build de mscorlib.dll.|  
|BclVersion – QFE|win:UInt16|Número de versão de hotfix de mscorlib.dll.|  
|VMVersion – versão principal|win:UInt16|Versão de clr.dll ou coreclr.dll, dependendo da SKU.|  
|VMVersion – versão secundária|win:UInt16|Versão secundária de clr.dll ou coreclr.dll, dependendo da SKU.|  
|VMVersion – número de build|win:UInt16|Número de build de clr.dll ou coreclr.dll.|  
|VMVersion – QFE|win:UInt16|Número de versão do hotfix de clr.dll ou coreclr.dll.|  
|StartupFlags|win:UInt32|Sinalizadores de inicialização definidos em mscoree.h.|  
|StartupMode|win:UInt8|0x01 – executável gerenciado.<br /><br /> 0x02 – CLR hospedado.<br /><br /> 0x04 – interoperabilidade gerenciada de C++.<br /><br /> 0x08 – ativado por COM.<br /><br /> 0x10 – outros.|  
|CommandLine|win:UnicodeString|Não nulo somente se StartupMode=0x01.|  
|ComObjectGUID|win:GUID|Não nulo somente se StartupMode=0x08.|  
|RuntimeDLLPath|win:UnicodeString|Caminho para o arquivo. dll do CLR que foi carregado no processo.|  
  
## <a name="see-also"></a>Consulte também

- [Eventos ETW no CLR](clr-etw-events.md)
