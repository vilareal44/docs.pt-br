---
title: 'Passo a passo: usando BatchBlock e BatchedJoinBlock para melhorar a eficiência'
ms.date: 03/30/2017
ms.technology: dotnet-standard
dev_langs:
- csharp
- vb
helpviewer_keywords:
- Task Parallel Library, dataflows
- TPL dataflow library, improving efficiency
ms.assetid: 5beb4983-80c2-4f60-8c51-a07f9fd94cb3
ms.openlocfilehash: e572c5a14958ccc069ae7649af8c8ed4eb967dc1
ms.sourcegitcommit: 33deec3e814238fb18a49b2a7e89278e27888291
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/02/2020
ms.locfileid: "84284579"
---
# <a name="walkthrough-using-batchblock-and-batchedjoinblock-to-improve-efficiency"></a>Passo a passo: usando BatchBlock e BatchedJoinBlock para melhorar a eficiência

A Biblioteca de Fluxo de dados TPL fornece as classes <xref:System.Threading.Tasks.Dataflow.BatchBlock%601?displayProperty=nameWithType> e <xref:System.Threading.Tasks.Dataflow.BatchedJoinBlock%602?displayProperty=nameWithType> para que você possa receber e armazenar em buffer os dados de uma ou mais fontes e, depois, propagar esses dados armazenados em buffer como uma coleção. Este mecanismo de envio em lote é útil quando você coleta dados de uma ou mais fontes e, em seguida, processa vários elementos de dados como um lote. Por exemplo, considere um aplicativo que usa o fluxo de dados para inserir registros em um banco de dados. Essa operação pode ser mais eficiente se vários itens forem inseridos ao mesmo tempo, em vez de um de cada vez sequencialmente. Este documento descreve como usar a classe <xref:System.Threading.Tasks.Dataflow.BatchBlock%601> para melhorar a eficiência dessas operações de inserção de banco de dados. Também descreve como usar a classe <xref:System.Threading.Tasks.Dataflow.BatchedJoinBlock%602> para capturar os resultados e todas as exceções que ocorrem quando o programa lê de um banco de dados.

[!INCLUDE [tpl-install-instructions](../../../includes/tpl-install-instructions.md)]

## <a name="prerequisites"></a>Pré-requisitos

1. Leia a seção Blocos de ingresso no documento [Fluxo de dados](dataflow-task-parallel-library.md) antes de começar este passo a passo.

2. Certifique-se de que você tenha uma cópia do banco de dados Northwind, Northwind.sdf, disponível em seu computador. Normalmente, esse arquivo está localizado na pasta %Arquivos de Programas%\Microsoft SQL Server Compact Edition\v3.5\Samples\\.

    > [!IMPORTANT]
    > Em algumas versões do Windows, não é possível se conectar ao Northwind.sdf quando o Visual Studio está em execução em um modo não administrador. Para conectar-se ao Northwind.sdf, inicie o Visual Studio ou um Prompt de Comando do Desenvolvedor para Visual Studio no modo **Executar como administrador**.

Este passo a passo contém as seguintes seções:

- [Criando o Aplicativo de Console](#creating)

- [Definir a classe do funcionário](#employeeClass)

- [Definir as operações de banco de dados do funcionário](#operations)

- [Adicionando dados de funcionários ao banco de dado sem usar o armazenamento em buffer](#nonBuffering)

- [Usar o armazenamento em buffer para adicionar dados de funcionário ao banco de dados](#buffering)

- [Usar o ingresso em buffer para ler dados de funcionário do banco de dados](#bufferedJoin)

- [O exemplo completo](#complete)

<a name="creating"></a>

## <a name="creating-the-console-application"></a>Criando o Aplicativo de Console

1. No Visual Studio, crie um projeto de aplicativo de **console** do Visual C# ou Visual Basic. Neste documento, o projeto é chamado `DataflowBatchDatabase`.

2. Em seu projeto, adicione uma referência ao System.Data.SqlServerCe.dll e uma referência a System.Threading.Tasks.Dataflow.dll.

3. Verifique se o Form1.cs (Form1.vb para Visual Basic) contém as seguintes instruções `using` (`Imports` em Visual Basic).

    [!code-csharp[TPLDataflow_BatchDatabase#1](../../../samples/snippets/csharp/VS_Snippets_Misc/tpldataflow_batchdatabase/cs/dataflowbatchdatabase.cs#1)]
    [!code-vb[TPLDataflow_BatchDatabase#1](../../../samples/snippets/visualbasic/VS_Snippets_Misc/tpldataflow_batchdatabase/vb/dataflowbatchdatabase.vb#1)]

4. Adicione os membros de dados a seguir à classe `Program`.

    [!code-csharp[TPLDataflow_BatchDatabase#2](../../../samples/snippets/csharp/VS_Snippets_Misc/tpldataflow_batchdatabase/cs/dataflowbatchdatabase.cs#2)]
    [!code-vb[TPLDataflow_BatchDatabase#2](../../../samples/snippets/visualbasic/VS_Snippets_Misc/tpldataflow_batchdatabase/vb/dataflowbatchdatabase.vb#2)]

<a name="employeeClass"></a>

## <a name="defining-the-employee-class"></a>Definir a classe do funcionário

Adicione a classe `Program` à classe `Employee`.

[!code-csharp[TPLDataflow_BatchDatabase#3](../../../samples/snippets/csharp/VS_Snippets_Misc/tpldataflow_batchdatabase/cs/dataflowbatchdatabase.cs#3)]
[!code-vb[TPLDataflow_BatchDatabase#3](../../../samples/snippets/visualbasic/VS_Snippets_Misc/tpldataflow_batchdatabase/vb/dataflowbatchdatabase.vb#3)]

A classe `Employee` contém três propriedades: `EmployeeID`, `LastName` e `FirstName`. Essas propriedades correspondem às colunas `Employee ID`, `Last Name` e `First Name` na tabela `Employees` no banco de dados Northwind. Para esta demonstração, a classe `Employee` também define o método `Random`, que cria um objeto `Employee` com valores aleatórios para suas propriedades.

<a name="operations"></a>

## <a name="defining-employee-database-operations"></a>Definir as operações de banco de dados do funcionário

Adicione à classe `Program` os métodos `InsertEmployees`, `GetEmployeeCount` e `GetEmployeeID`.

[!code-csharp[TPLDataflow_BatchDatabase#4](../../../samples/snippets/csharp/VS_Snippets_Misc/tpldataflow_batchdatabase/cs/dataflowbatchdatabase.cs#4)]
[!code-vb[TPLDataflow_BatchDatabase#4](../../../samples/snippets/visualbasic/VS_Snippets_Misc/tpldataflow_batchdatabase/vb/dataflowbatchdatabase.vb#4)]

O método `InsertEmployees` adiciona novos registros de funcionário ao banco de dados. O método `GetEmployeeCount` recupera o número de entradas na tabela `Employees`. O método `GetEmployeeID` recupera o identificador do primeiro funcionário que tem o nome fornecido. Cada um desses métodos usa uma cadeia de caracteres de conexão para o banco de dados Northwind e usa a funcionalidade no namespace `System.Data.SqlServerCe` para se comunicar com o banco de dados.

<a name="nonBuffering"></a>

## <a name="adding-employee-data-to-the-database-without-using-buffering"></a>Adicionar dados de funcionário ao banco de dados sem o uso de buffer

Adicione à classe `Program` os métodos `AddEmployees` e `PostRandomEmployees`.

[!code-csharp[TPLDataflow_BatchDatabase#5](../../../samples/snippets/csharp/VS_Snippets_Misc/tpldataflow_batchdatabase/cs/dataflowbatchdatabase.cs#5)]
[!code-vb[TPLDataflow_BatchDatabase#5](../../../samples/snippets/visualbasic/VS_Snippets_Misc/tpldataflow_batchdatabase/vb/dataflowbatchdatabase.vb#5)]

O método `AddEmployees` adiciona dados aleatórios de funcionários ao banco de dados usando o fluxo de dados. Ele cria um objeto <xref:System.Threading.Tasks.Dataflow.ActionBlock%601> que chama o método `InsertEmployees` para adicionar uma entrada de funcionário ao banco de dados. Em seguida, o método `AddEmployees` chama o método `PostRandomEmployees` para postar vários objetos `Employee` para o objeto <xref:System.Threading.Tasks.Dataflow.ActionBlock%601>. Depois, o método `AddEmployees` espera a conclusão de todas as operações de inserção.

<a name="buffering"></a>

## <a name="using-buffering-to-add-employee-data-to-the-database"></a>Usar o armazenamento em buffer para adicionar dados de funcionário ao banco de dados

Adicione à classe `Program` o método `AddEmployeesBatched`.

[!code-csharp[TPLDataflow_BatchDatabase#6](../../../samples/snippets/csharp/VS_Snippets_Misc/tpldataflow_batchdatabase/cs/dataflowbatchdatabase.cs#6)]
[!code-vb[TPLDataflow_BatchDatabase#6](../../../samples/snippets/visualbasic/VS_Snippets_Misc/tpldataflow_batchdatabase/vb/dataflowbatchdatabase.vb#6)]

Esse método é semelhante a `AddEmployees`, exceto que ele também usa a classe <xref:System.Threading.Tasks.Dataflow.BatchBlock%601> para armazenar em buffer vários objetos `Employee` antes de enviar esses objetos ao objeto <xref:System.Threading.Tasks.Dataflow.ActionBlock%601>. Como a classe <xref:System.Threading.Tasks.Dataflow.BatchBlock%601> propaga vários elementos como uma coleção, o objeto <xref:System.Threading.Tasks.Dataflow.ActionBlock%601> é modificado a fim de funcionar em uma matriz de objetos `Employee`. Assim como no método `AddEmployees`, `AddEmployeesBatched` chama o método `PostRandomEmployees` para postar vários objetos `Employee`; no entanto, `AddEmployeesBatched`posta esses objetos no objeto <xref:System.Threading.Tasks.Dataflow.BatchBlock%601>. O método `AddEmployeesBatched` também espera a conclusão de todas as operações de inserção.

<a name="bufferedJoin"></a>

## <a name="using-buffered-join-to-read-employee-data-from-the-database"></a>Usar o ingresso em buffer para ler dados de funcionário do banco de dados

Adicione à classe `Program` o método `GetRandomEmployees`.

[!code-csharp[TPLDataflow_BatchDatabase#7](../../../samples/snippets/csharp/VS_Snippets_Misc/tpldataflow_batchdatabase/cs/dataflowbatchdatabase.cs#7)]
[!code-vb[TPLDataflow_BatchDatabase#7](../../../samples/snippets/visualbasic/VS_Snippets_Misc/tpldataflow_batchdatabase/vb/dataflowbatchdatabase.vb#7)]

Esse método imprime as informações sobre funcionários aleatórios no console. Ele cria vários objetos `Employee` aleatórios e chama o método `GetEmployeeID` para recuperar o identificador exclusivo de cada objeto. Como o método `GetEmployeeID` lança uma exceção se nenhum funcionário corresponder aos nomes e sobrenomes indicados, o método `GetRandomEmployees` usa a classe <xref:System.Threading.Tasks.Dataflow.BatchedJoinBlock%602> para armazenar objetos `Employee` para chamada bem-sucedida a `GetEmployeeID`, e objetos <xref:System.Exception?displayProperty=nameWithType> para chamadas com falha. O objeto <xref:System.Threading.Tasks.Dataflow.ActionBlock%601> neste exemplo age em um objeto <xref:System.Tuple%602> que contém uma lista de objetos `Employee` e uma lista de objetos <xref:System.Exception>. O objeto <xref:System.Threading.Tasks.Dataflow.BatchedJoinBlock%602> propaga esse dados quando a soma dos objetos `Employee` e <xref:System.Exception> recebidos for igual ao tamanho do lote.

<a name="complete"></a>

## <a name="the-complete-example"></a>O Exemplo Completo

O exemplo a seguir mostra todo o código. O método `Main` compara o tempo necessário para executar inserções em lotes do banco de dados em comparação com o tempo para executar as inserções do banco de dados sem lote. Ele também demonstra o uso do ingresso em buffer para ler dados de funcionário do banco de dados e também informar erros.

[!code-csharp[TPLDataflow_BatchDatabase#100](../../../samples/snippets/csharp/VS_Snippets_Misc/tpldataflow_batchdatabase/cs/dataflowbatchdatabase.cs#100)]
[!code-vb[TPLDataflow_BatchDatabase#100](../../../samples/snippets/visualbasic/VS_Snippets_Misc/tpldataflow_batchdatabase/vb/dataflowbatchdatabase.vb#100)]

## <a name="see-also"></a>Veja também

- [Fluxo de dados](dataflow-task-parallel-library.md)
