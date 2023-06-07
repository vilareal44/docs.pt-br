---
title: Detecção automática de proxy
description: Saiba mais sobre a detecção automática de proxy, em que o sistema identifica um servidor proxy da Web e o usa para enviar solicitações em nome do cliente.
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
helpviewer_keywords:
- automatic proxy detections
- Web Proxy Auto-Discovery
- Web proxy
- detecting proxies automatically
- WebProxy class, automatic proxy detections
- proxies, automatically detecting
- network
- WPAD (Web Proxy Auto-Discovery)
ms.assetid: fcd9c3bd-93de-4c92-8ff3-837327ad18de
ms.openlocfilehash: dbd5d7fa671ae5ec3b7dc00205f0c9d8381bb3ce
ms.sourcegitcommit: da21fc5a8cce1e028575acf31974681a1bc5aeed
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/08/2020
ms.locfileid: "84502685"
---
# <a name="automatic-proxy-detection"></a>Detecção automática de proxy
A detecção automática de proxy é um processo pelo qual um servidor proxy Web é identificado pelo sistema e usado para enviar solicitações em nome do cliente. Esse recurso também é conhecido como WPAD (Descoberta Automática de Proxy Web). Quando a detecção automática de proxy está habilitada, o sistema tenta localizar um script de configuração de proxy que é responsável por retornar o conjunto de proxies que pode ser usado para a solicitação. Se o script de configuração de proxy for encontrado, o script será baixado, compilado e executado no computador local quando as informações de proxy, o fluxo da solicitação ou a resposta for obtida de uma solicitação que usa uma instância <xref:System.Net.WebProxy>.  
  
 A detecção automática de proxy é executada pela classe <xref:System.Net.WebProxy> e pode utilizar configurações no nível da solicitação, configurações nos arquivos de configuração e as configurações especificadas usando a caixa de diálogo **Rede Local (LAN)** do Internet Explorer.  
  
> [!NOTE]
> Exiba a caixa de diálogo **Configurações da Rede Local (LAN)** do Internet Explorer selecionando **Ferramentas** no menu principal do Internet Explorer e, em seguida, selecionando **Opções da Internet**. Em seguida, selecione a guia **Conexões** e, em seguida, clique em **Configurações de LAN**.  
  
 Quando a detecção automática de proxy está habilitada, a classe <xref:System.Net.WebProxy> tenta localizar o script de configuração de proxy da seguinte maneira:  
  
1. A função `InternetQueryOption` de WinINet é usada para localizar o script de configuração de proxy detectado mais recentemente pelo Internet Explorer.  
  
2. Se o script não for localizado, a classe <xref:System.Net.WebProxy> usará o protocolo DHCP para localizar o script. O servidor DHCP pode responder com o local (nome do host) do script ou com a URL completa do script.  
  
3. Se o DHCP não identificar o host do WPAD, o DNS será consultado em busca de um host com o WPAD como seu nome ou alias.  
  
4. Se o host não for identificado e o local de um script de configuração de proxy for especificado pelas configurações de LAN do Internet Explorer ou por um arquivo de configuração, esse local será usado.  
  
> [!NOTE]
> Os aplicativos em execução como um Serviço NT ou como parte do ASP.NET usam as configurações do servidor proxy do Internet Explorer (se disponíveis) do usuário que faz a invocação. Essas configurações podem não estar disponíveis para todos os aplicativos de serviço.  
  
 Os proxies são configurados por conectoide. Uma conectoide é um item da caixa de diálogo de conexão de rede e pode ser um dispositivo de rede físico (um modem ou uma placa Ethernet) ou uma interface virtual (como uma conexão VPN em execução em um dispositivo de rede). Quando um conectoide é alterado (por exemplo, uma conexão sem fio altera um ponto de acesso ou uma VPN é habilitada), o algoritmo de detecção de proxy é executado novamente.  
  
 Por padrão, as configurações de proxy do Internet Explorer são usadas para detectar o proxy. Se seu aplicativo estiver sendo executado sob uma conta não interativa (sem uma maneira conveniente de definir as configurações de proxy do IE) ou se desejar usar configurações de proxy diferentes das configurações do IE, você poderá configurar seu proxy criando um arquivo de configuração com o [ \<defaultProxy> elemento (configurações de rede)](../configure-apps/file-schema/network/defaultproxy-element-network-settings.md) e elementos [ \<proxy> de elemento (configurações de rede)](../configure-apps/file-schema/network/proxy-element-network-settings.md) definidos.  
  
 Para as solicitações criadas por você, desabilite a detecção automática de proxy no nível da solicitação usando um <xref:System.Net.WebRequest.Proxy%2A> nulo com a solicitação, conforme mostrado no exemplo de código a seguir.  
  
```csharp  
public static void DisableForMyRequest (Uri resource)  
{  
    WebRequest request = WebRequest.Create (resource);  
    request.Proxy = null;  
    WebResponse response = request.GetResponse ();  
}  
```  
  
```vb  
Public Shared Sub DisableForMyRequest(ByVal resource As Uri)  
    Dim request As WebRequest = WebRequest.Create(resource)  
    request.Proxy = Nothing  
    Dim response As WebResponse = request.GetResponse()  
    End Sub
```  
  
 As solicitações que não têm um proxy usam o proxy padrão do domínio do aplicativo, que está disponível na propriedade <xref:System.Net.WebRequest.DefaultWebProxy%2A>.  
  
## <a name="see-also"></a>Confira também

- <xref:System.Net.WebProxy>
- <xref:System.Net.WebRequest>
- [\<system.Net>Elemento (configurações de rede)](../configure-apps/file-schema/network/system-net-element-network-settings.md)
