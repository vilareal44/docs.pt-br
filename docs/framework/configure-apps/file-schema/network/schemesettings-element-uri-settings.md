---
title: Elemento <schemeSettings> (Configurações de URI)
ms.date: 03/30/2017
ms.assetid: 0ae45c6e-8c4c-4c0d-8b9f-a93824648890
ms.openlocfilehash: c745c90bb61b9ee393687d7f6db4fd11565c7dc7
ms.sourcegitcommit: b16c00371ea06398859ecd157defc81301c9070f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/06/2020
ms.locfileid: "79154641"
---
# <a name="schemesettings-element-uri-settings"></a>Elemento \<schemeSettings> (Configurações de URI)
Especifica como um <xref:System.Uri> será analisado quanto a esquemas específicos.  
  
[**\<configuration>**](../configuration-element.md)  
&nbsp;&nbsp;[**\<uri>**](uri-element-uri-settings.md)  
&nbsp;&nbsp;&nbsp;&nbsp;**\<schemeSettings>**  
  
## <a name="syntax"></a>Sintaxe  
  
```xml  
<schemeSettings>
</schemeSettings>  
```  
  
## <a name="attributes-and-elements"></a>Atributos e elementos  
 As seções a seguir descrevem atributos, elementos filho e elementos pai.  
  
### <a name="attributes"></a>Atributos  
 Nenhum  
  
### <a name="child-elements"></a>Elementos filho  
  
|**Elemento**|**Descrição**|  
|-----------------|---------------------|  
|[adicionar](add-element-for-schemesettings-uri-settings.md)|Adiciona uma configuração de esquema para um nome de esquema.|  
|[formatação](clear-element-for-schemesettings-uri-settings.md)|Limpa todas as configurações de esquema existentes.|  
|[remove](remove-element-for-schemesettings-uri-settings.md)|Remove uma configuração de esquema para um nome de esquema.|  
  
### <a name="parent-elements"></a>Elementos pai  
  
|**Elemento**|**Descrição**|  
|-----------------|---------------------|  
|[URI](uri-element-uri-settings.md)|Contém configurações que especificam como o .NET Framework trata os endereços da Web expressos usando URIs (identificadores de recursos uniformes).|  
  
## <a name="remarks"></a>Comentários  
 Por padrão, a <xref:System.Uri?displayProperty=nameWithType> classe não escapa os delimitadores de caminho codificados por porcentagem antes de executar a compactação de caminho. Isso foi implementado como um mecanismo de segurança contra ataques como o seguinte:  
  
 `http://www.contoso.com/..%2F..%2F/Windows/System32/cmd.exe?/c+dir+c:\`  
  
 Se esse URI for passado para os módulos que não manipulam a porcentagem de caracteres codificados corretamente, isso pode fazer com que o seguinte comando seja executado pelo servidor:  
  
 `c:\Windows\System32\cmd.exe /c dir c:\`  
  
 Por esse motivo, a <xref:System.Uri?displayProperty=nameWithType> classe não escapa primeiro os delimitadores de caminho e, em seguida, aplica a compactação de caminho. O resultado da passagem da URL mal-intencionada acima para o <xref:System.Uri?displayProperty=nameWithType> Construtor de classe resulta no seguinte URI:  
  
 `http://www.microsoft.com/Windows/System32/cmd.exe?/c+dir+c:\`  
  
 Esse comportamento padrão pode ser modificado para não cancelar o escape de delimitadores de caminho codificados por porcentagem usando a opção de configuração schemeSettings para um esquema específico.  
  
## <a name="configuration-files"></a>Arquivos de configuração  
 Esse elemento pode ser usado no arquivo de configuração do aplicativo ou no arquivo de configuração do computador (Machine. config).  
  
## <a name="example"></a>Exemplo  
 O exemplo a seguir mostra uma configuração usada pela <xref:System.Uri> classe para dar suporte a não escapar delimitadores de caminho codificados por porcentagem para o esquema http.  
  
```xml  
<configuration>  
  <uri>  
    <schemeSettings>  
      <add name="http" genericUriParserOptions="DontUnescapePathDotsAndSlashes"/>  
    </schemeSettings>  
  </uri>  
</configuration>  
```  
  
## <a name="element-information"></a>Informações do elemento  
  
|||
|-|-|  
|Namespace|Sistema|  
|Nome do Esquema||  
|Arquivo de validação||  
|Pode estar vazio||  
  
## <a name="see-also"></a>Confira também

- <xref:System.Configuration.SchemeSettingElement?displayProperty=nameWithType>
- <xref:System.Configuration.SchemeSettingElementCollection?displayProperty=nameWithType>
- <xref:System.Configuration.UriSection?displayProperty=nameWithType>
- <xref:System.Configuration.UriSection.SchemeSettings%2A?displayProperty=nameWithType>
- <xref:System.GenericUriParserOptions?displayProperty=nameWithType>
- <xref:System.Uri?displayProperty=nameWithType>
- [Esquema de configurações de rede](index.md)
