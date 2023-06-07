---
title: Configurando o serviço de compartilhamento de porta Net.TCP
ms.date: 03/30/2017
ms.assetid: b6dd81fa-68b7-4e1b-868e-88e5901b7ea0
ms.openlocfilehash: acfb720ba74cda62b2949263fcb31d356671b4f3
ms.sourcegitcommit: cdb295dd1db589ce5169ac9ff096f01fd0c2da9d
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/09/2020
ms.locfileid: "84597508"
---
# <a name="configuring-the-nettcp-port-sharing-service"></a>Configurando o serviço de compartilhamento de porta Net.TCP
Os serviços de hospedagem interna que usam o transporte net. TCP podem controlar várias configurações avançadas, como `ListenBacklog` e `MaxPendingAccepts` , que regem o comportamento do soquete TCP subjacente usado para comunicação de rede. No entanto, essas configurações para cada soquete se aplicam apenas no nível de associação se a associação de transporte tiver desabilitado o compartilhamento de porta, que está habilitado por padrão.  
  
 Quando uma associação net. TCP habilita o compartilhamento de porta (por configuração `portSharingEnabled =true` no elemento de associação de transporte), ele permite implicitamente um processo externo (ou seja, o SMSvcHost. exe, que hospeda o serviço de compartilhamento de porta Net. TCP) para gerenciar o soquete TCP em seu nome. Por exemplo, ao usar TCP, especifique:  
  
```xml  
<tcpTransport portSharingEnabled="true"  />  
```  
  
 Quando configurado dessa forma, todas as configurações de soquete especificadas no elemento de associação de transporte do serviço são ignoradas em favor das configurações de soquete especificadas por SMSvcHost. exe.  
  
 Para configurar o SMSvcHost. exe, crie um arquivo de configuração XML chamado SmSvcHost. exe. config e coloque-o no mesmo diretório físico que o executável SMSvcHost. exe (por exemplo, C:\Windows\Microsoft.NET\Framework\v4.5).  
  
 O exemplo a seguir ilustra um exemplo de SMSvcHost. exe. config, com as configurações padrão para todos os valores configuráveis declarados explicitamente.  
  
```xml  
<configuration>  
   <system.serviceModel.activation>  
       <net.tcp listenBacklog="16" <!-- 16 * # of processors -->  
          maxPendingAccepts="4"<!-- 4 * # of processors -->  
          maxPendingConnections="100"  
          receiveTimeout="00:00:30" <!-- 30 seconds -->  
          teredoEnabled="false">  
          <allowAccounts>  
             <!-- LocalSystem account -->  
             <add securityIdentifier="S-1-5-18"/>  
             <!-- LocalService account -->  
             <add securityIdentifier="S-1-5-19"/>  
             <!-- Administrators account -->  
             <add securityIdentifier="S-1-5-20"/>  
             <!-- Network Service account -->  
             <add securityIdentifier="S-1-5-32-544" />  
             <!-- IIS_IUSRS account (Vista only) -->  
             <add securityIdentifier="S-1-5-32-568"/>  
           </allowAccounts>  
       </net.tcp>  
    </system.serviceModel.activation>
</configuration>  
```  
  
## <a name="when-to-modify-smsvchostexeconfig"></a>Quando modificar SMSvcHost. exe. config  
 Em geral, deve-se ter cuidado ao modificar o conteúdo do arquivo SMSvcHost. exe. config, pois as definições de configuração especificadas nesse arquivo afetam todos os serviços em um computador que usa o serviço de compartilhamento de porta Net. TCP. Isso inclui aplicativos no Windows Vista que usam os recursos de ativação TCP do WAS (serviço de ativação de processos do Windows).  
  
 No entanto, às vezes, talvez seja necessário alterar a configuração padrão para o serviço de compartilhamento de porta Net. TCP. Por exemplo, o valor padrão para `maxPendingAccepts` é 4 * número de processadores. Os servidores que hospedam um grande número de serviços que usam o compartilhamento de porta podem aumentar esse valor para obter a taxa de transferência máxima. O valor padrão para `maxPendingConnections` é 100. Você deve considerar aumentar esse valor também se houver vários clientes simultâneos chamando o serviço e o serviço estiver removendo conexões de cliente.  
  
 SMSvcHost. exe. config também contém informações sobre as identidades de processo que podem fazer uso do serviço de compartilhamento de porta. Quando um processo se conecta ao serviço de compartilhamento de porta para fazer uso de uma porta TCP compartilhada, a identidade do processo de conexão é verificada em uma lista de identidades que têm permissão para usar o serviço de compartilhamento de porta. Essas identidades são especificadas como identificadores de segurança (SIDs) na \<allowAccounts> seção do arquivo SMSvcHost. exe. config. Por padrão, a permissão para usar o serviço de compartilhamento de porta é concedida a contas de sistema (LocalService, LocalSystem e NetworkService), bem como a membros do grupo Administradores. Os aplicativos que permitem que um processo em execução como outra identidade (por exemplo, uma identidade de usuário) se conectem ao serviço de compartilhamento de porta devem adicionar explicitamente o SID apropriado ao SMSvcHost. exe. config (essas alterações não são aplicadas até que o processo SMSvc. exe seja reiniciado).  
  
> [!NOTE]
> Em sistemas Windows Vista com UAC (controle de conta de usuário) habilitado, os usuários locais exigem permissões elevadas, mesmo que sua conta seja membro do grupo Administradores. Para permitir que esses usuários façam uso do serviço de compartilhamento de porta sem elevação, o SID do usuário (ou o SID de um grupo no qual o usuário é membro) deve ser adicionado explicitamente à \<allowAccounts> seção de SMSvcHost. exe. config.  
  
> [!WARNING]
> O arquivo SMSvcHost. exe. config padrão especifica um personalizado `etwProviderId` para impedir que o rastreamento SMSvcHost. exe interfira nos rastreamentos de serviço.  
  
## <a name="see-also"></a>Confira também

- [\<net.tcp>](../../configure-apps/file-schema/wcf/net-tcp.md)
