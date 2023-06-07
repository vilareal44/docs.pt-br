---
title: gRPC
description: Saiba mais sobre o gRPC, sua função em aplicativos nativos de nuvem e como ele difere da comunicação HTTP RESTful.
author: robvet
no-loc:
- Blazor
- Blazor WebAssembly
ms.date: 05/13/2020
ms.openlocfilehash: 4a0c88472d2b19efb2ff0f58395003b1b6409131
ms.sourcegitcommit: ef50c99928183a0bba75e07b9f22895cd4c480f8
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/07/2020
ms.locfileid: "87914897"
---
# <a name="grpc"></a>gRPC

Até agora neste livro, nos concentramos na comunicação [baseada em REST](https://docs.microsoft.com/azure/architecture/best-practices/api-design) . Vimos que REST é um estilo de arquitetura flexível que define operações baseadas em CRUD contra recursos de entidade. Os clientes interagem com recursos no HTTP com um modelo de comunicação de solicitação/resposta. Embora o REST seja amplamente implementado, uma tecnologia de comunicação mais nova, gRPC, ganhou uma enorme impulso na Comunidade nativa de nuvem.

## <a name="what-is-grpc"></a>O que é o gRPC?

o gRPC é uma estrutura moderna de alto desempenho que evolui o protocolo [RPC (chamada de procedimento remoto)](https://en.wikipedia.org/wiki/Remote_procedure_call) antigo. No nível do aplicativo, o gRPC simplifica mensagens entre clientes e serviços de back-end. Originado do Google, o gRPC é um software livre e parte do ecossistema [CNCF (Cloud Native Computing Foundation)](https://www.cncf.io/) de ofertas nativas de nuvem. CNCF considera gRPC um [projeto incubating](https://github.com/cncf/toc/blob/master/process/graduation_criteria.adoc). Incubating significa que os usuários finais estão usando a tecnologia em aplicativos de produção e o projeto tem um número íntegro de colaboradores.

Um aplicativo cliente gRPC típico exporá uma função local em processo que implementa uma operação de negócios. Nos bastidores, essa função local invoca outra função em um computador remoto. O que parece ser uma chamada local essencialmente se torna uma chamada fora do processo transparente para um serviço remoto. O direcionamento de RPC abstrai a comunicação de rede ponto a ponto, a serialização e a execução entre computadores.

Em aplicativos nativos de nuvem, os desenvolvedores costumam trabalhar em linguagens de programação, estruturas e tecnologias. Essa *interoperabilidade* complica os contratos de mensagem e o direcionamento necessário para comunicação entre plataformas.  o gRPC fornece uma "camada horizontal uniforme" que abstrai essas preocupações. O código dos desenvolvedores em sua plataforma nativa concentra-se na funcionalidade de negócios, enquanto o gRPC lida com o direcionamento de comunicação.

o gRPC oferece suporte abrangente entre as pilhas de desenvolvimento mais populares, incluindo Java, JavaScript, C#, go, Swift e NodeJS.

## <a name="grpc-benefits"></a>Benefícios do gRPC

gRPC usa HTTP/2 para seu protocolo de transporte. Embora seja compatível com HTTP 1,1, o HTTP/2 apresenta muitos recursos avançados:

- Um protocolo de enquadramento binário para transporte de dados-ao contrário de HTTP 1,1, que é baseado em texto.
- O suporte à multiplexação para enviar várias solicitações paralelas pela mesma conexão-HTTP 1,1 limita o processamento a uma mensagem de solicitação/resposta por vez.
- Comunicação bidirecional Full duplex para enviar solicitações de cliente e respostas de servidor simultaneamente.
- Streaming interno permitindo solicitações e respostas para transmitir conjuntos de dados grandes de forma assíncrona.
- Compactação de cabeçalho que reduz o uso da rede.

o gRPC é leve e tem alto desempenho. Pode ser até 8x mais rápido que a serialização JSON com mensagens 60-80% menores. Na linguagem Microsoft [Windows Communication Foundation (WCF)](https://docs.microsoft.com/dotnet/framework/wcf/whats-wcf) , o desempenho do gRPC excede a velocidade e a eficiência das [associações NetTcp](https://docs.microsoft.com/dotnet/api/system.servicemodel.nettcpbinding?view=netframework-4.8)altamente otimizadas. Diferentemente do NetTCP, que favorece o Microsoft Stack, o gRPC é uma plataforma cruzada.

## <a name="protocol-buffers"></a>Buffers de protocolo

o gRPC adota uma tecnologia de código aberto chamada [buffers de protocolo](https://developers.google.com/protocol-buffers/docs/overview). Eles fornecem um formato de serialização altamente eficiente e com neutralidade de plataforma para serializar mensagens estruturadas que os serviços enviam entre si. Usando uma IDL (linguagem de definição de interface) de plataforma cruzada, os desenvolvedores definem um contrato de serviço para cada microserviço. O contrato, implementado como um arquivo baseado em texto `.proto` , descreve os métodos, as entradas e as saídas de cada serviço. O mesmo arquivo de contrato pode ser usado para clientes e serviços gRPC criados em diferentes plataformas de desenvolvimento.

Usando o arquivo proto, o compilador Protobuf, `protoc` , gera o código de cliente e de serviço para sua plataforma de destino. O código inclui os seguintes componentes:

- Objetos fortemente tipados, compartilhados pelo cliente e serviço, que representam as operações de serviço e os elementos de dados de uma mensagem.
- Uma classe base fortemente tipada com o direcionamento de rede necessário que o serviço gRPC remoto pode herdar e estender.
- Um stub de cliente que contém o encanamento necessário para invocar o serviço gRPC remoto.

Em tempo de execução, cada mensagem é serializada como uma representação Protobuf padrão e trocada entre o cliente e o serviço remoto. Ao contrário de JSON ou XML, as mensagens Protobuf são serializadas como bytes binários compilados.

O livro, [gRPC para desenvolvedores do WCF](https://docs.microsoft.com/dotnet/architecture/grpc-for-wcf-developers/), disponível no site de arquitetura da Microsoft, fornece uma cobertura detalhada de GRPC e buffers de protocolo.

## <a name="grpc-support-in-net"></a>suporte do gRPC no .NET

o gRPC é integrado ao SDK do .NET Core 3,0 e posterior. As ferramentas a seguir dão suporte a ela:

- Visual Studio 2019, versão 16,3 ou posterior, com a carga de trabalho de desenvolvimento da Web instalada.
- Visual Studio Code
- a CLI do dotnet

O SDK inclui ferramentas para roteamento de ponto de extremidade, IoC interno e registro em log. O servidor Web Kestrel de código aberto dá suporte a conexões HTTP/2. A Figura 4-20 mostra um modelo do Visual Studio 2019 que aplica Scaffold um projeto de esqueleto para um serviço gRPC. Observe como o .NET Core dá suporte total ao Windows, Linux e macOS.

![Suporte do gRPC no Visual Studio 2019](./media/visual-studio-2019-grpc-template.png)

**Figura 4-20**. suporte do gRPC no Visual Studio 2019
  
A Figura 4-21 mostra o esqueleto do serviço gRPC gerado a partir do scaffolding interno incluído no Visual Studio 2019.  

![projeto gRPC no Visual Studio 2019](./media/grpc-project.png  )

**Figura 4-21**. projeto gRPC no Visual Studio 2019

Na figura anterior, observe o arquivo de descrição proto e o código do serviço. Como você verá em breve, o Visual Studio gera configuração adicional tanto na classe de inicialização quanto no arquivo de projeto subjacente.

## <a name="grpc-usage"></a>uso de gRPC

Favorecer o gRPC para os seguintes cenários:

- Comunicação de microserviço para microserviço de back-end síncrona em que uma resposta imediata é necessária para continuar o processamento.
- Ambientes poliglota que precisam dar suporte a plataformas de programação mista.
- Comunicação de baixa latência e alta taxa de transferência em que o desempenho é crítico.
- Comunicação em tempo real de ponto a ponto – o gRPC pode enviar mensagens em tempo real sem sondagem e tem excelente suporte para streaming bidirecional.
- Ambientes com restrição de rede – as mensagens gRPC binárias são sempre menores do que uma mensagem JSON equivalente com base em texto.

No momento da redação deste artigo, o gRPC é usado principalmente com os serviços de back-end. Os navegadores modernos não podem fornecer o nível de controle HTTP/2 necessário para dar suporte a um cliente gRPC de front-end. Dito isso, há suporte para [gRPC-Web com .net](https://devblogs.microsoft.com/aspnet/grpc-web-for-net-now-available/) que permite a comunicação gRPC de aplicativos baseados em navegador criados com JavaScript ou Blazor WebAssembly tecnologias. [gRPC-Web](https://github.com/grpc/grpc/blob/master/doc/PROTOCOL-WEB.md) permite que um aplicativo ASP.NET Core gRPC dê suporte aos recursos do gRPC em aplicativos de navegador:

- Clientes com rigidez de tipos e gerados por código
- Compactar mensagens Protobuf
- Transmissão de servidor

## <a name="grpc-implementation"></a>implementação de gRPC

A arquitetura de referência de microserviço, [eshop em contêineres](https://github.com/dotnet-architecture/eShopOnContainers), da Microsoft, mostra como implementar serviços gRPC em aplicativos .NET Core. A Figura 4-22 apresenta a arquitetura de back-end.

![Arquitetura de back-end para eShop em contêineres](./media/eshop-with-aggregators.png)

**Figura 4-22**. Arquitetura de back-end para eShop em contêineres

Na figura anterior, observe como o eShop adota o [back-end para o padrão de front-ends](https://docs.microsoft.com/azure/architecture/patterns/backends-for-frontends) (BFF) expondo vários gateways de API. Discutimos o padrão BFF anteriormente neste capítulo. Preste muita atenção ao microserviço do agregador (em cinza) que se situa entre o gateway da API de compra da Web e os microserviços de compra de back-end. O agregador recebe uma única solicitação de um cliente, a distribui para vários microserviços, agrega os resultados e os envia de volta para o cliente solicitante. Essas operações normalmente exigem comunicação síncrona para produzir uma resposta imediata. No eShop, as chamadas de back-end do agregador são executadas usando gRPC, conforme mostrado na Figura 4-23.

![gRPC em eShop em contêineres](./media/grpc-implementation.png)

**Figura 4-23**. gRPC em eShop em contêineres

a comunicação gRPC requer componentes de cliente e de servidor. Na figura anterior, observe como o agregador de compras implementa um cliente gRPC. O cliente faz chamadas gRPC síncronas (em vermelho) para os microserviços de back-end, cada uma implementando um servidor gRPC. Tanto o cliente quanto o servidor tiram proveito do gRPC integrado da SDK do .NET Core. Os *stubs* do lado do cliente fornecem o encanamento para invocar chamadas gRPC remotas. Os componentes do lado do servidor fornecem o direcionamento gRPC que as classes de serviço personalizadas podem herdar e consumir.

Os microserviços que expõem uma API RESTful e a comunicação gRPC exigem vários pontos de extremidade para gerenciar o tráfego. Você abriria um ponto de extremidade que escuta o tráfego HTTP para as chamadas RESTful e outro para chamadas gRPC. O ponto de extremidade gRPC deve ser configurado para o protocolo HTTP/2 que é necessário para a comunicação gRPC.

Embora possamos nos esforçarmos para desassociar os microserviços com padrões de comunicação assíncrona, algumas operações exigem chamadas diretas. gRPC deve ser a escolha primária para a comunicação síncrona direta entre os microserviços. Seu protocolo de comunicação de alto desempenho, com base em buffers de protocolo e HTTP/2, torna-o uma opção perfeita.

## <a name="looking-ahead"></a>Olhando para o futuro

Olhando para o futuro, o gRPC continuará a obter força para sistemas nativos de nuvem. Os benefícios de desempenho e a facilidade de desenvolvimento são atraentes. No entanto, o REST provavelmente estará por muito tempo. Ele é Excel para APIs expostas publicamente e por motivos de compatibilidade com versões anteriores.

>[!div class="step-by-step"]
>[Anterior](service-to-service-communication.md) 
> [Avançar](service-mesh-communication-infrastructure.md)
