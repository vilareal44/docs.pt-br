---
title: Recuperando e modificando dados
description: No .NET Framework, os provedores de dados no ADO.NET servem como uma ponte entre um aplicativo e uma fonte de dados para ler e atualizar dados.
ms.date: 03/30/2017
ms.assetid: 722e7f87-3691-46c6-87e8-7d159722d675
ms.openlocfilehash: f916324dc829526a5e6b0078021b09786755f666
ms.sourcegitcommit: 33deec3e814238fb18a49b2a7e89278e27888291
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/02/2020
ms.locfileid: "84286605"
---
# <a name="retrieving-and-modifying-data-in-adonet"></a>Recuperando e modificando dados no ADO.NET
A função principal de qualquer aplicativo de banco de dados é conectar-se a uma fonte de dados e recuperar os dados que ele contém. Os .NET Framework provedores de dados do ADO.NET servem como uma ponte entre um aplicativo e uma fonte de dados, permitindo que você execute comandos, bem como recuperar dados usando um **DataReader** ou um **DataAdapter**. A função principal de qualquer aplicativo de banco de dados é a capacidade de atualizar os dados que estão armazenados no banco de dados. No ADO.NET, a atualização de dados envolve o uso de **DataAdapter** e e de <xref:System.Data.DataSet> objetos **Command** ; e também pode envolver o uso de transações.  
  
## <a name="in-this-section"></a>Nesta seção  
 [Conectando a uma Fonte de Dados](connecting-to-a-data-source.md)  
 Descreve como estabelecer uma conexão com uma fonte de dados e como trabalhar com eventos de conexão.  
  
 [Cadeias de conexão](connection-strings.md)  
 Contém os tópicos que descrevem os vários aspectos do uso de cadeias de conexão, incluindo palavras-chave de cadeias de conexão, informações de segurança e seu respectivo armazenamento e recuperação.  
  
 [Pool de conexões](connection-pooling.md)  
 Descreve o pool de conexões para provedores de dados .NET Framework.  
  
 [Comandos e parâmetros](commands-and-parameters.md)  
 Contém os tópicos que descrevem como criar comandos e construtores de comandos, configurar parâmetros e executar comandos para recuperar e modificar dados.  
  
 [DataAdapters e DataReaders](dataadapters-and-datareaders.md)  
 Contém os tópicos que descrevem DataReaders, DataAdapters, parâmetros, manipulação de eventos DataAdapter e execução de operações em lote.  
  
 [Transações e simultaneidade](transactions-and-concurrency.md)  
 Contém os tópicos que descrevem como executar transações locais, transações distribuídas e trabalho com concorrência otimista.  
  
 [Recuperando identidade ou valores de Autonumber](retrieving-identity-or-autonumber-values.md)  
 Fornece um exemplo de mapeamento dos valores gerados para uma coluna de **identidade** em uma tabela SQL Server ou para um campo **AutoNumeração** em uma tabela do Microsoft Access, para uma coluna de uma linha inserida em uma tabela. Discute como mesclar valores de identidade em `DataTable`.  
  
 [Recuperando dados binários](retrieving-binary-data.md)  
 Descreve como recuperar dados binários ou estruturas de dados grandes usando o `CommandBehavior` .`SequentialAccess` para modificar o comportamento padrão de um `DataReader` .  
  
 [Modificando dados com procedimentos armazenados](modifying-data-with-stored-procedures.md)  
 Descreve como usar parâmetros de entrada e de saída de procedimentos armazenados para inserir uma linha em um banco de dados, retornando um novo valor de identidade.  
  
 [Recuperando informações de esquema de banco de dados](retrieving-database-schema-information.md)  
 Descreve como obter bancos de dados ou catálogos disponíveis, tabelas e modos de exibição em um banco de dados, restrições existentes para tabelas e outras informações de esquema de uma fonte de dados.  
  
 [DbProviderFactories](dbproviderfactories.md)  
 Descreve o modelo de fábrica do provedor e demonstra como usar as classes base no namespace `System.Data.Common`.  
  
 [Rastreamento de dados no ADO.NET](data-tracing.md)  
 Descreve como o ADO.NET fornece a funcionalidade interna de rastreamento de dados.  
  
 [Contadores de desempenho](performance-counters.md)  
 Descreve os contadores de desempenho disponíveis para `SqlClient` e `OracleClient`.  
  
 [Programação assíncrona](asynchronous-programming.md)  
 Descreve o suporte do ADO.NET para programação assíncrona.  
  
 [Suporte de streaming do SqlClient](sqlclient-streaming-support.md)  
 Discute como escrever aplicativos que transmitem dados de SQL Server sem tê-los totalmente carregados na memória.  
  
## <a name="see-also"></a>Veja também

- [Data Type Mappings in ADO.NET](data-type-mappings-in-ado-net.md) (Mapeamentos de tipo de dados no ADO.NET)
- [DataSets, DataTables e DataViews](./dataset-datatable-dataview/index.md)
- [Protegendo aplicativos ADO.NET](securing-ado-net-applications.md)
- [SQL Server e ADO.NET](./sql/index.md)
- [Visão geral do ADO.NET](ado-net-overview.md)
