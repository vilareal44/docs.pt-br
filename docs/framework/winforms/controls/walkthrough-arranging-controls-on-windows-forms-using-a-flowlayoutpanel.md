---
title: Organizar controles usando o FlowLayoutPanel
description: Saiba como usar o controle FlowLayoutPanel e o controle TableLayoutPanel para fornecer maneiras intuitivas de organizar controles em seu projeto de Windows Forms.
ms.date: 03/30/2017
helpviewer_keywords:
- FlowLayoutPanel control [Windows Forms], walkthroughs
- Windows Forms controls, arranging
- controls [Windows Forms], arranging with FlowLayoutPanel
- layout [Windows Forms], walkthroughs
ms.assetid: a1744323-0316-49c2-992e-ebfc0a976b85
ms.openlocfilehash: efcb38275be7b0cf94afb6b68aa139876f7cf5fd
ms.sourcegitcommit: e02d17b2cf9c1258dadda4810a5e6072a0089aee
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/01/2020
ms.locfileid: "85619280"
---
# <a name="walkthrough-arranging-controls-on-windows-forms-using-a-flowlayoutpanel"></a>Explicação passo a passo: organizando controles no Windows Forms utilizando um FlowLayoutPanel

Alguns aplicativos exigem um formulário com um layout que se organiza adequadamente à medida que o formulário é redimensionado ou conforme o tamanho do conteúdo é alterado. Quando você precisar de um layout dinâmico e não quiser manipular <xref:System.Windows.Forms.Control.Layout> eventos explicitamente em seu código, considere o uso de um painel de layout.

O <xref:System.Windows.Forms.FlowLayoutPanel> controle e o <xref:System.Windows.Forms.TableLayoutPanel> controle fornecem maneiras intuitivas de organizar controles em seu formulário. Ambos fornecem uma capacidade automática e configurável de controlar as posições relativas dos controles filho contidos neles e ambos oferecem recursos de layout dinâmico em tempo de execução, para que eles possam redimensionar e reposicionar os controles filho conforme as dimensões do formulário pai se alteram. Os painéis de layout podem ser aninhados em painéis de layout, para permitir a realização de interfaces do usuário sofisticadas.

O <xref:System.Windows.Forms.TableLayoutPanel> organiza seu conteúdo em uma grade, fornecendo funcionalidade semelhante ao \<table> elemento HTML. Suas células são organizadas em linhas e colunas, que podem ter diferentes tamanhos. Para obter mais informações, consulte [Passo a passo: organizando controles nos Windows Forms usando um TableLayoutPanel](walkthrough-arranging-controls-on-windows-forms-using-a-tablelayoutpanel.md).

O <xref:System.Windows.Forms.FlowLayoutPanel> organiza seu conteúdo em uma direção de fluxo específica: horizontal ou vertical. Seu conteúdo pode ser encapsulado de uma linha à outra ou de uma coluna à próxima. Como alternativa, seu conteúdo pode ser recortado, em vez de encapsulado. As tarefas ilustradas neste passo a passo incluem:

- Criação de um projeto dos Windows Forms

- Organizando controles horizontalmente e verticalmente

- Alterando a direção do fluxo

- Inserindo quebras de fluxo

- Organizando controles usando preenchimento e margens

- Inserindo controles ao clicar duas vezes neles na caixa de ferramentas

- Inserindo um controle ao desenhar seu contorno

- Inserindo controles usando o sinal de interpolação

- Reatribuição de controles existentes a um pai diferente

Quando terminar, você terá uma compreensão da função desempenhada por esses recursos de layout importantes.

## <a name="create-the-project"></a>Criar o projeto

1. No Visual Studio, crie um projeto de aplicativo baseado no Windows chamado "FlowLayoutPanelExample" (**arquivo**  >  **novo**  >  **projeto**  >  **Visual C#** ou **Visual Basic**  >  **área de trabalho clássica**  >  **Windows Forms aplicativo**).

2. Selecione o formulário no **Designer de Formulários**.

## <a name="arranging-controls-horizontally-and-vertically"></a>Organizando controles horizontalmente e verticalmente
 O <xref:System.Windows.Forms.FlowLayoutPanel> controle permite colocar controles ao longo de linhas ou colunas sem exigir que você especifique precisamente a posição de cada controle individual.

 O <xref:System.Windows.Forms.FlowLayoutPanel> controle pode redimensionar ou refluir seus controles filho à medida que as dimensões do formulário pai são alteradas.

### <a name="to-arrange-controls-horizontally-and-vertically-using-a-flowlayoutpanel"></a>Para organizar controles horizontalmente e verticalmente usando um FlowLayoutPanel

1. Arraste um <xref:System.Windows.Forms.FlowLayoutPanel> controle da **caixa de ferramentas** para seu formulário.

2. Arraste um <xref:System.Windows.Forms.Button> controle da **caixa de ferramentas** para o <xref:System.Windows.Forms.FlowLayoutPanel> . Observe que ele é movido automaticamente para o canto superior esquerdo do <xref:System.Windows.Forms.FlowLayoutPanel> controle.

3. Arraste outro <xref:System.Windows.Forms.Button> controle da **caixa de ferramentas** para o <xref:System.Windows.Forms.FlowLayoutPanel> . Observe que o <xref:System.Windows.Forms.Button> controle é movido automaticamente para uma posição ao lado do primeiro <xref:System.Windows.Forms.Button> controle. Se seu <xref:System.Windows.Forms.FlowLayoutPanel> for muito estreito para ajustar os dois controles na mesma linha, o novo <xref:System.Windows.Forms.Button> controle será automaticamente movido para a próxima linha.

4. Arraste vários outros <xref:System.Windows.Forms.Button> controles da **caixa de ferramentas** para o <xref:System.Windows.Forms.FlowLayoutPanel> . Continue colocando os <xref:System.Windows.Forms.Button> controles até que um passe para a próxima linha.

5. Altere o valor da propriedade <xref:System.Windows.Forms.FlowLayoutPanel> do controle <xref:System.Windows.Forms.FlowLayoutPanel.WrapContents%2A> para `false`. Observe que os controles filho não fluem mais para a próxima linha. Em vez disso, eles são movidos para a primeira linha e recortados.

6. Altere o valor da propriedade <xref:System.Windows.Forms.FlowLayoutPanel> do controle <xref:System.Windows.Forms.FlowLayoutPanel.WrapContents%2A> para `true`. Observe que os controles filho quebram novamente para a próxima linha.

7. Diminua a largura do <xref:System.Windows.Forms.FlowLayoutPanel> controle até que todos os <xref:System.Windows.Forms.Button> controles sejam movidos para a primeira coluna.

8. Aumente a largura do <xref:System.Windows.Forms.FlowLayoutPanel> controle até que todos os <xref:System.Windows.Forms.Button> controles sejam movidos para a primeira linha. Você pode precisar redimensionar o formulário para acomodar a largura maior.

## <a name="changing-flow-direction"></a>Alterando a direção do fluxo
 A <xref:System.Windows.Forms.FlowLayoutPanel.FlowDirection%2A> propriedade permite que você altere a direção na qual os controles são organizados. Você pode organizar os controles filho da esquerda para a direita, da direita para a esquerda, de cima para baixo ou de baixo para cima.

### <a name="to-change-the-flow-direction-in-a-flowlayoutpanel"></a>Para alterar a direção do fluxo em um FlowLayoutPanel

1. Altere o valor da propriedade <xref:System.Windows.Forms.FlowLayoutPanel> do controle <xref:System.Windows.Forms.FlowLayoutPanel.FlowDirection%2A> para <xref:System.Windows.Forms.FlowDirection.TopDown>. Observe que os controles filho são reorganizados em uma ou mais colunas, dependendo da altura do controle.

2. Redimensione o <xref:System.Windows.Forms.FlowLayoutPanel> para que sua altura seja menor que a coluna de <xref:System.Windows.Forms.Button> controles. Observe que o <xref:System.Windows.Forms.FlowLayoutPanel> reorganiza os controles filho para fluir para a próxima coluna. Continue a diminuir a altura e observe que os controles filho fluem para colunas consecutivas. Altere o valor da propriedade <xref:System.Windows.Forms.FlowLayoutPanel> do controle <xref:System.Windows.Forms.FlowLayoutPanel.FlowDirection%2A> para <xref:System.Windows.Forms.FlowDirection.RightToLeft>. Observe que as posições dos controles filho são revertidas. Observe o layout quando você altera o valor da <xref:System.Windows.Forms.FlowLayoutPanel.FlowDirection%2A> propriedade para <xref:System.Windows.Forms.FlowDirection.BottomUp> .

## <a name="inserting-flow-breaks"></a>Inserindo quebras de fluxo
 O <xref:System.Windows.Forms.FlowLayoutPanel> controle fornece uma propriedade FlowBreak para seus controles filho. Definir o valor da propriedade FlowBreak como `true` faz com que o <xref:System.Windows.Forms.FlowLayoutPanel> controle pare de dispor os controles na direção do fluxo atual e seja quebrado para a próxima linha ou coluna.

### <a name="to-insert-flow-breaks"></a>Para inserir quebras de fluxo

1. Altere o valor da propriedade <xref:System.Windows.Forms.FlowLayoutPanel> do controle <xref:System.Windows.Forms.FlowLayoutPanel.FlowDirection%2A> para <xref:System.Windows.Forms.FlowDirection.TopDown>.

2. Selecione um dos <xref:System.Windows.Forms.Button> controles no meio da coluna mais à esquerda.

3. Defina o valor da <xref:System.Windows.Forms.Button> Propriedade FlowBreak do controle como `true` . Observe que a coluna está quebrada e os controles que seguem o <xref:System.Windows.Forms.Button> fluxo de controle selecionado para a próxima coluna. Defina o valor da <xref:System.Windows.Forms.Button> Propriedade FlowBreak do controle como `false` para retornar ao comportamento original.

## <a name="positioning-controls-using-docking-and-anchoring"></a>Posicionando controles usando encaixe e ancoragem
 Os comportamentos de encaixe e ancoragem dos controles filho diferem dos comportamentos em outras caixas de controles. O encaixe e a ancoragem são relativos ao maior controle na direção do fluxo.

### <a name="to-position-controls-using-docking-and-anchoring"></a>Para posicionar controles usando encaixe e ancoragem

1. Aumente o tamanho de <xref:System.Windows.Forms.FlowLayoutPanel> até que os <xref:System.Windows.Forms.Button> controles sejam todos organizados em uma coluna.

2. Selecione o <xref:System.Windows.Forms.Button> controle superior. Aumente sua largura para que seja aproximadamente duas vezes mais largas que os outros <xref:System.Windows.Forms.Button> controles.

3. Selecione o segundo <xref:System.Windows.Forms.Button> controle. Altere o valor de sua <xref:System.Windows.Forms.Control.Anchor%2A> propriedade para <xref:System.Windows.Forms.AnchorStyles.Right> . Observe que ele é movido de forma que sua borda direita seja alinhada com a <xref:System.Windows.Forms.Button> borda direita do primeiro controle.

4. Altere o valor de sua <xref:System.Windows.Forms.Control.Anchor%2A> propriedade para <xref:System.Windows.Forms.AnchorStyles.Right> e <xref:System.Windows.Forms.AnchorStyles.Left> . Observe que ele é dimensionado para a mesma largura que o primeiro <xref:System.Windows.Forms.Button> controle.

5. Selecione o terceiro <xref:System.Windows.Forms.Button> controle. Altere o valor de sua <xref:System.Windows.Forms.Control.Dock%2A> propriedade para <xref:System.Windows.Forms.DockStyle.Fill> . Observe que ele é dimensionado para a mesma largura que o primeiro <xref:System.Windows.Forms.Button> controle.

## <a name="arranging-controls-using-padding-and-margins"></a>Organizando controles usando preenchimento e margens
 Você também pode organizar controles em seu <xref:System.Windows.Forms.FlowLayoutPanel> controle alterando as <xref:System.Windows.Forms.Control.Padding%2A> <xref:System.Windows.Forms.Control.Margin%2A> Propriedades e.

 A <xref:System.Windows.Forms.Control.Padding%2A> propriedade permite que você controle o posicionamento de controles dentro de uma <xref:System.Windows.Forms.FlowLayoutPanel> célula de controle. Especifica o espaçamento entre os controles filho e a <xref:System.Windows.Forms.FlowLayoutPanel> borda do controle.

 A <xref:System.Windows.Forms.Control.Margin%2A> propriedade permite que você controle o espaçamento entre os controles.

### <a name="to-arrange-controls-using-the-padding-and-margin-properties"></a>Para organizar controles usando as propriedades de preenchimento e margem

1. Altere o valor da propriedade <xref:System.Windows.Forms.FlowLayoutPanel> do controle <xref:System.Windows.Forms.Control.Dock%2A> para <xref:System.Windows.Forms.DockStyle.Fill>. Se o formulário for grande o suficiente, os <xref:System.Windows.Forms.Button> controles serão movidos para a primeira coluna do <xref:System.Windows.Forms.FlowLayoutPanel> controle.

2. Altere o valor da <xref:System.Windows.Forms.FlowLayoutPanel> Propriedade do controle <xref:System.Windows.Forms.Control.Padding%2A> expandindo a <xref:System.Windows.Forms.Control.Padding%2A> entrada na janela **Propriedades** e definindo a <xref:System.Windows.Forms.Padding.All%2A> propriedade como **20**. Para obter mais informações, consulte [Passo a passo: definindo o layout de controles dos Windows Forms com preenchimento, margens e a propriedade AutoSize](windows-forms-controls-padding-autosize.md). Observe que os controles filho são movidos para o centro do <xref:System.Windows.Forms.FlowLayoutPanel> controle. O maior valor para a <xref:System.Windows.Forms.Control.Padding%2A> Propriedade empurra os controles filho para longe das <xref:System.Windows.Forms.FlowLayoutPanel> bordas do controle.

3. Selecione todos os <xref:System.Windows.Forms.Button> controles no <xref:System.Windows.Forms.FlowLayoutPanel> e defina o valor da <xref:System.Windows.Forms.Control.Margin%2A> propriedade como **20**. Observe que o espaçamento entre os <xref:System.Windows.Forms.Button> controles aumenta, para que eles sejam movidos ainda mais. Talvez seja necessário redimensionar o <xref:System.Windows.Forms.FlowLayoutPanel> controle para que ele seja maior para ver todos os controles filho.

## <a name="inserting-controls-by-double-clicking-them-in-the-toolbox"></a>Inserindo controles ao clicar duas vezes neles na caixa de ferramentas
 Você pode preencher seu <xref:System.Windows.Forms.FlowLayoutPanel> controle clicando duas vezes em controles na **caixa de ferramentas**.

### <a name="to-insert-controls-by-double-clicking-in-the-toolbox"></a>Para inserir controles clicando duas vezes na caixa de ferramentas

1. Clique duas vezes no <xref:System.Windows.Forms.Button> ícone de controle na **caixa de ferramentas**. Observe que um novo <xref:System.Windows.Forms.Button> controle aparece no <xref:System.Windows.Forms.FlowLayoutPanel> controle.

2. Clique duas vezes em vários outros controles na **Caixa de ferramentas**. Observe que os novos controles aparecem sucessivamente no <xref:System.Windows.Forms.FlowLayoutPanel> controle.

## <a name="inserting-a-control-by-drawing-its-outline"></a>Inserindo um controle ao desenhar seu contorno
 Você pode inserir um controle em um <xref:System.Windows.Forms.FlowLayoutPanel> controle e especificar seu tamanho desenhando seu contorno em uma célula.

### <a name="to-insert-a-control-by-drawing-its-outline"></a>Para inserir um controle desenhando seu contorno

1. Na **caixa de ferramentas**, clique no <xref:System.Windows.Forms.Button> ícone de controle. Não o arraste para o formulário.

2. Mova o ponteiro do mouse sobre o <xref:System.Windows.Forms.FlowLayoutPanel> controle. Observe que o ponteiro muda para um cruzado com o <xref:System.Windows.Forms.Button> ícone de controle anexado.

3. Clique e mantenha o botão do mouse pressionado.

4. Arraste o ponteiro do mouse para desenhar a estrutura de tópicos do <xref:System.Windows.Forms.Button> controle. Quando estiver satisfeito com o tamanho, solte o botão do mouse. Observe que o <xref:System.Windows.Forms.Button> controle é criado no próximo local aberto do <xref:System.Windows.Forms.FlowLayoutPanel> controle.

## <a name="inserting-controls-using-the-insertion-bar"></a>Inserindo controles usando a barra de inserção
 Você pode inserir controles em uma posição específica em um <xref:System.Windows.Forms.FlowLayoutPanel> controle. Quando você arrasta um controle para a <xref:System.Windows.Forms.FlowLayoutPanel> área do cliente do controle, uma barra de inserção é exibida para indicar onde o controle será inserido.

### <a name="to-insert-a-control-using-the-caret"></a>Para inserir um controle usando o sinal de interpolação

1. Arraste um <xref:System.Windows.Forms.Button> controle da **caixa de ferramentas** para o <xref:System.Windows.Forms.FlowLayoutPanel> controle e aponte para o espaço entre dois <xref:System.Windows.Forms.Button> controles. Observe que uma barra de inserção é desenhada, indicando onde o <xref:System.Windows.Forms.Button> será colocado quando for descartado no <xref:System.Windows.Forms.FlowLayoutPanel> controle. Antes de soltar o novo <xref:System.Windows.Forms.Button> controle no <xref:System.Windows.Forms.FlowLayoutPanel> controle, mova o ponteiro do mouse para observar como a barra de inserção se move.

2. Descarte o novo <xref:System.Windows.Forms.Button> controle no <xref:System.Windows.Forms.FlowLayoutPanel> controle. Observe que o novo <xref:System.Windows.Forms.Button> controle não está alinhado com os outros, porque sua <xref:System.Windows.Forms.Control.Margin%2A> propriedade tem um valor diferente.

## <a name="reassigning-existing-controls-to-a-different-parent"></a>Reatribuição de controles existentes a um pai diferente
 Você pode atribuir controles que existem no formulário a um novo <xref:System.Windows.Forms.FlowLayoutPanel> controle.

### <a name="to-reparent-existing-controls"></a>Para reassociar os controles existentes

1. Arraste três <xref:System.Windows.Forms.Button> controles da **caixa de ferramentas** para o formulário. Posicione-os próximo entre si, mas deixe-os desalinhados.

2. Na **caixa de ferramentas**, clique no <xref:System.Windows.Forms.FlowLayoutPanel> ícone de controle. Não o arraste para o formulário.

3. Mova o ponteiro do mouse para perto dos três <xref:System.Windows.Forms.Button> controles. Observe que o ponteiro muda para um cruzado com o <xref:System.Windows.Forms.FlowLayoutPanel> ícone de controle anexado.

4. Clique e mantenha o botão do mouse pressionado.

5. Arraste o ponteiro do mouse para desenhar a estrutura de tópicos do <xref:System.Windows.Forms.FlowLayoutPanel> controle. Desenhe a estrutura de tópicos em torno dos três <xref:System.Windows.Forms.Button> controles.

6. Solte o botão do mouse. Observe que os três <xref:System.Windows.Forms.Button> controles são inseridos no <xref:System.Windows.Forms.FlowLayoutPanel> controle.

## <a name="next-steps"></a>Próximas etapas
 Você pode obter um layout complexo usando uma combinação de controles e painéis de layout. Sugestões para exploração adicional incluem:

- Redimensione um dos <xref:System.Windows.Forms.Button> controles para um tamanho maior e observe o efeito no layout.

- Os painéis de layout podem conter outros painéis de layout. Experimente soltar um <xref:System.Windows.Forms.TableLayoutPanel> controle no controle existente.

- Encaixar o <xref:System.Windows.Forms.FlowLayoutPanel> controle no formulário pai. Redimensione o formulário e observe o efeito no layout.

- Defina a <xref:System.Windows.Forms.Control.Visible%2A> propriedade de um dos controles como `false` e observe como os <xref:System.Windows.Forms.FlowLayoutPanel> refluxos em resposta.

## <a name="see-also"></a>Consulte também

- <xref:System.Windows.Forms.FlowLayoutPanel>
- <xref:System.Windows.Forms.TableLayoutPanel>
- [Explicação passo a passo: organizando controles no Windows Forms utilizando um TableLayoutPanel](walkthrough-arranging-controls-on-windows-forms-using-a-tablelayoutpanel.md)
- [Instruções passo a passo: organizando controles no Windows Forms usando guias de alinhamento](walkthrough-arranging-controls-on-windows-forms-using-snaplines.md)
- [Visão geral da propriedade AutoSize](autosize-property-overview.md)
- [Como encaixar controles no Windows Forms](how-to-dock-controls-on-windows-forms.md)
- [Como ancorar controles no Windows Forms](how-to-anchor-controls-on-windows-forms.md)
- [Passo a passo: definindo o layout de controles dos Windows Forms com preenchimento, margens e a propriedade AutoSize](windows-forms-controls-padding-autosize.md)
