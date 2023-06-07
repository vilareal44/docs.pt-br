---
title: Integração com visão geral de aplicativos COM
ms.date: 03/30/2017
helpviewer_keywords:
- COM [WCF], integration overview
ms.assetid: 02c5697f-6e2e-47d6-b715-f3a28aebfbd5
ms.openlocfilehash: c283e7cbc4cb4b8bc37dd1313480410df93a93bf
ms.sourcegitcommit: cdb295dd1db589ce5169ac9ff096f01fd0c2da9d
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/09/2020
ms.locfileid: "84596819"
---
# <a name="integrating-with-com-applications-overview"></a>Integração com visão geral de aplicativos COM

O Windows Communication Foundation (WCF) fornece ao desenvolvedor de código gerenciado um ambiente avançado para a criação de aplicativos conectados. No entanto, se você tiver um investimento substancial em código não gerenciado com base em COM e não quiser migrar, você ainda poderá integrar os serviços Web WCF diretamente em seu código existente usando o moniker do serviço WCF. O moniker do serviço pode ser usado em uma ampla variedade de ambientes de desenvolvimento baseados em COM, como o Office VBA, Visual Basic 6,0 ou Visual C++ 6,0.

> [!NOTE]
> O moniker do serviço usa um canal de comunicação do WCF para toda a comunicação. Os mecanismos de segurança e identidade para esse canal diferem daqueles usados em proxies padrão COM e DCOM. Além disso, como o moniker do serviço usa um canal de comunicação do WCF, o período de tempo limite padrão é de um minuto para todas as chamadas.

O moniker do serviço é usado com a `GetObject` função para fornecer ao desenvolvedor não gerenciado uma abordagem com rigidez de tipos e específico para chamar serviços Web WCF. Isso requer uma definição local e visível do contrato do serviço Web WCF e a associação a ser usada. Assim como outros clientes WCF, o moniker do serviço deve construir um canal digitado para o serviço, embora essa construção de canal ocorra de forma transparente para o programador COM na primeira chamada de método.

Em comum com outros clientes WCF, ao usar o moniker, os aplicativos especificam o endereço, a associação e o contrato para se comunicar com um serviço. O contrato pode ser especificado de uma das seguintes maneiras:

- Contrato tipado – o contrato é registrado como um tipo visível COM no computador cliente.

- Contrato WSDL – o contrato é fornecido na forma de um documento WSDL.

- Contrato MEX – o contrato é recuperado em tempo de execução de um ponto de extremidade de intercâmbio de metadados (MEX).

## <a name="parameters-supported-by-the-service-moniker"></a>Parâmetros com suporte no moniker do serviço

A tabela a seguir mostra os parâmetros que são suportados pelo moniker do serviço.

|Parâmetro|Descrição|
|---------------|-----------------|
|`address`|Local da URL do serviço.|
|`binding`|Nome da seção de associação da configuração do aplicativo.|
|`bindingConfiguration`|Instância de associação nomeada de dentro da seção de associação nomeada.|
|`contract`|Identificador de interface (IID) que representa o contrato de serviço ou o nome do contrato (de MEX).|
|`wsdl`|Documento WSDL que fornece uma forma alternativa de definição de contrato.|
|`spnIdentity`|A identidade do SPN (nome principal do servidor) a ser usada para se comunicar com o serviço.|
|`upnIdentity`|Identidade de UPN (nome principal do usuário) a ser usada para se comunicar com o serviço.|
|`dnsIdentity`|Identidade DNS a ser usada para se comunicar com o serviço.|
|`mexAddress`|Local da URL do ponto de extremidade de intercâmbio de metadados (MEX) do serviço.|
|`mexBinding`|Nome da seção de associação da configuração do aplicativo para se conectar ao ponto de extremidade MEX.|
|`mexBindingConfiguration`|Instância de associação nomeada de dentro da seção de associação nomeada para se conectar ao ponto de extremidade MEX.|
|`bindingNamespace`|Namespace do nome da seção de associação do MEX recuperado.|
|`contractNamespace`|Namespace do contrato do MEX recuperado.|
|`mexSpnIdentity`|A identidade do SPN (nome principal do servidor) a ser usada para se comunicar com o ponto de extremidade MEX.|
|`mexUpnIdentity`|Identidade de UPN (nome principal do usuário) a ser usada para se comunicar com o ponto de extremidade MEX.|
|`mexDnsIdentity`|Identidade DNS a ser usada para se comunicar com o ponto de extremidade MEX.|
|`serializer`|Especifique o uso do serializador "XML" ou "DataContract".|

> [!NOTE]
> Mesmo quando usado com clientes totalmente baseados em COM, o moniker do serviço exige que o WCF e o suporte ao .NET Framework 2,0 sejam instalados no computador cliente. Também é essencial que os aplicativos cliente que usam o moniker do serviço carreguem a versão apropriada do tempo de execução de .NET Framework. Ao usar o moniker em aplicativos do Office, um arquivo de configuração pode ser necessário para garantir que a versão correta da estrutura seja carregada. Por exemplo, com o Excel, o texto a seguir deve ser colocado em um arquivo chamado Excel. exe. config no mesmo diretório que o arquivo Excel. exe:
>
> `<?xml version="1.0" encoding="utf-8"?>`
>
> `<configuration xmlns=` `http://schemas.microsoft.com/.NetConfiguration/v2.0` `>`
>
> `<startup>`
>
> `<requiredRuntime version="v2.0.50727" />`
>
> `</startup>`
>
> `</configuration>`

## <a name="see-also"></a>Consulte também

- [Como registrar e configurar um Moniker de serviço](how-to-register-and-configure-a-service-moniker.md)
