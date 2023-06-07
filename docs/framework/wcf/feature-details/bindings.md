---
title: Associações do Windows Communication Foundation
ms.date: 03/30/2017
helpviewer_keywords:
- WCF [WCF], bindings
- Windows Communication Foundation [WCF], bindings
- bindings [WCF]
ms.assetid: 83639133-89f7-43f0-b4ef-8d9e57c08d25
ms.openlocfilehash: fd6b942dcabad07feda81af63bde23d3b663ce0f
ms.sourcegitcommit: cdb295dd1db589ce5169ac9ff096f01fd0c2da9d
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/09/2020
ms.locfileid: "84587606"
---
# <a name="windows-communication-foundation-bindings"></a>Associações do Windows Communication Foundation
O Windows Communication Foundation (WCF) separa como o software de um aplicativo é escrito de como ele se comunica com outros softwares. As associações são usadas para especificar os detalhes de transporte, codificação e protocolo necessários para que os clientes e serviços se comuniquem entre si. O WCF usa associações para gerar a representação de fio subjacente do ponto de extremidade, portanto, a maioria dos detalhes de ligação deve ser acordada pelas partes que estão se comunicando. A maneira mais fácil de conseguir isso é que os clientes de um serviço usem a mesma ligação que o ponto de extremidade para o serviço usa. Para obter mais informações sobre como fazer isso, consulte [usando associações para configurar serviços e clientes](../using-bindings-to-configure-services-and-clients.md).  
  
 Uma associação é composta de uma coleção de elementos de associação. Cada elemento descreve algum aspecto de como o ponto de extremidade se comunica com os clientes. Uma associação deve incluir pelo menos um elemento de associação de transporte, pelo menos um elemento de associação de codificação de mensagem (que o elemento de associação de transporte pode fornecer por padrão) e qualquer número de outros elementos de ligação de protocolo. O processo que cria um tempo de execução fora dessa descrição permite que cada elemento de ligação contribua com código para esse tempo de execução.  
  
 O WCF fornece associações que contêm seleções comuns de elementos de associação. Eles podem ser usados com suas configurações padrão ou você pode modificar esses valores padrão de acordo com os requisitos do usuário. Essas associações fornecidas pelo sistema têm propriedades que permitem o controle direto sobre os elementos de ligação e suas configurações. Você também pode facilmente trabalhar lado a lado com várias versões de uma associação, fornecendo a cada versão da Associação seu próprio nome. Para obter detalhes, consulte [Configurando associações fornecidas pelo sistema](configuring-system-provided-bindings.md).  
  
 Se você precisar de uma coleção de elementos de associação não fornecidos por uma dessas associações fornecidas pelo sistema, poderá criar uma associação personalizada que consiste na coleção de elementos de associação necessária. Essas associações personalizadas são fáceis de criar e não exigem uma nova classe, mas não fornecem propriedades para controlar os elementos de associação ou suas configurações. Você pode acessar os elementos de associação e modificar suas configurações por meio da coleção que os contém. Para obter detalhes, consulte [associações personalizadas](../extending/custom-bindings.md).  
  
## <a name="in-this-section"></a>Nesta seção  
 [Configurando associações fornecidas pelo sistema](configuring-system-provided-bindings.md)  
 Descreve como usar e modificar as associações que o WCF fornece para dar suporte a cenários comuns.  
  
 [Usando associações para configurar serviços e clientes](../using-bindings-to-configure-services-and-clients.md)  
 Descreve como definir associações de Windows Communication Foundation (WCF) para serviços e clientes de forma imperativa no código e usando a configuração declarativamente.  
  
 [Associações personalizadas](../extending/custom-bindings.md)  
 Descreve o que <xref:System.ServiceModel.Channels.CustomBinding> é um e quando ele é usado.  
  
## <a name="reference"></a>Referência  
 <xref:System.ServiceModel.Channels.Binding>  
  
 <xref:System.ServiceModel.Channels.BindingElement>  
  
 <xref:System.ServiceModel.Channels.CustomBinding>  
  
## <a name="related-sections"></a>Seções relacionadas  
 [Estendendo associações](../extending/extending-bindings.md)
