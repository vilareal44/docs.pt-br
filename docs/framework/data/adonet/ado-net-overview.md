---
title: Visão geral
description: Leia uma visão geral do ADO.NET em .NET Framework e leia sobre os recursos para obter explicações e exemplos mais detalhados.
ms.date: 03/30/2017
ms.assetid: ee3bc1d8-11db-4be4-89eb-c708cf04117d
ms.openlocfilehash: 2ff3b7ad197bfe1e1c12e382f3a59bd470c57a75
ms.sourcegitcommit: 33deec3e814238fb18a49b2a7e89278e27888291
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/02/2020
ms.locfileid: "84287149"
---
# <a name="adonet-overview"></a>Visão geral do ADO.NET
O ADO.NET fornece acesso consistente a fontes de dados como o SQL Server e o XML, e a fontes de dados expostas através do OLE DB e do ODBC. Os aplicativos do consumidor de compartilhamento de dados podem usar o ADO.NET para se conectar a essas fontes de dados, e para recuperar, manipular e atualizar os dados nelas contidos.  
  
 O ADO.NET separa o acesso a dados da manipulação de dados em componentes discretos que podem ser usados separadamente ou em tandem. O ADO.NET inclui os provedores de dados do .NET Framework para se conectar a um banco de dados, executar comandos e recuperar resultados. Esses resultados são processados diretamente, colocados em um objeto <xref:System.Data.DataSet> do ADO.NET para serem expostos para o usuário ad hoc, combinados com dados de várias fontes ou passados entre as camadas. O objeto `DataSet` também pode ser usado independentemente de um provedor de dados .NET Framework para gerenciar o local dos dados para o aplicativo ou originado no XML.  
  
 As classes do ADO.NET estão no System.Data.dll e são integradas às classes XML encontradas no System.Xml.dll. Para obter um exemplo de código que se conecta a um banco de dados do, recupera-os e exibe os dados em uma janela de console, consulte [exemplos de código ADO.net](ado-net-code-examples.md).  
  
 O ADO.NET fornece a funcionalidade para os desenvolvedores que gravam um código gerenciado semelhante à funcionalidade fornecida aos desenvolvedores COM nativos pelos objetos ActiveX Data Objects (ADO). É recomendável que você use o ADO.NET, e não o ADO, para acessar dados nos aplicativos .NET.  
  
 O ADO.NET fornece o método mais direto de acesso a dados no .NET Framework. Para uma abstração de nível superior que permite que os aplicativos funcionem em um modelo conceitual em vez do modelo de armazenamento subjacente, consulte o [Entity Framework ADO.net](./ef/index.md).  
  
 **Política de privacidade**: os assemblies System. Data. dll, System. Data. Design. dll, System. Data. OracleClient. dll, System. Data. SQLXML. dll, System. Data. Linq. dll, System. Data. SqlServerCe. dll e System. Data. DataSetExtensions. dll não fazem distinção entre os dados privados de um usuário e os dados não privados.  Esses assemblies não coletam, não armazenam nem transmitem dados privados de nenhum usuário. No entanto, os aplicativos de terceiros podem coletar, armazenar ou transmitir dados privados de um usuário usando esses assemblies.  
  
## <a name="in-this-section"></a>Nesta seção  
 [Arquitetura do ADO.NET](ado-net-architecture.md)  
 Fornece uma visão geral da arquitetura e dos componentes do ADO.NET.  
  
 [Opções e diretrizes da tecnologia ADO.NET](ado-net-technology-options-and-guidelines.md)  
 Descreve os produtos e as tecnologias incluídos na plataforma de dados de entidade.  
  
 [LINQ e o ADO.NET](linq-and-ado-net.md)  
 Descreve como a consulta integrada à linguagem (LINQ) é implementada no ADO.NET e fornece links para tópicos relevantes.  
  
 [Provedores de dados do .NET Framework](data-providers.md)  
 Fornece uma visão geral do design do provedor de dados .NET Framework e dos provedores de dados .NET Framework incluídos no ADO.NET.  
  
 [DataSets ADO.NET](ado-net-datasets.md)  
 Fornece uma visão geral do design e dos componentes do `DataSet`.  
  
 [Execução lado a lado no ADO.NET](side-by-side-execution.md)  
 Aborda as diferenças entre as versões do ADO.NET e seu efeito na execução lado a lado e na compatibilidade entre aplicativos.  
  
 [Exemplos de código do ADO.NET](ado-net-code-examples.md)  
 Fornece exemplos de código que recuperam dados usando os provedores de dados ADO.NET.  
  
## <a name="related-sections"></a>Seções relacionadas  
 [Novidades no ADO.NET](whats-new.md)  
 Apresenta recursos que são novos no ADO.NET.  
  
 [Protegendo aplicativos ADO.NET](securing-ado-net-applications.md)  
 Descreve práticas seguras de codificação ao usar o ADO.NET.  
  
 [Data Type Mappings in ADO.NET](data-type-mappings-in-ado-net.md) (Mapeamentos de tipo de dados no ADO.NET)  
 Descreve os mapeamentos de tipo de dados entre os tipos de dados do .NET Framework e os provedores de dados .NET Framework.  
  
 [Retrieving and Modifying Data in ADO.NET](retrieving-and-modifying-data.md) (Recuperando e modificando dados no ADO.NET)  
 Descreve como conectar-se a uma fonte de dados, recuperar dados e modificar dados. Isso inclui `DataReaders` e `DataAdapters`.  
  
## <a name="see-also"></a>Veja também

- [ADO.NET](index.md)
- [Acessando dados no Visual Studio](/visualstudio/data-tools/accessing-data-in-visual-studio)
