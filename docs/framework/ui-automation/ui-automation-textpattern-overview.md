---
title: Visão geral de TextPattern de automação da interface do usuário
description: Leia uma visão geral do padrão de controle TextPattern na automação da interface do usuário. Esse padrão de controle expõe o conteúdo textual de um controle, incluindo o formato e o estilo.
ms.date: 03/30/2017
helpviewer_keywords:
- UI Automation, TextPattern class
- TextPattern class
- classes, TextPattern
ms.assetid: 41787927-df1f-4f4a-aba3-641662854fc4
ms.openlocfilehash: 986b601bbd212582c09c559a9bb22983ba59fa17
ms.sourcegitcommit: 40de8df14289e1e05b40d6e5c1daabd3c286d70c
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/22/2020
ms.locfileid: "86924663"
---
# <a name="ui-automation-textpattern-overview"></a>Visão geral de TextPattern de automação da interface do usuário

> [!NOTE]
> Esta documentação destina-se a desenvolvedores do .NET Framework que querem usar as classes da [!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)] gerenciadas definidas no namespace <xref:System.Windows.Automation>. Para obter as informações mais recentes sobre a [!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)], consulte [Windows Automation API: UI Automation](/windows/win32/winauto/entry-uiauto-win32) (API de Automação do Windows: Automação da Interface do Usuário).

Esta visão geral descreve como usar [!INCLUDE[TLA#tla_uiautomation](../../../includes/tlasharptla-uiautomation-md.md)] o para expor o conteúdo textual, incluindo atributos de formato e estilo, de controles de texto em [!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)] plataformas com suporte. Esses controles incluem, entre outros, a estrutura de Microsoft .NET <xref:System.Windows.Controls.TextBox> e <xref:System.Windows.Controls.RichTextBox> também seus equivalentes do Win32.

Expor o conteúdo textual de um controle é realizado por meio do uso do <xref:System.Windows.Automation.TextPattern> padrão de controle, que representa o conteúdo de um contêiner de texto como um fluxo de texto. Por sua vez, <xref:System.Windows.Automation.TextPattern> o requer o suporte da <xref:System.Windows.Automation.Text.TextPatternRange> classe para expor atributos de formato e estilo. <xref:System.Windows.Automation.Text.TextPatternRange>dá suporte <xref:System.Windows.Automation.TextPattern> ao representação de trechos de texto contíguos ou múltiplos, disjunçãos em um contêiner de texto com uma coleção de <xref:System.Windows.Automation.Text.TextPatternRangeEndpoint.Start> pontos de <xref:System.Windows.Automation.Text.TextPatternRangeEndpoint.End> extremidade e. <xref:System.Windows.Automation.Text.TextPatternRange>oferece suporte a funcionalidades como seleção, comparação, recuperação e passagem.

> [!NOTE]
> As <xref:System.Windows.Automation.TextPattern> classes não fornecem um meio para inserir ou modificar texto. No entanto, dependendo do controle, isso pode ser realizado pelo [!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)] <xref:System.Windows.Automation.ValuePattern> ou por meio de entrada de teclado direta. Consulte o [exemplo inserir texto de TextPattern](https://github.com/Microsoft/WPF-Samples/tree/master/Accessibility/InsertText) para obter um exemplo.

A funcionalidade descrita nesta visão geral é vital para fornecedores de tecnologia assistencial e seus usuários finais. Tecnologias assistenciais podem [!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)] ser usadas para reunir informações completas de formatação de texto para o usuário e fornecer navegação programática e seleção de texto por <xref:System.Windows.Automation.Text.TextUnit> (caractere, palavra, linha ou parágrafo).

<a name="UI_Automation_TextPattern_vs__Cicero"></a>

## <a name="ui-automation-textpattern-vs-text-services-framework"></a>TextPattern da automação da interface do usuário versus estrutura de serviços de texto

A TSF (estrutura de serviços de texto) é uma estrutura de sistema simples e escalonável que permite serviços de linguagem natural e entrada de texto avançada na área de trabalho e em aplicativos. Além de fornecer interfaces para que os aplicativos exponham seu armazenamento de texto, ele também dá suporte a metadados para esse armazenamento de texto.

No entanto, o TSF foi projetado para aplicativos que precisam injetar entrada em cenários com reconhecimento de contexto, enquanto <xref:System.Windows.Automation.TextPattern> que é uma solução somente leitura (com a solução alternativa limitada mencionada acima), destinada a fornecer acesso otimizado a um armazenamento de texto para leitores de tela e dispositivos de braille.

Em suma, tecnologias acessíveis que exigem acesso somente leitura a um armazenamento de texto podem usar <xref:System.Windows.Automation.TextPattern> , mas precisarão da funcionalidade mais complexa do TSF para entrada com reconhecimento de contexto.

<a name="Control_Types"></a>

## <a name="control-types"></a>Tipos de controle

### <a name="text"></a>Texto

O controle texto é o elemento básico que representa uma parte do texto na tela.

Um controle de texto autônomo pode ser usado como um rótulo ou texto estático em um formulário. Os controles de texto também podem estar contidos na estrutura de um <xref:System.Windows.Automation.ControlType.ListItem> , <xref:System.Windows.Automation.ControlType.TreeItem> ou <xref:System.Windows.Automation.ControlType.DataItem> .

> [!NOTE]
> Os controles de texto podem não aparecer na exibição de conteúdo da [!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)] árvore (consulte [visão geral da árvore de automação da interface do usuário](ui-automation-tree-overview.md)). Isso ocorre porque os controles de texto geralmente são exibidos por meio da propriedade Name de outro controle. Por exemplo, o texto usado para rotular um controle de edição é exposto por meio da propriedade Name do controle de edição. Como o controle de edição está na exibição de conteúdo da [!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)] árvore, não é necessário que o próprio elemento de texto esteja nessa exibição da [!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)] árvore. O único texto que aparece na exibição de conteúdo é texto que não é informações redundantes. Isso permite que qualquer tecnologia assistencial filtre rapidamente apenas as informações de que seus usuários precisam.

### <a name="edit"></a>Editar

Os controles de edição permitem que um usuário exiba e edite uma única linha de texto.

> [!NOTE]
> A linha única de texto pode ser encapsulada em determinados cenários de layout.

### <a name="document"></a>Document

Os controles de documento permitem que um usuário navegue e obtenha informações de várias páginas de texto.

<a name="TextPattern_Client_API_s"></a>

## <a name="textpattern-client-apis"></a>APIs de cliente TextPattern

|||
|-|-|
|`System.Windows.Automation.TextPattern Class`|O ponto de entrada para o [!INCLUDE[TLA#tla_uiautomation](../../../includes/tlasharptla-uiautomation-md.md)] modelo de texto.<br /><br /> Essa classe também contém os dois <xref:System.Windows.Automation.TextPattern> ouvintes de eventos <xref:System.Windows.Automation.TextPattern.TextSelectionChangedEvent> e <xref:System.Windows.Automation.TextPattern.TextChangedEvent> .|
|`System.Windows.Automation.Text.TextPatternRange Class`|A representação de um intervalo de texto dentro de um contêiner de texto que dá suporte a <xref:System.Windows.Automation.TextPattern> .<br /><br /> Os clientes de automação da interface do usuário devem ter cuidado com a validade atual de um intervalo de texto criado usando o <xref:System.Windows.Automation.Text.TextPatternRange> . Se o texto original no controle de texto for completamente substituído pelo novo texto, o intervalo de texto atual se tornará inválido. No entanto, o intervalo de texto ainda pode ter alguma viabilidade se apenas parte do texto original for alterada e o controle de texto subjacente estiver gerenciando seu texto "ponteiro" com âncoras (ou pontos de extremidade) em vez de com o posicionamento de caracteres absoluto.<br /><br /> Os clientes podem escutar uma <xref:System.Windows.Automation.TextPattern.TextChangedEvent> notificação de qualquer alteração no conteúdo textual com a qual estão trabalhando.|
|`System.Windows.Automation.AutomationTextAttribute Class`|Usado para identificar os atributos de formatação de um intervalo de texto.|

<a name="TextPattern_Provider_API_s"></a>

## <a name="textpattern-provider-apis"></a>APIs do provedor de TextPattern

Elementos de interface do usuário ou controles que dão suporte <xref:System.Windows.Automation.TextPattern> ao implementar as <xref:System.Windows.Automation.Provider.ITextProvider> <xref:System.Windows.Automation.Provider.ITextRangeProvider> interfaces e, de forma nativa ou por meio [!INCLUDE[TLA#tla_uiautomation](../../../includes/tlasharptla-uiautomation-md.md)] de proxies, são capazes de expor informações detalhadas de atributo para qualquer texto que eles contenham, além de fornecer recursos robustos de navegação.

Um <xref:System.Windows.Automation.TextPattern> provedor não precisará dar suporte a todos os atributos de texto se o controle não tiver suporte para nenhum atributo específico.

Um <xref:System.Windows.Automation.TextPattern> provedor deve dar suporte <xref:System.Windows.Automation.TextPattern.GetSelection%2A> às <xref:System.Windows.Automation.Text.TextPatternRange.Select%2A> funções e se o controle oferecer suporte à seleção de texto ou ao posicionamento do cursor de texto (ou do cursor do sistema) na área de texto. Se o controle não oferecer suporte a essa funcionalidade, ele não precisará oferecer suporte a nenhum desses métodos. No entanto, o controle deve expor o tipo de seleção de texto compatível com a implementação da <xref:System.Windows.Automation.Provider.ITextProvider.SupportedTextSelection%2A> propriedade.

Um <xref:System.Windows.Automation.TextPattern> provedor deve sempre dar suporte às <xref:System.Windows.Automation.Text.TextUnit> constantes <xref:System.Windows.Automation.Text.TextUnit.Character> e também a <xref:System.Windows.Automation.Text.TextUnit.Document> qualquer outra <xref:System.Windows.Automation.Text.TextUnit> constante com suporte.

> [!NOTE]
> O provedor pode ignorar o suporte para um específico <xref:System.Windows.Automation.Text.TextUnit> , desferindo-se ao próximo maior <xref:System.Windows.Automation.Text.TextUnit> suporte na seguinte ordem:,,,,, <xref:System.Windows.Automation.Text.TextUnit.Character> <xref:System.Windows.Automation.Text.TextUnit.Format> <xref:System.Windows.Automation.Text.TextUnit.Word> <xref:System.Windows.Automation.Text.TextUnit.Line> <xref:System.Windows.Automation.Text.TextUnit.Paragraph> <xref:System.Windows.Automation.Text.TextUnit.Page> e <xref:System.Windows.Automation.Text.TextUnit.Document> .

|||
|-|-|
|`ITextProvider Interface`|Expõe métodos, propriedades e atributos que dão suporte a <xref:System.Windows.Automation.TextPattern> aplicativos cliente (consulte <xref:System.Windows.Automation.Provider.ITextProvider> ).|
|`ITextRangeProvider Interface`|Representa um intervalo de texto em um provedor de texto (consulte <xref:System.Windows.Automation.Provider.ITextRangeProvider> ).|
|`System.Windows.Automation.TextPatternIdentifiers Class`|Contém valores que são usados como identificadores para provedores de texto (consulte <xref:System.Windows.Automation.TextPatternIdentifiers> ).|

<a name="Security"></a>

## <a name="security"></a>Segurança

A [!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)] arquitetura foi projetada tendo em mente a segurança (consulte [visão geral da segurança de automação da interface do usuário](ui-automation-security-overview.md)). No entanto, as classes de TextPattern descritas nesta visão geral exigem algumas considerações de segurança específicas.

- [!INCLUDE[TLA#tla_uiautomation](../../../includes/tlasharptla-uiautomation-md.md)]os provedores de texto fornecem interfaces somente leitura e não fornecem a capacidade de alterar o texto existente em um controle.

- Os clientes de automação da interface do usuário só poderão usar [!INCLUDE[TLA#tla_uiautomation](../../../includes/tlasharptla-uiautomation-md.md)] se forem totalmente "confiáveis". Um exemplo disso seria a área de trabalho de logon protegida, onde somente aplicativos conhecidos e confiáveis podem ser executados.

- Os desenvolvedores de provedores de automação de interface do usuário devem estar cientes de que todas as informações que eles optam por serem expostas em seus controles por meio do [!INCLUDE[TLA#tla_uiautomation](../../../includes/tlasharptla-uiautomation-md.md)] são essencialmente públicas e totalmente acessíveis por outro código. [!INCLUDE[TLA#tla_uiautomation](../../../includes/tlasharptla-uiautomation-md.md)]Não faz nenhum esforço para determinar a confiabilidade de qualquer cliente de automação de interface do usuário e, portanto, o provedor de automação de interface do usuário não deve expor conteúdo protegido ou informações textuais confidenciais (como campos de senha).

- Uma das alterações mais significativas na segurança para o Windows Vista é amplamente conhecida como "entrada segura", que abrange tecnologias como, por exemplo, as contas de usuário menos privilegiadas (ou limitadas) e UIPI (isolamento de nível de privilégio de interface do usuário).

  - O UIPI impede que um programa controle e/ou monitore outro programa "privilegiado", impedindo ataques de mensagem de janela de processo cruzado que falsificam a entrada do usuário.

  - O LUA define os limites dos privilégios dos aplicativos que estão sendo executados pelos usuários no grupo Administradores. Os aplicativos não terão necessariamente privilégios de administrador, mas, em vez disso, serão executados com os privilégios mínimos necessários. Como consequência, pode haver algumas restrições impostas em cenários de LUA. Mais notavelmente o truncamento de cadeia de caracteres (incluindo cadeias de TextPattern), em que pode ser necessário limitar o tamanho das cadeias que estão sendo recuperadas de aplicativos de nível de administrador para que não sejam forçadas a alocar memória até o ponto de desabilitar o aplicativo.

<a name="Performance"></a>

## <a name="performance"></a>Desempenho

Como TextPattern se baseia em chamadas entre processos para a maior parte de sua funcionalidade, ele não fornece um mecanismo de cache para melhorar o desempenho ao processar o conteúdo. Isso é diferente de outros padrões de controle no [!INCLUDE[TLA#tla_uiautomation](../../../includes/tlasharptla-uiautomation-md.md)] que podem ser acessados usando os <xref:System.Windows.Automation.AutomationElement.GetCachedPattern%2A> <xref:System.Windows.Automation.AutomationElement.TryGetCachedPattern%2A> métodos ou.

Uma tática para melhorar o desempenho é garantindo que os clientes de automação da interface do usuário tentem recuperar blocos de texto de tamanho moderado usando <xref:System.Windows.Automation.Text.TextPatternRange.GetText%2A> . Por exemplo, as chamadas de gettext (1) incorrerão em ocorrências entre processos para cada caractere, enquanto uma chamada gettext (-1) incorrerá em uma ocorrência de processo cruzado, mas poderá ter alta latência dependendo do tamanho do provedor de texto.

<a name="Glossary"></a>

## <a name="textpattern-terminology"></a>Terminologia de TextPattern

**Attribute**\
Uma característica de formatação de um intervalo de texto (por exemplo, <xref:System.Windows.Automation.TextPattern.IsItalicAttribute> ou <xref:System.Windows.Automation.TextPattern.FontNameAttribute> ).

**Intervalo de degeneração**\
Um intervalo de degeneração é um intervalo de texto vazio ou com zero caracteres. Para os fins do padrão de controle TextPattern, o ponto de inserção de texto (ou o cursor do sistema) é considerado um intervalo de degeneramento. Se nenhum texto for selecionado, <xref:System.Windows.Automation.TextPattern.GetSelection%2A> retornará um intervalo de degeneração no ponto de inserção de texto e <xref:System.Windows.Automation.TextPattern.RangeFromPoint%2A> retornará um intervalo de degeneração como seu ponto de extremidade inicial. <xref:System.Windows.Automation.TextPattern.RangeFromChild%2A>e <xref:System.Windows.Automation.TextPattern.GetVisibleRanges%2A> pode retornar intervalos de degeneração quando o provedor de texto não encontrar nenhum intervalo de texto que corresponda à condição especificada. Esse intervalo de degeneração pode ser usado como um ponto de extremidade inicial dentro do provedor de texto. <xref:System.Windows.Automation.Text.TextPatternRange.FindText%2A>e <xref:System.Windows.Automation.Text.TextPatternRange.FindAttribute%2A> retorne uma referência nula ( `Nothing` no Microsoft Visual Basic .net) para evitar confusão com um intervalo descoberto em vez de um intervalo de degeneramento.

**Objeto inserido**\
Há dois tipos de objetos inseridos no [!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)] modelo de texto. Eles consistem em elementos de conteúdo baseados em texto, como hiperlinks ou tabelas, e elementos de controle, como imagens e botões. Para obter informações mais detalhadas, consulte [acessar objetos inseridos usando a automação da interface do usuário](access-embedded-objects-using-ui-automation.md).

**Extremidade**\
A <xref:System.Windows.Automation.Text.TextPatternRangeEndpoint.Start> parte absoluta ou o <xref:System.Windows.Automation.Text.TextPatternRangeEndpoint.End> ponto de um intervalo de texto dentro de um contêiner de texto.

![TextPatternRangeEndpoints &#40;&#41; de início e término.](./media/uia-textpattern-endpoints.PNG "UIA_TextPattern_Endpoints")
O seguinte ilustra um conjunto de pontos inicial e final.

**TextRange**\
Uma representação de um intervalo de texto, com pontos inicial e final, em um contêiner de texto, incluindo todos os atributos e funcionalidades associados.

<xref:System.Windows.Automation.Text.TextUnit>\
Uma unidade de texto predefinida (caractere, palavra, linha ou parágrafo) usada para navegar por segmentos lógicos de um intervalo de texto.

## <a name="see-also"></a>Veja também

- [Padrões de Controle para Clientes de Automação de IU](ui-automation-control-patterns-for-clients.md)
- [Visão Geral de Padrões de Controle de Automação de Interface de Usuário](ui-automation-control-patterns-overview.md)
- [Visão geral da árvore de automação de interface do usuário](ui-automation-tree-overview.md)
- [Usar armazenamento em cache em automação de interface do usuário](use-caching-in-ui-automation.md)
- [Padrões de controle de suporte em um provedor de automação da interface do usuário](support-control-patterns-in-a-ui-automation-provider.md)
- [Mapeamento de Padrão de Controles para Clientes de Automação de IU](control-pattern-mapping-for-ui-automation-clients.md)
- [Estrutura de serviços de texto](/windows/desktop/api/_tsf/)
