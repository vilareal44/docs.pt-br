---
title: Comparando serviços Web do ASP.NET ao WCF com base na finalidade e nos padrões usados
ms.date: 03/30/2017
ms.assetid: d3890278-fa9b-4902-91ea-8da73b7143cc
ms.openlocfilehash: b27f3516868ec70319ce37fbbd774a29347b0bc8
ms.sourcegitcommit: cdb295dd1db589ce5169ac9ff096f01fd0c2da9d
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/09/2020
ms.locfileid: "84597573"
---
# <a name="comparing-aspnet-web-services-to-wcf-based-on-purpose-and-standards-used"></a>Comparando serviços Web do ASP.NET ao WCF com base na finalidade e nos padrões usados
Os serviços Web do ASP.NET foram desenvolvidos para criar aplicativos que enviam e recebem mensagens usando o protocolo SOAP por HTTP. A estrutura das mensagens pode ser definida usando um esquema XML, e uma ferramenta é fornecida para facilitar a serialização das mensagens de e para os objetos .NET Framework. A tecnologia pode gerar metadados automaticamente para descrever os serviços Web no WSDL (Web Services Description Language) e uma segunda ferramenta é fornecida para gerar clientes para serviços Web a partir do WSDL.  
  
 O WCF é para permitir que os aplicativos de .NET Framework troquem mensagens com outras entidades de software. O SOAP é usado por padrão, mas as mensagens podem estar em qualquer formato e transmitidas usando qualquer protocolo de transporte. A estrutura das mensagens pode ser definida usando um esquema XML e há várias opções para serializar as mensagens de e para objetos .NET Framework. O WCF pode gerar metadados automaticamente para descrever os aplicativos criados usando a tecnologia em WSDL e também fornece uma ferramenta para gerar clientes para esses aplicativos a partir do WSDL.  
  
 Os padrões com suporte do ASP.NET Web Services são documentados em [benefícios de Web Services XML criados usando o ASP.net](https://docs.microsoft.com/previous-versions/dotnet/netframework-4.0/0859ebft(v=vs.100)). A lista mais abrangente de padrões com suporte pelo WCF é listada em [protocolos de serviços da Web com suporte de associações de interoperabilidade fornecidas pelo sistema](web-services-protocols-supported-by-system-provided-interoperability-bindings.md).  
  
## <a name="see-also"></a>Consulte também

- [Comparando os serviços Web ASP.NET com o WCF baseado em desenvolvimento](comparing-aspnet-web-services-to-wcf-based-on-development.md)
