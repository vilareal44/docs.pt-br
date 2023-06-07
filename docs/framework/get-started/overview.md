---
title: Visão geral do .NET Framework
description: Leia uma visão geral sobre o .NET, que é uma tecnologia que dá suporte à criação e execução de aplicativos e serviços Web do Windows.
ms.date: 03/30/2017
helpviewer_keywords:
- application development [.NET Framework]
- common language runtime
- common language runtime, about
- common language runtime, overview
ms.assetid: 29848c96-fc36-462d-8072-ba223a40b697
ms.openlocfilehash: 6beedb8e3fd03049cd58ce1d2dac78d1adb820ef
ms.sourcegitcommit: e02d17b2cf9c1258dadda4810a5e6072a0089aee
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/01/2020
ms.locfileid: "85618747"
---
# <a name="overview-of-net-framework"></a>Visão geral do .NET Framework

.NET Framework é uma tecnologia que dá suporte à criação e execução de aplicativos e serviços Web do Windows. O .NET Framework foi projetado para atender aos seguintes objetivos:

- Fornecer um ambiente de programação orientado a objeto consistente, independentemente de o código de objeto ser armazenado e executado localmente, executado localmente, mas distribuído pela Web ou executado remotamente.

- Para fornecer um ambiente de execução de código que minimize conflitos de implantação e criação de versão do software.

- Para fornecer um ambiente da execução que promova a execução segura do código, incluindo o código criado por terceiros desconhecidos ou semiconfiáveis.

- Para fornecer um ambiente de execução de código que elimine os problemas de desempenho dos ambientes interpretados ou com scripts.

- Para tornar a experiência do desenvolvedor consistente entre diversos tipos de aplicativos, como aplicativos baseados no Windows e aplicativos baseados na Web.

- Para criar toda a comunicação nos padrões do setor para garantir que o código baseado em .NET Framework se integre a qualquer outro código.

> [!NOTE]
> Para obter uma introdução geral ao .NET Framework para usuários e desenvolvedores [, consulte Introdução](index.md).

.NET Framework consiste na Common Language Runtime (CLR) e na biblioteca de classes .NET Framework. A Common Language Runtime é a base de .NET Framework. Pense no runtime como um agente que gerencia o código no runtime, fornecendo serviços principais como gerenciamento de memória, gerenciamento de threads e comunicação remota, enquanto também impõe a segurança de tipos rígida e outras formas de precisão de código que promovem segurança e robustez. Na verdade, o conceito de gerenciamento de código é um princípio fundamental do runtime. O código que segmenta o runtime é conhecido como código gerenciado e o código que não segmenta o runtime é conhecido como código não gerenciado. A biblioteca de classes é uma coleção abrangente e orientada a objetos de tipos reutilizáveis que você usa para desenvolver aplicativos que variam de aplicativos tradicionais de linha de comando ou GUI (interface gráfica do usuário) a aplicativos com base nas inovações mais recentes fornecidas pelo ASP.NET, como Web Forms e Web Services XML.

Os .NET Framework podem ser hospedados por componentes não gerenciados que carregam o Common Language Runtime em seus processos e iniciam a execução de código gerenciado, criando assim um ambiente de software que explora recursos gerenciados e não gerenciados. .NET Framework não apenas fornece vários hosts de tempo de execução, mas também oferece suporte ao desenvolvimento de hosts de tempo de execução de terceiros.

Por exemplo, o ASP.NET hospeda o runtime para fornecer um ambiente do servidor escalonável para código gerenciado. O ASP.NET trabalha diretamente com o tempo de execução para habilitar aplicativos ASP.NET e Web Services XML, ambos discutidos posteriormente neste artigo.

Internet Explorer é um exemplo de um aplicativo não gerenciado que hospeda o runtime (na forma de uma extensão tipo MIME). Usar o Internet Explorer para hospedar o runtime permite que você insira componentes gerenciados ou controles Windows Forms em documentos HTML. Hospedar o runtime dessa maneira torna possível o gerenciamento de código móvel, mas com melhorias significativas que somente o código gerenciado oferece, como execução semiconfiável e armazenamento isolado de arquivos.

A ilustração a seguir mostra o relacionamento do Common Language Runtime e da biblioteca de classes com seus aplicativos e com o sistema geral. A ilustração também mostra como o código gerenciado opera dentro uma arquitetura maior.

![Captura de tela que mostra como o código gerenciado opera dentro uma arquitetura maior.](./media/overview/language-runtime-class-library-relationship.gif)

As seções a seguir descrevem os principais recursos do .NET Framework mais detalhadamente.

## <a name="features-of-the-common-language-runtime"></a>Recursos do Common Language Runtime

O Common Language Runtime gerencia memória, execução de threads, execução de código, verificação de segurança do código, compilação e outros serviços do sistema. Esses recursos são intrínsecos ao código gerenciado, executado no Common Language Runtime.

Quanto à segurança, os componentes gerenciados recebem níveis de confiança variados, dependendo do número de fatores que incluem sua origem (como a Internet, a rede corporativa ou o computador local). Isso significa que um componente gerenciado pode ou não ser capaz de executar operações de acesso a arquivo, operações de acesso a Registro ou outras funções confidenciais, mesmo que esteja sendo usado no mesmo aplicativo ativo.

O runtime também impõe robustez ao código implementando uma infraestrutura rígida de verificação de tipo e código chamada CTS (Common Type System). O CTS assegura que todo código gerenciado seja autodescritivo. Os diversos compiladores de linguagem da Microsoft e de terceiros geram códigos gerenciados de acordo com o CTS. Isso significa que o código gerenciado pode consumir outros tipos e instâncias gerenciados, enquanto impõem rigorosamente a fidelidade e a segurança do tipo.

Além disso, o ambiente gerenciado do runtime elimina muitos problemas de software comuns. Por exemplo, o runtime identifica automaticamente o layout do objeto e gerencia referências a eles, liberando-os quando não estão mais sendo usados. Esse gerenciamento de memória automático resolve os dois erros mais comuns de aplicativo: perdas de memória e referências de memória inválidas.

O runtime também aumenta a produtividade do desenvolvedor. Por exemplo, os programadores escrevem aplicativos em sua linguagem de desenvolvimento preferida, mas aproveitam todo o runtime, a biblioteca de classes e os componentes escritos em outras linguagens por outros desenvolvedores. Qualquer fornecedor de compilador que optar por segmentar o runtime pode fazê-lo. Compiladores de linguagem que segmentam o .NET Framework disponibilizam os recursos do .NET Framework para códigos existentes, escritos nessa linguagem, facilitando bastante o processo de migração para aplicativos existentes.

Embora tenha sido criado para o software do futuro, o runtime também dá suporte ao software de hoje e de amanhã. Interoperabilidade entre códigos gerenciados e não gerenciados permite que os desenvolvedores continuem usando os componentes COM e DLLs necessários.

O runtime foi projetado para melhorar o desempenho. Embora o Common Language Runtime forneça vários serviços de tempo de execução padrão, o código gerenciado jamais é interpretado. Um recurso chamado compilação JIT (Just-In-Time) permite que todos os códigos gerenciados sejam executados na linguagem de computador nativa do sistema, na qual está em execução. Enquanto isso, o gerenciador de memória remove as possibilidades de memória fragmentada e aumenta a localidade de referência da memória, melhorando ainda mais o desempenho.

Por fim, o runtime pode ser hospedado por aplicativos do servidor de alto desempenho, como o Microsoft SQL Server e o IIS (Serviços de Informações da Internet). Essa infraestrutura permite que você use código gerenciado para escrever sua lógica de negócio, enquanto aproveita o desempenho superior dos melhores servidores de empresa que dão suporte à hospedagem de runtime.

## <a name="net-framework-class-library"></a>Biblioteca de classes .NET Framework

A biblioteca de classes .NET Framework é uma coleção de tipos reutilizáveis que se integram plenamente ao Common Language Runtime. A biblioteca de classes é orientada a objeto, fornecendo tipos dos quais seu próprio código gerenciado deriva funcionalidade. Isso não apenas torna os tipos do .NET Framework fáceis de usar, mas também reduz o tempo associado ao aprendizado de novos recursos do .NET Framework. Além disso, componentes de terceiros são totalmente integrados a classes do .NET Framework.

Por exemplo, as classes da coleção do .NET Framework implementam um conjunto de interfaces que você pode usar para desenvolver suas próprias coleções de classes. Sua coleção de classes se combina perfeitamente às classes do .NET Framework.

Como você esperaria de uma biblioteca de classes orientada a objeto, os tipos de .NET Framework permitem que você realize uma variedade de tarefas de programação comuns, incluindo gerenciamento de cadeia de caracteres, coleta de dados, conectividade de banco de dado e acesso a arquivos. Além dessas tarefas comuns, a biblioteca de classes inclui tipos que dão suporte a vários cenários de desenvolvimento especializados. Você pode usar .NET Framework para desenvolver os seguintes tipos de aplicativos e serviços:

- Aplicativos de console. Confira [Compilação de aplicativos de console](../../standard/building-console-apps.md).

- Aplicativos GUI do Windows (Windows Forms). Confira [Windows Forms](../winforms/index.md).

- Aplicativos WPF (Windows Presentation Foundation). Confira [Windows Presentation Foundation](../wpf/index.md).

- Aplicativos ASP.NET. Confira [Aplicativos Web com o ASP.NET](../develop-web-apps-with-aspnet.md).

- Serviços do Windows. Confira [Introdução a aplicativos do Serviço Windows](../windows-services/introduction-to-windows-service-applications.md).

- Aplicativos orientados a serviço usando o WCF (Windows Communication Foundation). Confira [Aplicativos orientados a serviço com WCF](../wcf/index.md).

- Aplicativos habilitados para fluxo de trabalho usando o Windows Workflow Foundation (WF). Confira [Windows Workflow Foundation](../windows-workflow-foundation/index.md).

As classes Windows Forms são um conjunto abrangente de tipos reutilizáveis que simplificam muito o desenvolvimento de GUI Windows. Se criar um aplicativo Web Form do ASP.NET, você poderá usar as classes Web Forms.

## <a name="see-also"></a>Consulte também

- [Requisitos do sistema](system-requirements.md)
- [Guia de instalação](../install/index.md)
- [Guia de desenvolvimento](../development-guide.md)
- [Ferramentas](../tools/index.md)
- [Exemplos e tutoriais do .NET](../../samples-and-tutorials/index.md)
- [Navegador de API do .NET](../../../api/index.md)
