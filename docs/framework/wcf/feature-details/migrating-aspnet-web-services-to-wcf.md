---
title: Migrando serviços Web do ASP.NET para o WCF
ms.date: 03/30/2017
ms.assetid: 1adbb931-f0b1-47f3-9caf-169e4edc9907
ms.openlocfilehash: fa707a4246d5bc9940417072c098b2973140f878
ms.sourcegitcommit: cdb295dd1db589ce5169ac9ff096f01fd0c2da9d
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/09/2020
ms.locfileid: "84598795"
---
# <a name="migrating-aspnet-web-services-to-wcf"></a>Migrando serviços Web do ASP.NET para o WCF
O ASP.NET fornece bibliotecas de classes e ferramentas de .NET Framework para a criação de serviços Web, bem como recursos para hospedar serviços no Serviços de Informações da Internet (IIS). O Windows Communication Foundation (WCF) fornece .NET Framework bibliotecas de classes, ferramentas e recursos de hospedagem para permitir que entidades de software se comuniquem usando qualquer protocolo, incluindo os usados pelos serviços Web.  Migrar serviços Web do ASP.NET para o WCF permite que seus aplicativos aproveitem os novos recursos e melhorias que são exclusivos do WCF.  
  
 O WCF tem várias vantagens importantes em relação aos serviços Web do ASP.NET. Embora as ferramentas de serviços Web do ASP.NET sejam exclusivamente para a criação de serviços da Web, o WCF fornece ferramentas que podem ser usadas quando entidades de software devem ser feitas para se comunicarem umas com as outras. Isso reduzirá o número de tecnologias que os desenvolvedores precisam saber para acomodar diferentes cenários de comunicação de software, o que, por sua vez, reduzirá o custo dos recursos de desenvolvimento de software, bem como o tempo para concluir os projetos de desenvolvimento de software.  
  
 Mesmo para projetos de desenvolvimento de serviço Web, o WCF dá suporte a mais protocolos de serviço Web do que o suporte aos serviços Web ASP.NET. Esses protocolos adicionais fornecem soluções mais sofisticadas envolvendo, entre outras coisas, sessões confiáveis e transações.  
  
 O WCF dá suporte a mais protocolos para transportar mensagens do que os serviços Web do ASP.NET. Os serviços Web do ASP.NET dão suporte apenas ao envio de mensagens usando o protocolo HTTP. O WCF dá suporte ao envio de mensagens usando HTTP, bem como o protocolo TCP, pipes nomeados e MSMQ (enfileiramento de mensagens da Microsoft). Mais importante, o WCF pode ser estendido para dar suporte a protocolos de transporte adicionais. Portanto, o software desenvolvido usando o WCF pode ser adaptado para trabalhar em conjunto com uma variedade maior de outros softwares, aumentando assim o possível retorno sobre o investimento.  
  
 O WCF fornece recursos muito mais ricos para implantar e gerenciar aplicativos do que o ASP.NET Web Services fornece. Além de um sistema de configuração, que ASP.NET também tem, o WCF oferece um editor de configuração, rastreamento de atividade de remetentes para receptores e voltar por qualquer número de intermediários, um visualizador de rastreamento, registro de mensagens, um grande número de contadores de desempenho e suporte para Instrumentação de Gerenciamento do Windows.  
  
 Considerando esses possíveis benefícios do WCF em relação aos serviços Web do ASP.NET, se você estiver usando o ou estiver considerando o uso de serviços Web do ASP.NET, terá várias opções:  
  
- Continue a usar os serviços Web do ASP.NET e, anteriormente, os benefícios proffered pelo WCF.  
  
- Continue usando os serviços Web do ASP.NET com a intenção de adotar o WCF em algum momento no futuro. Os tópicos nesta seção explicam como maximizar os clientes potenciais para poder usar novos aplicativos de serviço Web ASP.NET com aplicativos WCF futuros. Os tópicos nesta seção também explicam como criar novos serviços Web ASP.NET para facilitar sua migração para o WCF. No entanto, se a proteção dos serviços for importante, ou se as garantias de confiabilidade ou de transação forem necessárias, ou se os recursos de gerenciamento personalizado precisarem ser construídos, será uma opção melhor para adotar o WCF. O WCF foi projetado para cenários precisamente.  
  
- Adote o WCF para um novo desenvolvimento, enquanto continua a manter seus aplicativos de serviço Web ASP.NET existentes. Essa opção é muito provavelmente a ideal. Ele gera os benefícios do WCF e, ao mesmo tempo, reserva o custo de modificar os aplicativos existentes para usá-lo. Nesse cenário, novos aplicativos WCF podem coexistir com aplicativos ASP.NET existentes. Os novos aplicativos WCF poderão usar os serviços Web ASP.NET existentes, e o WCF poderá ser usado para programar novos recursos operacionais em aplicativos ASP.NET existentes em virtude do modo de compatibilidade do WCF ASP.NET.  
  
- Adote o WCF e migre aplicativos de serviço Web ASP.NET existentes para o WCF. Você pode escolher essa opção para aprimorar os aplicativos existentes com recursos fornecidos pelo WCF ou para reproduzir a funcionalidade de serviços Web ASP.NET existentes em aplicativos WCF novos e mais poderosos.  
  
> [!NOTE]
> Deve-se ter cuidado se um serviço WCF for hospedado pelo IIS 5. x e o ASP.NET for desinstalado. Quando um serviço WCF é hospedado pelo IIS 5. x, o código para o serviço pode ser solicitado se ASP.NET for desinstalado. Quando o ASP.NET é desinstalado em um sistema operacional que está executando o IIS 5. x e o WCF é desinstalado, um arquivo com a extensão. svc é considerado um arquivo de texto e o conteúdo, incluindo qualquer código-fonte, é retornado ao solicitante.  
  
 Esta seção descreve essas opções em detalhes, compara os serviços Web do ASP.NET com o WCF e fornece instruções sobre como migrar seu código de serviços Web do ASP.NET para o WCF.  
  
## <a name="see-also"></a>Consulte também

- [Antecipando a adoção do Windows Communication Foundation: facilitando a migração futura](anticipating-adopting-wcf-migration.md)
- [Antecipando a adoção do Windows Communication Foundation: facilitando a futura integração](anticipating-adopting-the-wcf-easing-future-integration.md)
- [Adotando o Windows Communication Foundation](adopting-wcf.md)
- [Comparando serviços Web do ASP.NET ao WCF com base na finalidade e nos padrões usados](comparing-aspnet-web-services-to-wcf-based-on-purpose-and-standards-used.md)
- [Comparando os serviços Web ASP.NET com o WCF baseado em desenvolvimento](comparing-aspnet-web-services-to-wcf-based-on-development.md)
