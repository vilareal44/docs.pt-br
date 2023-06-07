---
title: Arquitetura
description: 'Saiba mais sobre a arquitetura do ADO.NET, incluindo os dois componentes principais para acessar e manipular dados: os provedores de dados do .NET Framework e o DataSet.'
ms.date: 03/30/2017
ms.assetid: fcd45b99-ae8f-45ab-8b97-d887beda734e
ms.openlocfilehash: 91e5b1e33ed1bc6e2acf6068a03bb8185324470d
ms.sourcegitcommit: 33deec3e814238fb18a49b2a7e89278e27888291
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/02/2020
ms.locfileid: "84287175"
---
# <a name="adonet-architecture"></a>Arquitetura do ADO.NET
Antigamente, o processamento de dados dependia basicamente de um modelo de duas camadas baseado em conexão. Como o processamento de dados usa cada vez mais arquiteturas de várias camadas, os programadores estão adotando uma abordagem desconectada para fornecer melhor escalabilidade a seus aplicativos.  
  
## <a name="adonet-components"></a>Componentes do ADO.NET  
 Os dois componentes principais do ADO.NET para acessar e manipular dados são os provedores de dados .NET Framework e o <xref:System.Data.DataSet> .  
  
### <a name="net-framework-data-providers"></a>Provedores de dados .NET Framework  
 Os Provedores de Dados .NET Framework são componentes que foram criados explicitamente para manipulação de dados, e o rápido acesso a dados apenas de encaminhamento e somente leitura. O objeto `Connection` fornece conectividade a uma fonte de dados. O objeto `Command` permite o acesso aos comandos de banco de dados para retornar dados, modificar dados, executar procedimentos armazenados, e enviar ou recuperar informações de parâmetro. O `DataReader` fornece um fluxo de dados de alto desempenho da fonte de dados. Por fim, o `DataAdapter` faz a ponte entre o objeto `DataSet` e a fonte de dados. O `DataAdapter` usa os objetos `Command` para executar comandos SQL na fonte de dados a fim de carregar o `DataSet` com dados e reconciliar as alterações que foram feitas nos dados no `DataSet` com a fonte de dados. Para obter mais informações, consulte [.NET Framework provedores de dados](data-providers.md) e [recuperar e modificar dados no ADO.net](retrieving-and-modifying-data.md).  
  
### <a name="the-dataset"></a>O DataSet  
 O `DataSet` do ADO.NET é criado explicitamente para o acesso independente aos dados de qualquer fonte de dados. Consequentemente, ele pode ser usado com várias fontes de dados diferentes, usado com dados XML ou usado para gerenciar o local dos dados no aplicativo. O `DataSet` contém uma coleção de um ou mais objetos <xref:System.Data.DataTable> que consiste em linhas e em colunas de dados, e também em chave primária, chave estrangeira, restrição e informações de relação sobre os dados nos objetos `DataTable`. Para obter mais informações, consulte [conjuntos de dados, tabelas e](./dataset-datatable-dataview/index.md)dados.  
  
 O diagrama a seguir ilustra a relação entre um provedor de dados .NET Framework e um `DataSet` .  
  
 ![Gráfico do ADO.Net](./media/ado-1-bpuedev11.png "ado_1_bpuedev11")  
Arquitetura do ADO.NET  
  
### <a name="choosing-a-datareader-or-a-dataset"></a>Escolhendo um DataReader ou um DataSet  
 Quando você decide se seu aplicativo deve usar um `DataReader` (consulte [recuperando dados usando um DataReader](retrieving-data-using-a-datareader.md)) ou um `DataSet` (consulte [DataSets, DataTables e DataViews](./dataset-datatable-dataview/index.md)), considere o tipo de funcionalidade que seu aplicativo requer. Use um `DataSet` para fazer o seguinte:  
  
- Armazenar dados em cache localmente no aplicativo para que você possa manipulá-lo. Se você precisar ler somente os resultados de uma consulta, o `DataReader` será a melhor opção.  
  
- Estabelecer a comunicação remota dos dados entre camadas ou em um serviço Web XML.  
  
- Interagir com os dados dinamicamente, por exemplo, fazendo a associação a um controle do Windows Forms ou combinando e relacionando dados de várias fontes.  
  
- Executar um processamento extensivo nos dados sem precisar de uma conexão aberta com a fonte de dados, o que libera a conexão a ser usada por outros clientes.  
  
 Se você não precisar da funcionalidade fornecida pelo `DataSet`, poderá melhorar o desempenho do aplicativo usando o `DataReader` para retornar os dados apenas para encaminhamento e somente leitura. Embora o `DataAdapter` use o `DataReader` para preencher o conteúdo de um `DataSet` (consulte [Populando um conjunto de um DataSet de um DataAdapter](populating-a-dataset-from-a-dataadapter.md)), usando o `DataReader` , você pode aumentar o desempenho porque você economizará memória que seria consumida pelo `DataSet` e evitaria o processamento necessário para criar e preencher o conteúdo do `DataSet` .  
  
## <a name="linq-to-dataset"></a>LINQ to DataSet  
 O LINQ to DataSet fornece recursos de consulta e verificação de tipo em tempo de compilação nos dados armazenados em cache em um objeto DataSet. Ele permite que você grave consultas em uma das linguagens de desenvolvimento do .NET Framework, como C# ou Visual Basic. Para obter mais informações, consulte [LINQ to DataSet](linq-to-dataset.md).  
  
## <a name="linq-to-sql"></a>LINQ to SQL  
 O LINQ to SQL oferece suporte a consultas em um modelo de objeto que é mapeado para as estruturas de dados de um banco de dados relacional sem usar um modelo conceitual intermediário. Cada tabela é representada por uma classe separada, acoplando com segurança o modelo de objeto ao esquema do banco de dados relacional. O LINQ to SQL converte consultas integradas à linguagem no modelo de objeto na Transact-SQL e as envia ao banco de dados para execução. Quando o banco de dados retorna os resultados, o LINQ to SQL converte os resultados novamente em objetos. Para obter mais informações, consulte [LINQ to SQL](./sql/linq/index.md).  
  
## <a name="adonet-entity-framework"></a>ADO.NET Entity Framework  
 O ADO.NET Entity Framework foi criado para permitir que os desenvolvedores criem aplicativos de acesso a dados programando em um modelo de aplicativo conceitual, em vez de programar diretamente em um esquema de armazenamento relacional. O objetivo é diminuir a quantidade de código e de manutenção necessários a aplicativos orientados a dados. Para obter mais informações, consulte [ADO.NET Entity Framework](./ef/index.md).  
  
## <a name="wcf-data-services"></a>WCF Data Services  
 WCF Data Services é usado para implantar serviços de dados na Web ou em uma intranet. Os dados são estruturados como entidades e relações de acordo com as especificações do Modelo de Dados de Entidade. Os dados implantados nesse modelo são endereçáveis pelo protocolo HTTP padrão. Para obter mais informações, consulte [WCF Data Services 4,5](../wcf/index.md).  
  
## <a name="xml-and-adonet"></a>XML e ADO.NET  
 O ADO.NET aproveita o poder do XML para fornecer acesso desconectado aos dados. O ADO.NET foi projetado de lado a lado com as classes XML na .NET Framework; Ambos são componentes de uma única arquitetura.  
  
 ADO.NET e as classes XML no .NET Framework convergem no `DataSet` objeto. O `DataSet` pode ser preenchido com dados de uma fonte XML, quer ele seja um arquivo ou um fluxo XML. O `DataSet` pode ser gravado como XML compatível com World-Wide Web Consortium (W3C) que inclui seu esquema como a linguagem de definição de esquema XML (XSD), independentemente da fonte dos dados no `DataSet`. Como o formato nativo de serialização do `DataSet` é XML, ele é um meio excelente para mover os dados entre as camadas, tornando o `DataSet` uma opção ideal para a comunicação remota dos dados e do contexto de esquema para e de um serviço Web XML. Para obter mais informações, consulte [Documentos e dados XML](../../../standard/data/xml/index.md).  
  
## <a name="see-also"></a>Veja também

- [Visão geral do ADO.NET](ado-net-overview.md)
