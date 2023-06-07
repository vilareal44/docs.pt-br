---
title: Opções de dimensionamento no controle DataGridView
description: Saiba mais sobre as opções de dimensionamento no controle Windows Forms DataGridView. Linhas, colunas e cabeçalhos DataGridView alteram o tamanho como resultado de ocorrências diferentes.
ms.date: 03/30/2017
helpviewer_keywords:
- DataGridView control [Windows Forms], row sizing
- data grids [Windows Forms], column sizing
- DataGridView control [Windows Forms], column sizing
- DataGridView control [Windows Forms], sizing options
- data grids [Windows Forms], row sizing
- data grids [Windows Forms], sizing options
ms.assetid: a5620a9c-0d06-41e3-8934-c25ddb16c9e6
ms.openlocfilehash: 8ddf72c412db74df35bc3f035ff05d1e7e9738d1
ms.sourcegitcommit: e02d17b2cf9c1258dadda4810a5e6072a0089aee
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/01/2020
ms.locfileid: "85613716"
---
# <a name="sizing-options-in-the-windows-forms-datagridview-control"></a>Dimensionando opções no controle DataGridView dos Windows Forms
<xref:System.Windows.Forms.DataGridView>linhas, colunas e cabeçalhos podem alterar o tamanho como resultado de muitas ocorrências diferentes. A tabela a seguir mostra essas ocorrências.  
  
|Ocorrência|Descrição|  
|----------------|-----------------|  
|Redimensionamento do usuário|Os usuários podem fazer ajustes de tamanho arrastando ou clicando duas vezes nos divisores de linha, coluna ou cabeçalho.|  
|Redimensionamento do controle|No modo de preenchimento de coluna, as larguras de coluna são alteradas quando a largura do controle é alterada. Por exemplo, quando o controle é encaixado em seu formulário pai e o usuário redimensiona o formulário.|  
|Alteração do valor da célula|Nos modos de dimensionamento automático baseado em conteúdo, os tamanhos são alterados para ajustar os novos valores de exibição.|  
|Chamada de método|O redimensionamento programático baseado em conteúdo permite que você faça ajustes de tamanho oportunos com base nos valores de célula no momento da chamada de método.|  
|Configuração de propriedade|Você também pode definir valores específicos de largura e altura.|  
  
 Por padrão, o redimensionamento do usuário está habilitado, o dimensionamento automático está desabilitado e os valores de células são mais largos do que o recorte de suas colunas.  
  
 A tabela a seguir mostra cenários que você pode usar para ajustar o comportamento padrão ou para usar opções específicas de dimensionamento para atingir efeitos específicos.  
  
|Cenário|Implementação|  
|--------------|--------------------|  
|Use o modo de preenchimento de coluna para exibir dados de tamanhos semelhantes em um número relativamente pequeno de colunas que ocupem toda a largura do controle sem exibir a barra de rolagem horizontal.|Defina a propriedade <xref:System.Windows.Forms.DataGridView.AutoSizeColumnsMode%2A> como <xref:System.Windows.Forms.DataGridViewAutoSizeColumnsMode.Fill>.|  
|Use o modo de preenchimento de coluna com valores de exibição de tamanhos variados.|Defina a propriedade <xref:System.Windows.Forms.DataGridView.AutoSizeColumnsMode%2A> como <xref:System.Windows.Forms.DataGridViewAutoSizeColumnsMode.Fill>. Inicialize larguras de coluna relativas definindo as propriedades de coluna <xref:System.Windows.Forms.DataGridViewColumn.FillWeight%2A> ou chamando o método de controle <xref:System.Windows.Forms.DataGridView.AutoResizeColumns%2A> depois de preencher o controle com os dados.|  
|Use o modo de preenchimento de coluna com valores de importância variada.|Defina a propriedade <xref:System.Windows.Forms.DataGridView.AutoSizeColumnsMode%2A> como <xref:System.Windows.Forms.DataGridViewAutoSizeColumnsMode.Fill>. Defina <xref:System.Windows.Forms.DataGridViewColumn.MinimumWidth%2A> valores grandes para colunas que sempre devem exibir alguns de seus dados ou usar uma opção de dimensionamento diferente do modo de preenchimento para colunas específicas.|  
|Use o modo de preenchimento de coluna para evitar a exibição da tela de fundo do controle.|Defina a <xref:System.Windows.Forms.DataGridViewColumn.AutoSizeMode%2A> propriedade da última coluna como <xref:System.Windows.Forms.DataGridViewAutoSizeColumnMode.Fill> e use outras opções de dimensionamento para as outras colunas. Se as outras colunas usarem muito espaço disponível, defina a <xref:System.Windows.Forms.DataGridViewColumn.MinimumWidth%2A> propriedade da última coluna.|  
|Exiba uma coluna de largura fixa, como um ícone ou uma coluna de ID.|Defina <xref:System.Windows.Forms.DataGridViewColumn.AutoSizeMode%2A> como <xref:System.Windows.Forms.DataGridViewAutoSizeColumnMode.None> e <xref:System.Windows.Forms.DataGridViewColumn.Resizable%2A> para <xref:System.Windows.Forms.DataGridViewTriState.False> para a coluna. Inicialize sua largura definindo a <xref:System.Windows.Forms.DataGridViewColumn.Width%2A> propriedade ou chamando o método de controle <xref:System.Windows.Forms.DataGridView.AutoResizeColumn%2A> depois de preencher o controle com os dados.|  
|Ajuste os tamanhos automaticamente sempre que o conteúdo da célula for alterado para evitar recorte e otimizar o uso de espaço.|Defina uma propriedade de dimensionamento automático para um valor que represente um modo de dimensionamento baseado em conteúdo. Para evitar uma penalidade de desempenho ao trabalhar com grandes quantidades de dados, use um modo de dimensionamento que calcule apenas as linhas exibidas.|  
|Ajuste os tamanhos para ajustar os valores nas linhas exibidas para evitar penalidades de desempenho ao trabalhar com várias linhas.|Use os valores de enumeração de modo de dimensionamento apropriados com o redimensionamento automático ou programático. Para ajustar os tamanhos para ajustar os valores nas linhas exibidas recentemente durante a rolagem, chame um método de redimensionamento em um <xref:System.Windows.Forms.DataGridView.Scroll> manipulador de eventos. Para personalizar o usuário clique duas vezes em redimensionamento para que apenas os valores nas linhas exibidas determinem os novos tamanhos, chame um método de redimensionamento em um <xref:System.Windows.Forms.DataGridView.RowDividerDoubleClick> <xref:System.Windows.Forms.DataGridView.ColumnDividerDoubleClick> manipulador de eventos ou.|  
|Ajuste os tamanhos para ajustar o conteúdo da célula apenas em momentos específicos para evitar penalidades de desempenho ou permitir o redimensionamento do usuário.|Chame um método de redimensionamento baseado em conteúdo em um manipulador de eventos. Por exemplo, use o <xref:System.Windows.Forms.DataGridView.DataBindingComplete> evento para inicializar tamanhos após a associação e manipule o <xref:System.Windows.Forms.DataGridView.CellValidated> <xref:System.Windows.Forms.DataGridView.CellValueChanged> evento ou para ajustar os tamanhos para compensar as edições do usuário ou as alterações em uma fonte de dados associada.|  
|Ajuste as alturas de linhas para conteúdo de célula multilinha.|Verifique se as larguras de coluna são apropriadas para a exibição de parágrafos de texto e use o dimensionamento de linhas baseado em conteúdo automático ou programático para ajustar as alturas. Verifique também se as células com conteúdo multilinha são exibidas usando um <xref:System.Windows.Forms.DataGridViewCellStyle.WrapMode%2A> valor de estilo de célula de <xref:System.Windows.Forms.DataGridViewTriState.True> .<br /><br /> Normalmente, você usará o modo de dimensionamento automático de coluna para manter as larguras de coluna ou defini-las como larguras específicas antes que as alturas das linhas sejam ajustadas.|  
  
## <a name="resizing-with-the-mouse"></a>Redimensionando com o mouse  
 Por padrão, os usuários podem redimensionar linhas, colunas e cabeçalhos que não usam um modo de dimensionamento automático com base nos valores de célula. Para impedir que os usuários redimensionem com outros modos, como o modo de preenchimento de coluna, defina uma ou mais das seguintes <xref:System.Windows.Forms.DataGridView> Propriedades:  
  
- <xref:System.Windows.Forms.DataGridView.AllowUserToResizeColumns%2A>  
  
- <xref:System.Windows.Forms.DataGridView.AllowUserToResizeRows%2A>  
  
- <xref:System.Windows.Forms.DataGridView.ColumnHeadersHeightSizeMode%2A>  
  
- <xref:System.Windows.Forms.DataGridView.RowHeadersWidthSizeMode%2A>  
  
 Você também pode impedir que os usuários redimensionem linhas ou colunas individuais definindo suas <xref:System.Windows.Forms.DataGridViewBand.Resizable%2A> Propriedades. Por padrão, o <xref:System.Windows.Forms.DataGridViewBand.Resizable%2A> valor da propriedade é baseado no <xref:System.Windows.Forms.DataGridView.AllowUserToResizeColumns%2A> valor da propriedade para colunas e no <xref:System.Windows.Forms.DataGridView.AllowUserToResizeRows%2A> valor da propriedade para linhas. Se você definir explicitamente <xref:System.Windows.Forms.DataGridViewBand.Resizable%2A> como <xref:System.Windows.Forms.DataGridViewTriState.True> ou <xref:System.Windows.Forms.DataGridViewTriState.False> , no entanto, o valor especificado substituirá o valor de controle por essa linha ou coluna. Defina <xref:System.Windows.Forms.DataGridViewBand.Resizable%2A> como <xref:System.Windows.Forms.DataGridViewTriState.NotSet> para restaurar a herança.  
  
 Como o <xref:System.Windows.Forms.DataGridViewTriState.NotSet> restaura a herança de valor, a <xref:System.Windows.Forms.DataGridViewBand.Resizable%2A> Propriedade nunca retornará um <xref:System.Windows.Forms.DataGridViewTriState.NotSet> valor, a menos que a linha ou coluna não tenha sido adicionada a um <xref:System.Windows.Forms.DataGridView> controle. Se você precisar determinar se o <xref:System.Windows.Forms.DataGridViewBand.Resizable%2A> valor da propriedade de uma linha ou coluna é herdado, examine sua <xref:System.Windows.Forms.DataGridViewElement.State%2A> propriedade. Se o <xref:System.Windows.Forms.DataGridViewElement.State%2A> valor incluir o <xref:System.Windows.Forms.DataGridViewElementStates.ResizableSet> sinalizador, o <xref:System.Windows.Forms.DataGridViewBand.Resizable%2A> valor da propriedade não será herdado.  
  
## <a name="automatic-sizing"></a>Dimensionamento automático  
 Há dois tipos de dimensionamento automático no <xref:System.Windows.Forms.DataGridView> controle: modo de preenchimento de coluna e dimensionamento automático baseado em conteúdo.  
  
 O modo de preenchimento de coluna faz com que as colunas visíveis no controle preencham a largura da área de exibição do controle. Para obter mais informações sobre esse modo, consulte [Modo de preenchimento da coluna no controle DataGridView dos Windows Forms](column-fill-mode-in-the-windows-forms-datagridview-control.md).  
  
 Você também pode configurar as linhas, as colunas e os cabeçalhos para ajustar automaticamente seus tamanhos a fim de ajustar o conteúdo de suas células. Nesse caso, o ajuste de tamanho ocorre sempre que o conteúdo da célula é alterado.  
  
> [!NOTE]
> Se você mantiver valores de célula em um cache de dados personalizado usando o modo virtual, o dimensionamento automático ocorrerá quando o usuário editar um valor de célula, mas não ocorrer quando você alterar um valor em cache fora de um <xref:System.Windows.Forms.DataGridView.CellValuePushed> manipulador de eventos. Nesse caso, chame o <xref:System.Windows.Forms.DataGridView.UpdateCellValue%2A> método para forçar o controle a atualizar a exibição de célula e aplicar os modos de dimensionamento automático atuais.  
  
 Se o dimensionamento automático baseado em conteúdo estiver habilitado apenas para uma dimensão — ou seja, para linhas, mas não para colunas, ou para colunas, mas não para linhas, e <xref:System.Windows.Forms.DataGridViewCellStyle.WrapMode%2A> também estiver habilitado, o ajuste de tamanho também ocorrerá sempre que a outra dimensão for alterada. Por exemplo, se as linhas, mas não as colunas estiverem configuradas para o dimensionamento automático e <xref:System.Windows.Forms.DataGridViewCellStyle.WrapMode%2A> estiverem habilitadas, os usuários poderão arrastar separadores de colunas para alterar a largura de uma coluna e as alturas das linhas serão ajustadas automaticamente para que o conteúdo da célula ainda seja totalmente exibido.  
  
 Se você configurar linhas e colunas para o dimensionamento automático baseado em conteúdo e <xref:System.Windows.Forms.DataGridViewCellStyle.WrapMode%2A> estiver habilitado, o <xref:System.Windows.Forms.DataGridView> controle ajustará os tamanhos sempre que o conteúdo da célula for alterado e usará uma proporção ideal de altura para largura de célula ao calcular novos tamanhos.  
  
 Para configurar o modo de dimensionamento para cabeçalhos e linhas e colunas que não substituem o valor de controle, defina uma ou mais das seguintes <xref:System.Windows.Forms.DataGridView> Propriedades:  
  
- <xref:System.Windows.Forms.DataGridView.ColumnHeadersHeightSizeMode%2A>  
  
- <xref:System.Windows.Forms.DataGridView.RowHeadersWidthSizeMode%2A>  
  
- <xref:System.Windows.Forms.DataGridView.AutoSizeColumnsMode%2A>  
  
- <xref:System.Windows.Forms.DataGridView.AutoSizeRowsMode%2A>  
  
 Para substituir o modo de dimensionamento de coluna do controle para uma coluna individual, defina sua <xref:System.Windows.Forms.DataGridViewColumn.AutoSizeMode%2A> propriedade com um valor diferente de <xref:System.Windows.Forms.DataGridViewAutoSizeColumnMode.NotSet> . Na verdade, o modo de dimensionamento de uma coluna é determinado por sua <xref:System.Windows.Forms.DataGridViewColumn.InheritedAutoSizeMode%2A> propriedade. O valor dessa propriedade é baseado no valor da propriedade da coluna, <xref:System.Windows.Forms.DataGridViewColumn.AutoSizeMode%2A> a menos que esse valor seja <xref:System.Windows.Forms.DataGridViewAutoSizeColumnMode.NotSet> ; nesse caso, o <xref:System.Windows.Forms.DataGridView.AutoSizeColumnsMode%2A> valor do controle é herdado.  
  
 Use o redimensionamento automático baseado em conteúdo com cuidado ao trabalhar com grandes quantidades de dados. Para evitar penalidades de desempenho, use os modos de dimensionamento automático que calculam tamanhos base apenas nas linhas exibidas em vez de analisar cada linha no controle. Para obter o desempenho máximo, use o redimensionamento programático para que você possa redimensionar em momentos específicos, como imediatamente depois que novos dados são carregados.  
  
 Os modos de dimensionamento automático baseados em conteúdo não afetam linhas, colunas ou cabeçalhos que você ocultou definindo a propriedade de linha ou coluna <xref:System.Windows.Forms.DataGridViewBand.Visible%2A> ou o controle <xref:System.Windows.Forms.DataGridView.RowHeadersVisible%2A> ou <xref:System.Windows.Forms.DataGridView.ColumnHeadersVisible%2A> as propriedades para `false` . Por exemplo, se uma coluna for ocultada depois de ser dimensionada automaticamente para ajustar um valor de célula grande, a coluna oculta não mudará seu tamanho se a linha que contém o valor da célula grande for excluída. O dimensionamento automático não ocorre quando a visibilidade é alterada, portanto, alterar a propriedade de coluna <xref:System.Windows.Forms.DataGridViewColumn.Visible%2A> de volta para `true` não irá forçá-la a recalcular seu tamanho com base em seu conteúdo atual.  
  
 O redimensionamento programático baseado em conteúdo afeta colunas, linhas e cabeçalhos independentemente da visibilidade.  
  
## <a name="programmatic-resizing"></a>Redimensionamento programático  
 Quando o dimensionamento automático está desabilitado, você pode definir programaticamente a largura ou a altura exata de linhas, colunas ou cabeçalhos usando as propriedades a seguir:  
  
- <xref:System.Windows.Forms.DataGridView.RowHeadersWidth%2A?displayProperty=nameWithType>  
  
- <xref:System.Windows.Forms.DataGridView.ColumnHeadersHeight%2A?displayProperty=nameWithType>  
  
- <xref:System.Windows.Forms.DataGridViewRow.Height%2A?displayProperty=nameWithType>  
  
- <xref:System.Windows.Forms.DataGridViewColumn.Width%2A?displayProperty=nameWithType>  
  
 Você também pode redimensionar de forma programática as linhas, as colunas e os cabeçalhos para que se ajustem ao conteúdo usando os seguintes métodos:  
  
- <xref:System.Windows.Forms.DataGridView.AutoResizeColumn%2A>  
  
- <xref:System.Windows.Forms.DataGridView.AutoResizeColumns%2A>  
  
- <xref:System.Windows.Forms.DataGridView.AutoResizeColumnHeadersHeight%2A>  
  
- <xref:System.Windows.Forms.DataGridView.AutoResizeRow%2A>  
  
- <xref:System.Windows.Forms.DataGridView.AutoResizeRows%2A>  
  
- <xref:System.Windows.Forms.DataGridView.AutoResizeRowHeadersWidth%2A>  
  
 Esses métodos redimensionarão as linhas, as colunas ou os cabeçalhos uma única vez em vez de configurá-los para redimensionamento contínuo. Os novos tamanhos são calculados automaticamente para exibir todo o conteúdo da célula sem recorte. Quando você redimensiona programaticamente colunas com <xref:System.Windows.Forms.DataGridViewColumn.InheritedAutoSizeMode%2A> valores de propriedade de <xref:System.Windows.Forms.DataGridViewAutoSizeColumnMode.Fill> , no entanto, as larguras baseadas em conteúdo calculadas são usadas para ajustar proporcionalmente os valores de propriedade da coluna <xref:System.Windows.Forms.DataGridViewColumn.FillWeight%2A> , e a largura da coluna realmente é calculada de acordo com essas novas proporções para que todas as colunas preencham a área de exibição disponível do controle.  
  
 O redimensionando programático é útil para evitar penalidades de desempenho com o redimensionamento contínuo. Ele também é útil para fornecer os tamanhos inicias de cabeçalhos, colunas e linhas redimensionáveis pelo usuário e para o modo de preenchimento de coluna.  
  
 Normalmente, você chamará os métodos de redimensionamento programáticos em momentos específicos. Por exemplo, você pode redimensionar de forma programática todas as colunas imediatamente após o carregamento de dados ou redimensionar de forma programática uma linha específica depois que um valor de célula específico foi modificado.  
  
## <a name="customizing-content-based-sizing-behavior"></a>Personalizando o comportamento de dimensionamento baseado em conteúdo  
 Você pode personalizar comportamentos de dimensionamento ao trabalhar com <xref:System.Windows.Forms.DataGridView> tipos de célula, linha e coluna derivadas, substituindo os <xref:System.Windows.Forms.DataGridViewCell.GetPreferredSize%2A?displayProperty=nameWithType> <xref:System.Windows.Forms.DataGridViewRow.GetPreferredHeight%2A?displayProperty=nameWithType> métodos, ou, <xref:System.Windows.Forms.DataGridViewColumn.GetPreferredWidth%2A?displayProperty=nameWithType> ou chamando sobrecargas de método de redimensionamento protegido em um <xref:System.Windows.Forms.DataGridView> controle derivado. As sobrecargas de método de redimensionamento protegidas são projetadas para funcionar em pares para atingir uma razão ideal de altura para largura da célula, evitando células excessivamente altas ou largas. Por exemplo, se você chamar a `AutoResizeRows(DataGridViewAutoSizeRowsMode,Boolean)` sobrecarga do <xref:System.Windows.Forms.DataGridView.AutoResizeRows%2A> método e passar um valor de `false` para o <xref:System.Boolean> parâmetro, a sobrecarga calculará as alturas e larguras ideais para as células na linha, mas ajustará apenas as alturas de linha. Em seguida, você deve chamar o <xref:System.Windows.Forms.DataGridView.AutoResizeColumns%2A> método para ajustar as larguras da coluna para o ideal calculado.  
  
## <a name="content-based-sizing-options"></a>Opções de dimensionamento baseado em conteúdo  
 As enumerações usadas pelos métodos e propriedades de dimensionamento têm valores semelhantes para dimensionamento baseado em conteúdo. Com esses valores, você pode limitar quais células são usadas para calcular o tamanho preferencial. Para todas as enumerações de dimensionamento, valores com nomes que se referem a células exibidas limitam seus cálculos para células em linhas exibidas. Excluir linhas é útil para evitar uma penalidade de desempenho ao trabalhar com uma grande quantidade de linhas. Você também pode restringir os cálculos para valores de célula nas células de cabeçalho ou que não sejam de cabeçalho.  
  
## <a name="see-also"></a>Consulte também

- <xref:System.Windows.Forms.DataGridView>
- <xref:System.Windows.Forms.DataGridView.AllowUserToResizeColumns%2A?displayProperty=nameWithType>
- <xref:System.Windows.Forms.DataGridView.AllowUserToResizeRows%2A?displayProperty=nameWithType>
- <xref:System.Windows.Forms.DataGridView.ColumnHeadersHeightSizeMode%2A?displayProperty=nameWithType>
- <xref:System.Windows.Forms.DataGridView.RowHeadersWidthSizeMode%2A?displayProperty=nameWithType>
- <xref:System.Windows.Forms.DataGridViewBand.Resizable%2A?displayProperty=nameWithType>
- <xref:System.Windows.Forms.DataGridView.AutoSizeColumnsMode%2A?displayProperty=nameWithType>
- <xref:System.Windows.Forms.DataGridView.AutoSizeRowsMode%2A?displayProperty=nameWithType>
- <xref:System.Windows.Forms.DataGridViewColumn.AutoSizeMode%2A?displayProperty=nameWithType>
- <xref:System.Windows.Forms.DataGridViewColumn.InheritedAutoSizeMode%2A?displayProperty=nameWithType>
- <xref:System.Windows.Forms.DataGridView.RowHeadersWidth%2A?displayProperty=nameWithType>
- <xref:System.Windows.Forms.DataGridView.ColumnHeadersHeight%2A?displayProperty=nameWithType>
- <xref:System.Windows.Forms.DataGridViewRow.Height%2A?displayProperty=nameWithType>
- <xref:System.Windows.Forms.DataGridViewColumn.Width%2A?displayProperty=nameWithType>
- <xref:System.Windows.Forms.DataGridView.AutoResizeColumn%2A?displayProperty=nameWithType>
- <xref:System.Windows.Forms.DataGridView.AutoResizeColumns%2A?displayProperty=nameWithType>
- <xref:System.Windows.Forms.DataGridView.AutoResizeColumnHeadersHeight%2A?displayProperty=nameWithType>
- <xref:System.Windows.Forms.DataGridView.AutoResizeRow%2A?displayProperty=nameWithType>
- <xref:System.Windows.Forms.DataGridView.AutoResizeRows%2A?displayProperty=nameWithType>
- <xref:System.Windows.Forms.DataGridView.AutoResizeRowHeadersWidth%2A?displayProperty=nameWithType>
- <xref:System.Windows.Forms.DataGridViewAutoSizeRowMode>
- <xref:System.Windows.Forms.DataGridViewAutoSizeRowsMode>
- <xref:System.Windows.Forms.DataGridViewAutoSizeColumnMode>
- <xref:System.Windows.Forms.DataGridViewAutoSizeColumnsMode>
- <xref:System.Windows.Forms.DataGridViewColumnHeadersHeightSizeMode>
- <xref:System.Windows.Forms.DataGridViewRowHeadersWidthSizeMode>
- [Redimensionando colunas e linhas no controle DataGridView dos Windows Forms](resizing-columns-and-rows-in-the-windows-forms-datagridview-control.md)
- [Modo de preenchimento da coluna no controle DataGridView dos Windows Forms](column-fill-mode-in-the-windows-forms-datagridview-control.md)
- [Como definir os modos de dimensionamento do controle DataGridView dos Windows Forms](how-to-set-the-sizing-modes-of-the-windows-forms-datagridview-control.md)
