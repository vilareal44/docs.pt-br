---
title: <add> de <serviceActivations>
ms.date: 03/30/2017
ms.assetid: e5b01fc8-ee84-48b7-95fd-95ab54fa871f
ms.openlocfilehash: a0f68717f765482f53e675458fae63d1a374d6fb
ms.sourcegitcommit: b16c00371ea06398859ecd157defc81301c9070f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/06/2020
ms.locfileid: "70850321"
---
# <a name="add-of-serviceactivations"></a>\<add> de \<serviceActivations>

Um elemento de configuração que permite definir configurações de ativação de serviço virtual que são mapeadas para seus tipos de serviço do Windows Communication Foundation (WCF). Isso possibilita a ativação de serviços hospedados no WAS/IIS sem um arquivo. svc.

[**\<configuration>**](../configuration-element.md)\
&nbsp;&nbsp;[**\<system.serviceModel>**](system-servicemodel.md)\
&nbsp;&nbsp;&nbsp;&nbsp;[**\<serviceHostingEnvironment>**](servicehostingenvironment.md)\
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;[**\<serviceActivations>**](serviceactivations.md)\
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;**\<add>**  

## <a name="syntax"></a>Sintaxe

```xml
<serviceHostingEnvironment>
    <serviceActivations>
      <add factory="String"
           service="String" />
  </serviceActivations>
</serviceHostingEnvironment>
```

## <a name="attributes-and-elements"></a>Atributos e elementos

As seções a seguir descrevem atributos, elementos filho e elementos pai.

### <a name="attributes"></a>Atributos

|Atributo|Descrição|
|---------------|-----------------|
|fábrica|Uma cadeia de caracteres que especifica o nome do tipo CLR da fábrica que gera um elemento de ativação de serviço.|
|serviço|O ServiceType que implementa o serviço (o TypeName totalmente qualificado ou o TypeName curto (quando é colocado na pasta App_Code).|
|relativeAddress|O endereço relativo no aplicativo IIS atual-por exemplo, "Service. svc". No WCF 4,0, esse endereço relativo deve conter uma das extensões de arquivo conhecidas (. svc,. xamlx,...). Nenhum arquivo físico deve existir para o relativeUrl|

### <a name="child-elements"></a>Elementos filho

Nenhum.

### <a name="parent-elements"></a>Elementos pai

|Elemento|Descrição|
|-------------|-----------------|
|[\<serviceHostingEnvironment>](servicehostingenvironment.md)|Uma seção de configuração que descreve as configurações de ativação.|

## <a name="remarks"></a>Comentários

O exemplo a seguir mostra como definir as configurações de ativação no arquivo Web. config.

```xml
<configuration>
  <system.serviceModel>
    <serviceHostingEnvironment>
      <serviceActivations>
        <add service="GreetingService" />
      </serviceActivations>
    </serviceHostingEnvironment>
  </system.serviceModel>
</configuration>
```

Usando essa configuração, você pode ativar o GreetingService sem usar um arquivo. svc.

Observe que `<serviceHostingEnvironment>` é uma configuração de nível de aplicativo. Você precisa posicionar o `web.config` que contém a configuração na raiz do aplicativo virtual. Além disso, `serviceHostingEnvironment` é uma seção de reherança de machineToApplication. Se você registrar um único serviço na raiz do computador, cada serviço no aplicativo herdará esse serviço.

A ativação baseada em configuração dá suporte à ativação por meio do protocolo http e não http. Ele requer extensões no relativeAddress, ou seja,. svc,. xoml ou. xamlx. Você pode mapear suas próprias extensões para o know buildProviders, que permitirá que você ative o serviço em qualquer extensão. Após o conflito, a `<serviceActivations>` seção substitui os registros. svc.

## <a name="see-also"></a>Confira também

- <xref:System.ServiceModel.Configuration.ServiceActivationElement>
- <xref:System.ServiceModel.Configuration.ServiceHostingEnvironmentSection>
- <xref:System.ServiceModel.ServiceHostingEnvironment>
