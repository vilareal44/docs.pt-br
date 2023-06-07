---
title: 'Passo a passo: modelo e consulta de objeto simples (C#)'
description: Siga este passo a passos para criar uma classe de entidade que modela uma tabela em um banco de dados de exemplo. Em seguida, crie uma consulta simples para listar clientes em um determinado local.
ms.date: 03/30/2017
ms.assetid: 419961cc-92d6-45f5-ae8a-d485bdde3a37
ms.openlocfilehash: 4637fabecc1726d8fec12857a667073912cfbed5
ms.sourcegitcommit: 33deec3e814238fb18a49b2a7e89278e27888291
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/02/2020
ms.locfileid: "84286295"
---
# <a name="walkthrough-simple-object-model-and-query-c"></a>Passo a passo: modelo e consulta de objeto simples (C#)

Este passo a passo fornece um cenário completo fundamental do [!INCLUDE[vbtecdlinq](../../../../../../includes/vbtecdlinq-md.md)] com complexidades mínimas. Você criará uma classe de entidade que modela a tabela Customers no banco de dados de exemplo Northwind. Em seguida, criará uma consulta simples para listar os clientes que estão localizados em Londres.

Este passo a passo é orientado a código por design para ajudar a mostrar os conceitos do [!INCLUDE[vbtecdlinq](../../../../../../includes/vbtecdlinq-md.md)]. Em termos gerais, você usaria o Object Relational Designer para criar seu modelo de objeto.

[!INCLUDE[note_settings_general](../../../../../../includes/note-settings-general-md.md)]

Esse passo a passo foi escrito usando as configurações de desenvolvimento do Visual C#.

## <a name="prerequisites"></a>Pré-requisitos

- Este passo a passo usa uma pasta dedicada ("c:\linqtest5") para armazenar arquivos. Crie essa pasta antes de iniciar o passo a passo.

- Este passo a passo requer o banco de dados de exemplo Northwind. Se você não tiver esse banco de dados no seu computador de desenvolvimento, poderá baixá-lo no site de download da Microsoft. Para obter instruções, consulte [baixar bancos de dados de exemplo](downloading-sample-databases.md). Depois de baixar o banco de dados, copie o arquivo para a pasta c:\linqtest5.

## <a name="overview"></a>Visão geral

Este passo a passo consiste em seis tarefas principais:

- Criando uma [!INCLUDE[vbtecdlinq](../../../../../../includes/vbtecdlinq-md.md)] solução no Visual Studio.

- Mapear uma classe para uma tabela do banco de dados.

- Designar propriedades na classe para representar colunas do banco de dados.

- Especificar a conexão ao banco de dados Northwind.

- Criar uma consulta simples para execução no banco de dados.

- Executar a consulta e observar os resultados.

## <a name="creating-a-linq-to-sql-solution"></a>Criando uma solução LINQ to SQL

Nesta primeira tarefa, você cria uma solução do Visual Studio que contém as referências necessárias para compilar e executar um [!INCLUDE[vbtecdlinq](../../../../../../includes/vbtecdlinq-md.md)] projeto.

### <a name="to-create-a-linq-to-sql-solution"></a>Para criar uma solução LINQ to SQL

1. No menu **arquivo** do Visual Studio, aponte para **novo**e clique em **projeto**.

2. No painel **tipos de projeto** da caixa de diálogo **novo projeto** , clique em **Visual C#**.

3. No painel **Modelos**, clique em **Aplicativo de Console**.

4. Na caixa **nome** , digite **LinqConsoleApp**.

5. Na caixa **local** , verifique onde você deseja armazenar os arquivos de projeto.

6. Clique em **OK**.

## <a name="adding-linq-references-and-directives"></a>Adicionando referências e diretivas LINQ

Este passo a passo usa assemblies que não podem ser instalados por padrão em seu projeto. Se System. Data. Linq não estiver listado como uma referência em seu projeto (expanda o nó **referências** no **Gerenciador de soluções**), adicione-o, conforme explicado nas etapas a seguir.

### <a name="to-add-systemdatalinq"></a>Para adicionar System.Data.Linq

1. Em **Gerenciador de soluções**, clique com o botão direito do mouse em **referências**e clique em **Adicionar referência**.

2. Na caixa de diálogo **Adicionar referência** , clique em **.net**, clique no assembly System. Data. LINQ e, em seguida, clique em **OK**.

     O assembly é adicionado ao projeto.

3. Adicione as seguintes diretivas na parte superior de **Program.cs**:

     [!code-csharp[DLinqWalk1CS#1](../../../../../../samples/snippets/csharp/VS_Snippets_Data/DLinqWalk1CS/cs/Program.cs#1)]

## <a name="mapping-a-class-to-a-database-table"></a>Mapeando uma classe para uma tabela do banco de dados

Nesta etapa, você cria uma classe e a mapeia para uma tabela do banco de dados. Essa classe é chamada de classe de *entidade*. Observe que o mapeamento é realizado simplesmente adicionando o atributo <xref:System.Data.Linq.Mapping.TableAttribute>. A propriedade <xref:System.Data.Linq.Mapping.TableAttribute.Name%2A> especifica o nome da tabela no banco de dados.

### <a name="to-create-an-entity-class-and-map-it-to-a-database-table"></a>Para criar uma classe de entidade e mapeá-la para uma tabela do banco de dados.

- Digite ou cole o código a seguir em Program.cs imediatamente acima da declaração de classe `Program`:

     [!code-csharp[DLinqWalk1CS#2](../../../../../../samples/snippets/csharp/VS_Snippets_Data/DLinqWalk1CS/cs/Program.cs#2)]

## <a name="designating-properties-on-the-class-to-represent-database-columns"></a>Designando propriedades na classe para representar colunas do banco de dados

Nesta etapa, você realiza várias tarefas.

- Você usa o atributo <xref:System.Data.Linq.Mapping.ColumnAttribute> para designar as propriedades `CustomerID` e `City` na classe de entidade como representação de colunas na tabela do banco de dados.

- Você designa a propriedade `CustomerID` como a representação de uma coluna de chave primária no banco de dados.

- Você designa os campos `_CustomerID` e `_City` para armazenamento particular. O [!INCLUDE[vbtecdlinq](../../../../../../includes/vbtecdlinq-md.md)] pode então armazenar e recuperar valores diretamente, em vez de usar acessadores públicos, que podem incluir lógica de negócios.

### <a name="to-represent-characteristics-of-two-database-columns"></a>Para representar características de duas colunas do banco de dados

- Digite ou cole o código a seguir em Program.cs dentro das chaves para a classe `Customer`.

     [!code-csharp[DLinqWalk1CS#3](../../../../../../samples/snippets/csharp/VS_Snippets_Data/DLinqWalk1CS/cs/Program.cs#3)]

## <a name="specifying-the-connection-to-the-northwind-database"></a>Especificando a conexão ao banco de dados Northwind

Nesta etapa você usa um objeto <xref:System.Data.Linq.DataContext> para estabelecer uma conexão entre as estruturas de dados baseadas em código e o próprio banco de dados. O <xref:System.Data.Linq.DataContext> é o canal principal por meio do qual você recupera objetos do banco de dados e envia alterações.

Você também declara um `Table<Customer>` para atuar como a tabela lógica tipada para suas consultas na tabela Customers do banco de dados. Você criará e executará essas consultas em etapas posteriores.

### <a name="to-specify-the-database-connection"></a>Para especificar a conexão do banco de dados

- Digite ou cole o código a seguir no método `Main`:

     Observe que supõe-se que o arquivo `northwnd.mdf` está na pasta linqtest5. Para obter mais informações, consulte a seção de pré-requisitos anteriormente neste passo a passo.

     [!code-csharp[DLinqWalk1CS#4](../../../../../../samples/snippets/csharp/VS_Snippets_Data/DLinqWalk1CS/cs/Program.cs#4)]

## <a name="creating-a-simple-query"></a>Criando uma consulta simples

Nesta etapa, você cria uma consulta para localizar quais clientes da tabela Customers do banco de dados estão localizados em Londres. O código da consulta nesta etapa apenas descreve a consulta. Não a executa. Essa abordagem é conhecida como *execução adiada*. Para obter mais informações, consulte [Introdução a Consultas de LINQ (C#)](../../../../../csharp/programming-guide/concepts/linq/introduction-to-linq-queries.md).

Você também gerará uma saída de log para mostrar os comandos SQL gerados pelo [!INCLUDE[vbtecdlinq](../../../../../../includes/vbtecdlinq-md.md)]. Este recurso de log (que usa <xref:System.Data.Linq.DataContext.Log%2A>) é útil para depuração e para determinar se os comandos que estão sendo enviados ao banco de dados representam precisamente sua consulta.

### <a name="to-create-a-simple-query"></a>Para criar uma consulta simples

- Digite ou cole o código a seguir no método `Main` depois da declaração `Table<Customer>`.

     [!code-csharp[DLinqWalk1ACS#5](../../../../../../samples/snippets/csharp/VS_Snippets_Data/DLinqWalk1ACS/cs/Program.cs#5)]

## <a name="executing-the-query"></a>Executando a consulta

Nesta etapa, você realmente executa a consulta. As expressões de consulta que você criou nas etapas anteriores não são avaliadas até que os resultados sejam necessários. Quando você começa a iteração `foreach`, um comando SQL é executado no banco de dados e os objetos são materializados.

### <a name="to-execute-the-query"></a>Para executar a consulta.

1. Digite ou cole o seguinte código ao final do método `Main` (após a descrição de consulta).

     [!code-csharp[DLinqWalk1ACS#6](../../../../../../samples/snippets/csharp/VS_Snippets_Data/DLinqWalk1ACS/cs/Program.cs#6)]

2. Pressione F5 para depurar o aplicativo.

    > [!NOTE]
    > Se seu aplicativo gerar um erro de tempo de execução, consulte a seção de solução de problemas do [Learning by passo a passos](learning-by-walkthroughs.md).

     Os resultados da consulta na janela do console devem aparecer da seguinte maneira:

     `ID=AROUT, City=London`

     `ID=BSBEV, City=London`

     `ID=CONSH, City=London`

     `ID=EASTC, City=London`

     `ID=NORTS, City=London`

     `ID=SEVES, City=London`

3. Pressione Enter na janela Console para fechar o aplicativo.

## <a name="next-steps"></a>Próximas etapas

O tópico [passo a passos: consultando entre relações (C#)](walkthrough-querying-across-relationships-csharp.md) continua onde este passo a passos termina. A consulta através de instruções de relações demonstra como o [!INCLUDE[vbtecdlinq](../../../../../../includes/vbtecdlinq-md.md)] pode consultar em tabelas, semelhantes a *junções* em um banco de dados relacional.

Se você desejar realizar o passo a passo Consulta entre relações, salve a solução do passo a passo que você acabou de concluir, que é um pré-requisito.

## <a name="see-also"></a>Veja também

- [Aprendendo com explicações passo a passo](learning-by-walkthroughs.md)
