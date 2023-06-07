---
title: Extensibilidade de metadados
ms.date: 03/30/2017
ms.assetid: f92fcc76-0806-4c84-9d63-7aae0d3899de
ms.openlocfilehash: 1e8e7f511b38cf3326b9abbca57df769317a26a4
ms.sourcegitcommit: cdb295dd1db589ce5169ac9ff096f01fd0c2da9d
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/09/2020
ms.locfileid: "84584617"
---
# <a name="metadata-extensibility"></a>Extensibilidade de metadados
Esta seção contém exemplos que demonstram metadados personalizados.  
  
## <a name="in-this-section"></a>Nesta seção  
 [Ponto de extremidade de metadados seguros personalizados](custom-secure-metadata-endpoint.md)  
 Demonstra como implementar um serviço com um ponto de extremidade de metadados seguro que usa uma das associações de troca que não são de metadados e como configurar a [ferramenta de utilitário de metadados ServiceModel (svcutil. exe)](../servicemodel-metadata-utility-tool-svcutil-exe.md) ou clientes para buscar os metadados desse ponto de extremidade de metadados.  
  
 [Publicação personalizada de WSDL](custom-wsdl-publication.md)  
 Demonstra como implementar um <xref:System.ServiceModel.Description.IWsdlExportExtension?displayProperty=nameWithType> em um atributo personalizado <xref:System.ServiceModel.Description.IContractBehavior?displayProperty=nameWithType> para exportar Propriedades de atributo como anotações WSDL, entre outras funcionalidades.
