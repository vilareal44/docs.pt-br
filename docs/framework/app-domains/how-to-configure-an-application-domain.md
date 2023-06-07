---
title: 'Como: Configurar um domínio do aplicativo'
description: Configure um domínio de aplicativo no .NET. Você pode fornecer ao CLR informações de configuração para um novo domínio de aplicativo usando a classe AppDomainSetup.
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
- cpp
helpviewer_keywords:
- application domains, configuring
- ApplicationBase property
ms.assetid: 07ea8438-7a34-49f0-a7e8-3d6ff7e4a482
ms.openlocfilehash: 27afcf161bec74143fafb5dceb20597de73e23d4
ms.sourcegitcommit: 1c37a894c923bea021a3cc38ce7cba946357bbe1
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/19/2020
ms.locfileid: "85104862"
---
# <a name="how-to-configure-an-application-domain"></a>Como: Configurar um domínio do aplicativo
Você pode fornecer o Common Language Runtime com informações de configuração para um novo domínio de aplicativo usando a classe <xref:System.AppDomainSetup>. Ao criar seus próprios domínios de aplicativo, a propriedade mais importante é <xref:System.AppDomainSetup.ApplicationBase%2A>. As outras propriedades **AppDomainSetup** são usadas principalmente por hosts de runtime para configurar um domínio de aplicativo específico.  
  
 A propriedade **ApplicationBase** define o diretório raiz do aplicativo. Quando o runtime precisar atender a uma solicitação de tipo, ele investigará em busca do assembly que contém o tipo no diretório especificado pela propriedade **ApplicationBase**.  
  
> [!NOTE]
> Um novo domínio de aplicativo herdará apenas a propriedade **ApplicationBase** do criador.  
  
 O exemplo a seguir cria uma instância da classe **AppDomainSetup**, usa essa classe para criar um novo domínio de aplicativo, escreve as informações no console e então descarrega o domínio de aplicativo.  
  
## <a name="example"></a>Exemplo  
 [!code-cpp[ADApplicationBase#2](../../../samples/snippets/cpp/VS_Snippets_CLR/ADApplicationBase/CPP/source2.cpp#2)]
 [!code-csharp[ADApplicationBase#2](../../../samples/snippets/csharp/VS_Snippets_CLR/ADApplicationBase/CS/source2.cs#2)]
 [!code-vb[ADApplicationBase#2](../../../samples/snippets/visualbasic/VS_Snippets_CLR/ADApplicationBase/VB/source2.vb#2)]  
  
## <a name="see-also"></a>Confira também

- [Programação com domínios do aplicativo](application-domains.md#programming-with-application-domains)
- [Usando domínios do aplicativo](use.md)
