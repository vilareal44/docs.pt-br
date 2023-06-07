---
title: Visão geral do Entity Framework
description: O Entity Framework no ADO.NET dá suporte ao desenvolvimento de aplicativos orientados a dados que funcionam em um nível mais alto de abstração do que os aplicativos tradicionais.
ms.date: 09/17/2018
ms.assetid: a2166b3d-d8ba-4a0a-8552-6ba1e3eaaee0
ms.openlocfilehash: 1f1ab5d44c2d6c7e1f54a761dbc706d537664ef6
ms.sourcegitcommit: 33deec3e814238fb18a49b2a7e89278e27888291
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/02/2020
ms.locfileid: "84286799"
---
# <a name="entity-framework-overview"></a>Visão geral de Entity Framework

O Entity Framework é um conjunto de tecnologias no ADO.NET que dão suporte ao desenvolvimento de aplicativos de software orientado a dados. Os arquitetos e desenvolvedores de aplicativos orientados a dados lutam com a necessidade de realizar dois objetivos muito diferentes. Precisam modelar as entidades, as relações e a lógica dos problemas de negócios que estão solucionando e também precisam trabalhar com os mecanismos de dados usados para armazenar e recuperar os dados. Os dados podem se estender por vários sistemas de armazenamento, cada um com seus próprios protocolos. Mesmo aplicativos que trabalhem com um único sistema de armazenamento devem balancear os requisitos do sistema de armazenamento com os requisitos de gravação eficiente e código de aplicativo que possa ser mantido.

O Entity Framework permite que os desenvolvedores trabalhem com dados na forma de objetos e propriedades específicos de domínio, como clientes e endereços de clientes, sem ter que se preocupar com as tabelas e colunas de banco de dados subjacentes em que esses data são armazenados. Com o Entity Framework, os desenvolvedores podem trabalhar em um nível mais alto de abstração ao lidar com dados e podem criar e manter aplicativos orientados a dados com menos códigos do que em aplicativos tradicionais. Como o Entity Framework é um componente do .NET Framework, os aplicativos Entity Framework podem ser executados em qualquer computador no qual o .NET Framework a partir da versão 3,5 SP1 esteja instalado.

## <a name="give-life-to-models"></a>Dê vida aos modelos
 Uma abordagem de design antiga e comum ao criar um aplicativo ou serviço é a divisão do aplicativo ou serviço em três partes: um modelo de domínio, um modelo lógico e um modelo físico. O modelo de domínio define as entidades e as relações no sistema que está sendo modelado. O modelo lógico de um banco de dados relacional normaliza as entidades e relações em tabelas com restrições de chave estrangeira. O modelo físico aborda os recursos de um mecanismo de dados específico definindo detalhes de armazenamento, como o particionamento e a indexação.

 O modelo físico é refinado pelos administradores de banco de dados para melhorar o desempenho, mas os programadores que escrevem código de aplicativos limitam-se principalmente a trabalhar com o modelo lógico escrevendo consultas SQL e chamando procedimentos armazenados. Os modelos de domínio geralmente são usados como uma ferramenta para capturar e comunicar os requisitos de um aplicativo, geralmente como diagramas inertes, que são exibidos e discutidos nas etapas iniciais de um projeto e em seguida abandonados. Muitas equipes de desenvolvimento ignoram a criação de um modelo conceitual e começam especificando tabelas, colunas e chaves em um banco de dados relacional.

 O Entity Framework dá vida aos modelos, permitindo que os desenvolvedores consultem entidades e relações no modelo de domínio (chamado de modelo *conceitual* na Entity Framework), ao mesmo tempo em que dependem do Entity Framework para converter essas operações em comandos específicos da fonte de dados. Isso libera os aplicativos de dependências embutidas no código em uma fonte de dados específica.

 Ao trabalhar com Code First, o modelo conceitual está mapeado para o modelo de armazenamento no código. O Entity Framework pode inferir o modelo conceitual com base nos tipos de objeto e configurações adicionais que você definir. Os metadados de mapeamento são gerados durante o tempo de execução com base em uma combinação de como você definiu seus tipos de domínio e informações de configurações adicionais que você fornece no código. Entity Framework gera o banco de dados conforme necessário com base nos metadados. Para obter mais informações, consulte [criando um modelo](/ef/ef6/modeling/).

 Ao trabalhar com as Ferramentas do Modelo de Dados de Entidade, o modelo conceitual, o modelo de armazenamento e os mapeamentos entre os dois são expressos em esquemas baseados em XML e definidos em arquivos que têm extensões de nome correspondentes:

- O CSDL (linguagem de definição de esquema conceitual) define o modelo conceitual. CSDL é a implementação do Entity Framework do [modelo de dados de entidade](../entity-data-model.md). A extensão do arquivo é .csdl.

- O SSDL (linguagem de definição de esquema de repositório) define o modelo de armazenamento, que também é chamado de modelo lógico. A extensão do arquivo é .ssdl.

- O MSL (linguagem de especificação de mapeamento) define os mapeamentos entre o armazenamento e os modelos conceituais. A extensão do arquivo é .msl.

O modelo e os mapeamentos de armazenamento podem ser alterados quando necessário sem necessidade de alterações no modelo conceitual, nas classes de dados ou no código do aplicativo. Como os modelos de armazenamento são específicos ao provedor, você pode trabalhar com um modelo conceitual consistente entre várias fontes de dados.

O Entity Framework usa esses arquivos de modelo e de mapeamento para criar, ler, atualizar e excluir operações em relação a entidades e relações no modelo conceitual para operações equivalentes na fonte de dados. O Entity Framework até mesmo dá suporte a entidades de mapeamento no modelo conceitual para procedimentos armazenados na fonte de dados. Para obter mais informações, consulte [especificações de CSDL, SSDL e MSL](/ef/ef6/modeling/designer/advanced/edmx/csdl-spec).

## <a name="map-objects-to-data"></a>Mapear objetos para dados
 A programação orientada a objeto impõe um desafio para interagir com os sistemas de armazenamento de dados. Embora a organização das classes com frequência espelhe a organização de tabelas de banco de dados relacional, o ajuste não é perfeito. Com frequência, várias tabelas normalizadas correspondem a uma única classe, e as relações entre classes são representadas de maneira diferente de como as relações entre tabelas são representadas. Por exemplo, para representar o cliente para um pedido de venda, uma classe `Order` pode usar uma propriedade que contém uma referência a uma instância de uma classe `Customer`, enquanto uma linha da tabela `Order` em um banco de dados contém uma coluna (ou conjunto de colunas) de chave estrangeira com um valor que corresponde a um valor de chave primária na tabela `Customer`. Uma classe `Customer` pode ter uma propriedade denominada `Orders` que contém uma coleção de instâncias da classe `Order`, enquanto a tabela `Customer` em um banco de dados não tem nenhuma coluna comparável. O Entity Framework fornece aos desenvolvedores a flexibilidade para representar as relações dessa forma ou para relações de modelo mais próximas, conforme elas são representadas no banco de dados.

 As soluções existentes tentaram preencher essa lacuna, que é frequentemente chamada de “incompatibilidade de impedância”, mapeando apenas classes e propriedades orientados a objetos para tabelas e colunas relacionais. Em vez de usar essa abordagem tradicional, o Entity Framework mapeia tabelas relacionais, colunas e restrições de chave estrangeira em modelos lógicos para entidades e relações em modelos conceituais. Isso permite maior flexibilidade para definir objetos e otimizar o modelo lógico. As ferramentas de Modelo de Dados de Entidade geram classes de dados extensíveis com base no modelo conceitual. Essas classes são classes parciais que podem ser estendidas com a adição de membros feita pelo desenvolvedor. Por padrão, as classes geradas para um modelo conceitual específico derivam de classes base que fornecem serviços para materializar entidades como objetos e para controlar e salvar alterações. Os desenvolvedores podem usar essas classes para trabalhar com as entidades e relações como objetos relacionados por associações. Os desenvolvedores também podem personalizar as classes que são geradas para um modelo conceitual. Para obter mais informações, consulte [trabalhando com objetos](working-with-objects.md).

## <a name="access-and-change-entity-data"></a>Acessar e alterar dados de entidade

Mais do que apenas outra solução de mapeamento relacional de objeto, o Entity Framework é fundamentalmente para permitir que os aplicativos acessem e alterem dados que são representados como entidades e relações no modelo conceitual. O Entity Framework usa informações no modelo e arquivos de mapeamento para converter consultas de objeto em tipos de entidade representados no modelo conceitual em consultas específicas à fonte de dados. Os resultados da consulta são materializados em objetos que o Entity Framework gerencia. O Entity Framework fornece as seguintes maneiras de consultar um modelo conceitual e retornar objetos:

- LINQ to Entities. Fornece suporte a LINQ (consulta integrada à linguagem) para consultar tipos de entidade que são definidos em um modelo conceitual. Para obter mais informações, consulte [LINQ to Entities](./language-reference/linq-to-entities.md).

- [!INCLUDE[esql](../../../../../includes/esql-md.md)]. Um dialeto de SQL independente de armazenamento que funciona diretamente com entidades no modelo conceitual e que dá suporte a conceitos de Modelo de Dados de Entidade. [!INCLUDE[esql](../../../../../includes/esql-md.md)]é usado com consultas de objeto e consultas que são executadas usando o provedor EntityClient. Para obter mais informações, consulte [Entity SQL visão geral](./language-reference/entity-sql-overview.md).

O Entity Framework inclui o provedor de dados EntityClient. Esse provedor gerencia as conexões, traduz as consultas de entidade em consultas específicas à fonte de dados e retorna um leitor de dados que o Entity Framework usa para materializar dados de entidade em objetos. Quando a materialização de objeto não é necessária, o provedor EntityClient também pode ser usado como um provedor de dados ADO.NET padrão, permitindo que os aplicativos executem [!INCLUDE[esql](../../../../../includes/esql-md.md)] consultas e consumam o leitor de dados somente leitura retornado. Para obter mais informações, consulte [EntityClient Provider para o Entity Framework](entityclient-provider-for-the-entity-framework.md).

O diagrama a seguir ilustra a arquitetura de Entity Framework para acessar dados:

![Diagrama de arquitetura do Entity Framework](./media/wd-efarchdiagram.gif "wd_EFArchDiagram")

As ferramentas de Modelo de Dados de Entidade podem gerar uma classe derivada de `System.Data.Objects.ObjectContext` ou `System.Data.Entity.DbContext` que representa o contêiner de entidade no modelo conceitual. Esse contexto de objeto fornece os recursos para controlar alterações e gerenciar identidades, simultaneidade e relações. Essa classe também expõe um método `SaveChanges` que grava inserções, atualizações e exclusões na fonte de dados. Como consultas, essas alterações são feitas pelos comandos gerados automaticamente pelo sistema ou por procedimentos armazenados especificados pelo desenvolvedor.

## <a name="data-providers"></a>Provedores de dados

O `EntityClient` provedor estende o modelo de provedor ADO.net acessando dados em termos de entidades conceituais e relações. Executa consultas que usam [!INCLUDE[esql](../../../../../includes/esql-md.md)]. O [!INCLUDE[esql](../../../../../includes/esql-md.md)] fornece a linguagem de consulta subjacente que permite que o `EntityClient` se comunique com o banco de dados. Para obter mais informações, consulte [EntityClient Provider para o Entity Framework](entityclient-provider-for-the-entity-framework.md).

O Entity Framework inclui um Provedor de Dados de SqlClient atualizado que dá suporte a árvores de comando canônicas. Para obter mais informações, consulte [SqlClient para o Entity Framework](sqlclient-for-the-entity-framework.md).

## <a name="entity-data-model-tools"></a>Ferramentas de modelo de dados de entidade

Junto com o tempo de execução do Entity Framework, o Visual Studio inclui as ferramentas de mapeamento e modelagem. Para obter mais informações, consulte [modelagem e mapeamento](modeling-and-mapping.md).

## <a name="learn-more"></a>Saiba mais

Para saber mais sobre o Entity Framework, consulte:

[Introdução](getting-started.md) -fornece informações sobre como colocar em funcionamento rapidamente usando o guia de [início rápido](https://docs.microsoft.com/previous-versions/dotnet/netframework-4.0/bb399182(v=vs.100)), que mostra como criar um aplicativo simples de Entity Framework.

[Terminologia Entity Framework](terminology.md) – define muitos dos termos que são introduzidos pelo modelo de dados de entidade e pelo Entity Framework e que são usados na documentação do Entity Framework.

[Recursos de Entity Framework](resources.md) -fornece links para tópicos conceituais e links para tópicos e recursos externos para a criação de aplicativos Entity Framework.

## <a name="see-also"></a>Veja também

- [ADO.NET Entity Framework](index.md)
