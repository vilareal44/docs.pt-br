---
title: Opções e diretrizes de tecnologia
ms.date: 03/30/2017
ms.assetid: c8577281-38e6-4ce5-b036-572039a4c3d8
ms.openlocfilehash: e4016511920904ea14eac844a2564d6a77d9a817
ms.sourcegitcommit: 71b8f5a2108a0f1a4ef1d8d75c5b3e129ec5ca1e
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/29/2020
ms.locfileid: "84202285"
---
# <a name="adonet-technology-options-and-guidelines"></a>Opções e diretrizes da tecnologia ADO.NET

A Plataforma de Dados do ADO.NET é uma estratégia de várias versões para diminuir a quantidade de codificação e de manutenção necessária para habilitar os desenvolvedores para programarem com modelos de dados de entidade conceituais. Essa plataforma inclui o ADO.NET Entity Framework e as tecnologias relacionadas.  
  
## <a name="entity-framework"></a>Entity Framework  
 O ADO.NET Entity Framework foi criado para permitir que os desenvolvedores criem aplicativos de acesso a dados programando em um modelo de aplicativo conceitual, em vez de programar diretamente em um esquema de armazenamento relacional. O objetivo é diminuir a quantidade de código e de manutenção necessários a aplicativos orientados a dados. Para obter mais informações, consulte [ADO.NET Entity Framework](./ef/index.md).  
  
### <a name="entity-data-model-edm"></a>Modelo de Dados de Entidade (EDM)  
 Um EDM (Modelo de Dados de Entidade) é uma especificação de design que define os dados do aplicativo como conjuntos de entidades e relacionamentos. Os dados nesse modelo dão suporte ao mapeamento relacional de objetos e à capacidade de programação de dados entre os limites do aplicativo.  
  
### <a name="object-services"></a>Serviços de Objeto  
 Os Serviços de Objeto permitem que os programadores interajam com o modelo conceitual através de um conjunto de classes CLR (Common Language Runtime). Essas classes podem ser geradas automaticamente no modelo conceitual ou podem ser desenvolvidas independente para refletir a estrutura do modelo conceitual. Os Serviços de Objeto também fornecem suporte à infraestrutura do Entity Framework, incluindo serviços como gerenciamento de estado, controle de alterações, resolução de identidade, relacionamentos de carregamento e navegação, propagação de alterações de objetos para modificações do banco de dados e suporte à criação de consultas para o Entity SQL. Para obter mais informações, consulte [Visão geral dos serviços de objeto (Entity Framework)](https://docs.microsoft.com/previous-versions/bb386871(v=vs.100)).  
  
### <a name="linq-to-entities"></a>LINQ to Entities  
 LINQ to Entities é uma implementação de LINQ (consulta integrada à linguagem) que permite aos desenvolvedores criar consultas com rigidez de tipos no contexto do objeto Entity Framework usando expressões LINQ e operadores de consulta padrão do LINQ. LINQ to Entities permite que os desenvolvedores trabalhem com um modelo conceitual com um mapeamento relacional de objeto flexível entre Microsoft SQL Server e bancos de dados de terceiros. Para obter mais informações, consulte [LINQ to Entities](./ef/language-reference/linq-to-entities.md).  
  
### <a name="entity-sql"></a>Entity SQL  
 O Entity SQL é uma linguagem de consulta baseada em texto criada para interagir com um Modelo de Dados de Entidade. O Entity SQL é um dialeto SQL que contém constructos para consulta em termos dos conceitos de modelagem de nível mais alto, como herança, tipos complexos e relações explícitas. Os desenvolvedores também podem usar o Entity SQL diretamente com Serviços de Objeto. Para obter mais informações, consulte [Entity SQL Language](./ef/language-reference/entity-sql-language.md).  
  
### <a name="entityclient"></a>EntityClient  
 O EntityClient é um novo provedor de dados .NET Framework usado para interagir com um Modelo de Dados de Entidade. O EntityClient segue o padrão do provedor de dados .NET Framework de expor os objetos <xref:System.Data.EntityClient.EntityConnection> e <xref:System.Data.EntityClient.EntityCommand> que retornam um <xref:System.Data.EntityClient.EntityDataReader>. O EntityClient funciona com a linguagem Entity SQL, fornecendo mapeamento flexível para provedores de dados específicos a armazenamento. Para obter mais informações, consulte [EntityClient Provider para o Entity Framework](./ef/entityclient-provider-for-the-entity-framework.md).  
  
### <a name="entity-data-model-tools"></a>Ferramentas do Modelo de Dados de Entidade  
 O Entity Framework fornece ferramentas de linha de comando, assistentes e designers para facilitar a criação de aplicativos EDM. O controle EntityDataSource dá suporte a cenários de vinculação de dados baseados no EDM. A superfície de programação do controle EntityDataSource é semelhante a outros controles de fonte de dados no Visual Studio. Para obter mais informações, consulte [ADO.NET modelo de dados de entidade Tools](https://docs.microsoft.com/previous-versions/dotnet/netframework-4.0/bb399249(v=vs.100)).  
  
## <a name="linq-to-sql"></a>LINQ to SQL  
 O LINQ to SQL é uma implementação do OR/M (mapeamento relacional de objeto) que permite que você modele um banco de dados SQL Server usando classes do .NET Framework. LINQ to SQL permite que você consulte seu banco de dados usando LINQ, bem como atualizar, inserir e excluir. O LINQ to SQL dá suporte a transações, exibições e procedimentos armazenados, fornecendo uma maneira fácil de integrar regras de validação de dados e de lógica de negócios no modelo de dados. Você pode usar o O/R Designer (Designer Relacional de Objeto) para modelar as classes e as associações de entidades que são baseadas em objetos em um banco de dados. Para obter mais informações, consulte [Ferramentas LINQ to SQL no Visual Studio](/visualstudio/data-tools/linq-to-sql-tools-in-visual-studio2).  
  
## <a name="wcf-data-services"></a>WCF Data Services  
 WCF Data Services implanta serviços de dados na Web ou em uma intranet. Os dados são estruturados como entidades e relações de acordo com as especificações do Modelo de Dados de Entidade. Os dados implantados nesse modelo são endereçáveis pelo protocolo HTTP padrão. Para obter mais informações, consulte [WCF Data Services 4,5](../wcf/index.md).  
  
## <a name="see-also"></a>Veja também

- [Visão geral do ADO.NET](ado-net-overview.md)
- [Novidades no ADO.NET](whats-new.md)
