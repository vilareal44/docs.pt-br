---
title: Associação de dados
description: Saiba como associar a uma matriz de valores que você calcula no tempo de execução, ler de um arquivo ou derivar dos valores de outros controles em Windows Forms.
ms.date: 03/30/2017
helpviewer_keywords:
- master-details lists
- data [Windows Forms], data binding
- reports [Windows Forms], Windows Forms
- lookup tables [Windows Forms], data binding
- data [Windows Forms], complex data binding
- data binding [Windows Forms], Windows Forms
- data [Windows Forms], simple data binding
- Windows Forms controls, data binding
- data-bound controls [Windows Forms], Windows Forms
ms.assetid: 419aac5e-819b-4aad-88b0-73a2f8c0bd27
ms.openlocfilehash: 78e8a3287c565cfa7dae56b68c0eb57f48c30ea2
ms.sourcegitcommit: e02d17b2cf9c1258dadda4810a5e6072a0089aee
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/01/2020
ms.locfileid: "85619657"
---
# <a name="data-binding-and-windows-forms"></a>Associação de dados e o Windows Forms
No Windows Forms, você pode vincular não apenas a fontes de dados tradicionais, mas também a praticamente qualquer estrutura que contenha dados. Você pode vincular a uma matriz de valores que você calcula no tempo de execução, lê de um arquivo ou deriva dos valores de outros controles.  
  
 Além disso, você pode vincular qualquer propriedade de qualquer controle à fonte de dados. Na vinculação de dados tradicional, você geralmente vincula a propriedade de exibição — por exemplo, a propriedade <xref:System.Windows.Forms.Control.Text%2A> de um controle <xref:System.Windows.Forms.TextBox> — à fonte de dados. Com o .NET Framework, você também tem a opção de definir outras propriedades por meio de associação também. É possível usar a vinculação para realizar as seguintes tarefas:  
  
- Definir o grafo de um controle de imagem.  
  
- Definir a cor do plano de fundo de um ou mais controles.  
  
- Definir o tamanho dos controles.  
  
 Basicamente, a vinculação de dados é uma maneira automática de configurar qualquer propriedade acessível do tempo de execução de qualquer controle em um formulário.  
  
## <a name="types-of-data-binding"></a>Tipos de vinculação de dados  
 O Windows Forms pode tirar proveito de dois tipos de vinculação de dados: vinculação simples e vinculação complexa. Cada uma oferece vantagens diferentes.  
  
|Tipo de vinculação de dados|Descrição|  
|--------------------------|-----------------|  
|Vinculação de dados simples|A capacidade de um controle em vincular a um elemento de dados único, tal como um valor em uma coluna em uma tabela do conjunto de dados. Esse é o tipo de vinculação típica para controles, tais como um controle <xref:System.Windows.Forms.TextBox> ou um controle <xref:System.Windows.Forms.Label>, que são controles que normalmente exibem apenas um único valor. Na verdade, qualquer propriedade em um controle pode ser vinculada a um campo em um banco de dados. Há amplo suporte para esse recurso no Visual Studio.<br /><br /> Para obter mais informações, consulte:<br /><br /> -   [Interfaces relacionadas à vinculação de dados](interfaces-related-to-data-binding.md)<br />-   [Como navegar dados no Windows Forms](how-to-navigate-data-in-windows-forms.md)<br />-   [Como criar um controle de associação simples em um formulário do Windows](how-to-create-a-simple-bound-control-on-a-windows-form.md)|  
|Vinculação de dados complexos|A capacidade de um controle para vincular a mais de um elemento de dados, geralmente mais de um registro em um banco de dados. A vinculação complexa também é chamada de vinculação baseada em lista. Exemplos de controles que suportam vinculação complexa são os controles <xref:System.Windows.Forms.DataGridView>, <xref:System.Windows.Forms.ListBox> e <xref:System.Windows.Forms.ComboBox>. Para obter um exemplo de vinculação de dados complexa, consulte [como associar um Windows Forms caixa de combinação ou controle ListBox aos dados](./controls/how-to-bind-a-windows-forms-combobox-or-listbox-control-to-data.md).|  
  
## <a name="bindingsource-component"></a>Componente BindingSource  
 Para simplificar a vinculação de dados, o Windows Forms permite que você vincule uma fonte de dados ao componente <xref:System.Windows.Forms.BindingSource> e, em seguida, vincule controles à <xref:System.Windows.Forms.BindingSource>. Você pode usar a <xref:System.Windows.Forms.BindingSource> em cenários de vinculação simples ou complexos. Em ambos os casos, a <xref:System.Windows.Forms.BindingSource> atua como intermediário entre a fonte de dados e os controles de vinculação, fornecendo gerenciamento de moeda para notificação de alteração e outros serviços.  
  
## <a name="common-scenarios-that-employ-data-binding"></a>Cenários comuns que utilizam a vinculação de dados  
 Praticamente todos os aplicativos comerciais usam leitura de informações de fontes de dados de um tipo ou de outro, normalmente por meio da vinculação de dados. A lista a seguir mostra alguns dos cenários mais comuns que utilizam a vinculação de dados, como o método de apresentação de dados e a manipulação.  
  
|Cenário|Descrição|  
|--------------|-----------------|  
|Relatório|Os relatórios fornecem uma maneira flexível para exibir e resumir dados em um documento impresso. É muito comum criar um relatório que imprima o conteúdo selecionado de uma fonte de dados na tela ou em uma impressora. Relatórios comuns incluem listas, faturas e resumos. Os itens são geralmente formatados em colunas de listas, com subitens organizados em cada item da lista, mas você deve escolher o layout que melhor atenda aos dados.|  
|Entrada de dados|É uma forma comum de inserir grandes quantidades de dados relacionados ou de solicitar informações aos usuários por meio de um formulário de entrada de dados. Os usuários podem inserir informações ou selecionar opções usando caixas de texto, botões de opção, listas suspensas e caixas de seleção. As informações são então enviadas e armazenadas em um banco de dados, cuja estrutura se baseia nas informações inseridas.|  
|Relação mestre/detalhes|Um aplicativo mestre/detalhes é um formato para pesquisa em dados relacionados. Especificamente, há duas tabelas de dados com uma relação de conexão entre elas — no exemplo de negócios clássico, uma tabela "Clientes" e uma tabela "Pedidos" com uma relação entre elas vinculando clientes e seus respectivos pedidos. Para obter mais informações sobre como criar um aplicativo mestre/de detalhes com dois <xref:System.Windows.Forms.DataGridView> controles Windows Forms, consulte [como: criar um formulário mestre/detalhado usando dois controles DataGridView Windows Forms](./controls/create-a-master-detail-form-using-two-datagridviews.md)|  
|Tabela de pesquisa|Outro cenário comum de apresentação/manipulação de dados é a pesquisa de tabela. Geralmente, como parte de uma exibição de dados maior, um controle <xref:System.Windows.Forms.ComboBox> é usado para exibir e manipular os dados. A chave é que os dados exibidos no controle <xref:System.Windows.Forms.ComboBox> sejam diferentes dos dados gravados no banco de dados. Por exemplo, se você tiver um controle <xref:System.Windows.Forms.ComboBox> exibindo os itens disponíveis em uma mercearia, provavelmente gostaria de ver os nomes dos produtos (pão, leite, ovos). No entanto, para facilitar a recuperação de informações do banco de dados e para a normalização do banco de dados, você provavelmente armazenará as informações dos itens específicos em uma determinada ordem como números de item (nº 501, nº 603 e assim por diante). Assim, há uma conexão implícita entre o "nome amigável" do item na mercearia no controle <xref:System.Windows.Forms.ComboBox> em seu formulário e o número do item relacionado que está presente em um pedido. Essa é a essência de uma pesquisa de tabela. Para obter mais informações, consulte [como: criar uma tabela de pesquisa com o componente Windows Forms BindingSource](./controls/how-to-create-a-lookup-table-with-the-windows-forms-bindingsource-component.md).|  
  
## <a name="see-also"></a>Consulte também

- <xref:System.Windows.Forms.Binding>
- [Associação de dados do Windows Forms](windows-forms-data-binding.md)
- [Como associar o controle DataGrid do Windows Forms a uma fonte de dados](./controls/how-to-bind-the-windows-forms-datagrid-control-to-a-data-source.md)
- [Componente BindingSource](./controls/bindingsource-component.md)
