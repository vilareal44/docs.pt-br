---
title: Visão geral da globalização e localização
description: Saiba mais sobre a localização e a globalização para Windows Presentation Foundation, incluindo layout automático, assemblies satélite e atributos localizados.
ms.date: 03/30/2017
helpviewer_keywords:
- globalization [WPF], about globalization
- localization [WPF], about localization
ms.assetid: 56e5a5c8-6c96-4d19-b8e1-a5be1dc564af
ms.openlocfilehash: 9a54ce2c265b989f99383ff9cdec961c80dd655c
ms.sourcegitcommit: 87cfeb69226fef01acb17c56c86f978f4f4a13db
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/24/2020
ms.locfileid: "87168293"
---
# <a name="wpf-globalization-and-localization-overview"></a>Visão geral de globalização e localização do WPF

Ao limitar a disponibilidade do produto a apenas um idioma, você limita sua base de clientes potencial a uma fração da população de 7.500.000.000 do nosso mundo. Se desejar que seus aplicativos alcancem um público global, a localização econômica de seu produto será uma das melhores e mais econômicas maneiras de alcançar mais clientes.

Esta visão geral apresenta a globalização e a localização no [!INCLUDE[TLA#tla_winclient](../../../../includes/tlasharptla-winclient-md.md)] . A globalização é o design e o desenvolvimento de aplicativos que são executados em várias localizações. Por exemplo, a globalização dá suporte a interfaces do usuário localizadas e a dados regionais para usuários em culturas diferentes. [!INCLUDE[TLA2#tla_winclient](../../../../includes/tla2sharptla-winclient-md.md)]fornece recursos de design globalizados, incluindo layout automático, assemblies satélite e atributos localizados e comentários.

A localização é a tradução de recursos do aplicativo em versões localizadas para culturas específicas às quais o aplicativo dá suporte. Ao localizar no [!INCLUDE[TLA2#tla_winclient](../../../../includes/tla2sharptla-winclient-md.md)] , você usa as APIs no <xref:System.Windows.Markup.Localizer> namespace. Essas APIs capacitam a ferramenta de linha de comando de [exemplo de ferramenta LocBaml](https://github.com/microsoft/WPF-Samples/tree/master/Tools/LocBaml) . Para obter informações sobre como criar e usar LocBaml, consulte [localizar um aplicativo](how-to-localize-an-application.md).

## <a name="best-practices-for-globalization-and-localization-in-wpf"></a>Melhores práticas de globalização e localização no WPF

Você pode aproveitar ao máximo a funcionalidade de globalização e localização criada no [!INCLUDE[TLA2#tla_winclient](../../../../includes/tla2sharptla-winclient-md.md)] seguindo o design da interface do usuário e as dicas relacionadas à localização que esta seção fornece.

### <a name="best-practices-for-wpf-ui-design"></a>Melhores práticas de design de interface do usuário no WPF

Ao projetar um [!INCLUDE[TLA2#tla_winclient](../../../../includes/tla2sharptla-winclient-md.md)] – baseado [!INCLUDE[TLA2#tla_ui](../../../../includes/tla2sharptla-ui-md.md)] , considere implementar essas práticas recomendadas:

- Escreva seu [!INCLUDE[TLA2#tla_ui](../../../../includes/tla2sharptla-ui-md.md)] no [!INCLUDE[TLA2#tla_xaml](../../../../includes/tla2sharptla-xaml-md.md)] ; Evite criar [!INCLUDE[TLA2#tla_ui](../../../../includes/tla2sharptla-ui-md.md)] em código. Quando você cria seu [!INCLUDE[TLA2#tla_ui](../../../../includes/tla2sharptla-ui-md.md)] usando o [!INCLUDE[TLA2#tla_xaml](../../../../includes/tla2sharptla-xaml-md.md)] , você o expõe por meio de APIs de localização internas.

- Evite usar posições absolutas e tamanhos fixos para definir o layout de conteúdo; em vez disso, use o dimensionamento relativo ou automático.

  - Use <xref:System.Windows.Window.SizeToContent%2A> e mantenha larguras e alturas definidas como `Auto` .

  - Evite usar <xref:System.Windows.Controls.Canvas> para fazer o layout de [!INCLUDE[TLA2#tla_ui](../../../../includes/tla2sharptla-ui-md.md)] s.

  - Use <xref:System.Windows.Controls.Grid> e seu recurso de compartilhamento de tamanho.

- Forneça espaço extra nas margens, pois, de modo geral, o texto localizado exige mais espaço. O espaço extra permite possíveis caracteres estendidos.

- Habilitar <xref:System.Windows.Controls.TextBlock.TextWrapping%2A> no <xref:System.Windows.Controls.TextBlock> para evitar o recorte.

- Defina o `xml:lang` atributo. Esse atributo descreve a cultura de um elemento específico e seus elementos filho. O valor dessa propriedade altera o comportamento de vários recursos no [!INCLUDE[TLA2#tla_winclient](../../../../includes/tla2sharptla-winclient-md.md)] . Por exemplo, ele altera o comportamento de hifenização, verificação ortográfica, substituição de números, formatação de scripts complexos e fallback de fontes. Consulte [Globalization for WPF](globalization-for-wpf.md) para obter mais informações sobre como definir o [tratamento XML: lang em XAML](../../../desktop-wpf/xaml-services/xml-language-handling.md).

- Crie uma fonte composta personalizada para obter um melhor controle das fontes usadas para diferentes idiomas. Por padrão, [!INCLUDE[TLA2#tla_winclient](../../../../includes/tla2sharptla-winclient-md.md)] o usa a fonte GlobalUserInterface. Composite no diretório Windows\Fonts.

- Quando você cria aplicativos de navegação que podem ser localizados em uma cultura que apresenta texto em um formato da direita para a esquerda, defina explicitamente a <xref:System.Windows.FlowDirection> de cada página para garantir que a página não seja herdada <xref:System.Windows.FlowDirection> do <xref:System.Windows.Navigation.NavigationWindow> .

- Quando você cria aplicativos de navegação autônomos que são hospedados fora de um navegador, defina o <xref:System.Windows.Application.StartupUri%2A> para o aplicativo inicial como um <xref:System.Windows.Navigation.NavigationWindow> em vez de em uma página (por exemplo, `<Application StartupUri="NavigationWindow.xaml">` ). Esse design permite que você altere o <xref:System.Windows.FlowDirection> da janela e a barra de navegação. Para obter mais informações e um exemplo, consulte [exemplo de home page de globalização](https://github.com/microsoft/WPF-Samples/tree/master/Globalization%20and%20Localization/GlobalizationHomepage).

### <a name="best-practices-for-wpf-localization"></a>Melhores práticas de localização no WPF

Ao localizar [!INCLUDE[TLA2#tla_winclient](../../../../includes/tla2sharptla-winclient-md.md)] aplicativos baseados em, considere a implementação dessas práticas recomendadas:

- Use comentários de localização para fornecer contexto extra para os localizadores.

- Use atributos de localização para controlar a localização em vez de omitir seletivamente <xref:System.Windows.Markup.Localizer.BamlLocalizableResourceKey.Uid%2A> Propriedades em elementos. Consulte [atributos e comentários de localização](localization-attributes-and-comments.md) para obter mais informações.

- Use `msbuild -t:updateuid` e `-t:checkuid` para adicionar e verificar <xref:System.Windows.Markup.Localizer.BamlLocalizableResourceKey.Uid%2A> as propriedades no seu [!INCLUDE[TLA2#tla_xaml](../../../../includes/tla2sharptla-xaml-md.md)] . Use <xref:System.Windows.Markup.Localizer.BamlLocalizableResourceKey.Uid%2A> as propriedades para controlar as alterações entre o desenvolvimento e a localização. <xref:System.Windows.Markup.Localizer.BamlLocalizableResourceKey.Uid%2A>as propriedades ajudam você a localizar novas alterações de desenvolvimento. Se você adicionar <xref:System.Windows.Markup.Localizer.BamlLocalizableResourceKey.Uid%2A> Propriedades manualmente a um [!INCLUDE[TLA2#tla_ui](../../../../includes/tla2sharptla-ui-md.md)] , a tarefa será normalmente entediante e menos precisa.

  - Não edite ou altere <xref:System.Windows.Markup.Localizer.BamlLocalizableResourceKey.Uid%2A> as propriedades depois de começar a localização.

  - Não use Propriedades duplicadas <xref:System.Windows.Markup.Localizer.BamlLocalizableResourceKey.Uid%2A> (Lembre-se dessa dica ao usar o comando copiar e colar).

  - Defina o `UltimateResourceFallback` local em AssemblyInfo. \* para especificar o idioma apropriado para fallback (por exemplo, `[assembly: NeutralResourcesLanguage("en-US",   UltimateResourceFallbackLocation.Satellite)]` ).

    Se você decidir incluir o idioma de origem no assembly principal omitindo a `<UICulture>` marca no arquivo do projeto, defina o `UltimateResourceFallback` local como o assembly principal em vez do satélite (por exemplo, `[assembly: NeutralResourcesLanguage("en-US", UltimateResourceFallbackLocation.MainAssembly)]` ).

## <a name="localize-a-wpf-application"></a>Localizar um aplicativo WPF

Ao localizar um [!INCLUDE[TLA2#tla_winclient](../../../../includes/tla2sharptla-winclient-md.md)] aplicativo, você tem várias opções. Por exemplo, você pode associar os recursos localizáveis em seu aplicativo a um arquivo XML, armazenar texto localizável em tabelas resx ou fazer com que seu localizador use [!INCLUDE[TLA#tla_xaml](../../../../includes/tlasharptla-xaml-md.md)] arquivos. Esta seção descreve um fluxo de trabalho de localização que usa a forma de BAML do XAML, que fornece vários benefícios:

- Você pode localizar depois de Compilar.

- Você pode atualizar para uma versão mais recente da forma de BAML do XAML com localizações de uma versão mais antiga da forma de BAML do XAML, para que você possa localizar ao mesmo tempo que desenvolve.

- Você pode validar os elementos de origem e a semântica originais em tempo de compilação porque a forma de BAML do XAML é a forma compilada do [!INCLUDE[TLA2#tla_xaml](../../../../includes/tla2sharptla-xaml-md.md)] .

### <a name="localization-build-process"></a>Processo de build para localização

Quando você desenvolve um [!INCLUDE[TLA2#tla_winclient](../../../../includes/tla2sharptla-winclient-md.md)] aplicativo, o processo de compilação para localização é o seguinte:

- O desenvolvedor cria e globalizes o [!INCLUDE[TLA2#tla_winclient](../../../../includes/tla2sharptla-winclient-md.md)] aplicativo. No arquivo de projeto que o desenvolvedor define `<UICulture>en-US</UICulture>` para que, quando o aplicativo for compilado, um assembly principal com neutralidade de idioma será gerado. Esse assembly tem um arquivo satélite .resources.dll que contém todos os recursos localizáveis. Opcionalmente, você pode manter o idioma de origem no assembly principal porque nossas APIs de localização dão suporte à extração do assembly principal.

- Quando o arquivo é compilado na compilação, o [!INCLUDE[TLA2#tla_xaml](../../../../includes/tla2sharptla-xaml-md.md)] é convertido para a forma de BAML do XAML. Os arquivos culturalmente neutro `MyDialog.exe` e dependente cultural (inglês) `MyDialog.resources.dll` são lançados para o cliente de língua inglesa.

### <a name="localization-workflow"></a>Fluxo de trabalho de localização

O processo de localização começa depois que o `MyDialog.resources.dll` arquivo não local é criado. Os [!INCLUDE[TLA2#tla_ui](../../../../includes/tla2sharptla-ui-md.md)] elementos e as propriedades no seu original [!INCLUDE[TLA2#tla_xaml](../../../../includes/tla2sharptla-xaml-md.md)] são extraídos da forma de BAML do XAML em pares de chave-valor usando as APIs em <xref:System.Windows.Markup.Localizer> . Os localizadores usam os pares chave-valor para localizar o aplicativo. É possível gerar um novo .resource.dll com base nos novos valores após a conclusão da localização.

As chaves dos pares chave-valor são `x:Uid` valores que são colocados pelo desenvolvedor no original [!INCLUDE[TLA2#tla_xaml](../../../../includes/tla2sharptla-xaml-md.md)] . Esses `x:Uid` valores permitem que a API acompanhe e mescle as alterações que ocorrem entre o desenvolvedor e o localizador durante a localização. Por exemplo, se o desenvolvedor alterar o [!INCLUDE[TLA2#tla_ui](../../../../includes/tla2sharptla-ui-md.md)] depois que o localizador começar a localizar, você poderá mesclar a alteração de desenvolvimento com o trabalho de localização já concluído para que o mínimo de trabalho de tradução seja perdido.

O gráfico a seguir mostra um fluxo de trabalho típico de localização que se baseia no formato BAML de XAML. Este diagrama pressupõe que o desenvolvedor grave o aplicativo em inglês. O desenvolvedor cria e globaliza o aplicativo do WPF. No arquivo de projeto que o desenvolvedor define `<UICulture>en-US</UICulture>` para que, na compilação, um assembly principal de idioma neutro é gerado com um .resources.dll satélite contendo todos os recursos localizáveis. Como alternativa, é possível manter o idioma de origem no assembly principal, pois as APIs de localização do WPF dão suporte à extração do assembly principal. Após o processo de build, o XAML é compilado para o BAML. O MyDialog.exe.resources.dll com neutralidade de cultura é enviado para o cliente falante de inglês.

![Diagrama mostrando o fluxo de trabalho de localização.](./media/wpf-globalization-and-localization-overview/localization-workflow.png)

![Diagrama mostrando o fluxo de trabalho não local.](./media/wpf-globalization-and-localization-overview/unlocalized-workflow.png)

## <a name="examples-of-wpf-localization"></a>Exemplos de localização no WPF

Esta seção contém exemplos de aplicativos localizados para ajudá-lo a entender como compilar e localizar [!INCLUDE[TLA2#tla_winclient](../../../../includes/tla2sharptla-winclient-md.md)] aplicativos.

### <a name="run-dialog-box-example"></a>Exemplo da caixa de diálogo Executar

Os gráficos a seguir mostram a saída da amostra da caixa de diálogo **executar** .

**Inglês:**

![Captura de tela mostrando uma caixa de diálogo de execução em inglês.](./media/wpf-globalization-and-localization-overview/run-dialog-box-english.png)

**Alemão:**

![Captura de tela mostrando uma caixa de diálogo de execução em alemão.](./media/wpf-globalization-and-localization-overview/run-dialog-box-german.png)

**Criando uma caixa de diálogo Executar global**

Este exemplo produz uma caixa de diálogo de **execução** usando [!INCLUDE[TLA2#tla_winclient](../../../../includes/tla2sharptla-winclient-md.md)] e [!INCLUDE[TLA2#tla_xaml](../../../../includes/tla2sharptla-xaml-md.md)] . Essa caixa de diálogo é equivalente à caixa de diálogo **executar** que está disponível no menu Iniciar do Microsoft Windows.

Alguns destaques para a criação de caixas de diálogo globais são:

**Layout automático**

*Em Window1.xaml:*

`<Window SizeToContent="WidthAndHeight">`

A propriedade Window anterior redimensiona automaticamente a janela de acordo com o tamanho do conteúdo. Essa propriedade impede que a janela corte o conteúdo que aumenta de tamanho após a localização; ela também remove o espaço desnecessário quando o conteúdo diminui de tamanho após a localização.

`<Grid x:Uid="Grid_1">`

<xref:System.Windows.Markup.Localizer.BamlLocalizableResourceKey.Uid%2A>as propriedades são necessárias para [!INCLUDE[TLA2#tla_winclient](../../../../includes/tla2sharptla-winclient-md.md)] que as APIs de localização funcionem corretamente.

Eles são usados pelas [!INCLUDE[TLA2#tla_winclient](../../../../includes/tla2sharptla-winclient-md.md)] APIs de localização para controlar as alterações entre o desenvolvimento e a localização do [!INCLUDE[TLA#tla_ui](../../../../includes/tlasharptla-ui-md.md)] . <xref:System.Windows.Markup.Localizer.BamlLocalizableResourceKey.Uid%2A>as propriedades permitem que você mescle uma versão mais recente do [!INCLUDE[TLA2#tla_ui](../../../../includes/tla2sharptla-ui-md.md)] com uma localização mais antiga do [!INCLUDE[TLA2#tla_ui](../../../../includes/tla2sharptla-ui-md.md)] . Você adiciona uma <xref:System.Windows.Markup.Localizer.BamlLocalizableResourceKey.Uid%2A> Propriedade executando `msbuild -t:updateuid RunDialog.csproj` em um shell de comando. Esse é o método recomendado para adicionar <xref:System.Windows.Markup.Localizer.BamlLocalizableResourceKey.Uid%2A> Propriedades porque adicioná-las manualmente normalmente é demorado e menos preciso. Você pode verificar se <xref:System.Windows.Markup.Localizer.BamlLocalizableResourceKey.Uid%2A> as propriedades estão definidas corretamente executando `msbuild -t:checkuid RunDialog.csproj` .

O [!INCLUDE[TLA2#tla_ui](../../../../includes/tla2sharptla-ui-md.md)] é estruturado usando o <xref:System.Windows.Controls.Grid> controle, que é um controle útil para aproveitar o layout automático no [!INCLUDE[TLA2#tla_winclient](../../../../includes/tla2sharptla-winclient-md.md)] . Observe que a caixa de diálogo é dividida em três linhas e cinco colunas. Não uma das definições de linha e coluna tem um tamanho fixo; Portanto, os [!INCLUDE[TLA2#tla_ui](../../../../includes/tla2sharptla-ui-md.md)] elementos que estão posicionados em cada célula podem se adaptar a aumentos e diminuições de tamanho durante a localização.

[!code-xaml[GlobalizationRunDialog#GridColumnDef](~/samples/snippets/csharp/VS_Snippets_Wpf/GlobalizationRunDialog/CS/Window1.xaml#gridcolumndef)]

As duas primeiras colunas em que o rótulo **abrir:** e <xref:System.Windows.Controls.ComboBox> são colocadas usam 10% da [!INCLUDE[TLA2#tla_ui](../../../../includes/tla2sharptla-ui-md.md)] largura total.

[!code-xaml[GlobalizationRunDialog#GridColumnDef2](~/samples/snippets/csharp/VS_Snippets_Wpf/GlobalizationRunDialog/CS/Window1.xaml#gridcolumndef2)]

Observe que o exemplo usa o recurso de dimensionamento compartilhado do <xref:System.Windows.Controls.Grid> . As três últimas colunas aproveitam isso colocando-se no mesmo <xref:System.Windows.Controls.DefinitionBase.SharedSizeGroup%2A> . Como normalmente se esperaria do nome da propriedade, isso permite que as colunas compartilhem o mesmo tamanho. Então, quando o botão "procurar..." é localizado para a cadeia de caracteres mais longa "Durchsuchen...", todos os botões crescem em largura, em vez de ter um botão pequeno "OK" e um "Durchsuchen..." desproporcional. Button.

**XML: lang**

`xml:lang="en-US"`

Observe o [tratamento XML: lang no XAML](../../../desktop-wpf/xaml-services/xml-language-handling.md) colocado no elemento raiz do [!INCLUDE[TLA2#tla_ui](../../../../includes/tla2sharptla-ui-md.md)] . Essa propriedade descreve a cultura de determinado elemento e seus filhos. Esse valor é usado por vários recursos no [!INCLUDE[TLA2#tla_winclient](../../../../includes/tla2sharptla-winclient-md.md)] e deve ser alterado adequadamente durante a localização. Esse valor altera qual dicionário de idioma deve ser usado para hifenizar e fazer a verificação ortográfica das palavras. Ele também afeta a exibição de dígitos e como o sistema de fallback de fontes seleciona qual fonte deve ser usada. Finalmente, a propriedade afeta a maneira como os números são exibidos e o modo como os textos escritos em scripts complexos são formatados. O valor padrão é “en-US”.

**Compilando um assembly de recursos satélite**

*Em .csproj:*

Edite o `.csproj` arquivo e adicione a seguinte marca a um incondicional `<PropertyGroup>` :

`<UICulture>en-US</UICulture>`

Observe a adição de um `UICulture` valor. Quando definido como um valor válido, como <xref:System.Globalization.CultureInfo> en-US, compilar o projeto irá gerar um assembly satélite com todos os recursos localizáveis nele.

`<Resource Include="RunIcon.JPG">`

`<Localizable>False</Localizable>`

`</Resource>`

O não `RunIcon.JPG` precisa ser localizado porque deve parecer o mesmo para todas as culturas. `Localizable`é definido como para `false` que permaneça no assembly principal de neutralidade do idioma em vez do assembly satélite. O valor padrão de todos os recursos não compiláveis é `Localizable` definido como `true` .

**Localizando a caixa de diálogo Executar**

**Analisar**

Depois de criar o aplicativo, a primeira etapa da localização é analisar os recursos localizáveis fora do assembly satélite. Para os fins deste tópico, use a ferramenta LocBaml de exemplo que pode ser encontrada no [exemplo de ferramenta LocBaml](https://github.com/microsoft/WPF-Samples/tree/master/Tools/LocBaml). Observe que LocBaml é apenas uma ferramenta de exemplo para ajudá-lo a começar a criar uma ferramenta de localização que se ajuste ao seu processo de localização. Usando LocBaml, execute o seguinte para analisar: **LocBaml/parse RunDialog.resources.dll/out:** para gerar um arquivo "RunDialog.resources.dll.CSV".

**Localize**

Use seu editor de CSV favorito que dá suporte a Unicode para editar esse arquivo. Filtre todas as entradas com a categoria de localização “Nenhum”. Você deverá ver as seguintes entradas:

|Chave de Recurso|Categoria de Localização|Valor|
|-|-|-|
|Button_1:System.Windows.Controls.Button.$Content|Botão|OK|
|Button_2:System.Windows.Controls.Button.$Content|Botão|Cancelar|
|Button_3:System.Windows.Controls.Button.$Content|Botão|Procurar...|
|ComboBox_1:System.Windows.Controls.ComboBox.$Content|ComboBox||
|TextBlock_1:System.Windows.Controls.TextBlock.$Content|Texto|Digite o nome de um programa, uma pasta, um documento ou recurso da Internet e o Windows o abrirá para você.|
|TextBlock_2:System.Windows.Controls.TextBlock.$Content|Texto|Aberto:|
|Window_1:System.Windows.Window.Title|Título|Executar|

A localização do aplicativo para o alemão exigirá as seguintes traduções:

|Chave de Recurso|Categoria de Localização|Valor|
|-|-|-|
|Button_1:System.Windows.Controls.Button.$Content|Botão|OK|
|Button_2:System.Windows.Controls.Button.$Content|Botão|Abbrechen|
|Button_3:System.Windows.Controls.Button.$Content|Botão|Durchsuchen…|
|ComboBox_1:System.Windows.Controls.ComboBox.$Content|ComboBox||
|TextBlock_1:System.Windows.Controls.TextBlock.$Content|Texto|Geben Sie den Namen eines Programms, Ordners, Dokuments oder einer Internetresource an.|
|TextBlock_2:System.Windows.Controls.TextBlock.$Content|Texto|Öffnen:|
|Window_1:System.Windows.Window.Title|Título|Executar|

**Generate**

A última etapa da localização envolve a criação do assembly satélite recém-localizado. Faça isso com o seguinte comando do LocBaml:

**LocBaml.exe /generate RunDialog.resources.dll /trans:RunDialog.resources.dll.CSV /out: . /cul:de-DE**

No Windows em alemão, se esse resources.dll for colocado em uma pasta de de-DE ao lado do assembly principal, esse recurso será carregado automaticamente em vez do que estiver na pasta en-US. Se você não tiver uma versão em alemão do Windows para testar isso, defina a cultura para qualquer cultura do Windows que você esteja usando (por exemplo, `en-US` ) e substitua a DLL de recursos originais.

**Carregamento de recursos satélite**

|MyDialog.exe|en-US\MyDialog.resources.dll|de-DE\MyDialog.resources.dll|
|------------------|------------------------------------|------------------------------------|
|Código|BAML original em inglês|BAML localizado|
|Recursos com neutralidade de cultura|Outros recursos em inglês|Outros recursos localizados para o alemão|

O .NET escolhe automaticamente qual assembly de recursos de satélite carregar com base no aplicativo <xref:System.Threading.Thread.CurrentUICulture%2A?displayProperty=nameWithType> . O padrão é a cultura do seu sistema operacional Windows. Se você estiver usando janelas em alemão, o arquivo de *de-DE\MyDialog.resources.dll* será carregado. Se você estiver usando janelas em inglês, o arquivo de *en-US\MyDialog.resources.dll* será carregado. Você pode definir o recurso de fallback final para seu aplicativo especificando o `NeutralResourcesLanguage` atributo no arquivo *AssemblyInfo* do seu projeto. Por exemplo, se você especificar:

`[assembly: NeutralResourcesLanguage("en-US", UltimateResourceFallbackLocation.Satellite)]`

em seguida, o arquivo de *en-US\MyDialog.resources.dll* será usado com janelas em alemão se nenhum dos seguintes arquivos estiver disponível: *de-DE\MyDialog.resources.dll* ou *de\MyDialog.resources.dll*.

### <a name="microsoft-saudi-arabia-homepage"></a>Home page da Microsoft na Arábia Saudita

Os gráficos a seguir mostram uma home page em inglês e em árabe. Para obter o exemplo completo que produz esses gráficos, consulte [exemplo de home page de globalização](https://github.com/microsoft/WPF-Samples/tree/master/Globalization%20and%20Localization/GlobalizationHomepage).

**Inglês:**

![Captura de tela mostrando um home page em inglês.](./media/wpf-globalization-and-localization-overview/english-home-page-sample.jpg)

**Árabe:**

![Captura de tela mostrando um home page árabe.](./media/wpf-globalization-and-localization-overview/arabic-home-page-sample.jpg)

### <a name="designing-a-global-microsoft-home-page"></a>Criando um Microsoft home page global

Este exemplo fictício do site da Microsoft na Arábia Saudita ilustra os recursos de globalização fornecidos para idiomas RightToLeft. Linguagens como hebraico e árabe têm uma ordem de leitura da direita para a esquerda, de modo que o layout de [!INCLUDE[TLA2#tla_ui](../../../../includes/tla2sharptla-ui-md.md)] deve muitas vezes ser disposto de forma muito diferente do que seria em idiomas da esquerda para a direita, como o inglês. A localização de um idioma da esquerda para a direita para um idioma da direita para a esquerda ou vice-versa pode apresentar muitos desafios. O [!INCLUDE[TLA2#tla_winclient](../../../../includes/tla2sharptla-winclient-md.md)] foi projetado para facilitar grande parte dessas localizações.

**FlowDirection**

*Homepage.xaml:*

[!code-xaml[GlobalizationHomepage#Homepage](~/samples/snippets/csharp/VS_Snippets_Wpf/GlobalizationHomepage/CS/Homepage.xaml#homepage)]

Observe a <xref:System.Windows.FrameworkElement.FlowDirection%2A> propriedade em <xref:System.Windows.Controls.Page> . Alterar essa propriedade para <xref:System.Windows.FlowDirection.RightToLeft> alterará o <xref:System.Windows.FrameworkElement.FlowDirection%2A> dos <xref:System.Windows.Controls.Page> seus elementos filhos e para que o layout dele [!INCLUDE[TLA2#tla_ui](../../../../includes/tla2sharptla-ui-md.md)] seja invertido para ser da direita para a esquerda como um usuário árabe esperaria. É possível substituir o comportamento da herança especificando um explícito <xref:System.Windows.FrameworkElement.FlowDirection%2A> em qualquer elemento. A <xref:System.Windows.FrameworkElement.FlowDirection%2A> propriedade está disponível em qualquer <xref:System.Windows.FrameworkElement> elemento ou de documento relacionado e tem um valor implícito de <xref:System.Windows.FlowDirection.LeftToRight> .

Observe que até mesmo os pincéis do gradiente do plano de fundo são invertidos corretamente quando a raiz <xref:System.Windows.FrameworkElement.FlowDirection%2A> é alterada:

**FlowDirection="LeftToRight"**

![Captura de tela mostrando o fluxo de gradiente da esquerda para a direita.](./media/wpf-globalization-and-localization-overview/gradient-flow-left-right.png)

**FlowDirection="RightToLeft"**

![Captura de tela mostrando o fluxo de gradiente da direita para a esquerda.](./media/wpf-globalization-and-localization-overview/gradient-flow-right-left.png)

**Evitar usar dimensões fixas para painéis e controles**

Dê uma olhada na homepage. XAML, observe que, além da largura e da altura fixas especificadas para o todo [!INCLUDE[TLA2#tla_ui](../../../../includes/tla2sharptla-ui-md.md)] na parte superior <xref:System.Windows.Controls.DockPanel> , não há outras dimensões fixas. Evite usar dimensões fixas para evitar o recorte do texto localizado, que pode ser maior que o texto de origem. Os painéis e controles do [!INCLUDE[TLA2#tla_winclient](../../../../includes/tla2sharptla-winclient-md.md)] serão automaticamente redimensionados de acordo com o conteúdo que eles contêm. A maioria dos controles também tem dimensões mínimas e máximas que você pode definir para mais controle (por exemplo, MinWidth = "20"). Com <xref:System.Windows.Controls.Grid> o, você também pode definir larguras e alturas relativas usando ' \* ' (por exemplo, `Width="0.25*"` ) ou usar seu recurso de compartilhamento de tamanho de célula.

**Comentários de localização**

Há muitos casos em que o conteúdo pode ser ambíguo e difícil de ser traduzido. O desenvolvedor ou o designer tem a capacidade de fornecer um contexto extra e comentários para os localizadores por meio de comentários de localização. Por exemplo, a localização. Comments abaixo esclarece o uso do caractere "&#124;".

[!code-xaml[GlobalizationHomepage#LocalizationComment](~/samples/snippets/csharp/VS_Snippets_Wpf/GlobalizationHomepage/CS/Homepage.xaml#localizationcomment)]

Esse comentário se torna associado ao conteúdo de TextBlock_1 e, no caso da ferramenta LocBaml, (consulte [localizar um aplicativo](how-to-localize-an-application.md)), ele pode ser visto na 6º coluna da linha de TextBlock_1 no arquivo de saída. csv:

|Chave de Recurso|Categoria|Legível|Modificável|Comentário|Valor|
|-|-|-|-|-|-|
|TextBlock_1:System.Windows.Controls.TextBlock.$Content|Texto|TRUE|TRUE|Esse caractere é usado como uma regra decorativa.|&#124;|

Os comentários podem ser colocados no conteúdo ou na propriedade de qualquer elemento com a seguinte sintaxe:

[!code-xaml[GlobalizationHomepage#LocalizationCommentsProp](~/samples/snippets/csharp/VS_Snippets_Wpf/GlobalizationHomepage/CS/Homepage.xaml#localizationcommentsprop)]

**Atributos de localização**

De modo geral, o desenvolvedor ou o gerente de localização precisa controlar o que os localizadores podem ler e modificar. Por exemplo, talvez você não deseje que o localizador traduza o nome de sua empresa ou palavras que se referem ao conteúdo legal. [!INCLUDE[TLA2#tla_winclient](../../../../includes/tla2sharptla-winclient-md.md)]fornece atributos que permitem que você defina a legibilidade, a modificação e a categoria do conteúdo ou da propriedade de um elemento que sua ferramenta de localização pode usar para bloquear, ocultar ou classificar elementos. Para obter mais informações, consulte <xref:System.Windows.Localization.Attributes%2A>. Para as finalidades desta amostra, a ferramenta LocBaml somente gera os valores desses atributos. Todos os controles do [!INCLUDE[TLA2#tla_winclient](../../../../includes/tla2sharptla-winclient-md.md)] têm valores padrão para esses atributos, mas eles podem ser substituídos. Por exemplo, o exemplo a seguir substitui os atributos de localização padrão para `TextBlock_1` e define o conteúdo como legível, mas não modificável para localizadores.

[!code-xaml[LocalizationComAtt#LocalizationAttributes](~/samples/snippets/csharp/VS_Snippets_Wpf/LocalizationComAtt/CSharp/Attributes.xaml#localizationattributes)]

Além dos atributos de legibilidade e modificação, o [!INCLUDE[TLA2#tla_winclient](../../../../includes/tla2sharptla-winclient-md.md)] fornece uma enumeração de categorias de interface do usuário comuns ( <xref:System.Windows.LocalizationCategory> ) que podem ser usadas para dar mais contexto aos localizadores. As [!INCLUDE[TLA2#tla_winclient](../../../../includes/tla2sharptla-winclient-md.md)] categorias padrão para controles de plataforma também podem ser substituídas em [!INCLUDE[TLA2#tla_xaml](../../../../includes/tla2sharptla-xaml-md.md)] :

[!code-xaml[LocalizationComAtt#LocalizationAttributesOverridden](~/samples/snippets/csharp/VS_Snippets_Wpf/LocalizationComAtt/CSharp/Attributes.xaml#localizationattributesoverridden)]

Os atributos de localização padrão que o [!INCLUDE[TLA2#tla_winclient](../../../../includes/tla2sharptla-winclient-md.md)] fornece também podem ser substituídos por meio de código, para que você possa definir corretamente os valores padrão corretos para controles personalizados. Por exemplo:

```csharp
[Localizability(Readability = Readability.Readable, Modifiability=Modifiability.Unmodifiable, LocalizationCategory.None)]
public class CorporateLogo : TextBlock
{
    // ...
}
```

Os atributos por instância definidos em [!INCLUDE[TLA2#tla_xaml](../../../../includes/tla2sharptla-xaml-md.md)] terão precedência sobre os valores definidos no código em controles personalizados. Para obter mais informações sobre atributos e comentários, consulte [atributos e comentários de localização](localization-attributes-and-comments.md).

**Fallback de fontes e fontes de composição**

Se você especificar uma fonte que não dê suporte a um determinado intervalo ponto, [!INCLUDE[TLA2#tla_winclient](../../../../includes/tla2sharptla-winclient-md.md)] o fará fallback automaticamente para um que faça isso usando a interface de usuário global. CompositeFont que está localizada em seu diretório Windows\Fonts. Fontes compostas funcionam assim como qualquer outra fonte e podem ser usadas explicitamente definindo um elemento `FontFamily` (por exemplo, `FontFamily="Global User Interface"` ). Você pode especificar sua própria preferência de fallback de fontes criando sua própria fonte de composição e especificando qual fonte será usada para intervalos de ponto de código e idiomas específicos.

Para obter mais informações sobre fontes compostas, consulte <xref:System.Windows.Media.FontFamily> .

**Localizando a home page da Microsoft**

É possível seguir as mesmas etapas do exemplo da Caixa de Diálogo Executar para localizar esse aplicativo. O arquivo. csv localizado para árabe está disponível para você no [exemplo de Home Page da globalização](https://github.com/microsoft/WPF-Samples/tree/master/Globalization%20and%20Localization/GlobalizationHomepage).
