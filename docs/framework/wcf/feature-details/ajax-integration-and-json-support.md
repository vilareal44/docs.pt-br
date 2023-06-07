---
title: Integração de AJAX e suporte para JSON
ms.date: 03/30/2017
helpviewer_keywords:
- AJAX integration and JSON support [WCF]
ms.assetid: 3851a8fc-d861-4ac1-873c-96af0343d3a7
ms.openlocfilehash: 3054c3c6faa8dfe621c706d8df031c23d497da0c
ms.sourcegitcommit: cdb295dd1db589ce5169ac9ff096f01fd0c2da9d
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/09/2020
ms.locfileid: "84576506"
---
# <a name="ajax-integration-and-json-support"></a>Integração de AJAX e suporte para JSON
O suporte do Windows Communication Foundation (WCF) para JavaScript e XML assíncronos ASP.NET (AJAX) e o formato de dados JavaScript Object Notation (JSON) permitem que os serviços WCF exponham operações a clientes AJAX. Os clientes AJAX são páginas da Web que executam código JavaScript e acessam esses serviços WCF usando solicitações HTTP. Os tópicos nesta seção fornecem informações sobre esse suporte e sobre como implementá-lo.  
  
 Para obter mais informações sobre o ASP.NET AJAX e sua integração com o ASP.NET 2,0, consulte [visão geral do ASP.NET AJAX](https://docs.microsoft.com/previous-versions/aspnet/bb398874(v=vs.100)).  
  
## <a name="in-this-section"></a>Nesta seção  
 [Criando serviços do WCF para o AJAX ASP.NET](creating-wcf-services-for-aspnet-ajax.md)  
 Descreve como um serviço WCF pode ser exposto a clientes AJAX adicionando o ponto de extremidade AJAX apropriado por meio da configuração ou usando uma fábrica de host de serviço personalizada para gerar um host de serviço que configura o ponto de extremidade AJAX automaticamente.  
  
 [Criando serviços WCF AJAX sem ASP.NET](creating-wcf-ajax-services-without-aspnet.md)  
 Descreve como criar um serviço WCF sem usar o ASP.NET.  
  
 [Suporte para JSON e outros formatos de transferência de dados](support-for-json-and-other-data-transfer-formats.md)  
 Descreve o suporte do formato JSON normalmente usado (em vez de XML) para mensagens com serviços AJAX ASP.NET.  
  
 [Como migrar serviços habilitados para AJAX ASP.NET para o WCF](how-to-migrate-ajax-enabled-aspnet-web-services-to-wcf.md)  
 Descreve como migrar um serviço Web ASP.NET habilitado para AJAX para um serviço Web WCF.  
  
## <a name="see-also"></a>Consulte também

- <xref:System.ServiceModel.Activation.WebScriptServiceHostFactory>
- [Modelo de programação WCF Web HTTP](wcf-web-http-programming-model.md)
