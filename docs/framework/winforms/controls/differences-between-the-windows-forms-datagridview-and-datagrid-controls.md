---
title: Diferenças entre os controles DataGridView e DataGrid
description: Aprenda as várias diferenças entre Windows Forms os recursos de controles DataGridView e DataGrid, bem como as diferenças em sua arquitetura.
ms.date: 03/30/2017
helpviewer_keywords:
- data grids [Windows Forms
- DataGrid control [Windows Forms], DataGridView control compared
- DataGridView control [Windows Forms], DataGrid control compared
ms.assetid: d412c786-140e-4210-8a56-a68467530a55
ms.openlocfilehash: 1438182d764097ae4f8fd7df046ad72c9213da19
ms.sourcegitcommit: cb27c01a8b0b4630148374638aff4e2221f90b22
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/09/2020
ms.locfileid: "86174582"
---
# <a name="differences-between-the-windows-forms-datagridview-and-datagrid-controls"></a>Diferenças entre os controles DataGridView e DataGrid dos Windows Forms
O <xref:System.Windows.Forms.DataGridView> controle é um novo controle que substitui o <xref:System.Windows.Forms.DataGrid> controle. O <xref:System.Windows.Forms.DataGridView> controle fornece vários recursos básicos e avançados que estão faltando no <xref:System.Windows.Forms.DataGrid> controle. Além disso, a arquitetura do <xref:System.Windows.Forms.DataGridView> controle torna muito mais fácil estender e personalizar do que o <xref:System.Windows.Forms.DataGrid> controle.  
  
 A tabela a seguir descreve alguns dos principais recursos disponíveis no <xref:System.Windows.Forms.DataGridView> controle que estão faltando no <xref:System.Windows.Forms.DataGrid> controle.  
  
|Recurso de controle DataGridView|Descrição|  
|----------------------------------|-----------------|  
|Vários tipos de coluna|O <xref:System.Windows.Forms.DataGridView> controle fornece mais tipos de coluna internos do que o <xref:System.Windows.Forms.DataGrid> controle. Esses tipos de coluna atendem às necessidades dos cenários mais comuns, mas também são mais fáceis de estender ou substituir do que os tipos de coluna no <xref:System.Windows.Forms.DataGrid> controle. Para obter mais informações, consulte [Tipos de coluna no controle DataGridView dos Windows Forms](column-types-in-the-windows-forms-datagridview-control.md).|  
|Várias maneiras de exibir dados|O <xref:System.Windows.Forms.DataGrid> controle é limitado à exibição de dados de uma fonte de dados externa. O <xref:System.Windows.Forms.DataGridView> controle, no entanto, pode exibir dados desvinculados armazenados no controle, dados de uma fonte de dados ligada ou dados vinculados e desvinculados juntos. Você também pode implementar o modo virtual no <xref:System.Windows.Forms.DataGridView> controle para fornecer gerenciamento de dados personalizado. Para obter mais informações, consulte [Modos de exibição dos dados no controle DataGridView dos Windows Forms](data-display-modes-in-the-windows-forms-datagridview-control.md).|  
|Várias maneiras de personalizar a exibição de dados|O <xref:System.Windows.Forms.DataGridView> controle fornece muitas propriedades e eventos que permitem especificar como os dados são formatados e exibidos. Por exemplo, você pode alterar a aparência de células, linhas e colunas dependendo dos dados que elas contêm ou pode substituir os dados de um tipo de dados por dados equivalente de outro tipo. Para obter mais informações, consulte [Formatação de dados no controle DataGridView dos Windows Forms](data-formatting-in-the-windows-forms-datagridview-control.md).|  
|Várias opções para alterar o comportamento e a aparência de célula, linha, coluna e cabeçalho|O <xref:System.Windows.Forms.DataGridView> controle permite que você trabalhe com componentes individuais de grade de várias maneiras. Por exemplo, você pode congelar linhas e colunas para evitar que elas rolem; ocultar linhas, colunas e cabeçalhos; alterar o modo como os tamanhos de linha, coluna e cabeçalho são ajustados; alterar a maneira como os usuários fazem seleções; e fornecer Dicas de Ferramentas e menus de atalho para células, linhas e colunas individuais.|  
  
 O <xref:System.Windows.Forms.DataGrid> controle é retido para compatibilidade com versões anteriores e necessidades especiais. Para quase todas as finalidades, você deve usar o <xref:System.Windows.Forms.DataGridView> controle. O único recurso que está disponível no <xref:System.Windows.Forms.DataGrid> controle que não está disponível no <xref:System.Windows.Forms.DataGridView> controle é a exibição hierárquica das informações de duas tabelas relacionadas em um único controle. Você deve usar dois <xref:System.Windows.Forms.DataGridView> controles para exibir informações de duas tabelas que estão em uma relação mestre/detalhes.  
  
## <a name="upgrading-to-the-datagridview-control"></a>Atualizando para o controle DataGridView  
 Se você tiver aplicativos existentes que usam o <xref:System.Windows.Forms.DataGrid> controle em um cenário simples vinculado a dados sem personalizações, poderá simplesmente substituir o controle antigo pelo novo controle. Ambos os controles usam a arquitetura de ligação de dados do Windows Forms padrão, de modo que o <xref:System.Windows.Forms.DataGridView> controle exibirá os dados associados sem nenhuma configuração adicional necessária. No entanto, talvez você queira aproveitar as melhorias de vinculação de dados, associando os dados a um <xref:System.Windows.Forms.BindingSource> componente, que você pode associar ao <xref:System.Windows.Forms.DataGridView> controle. Para obter mais informações, consulte [Componente BindingSource](bindingsource-component.md).  
  
 Como o <xref:System.Windows.Forms.DataGridView> controle tem uma arquitetura totalmente nova, não há nenhum caminho de conversão direto que permitirá o uso <xref:System.Windows.Forms.DataGrid> de personalizações com o <xref:System.Windows.Forms.DataGridView> controle. Muitas <xref:System.Windows.Forms.DataGrid> personalizações são desnecessárias com o <xref:System.Windows.Forms.DataGridView> controle, no entanto, devido aos recursos internos disponíveis no novo controle. Se você tiver criado tipos de coluna personalizados para o <xref:System.Windows.Forms.DataGrid> controle que deseja usar com o <xref:System.Windows.Forms.DataGridView> controle, precisará implementá-los novamente usando a nova arquitetura. Para obter mais informações, consulte [Personalizando o controle DataGridView dos Windows Forms](customizing-the-windows-forms-datagridview-control.md).  
  
## <a name="see-also"></a>Veja também

- <xref:System.Windows.Forms.DataGridView>
- <xref:System.Windows.Forms.DataGrid>
- <xref:System.Windows.Forms.BindingSource>
- [Controle DataGridView](datagridview-control-windows-forms.md)
- [Controle DataGrid](datagrid-control-windows-forms.md)
- [Componente BindingSource](bindingsource-component.md)
- [Tipos de coluna no controle DataGridView dos Windows Forms](column-types-in-the-windows-forms-datagridview-control.md)
- [Estilos de Célula no Controle DataGridView dos Windows Forms](cell-styles-in-the-windows-forms-datagridview-control.md)
- [Modos de exibição de dados no controle DataGridView dos Windows Forms](data-display-modes-in-the-windows-forms-datagridview-control.md)
- [Formatação de dados no controle DataGridView dos Windows Forms](data-formatting-in-the-windows-forms-datagridview-control.md)
- [Dimensionando opções no controle DataGridView dos Windows Forms](sizing-options-in-the-windows-forms-datagridview-control.md)
- [Modos de classificação da coluna no controle DataGridView do Windows Forms](column-sort-modes-in-the-windows-forms-datagridview-control.md)
- [Modos de seleção no controle DataGridView dos Windows Forms](selection-modes-in-the-windows-forms-datagridview-control.md)
- [Personalizando o controle DataGridView dos Windows Forms](customizing-the-windows-forms-datagridview-control.md)
