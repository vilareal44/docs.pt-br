---
title: Configuração de proxy
description: Saiba mais sobre como configurar servidores proxy adaptáveis e estáticos. A configuração de proxy controla como um servidor proxy trata as solicitações do cliente para recursos.
ms.date: 06/18/2018
helpviewer_keywords:
- Networking
- adaptive proxies
- static proxies
- Network Resources
- Internet, proxy configuration
- proxies
- network, proxy configuration
- proxies, configuring
ms.assetid: 353c0a8b-4cee-44f6-8e65-60e286743df9
ms.openlocfilehash: 4d62f5736e9aa469be49d101e85851bc01b7c159
ms.sourcegitcommit: 6219b1e1feccb16d88656444210fed3297f5611e
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/22/2020
ms.locfileid: "85141599"
---
# <a name="proxy-configuration"></a>Configuração de proxy
Um servidor proxy manipula as solicitações de clientes para recursos. Um proxy pode retornar um recurso solicitado do seu cache ou encaminhar a solicitação para o servidor na qual o recurso reside. Proxies podem melhorar o desempenho da rede, reduzindo o número de solicitações enviadas a servidores remotos. Proxies também podem ser usados para restringir o acesso aos recursos.  
  
## <a name="adaptive-proxies"></a>Proxies adaptáveis  
 No .NET Framework, os proxies existem em duas variedades: adaptáveis e estáticos. Proxies adaptáveis ajustam suas configurações quando as configurações de rede são alteradas. Por exemplo, se um usuário de laptop inicia uma conexão de rede dial-up, um proxy adaptável reconheceria essa alteração, descobrir e executaria seu novo script de configuração e ajustaria suas configurações adequadamente.  
  
 Proxies adaptáveis são configurados por um script de configuração (consulte [Detecção automática de proxy](automatic-proxy-detection.md)). O script gera um conjunto de protocolos de aplicativo e um proxy para cada protocolo.  
  
 Alterações no ambiente de rede podem exigir que o sistema use um novo conjunto de proxies. Se uma conexão de rede ficar inoperante ou uma nova conexão de rede for inicializada, o sistema deve descobrir a origem correta do script de configuração no novo ambiente e executar o novo script.  
  
 Você pode usar o `usesystemdefault` atributo do [`<proxy>`](../configure-apps/file-schema/network/proxy-element-network-settings.md) elemento em seu arquivo de configuração. O atributo `usesystemdefault` controla se as configurações de proxy estático (endereço de proxy, lista de ignoráveis e ignorar no local) devem ser lidas das configurações de proxy do Internet Explorer para o usuário. Se esse valor for definido como `true`, as configurações de proxy estático do Internet Explorer serão usadas. Se esse valor for `false` ou não estiver definido, as configurações de proxy estático poderão ser especificadas na configuração e substituirão as configurações de proxy do Internet Explorer. Esse valor também deve ser definido como `false` ou não estar definido para que proxies adaptáveis sejam habilitados.  
  
 O exemplo a seguir mostra uma configuração de proxy adaptável típica.  
  
```xml  
<system.net>  
    <defaultProxy>  
      <proxy usesystemdefault="false" />
    </defaultProxy>  
</system.net>  
```  
  
## <a name="static-proxies"></a>Proxies estáticos  
 Proxies estáticos geralmente são configurados explicitamente por um aplicativo ou quando um arquivo de configuração é invocado por um aplicativo ou pelo sistema. Proxies estáticos são úteis em redes em que a topologia é alterada com frequência, como um computador desktop conectado a uma rede corporativa.  
  
 Várias opções de controlam como um proxy estático funciona. Você pode especificar o seguinte:  
  
- O endereço do proxy.  
  
- Se o proxy deve ou não ser ignorado para endereços locais.  
  
- Se o proxy deve ou não ser ignorado para um conjunto de endereços.  
  
 A tabela a seguir mostra as opções de configuração para um proxy estático.  
  
|Definição do arquivo de configuração, propriedade ou atributo|Description|  
|--------------------------------------------------------|-----------------|  
|`proxyaddress` ou <xref:System.Net.WebProxy.Address>|O endereço do proxy a ser usado.|  
|`bypassonlocal` ou <xref:System.Net.WebProxy.BypassProxyOnLocal>|Controla se o proxy é ignorado para endereços locais.|  
|`bypasslist` ou <xref:System.Net.WebProxy.BypassArrayList>|Descreve, com expressões regulares, um conjunto de endereços que ignora o proxy.|  
|`usesystemdefault`|Controla se as configurações de proxy estático (endereço de proxy, lista de ignoráveis e ignorar no local) devem ser lido das configurações de proxy do Internet Explorer para o usuário. Se esse valor for definido como `true`, então as configurações de proxy estático do Internet Explorer serão usadas. No .NET Framework 2.0, quando esse valor é definido como `true`, as configurações de proxy do Internet Explorer não são substituídas por outras configurações de proxy no arquivo de configuração. No .NET Framework 1.1, as configurações de proxy do Internet Explorer podem ser substituídas por outras configurações de proxy no arquivo de configuração.<br /><br /> Se esse valor for `false` ou não estiver definido, então as configurações de proxy estático poderão ser especificadas na configuração e substituirão as configurações de proxy do Internet Explorer. Esse valor também deve ser definido como `false` ou não estar definido para que proxies adaptáveis sejam habilitados.|  
  
 O exemplo a seguir mostra uma configuração de proxy estático típica.  
  
```xml  
<system.net>  
    <defaultProxy>  
        <proxy  proxyaddress="http://proxy.contoso.com:3128"  
                bypassonlocal="True"  
        />  
        <bypasslist>  
            <add address="[a-z]+.blueyonderairlines.com$" />  
        </bypasslist>  
    </defaultProxy>  
</system.net>  
```  
  
## <a name="see-also"></a>Veja também

- <xref:System.Net.WebProxy>
- <xref:System.Net.GlobalProxySelection>
- [Detecção automática de proxy](automatic-proxy-detection.md)
