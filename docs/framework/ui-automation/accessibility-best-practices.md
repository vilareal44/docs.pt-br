---
title: Práticas recomendadas de Acessibilidade
description: Saiba mais sobre as práticas recomendadas de acessibilidade no .NET. Explore o acesso programático, as configurações do usuário, o design da interface de usuário visual, a navegação e as interfaces multimodal.
ms.date: 03/30/2017
helpviewer_keywords:
- best practices for accessibility
- accessibility, best practices for
ms.assetid: e6d5cd98-21a3-4b01-999c-fb953556d0e6
ms.openlocfilehash: 2980881bbcd34ca82f6cca7723cf976e0890f463
ms.sourcegitcommit: b7a8b09828bab4e90f66af8d495ecd7024c45042
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/04/2020
ms.locfileid: "87557080"
---
# <a name="accessibility-best-practices"></a>Práticas recomendadas de acessibilidade

> [!NOTE]
> Este artigo destina-se a .NET Framework desenvolvedores que desejam usar as classes de automação de interface do usuário gerenciadas definidas no <xref:System.Windows.Automation> namespace. Para obter as informações mais recentes sobre automação da interface do usuário, consulte [API de automação do Windows: automação da interface do usuário](/windows/win32/winauto/entry-uiauto-win32).  
  
 A implementação das seguintes práticas recomendadas em controles ou aplicativos melhorará sua acessibilidade para as pessoas que usam dispositivos de tecnologia assistencial. Muitas dessas práticas recomendadas se concentram em um bom design de interface do usuário (IU). Cada prática recomendada inclui informações de implementação para controles de Windows Presentation Foundation (WPF) ou aplicativos. Em muitos casos, o trabalho para atender a essas práticas recomendadas já está incluído nos controles do WPF.  
  
<a name="Programmatic_Access"></a>
## <a name="programmatic-access"></a>Acesso Programático  
 O acesso programático envolve garantir que todos os elementos da interface do usuário sejam rotulados, que os valores de propriedade sejam expostos e que os eventos apropriados sejam gerados. Para controles padrão do WPF, a maior parte desse trabalho já foi feita por meio do <xref:System.Windows.Automation.Peers.AutomationPeer> . Os controles personalizados exigem trabalho adicional para garantir que o acesso programático seja implementado corretamente.  
  
<a name="Enable_Programmatic_Access_to_all_UI_Elements_and_Text"></a>
### <a name="enable-programmatic-access-to-all-ui-elements-and-text"></a>Habilitar o acesso programático a todos os elementos e texto da interface do usuário  
 Elementos da interface do usuário devem habilitar o acesso programático. Se a interface do usuário for um controle padrão do WPF, o suporte ao acesso programático será incluído no controle. Se o controle for um controle personalizado – um controle que tenha sido subclasse de um controle comum ou um controle que tenha sido subclasse do controle – em seguida, você deve verificar a <xref:System.Windows.Automation.Peers.AutomationPeer> implementação de áreas que podem precisar de modificação.  
  
 Seguir essa prática recomendada permite que fornecedores de tecnologia assistencial identifiquem e manipulem elementos da interface do usuário do produto.  
  
<a name="Place_Names__Titles_and_Descriptions_on_UI_Objects_"></a>
### <a name="place-names-titles-and-descriptions-on-ui-objects-frames-and-pages"></a>Coloque nomes, títulos e descrições em objetos, quadros e páginas da interface do usuário  
 Tecnologias assistenciais, especialmente leitores de tela, usam o título para entender o local do quadro, objeto ou página no esquema de navegação. Portanto, o título deve ser descritivo. Por exemplo, um título de página da Web de "página da Web da Microsoft" é inútil se o usuário navegou profundamente em alguma área específica. Um título descritivo é essencial para os usuários que são cegos e dependem de leitores de tela. Da mesma forma, para controles WPF, <xref:System.Windows.Automation.AutomationProperties.NameProperty> e <xref:System.Windows.Automation.AutomationProperties.HelpTextProperty> são importantes para dispositivos de tecnologia assistencial.  
  
 Seguir essa prática recomendada permite que tecnologias assistenciais identifiquem e manipulem a interface do usuário em aplicativos e controles de exemplo.  
  
<a name="Ensure_Programmatic_Events_are_Triggered_by_all_UI"></a>
### <a name="ensure-programmatic-events-are-triggered-by-all-ui-activities"></a>Garantir que eventos programáticos sejam disparados por todas as atividades de IU  
 Seguir essa prática recomendada permite que as tecnologias assistenciais escutem as alterações na interface do usuário e notifique-o sobre essas alterações.  
  
<a name="User_Settings"></a>
## <a name="user-settings"></a>Configurações do usuário  
 A prática recomendada nesta seção garante que os controles ou aplicativos não substituam as configurações do usuário.  
  
<a name="Respect_all_System_Wide_Settings_and_do_not_Interfere"></a>
### <a name="respect-all-system-wide-settings-and-do-not-interfere-with-accessibility-functions"></a>Respeitar todas as configurações de todo o sistema e não interferem nas funções de acessibilidade  
 Os usuários podem usar o painel de controle para definir alguns sinalizadores em todo o sistema; outros sinalizadores podem ser definidos programaticamente. Essas configurações não devem ser alteradas por controles ou aplicativos. Além disso, os aplicativos devem dar suporte às configurações de acessibilidade do sistema operacional do host.  
  
 Seguir essa prática recomendada permite que os usuários definam as configurações de acessibilidade e saibam que essas configurações não serão alteradas pelos aplicativos.  
  
<a name="Visual_UI_Design"></a>
## <a name="visual-ui-design"></a>Design de interface do usuário Visual  
 As práticas recomendadas nesta seção garantem que os controles ou aplicativos usem cores e imagens com eficiência e possam ser usados por tecnologias assistenciais.  
  
<a name="Don_t_Hard_Code_Colors"></a>
### <a name="dont-hard-code-colors"></a>Não Embutir Cores  
 As pessoas que têm cores cegas, têm pouca visão ou estão usando uma tela preta e branca podem não ser capazes de usar aplicativos com cores embutidas em código.  
  
 Seguir essa prática recomendada permite que os usuários ajustem combinações de cores com base em necessidades individuais.  
  
<a name="Support_High_Contrast_and_all_System_Display_Attributes"></a>
### <a name="support-high-contrast-and-all-system-display-attributes"></a>Suporte a Alto Contraste e todos os atributos de exibição do sistema  
 Os aplicativos não devem interromper ou desabilitar as configurações de contraste de todo o sistema, selecionadas pelo usuário, seleções de cores ou outras configurações de vídeo e atributos de todo o sistema. As configurações de todo o sistema adotadas por um usuário aprimoram a acessibilidade dos aplicativos, portanto, eles não devem ser desabilitados ou desconsiderados por aplicativos. A cor deve ser usada em sua combinação correta de primeiro plano em segundo plano para fornecer um contraste adequado. Não misture cores não relacionadas e não Reverta as cores.  
  
 Muitos usuários exigem combinações específicas de alto contraste, como texto em branco em um plano de fundo preto. Desenhar esses invertidos, como texto preto em um plano de fundo branco, faz com que o plano de fundo seja sangrado em primeiro plano e pode dificultar a leitura para alguns usuários.  
  
<a name="Ensure_all_UI_Correctly_Scales_by_any_DPI_Setting"></a>
### <a name="ensure-all-ui-correctly-scales-by-any-dpi-setting"></a>Garantir que toda a interface do usuário seja dimensionada corretamente por qualquer configuração de DPI  
 Verifique se toda a interface do usuário pode ser dimensionada corretamente por qualquer configuração de pontos por polegada (DPI). Além disso, verifique se os elementos da interface do usuário se encaixam em uma tela de 1024 x 768 com 120 pontos por polegada (DPI).  
  
<a name="Navigation"></a>
## <a name="navigation"></a>Navegação  
 As práticas recomendadas nesta seção garantem que a navegação tenha sido resolvida para controles e aplicativos.  
  
<a name="Provide_Keyboard_Interface_for_all_UI_Elements"></a>
### <a name="provide-keyboard-interface-for-all-ui-elements"></a>Fornecer interface de teclado para todos os elementos da interface do usuário  
 As paradas de tabulação, especialmente quando planejadas cuidadosamente, dão aos usuários outra maneira de navegar pela interface do usuário.  
  
 Os aplicativos devem fornecer as seguintes interfaces de teclado:  
  
- tabulações para todos os controles com os quais o usuário pode interagir, como botões, links ou caixas de listagem  
  
- ordem de tabulação lógica  
  
<a name="Show_the_Keyboard_Focus"></a>
### <a name="show-the-keyboard-focus"></a>Mostrar o foco do teclado  
 Os usuários precisam saber qual objeto tem o foco do teclado para que eles possam prever o efeito de seus pressionamentos de teclas. Para destacar o foco do teclado, use cores, fontes ou elementos gráficos, como retângulos ou ampliação. Para forma audível realçar o foco do teclado, altere o volume, a densidade ou a qualidade de tons.  
  
 Para evitar confusão, os aplicativos devem ocultar todos os indicadores de foco visual e as seleções Dim que estão localizadas em janelas inativas (ou painéis).  
  
 Os aplicativos devem fazer o seguinte com o foco do teclado:  
  
- um item sempre deve ter o foco do teclado  
  
- o foco do teclado deve ser visível e óbvio  
  
- as seleções e/ou os itens focados devem ser visualmente realçados  
  
<a name="Support_Navigation_Standards_and_Powerful_Navigation"></a>
### <a name="support-navigation-standards-and-powerful-navigation-schemes"></a>Suporte a padrões de navegação e esquemas de navegação avançados  
 Diferentes aspectos da navegação por teclado fornecem maneiras diferentes para os usuários navegarem pela interface do usuário.  
  
 Os aplicativos devem fornecer as seguintes interfaces de teclado:  
  
- teclas de atalho e chaves de acesso sublinhadas para todos os comandos, menus e controles  
  
- atalhos de teclado para links importantes  
  
- todos os itens de menu têm uma chave de acesso; todos os botões têm teclas de aceleração, todos os comandos têm uma tecla aceleradora.  
  
<a name="Do_not_let_Mouse_Location_Interfere_with_Keyboard"></a>
### <a name="do-not-let-mouse-location-interfere-with-keyboard-navigation"></a>Não permitir que o local do mouse interfira na navegação do teclado  
 O local do mouse não deve interferir na navegação do teclado. Por exemplo, se o mouse estiver posicionado em algum lugar e o usuário estiver navegando com o teclado, um clique do mouse não deverá ocorrer a menos que seja iniciado pelo usuário.  
  
<a name="Multimodal_Interface"></a>
## <a name="multimodal-interface"></a>Interface multimodal  
 As práticas recomendadas nesta seção garantem que a interface do usuário do aplicativo inclua alternativas para elementos visuais.  
  
<a name="Provide_User_Selectable_Equivalents_for_Non_Text"></a>
### <a name="provide-user-selectable-equivalents-for-non-text-elements"></a>Fornecer equivalentes selecionáveis pelo usuário para elementos que não são de texto  
 Para cada elemento que não seja texto, forneça um equivalente selecionável pelo usuário para texto, transcrições ou descrições de áudio, como texto alternativo, legendas ou comentários visuais.  
  
 Elementos sem texto abrangem uma ampla gama de elementos da interface do usuário, incluindo: imagens, regiões de mapa de imagem, animações, applets, quadros, scripts, botões gráficos, sons, arquivos de áudio autônomos e vídeo. Elementos que não são de texto são importantes quando contêm informações visuais, fala ou informações gerais de áudio que o usuário precisa acessar para entender o conteúdo da interface do usuário.  
  
<a name="Use_Color_but_also_Provide_Alternatives_to_Color"></a>
### <a name="use-color-but-also-provide-alternatives-to-color"></a>Usar cor, mas também fornecer alternativas para cor  
 Use cores para aprimorar, enfatizar ou reiterar as informações mostradas por outros meios, mas não comunicar informações usando apenas a cor. Os usuários que têm cores cegas ou têm um monitor monocromático precisam de alternativas para cores.  
  
<a name="Use_Standard_Input_APIs_with_Devices_Independent"></a>
### <a name="use-standard-input-apis-with-device-independent-calls"></a>Usar APIs de entrada padrão com chamadas independentes de dispositivo  
 As chamadas independentes de dispositivo garantem a igualdade de recursos de teclado e mouse, fornecendo à tecnologia assistencial as informações necessárias sobre a interface do usuário.  
  
## <a name="see-also"></a>Confira também

- <xref:System.Windows.Automation.Peers>
- [Exemplo de controle personalizado NumericUpDown com tema e suporte de Automação da Interface do Usuário](https://docs.microsoft.com/previous-versions/dotnet/netframework-3.5/ms771573(v=vs.90))
- [Diretrizes para design de interface de usuário de teclado](https://docs.microsoft.com/previous-versions/windows/desktop/dnacc/guidelines-for-keyboard-user-interface-design)
