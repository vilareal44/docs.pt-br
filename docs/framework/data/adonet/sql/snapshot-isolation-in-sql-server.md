---
title: Isolamento de instantâneo no SQL Server
description: Leia uma visão geral de isolamento de instantâneo e controle de versão de linha no SQL Server e saiba como gerenciar a simultaneidade com níveis de isolamento.
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
ms.assetid: 43ae5dd3-50f5-43a8-8d01-e37a61664176
ms.openlocfilehash: 7fa769448dd922925a5eccf4c85bd1840155df68
ms.sourcegitcommit: 33deec3e814238fb18a49b2a7e89278e27888291
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/02/2020
ms.locfileid: "84286238"
---
# <a name="snapshot-isolation-in-sql-server"></a>Isolamento de instantâneo no SQL Server
O isolamento de instantâneo aprimora a simultaneidade para aplicativos OLTP.  
  
## <a name="understanding-snapshot-isolation-and-row-versioning"></a>Entendendo o isolamento de instantâneo e o controle de versão de linha  
 Depois que o isolamento de instantâneo estiver habilitado, as versões de linha atualizadas para cada transação deverão ser mantidas.  Antes do SQL Server 2019, essas versões eram armazenadas em **tempdb**. SQL Server 2019 apresenta um novo recurso, a ADR (recuperação de banco de dados acelerada) que exige seu próprio conjunto de versões de linha.  Portanto, a partir de SQL Server 2019, se ADR não estiver habilitado, as versões de linha serão mantidas em **tempdb** como sempre.  Se ADR estiver habilitado, todas as versões de linha, relacionadas ao isolamento de instantâneo e ADR, serão mantidas no repositório de versão persistente do ADR (PVS), que está localizado no banco de dados do usuário em um grupo de arquivos que o usuário especifica. Um número de sequência de transação exclusivo identifica cada transação. Esses números exclusivos são registrados para cada versão de linha. A transação funciona com as versões de linha mais recentes que têm um número de sequência anterior ao número de sequência da transação. As versões de linha mais recentes criadas depois que a transação foi iniciada são ignoradas pela transação.  
  
 O termo "instantâneo" reflete o fato de que todas as consultas na transação veem a mesma versão (ou instantâneo) do banco de dados, com base no estado do banco de dados no momento em que a transação é iniciada. Nenhum bloqueio é adquirido nas linhas de dados subjacentes ou páginas de dados em uma transação de instantâneo, o que permite que outras transações sejam executadas sem serem bloqueadas por uma transação anterior não concluída. Transações que modificam dados não bloqueiam transações que leem dados e transações que leem dados não bloqueiam transações que gravam dados, como normalmente fariam no nível de isolamento READ COMMITTED padrão no SQL Server. Esse comportamento de não bloqueio também reduz consideravelmente a probabilidade de deadlocks para transações complexas.  
  
 O isolamento de instantâneo usa um modelo de simultaneidade otimista. Se uma transação de instantâneo tentar confirmar modificações a dados alterados desde o início da transação, a transação será revertida e um erro será gerado. Você poderá evitar isso usando dicas de UPDLOCK para instruções SELECT que acessem dados a serem modificados. Para obter mais informações, confira "Dicas de bloqueio" nos Manuais Online do SQL Server.  
  
 Antes de ser usado em transações, o isolamento de instantâneo precisa ser habilitado ao configurar a opção ALLOW_SNAPSHOT_ISOLATION no banco de dados. Isso ativa o mecanismo para armazenar versões de linha no banco de dados temporário (**tempdb**). Você precisa habilitar o isolamento de instantâneo, usando a instrução ALTER DATABASE do Transact-SQL, em cada banco de dados que usa esse recurso. Nesse sentido, o isolamento de instantâneos difere dos níveis de isolamento tradicionais de READ COMMITTED, REPEATABLE READ, SERIALIZABLE e READ UNCOMMITTED, que não exigem nenhuma configuração. As seguintes instruções ativam o isolamento de instantâneo e substituem o comportamento READ COMMITTED padrão por SNAPSHOT:  
  
```sql  
ALTER DATABASE MyDatabase  
SET ALLOW_SNAPSHOT_ISOLATION ON  
  
ALTER DATABASE MyDatabase  
SET READ_COMMITTED_SNAPSHOT ON  
```  
  
 Configurar a opção READ_COMMITTED_SNAPSHOT ON permite o acesso a linhas com controle de versão no nível de isolamento READ COMMITTED padrão. Se a opção READ_COMMITTED_SNAPSHOT for definida como OFF, você precisará definir explicitamente o nível de isolamento do instantâneo para cada sessão a fim de acessar linhas com controle de versão.  
  
## <a name="managing-concurrency-with-isolation-levels"></a>Gerenciando simultaneidade com níveis de isolamento  
 O nível de isolamento sob o qual uma instrução Transact-SQL é executada determina o comportamento de bloqueio e de controle de versão de linha dessa instrução. Um nível de isolamento tem escopo que abrange toda a conexão e, uma vez definido para uma conexão com a instrução SET TRANSACTION ISOLATION LEVEL, ele permanece em vigor até que a conexão seja fechada ou outro nível de isolamento seja definido. Quando uma conexão é fechada e retornada ao pool, o nível de isolamento da última instrução SET TRANSACTION ISOLATION LEVEL é mantido. As conexões subsequentes reutilizando uma conexão em pool usam o nível de isolamento que estava em vigor no momento em que a conexão em questão foi colocada em pool.  
  
 As consultas individuais emitidas em uma conexão podem conter dicas de bloqueio que modificam o isolamento para uma só instrução ou transação, mas não afetam o nível de isolamento da conexão. Os níveis de isolamento ou as dicas de bloqueio definidas em procedimentos armazenados ou funções não alteram o nível de isolamento da conexão que os chama e estão em vigor apenas pela duração do procedimento armazenado ou da chamada de função.  
  
 Quatro níveis de isolamento definidos no padrão SQL-92 são compatíveis com versões anteriores do SQL Server:  
  
- READ UNCOMMITTED é o nível de isolamento menos restritivo, pois ignora os bloqueios estabelecidos por outras transações. Transações em execução sob READ UNCOMMITTED podem ler valores de dados modificados que ainda não foram confirmados por outras transações; essas são chamadas de leituras "sujas".  
  
- READ COMMITTED é o nível de isolamento padrão para o SQL Server. Ele impede a realização de leituras sujas especificando que as instruções não podem ler valores de dados que foram modificados, mas ainda não confirmados por outras transações. Outras transações ainda podem modificar, inserir ou excluir dados entre execuções de instruções individuais dentro da transação atual, resultando em leituras não repetíveis ou dados fantasmas.  
  
- REPEATABLE READ é um nível de isolamento mais restritivo do que READ COMMITTED. Ele engloba READ COMMITTED e, adicionalmente, especifica que nenhuma outra transação pode modificar nem excluir dados que tenham sido lidos pela transação atual até que a transação atual seja confirmada. A simultaneidade é menor do que a encontrada em READ COMMITTED, porque os bloqueios compartilhados nos dados lidos são mantidos até o término da transação, em vez de serem liberados ao final de cada instrução.  
  
- SERIALIZABLE é o nível de isolamento mais restritivo, pois ele bloqueia intervalos de chaves inteiros até que a transação seja concluída. Ele engloba REPEATABLE READ e adiciona a restrição de que outras transações não podem inserir novas linhas em intervalos que foram lidos pela transação até que a transação esteja concluída.  
  
 Para obter mais informações, confira o [Guia de controle de versão de linha e bloqueio de transação](/sql/relational-databases/sql-server-transaction-locking-and-row-versioning-guide).  
  
### <a name="snapshot-isolation-level-extensions"></a>Extensões de nível de isolamento de instantâneo  
 O SQL Server introduziu extensões nos níveis de isolamento SQL-92 com a introdução do nível de isolamento do SNAPSHOT e uma implementação adicional de READ COMMITTED. O nível de isolamento READ_COMMITTED_SNAPSHOT pode substituir READ COMMITTED de maneira transparente para todas as transações.  
  
- O nível de isolamento SNAPSHOT especifica que dados lidos dentro de uma transação nunca refletirão alterações feitas por outras transações simultâneas. A transação usa as versões de linhas de dados que existem quando a transação é iniciada. Nenhum bloqueio é colocado nos dados quando eles são lidos, então transações com nível de isolamento SNAPSHOT não bloqueiam a gravação de dados por outras transações. Transações que gravam dados não impedem que transações snapshot leiam dados. Para poder usar o isolamento de instantâneo, você precisa habilitá-lo, configurando a opção de banco de dados ALLOW_SNAPSHOT_ISOLATION.  
  
- A opção de banco de dados READ_COMMITTED_SNAPSHOT determina o comportamento do nível de isolamento READ COMMITTED padrão quando o isolamento de instantâneo está habilitado em um banco de dados. Se você não especificar READ_COMMITTED_SNAPSHOT como ON, READ COMMITTED será aplicado a todas as transações implícitas. Isso resulta no mesmo comportamento obtido ao configurar READ_COMMITTED_SNAPSHOT como OFF (o padrão). Quando READ_COMMITTED_SNAPSHOT OFF está em vigor, o Mecanismo de Banco de Dados usa bloqueios compartilhados para impor o nível de isolamento padrão. Se você definir a opção de banco de dados READ_COMMITTED_SNAPSHOT como ON, o mecanismo de banco de dados usará o controle de versão de linha e o isolamento de instantâneo como o padrão, em vez de usar bloqueios para proteger os dados.  
  
## <a name="how-snapshot-isolation-and-row-versioning-work"></a>Como funcionam o isolamento de instantâneo e o controle de versão de linha  
 Quando o nível de isolamento SNAPSHOT está habilitado, cada vez que uma linha é atualizada, o Mecanismo de Banco de Dados do SQL Server armazena uma cópia da linha original em **tempdb** e adiciona um número de sequência da transação à linha. A seguinte sequência de eventos ocorre:  
  
- Uma nova transação é iniciada e recebe um número de sequência de transação.  
  
- O Mecanismo de Banco de Dados lê uma linha dentro da transação e recupera a versão de linha de **tempdb** ao qual o número de sequência está próximo, e menor do que o número de sequência de transação.  
  
- O Mecanismo de Banco de Dados verifica se o número de sequência da transação não está na lista de números de sequência de transação das transações não confirmadas ativas quando a transação de instantâneo foi iniciada.  
  
- A transação lê a versão da linha de **tempdb** que estava atual no início da transação. Ela não verá novas linhas inseridas depois que a transação foi iniciada, porque esses valores de número de sequência serão maiores que o valor do número de sequência da transação.  
  
- A transação atual verá as linhas que foram excluídas depois que a transação iniciou, porque haverá uma versão de linha em **tempdb** com um valor menor do número de sequência.  
  
 O resultado efetivo do isolamento de instantâneo é que a transação vê todos os dados como eles existiam no início da transação, sem respeitar nem colocar nenhum bloqueio nas tabelas subjacentes. Isso pode resultar em aprimoramentos de desempenho em situações em que há contenção.  
  
 Uma transação de instantâneo sempre usa o controle de simultaneidade otimista, transacionando todos os bloqueios que impediriam que outras transações atualizassem as linhas. Se uma transação de instantâneo tentar confirmar uma atualização para uma linha que foi alterada após o início da transação, a transação será revertida e um erro será gerado.  
  
## <a name="working-with-snapshot-isolation-in-adonet"></a>Trabalhando com isolamento de instantâneo no ADO.NET  
 O isolamento de instantâneo é compatível com o ADO.NET graças ao uso da classe <xref:System.Data.SqlClient.SqlTransaction>. Se um banco de dados tiver sido habilitado para o isolamento de instantâneo, mas não estiver configurado para READ_COMMITTED_SNAPSHOT ON, você deverá iniciar um <xref:System.Data.SqlClient.SqlTransaction> usando o valor de enumeração **IsolationLevel.Snapshot** ao chamar o método <xref:System.Data.SqlClient.SqlConnection.BeginTransaction%2A>. Esse fragmento de código pressupõe que a conexão é um objeto <xref:System.Data.SqlClient.SqlConnection> aberto.  
  
```vb  
Dim sqlTran As SqlTransaction = _  
  connection.BeginTransaction(IsolationLevel.Snapshot)  
```  
  
```csharp  
SqlTransaction sqlTran =
  connection.BeginTransaction(IsolationLevel.Snapshot);  
```  
  
### <a name="example"></a>Exemplo  
 O exemplo a seguir demonstra como os diferentes níveis de isolamento se comportam ao tentar acessar dados bloqueados e não se destina a ser usado em código de produção.  
  
 O código conecta-se ao banco de dados de exemplo **AdventureWorks** no SQL Server e cria uma tabela denominada **TestSnapshot** e insere uma linha de dados. O código usa a instrução ALTER DATABASE do Transact-SQL para ativar o isolamento de instantâneo para o banco de dados, mas não define a opção READ_COMMITTED_SNAPSHOT, deixando o comportamento padrão de nível de isolamento READ COMMITTED em vigor. Em seguida, o código executa as seguintes ações:  
  
- Ele começa, mas não conclui, sqlTransaction1, que usa o nível de isolamento SERIALIZABLE para iniciar uma transação de atualização. Isso faz com que a tabela seja bloqueada.  
  
- Abre uma segunda conexão e inicia uma segunda transação usando o nível de isolamento SNAPSHOT para ler os dados na tabela **TestSnapshot**. Como o isolamento de instantâneo está habilitado, essa transação pode ler os dados que existiam antes de a sqlTransaction1 ser iniciada.  
  
- Abre uma terceira conexão e inicia uma transação usando o nível de isolamento READ COMMITTED para tentar ler os dados na tabela. Nesse caso, o código não pode ler os dados porque não é possível ler após os bloqueios colocados na tabela na primeira transação e expirar. O mesmo resultado ocorreria se os níveis de isolamento REPETIdos de leitura e SERIALIZÁvel fossem usados porque esses níveis de isolamento também não conseguem ler os bloqueios colocados na primeira transação.  
  
- Ele abre uma quarta conexão e inicia uma transação usando o nível de isolamento READ UNCOMMITTED, que executa uma leitura suja do valor não confirmado em sqlTransaction1. Esse valor poderá nunca realmente existir no banco de dados se a primeira transação não for confirmada.  
  
- Ele primeiro reverte a primeira transação e a limpa excluindo a tabela **TestSnapshot** e desativando o isolamento de instantâneo para o banco de dados **AdventureWorks**.  
  
> [!NOTE]
> Os exemplos a seguir usam a mesma cadeia de conexão com o pool de conexão desativado. Se uma conexão for colocada em pool, redefinir o nível de isolamento dela não redefinirá o nível de isolamento no servidor. Como resultado, as conexões subsequentes que usam a mesma conexão interna em pool começam com seus respectivos níveis de isolamento definidos para o da conexão em pool. Uma alternativa para desativar o pool de conexões é definir o nível de isolamento explicitamente para cada conexão.  
  
 [!code-csharp[DataWorks SnapshotIsolation.Demo#1](../../../../../samples/snippets/csharp/VS_Snippets_ADO.NET/DataWorks SnapshotIsolation.Demo/CS/source.cs#1)]
 [!code-vb[DataWorks SnapshotIsolation.Demo#1](../../../../../samples/snippets/visualbasic/VS_Snippets_ADO.NET/DataWorks SnapshotIsolation.Demo/VB/source.vb#1)]  
  
### <a name="example"></a>Exemplo  
 O exemplo a seguir demonstra o comportamento do isolamento de instantâneo quando os dados estão sendo modificados. O código executa as seguintes ações:  
  
- Conecta-se a um banco de dados de exemplo **AdventureWorks** e permite o isolamento de SNAPSHOT.  
  
- Cria uma tabela denominada **TestSnapshotUpdate** e insere três linhas de dados de exemplo.  
  
- Começa sqlTransaction1 usando o isolamento de instantâneo, mas não a conclui. Três linhas de dados são selecionadas na transação.  
  
- Cria um segundo **SqlConnection** para **AdventureWorks** e cria uma segunda transação usando o nível de isolamento READ COMMITTED que atualiza um valor em uma das linhas selecionadas em sqlTransaction1.  
  
- Confirma sqlTransaction2.  
  
- Retorna para sqlTransaction1 e tenta atualizar a mesma linha que sqlTransaction1 já confirmou. O erro 3960 é gerado e sqlTransaction1 é revertida automaticamente. O **SqlException.Number** e **SqlException.Message** são exibidos na janela do console.  
  
- Executa o código de limpeza para desligar o isolamento de instantâneo no **AdventureWorks** e excluir a tabela **TestSnapshotUpdate**.  
  
 [!code-csharp[DataWorks SnapshotIsolation.DemoUpdate#1](../../../../../samples/snippets/csharp/VS_Snippets_ADO.NET/DataWorks SnapshotIsolation.DemoUpdate/CS/source.cs#1)]
 [!code-vb[DataWorks SnapshotIsolation.DemoUpdate#1](../../../../../samples/snippets/visualbasic/VS_Snippets_ADO.NET/DataWorks SnapshotIsolation.DemoUpdate/VB/source.vb#1)]  
  
### <a name="using-lock-hints-with-snapshot-isolation"></a>Usando dicas de bloqueio com isolamento de instantâneo  
 No exemplo anterior, a primeira transação seleciona dados e uma segunda transação atualiza os dados antes que a primeira transação possa ser concluída, causando um conflito de atualização quando a primeira transação tenta atualizar a mesma linha. Você pode reduzir a chance de conflitos de atualização em transações de instantâneo de execução longa pelo fornecimento de dicas de bloqueio no início da transação. A seguinte instrução SELECT usa a dica UPDLOCK para bloquear as linhas selecionadas:  
  
```sql  
SELECT * FROM TestSnapshotUpdate WITH (UPDLOCK)
  WHERE PriKey BETWEEN 1 AND 3  
```  
  
 O uso da dica de bloqueio UPDLOCK bloqueia as linhas que tentam atualizar as linhas antes da primeira transação ser concluída. Isso garante que as linhas selecionadas não tenham conflitos quando forem atualizadas posteriormente na transação. Confira "Dicas de bloqueio" nos Manuais Online do SQL Server.  
  
 Se o aplicativo tem muitos conflitos, o isolamento de instantâneo pode não ser a melhor opção. As dicas só devem ser usadas quando realmente necessário. Seu aplicativo não deve ser projetado para que ele dependa constantemente de dicas de bloqueio para sua operação.  
  
## <a name="see-also"></a>Veja também

- [SQL Server e ADO.NET](index.md)
- [Visão geral do ADO.NET](../ado-net-overview.md)
- [Guia de Controle de Versão de Linha e Bloqueio de Transações](/sql/relational-databases/sql-server-transaction-locking-and-row-versioning-guide)
