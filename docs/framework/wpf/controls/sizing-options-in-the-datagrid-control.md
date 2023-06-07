---
title: Opções de dimensionamento no controle DataGrid
description: Saiba como definir linhas e colunas individuais no controle DataGrid de Windows Presentation Foundation para dimensionar o conteúdo ou para valores específicos.
ms.date: 03/30/2017
helpviewer_keywords:
- DataGrid control [WPF], sizing
- size [WPF], DataGrid
- automatically size DataGrid [WPF]
ms.assetid: 96a0e47e-b010-4302-98ef-2daac446d8db
ms.openlocfilehash: 0f10e385cbf18a26989614363ca6cd9f92bc83ec
ms.sourcegitcommit: 87cfeb69226fef01acb17c56c86f978f4f4a13db
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/24/2020
ms.locfileid: "87164737"
---
# <a name="sizing-options-in-the-datagrid-control"></a>Opções de dimensionamento no controle DataGrid
Várias opções estão disponíveis para controlar como os <xref:System.Windows.Controls.DataGrid> tamanhos em si. O <xref:System.Windows.Controls.DataGrid> , e as linhas e colunas individuais no <xref:System.Windows.Controls.DataGrid> , podem ser definidos para dimensionar automaticamente para seu conteúdo ou podem ser definidos para valores específicos. Por padrão, o <xref:System.Windows.Controls.DataGrid> aumentará e diminuirá para se ajustar ao tamanho do seu conteúdo.  
  
## <a name="sizing-the-datagrid"></a>Dimensionamento de DataGrid  
  
### <a name="cautions-when-using-automatic-sizing"></a>Cuidados ao usar o dimensionamento automático  
 Por padrão, as <xref:System.Windows.FrameworkElement.Height%2A> <xref:System.Windows.FrameworkElement.Width%2A> Propriedades e de <xref:System.Windows.Controls.DataGrid> são definidas como <xref:System.Double.NaN?displayProperty=nameWithType> (" `Auto` " em XAML) e o <xref:System.Windows.Controls.DataGrid> será ajustado para o tamanho do seu conteúdo.  
  
 Quando colocado dentro de um contêiner que não restringe o tamanho de seus filhos, como um <xref:System.Windows.Controls.Canvas> ou <xref:System.Windows.Controls.StackPanel> , o <xref:System.Windows.Controls.DataGrid> será ampliado além dos limites visíveis do contêiner e as barras de rolagem não serão mostradas. Esta condição tem implicações de desempenho e usabilidade.  
  
 Quando associado a um conjunto de dados, se o <xref:System.Windows.FrameworkElement.Height%2A> do <xref:System.Windows.Controls.DataGrid> não for restrito, ele continuará a adicionar uma linha para cada item de dados no conjunto de dados associado. Isso pode fazer com que o <xref:System.Windows.Controls.DataGrid> cresça fora dos limites visíveis do seu aplicativo à medida que as linhas são adicionadas. O <xref:System.Windows.Controls.DataGrid> não mostrará barras de rolagem nesse caso, pois seu <xref:System.Windows.FrameworkElement.Height%2A> continuará crescendo para acomodar as novas linhas.  
  
 Um objeto é criado para cada linha no <xref:System.Windows.Controls.DataGrid> . Se você estiver trabalhando com um grande conjunto de dados e permitir que o <xref:System.Windows.Controls.DataGrid> seja automaticamente dimensionado, a criação de um grande número de objetos poderá afetar o desempenho do seu aplicativo.  
  
 Para evitar esses problemas ao trabalhar com grandes conjuntos de dados, é recomendável definir especificamente o <xref:System.Windows.FrameworkElement.Height%2A> do <xref:System.Windows.Controls.DataGrid> ou colocá-lo em um contêiner que restringirá seu <xref:System.Windows.FrameworkElement.Height%2A> , como um <xref:System.Windows.Controls.Grid> . Quando o <xref:System.Windows.FrameworkElement.Height%2A> for restrito, o <xref:System.Windows.Controls.DataGrid> só criará as linhas que se ajustarão em seu especificado <xref:System.Windows.FrameworkElement.Height%2A> e reciclará essas linhas conforme necessário para exibir novos dados.  
  
### <a name="setting-the-datagrid-size"></a>Configurando o tamanho de DataGrid  
 O <xref:System.Windows.Controls.DataGrid> pode ser definido para o tamanho automático dentro dos limites especificados ou <xref:System.Windows.Controls.DataGrid> pode ser definido como um tamanho específico. A tabela a seguir mostra as propriedades que podem ser definidas para controlar o <xref:System.Windows.Controls.DataGrid> tamanho.  
  
|Propriedade|DESCRIÇÃO|  
|--------------|-----------------|  
|<xref:System.Windows.FrameworkElement.Height%2A>|Define uma altura específica para o <xref:System.Windows.Controls.DataGrid> .|  
|<xref:System.Windows.FrameworkElement.MaxHeight%2A>|Define o limite superior para a altura do <xref:System.Windows.Controls.DataGrid> . O <xref:System.Windows.Controls.DataGrid> será aumentado verticalmente até atingir essa altura.|  
|<xref:System.Windows.FrameworkElement.MinHeight%2A>|Define o limite inferior para a altura do <xref:System.Windows.Controls.DataGrid> . O <xref:System.Windows.Controls.DataGrid> será reduzido verticalmente até atingir essa altura.|  
|<xref:System.Windows.FrameworkElement.Width%2A>|Define uma largura específica para o <xref:System.Windows.Controls.DataGrid> .|  
|<xref:System.Windows.FrameworkElement.MaxWidth%2A>|Define o limite superior para a largura do <xref:System.Windows.Controls.DataGrid> . O <xref:System.Windows.Controls.DataGrid> aumentará horizontalmente até atingir essa largura.|  
|<xref:System.Windows.FrameworkElement.MinWidth%2A>|Define o limite inferior para a largura do <xref:System.Windows.Controls.DataGrid> . O <xref:System.Windows.Controls.DataGrid> será reduzido horizontalmente até atingir essa largura.|  
  
## <a name="sizing-rows-and-row-headers"></a>Dimensionando linhas e cabeçalhos de linha  
  
### <a name="datagrid-rows"></a>Linhas DataGrid  
 Por padrão, a <xref:System.Windows.Controls.DataGrid> propriedade de uma linha <xref:System.Windows.FrameworkElement.Height%2A> é definida como <xref:System.Double.NaN?displayProperty=nameWithType> (" `Auto` " em XAML) e a altura da linha se expandirá para o tamanho do seu conteúdo. A altura de todas as linhas no <xref:System.Windows.Controls.DataGrid> pode ser especificada definindo a <xref:System.Windows.Controls.DataGrid.RowHeight%2A?displayProperty=nameWithType> propriedade. Os usuários podem alterar a altura da linha arrastando os divisores de cabeçalho de linha.  
  
### <a name="datagrid-row-headers"></a>Cabeçalhos de linha DataGrid  
 Para exibir cabeçalhos de linha, a <xref:System.Windows.Controls.DataGrid.HeadersVisibility%2A> propriedade deve ser definida como <xref:System.Windows.Controls.DataGridHeadersVisibility.Row?displayProperty=nameWithType> ou <xref:System.Windows.Controls.DataGridHeadersVisibility.All?displayProperty=nameWithType> . Por padrão, os cabeçalhos de linha são exibidos e são dimensionados automaticamente para ajustar seu conteúdo. Os cabeçalhos de linha podem receber uma largura específica, definindo a <xref:System.Windows.Controls.DataGrid.RowHeaderWidth%2A?displayProperty=nameWithType> propriedade.  
  
## <a name="sizing-columns-and-column-headers"></a>Dimensionamento de colunas e cabeçalhos de coluna  
  
### <a name="datagrid-columns"></a>Colunas DataGrid  
 O <xref:System.Windows.Controls.DataGrid> usa valores de <xref:System.Windows.Controls.DataGridLength> e a <xref:System.Windows.Controls.DataGridLengthUnitType> estrutura para especificar modos de dimensionamento absolutos ou automáticos.  
  
 A tabela a seguir mostra os valores fornecidos pela <xref:System.Windows.Controls.DataGridLengthUnitType> estrutura.  
  
|Nome|Descrição|  
|----------|-----------------|  
|<xref:System.Windows.Controls.DataGridLengthUnitType.Auto>|O modo de dimensionamento automático padrão dimensiona <xref:System.Windows.Controls.DataGrid> colunas com base no conteúdo de células e cabeçalhos de coluna.|  
|<xref:System.Windows.Controls.DataGridLengthUnitType.SizeToCells>|O modo de dimensionamento automático baseado em célula dimensiona <xref:System.Windows.Controls.DataGrid> colunas com base no conteúdo das células na coluna, não incluindo cabeçalhos de coluna.|  
|<xref:System.Windows.Controls.DataGridLengthUnitType.SizeToHeader>|O modo de dimensionamento automático baseado em cabeçalho dimensiona <xref:System.Windows.Controls.DataGrid> colunas com base no conteúdo de cabeçalhos de coluna apenas.|  
|<xref:System.Windows.Controls.DataGridLengthUnitType.Pixel>|O modo de dimensionamento baseado em pixels dimensiona <xref:System.Windows.Controls.DataGrid> colunas com base no valor numérico fornecido.|  
|<xref:System.Windows.Controls.DataGridLengthUnitType.Star>|O modo de dimensionamento em estrela é usado para distribuir o espaço disponível em proporções ponderadas.<br /><br /> No XAML, os valores de estrela são expressos como n*, em que n representa um valor numérico. 1\* equivale a \*. Por exemplo, se duas colunas em uma <xref:System.Windows.Controls.DataGrid> tiverem larguras de \* e 2 \* , a primeira coluna receberia uma parte do espaço disponível e a segunda coluna receberia duas partes do espaço disponível.|  
  
 A <xref:System.Windows.Controls.DataGridLengthConverter> classe pode ser usada para converter dados entre valores numéricos ou de cadeia de caracteres e <xref:System.Windows.Controls.DataGridLength> valores.  
  
 Por padrão, a <xref:System.Windows.Controls.DataGrid.ColumnWidth%2A?displayProperty=nameWithType> propriedade é definida como <xref:System.Windows.Controls.DataGridLength.SizeToHeader%2A> e a <xref:System.Windows.Controls.DataGridColumn.Width%2A?displayProperty=nameWithType> propriedade é definida como <xref:System.Windows.Controls.DataGridLength.Auto%2A> . Quando o modo de dimensionamento for definido como <xref:System.Windows.Controls.DataGridLength.Auto%2A> ou <xref:System.Windows.Controls.DataGridLength.SizeToCells%2A> , as colunas aumentarão para a largura de seu conteúdo visível mais amplo. Ao rolar, esses modos de dimensionamento farão as colunas expandirem se o conteúdo que é maior do que o tamanho atual da coluna for colocado na exibição. A coluna não será reduzida depois que o conteúdo estiver fora da exibição.  
  
 As colunas no <xref:System.Windows.Controls.DataGrid> também podem ser definidas para dimensionar automaticamente somente dentro dos limites especificados, ou colunas podem ser definidas para um tamanho específico. A tabela a seguir mostra as propriedades que podem ser definidas para controlar o tamanho das colunas.  
  
|Propriedade|DESCRIÇÃO|  
|--------------|-----------------|  
|<xref:System.Windows.Controls.DataGrid.MaxColumnWidth%2A?displayProperty=nameWithType>|Define o limite superior para todas as colunas no <xref:System.Windows.Controls.DataGrid> .|  
|<xref:System.Windows.Controls.DataGridColumn.MaxWidth%2A?displayProperty=nameWithType>|Define o limite superior para uma coluna individual. Substitui <xref:System.Windows.Controls.DataGrid.MaxColumnWidth%2A?displayProperty=nameWithType>.|  
|<xref:System.Windows.Controls.DataGrid.MinColumnWidth%2A?displayProperty=nameWithType>|Define o limite inferior para todas as colunas no <xref:System.Windows.Controls.DataGrid> .|  
|<xref:System.Windows.Controls.DataGridColumn.MinWidth%2A?displayProperty=nameWithType>|Define o limite inferior para uma coluna individual. Substitui <xref:System.Windows.Controls.DataGrid.MinColumnWidth%2A?displayProperty=nameWithType>.|  
|<xref:System.Windows.Controls.DataGrid.ColumnWidth%2A?displayProperty=nameWithType>|Define uma largura específica para todas as colunas no <xref:System.Windows.Controls.DataGrid> .|  
|<xref:System.Windows.Controls.DataGridColumn.Width%2A?displayProperty=nameWithType>|Define uma largura específica para uma coluna individual. Substitui <xref:System.Windows.Controls.DataGrid.ColumnWidth%2A?displayProperty=nameWithType>.|  
  
### <a name="datagrid-column-headers"></a>Cabeçalhos da coluna DataGrid  
 Por padrão, os <xref:System.Windows.Controls.DataGrid> cabeçalhos de coluna são exibidos. Para ocultar os cabeçalhos de coluna, a <xref:System.Windows.Controls.DataGrid.HeadersVisibility%2A> propriedade deve ser definida como <xref:System.Windows.Controls.DataGridHeadersVisibility.Row?displayProperty=nameWithType> ou <xref:System.Windows.Controls.DataGridHeadersVisibility.None?displayProperty=nameWithType> . Por padrão, quando os cabeçalhos de coluna, eles são dimensionados automaticamente para ajustar seu conteúdo. Os cabeçalhos de coluna podem receber uma altura específica, definindo a <xref:System.Windows.Controls.DataGrid.ColumnHeaderHeight%2A?displayProperty=nameWithType> propriedade.  
  
### <a name="resizing-with-the-mouse"></a>Redimensionando com o mouse  
 Os usuários podem redimensionar <xref:System.Windows.Controls.DataGrid> linhas e colunas arrastando os divisores de cabeçalho de linha ou coluna. O <xref:System.Windows.Controls.DataGrid> também dá suporte ao redimensionamento automático de linhas e colunas clicando duas vezes no divisor de cabeçalho de linha ou coluna. Para impedir que um usuário redimensione colunas específicas, defina a <xref:System.Windows.Controls.DataGridColumn.CanUserResize%2A?displayProperty=nameWithType> propriedade como `false` para as colunas individuais. Para impedir que os usuários redimensionem todas as colunas, defina a <xref:System.Windows.Controls.DataGrid.CanUserResizeColumns%2A?displayProperty=nameWithType> propriedade como `false` . Para impedir que os usuários redimensionem todas as linhas, defina a <xref:System.Windows.Controls.DataGrid.CanUserResizeRows%2A?displayProperty=nameWithType> propriedade como `false` .  
  
## <a name="see-also"></a>Confira também

- <xref:System.Windows.Controls.DataGrid>
- <xref:System.Windows.Controls.DataGridColumn>
- <xref:System.Windows.Controls.DataGridLength>
- <xref:System.Windows.Controls.DataGridLengthConverter>
