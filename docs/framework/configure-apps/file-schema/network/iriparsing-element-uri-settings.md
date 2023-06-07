---
title: Elemento <iriParsing> (Configurações de URI)
ms.date: 03/30/2017
ms.assetid: 953d0b53-445e-41f9-b302-77c4030852ce
ms.openlocfilehash: fd617d1b4ac8e532c6f9aeaa01465e9866b059e9
ms.sourcegitcommit: b16c00371ea06398859ecd157defc81301c9070f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/06/2020
ms.locfileid: "71698100"
---
# <a name="iriparsing-element-uri-settings"></a>Elemento \<iriParsing> (Configurações de URI)
Especifica se a análise de IRI (Identificador de Recurso Internacional) é aplicada a um <xref:System.Uri> e se as regras de análise de IRI devem ser aplicada.  
  
[**\<configuration>**](../configuration-element.md)  
&nbsp;&nbsp;[**\<uri>**](uri-element-uri-settings.md)  
&nbsp;&nbsp;&nbsp;&nbsp;**\<iriParsing>**  
  
## <a name="syntax"></a>Sintaxe  
  
```xml  
<iriParsing  
  enabled="true|false"  
/>  
```  
  
## <a name="attributes-and-elements"></a>Atributos e elementos  
 As seções a seguir descrevem atributos, elementos filho e elementos pai.  
  
### <a name="attributes"></a>Atributos  
  
|**Elemento**|**Descrição**|  
|-----------------|---------------------|  
|`enabled`|Especifica se a análise de IRI está habilitada. O valor padrão é `false`.|  
  
### <a name="child-elements"></a>Elementos filho  
 Nenhum  
  
### <a name="parent-elements"></a>Elementos pai  
  
|**Elemento**|**Descrição**|  
|-----------------|---------------------|  
|[URI](uri-element-uri-settings.md)|Contém configurações que especificam como o .NET Framework trata os endereços da Web expressos usando URIs (identificadores de recursos uniformes).|  
  
## <a name="remarks"></a>Comentários  
 A <xref:System.Uri> classe existente foi estendida no .NET Framework 3,5. 3,0 SP1 e 2,0 SP1 para fornecer suporte para IRI (identificadores de recursos internacionais) e IDNs (nomes de domínio internacionalizados). Os usuários atuais não verão nenhuma alteração do comportamento .NET Framework 2,0, a menos que eles especificamente habilitem o suporte a IRI e IDN. Isso garante a compatibilidade do aplicativo com versões anteriores do .NET Framework.  
  
 Para habilitar o suporte para IRI, as duas alterações a seguir são necessárias:  
  
1. Adicione a seguinte linha ao arquivo Machine. config no diretório .NET Framework 2,0  
  
    ```xml  
    <section name="uri" type="System.Configuration.UriSection, System, Version=2.0.0.0, Culture=neutral, PublicKeyToken=b77a5c561934e089" />  
    ```  
  
2. Especifique se as regras de análise de IRI devem ser aplicadas. Isso pode ser feito no arquivo machine.config ou em app.config.  
  
 Habilitar a análise de IRI (iriParsing Enabled = `true` ) fará a normalização e a verificação de caracteres de acordo com as regras de IRI mais recentes no RFC 3987. O valor padrão é `false` e fará a normalização e a verificação de caracteres de acordo com rfc 2396 e rfc 3986 (para literais IPv6).  
  
### <a name="configuration-files"></a>Arquivos de configuração  
 Esse elemento pode ser usado no arquivo de configuração do aplicativo ou no arquivo de configuração do computador (Machine. config).  
  
## <a name="example"></a>Exemplo  
  
### <a name="description"></a>Descrição  
 O exemplo a seguir mostra uma configuração usada pela <xref:System.Uri> classe para dar suporte a análise de IRI e nomes IDN.  
  
### <a name="code"></a>Código  
  
```xml  
<configuration>  
  <uri>  
    <idn enabled="All" />  
    <iriParsing enabled="true" />  
  </uri>  
</configuration>  
```  
  
## <a name="see-also"></a>Confira também

- <xref:System.Configuration.IriParsingElement?displayProperty=nameWithType>
- <xref:System.Configuration.UriSection?displayProperty=nameWithType>
- [Esquema de configurações de rede](index.md)
