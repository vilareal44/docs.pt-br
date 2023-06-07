---
title: Blazor modelos de Hospedagem de aplicativo
description: Conheça as diferentes maneiras de hospedar um Blazor aplicativo, incluindo no navegador no WebAssembly ou no servidor.
author: danroth27
ms.author: daroth
no-loc:
- Blazor
- WebAssembly
ms.date: 09/11/2019
ms.openlocfilehash: 2ebb021d2fce46a91a006227ccf9ba0cbcc5eea5
ms.sourcegitcommit: 0100be20fcf23f61dab672deced70059ed71bb2e
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/17/2020
ms.locfileid: "88267601"
---
# <a name="no-locblazor-app-hosting-models"></a>Blazor modelos de Hospedagem de aplicativo

Blazor os aplicativos podem ser hospedados no IIS, assim como ASP.NET Web Forms aplicativos. Blazor os aplicativos também podem ser hospedados de uma das seguintes maneiras:

- No lado do cliente no navegador em WebAssembly .
- No lado do servidor em um aplicativo ASP.NET Core.

## <a name="no-locblazor-no-locwebassembly-apps"></a>BlazorWebAssemblyaplicativos

BlazorWebAssemblyos aplicativos são executados diretamente no navegador em um WebAssembly tempo de execução .net baseado em. Blazoros WebAssembly aplicativos funcionam de forma semelhante às estruturas JavaScript de front-end, como angular ou reagir. No entanto, em vez de escrever JavaScript, você escreve C#. O tempo de execução do .NET é baixado com o aplicativo junto com o assembly do aplicativo e as dependências necessárias. Não são necessários plug-ins ou extensões de navegador.

Os assemblies baixados são assemblies normais do .NET, como você usaria em qualquer outro aplicativo .NET. Como o tempo de execução dá suporte a .NET Standard, você pode usar bibliotecas de .NET Standard existentes com seu Blazor WebAssembly aplicativo. No entanto, esses assemblies ainda serão executados na área restrita de segurança do navegador. Algumas funcionalidades podem gerar um <xref:System.PlatformNotSupportedException> , como tentar acessar o sistema de arquivos ou abrir conexões de rede arbitrárias.

Quando o aplicativo é carregado, o tempo de execução do .NET é iniciado e apontado para o assembly do aplicativo. A lógica de inicialização do aplicativo é executada e os componentes raiz são renderizados. Blazor calcula as atualizações da interface do usuário com base na saída renderizada dos componentes. As atualizações do DOM são aplicadas.

![::: no-Loc (mais alto):::::: no-Loc (Webassembly):::](media/hosting-models/blazor-webassembly.png)

Blazoros WebAssembly aplicativos executam puramente do lado do cliente. Esses aplicativos podem ser implantados em soluções de Hospedagem de site estáticos, como páginas do GitHub ou Hospedagem de sites estáticos do Azure. O .NET não é necessário no servidor. A vinculação profunda com partes do aplicativo normalmente requer uma solução de roteamento no servidor. A solução de roteamento redireciona as solicitações para a raiz do aplicativo. Por exemplo, esse redirecionamento pode ser tratado usando regras de reescrita de URL no IIS.

Para obter todos os benefícios do Blazor desenvolvimento para a Web .net de pilha completa, hospede seu Blazor WebAssembly aplicativo com ASP.NET Core. Usando o .NET no cliente e no servidor, você pode compartilhar facilmente o código e criar seu aplicativo usando um conjunto consistente de linguagens, estruturas e ferramentas. Blazor fornece modelos convenientes para configurar uma solução que contenha um Blazor WebAssembly aplicativo e um projeto de host ASP.NET Core. Quando a solução é criada, os arquivos estáticos internos do Blazor aplicativo são hospedados pelo aplicativo ASP.NET Core com o roteamento de fallback já configurado.

## <a name="no-locblazor-server-apps"></a>Blazor Aplicativos de servidor

Lembre-se da discussão sobre [ Blazor arquitetura](architecture-comparison.md#blazor) que Blazor os componentes renderizam sua saída para uma abstração intermediária chamada a `RenderTree` . BlazorEm seguida, a estrutura compara o que foi renderizado com o que foi renderizado anteriormente. As diferenças são aplicadas ao DOM. Blazor os componentes são dissociados de como a saída renderizada é aplicada. Consequentemente, os próprios componentes não precisam ser executados no mesmo processo que o processo que atualiza a interface do usuário. Na verdade, eles nem precisam ser executados no mesmo computador.

Em Blazor aplicativos de servidor, os componentes são executados no servidor em vez de no lado do cliente no navegador. Os eventos de interface do usuário que ocorrem no navegador são enviados para o servidor em uma conexão em tempo real. Os eventos são expedidos para as instâncias de componente corretas. Os componentes são renderizados e a comparação de interface do usuário calculada é serializada e enviada ao navegador onde é aplicada ao DOM.

![::: no-Loc (mais alto)::: servidor](media/hosting-models/blazor-server.png)

O Blazor modelo de hospedagem do servidor pode parecer familiar se você usou o ASP.NET AJAX e o <xref:System.Web.UI.UpdatePanel> controle. O `UpdatePanel` controle manipula a aplicação de atualizações de página parcial em resposta a eventos de gatilho na página. Quando disparado, o `UpdatePanel` solicita uma atualização parcial e, em seguida, a aplica sem a necessidade de atualizar a página. O estado da interface do usuário é gerenciado usando `ViewState` . Blazor Os aplicativos de servidor são ligeiramente diferentes, pois o aplicativo requer uma conexão ativa com o cliente. Além disso, todo o estado da interface do usuário é mantido no servidor. Além dessas diferenças, os dois modelos são conceitualmente semelhantes.

## <a name="how-to-choose-the-right-no-locblazor-hosting-model"></a>Como escolher o modelo de Blazor hospedagem correto

Conforme descrito nos [ Blazor documentos do modelo de hospedagem](/aspnet/core/blazor/hosting-models), os diferentes modelos de Blazor hospedagem têm compensações diferentes.

O Blazor WebAssembly modelo de hospedagem tem os seguintes benefícios:

- Não há nenhuma dependência do lado do servidor .NET. O aplicativo está totalmente funcionando depois de baixado para o cliente.
- Recursos e funcionalidades do cliente são totalmente aproveitados.
- O trabalho é descarregado do servidor para o cliente.
- Um servidor Web ASP.NET Core não é necessário para hospedar o aplicativo. Cenários de implantação sem servidor são possíveis (por exemplo, servindo o aplicativo de uma CDN).

As desvantagens do modelo de Blazor WebAssembly hospedagem são:

- Os recursos do navegador restringem o aplicativo.
- O hardware e o software compatíveis do cliente (por exemplo, WebAssembly suporte) são necessários.
- O tamanho do download é maior e os aplicativos demoram mais para serem carregados.
- O suporte ao tempo de execução e às ferramentas do .NET é menos maduro. Por exemplo, há limitações no suporte e na depuração de [.net Standard](../../standard/net-standard.md) .

Por outro lado, o Blazor modelo de Hospedagem de servidor oferece os seguintes benefícios:

- O tamanho do download é muito menor do que um aplicativo do lado do cliente e o aplicativo é carregado muito mais rapidamente.
- O aplicativo aproveita totalmente os recursos do servidor, incluindo o uso de qualquer API compatível com o .NET Core.
- O .NET Core no servidor é usado para executar o aplicativo, portanto, as ferramentas .NET existentes, como depuração, funcionam conforme o esperado.
- Há suporte para clientes finos. Por exemplo, aplicativos do lado do servidor funcionam com navegadores que não dão suporte a WebAssembly e em dispositivos com restrição de recursos.
- A base de código .NET/C# do aplicativo, incluindo o código de componente do aplicativo, não é servida aos clientes.

As desvantagens do modelo de Blazor hospedagem do servidor são:

- Maior latência de interface do usuário. Cada interação do usuário envolve um salto de rede.
- Não há suporte offline. Se a conexão do cliente falhar, o aplicativo para de funcionar.
- A escalabilidade é desafiadora para aplicativos com muitos usuários. O servidor deve gerenciar várias conexões de cliente e manipular o estado do cliente.
- Um servidor de ASP.NET Core é necessário para atender ao aplicativo. Cenários de implantação sem servidor não são possíveis. Por exemplo, você não pode servir o aplicativo de uma CDN.

A lista anterior de compensações pode ser intimidadora, mas seu modelo de hospedagem pode ser alterado posteriormente. Independentemente do Blazor modelo de hospedagem selecionado, o modelo de componente é *o mesmo*. Em princípio, os mesmos componentes podem ser usados com o modelo de hospedagem. O código do aplicativo não é alterado; no entanto, é uma boa prática introduzir abstrações para que seus componentes fiquem de hospedagem independente de modelo. As abstrações permitem que seu aplicativo adote com mais facilidade um modelo de hospedagem diferente.

## <a name="deploy-your-app"></a>Implante seu aplicativo

Os aplicativos de Web Forms ASP.NET normalmente são hospedados no IIS em um computador ou cluster do Windows Server. Blazor os aplicativos também podem:

- Ser hospedado no IIS, seja como arquivos estáticos ou como um aplicativo ASP.NET Core.
- Aproveite a flexibilidade de ASP.NET Core para ser hospedada em várias plataformas e infraestruturas de servidor. Por exemplo, você pode hospedar um Blazor aplicativo usando [Nginx](/aspnet/core/host-and-deploy/linux-nginx) ou [Apache](/aspnet/core/host-and-deploy/linux-apache) no Linux. Para obter mais informações sobre como publicar e implantar Blazor aplicativos, consulte a Blazor documentação de [hospedagem e implantação](/aspnet/core/host-and-deploy/blazor/) .

Na próxima seção, veremos como os projetos para Blazor WebAssembly e os aplicativos de Blazor servidor são configurados.

>[!div class="step-by-step"]
>[Anterior](architecture-comparison.md) 
> [Avançar](project-structure.md)
