---
title: Elemento <socket> (Configurações de Rede)
ms.date: 03/30/2017
f1_keywords:
- http://schemas.microsoft.com/.NetConfiguration/v2.0#configuration/system.net/settings/socket
- http://schemas.microsoft.com/.NetConfiguration/v2.0#socket
helpviewer_keywords:
- <socket> element
- socket element
ms.assetid: 366c634c-7d16-478f-aedf-053eda94a1a0
ms.openlocfilehash: 0e2b369eccfbc658a790ef61a961315a88361669
ms.sourcegitcommit: b16c00371ea06398859ecd157defc81301c9070f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/06/2020
ms.locfileid: "74089084"
---
# <a name="socket-element-network-settings"></a>Elemento \<socket> (Configurações de Rede)
Especifica se as operações de soquete usam portas de conclusão.  

[**\<configuration>**](../configuration-element.md)\
&nbsp;&nbsp;[**\<system.net>**](system-net-element-network-settings.md)\
&nbsp;&nbsp;&nbsp;&nbsp;[**\<settings>**](settings-element-network-settings.md)\
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;**\<socket>**

## <a name="syntax"></a>Sintaxe  
  
```xml  
<socket  
  alwaysUseCompletionPortsForConnect="true|false"  
  alwaysUseCompletionPortsForAccept="true|false"  
  ipProtectionLevel="EdgeRestricted|Restricted|Unrestricted|Unspecified"  
/>  
```  
  
## <a name="attributes-and-elements"></a>Atributos e elementos  
 As seções a seguir descrevem atributos, elementos filho e elementos pai.  
  
### <a name="attributes"></a>Atributos  
  
|**Atributo**|**Descrição**|  
|-------------------|---------------------|  
|`alwaysUseCompletionPortsForAccept`|Indica se o soquete sempre deve usar as portas de conclusão para aceitar chamadas de método. O valor padrão é `false`.|  
|`alwaysUseCompletionPortsForConnect`|Indica se o soquete sempre deve usar portas de conclusão para chamadas de método Connect. O valor padrão é `false`.|  
|`ipProtectionLevel`|Especifica o padrão <xref:System.Net.Sockets.IPProtectionLevel?displayProperty=nameWithType> a ser usado para um soquete. O valor padrão depende da versão do Windows.|  
  
### <a name="child-elements"></a>Elementos filho  
 Nenhum.  
  
### <a name="parent-elements"></a>Elementos pai  
  
|**Elemento**|**Descrição**|  
|-----------------|---------------------|  
|[configurações](settings-element-network-settings.md)|Configura as opções de rede básicaspara o namespace <xref:System.Net>.|  
  
## <a name="remarks"></a>Comentários  
 Os `alwaysUseCompletionPortsForAccept` `alwaysUseCompletionPortsForConnect` atributos e são usados para especificar o comportamento padrão em relação ao uso de portas de conclusão pelas classes no <xref:System.Net.Sockets?displayProperty=nameWithType> namespace. As portas de conclusão são recomendadas para aplicativos de servidor de alto desempenho.  
  
 O valor padrão para os `alwaysUseCompletionPortsForAccept` `alwaysUseCompletionPortsForConnect` atributos e é **false**.  
  
 O <xref:System.Net.Configuration.SocketElement.AlwaysUseCompletionPortsForAccept%2A> pode ser usado para obter o valor atual do `alwaysUseCompletionPortsForAccept` atributo dos arquivos de configuração aplicáveis. O <xref:System.Net.Configuration.SocketElement.AlwaysUseCompletionPortsForConnect%2A> pode ser usado para obter o valor atual do `alwaysUseCompletionPortsForConnect` atributo dos arquivos de configuração aplicáveis.  
  
 O `ipProtectionLevel` atributo especifica o padrão <xref:System.Net.Sockets.IPProtectionLevel?displayProperty=nameWithType> a ser usado para um soquete. A <xref:System.Net.Configuration.SocketElement.IPProtectionLevel%2A> propriedade permite a configuração de uma restrição para um soquete IPv6 para um escopo especificado, como endereços com o mesmo link local ou local do site. Essa opção permite que os aplicativos coloquem restrições de acesso em soquetes IPv6. Essas restrições permitem que um aplicativo em execução em uma LAN privada proteja-se de modo simples e robusto contra ataques externos. Essa opção amplia ou limita o escopo de um soquete de escuta, habilitando o acesso irrestrito de usuários públicos e privados quando apropriado ou restringindo o acesso somente ao mesmo site, conforme necessário.  
  
 Essa `ipProtectionLevel` configuração de atributo afeta apenas o tráfego de entrada inicial:  
  
- Um servidor TCP escutando conexões de entrada em um soquete.  
  
- Um aplicativo UDP que recebe um pacote em um soquete.  
  
 Essa definição de configuração não afeta as conexões TCP já estabelecidas (o tráfego é irrestrito em ambas as direções) e não afeta um aplicativo que envia pacotes UDP.  
  
 Os valores possíveis para a `ipProtectionLevel` configuração de atributo correspondem aos níveis de proteção definidos especificados na <xref:System.Net.Sockets.IPProtectionLevel?displayProperty=nameWithType> enumeração da seguinte maneira:  
  
|**Valor do atributo**|**Descrição**|  
|-|-|  
|EdgeRestricted|O nível de proteção de IP tem restrição de borda. Esse valor seria usado por aplicativos projetados para operar pela Internet. Essa configuração não permite a passagem de NAT (conversão de endereços de rede) usando a implementação do Windows Teredo. Esses aplicativos podem ignorar firewalls IPv4, de modo que os aplicativos devem ser protegidos contra ataques de Internet direcionados à porta aberta. No Windows Server 2003 e Windows XP, o valor padrão para o nível de proteção de IP em um soquete tem restrição de borda.|  
|Restrito|O nível de proteção de IP é restrito. Esse valor será usado por aplicativos de intranet que não implementam cenários de Internet. Esses aplicativos geralmente não são testados nem protegidos contra ataques do estilo usado na Internet. Essa configuração limitará o tráfego recebido para apenas link-local.|  
|Irrestrito|O nível de proteção de IP é irrestrito. Esse valor seria usado por aplicativos projetados para operar com a Internet, incluindo os aplicativos que aproveitam os recursos de passagem NAT IPv6 internos no Windows (Teredo, por exemplo). Esses aplicativos podem ignorar firewalls IPv4, de modo que os aplicativos devem ser protegidos contra ataques de Internet direcionados à porta aberta. No Windows Server 2008 R2 e Windows Vista, o valor padrão para o nível de proteção de IP em um soquete é irrestrito.|  
|Não Especificado|O nível de proteção de IP não é especificado. No Windows 7 e Windows Server 2008 R2, o valor padrão para o nível de proteção de IP em um soquete é não especificado.|  
  
 O valor padrão para o `ipProtectionLevel` atributo não é **especificado**.  
  
 A <xref:System.Net.Configuration.SocketElement.IPProtectionLevel%2A> propriedade pode ser usada para obter o valor atual do `ipProtectionLevel` atributo dos arquivos de configuração aplicáveis.  
  
## <a name="configuration-files"></a>Arquivos de configuração  
 Esse elemento pode ser usado no arquivo de configuração do aplicativo ou no arquivo de configuração do computador (Machine. config).  
  
## <a name="example"></a>Exemplo  
 O exemplo a seguir mostra como especificar que as portas de conclusão devem ser usadas e que o padrão <xref:System.Net.Sockets.IPProtectionLevel?displayProperty=nameWithType> deve ser irrestrito.  
  
```xml  
<configuration>  
  <system.net>  
    <settings>  
      <socket  
        alwaysUseCompletionPortsForAccept="true"  
        alwaysUseCompletionPortsForConnect="true"  
        ipProtectionLevel="Unrestricted"  
       />  
    </settings>  
  </system.net>  
</configuration>  
```  
  
## <a name="see-also"></a>Confira também

- <xref:System.Net?displayProperty=nameWithType>
- <xref:System.Net.Configuration.SocketElement?displayProperty=nameWithType>
- <xref:System.Net.Sockets?displayProperty=nameWithType>
- <xref:System.Net.Sockets.IPProtectionLevel?displayProperty=nameWithType>
- <xref:System.Net.Sockets.SocketOptionName.IPProtectionLevel?displayProperty=nameWithType>
- [Esquema de configurações de rede](index.md)
