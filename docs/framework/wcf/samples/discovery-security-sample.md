---
title: Exemplo de segurança de descoberta
ms.date: 03/30/2017
ms.assetid: b8db01f4-b4a1-43fe-8e31-26d4e9304a65
ms.openlocfilehash: c6ec9b7e13234b7dae03541eb09ccba98f4cc93a
ms.sourcegitcommit: ee5b798427f81237a3c23d1fd81fff7fdc21e8d3
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/28/2020
ms.locfileid: "84144897"
---
# <a name="discovery-security-sample"></a>Exemplo de segurança de descoberta

A especificação de descoberta não exige que os pontos de extremidade que participam do processo de descoberta sejam seguros. Aprimorar as mensagens de descoberta com segurança atenua vários tipos de ataques (alteração de mensagem, negação de serviço, repetição, falsificação). Este exemplo implementa canais personalizados que computam e verificam assinaturas de mensagens usando o formato de assinatura Compact (descrito na seção 8,2 da especificação do WS-Discovery). O exemplo dá suporte à [especificação de descoberta 2005](http://specs.xmlsoap.org/ws/2005/04/discovery/ws-discovery.pdf) e à [versão 1,1](http://docs.oasis-open.org/ws-dd/discovery/1.1/cs-01/wsdd-discovery-1.1-spec-cs-01.pdf).  
  
 O canal personalizado é aplicado na parte superior da pilha de canais existente para pontos de extremidade de descoberta e anúncio. Dessa forma, um cabeçalho de assinatura é aplicado a cada mensagem enviada. A assinatura é verificada em mensagens recebidas e quando não corresponde ou quando as mensagens não têm uma assinatura, as mensagens são descartadas. Para assinar e verificar mensagens, o exemplo usa certificados.  
  
## <a name="discussion"></a>Discussão  
 O WCF é extensível e permite aos usuários a possibilidade de personalizar os canais conforme desejado. O exemplo implementa um elemento de associação segura de descoberta que cria canais seguros. Os canais seguros se aplicam e verificam as assinaturas de mensagens e são aplicadas sobre a pilha atual.  
  
 O elemento de associação segura cria fábricas de canal e ouvintes de canal seguros.  
  
## <a name="secure-channel-factory"></a>Fábrica de canal seguro  
 A fábrica de canal seguro cria canais de saída ou duplex que adicionam uma assinatura compacta a cabeçalhos de mensagens. Para manter as mensagens tão pequenas quanto possível o formato de assinatura compacta é usado. A estrutura de uma assinatura compacta é mostrada no exemplo a seguir.  
  
```xml  
<d:Security ... >
  [<d:Sig Scheme="xs:anyURI"
         [KeyId="xs:base64Binary"]?  
          Refs="..."  
         [PrefixList]="xs:NMTOKENS"
          Sig="xs:base64Binary"
          ... />]?  
  ...
</d:Security>  
```  
  
> [!NOTE]
> O `PrefixList` foi adicionado no protocolo de versão de descoberta 2008.  
  
 Para calcular a assinatura, o exemplo determina os itens de assinatura expandidos. Uma assinatura XML ( `SignedInfo` ) é criada, usando o `ds` prefixo de namespace, conforme exigido pela especificação WS-Discovery. O corpo e todos os cabeçalhos nos namespaces de descoberta e endereçamento são referenciados na assinatura, para que eles não possam ser adulterados. Cada elemento referenciado é transformado usando a canonicalização exclusiva ( <http://www.w3.org/2001/10/xml-exc-c14n#> ) e, em seguida, um valor de Resumo de SHA-1 é computado ( <http://www.w3.org/2000/09/xmldsig#sha1> ). Com base em todos os elementos referenciados e seus valores de resumo, o valor da assinatura é calculado usando o algoritmo RSA ( <http://www.w3.org/2000/09/xmldsig#rsa-sha1> ).  
  
 As mensagens são assinadas com um certificado especificado pelo cliente. O local do repositório, o nome e o nome da entidade do certificado devem ser especificados quando o elemento de associação é criado. O `KeyId` na assinatura compacta representa o identificador de chave do token de assinatura e é o identificador de chave da entidade (esqui) do token de assinatura ou (se a esqui não existir) um hash SHA-1 da chave pública do token de assinatura.  
  
## <a name="secure-channel-listener"></a>Ouvinte de canal seguro  
 O ouvinte de canal seguro cria canais de entrada ou duplex que verificam a assinatura compacta em mensagens recebidas. Para verificar a assinatura, o `KeyId` especificado na assinatura compacta anexada à mensagem é usado para selecionar um certificado do repositório especificado. Se a mensagem não tiver uma assinatura ou a verificação de assinatura falhar, as mensagens serão descartadas. Para usar a associação segura, o exemplo define uma fábrica que cria personalizado <xref:System.ServiceModel.Discovery.UdpDiscoveryEndpoint> e <xref:System.ServiceModel.Discovery.UdpAnnouncementEndpoint> com o elemento de associação seguro de descoberta adicionado. Esses pontos de extremidade seguros podem ser usados em ouvintes de anúncio de descoberta e serviços detectáveis.  
  
## <a name="sample-details"></a>Detalhes de exemplo  
 O exemplo inclui uma biblioteca e quatro aplicativos de console:
  
- **DiscoverySecurityChannels**: uma biblioteca que expõe a associação segura. A biblioteca computa e verifica a assinatura compactada para mensagens de saída/entrada.  
  
- **Serviço**: um serviço expondo o contrato do ICalculatorService, auto-hospedado. O serviço é marcado como detectável. O usuário especifica os detalhes do certificado usado para assinar mensagens especificando o local e o nome do repositório e o nome da entidade ou outro identificador exclusivo para o certificado e a loja em que os certificados do cliente estão localizados (os certificados usados para verificar a assinatura de mensagens de entrada). Com base nesses detalhes, um UdpDiscoveryEndpoint com segurança adicional é criado e usado.  
  
- **Cliente**: essa classe tenta descobrir um ICalculatorService e chamar métodos no serviço. Novamente, um <xref:System.ServiceModel.Discovery.UdpDiscoveryEndpoint> com segurança adicional é criado e usado para assinar e verificar as mensagens.  
  
- **AnnouncementListener**: um serviço hospedado automaticamente que escuta anúncios online e offline e usa o ponto de extremidade de anúncio seguro.  
  
> [!NOTE]
> Se setup. bat for executado várias vezes, o Gerenciador de certificados solicitará que você escolha um certificado a ser adicionado, pois há certificados duplicados. Nesse caso, setup. bat deve ser anulado e Cleanup. bat deve ser chamado, pois as duplicatas já foram criadas. O Cleanup. bat também solicita que você escolha um certificado a ser excluído. Selecione um certificado na lista e continue executando Cleanup. bat até que nenhum certificado esteja restante.  
  
#### <a name="to-use-this-sample"></a>Para usar este exemplo  
  
1. Execute o script setup. bat de um Prompt de Comando do Desenvolvedor para o Visual Studio. O exemplo usa certificados para assinar e verificar mensagens. O script cria os certificados usando MakeCert. exe e os instala usando o certmgr. exe. O script deve ser executado com privilégios de administrador.  
  
2. Para compilar e executar o exemplo, abra o arquivo Security. sln no Visual Studio e escolha **Recompilar tudo**. Atualizar as propriedades da solução para iniciar vários projetos: selecione **Iniciar** para todos os projetos, exceto DiscoverySecureChannels. Execute a solução normalmente.  
  
3. Depois de concluir o exemplo, execute o script Cleanup. bat que remove os certificados criados para este exemplo.  
  
> [!IMPORTANT]
> Os exemplos podem já estar instalados no seu computador. Verifique o seguinte diretório (padrão) antes de continuar.  
>
> `<InstallDrive>:\WF_WCF_Samples`  
>
> Se esse diretório não existir, vá para [Windows Communication Foundation (WCF) e exemplos de Windows Workflow Foundation (WF) para .NET Framework 4](https://www.microsoft.com/download/details.aspx?id=21459) para baixar todos os Windows Communication Foundation (WCF) e [!INCLUDE[wf1](../../../../includes/wf1-md.md)] exemplos. Este exemplo está localizado no seguinte diretório.  
>
> `<InstallDrive>:\WF_WCF_Samples\WCF\Scenario\DiscoveryScenario`  
