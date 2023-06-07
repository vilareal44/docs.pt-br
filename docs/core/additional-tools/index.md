---
title: Ferramentas adicionais
description: Uma visão geral das ferramentas adicionais que você pode instalar que oferecem suporte e estendem a funcionalidade do .NET Core.
author: mlacouture
ms.date: 02/13/2020
ms.custom: mvc
ms.openlocfilehash: f7bfa660f7521adf4950d5bbdd59628bb88cca4d
ms.sourcegitcommit: 8bfeb5930ca48b2ee6053f16082dcaf24d46d221
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/18/2020
ms.locfileid: "88557926"
---
# <a name="net-core-additional-tools-overview"></a>Visão geral das ferramentas adicionais do .NET Core

Esta seção compila uma lista de ferramentas que dão suporte e ampliam a funcionalidade do .NET Core, além da CLI do .NET Core.

## <a name="net-core-uninstall-tool"></a>Ferramenta de Desinstalação do .NET Core

A [ferramenta de desinstalação do .NET Core](https://github.com/dotnet/cli-lab/releases) ( `dotnet-core-uninstall` ) permite que você limpe SDKs e tempos de execução do .NET Core em um sistema, de modo que apenas as versões especificadas permaneçam. Uma coleção de opções está disponível para especificar quais versões são desinstaladas.

## <a name="net-core-diagnostic-tools"></a>Ferramentas de diagnóstico do .NET Core

[dotnet-os contadores](../diagnostics/dotnet-counters.md) são uma ferramenta de monitoramento de desempenho para a investigação de desempenho e monitoramento de integridade de primeiro nível.

[dotnet-o despejo](../diagnostics/dotnet-dump.md) fornece uma maneira de coletar e analisar despejos do Windows e do Linux Core sem um depurador nativo.

[dotnet-gcdump](../diagnostics/dotnet-gcdump.md) fornece uma maneira de coletar despejos de GC (coletor de lixo) de processos do .net em tempo real.

[dotnet-Trace](../diagnostics/dotnet-trace.md) coleta dados de criação de perfil de seu aplicativo que podem ajudar em cenários em que você precisa descobrir o que faz com que um aplicativo seja executado lentamente.

## <a name="wcf-web-service-reference-tool"></a>Ferramenta de referência do serviço Web WCF

A ferramenta de referência do [serviço Web](wcf-web-service-reference-guide.md) WCF (Windows Communication Foundation) é um provedor de serviço conectado do Visual Studio que fez sua estréia no [Visual Studio 2017 versão 15,5](/visualstudio/releasenotes/vs2017-relnotes-v15.5#WCFTools). Essa ferramenta recupera metadados de um serviço Web na solução atual, em um local de rede ou em um arquivo WSDL. Ele gera um arquivo de origem compatível com o .NET Core, definindo uma classe proxy WCF com métodos que você pode usar para acessar as operações do serviço Web.

## <a name="wcf-dotnet-svcutil-tool"></a>Ferramenta dotnet-svcutil do WCF

A ferramenta do WCF [dotnet-SvcUtil](dotnet-svcutil-guide.md) é uma ferramenta .NET que recupera metadados de um serviço Web em um local de rede ou em um arquivo WSDL. Ele gera um arquivo de origem compatível com o .NET Core, definindo uma classe proxy WCF com métodos que você pode usar para acessar as operações do serviço Web.

A ferramenta **dotnet-SvcUtil** é uma alternativa para o provedor de serviços conectados do [**WCF Web Service Reference**](wcf-web-service-reference-guide.md) do Visual Studio, que foi enviado pela primeira vez com o Visual Studio 2017 versão 15,5. A ferramenta **dotnet-SvcUtil** , como uma ferramenta .net, está disponível no Linux, no MacOS e no Windows.

## <a name="wcf-dotnet-svcutilxmlserializer-tool"></a>Ferramenta dotnet-svcutil.xmlserializer da WCF

No .NET Framework, é possível gerar previamente um assembly de serialização usando a ferramenta svcutil. A [ ferramenta do serializadordotnet-svcutil.xml](dotnet-svcutil.xmlserializer-guide.md) do WCF fornece funcionalidade semelhante no .NET Core. Ele gera previamente o código de serialização C# para os tipos no aplicativo cliente, que são usados pelo Contrato de Serviço da WCF e podem ser serializados pelo <xref:System.Xml.Serialization.XmlSerializer>. Isso melhora o desempenho de inicialização da serialização de XML ao serializar ou desserializar objetos desses tipos.

## <a name="xml-serializer-generator"></a>Gerador de serializador de XML

Como o [XML Serializer Generator (sgen.exe)](../../standard/serialization/xml-serializer-generator-tool-sgen-exe.md) para o .NET Framework, o [pacote NuGet Microsoft.XmlSerializer.Generator](https://www.nuget.org/packages/Microsoft.XmlSerializer.Generator) é a solução para bibliotecas .NET Core e .NET Standard. Ele cria um assembly de serialização de XML para tipos contidos em um assembly a fim de melhorar o desempenho de inicialização da serialização de XML ao serializar ou desserializar objetos desses tipos usando <xref:System.Xml.Serialization.XmlSerializer>.
