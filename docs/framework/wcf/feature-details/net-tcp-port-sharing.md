---
title: Compartilhamento de porta Net.TCP
description: Saiba mais sobre um protocolo baseado em TCP para comunicação de alto desempenho e o serviço que permite que as portas sejam compartilhadas entre vários processos de usuário no WCF.
ms.date: 03/30/2017
helpviewer_keywords:
- port activation [WCF]
- port sharing [WCF]
ms.assetid: f13692ee-a179-4439-ae72-50db9534eded
ms.openlocfilehash: a9579c588906f509dd835d3c9b25571495d147e0
ms.sourcegitcommit: 358a28048f36a8dca39a9fe6e6ac1f1913acadd5
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/23/2020
ms.locfileid: "85245239"
---
# <a name="nettcp-port-sharing"></a>Compartilhamento de porta Net.TCP
O Windows Communication Foundation (WCF) fornece um novo protocolo de rede baseado em TCP (net. TCP://) para comunicação de alto desempenho. O WCF também apresenta um novo componente do sistema, o serviço de compartilhamento de porta Net. TCP que permite que as portas net. TCP sejam compartilhadas entre vários processos de usuário.  
  
## <a name="background-and-motivation"></a>Motivação e Segundo Plano  
 Quando o protocolo TCP/IP foi introduzido pela primeira vez, apenas um pequeno número de protocolos de aplicativo fez uso dele. Os números de porta usados pelo TCP/IP para diferenciar os aplicativos atribuindo um número de porta de 16 bits exclusivo a cada protocolo de aplicativo. Por exemplo, o tráfego HTTP hoje é padronizado para usar a porta TCP 80, o SMTP usa a porta TCP 25 e o FTP usa as portas TCP 20 e 21. Outros aplicativos que usam TCP como transporte podem escolher outro número de porta disponível, seja por convenção ou por meio de padronização formal.  
  
 O uso de números de porta para distinguir entre aplicativos apresentou problemas de segurança. Os firewalls geralmente são configurados para bloquear o tráfego TCP em todas as portas, exceto por alguns pontos de entrada conhecidos, de modo que a implantação de um aplicativo que usa uma porta não padrão é geralmente complicada ou até mesmo impossível devido à presença de firewalls corporativos e pessoais. Aplicativos que podem se comunicar por portas padrão e conhecidas que já são permitidas, reduzir a superfície de ataque externa. Muitos aplicativos de rede usam o protocolo HTTP porque a maioria dos firewalls são configurados por padrão para permitir o tráfego na porta TCP 80.  
  
 O modelo de HTTP.SYS no qual o tráfego para muitos aplicativos HTTP diferentes é multiplexado em uma única porta TCP se tornou padrão na plataforma Windows. Isso fornece um ponto comum de controle para administradores de firewall, permitindo que os desenvolvedores de aplicativos minimizem o custo de implantação da criação de novos aplicativos que podem fazer uso da rede.  
  
 A capacidade de compartilhar portas em vários aplicativos HTTP tem sido um recurso do Serviços de Informações da Internet (IIS). No entanto, apenas com a introdução do HTTP.SYS (o ouvinte do protocolo HTTP no modo kernel) com o IIS 6,0 que essa infraestrutura estava totalmente generalizada. Na verdade, HTTP.SYS permite que processos de usuário arbitrários compartilhem as portas TCP dedicadas ao tráfego HTTP. Esse recurso permite que muitos aplicativos HTTP coexistam na mesma máquina física em processos isolados e separados enquanto compartilham a infraestrutura de rede necessária para enviar e receber tráfego pela porta TCP 80. O serviço de compartilhamento de porta Net. TCP permite o mesmo tipo de compartilhamento de porta para aplicativos net. TCP.  
  
## <a name="port-sharing-architecture"></a>Arquitetura de compartilhamento de porta  
 A arquitetura de compartilhamento de porta no WCF tem três componentes principais:  
  
- Um processo de trabalho: qualquer processo de comunicação via net. TCP://usando portas compartilhadas.  
  
- O transporte TCP do WCF: implementa o protocolo net. TCP://.  
  
- O serviço de compartilhamento de porta Net. TCP: permite que muitos processos de trabalho compartilhem a mesma porta TCP.  
  
 O serviço de compartilhamento de porta Net. TCP é um serviço do Windows no modo de usuário que aceita net. TCP://Connections em nome dos processos de trabalho que se conectam através dele. Quando chega uma conexão de soquete, o serviço de compartilhamento de porta inspeciona o fluxo de mensagens de entrada para obter seu endereço de destino. Com base nesse endereço, o serviço de compartilhamento de porta pode rotear o fluxo de dados para o aplicativo que, por fim, o processa.  
  
 Quando um serviço WCF que usa net. TCP://compartilhamento de porta é aberto, a infraestrutura de transporte TCP do WCF não abre diretamente um soquete TCP no processo do aplicativo. Em vez disso, a infraestrutura de transporte registra o endereço base Uniform Resource Identifier (URI) do serviço com o serviço de compartilhamento de porta Net. TCP e aguarda o serviço de compartilhamento de porta escutar mensagens em seu nome.  O serviço de compartilhamento de porta distribui mensagens endereçadas ao serviço de aplicativo à medida que elas chegam.  
  
## <a name="installing-port-sharing"></a>Instalando o compartilhamento de porta  
 O serviço de compartilhamento de porta Net. TCP está disponível em todos os sistemas operacionais que dão suporte ao WinFX, mas o serviço não está habilitado por padrão. Como uma precaução de segurança, um administrador deve habilitar manualmente o serviço de compartilhamento de porta Net. TCP antes de ser usado pela primeira vez. O serviço de compartilhamento de porta Net. TCP expõe opções de configuração que permitem manipular várias características dos soquetes de rede pertencentes ao serviço de compartilhamento de porta. Para obter mais informações, consulte [como habilitar o serviço de compartilhamento de porta Net. TCP](how-to-enable-the-net-tcp-port-sharing-service.md).  
  
## <a name="using-nettcp-port-sharing-in-an-application"></a>Usando o compartilhamento de porta Net. TCP em um aplicativo  
 A maneira mais fácil de usar net. TCP://compartilhamento de porta em seu aplicativo WCF é expor um serviço usando o <xref:System.ServiceModel.NetTcpBinding> e, em seguida, habilitar o serviço de compartilhamento de porta Net. TCP usando a <xref:System.ServiceModel.NetTcpBinding.PortSharingEnabled%2A> propriedade.  
  
 Para obter mais informações sobre como fazer isso, consulte [como: configurar um serviço WCF para usar o compartilhamento de porta](how-to-configure-a-wcf-service-to-use-port-sharing.md).  
  
## <a name="security-implications-of-port-sharing"></a>Implicações de segurança do compartilhamento de porta  
 Embora o serviço de compartilhamento de porta Net. TCP forneça uma camada de processamento entre aplicativos e a rede, os aplicativos que usam o compartilhamento de porta ainda devem ser protegidos como se estivessem ouvindo diretamente na rede. Especificamente, os aplicativos que usam o compartilhamento de porta devem avaliar os privilégios de processo sob os quais eles são executados. Considere executar seu aplicativo usando a conta de serviço de rede interna, que é executada com o conjunto mínimo de privilégios de processo necessários para a comunicação de rede.  
  
## <a name="see-also"></a>Veja também

- [Configurando o serviço de compartilhamento de porta Net.TCP](configuring-the-net-tcp-port-sharing-service.md)
- [Hosting](hosting.md)
- [Como configurar um serviço WCF para usar compartilhamento de porta](how-to-configure-a-wcf-service-to-use-port-sharing.md)
- [Como habilitar o serviço de compartilhamento de porta Net.TCP](how-to-enable-the-net-tcp-port-sharing-service.md)
