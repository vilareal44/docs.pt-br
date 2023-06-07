---
title: Elemento <network> (Configurações de Rede)
description: O <network> elemento de configurações de rede configura as opções de rede para uma opção de servidor SMTP externo no .NET Framework.
ms.date: 03/30/2017
f1_keywords:
- http://schemas.microsoft.com/.NetConfiguration/v2.0#network
- http://schemas.microsoft.com/.NetConfiguration/v2.0#configuration/system.net/mailSettings/smtp/network
helpviewer_keywords:
- <network> element
- network element
ms.assetid: 2c2c6ad4-ed11-48ab-b28e-2bc0ba9b42c7
ms.openlocfilehash: 36857e63871b4672df349934594f0887a042609e
ms.sourcegitcommit: da21fc5a8cce1e028575acf31974681a1bc5aeed
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/08/2020
ms.locfileid: "84504544"
---
# <a name="network-element-network-settings"></a>Elemento \<network> (Configurações de Rede)
Configura as opções de rede para um servidor de protocolo SMTP externo.  

[**\<configuration>**](../configuration-element.md)\
&nbsp;&nbsp;[**\<system.net>**](system-net-element-network-settings.md)\
&nbsp;&nbsp;&nbsp;&nbsp;[**\<mailSettings>**](mailsettings-element-network-settings.md)\
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;[**\<smtp>**](smtp-element-network-settings.md)\
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;**\<network>**

## <a name="syntax"></a>Sintaxe  
  
```xml  
<network  
  clientDomain="string"
  defaultCredentials="true|false"  
  enableSsl="true|false"  
  host="string"
  password="string"  
  port="integer"
  targetName="string"  
  userName="string"  
/>  
```  
  
## <a name="attributes-and-elements"></a>Atributos e elementos  
 As seções a seguir descrevem atributos, elementos filho e elementos pai.  
  
### <a name="attributes"></a>Atributos  
  
|Atributo|Descrição|  
|---------------|-----------------|  
|`clientDomain`|Especifica o nome de domínio do cliente a ser usado na solicitação inicial do protocolo SMTP para se conectar ao servidor de email SMTP. O valor padrão é o nome de localhost do computador local que está enviando a solicitação.|  
|`defaultCredentials`|Especifica se as credenciais de usuário padrão devem ser usadas para acessar o servidor de email SMTP para transações SMTP. O valor padrão é `false`.|  
|`enableSsl`|Especifica se o SSL é usado para acessar um servidor de email SMTP. O valor padrão é `false`.|  
|`host`|Especifica o nome do host do servidor de email SMTP a ser usado para transações SMTP. Esse atributo não tem valor padrão.|  
|`password`|Especifica a senha a ser usada para autenticação no servidor de email SMTP. Esse atributo não tem valor padrão.|  
|`port`|Especifica o número da porta a ser usada para se conectar ao servidor de email SMTP. O valor padrão é 25.|  
|`targetName`|Especifica o nome do provedor de serviços (SPN) a ser usado para autenticação ao usar a proteção estendida para transações SMTP. Esse atributo não tem valor padrão.|  
|`userName`|Especifica o nome de usuário a ser usado para autenticação no servidor de email SMTP. Esse atributo não tem valor padrão.|  
  
### <a name="child-elements"></a>Elementos filho  
 Nenhum.  
  
### <a name="parent-elements"></a>Elementos pai  
  
|Elemento|Descrição|  
|-------------|-----------------|  
|[\<smtp>Elemento (configurações de rede)](smtp-element-network-settings.md)|Configura as opções de envio de email do protocolo SMTP.|  
  
## <a name="remarks"></a>Comentários  
 Alguns servidores SMTP exigem que você se autentique no servidor antes de usar o. Se você quiser se autenticar usando as credenciais de rede padrão em seu host, defina o `defaultCredentials` atributo como `true` . A <xref:System.Net.Configuration.SmtpNetworkElement.DefaultCredentials%2A?displayProperty=nameWithType> propriedade pode ser usada para obter o valor atual do `defaultCredentials` atributo dos arquivos de configuração aplicáveis.  
  
 Você também pode usar a autenticação básica (um nome de usuário e senha) para se autenticar no servidor SMTP. Para usar essa opção, você deve especificar um nome de usuário e uma senha válidos para o servidor SMTP especificado.  
  
> [!NOTE]
> A autenticação básica envia `userName` os `password` valores e ao servidor sem criptografia. Qualquer pessoa que monitorar o tráfego de rede pode exibir suas credenciais e usá-las para se conectar ao servidor. Você deve considerar o uso de um mecanismo de autenticação mais seguro, como o Kerberos ou o NT LAN Manager (NTLM). Se `defaultCredentials` for `true` , o Kerberos ou NTLM será usado se o servidor oferecer suporte a esses protocolos.  
  
 As opções de autenticação básica e credenciais de rede padrão são mutuamente exclusivas; Se você definir `defaultCredentials` como `true` e especificar um nome de usuário e uma senha, a credencial de rede padrão será usada e os dados de autenticação básica serão ignorados.  
  
 Para a autenticação básica, se você especificar um `userName` , também deverá especificar um `password` para autenticação por conta própria no servidor de email.  
  
 A <xref:System.Net.Configuration.SmtpNetworkElement.UserName%2A?displayProperty=nameWithType> propriedade pode ser usada para obter o valor atual do `userName` atributo dos arquivos de configuração aplicáveis. A <xref:System.Net.Configuration.SmtpNetworkElement.Password%2A?displayProperty=nameWithType> propriedade pode ser usada para obter o valor atual do `password` atributo dos arquivos de configuração aplicáveis. Um `password` atributo normalmente não seria inserido em arquivos de configuração por motivos de segurança.  
  
 O `clientDomain` atributo altera o nome de domínio do cliente usado na solicitação inicial do protocolo SMTP para um servidor SMTP. O `clientDomain` atributo pode ser definido como o nome de domínio totalmente qualificado do computador local, em vez do nome do localhost usado por padrão. Isso fornece maior conformidade com os padrões de protocolo SMTP. O valor padrão é o nome de localhost do computador local que está enviando a solicitação. A <xref:System.Net.Configuration.SmtpNetworkElement.ClientDomain%2A?displayProperty=nameWithType> propriedade pode ser usada para obter o valor atual do `clientDomain` atributo dos arquivos de configuração aplicáveis.  
  
 O `targetName` atributo é usado para autenticação ao usar a proteção estendida. O valor padrão é no formato "SMTPSVC/ \<host> ", em que \<host> é o nome do host do servidor de email SMTP. A <xref:System.Net.Configuration.SmtpNetworkElement.TargetName%2A?displayProperty=nameWithType> propriedade pode ser usada para obter o valor atual do `targetName` atributo dos arquivos de configuração aplicáveis.  
  
 O `enableSsl` atributo especifica se o SSL é usado para acessar um servidor de email SMTP. A <xref:System.Net.Mail.SmtpClient?displayProperty=nameWithType> classe dá suporte apenas à extensão de serviço SMTP para segurança SMTP pela camada de transporte, conforme definido no RFC 3207. Nesse modo, a sessão SMTP começa em um canal não criptografado. em seguida, um comando STARTTLS é emitido pelo cliente para o servidor para alternar para a comunicação segura usando SSL. Consulte RFC 3207 publicado pela Internet Engineering Task Force (IETF) para obter mais informações.  
  
 Um método de conexão alternativo é onde uma sessão SSL é estabelecida antecipadamente antes que qualquer comando de protocolo seja enviado. Esse método de conexão às vezes é chamado de SMTPs e, por padrão, usa a porta 465. No momento, não há suporte para esse método de conexão alternativo usando SSL.  
  
 A <xref:System.Net.Configuration.SmtpNetworkElement.EnableSsl%2A?displayProperty=nameWithType> propriedade pode ser usada para obter o valor atual do `enableSsl` atributo dos arquivos de configuração aplicáveis.  
  
## <a name="example"></a>Exemplo  
 O exemplo a seguir especifica os parâmetros de SMTP apropriados para enviar email usando as credenciais de rede padrão.  
  
```xml  
<configuration>  
  <system.net>  
    <mailSettings>  
      <smtp deliveryMethod="Network">  
        <network  
          clientDomain="www.contoso.com"  
          defaultCredentials="true"  
          enableSsl="false"  
          host="mail.contoso.com"  
          port="25"  
        />  
      </smtp>  
    </mailSettings>  
  </system.net>  
</configuration>  
```  
  
## <a name="see-also"></a>Confira também

- <xref:System.Net.Configuration.SmtpNetworkElement?displayProperty=nameWithType>
- <xref:System.Net.Configuration.SmtpSection?displayProperty=nameWithType>
- <xref:System.Net.Mail.SmtpClient?displayProperty=nameWithType>
- [Esquema de configurações de rede](index.md)
