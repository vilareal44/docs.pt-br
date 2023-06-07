---
title: Integração com visão geral de aplicativos COM+
ms.date: 03/30/2017
helpviewer_keywords:
- Windows Communication Foundation, COM+ integration
- WCF, COM+ integration
ms.assetid: e481e48f-7096-40eb-9f20-7f0098412941
ms.openlocfilehash: 57a1537e1bde1efcd3586d032efee063561efcca
ms.sourcegitcommit: cdb295dd1db589ce5169ac9ff096f01fd0c2da9d
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/09/2020
ms.locfileid: "84586488"
---
# <a name="integrating-with-com-applications-overview"></a>Integração com visão geral de aplicativos COM+
O Windows Communication Foundation (WCF) fornece um ambiente avançado para a criação de aplicativos distribuídos. Se você já estiver usando a lógica de aplicativo baseada em componentes hospedada no COM+, poderá usar o WCF para estender sua lógica existente em vez de ter que regravá-la. Um cenário comum é quando você deseja expor a lógica de negócios dos serviços corporativos ou COM+ existentes por meio de serviços Web.  
  
 Quando uma interface em um componente COM+ é exposta como um serviço Web, a especificação e o contrato desses serviços são determinados por um mapeamento automático que é executado no momento da inicialização do aplicativo. A lista a seguir mostra o modelo conceitual para este mapeamento:  
  
- Um serviço é definido para cada classe COM exposta.  
  
- O contrato para o serviço é derivado diretamente da definição de interface do componente selecionado com a possibilidade de exclusão de método definida na configuração.  
  
- As operações nesse contrato são derivadas diretamente dos métodos na definição de interface do componente.  
  
- Os parâmetros para essas operações são derivados diretamente do tipo de interoperabilidade COM que corresponde aos parâmetros do método do componente.  
  
 Os endereços padrão e as associações de transporte para o serviço são fornecidos em um arquivo de configuração de serviço, mas podem ser reconfigurados conforme necessário.  
  
> [!NOTE]
> Os contratos para os serviços Web expostos permanecem constantes, desde que as interfaces e a configuração do COM+ permaneçam inalteradas. Uma modificação em várias interfaces não atualiza automaticamente os serviços disponíveis e requer a nova execução da ferramenta de configuração do modelo de serviço COM+ (ComSvcConfig. exe).  
  
 Os requisitos de autenticação e autorização do aplicativo COM+ e seus componentes continuam sendo aplicados quando usados como um serviço Web.  
  
 Se o chamador iniciar uma transação de serviço Web, os componentes marcados como inscrição transacional dentro desse escopo de transação.  
  
 As etapas a seguir são necessárias para expor a interface de um componente COM+ como um serviço Web sem modificar o componente:  
  
1. Determine se a interface do componente COM+ pode ser exposta como um serviço Web.  
  
2. Selecione um modo de hospedagem apropriado.  
  
3. Use a ferramenta de configuração do modelo de serviço COM+ (ComSvcConfig. exe) para adicionar um serviço Web para a interface. Para obter mais informações sobre como usar o ComSvcConfig. exe, consulte [como usar a ferramenta de configuração do modelo de serviço com+](how-to-use-the-com-service-model-configuration-tool.md).  
  
4. Defina as configurações de serviço adicionais no arquivo de configuração do aplicativo. Para obter mais informações sobre como configurar um componente, consulte [How to: configure com+ Service Settings](how-to-configure-com-service-settings.md).  
  
## <a name="supported-interfaces"></a>Interfaces com suporte  
 Há algumas restrições sobre o tipo de interfaces que podem ser expostas como um serviço Web. Não há suporte para os seguintes tipos de interfaces:  
  
- Interfaces que passam referências de objeto como parâmetros – a abordagem de referência de objeto limitada a seguir é descrita na seção suporte limitado de referência de objeto.  
  
- Interfaces que passam tipos que não são compatíveis com o .NET Framework conversões de interoperabilidade COM.  
  
- Interfaces para aplicativos que têm o pool de aplicativos habilitado quando hospedado pelo COM+.  
  
- Interfaces de componentes que são marcados como particulares para o aplicativo.  
  
- Interfaces de infraestrutura COM+.  
  
- Interfaces do aplicativo do sistema.  
  
- Interfaces de componentes de serviços corporativos que não foram adicionados ao cache de assembly global.  
  
### <a name="limited-object-reference-support"></a>Suporte à referência de objeto limitado  
 Como vários componentes COM+ implantados usam objetos por parâmetros de referência, como retornar um objeto Recordset ADO, a integração COM+ inclui suporte limitado para parâmetros de referência de objeto. O suporte é limitado a objetos que implementam a `IPersistStream` interface com. Isso inclui objetos ADO RecordSet e pode ser implementado para objetos COM específicos do aplicativo.  
  
 Para habilitar esse suporte, a ferramenta ComSvcConfig. exe fornece a opção **allowreferences** que desabilita o parâmetro de assinatura do método regular e verifica se a ferramenta é executada para garantir que os parâmetros de referência do objeto não estejam sendo usados. Além disso, os tipos de objeto que você passa como parâmetros devem ser nomeados e identificados dentro do `persistableTypes` elemento de configuração <> que é um filho do `comContract` elemento> <.  
  
 Quando esse recurso é usado, o serviço de integração COM+ usa a `IPersistStream` interface para serializar ou desserializar a instância do objeto. Se a instância do objeto não oferecer suporte `IPersistStream` , uma exceção será lançada.  
  
 Em um aplicativo cliente, os métodos no <xref:System.ServiceModel.ComIntegration.PersistStreamTypeWrapper> objeto podem ser usados para passar um objeto para um serviço e, de modo semelhante, para recuperar um objeto.  
  
> [!NOTE]
> Devido à natureza personalizada e específica da plataforma da abordagem de serialização, isso é mais adequado para uso entre clientes WCF e serviços WCF.  
  
## <a name="selecting-the-hosting-mode"></a>Selecionando o modo de hospedagem  
 O COM+ expõe os serviços Web em um dos seguintes modos de hospedagem:  
  
- Hospedado pelo COM+  
  
     O serviço Web é hospedado no processo do servidor COM+ dedicado do aplicativo (Dllhost. exe). Esse modo exige que o aplicativo seja explicitamente iniciado antes que possa receber solicitações de serviço Web. As opções COM+ "executar como um serviço NT" ou "deixar em execução quando ocioso" podem ser usadas para evitar o desligamento ocioso do aplicativo e seus serviços. Esse modo fornece o serviço Web e o acesso DCOM ao aplicativo do servidor.  
  
- Hospedado na Web  
  
     O serviço Web é hospedado em um processo de trabalho do servidor Web. Esse modo não exige que o COM+ esteja ativo quando a solicitação inicial é recebida. Se o aplicativo não estiver ativo quando essa solicitação for recebida, ele será ativado automaticamente antes de processar a solicitação. Esse modo também fornece o serviço Web e o acesso DCOM ao aplicativo de servidor, mas causa um salto de processo para solicitações de serviço Web. Isso normalmente requer que o cliente habilite a representação. No WCF, isso pode ser feito com a <xref:System.ServiceModel.Security.WindowsClientCredential.AllowedImpersonationLevel%2A> propriedade da <xref:System.ServiceModel.Security.WindowsClientCredential> classe, que é acessada como uma propriedade da <xref:System.ServiceModel.ChannelFactory%601> classe genérica, bem como o <xref:System.Security.Principal.TokenImpersonationLevel.Impersonation> valor de enumeração.  
  
- Hospedado na Web em processo  
  
     O serviço Web e a lógica do aplicativo COM+ são hospedados dentro do processo de trabalho do servidor Web. Isso fornece a ativação automática do modo hospedado na Web sem causar um salto de processo para solicitações de serviço Web. A desvantagem é que o aplicativo de servidor não pode ser acessado por meio do DCOM.  
  
### <a name="security-considerations"></a>Considerações de segurança  
 Assim como outros serviços do WCF, as configurações de segurança para o serviço exposto são administradas por meio de definições de configuração para o canal do WCF. As configurações de segurança DCOM tradicionais, como as configurações de permissões de todo o computador DCOM, não são impostas. Para impor funções de aplicativo COM+, a autorização "verificações de acesso no nível de componente" deve ser habilitada para o componente.  
  
 O uso de uma associação não segura pode deixar a comunicação aberta para violação ou divulgação de informações. Para evitar isso, é recomendável que você use uma associação segura.  
  
 Para os modos hospedados pelo COM+ e hospedado na Web, os aplicativos cliente devem permitir que o processo do servidor represente o usuário cliente. Isso pode ser feito em clientes WCF definindo o nível de representação como <xref:System.Security.Principal.TokenImpersonationLevel.Impersonation> .  
  
 Com o Serviços de Informações da Internet (IIS) ou o WAS (serviço de ativação de processos do Windows) usando o transporte HTTP, a ferramenta Httpcfg. exe pode ser usada para reservar um endereço de ponto de extremidade de transporte. Em outras configurações, é importante proteger contra serviços não autorizados que atuam como o serviço pretendido. Para impedir que um serviço invasor inicie no ponto de extremidade desejado, o serviço legítimo pode ser configurado para ser executado como um serviço NT. Isso permite que o serviço legítimo solicite o endereço do ponto de extremidade antes de qualquer serviço não autorizado.  
  
 Ao expor um aplicativo COM+ com funções COM+ configuradas como um serviço hospedado na Web, a "Iniciar conta de processo do IIS" deve ser adicionada a uma das funções do aplicativo. Essa conta, normalmente com o nome IWAM_machinename, deve ser adicionada para habilitar o desligamento limpo de objetos após o uso. A conta não deve receber permissões adicionais.  
  
 Os recursos de reciclagem de processos COM+ não podem ser usados em aplicativos integrados. Se o aplicativo estiver configurado para usar a reciclagem de processos e os componentes estiverem em execução em um processo hospedado no COM+, o serviço não será iniciado. Esse requisito não inclui serviços usando o modo de processamento hospedado na Web, pois as configurações de reciclagem de processo não são aplicadas.  
  
## <a name="see-also"></a>Consulte também

- [Visão geral da integração com aplicativos COM](integrating-with-com-applications-overview.md)
