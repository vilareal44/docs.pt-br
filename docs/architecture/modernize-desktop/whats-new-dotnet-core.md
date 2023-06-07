---
title: Novidades do .NET Core para Área de Trabalho
description: Saiba mais sobre o .NET Core, as diferenças entre o .NET Core e o .NET Framework e os novos recursos que foram adicionados.
ms.date: 05/12/2020
ms.openlocfilehash: b4fc0cb2841fe13b000223aefc5eaf63bd911994
ms.sourcegitcommit: ee5b798427f81237a3c23d1fd81fff7fdc21e8d3
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/28/2020
ms.locfileid: "84144259"
---
# <a name="whats-new-with-net-core-for-desktop"></a>Novidades do .NET Core para Área de Trabalho

A partir do .NET Core 3,0, o .NET Core dá suporte ao Windows Forms e ao WPF. Então, agora você tem uma opção entre .NET Framework e o .NET Core para seus aplicativos de desktop. Este capítulo descreve o que é o .NET Core e quais são seus benefícios para aplicativos de área de trabalho.

## <a name="the-motivation-behind-net-core"></a>A motivação por trás do .NET Core

Desde sua inicialização em 2002, a .NET Framework evoluiu durante os anos para dar suporte a muitas tecnologias como Windows Forms, ASP.NET, Entity Framework, Windows Store e muitos outros. Todos eles são diferentes por natureza. Portanto, a Microsoft estava se aproximando dessa evolução fazendo partes do .NET Framework e criando uma pilha de aplicativos diferente para cada tecnologia. Dessa forma, os recursos de desenvolvimento podem ser personalizados para as necessidades da pilha específica, o que maximizava o potencial de cada plataforma. Isso leva à fragmentação nas versões do .NET Framework mantidas por diferentes equipes independentes. Todas essas pilhas têm uma estrutura comum, contendo um modelo de aplicativo, uma estrutura e um tempo de execução, mas elas diferem na implementação de cada uma dessas partes.

Se você estiver direcionando apenas uma dessas plataformas, poderá usar esse modelo. No entanto, em muitos casos, talvez seja necessário mais de uma plataforma de destino na mesma solução. Por exemplo, seu aplicativo pode ter uma parte de administrador da área de trabalho, um site voltado para o cliente que compartilha a lógica de back-end em execução em um servidor e até mesmo um cliente móvel. Nesse caso, você precisa de uma experiência de codificação unificada que possa abranger todas essas verticais do .NET.

No momento em que o Windows 8 foi lançado, o conceito de PCLs (bibliotecas de classe portátil) nasceu. Originalmente, a .NET Framework foi projetada em contraposição de que ela sempre seria implantada como uma única unidade, portanto, a [fatoração](https://wikipedia.org/wiki/Decomposition_(computer_science)) não era uma preocupação. Para enfrentar o problema do compartilhamento de código entre os verticais, a força de condução estava em como refatorar a estrutura. A ideia dos contratos é fornecer uma área de superfície de API bem fatorada. Os contratos são simplesmente assemblies que você compila em relação ao e são projetados com a fatoração adequada em mente, tomando cuidado com as dependências entre eles.

Isso leva a um raciocínio sobre as diferenças de API entre os verticais no nível do assembly, em oposição ao nível de API individual que tínhamos antes. Esse aspecto habilitou uma experiência de biblioteca de classes que pode ser direcionada a várias verticais, também conhecidas como bibliotecas de classes portáteis.

![Histórico de liberação de .NET Framework](./media/whats-new-dotnet-core/release-history.png)

Com o PCL, a experiência de desenvolvimento é unificada entre os verticais com base na forma da API. E a necessidade mais urgente de criar bibliotecas em execução em diferentes verticais também é abordada. Mas há um grande desafio: as APIs são apenas portáteis quando a implementação é movida para frente em todas as verticais.

Uma abordagem melhor é unificar as implementações entre as verticais fornecendo uma implementação bem fatorada em vez de uma exibição bem fatorada. É muito mais simples perguntar a cada equipe que possui um componente específico para pensar sobre como suas APIs funcionam em todas as verticais do que tentar fornecer retroativamente uma pilha de API consistente na parte superior. É aí que entra o .NET Standard. Consulte os detalhes na próxima seção.

Outro grande desafio tem a ver com o modo como o .NET Framework é implantado. A .NET Framework é uma estrutura de todo o computador. Todas as alterações feitas a ele afetam todos os aplicativos que assumem uma dependência. Embora esse modelo de implantação tenha muitas vantagens, como reduzir o espaço em disco e o acesso centralizado aos serviços, ele apresenta algumas armadilhas.

Para começar, é difícil para os desenvolvedores de aplicativos assumirem uma dependência de uma estrutura lançada recentemente. Eles precisam assumir uma dependência do sistema operacional mais recente ou fornecer um instalador de aplicativo que instale o .NET Framework junto com o aplicativo. Se você for um desenvolvedor da Web, talvez nem tenha essa opção, já que o departamento de ti estabelece a versão com suporte do servidor.

Mesmo que você esteja disposto a se deparar com o problema de fornecer um instalador para encadear a instalação do .NET Framework, talvez descubra que a atualização do .NET Framework pode interromper outros aplicativos.

Apesar dos esforços para fornecer versões anteriores compatíveis da estrutura, há alterações compatíveis que podem interromper aplicativos. Por exemplo, a adição de uma interface a um tipo existente pode alterar a forma como esse tipo é serializado e causar problemas de interrupção dependendo do código existente. Como a base instalada do .NET Framework é enorme, combater esses cenários de interrupção reduz o ritmo das inovações dentro do .NET Framework.

Para resolver todos esses problemas, a Microsoft desenvolveu o .NET Core para abordar a evolução da plataforma .NET.

## <a name="introduction-to-net-core"></a>Introdução ao .NET Core

O .NET Core é a evolução da tecnologia .NET da Microsoft em uma plataforma modular, de plataforma cruzada, de software livre e pronta para a nuvem. Ele é executado no Windows, no macOS e no Linux com planos para também ser executado em arquiteturas baseadas em ARM, como Android e IoT.

A finalidade do .NET Core é fornecer uma plataforma unificada para todos os tipos de aplicativos, que inclui Windows, plataforma cruzada e aplicativos móveis. [.Net Standard](../../standard/net-standard.md) habilita isso fornecendo APIs base compartilhadas, que cada modelo de aplicativo precisa e excluindo qualquer API específica de modelo de aplicativo.

Essa estrutura oferece aos aplicativos muitos benefícios em termos de eficiência e desempenho, simplificando o empacotamento e a implantação nas diferentes plataformas com suporte.

Os benefícios do .NET Core vêm dessas três características:

- **Plataforma cruzada:** Ele permite a execução de aplicativos em diferentes plataformas (Windows, macOS e Linux).
- Software livre **:** a plataforma .NET Core é uma fonte aberta e está disponível por meio do GitHub, incentivando as contribuições de transparência e da Comunidade.
- **Com suporte:** A Microsoft oficialmente dá suporte ao .NET Core.

A partir do .NET Core 3,0, além do suporte existente para Web e nuvem, há também suporte para domínios de desktop, IoT e ia. O objetivo dessa estrutura é impressionante: para direcionar cada tipo de desenvolvimento do .NET presente e futuro. A Microsoft planeja concluir essa visão com o .NET 5 no final de 2020. O nome "principal" foi removido para reforçar sua exclusividade no mundo do .NET.

![Todos os domínios do .NET 5](./media/whats-new-dotnet-core/all-domains-of-dotnet5.png)

## <a name="net-framework-vs-net-core"></a>.NET Framework vs. .NET Core

Agora que você entende a relevância do .NET Core dentro da estratégia da Microsoft para .NET, você deve estar se perguntando o que acontece com .NET Framework. Você pode estar fazendo perguntas como: você precisa abandonar? Vai desaparecer? Quais são as minhas opções para modernizar os aplicativos que tenho em .NET Framework?

Em 2019, a última versão do **.NET Framework-4,8** foi lançada. Ele incluía três aprimoramentos importantes para aplicativos de desktop:

- **Controles de mídia e navegador modernos**: hoje, os aplicativos de área de trabalho .NET usam o Internet Explorer e o Windows Media Player para mostrar HTML e reproduzir arquivos de mídia. Como esses controles herdados não mostram o HTML mais recente ou reproduzem os arquivos de mídia mais recentes, foram adicionados novos controles que tiram proveito do Microsoft Edge e de media players mais recentes que dão suporte aos padrões mais recentes.
- **Acesso aos controles UWP**: o UWP contém novos controles que aproveitam os recursos e telas de toque mais recentes do Windows. Você não precisa reescrever seus aplicativos para usar esses novos recursos e controles, para que você possa usar esses novos recursos em seu código do WPF ou Windows Forms existente.
- **Aprimoramentos de DPI alto**: a resolução de monitores está aumentando para as resoluções de 4K e 8K. Portanto, .NET Framework 4,8 adiciona novas melhorias de HDPI para garantir que seus aplicativos existentes Windows Forms e WPF possam ser ótimos nessas novas exibições.

Como o .NET Framework é instalado em milhões de computadores, a Microsoft continuará a dar suporte a ele, mas não adicionará novos recursos.

O .NET Core é a versão de software livre, entre plataformas e de movimentação rápida do .NET. Devido à sua natureza lado a lado, ele pode fazer alterações sem o medo de interromper qualquer aplicativo. Isso significa que o .NET Core receberá novas APIs e recursos de linguagem ao longo do tempo que .NET Framework não. Além disso, o **.NET Core** já tem recursos que eram impossíveis para .NET Framework, como:

- **Versões lado a lado do .NET com suporte para Windows Forms e WPF**: isso resolve o problema de efeitos colaterais ao atualizar a versão da estrutura da máquina. Várias versões do .NET Core podem ser instaladas no mesmo computador e cada aplicativo especifica qual versão do .NET Core deve ser usada. Ainda mais, agora você pode desenvolver e executar o Windows Forms e o WPF sobre o .NET Core.
- **Inserir o .net diretamente em um aplicativo**: você pode implantar o .NET Core como parte do seu pacote de aplicativos. Isso permite que você aproveite a versão, os recursos e as APIs mais recentes sem precisar aguardar a instalação de uma versão específica no computador.
- Aproveite os **recursos do .NET Core**: o .NET Core é a versão de software livre de movimentação rápida do .net. Sua natureza lado a lado permite uma introdução rápida de novas APIs inovadoras e aprimoramentos de BCL (bibliotecas de classes base) sem o risco de perder a compatibilidade. Agora, os aplicativos Windows Forms e WPF podem aproveitar os recursos mais recentes do .NET Core, que também incluem correções mais fundamentais para o desempenho do tempo de execução, suporte a alto DPI e assim por diante.

Uma parte essencial do roteiro para a Microsoft era facilitar os desenvolvedores a migrar aplicativos para o .NET Core e, no futuro, para o .NET 5. Mas se você tiver aplicativos .NET Framework existentes, não se sentirá pressão para mudar para o .NET Core. .NET Framework terá suporte total e sempre fará parte do Windows. No entanto, se você quiser usar os recursos de linguagem mais recentes e as APIs no futuro, precisará mover seus aplicativos para o .NET Core.

Para seus aplicativos de área de trabalho novos, recomendamos iniciar diretamente no .NET Core. É leve e de plataforma cruzada, é executada lado a lado, tem alto desempenho e se adapta perfeitamente a arquiteturas de contêineres e de microserviços.

![Você pode atualizar seus aplicativos .NET Framework usando a versão mais recente do .NET Framework ou portar seus aplicativos para o .NET Core](./media/whats-new-dotnet-core/framework-vs-core.png)

## <a name="net-standard-vs-pcl"></a>.NET Standard vs. PCL

O [.NET Standard](../../standard/net-standard.md) é uma especificação formal de APIs do .NET que devem estar disponíveis em todas as implementações do .NET. A motivação por trás do .NET Standard é estabelecer maior uniformidade no ecossistema do .NET. .NET Standard é uma especificação de APIs do .NET que compõem um conjunto uniforme de contratos para compilar seu código. Esses contratos são implementados em cada tipo de .NET, permitindo assim a portabilidade entre diferentes implementações do .NET.

O .NET Standard permite os seguintes principais cenários:

- Define o conjunto uniforme de APIs de bibliotecas de classes base para que todas as implementações do .NET implementem, independentemente da carga de trabalho.
- Permite que os desenvolvedores criem bibliotecas portáteis, utilizáveis entre implementações do .NET, usando esse mesmo conjunto de APIs.

.NET Standard é a evolução do PCLs e a lista a seguir mostra as diferenças fundamentais entre .NET Standard e PCLs:

- .NET Standard é um conjunto de APIs organizadas, escolhido pela Microsoft. PCLs não são.
- As APIs que um PCL contém dependem das plataformas que você optar por direcionar ao criá-lo. Isso torna um PCL somente compartilhável para os destinos específicos que você escolher.
- .NET Standard é independente de plataforma, ele pode ser executado em qualquer lugar, no Windows, no macOS, no Linux e assim por diante.
- O PCLs também pode executar a plataforma cruzada, mas eles têm um alcance mais limitado. PCLs só pode visar um conjunto limitado de plataformas.

## <a name="new-desktop-features-in-net-core"></a>Novos recursos de área de trabalho no .NET Core

### <a name="support-for-windows-forms-and-wpf"></a>Suporte para Windows Forms e WPF

O Windows Forms e o WPF fazem parte do .NET Core desde a versão 3,0. Ambas as estruturas de apresentação são apenas para o Windows, portanto, não são de plataforma cruzada. Você pode considerar o WPF como uma camada avançada sobre o DirectX e Windows Forms como uma camada mais fina em GDI+. O WPF e o Windows Forms fazem um ótimo trabalho para expor e exercer grande parte da funcionalidade do aplicativo de desktop no Windows. Portanto, Windows Forms e o WPF estão disponíveis para .NET Core e .NET Framework. Agora você pode iniciar seus novos aplicativos de área de trabalho direcionando o .NET Core e migrar os existentes de .NET Framework para o .NET Core.

Uma nova versão do .NET Standard, versão 2,1, foi lançada ao mesmo tempo que o .NET Core 3,0. Conforme esperado, as versões do .NET Core 3. x oferecem suporte a .NET Standard 2,1 e versões anteriores.

Além disso, é importante observar que as implementações Windows Forms e WPF para .NET Core são de software livre.

### <a name="xaml-islands"></a>Ilhas XAML

As [ilhas XAML](/windows/apps/desktop/modernize/xaml-islands) são um conjunto de componentes para que os desenvolvedores usem os novos controles do Windows 10 (controles XAML do UWP) no WPF atual, Windows Forms e aplicativos nativos do Win32 (como o MFC). Você pode ter suas "ilhas" dos controles XAML do UWP sempre que desejar dentro de seus aplicativos Win32.

Essas ilhas XAML são possíveis porque o Windows 10, versão 1903, apresenta um conjunto de APIs que permite hospedar conteúdo do UWP XAML em janelas Win32 usando manipuladores do Windows (HWnds). Observe que somente os aplicativos em execução no Windows 10 1903 e superior podem usar as ilhas XAML.

Para facilitar a criação de ilhas XAML para desenvolvedores Windows Forms e WPF, o kit de ferramentas da Comunidade do Windows introduz um conjunto de wrappers .NET em vários pacotes NuGet. Esses wrappers são os controles encapsulados e de hospedagem:

- Os controles resumidos [WebView](/windows/communitytoolkit/controls/wpf-winforms/webview), [WebViewCompatible](/windows/communitytoolkit/controls/wpf-winforms/webviewcompatible), [InkCanvas](/windows/communitytoolkit/controls/wpf-winforms/inkcanvas), [mediaplayerelement](/windows/communitytoolkit/controls/wpf-winforms/mediaplayerelement)e [MapControl](/windows/communitytoolkit/controls/wpf-winforms/mapcontrol) encapsulam alguns controles XAML do UWP em controles Windows Forms ou WPF, ocultando os conceitos de UWP para esses desenvolvedores.
- O controle [WindowsXamlHost](/windows/communitytoolkit/controls/wpf-winforms/windowsxamlhost) para Windows Forms e o WPF permite que outros controles XAML não encapsulados UWP e controles personalizados possam ser carregados em uma ilha XAML.

### <a name="access-to-all-windows-10-apis"></a>Acesso a todas as APIs do Windows 10

O Windows 10 tem uma grande quantidade de API disponível para os desenvolvedores trabalharem. Essas APIs fornecem acesso a uma ampla variedade de funcionalidades, como autenticação, Bluetooth, compromissos e contatos. Agora essas APIs são expostas por meio do .NET Core e oferecem aos desenvolvedores do Windows a chance de criar aplicativos de área de trabalho avançados utilizando os recursos presentes no Windows 10.

### <a name="side-by-side-support-and-self-contained-exes"></a>Suporte lado a lado e EXEs independentes

O modelo de implantação do .NET Core é um dos maiores benefícios que os desenvolvedores de área de trabalho do Windows experimentarão com o .NET Core. A capacidade de instalar globalmente o .NET Core fornece muitos dos mesmos benefícios de instalação central e de manutenção do .NET Framework, enquanto não requer atualizações in-loco.

Quando uma nova versão do .NET Core é lançada, você pode atualizar cada aplicativo em um computador, conforme necessário, sem qualquer preocupação de afetar outros aplicativos. As novas versões do .NET Core são instaladas em seus próprios diretórios e existem "lado a lado" entre si.

Se você precisar implantar com isolamento, poderá implantar o .NET Core com seu aplicativo. O .NET Core incluirá seu aplicativo com o tempo de execução do .NET Core como em um único executável.

Essas opções de implantação foram solicitadas pelos desenvolvedores por muito tempo, mas eram difíceis de obter usando .NET Framework. A arquitetura modular usada pelo .NET Core torna essas opções de implantação flexíveis possíveis.

### <a name="performance"></a>Desempenho

Desde seu início, visando as cargas de trabalho da Web e da nuvem, o .NET Core teve o desempenho conectado a seu DNA. O código do servidor deve ser eficaz o suficiente para atender a cenários de alta simultaneidade e pontuações do .NET Core hoje como a melhor plataforma da Web de desempenho no mercado.

Você pode aproveitar essas melhorias de desempenho ao usar o .NET Core para criar sua próxima geração de aplicativos de área de trabalho.

## <a name="benefits-of-open-source"></a>Benefícios do software livre

Apenas algumas palavras sobre o .NET Core são de código aberto. Criar uma pilha de plataforma cruzada é algo complexo que precisa da interação de equipes especializadas em cada uma das plataformas de destino. Esse esforço precisa de muita colaboração de dentro e fora da Microsoft. Ao torná-lo de software livre e, portanto, abrir para a colaboração pública, você obtém o estilo de desenvolvimento ágil, que eleva a barra de qualidade, uma vez que os problemas são detectados por uma comunidade enorme e ativa de desenvolvedores.

Esse é um fator-chave de sucesso do .NET Core que continuará a acelerar o roteiro mencionado anteriormente: para ser a única plataforma .NET que qualquer desenvolvedor precisará criar qualquer aplicativo.

>[!div class="step-by-step"]
>[Anterior](why-modern-applications.md) 
> [Avançar](migrate-modern-applications.md)
