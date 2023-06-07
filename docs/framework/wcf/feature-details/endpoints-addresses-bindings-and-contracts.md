---
title: 'Pontos de extremidade: endereços, associações e contratos'
description: Saiba como toda a comunicação com um serviço WCF ocorre por meio dos pontos de extremidade de serviço, que fornecem aos clientes acesso à funcionalidade oferecida pelo serviço.
ms.date: 03/30/2017
helpviewer_keywords:
- endpoints [WCF]
- Windows Communication Foundation [WCF], endpoints
- WCF [WCF], endpoints
ms.assetid: 9ddc46ee-1883-4291-9926-28848c57e858
ms.openlocfilehash: ce0874bfed716716b6fd1801b35a4266095cd752
ms.sourcegitcommit: 358a28048f36a8dca39a9fe6e6ac1f1913acadd5
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/23/2020
ms.locfileid: "85247306"
---
# <a name="endpoints-addresses-bindings-and-contracts"></a>Pontos de extremidade: endereços, associações e contratos

Toda a comunicação com um serviço Windows Communication Foundation (WCF) ocorre por meio dos *pontos de extremidade* do serviço. Os pontos de extremidade fornecem aos clientes acesso à funcionalidade oferecida por um serviço WCF.

Cada ponto de extremidade consiste em quatro propriedades:

- Um endereço que indica onde o ponto de extremidade pode ser encontrado.

- Uma associação que especifica como um cliente pode se comunicar com o ponto de extremidade.

- Um contrato que identifica as operações disponíveis.

- Um conjunto de comportamentos que especificam detalhes de implementação local do ponto de extremidade.

Este tópico aborda essa estrutura de ponto de extremidade e explica como ela é representada no modelo de objeto do WCF.

## <a name="the-structure-of-an-endpoint"></a>A estrutura de um ponto de extremidade

Cada ponto de extremidade consiste no seguinte:

- Endereço: o endereço identifica exclusivamente o ponto de extremidade e informa aos possíveis consumidores do serviço onde ele está localizado. Ele é representado no modelo de objeto do WCF pela <xref:System.ServiceModel.EndpointAddress> classe. Uma <xref:System.ServiceModel.EndpointAddress> classe contém:

  - Uma <xref:System.ServiceModel.EndpointAddress.Uri%2A> propriedade, que representa o endereço do serviço.

  - Uma <xref:System.ServiceModel.EndpointAddress.Identity%2A> propriedade, que representa a identidade de segurança do serviço e uma coleção de cabeçalhos de mensagens opcionais. Os cabeçalhos de mensagem opcionais são usados para fornecer informações de endereçamento adicionais e mais detalhadas para identificar ou interagir com o ponto de extremidade.

  Para obter mais informações, consulte [especificando um endereço de ponto de extremidade](../specifying-an-endpoint-address.md).

- Binding: a Binding especifica como se comunicar com o ponto de extremidade. Isso inclui:

  - O protocolo de transporte a ser usado (por exemplo, TCP ou HTTP).

  - A codificação a ser usada para as mensagens (por exemplo, texto ou binário).

  - Os requisitos de segurança necessários (por exemplo, segurança de mensagem SSL ou SOAP).

  Para obter mais informações, consulte [visão geral das associações do WCF](../bindings-overview.md). Uma associação é representada no modelo de objeto do WCF pela classe base abstrata <xref:System.ServiceModel.Channels.Binding> . Para a maioria dos cenários, os usuários podem usar uma das associações fornecidas pelo sistema. Para obter mais informações, consulte [associações fornecidas pelo sistema](../system-provided-bindings.md).

- Contratos: o contrato descreve qual funcionalidade o ponto de extremidade expõe para o cliente. Um contrato especifica:

  - Quais operações podem ser chamadas por um cliente.

  - A forma da mensagem.

  - O tipo de parâmetros de entrada ou dados necessários para chamar a operação.

  - Que tipo de mensagem de processamento ou resposta o cliente pode esperar.

  Para obter mais informações sobre como definir um contrato, consulte [projetando contratos de serviço](../designing-service-contracts.md).

- Comportamentos: você pode usar comportamentos de ponto de extremidade para personalizar o comportamento local do ponto de extremidade de serviço. Os comportamentos de ponto de extremidade obtêm isso participando do processo de criação de um tempo de execução do WCF. Um exemplo de um comportamento de ponto de extremidade é a <xref:System.ServiceModel.Description.ServiceEndpoint.ListenUri%2A> propriedade, que permite especificar um endereço de escuta diferente do SOAP ou do endereço WSDL (Web Services Description Language). Para obter mais informações, consulte [ClientViaBehavior](../diagnostics/wmi/clientviabehavior.md).

## <a name="defining-endpoints"></a>Definindo pontos de extremidade

Você pode especificar o ponto de extremidade para um serviço de forma imperativa usando código ou declarativamente por meio da configuração. Para obter mais informações, consulte [como: criar um ponto de extremidade de serviço em configuração](how-to-create-a-service-endpoint-in-configuration.md) e [como criar um ponto de extremidade de serviço no código](how-to-create-a-service-endpoint-in-code.md).

## <a name="in-this-section"></a>Nesta seção

Esta seção explica a finalidade de associações, pontos de extremidade e endereços; mostra como configurar uma associação e um ponto de extremidade; e demonstra como usar o `ClientVia` comportamento e a `ListenUri` propriedade.

[Atende](endpoint-addresses.md)\
Descreve como os pontos de extremidade são endereçados no WCF.

[Associações](bindings.md)\
Descreve como as associações são usadas para especificar os detalhes de transporte, codificação e protocolo necessários para que os clientes e serviços se comuniquem entre si.

[Contratos](contracts.md)\
Descreve como os contratos definem os métodos de um serviço.

[Como criar um ponto de extremidade de serviço na configuração](how-to-create-a-service-endpoint-in-configuration.md)\
Descreve como criar um ponto de extremidade de serviço na configuração.

[Como criar um ponto de extremidade de serviço no código](how-to-create-a-service-endpoint-in-code.md)\
Descreve como criar um ponto de extremidade de serviço no código.

[Como: usar Svcutil.exe para validar o código de serviço compilado](how-to-use-svcutil-exe-to-validate-compiled-service-code.md)\
Descreve como detectar erros em implementações de serviço e configurações sem hospedar o serviço usando a [ferramenta de utilitário de metadados ServiceModel (Svcutil.exe)](../servicemodel-metadata-utility-tool-svcutil-exe.md).

## <a name="see-also"></a>Veja também

- [Configurando serviços](../configuring-services.md)
- [Estendendo associações](../extending/extending-bindings.md)
