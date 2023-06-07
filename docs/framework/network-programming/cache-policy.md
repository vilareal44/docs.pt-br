---
title: Política de Cache
description: Saiba mais sobre a política de cache, as regras que determinam se uma solicitação pode ser satisfeita usando uma cópia armazenada em cache do recurso solicitado.
ms.date: 03/30/2017
helpviewer_keywords:
- time-based cache policies
- location-based cache policies
- cache [.NET Framework], policies
- request cache policies
- freshness of cached resources
- content cache policies
- expired content
ms.assetid: 1a7e04ec-7872-41c2-96c6-52566dcb412b
ms.openlocfilehash: d63d2b6bf8426968d2120647c8ecea2b7602825a
ms.sourcegitcommit: da21fc5a8cce1e028575acf31974681a1bc5aeed
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/08/2020
ms.locfileid: "84502659"
---
# <a name="cache-policy"></a>Política de Cache
Uma política de cache define as regras que são usadas para determinar se uma solicitação pode ser atendida usando uma cópia armazenada em cache do recurso solicitado. Os aplicativos especificam requisitos de cache de cliente para atualização, mas a política de cache efetiva é determinada pelos requisitos de cache de cliente, requisitos de expiração de conteúdo do servidor e requisitos de revalidação do servidor. A interação dos requisitos da política de cache de cliente e do servidor sempre resulta na política de cache mais conservadora, para ajudar a garantir que o conteúdo mais atualizado é retornado para o aplicativo cliente.  
  
 As políticas de cache são baseadas na localização ou em tempo. Uma política de cache baseada na localização define a atualização das entradas armazenadas em cache de acordo com o local em que o recurso solicitado pode ser obtido. Uma política de cache baseada em tempo define a atualização das entradas armazenadas em cache usando a hora em que o recurso foi recuperado, os cabeçalhos retornados com o recurso e a hora atual. A maioria dos aplicativos pode usar a política de cache baseada em tempo padrão, que implementa a política de cache especificada no RFC 2616, disponível no site da [IETF (Internet Engineering Task Force)](https://www.ietf.org/).  
  
 As classes descritas na tabela a seguir são usadas para especificar políticas de cache.  
  
|Nome da classe|Descrição|  
|----------------|-----------------|  
|<xref:System.Net.Cache.HttpRequestCachePolicy>|Representa as políticas de cache baseadas na localização e em tempo para recursos solicitados usando objetos <xref:System.Net.HttpWebRequest>.|  
|<xref:System.Net.Cache.RequestCachePolicy>|Representa as políticas de cache baseadas na localização ou a política de cache baseada em tempo <xref:System.Net.Cache.RequestCacheLevel.Default> para recursos solicitados usando objetos <xref:System.Net.WebRequest>.|  
|<xref:System.Net.Cache.HttpCacheAgeControl>|Especifica os valores usados para criar objetos <xref:System.Net.Cache.HttpRequestCachePolicy> baseados em tempo.|  
|<xref:System.Net.Cache.HttpRequestCacheLevel>|Especifica os valores usados para criar objetos <xref:System.Net.Cache.HttpRequestCachePolicy> baseados na localização e em tempo.|  
|<xref:System.Net.Cache.RequestCacheLevel>|Especifica os valores usados para criar objetos <xref:System.Net.Cache.RequestCachePolicy> baseados na localização e em tempo <xref:System.Net.Cache.RequestCacheLevel.Default>.|  
  
 Defina uma política de cache para todas as solicitações feitas pelo aplicativo ou para solicitações individuais. Ao especificar uma política de cache no nível do aplicativo e uma política de cache no nível da solicitação, a política no nível da solicitação é usada. Especifique uma política de cache no nível do aplicativo de forma programática ou usando os arquivos de configuração do aplicativo ou do computador. Para obter mais informações, consulte [ \<requestCaching> elemento (configurações de rede)](../configure-apps/file-schema/network/requestcaching-element-network-settings.md).  
  
 Para criar uma política de cache, você deve criar um objeto de política criando uma instância da classe <xref:System.Net.Cache.RequestCachePolicy> ou <xref:System.Net.Cache.HttpRequestCachePolicy>. Para especificar a política em uma solicitação, defina a propriedade <xref:System.Net.WebRequest.CachePolicy%2A> da solicitação com o objeto de política. Ao definir uma política no nível do aplicativo de forma programática, defina a propriedade <xref:System.Net.HttpWebRequest.DefaultCachePolicy%2A> com o objeto de política.  
  
 Para obter exemplos de código que demonstram como criar e usar políticas de cache, consulte [Configurando o cache em aplicativos de rede](configuring-caching-in-network-applications.md).  
  
## <a name="see-also"></a>Confira também

- [Gerenciamento de cache para aplicativos de rede](cache-management-for-network-applications.md)
- [Políticas de cache baseadas na localização](location-based-cache-policies.md)
- [Políticas de cache baseadas em tempo](time-based-cache-policies.md)
- [Configurando o cache em aplicativos de rede](configuring-caching-in-network-applications.md)
