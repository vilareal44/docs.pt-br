---
title: Interpretando o rastreamento de rede
description: Descubra como usar o rastreamento para capturar chamadas que seu aplicativo faz para vários membros da classe System.Net no .NET Framework.
ms.date: 03/30/2017
helpviewer_keywords:
- TraceMode attribute
- hexidecimal data, network tracing output
- network tracing, analyzing
- protocolonly
- text, network tracing output
- includehex
ms.assetid: ad22b4b8-00af-4778-9cca-cb609ce1f8ff
ms.openlocfilehash: 7a17e4ba14d8c5fe136667c4eb5bc5b2fd7a8242
ms.sourcegitcommit: da21fc5a8cce1e028575acf31974681a1bc5aeed
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/08/2020
ms.locfileid: "84502360"
---
# <a name="interpreting-network-tracing"></a>Interpretando o rastreamento de rede
Quando o rastreamento de rede está habilitado, você pode usar o rastreamento para capturar as chamadas que seu aplicativo faz para membros de classe <xref:System.Net> diversos. A saída dessas chamadas pode ser semelhante aos exemplos a seguir.  
  
```output
[588]   (4357)   Entering Socket#33574638::Send()  
[588]   (4387)   Exiting Socket#33574638::Send()-> 61#61
```  
  
 No exemplo anterior, [588] é o identificador exclusivo do thread atual. (4357) e (4387) são os carimbos de data/hora que indicam o número de milissegundos decorridos desde que o aplicativo foi iniciado. Os dados após o carimbo de data/hora mostram o aplicativo entrando e saindo do método **Socket.Send**. O objeto executando o método **Send** tem 33574638 como seu identificador exclusivo. O rastreamento de saída do método inclui o valor retornado (que é 61 no exemplo anterior).  
  
 Rastreamentos de rede podem capturar o tráfego de rede que é enviado e recebido pelo seu aplicativo usando os protocolos de nível de aplicativo como o protocolo HTTP. Esses dados podem ser capturados como texto e, opcionalmente, como dados hexadecimais. Dados hexadecimais estão disponíveis quando você especifica **includehex** como o valor do atributo **tracemode**. (Para obter informações detalhadas sobre esse atributo, consulte [como: configurar o rastreamento de rede](how-to-configure-network-tracing.md).) O rastreamento de exemplo a seguir foi gerado usando **includehex**.  
  
 `[1692]   (1142)   00000000 : 47 45 54 20 2F 77 70 61-64 2E 64 61 74 20 48 54 : GET /wpad.dat HT`  
  
 `[1692]   (1142)   00000010 : 54 50 2F 31 2E 31 0D 0A-48 6F 73 74 3A 20 69 74 : TP/1.1..Host: it`  
  
 `[1692]   (1142)   00000020 : 67 70 72 6F 78 79 0D 0A-43 6F 6E 6E 65 63 74 69 : gproxy..Connecti`  
  
 `[1692]   (1142)   00000030 : 6F 6E 3A 20 43 6C 6F 73-65 0D 0A 0D 0A     : on: Close....`  
  
 Para omitir dados hexadecimais, especifique **protocolonly** como o valor para o atributo **tracemode**. O exemplo a seguir mostra o rastreamento quando **protocolonly** é especificado.  
  
 `[2444]   (594)   Data from ConnectStream#33574638::WriteHeaders<<GET /wpad.dat HTTP/1.1`  
  
 `Host: itgproxy`  
  
 `Connection: Close`  
  
## <a name="see-also"></a>Confira também

- [Habilitando o rastreamento de rede](enabling-network-tracing.md)
- [Como configurar o rastreamento de rede](how-to-configure-network-tracing.md)
- [Rastreamento de rede no .NET Framework](network-tracing.md)
