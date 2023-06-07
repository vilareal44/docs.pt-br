---
title: Suporte a ligações de site do IIS
description: Saiba como fornecer vários endereços base que usam o mesmo protocolo no mesmo site ao hospedar um serviço WCF no IIS.
ms.date: 03/30/2017
ms.assetid: 40440495-254d-45c8-a8c6-b29f364892ba
ms.openlocfilehash: 290dca03dbed7d0a7442a3903b735eb189929ed1
ms.sourcegitcommit: 358a28048f36a8dca39a9fe6e6ac1f1913acadd5
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/23/2020
ms.locfileid: "85244862"
---
# <a name="supporting-multiple-iis-site-bindings"></a>Suporte a ligações de site do IIS
Ao hospedar um serviço de Windows Communication Foundation (WCF) em Serviços de Informações da Internet (IIS) 7,0, talvez você queira fornecer vários endereços base que usam o mesmo protocolo no mesmo site. Isso permite que o mesmo serviço responda a vários URIs diferentes. Isso é útil quando você deseja hospedar um serviço que escuta em `http://www.contoso.com` e `http://contoso.com` . Também é útil criar um serviço que tenha um endereço base para usuários internos e um endereço base separado para usuários externos. Por exemplo: `http://internal.contoso.com` e `http://www.contoso.com`.  
  
> [!NOTE]
> Essa funcionalidade só está disponível usando o protocolo HTTP.  
  
## <a name="multiple-base-addresses"></a>Vários endereços base  
 Esse recurso só está disponível para serviços WCF hospedados no IIS. Esse recurso não está habilitado por padrão. Para habilitá-lo, você deve adicionar o `multipleSiteBindingsEnabled` atributo ao `serviceHostingEnvironment` elemento <> no arquivo de Web.config e defini-lo como `true` , conforme mostrado no exemplo a seguir.  
  
```xml  
<serviceHostingEnvironment multipleSiteBindingsEnabled="true"/>  
```  
  
 Ao hospedar um serviço WCF no IIS, o IIS cria um endereço base para você com base no URI para o diretório virtual que contém o aplicativo. Você pode adicionar outros endereços base que usam o mesmo protocolo usando o Serviços de Informações da Internet Manager para adicionar uma ou mais associações ao seu site. Para cada associação, especifique um protocolo (HTTP ou HTTPS), um endereço IP, uma porta e um nome de host. Para obter mais informações sobre como usar o Serviços de Informações da Internet Manager, consulte [Gerenciador do IIS (IIS 7)](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/cc753842(v=ws.10)). Para obter mais informações sobre como adicionar associações a um site, consulte [criar um site da Web (IIS 7)](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/cc772350(v=ws.10))  
  
 A especificação de vários endereços base para o mesmo site afeta o conteúdo da página de ajuda do WCF, a importação do esquema e as informações de WSDL/MEX geradas pelo serviço. A página de ajuda do WCF exibe a linha de comando a ser usada para gerar um cliente WCF que pode se comunicar com o serviço. Essa linha de comando contém apenas o primeiro endereço especificado na associação IIS para o site. Da mesma forma, ao importar o esquema, apenas o primeiro endereço base especificado na associação do IIS é usado. Os dados WSDL e MEX contêm todos os endereços base especificados nas associações do IIS.  
  
> [!WARNING]
> Isso significa que, se um serviço tiver dois endereços base, um para usuários internos e outro para usuários externos, ambos serão especificados nas informações de WSDL/MEX geradas pelo serviço.
