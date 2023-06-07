---
title: TextPattern Visão geral de objetos inseridos
description: Leia uma visão geral de como a automação da interface do usuário expõe objetos inseridos ou elementos filho, dentro de um documento de texto ou contêiner usando TextPattern e TextPatternRange.
ms.date: 03/30/2017
helpviewer_keywords:
- UI Automation, TextPattern class
- embedded objects, accessing
- accessing embedded objects
- embedded objects, UI Automation
ms.assetid: 93fdfbb9-0025-4b72-8ca0-0714adbb70d5
ms.openlocfilehash: 0a06fb72b280fc61faeb12f6f2c3a05d957ec7b9
ms.sourcegitcommit: 87cfeb69226fef01acb17c56c86f978f4f4a13db
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/24/2020
ms.locfileid: "87163564"
---
# <a name="textpattern-and-embedded-objects-overview"></a>TextPattern Visão geral de objetos inseridos
> [!NOTE]
> Esta documentação destina-se a desenvolvedores do .NET Framework que querem usar as classes da [!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)] gerenciadas definidas no namespace <xref:System.Windows.Automation>. Para obter as informações mais recentes sobre a [!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)], consulte [Windows Automation API: UI Automation](/windows/win32/winauto/entry-uiauto-win32) (API de Automação do Windows: Automação da Interface do Usuário).  
  
 Esta visão geral descreve como o [!INCLUDE[TLA#tla_uiautomation](../../../includes/tlasharptla-uiautomation-md.md)] expõe objetos inseridos ou elementos filho, dentro de um documento de texto ou contêiner.  
  
 Em [!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)] um objeto incorporado, há qualquer elemento que tenha limites não textuais; por exemplo, uma imagem, um hiperlink, uma tabela ou um tipo de documento, como uma planilha do Microsoft Excel ou um arquivo do Microsoft Windows Media. Isso difere da definição padrão, em que um elemento é criado em um aplicativo e inserido, ou vinculado, dentro de outro. Se o objeto pode ser editado em seu aplicativo original é irrelevante no contexto de [!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)] .  
  
<a name="Embedded_Objects_and_the_UI_Automation_Tree"></a>
## <a name="embedded-objects-and-the-ui-automation-tree"></a>Objetos incorporados e a árvore de automação da interface do usuário  
 Os objetos inseridos são tratados como elementos individuais dentro da exibição de controle da [!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)] árvore. Eles são expostos como filhos do contêiner de texto para que possam ser acessados por meio do mesmo modelo que outros controles no [!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)] .  
  
 ![Tabela inserida com imagem em um contêiner de texto](./media/uia-textpattern-embedded-objects-overview-example1.png "UIA_TextPattern_Embedded_Objects_Overview_Example1")  
Exemplo de um contêiner de texto com objetos de tabela, imagem e hiperlink inseridos  
  
 ![Exibição de conteúdo para o exemplo anterior](./media/uia-textpattern-embedded-objects-overview-example2.PNG "UIA_TextPattern_Embedded_Objects_Overview_Example2")  
Exemplo da exibição de conteúdo para uma parte do contêiner de texto anterior  
  
<a name="Expose_Embedded_Objects_Using_TextPattern_and"></a>
## <a name="expose-embedded-objects-using-textpattern-and-textpatternrange"></a>Expor objetos inseridos usando TextPattern e TextPatternRange  
 Usado em conjunto, a <xref:System.Windows.Automation.TextPattern> classe de padrão de controle e a <xref:System.Windows.Automation.Text.TextPatternRange> classe expõem métodos e propriedades que facilitam a navegação e a consulta de objetos inseridos.  
  
 O conteúdo textual (ou texto interno) de um contêiner de texto e um objeto incorporado, como um hiperlink ou célula de tabela, é exposto como um fluxo de texto único e contínuo na exibição de controle e na exibição de conteúdo da [!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)] árvore; os limites de objeto são ignorados. Se um cliente de automação de interface do usuário estiver recuperando o texto com a finalidade de citar, interpretar ou analisar de alguma maneira, o intervalo de texto deverá ser verificado em casos especiais, como uma tabela com conteúdo textual ou outros objetos incorporados. Isso pode ser feito chamando <xref:System.Windows.Automation.Text.TextPatternRange.GetChildren%2A> para obter um <xref:System.Windows.Automation.AutomationElement> para cada objeto inserido e, em seguida, chamar <xref:System.Windows.Automation.TextPattern.RangeFromChild%2A> para obter um intervalo de texto para cada elemento. Isso é feito recursivamente até que todo o conteúdo textual seja recuperado.  
  
 ![Intervalos de texto abrangidos por objetos inseridos.](./media/uia-textpattern-embeddedobjecttextranges.png "UIA_TextPattern_EmbeddedObjectTextRanges")  
Exemplo de um fluxo de texto com objetos incorporados e seus intervalos de intervalo  
  
 Quando for necessário percorrer o conteúdo de um intervalo de texto, uma série de etapas será envolvida nos bastidores para que o <xref:System.Windows.Automation.Text.TextPatternRange.Move%2A> método seja executado com êxito.  
  
1. O intervalo de texto é normalizado; ou seja, o intervalo de texto é recolhido para um intervalo de degeneração no <xref:System.Windows.Automation.Text.TextPatternRangeEndpoint.Start> ponto de extremidade, o que torna o <xref:System.Windows.Automation.Text.TextPatternRangeEndpoint.End> ponto de extremidade supérfluo. Essa etapa é necessária para remover a ambiguidade em situações em que um intervalo de texto abrange <xref:System.Windows.Automation.Text.TextUnit> limites: por exemplo, `{The URL https://www.microsoft.com is embedded in text` onde "{" e "}" são os pontos de extremidade do intervalo de texto.  
  
2. O intervalo resultante é movido para trás no <xref:System.Windows.Automation.TextPattern.DocumentRange%2A> até o início do limite solicitado <xref:System.Windows.Automation.Text.TextUnit> .  
  
3. O intervalo é movido para frente ou para trás no <xref:System.Windows.Automation.TextPattern.DocumentRange%2A> pelo número solicitado de <xref:System.Windows.Automation.Text.TextUnit> limites.  
  
4. O intervalo é então expandido de um estado de intervalo degenerado movendo o <xref:System.Windows.Automation.Text.TextPatternRangeEndpoint.End> ponto de extremidade por um <xref:System.Windows.Automation.Text.TextUnit> limite solicitado.  
  
 ![Ajustes de intervalo por movimento & ExpandToEnclosingUnit](./media/uia-textpattern-moveandexpand-examples.png "UIA_TextPattern_MoveAndExpand_Examples")  
Exemplos de como um intervalo de texto é ajustado para Move () e ExpandToEnclosingUnit ()  
  
<a name="Common_Scenarios"></a>
## <a name="common-scenarios"></a>Cenários comuns  
 As seções a seguir apresentam exemplos dos cenários mais comuns que envolvem objetos incorporados.  
  
 Legenda para os exemplos mostrados:  
  
 { =<xref:System.Windows.Automation.Text.TextPatternRangeEndpoint.Start>  
  
 } =<xref:System.Windows.Automation.Text.TextPatternRangeEndpoint.End>  
  
### <a name="hyperlink"></a>Hyperlink  

**Exemplo 1 – um intervalo de texto que contém um hiperlink de texto inserido**
  
`{The URL https://www.microsoft.com is embedded in text}.`
  
|Método chamado|Resultado|  
|-------------------|------------|  
|<xref:System.Windows.Automation.Text.TextPatternRange.GetText%2A>|Retorna a cadeia de caracteres `The URL https://www.microsoft.com is embedded in text`.|  
|<xref:System.Windows.Automation.Text.TextPatternRange.GetEnclosingElement%2A>|Retorna o <xref:System.Windows.Automation.AutomationElement> mais interno que inclui o intervalo de texto; nesse caso, o <xref:System.Windows.Automation.AutomationElement> que representa o próprio provedor de texto.|  
|<xref:System.Windows.Automation.Text.TextPatternRange.GetChildren%2A>|Retorna um <xref:System.Windows.Automation.AutomationElement> representando o controle de hiperlink.|  
|<xref:System.Windows.Automation.TextPattern.RangeFromChild%2A>em que <xref:System.Windows.Automation.AutomationElement> é o objeto retornado pelo `GetChildren` método anterior.|Retorna o intervalo que representa `https://www.microsoft.com` .|  
  
 **Exemplo 2 – um intervalo de texto que abrange parcialmente um hiperlink de texto inserido**  
  
 A URL `https://{[www]}` é inserida em texto.  
  
|Método chamado|Resultado|  
|-------------------|------------|  
|<xref:System.Windows.Automation.Text.TextPatternRange.GetText%2A>|Retorna a cadeia de caracteres "www".|  
|<xref:System.Windows.Automation.Text.TextPatternRange.GetEnclosingElement%2A>|Retorna o <xref:System.Windows.Automation.AutomationElement> mais interno que inclui o intervalo de texto; nesse caso, o controle HyperLink.|  
|<xref:System.Windows.Automation.Text.TextPatternRange.GetChildren%2A>|Retorna `null` porque o intervalo de texto não abrange a cadeia de caracteres da URL inteira.|  
  
**Exemplo 3-um intervalo de texto que abrange parcialmente o conteúdo de um contêiner de texto. O contêiner de texto tem um hiperlink de texto inserido que não faz parte do intervalo de texto.**  
  
`{The URL} [https://www.microsoft.com](https://www.microsoft.com) is embedded in text.`
  
|Método chamado|Resultado|  
|-------------------|------------|  
|<xref:System.Windows.Automation.Text.TextPatternRange.GetText%2A>|Retorna a cadeia de caracteres "a URL".|  
|<xref:System.Windows.Automation.Text.TextPatternRange.GetEnclosingElement%2A>|Retorna o <xref:System.Windows.Automation.AutomationElement> mais interno que inclui o intervalo de texto; nesse caso, o <xref:System.Windows.Automation.AutomationElement> que representa o próprio provedor de texto.|  
|<xref:System.Windows.Automation.Text.TextPatternRange.Move%2A>com parâmetros de (TextUnit. Word, 1).|Move o intervalo de texto span para "http", pois o texto do hiperlink é composto por palavras individuais. Nesse caso, o hiperlink não é tratado como um único objeto.<br /><br /> A URL {[http]} está inserida no texto.|  
  
<a name="Image"></a>
### <a name="image"></a>Image  
 **Exemplo 1 – um intervalo de texto que contém uma imagem inserida**  
  
 {O ![exemplo de imagem inserida](./media/uia-textpattern-embedded-objects-overview-imageexample.PNG "UIA_TextPattern_Embedded_Objects_Overview_ImageExample") da imagem é inserido em texto}.  
  
|Método chamado|Resultado|  
|-------------------|------------|  
|<xref:System.Windows.Automation.Text.TextPatternRange.GetText%2A>|Retorna a cadeia de caracteres "o é inserido em texto". Qualquer texto alternativo associado à imagem não pode ser incluído no fluxo de texto.|  
|<xref:System.Windows.Automation.Text.TextPatternRange.GetEnclosingElement%2A>|Retorna o <xref:System.Windows.Automation.AutomationElement> mais interno que inclui o intervalo de texto; nesse caso, o <xref:System.Windows.Automation.AutomationElement> que representa o próprio provedor de texto.|  
|<xref:System.Windows.Automation.Text.TextPatternRange.GetChildren%2A>|Retorna um <xref:System.Windows.Automation.AutomationElement> representando o controle de imagem.|  
|<xref:System.Windows.Automation.TextPattern.RangeFromChild%2A>em que <xref:System.Windows.Automation.AutomationElement> é o objeto retornado pelo <xref:System.Windows.Automation.Text.TextPatternRange.GetChildren%2A> método anterior.|Retorna o intervalo de degeneração que representa "![exemplo de imagem inserida](./media/uia-textpattern-embedded-objects-overview-imageexample.PNG "UIA_TextPattern_Embedded_Objects_Overview_ImageExample")".|  
  
 **Exemplo 2 – um intervalo de texto que abrange parcialmente o conteúdo de um contêiner de texto. O contêiner de texto tem uma imagem inserida que não faz parte do intervalo de texto.**  
  
 {A imagem} O ![exemplo de imagem inserida](./media/uia-textpattern-embedded-objects-overview-imageexample.PNG "UIA_TextPattern_Embedded_Objects_Overview_ImageExample") é inserido no texto.  
  
|Método chamado|Resultado|  
|-------------------|------------|  
|<xref:System.Windows.Automation.Text.TextPatternRange.GetText%2A>|Retorna a cadeia de caracteres "a imagem".|  
|<xref:System.Windows.Automation.Text.TextPatternRange.GetEnclosingElement%2A>|Retorna o <xref:System.Windows.Automation.AutomationElement> mais interno que inclui o intervalo de texto; nesse caso, o <xref:System.Windows.Automation.AutomationElement> que representa o próprio provedor de texto.|  
|<xref:System.Windows.Automation.Text.TextPatternRange.Move%2A>com parâmetros de (TextUnit. Word, 1).|Move o intervalo de intervalos de texto para "is". Como apenas objetos inseridos baseados em texto são considerados parte do fluxo de texto, a imagem neste exemplo não afeta a movimentação ou o valor de retorno (1 nesse caso).|  
  
<a name="Table"></a>
### <a name="table"></a>Tabela  
  
### <a name="table-used-for-examples"></a>Tabela usada para obter exemplos  
  
|Célula com imagem|Célula com texto|  
|---------------------|--------------------|  
|![Exemplo de imagem inserida](./media/uia-textpattern-embedded-objects-overview-imageexample.PNG "UIA_TextPattern_Embedded_Objects_Overview_ImageExample")|X|  
|![Exemplo de imagem inserida 2](./media/uia-textpattern-embedded-objects-overview-imageexample2.PNG "UIA_TextPattern_Embedded_Objects_Overview_ImageExample2")|Y|  
|![Exemplo de imagem inserida 3](./media/uia-textpattern-embedded-objects-overview-imageexample3.PNG "UIA_TextPattern_Embedded_Objects_Overview_ImageExample3")<br /><br /> Imagem para Z|Z|  
  
 **Exemplo 1-obter o contêiner de texto do conteúdo de uma célula.**  
  
|Método chamado|Resultado|  
|-------------------|------------|  
|<xref:System.Windows.Automation.GridPattern.GetItem%2A>com parâmetros (0, 0)|Retorna o <xref:System.Windows.Automation.AutomationElement> que representa o conteúdo da célula da tabela; nesse caso, o elemento é um controle de texto.|  
|<xref:System.Windows.Automation.TextPattern.RangeFromChild%2A>em que <xref:System.Windows.Automation.AutomationElement> é o objeto retornado pelo `GetItem` método anterior.|Retorna o intervalo que abrange o exemplo de ![imagem inserida](./media/uia-textpattern-embedded-objects-overview-imageexample.PNG "UIA_TextPattern_Embedded_Objects_Overview_ImageExample")da imagem.|  
|<xref:System.Windows.Automation.Text.TextPatternRange.GetEnclosingElement%2A>para o objeto retornado pelo método anterior `RangeFromChild` .|Retorna o <xref:System.Windows.Automation.AutomationElement> que representa a célula da tabela; nesse caso, o elemento é um controle de texto que dá suporte a TableItemPattern.|  
|<xref:System.Windows.Automation.Text.TextPatternRange.GetEnclosingElement%2A>para o objeto retornado pelo método anterior `GetEnclosingElement` .|Retorna o <xref:System.Windows.Automation.AutomationElement> que representa a tabela.|  
|<xref:System.Windows.Automation.Text.TextPatternRange.GetEnclosingElement%2A>para o objeto retornado pelo método anterior `GetEnclosingElement` .|Retorna o <xref:System.Windows.Automation.AutomationElement> que representa o próprio provedor de texto.|  
  
 **Exemplo 2 – obter o conteúdo de texto de uma célula.**  
  
|Método chamado|Resultado|  
|-------------------|------------|  
|<xref:System.Windows.Automation.GridPattern.GetItem%2A>com parâmetros de (1, 1).|Retorna o <xref:System.Windows.Automation.AutomationElement> que representa o conteúdo da célula da tabela; nesse caso, o elemento é um controle de texto.|  
|<xref:System.Windows.Automation.TextPattern.RangeFromChild%2A>em que <xref:System.Windows.Automation.AutomationElement> é o objeto retornado pelo `GetItem` método anterior.|Retorna "Y".|  
  
## <a name="see-also"></a>Confira também

- <xref:System.Windows.Automation.TextPattern>
- <xref:System.Windows.Automation.Text.TextPatternRange>
- <xref:System.Windows.Automation.Provider.ITextProvider>
- <xref:System.Windows.Automation.Provider.ITextRangeProvider>
- [Acessar objetos inseridos usando automação de interface de usuário](access-embedded-objects-using-ui-automation.md)
- [Expor o conteúdo de uma tabela usando automação de interface do usuário](expose-the-content-of-a-table-using-ui-automation.md)
- [Percorrer texto usando automação de interface do usuário](traverse-text-using-ui-automation.md)
- [Exemplo de pesquisa e seleção de TextPattern](https://github.com/Microsoft/WPF-Samples/tree/master/Accessibility/FindText)
