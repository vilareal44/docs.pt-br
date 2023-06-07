---
title: Formatos de metadados
ms.date: 03/30/2017
ms.assetid: baad1e68-28fc-4a6a-8a43-75e47e7fa871
ms.openlocfilehash: a74a57843beaba09b969678a34cad3ad8bed7050
ms.sourcegitcommit: cdb295dd1db589ce5169ac9ff096f01fd0c2da9d
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/09/2020
ms.locfileid: "84598834"
---
# <a name="metadata-formats"></a>Formatos de metadados
Windows Communication Foundation (WCF) dá suporte aos formatos de metadados na tabela a seguir.  
  
## <a name="metadata-specifications-and-usage"></a>Especificações e uso de metadados  
  
|Protocolo|Especificação e uso|  
|--------------|-----------------------------|  
|WSDL 1,1|[WSDL (Web Services Description Language) 1,1](https://www.w3.org/TR/wsdl/)<br /><br /> O WCF usa WSDL (Web Services Description Language) para descrever os serviços.|  
|esquema XML|[Esquema XML parte 2: datadigits segunda edição](https://www.w3.org/TR/2004/REC-xmlschema-2-20041028/) e [esquema XML parte 1: estruturas segunda edição](https://www.w3.org/TR/2004/REC-xmlschema-1-20041028/)<br /><br /> O WCF usa o esquema XML para descrever os tipos de dados usados nas mensagens.|  
|Política WS|[Política de serviços Web 1,2-estrutura (WS-Policy)](https://www.w3.org/Submission/WS-Policy/)<br /><br /> [Política de serviços Web 1,5-estrutura](https://www.w3.org/TR/ws-policy/)<br /><br /> O WCF usa as especificações de WS-Policy 1,2 ou 1,5 com declarações específicas de domínio para descrever os requisitos de serviço e os recursos.|  
|Anexos de política do WS|[Política de serviços Web 1,2-anexo (WS-PolicyAttachment)](https://www.w3.org/Submission/WS-PolicyAttachment/)<br /><br /> O WCF implementa anexos de WS-Policy para anexar expressões de política em vários escopos no WSDL.|  
|Troca de metadados WS|[WS-MetadataExchange (troca de metadados de serviços Web) versão 1,1](https://specs.xmlsoap.org/ws/2004/09/mex/WS-MetadataExchange.pdf)<br /><br /> O WCF implementa o WS-MetadataExchange para recuperar o esquema XML, o WSDL e o WS-Policy.|  
|Associação de endereçamento do WS para WSDL|[Endereçamento de serviços Web 1,0-Associação de WSDL](https://www.w3.org/TR/ws-addr-wsdl/)<br /><br /> O WCF implementa a associação WS-Addressing para WSDL para anexar informações de endereçamento em WSDL.|  
  
## <a name="see-also"></a>Consulte também

- [Associações de interoperabilidade fornecidas pelo sistema oferece suporte para protocolos de serviços Web](web-services-protocols-supported-by-system-provided-interoperability-bindings.md)
- [WSDL e política](wsdl-and-policy.md)
