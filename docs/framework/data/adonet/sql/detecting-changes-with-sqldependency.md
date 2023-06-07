---
title: Detectando alterações com SqlDependency
description: Associe um objeto SqlDependency a um SqlCommand para detectar quando os resultados da consulta são diferentes daqueles recuperados originalmente em ADO.NET.
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
ms.assetid: e6a58316-f005-4477-92e1-45cc2eb8c5b4
ms.openlocfilehash: b196d42477e1778c45df64b1390502645fdd649d
ms.sourcegitcommit: 33deec3e814238fb18a49b2a7e89278e27888291
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/02/2020
ms.locfileid: "84286462"
---
# <a name="detecting-changes-with-sqldependency"></a>Detectando alterações com SqlDependency

Um objeto <xref:System.Data.SqlClient.SqlDependency> pode ser associado a um <xref:System.Data.SqlClient.SqlCommand> para detectar quando os resultados da consulta são diferentes dos recuperados originalmente. Você também pode atribuir um delegado ao evento `OnChange`, que será acionado quando os resultados forem alterados para um comando associado. Antes de executar o comando, você deve associar o <xref:System.Data.SqlClient.SqlDependency> a ele. A propriedade `HasChanges` do <xref:System.Data.SqlClient.SqlDependency> também pode ser usada para determinar se os resultados da consulta foram alterados desde que os dados foram recuperados pela primeira vez.

## <a name="security-considerations"></a>Considerações de segurança

A infraestrutura de dependência depende de um <xref:System.Data.SqlClient.SqlConnection> que é aberto quando <xref:System.Data.SqlClient.SqlDependency.Start%2A> é chamado para receber notificações de que os dados subjacentes foram alterados para um determinado comando. A capacidade de um cliente iniciar a chamada para `SqlDependency.Start` é controlada por meio do uso de <xref:System.Data.SqlClient.SqlClientPermission> e de atributos de segurança de acesso do código. Para obter mais informações, consulte [habilitando notificações de consulta](enabling-query-notifications.md) e [segurança de acesso de código e ADO.net](../code-access-security.md).

### <a name="example"></a>Exemplo

As seguintes etapas ilustram como declarar uma dependência, executar um comando e receber uma notificação quando o conjunto de resultados for alterado:

1. Inicia uma conexão `SqlDependency` com o servidor.

2. Crie objetos <xref:System.Data.SqlClient.SqlConnection> e <xref:System.Data.SqlClient.SqlCommand> para se conectar ao servidor e definir uma instrução Transact-SQL.

3. Crie um objeto `SqlDependency` ou use um existente e associe-o ao objeto `SqlCommand`. Internamente, isso cria um objeto <xref:System.Data.Sql.SqlNotificationRequest> e o associa ao objeto de comando, conforme necessário. Essa solicitação de notificação contém um identificador interno que identifica exclusivamente esse objeto `SqlDependency`. Ele também iniciará o ouvinte de cliente se ainda não estiver ativo.

4. Assine um manipulador de eventos para o evento `OnChange` do objeto `SqlDependency`.

5. Execute o comando usando qualquer um dos métodos `Execute` do objeto `SqlCommand`. Já que o comando está associado ao objeto de notificação, o servidor reconhece que ele precisa gerar uma notificação e as informações da fila apontarão para a fila de dependências.

6. Interrompa a conexão de `SqlDependency` com o servidor.

Se qualquer usuário alterar posteriormente os dados subjacentes, o Microsoft SQL Server detectará a existência de uma notificação pendente para tal alteração e postará uma notificação que será processada e encaminhada ao cliente por meio do `SqlConnection` subjacente, criado por uma chamada a `SqlDependency.Start`. O ouvinte de cliente recebe a mensagem de invalidação. Em seguida, o ouvinte de cliente localiza o objeto `SqlDependency` associado e aciona o evento `OnChange`.

O fragmento de código a seguir mostra o padrão de design que você usaria para criar um aplicativo de exemplo.

```vb
Sub Initialization()
    ' Create a dependency connection.
    SqlDependency.Start(connectionString, queueName)
End Sub

Sub SomeMethod()
    ' Assume connection is an open SqlConnection.
    ' Create a new SqlCommand object.
    Using command As New SqlCommand( _
      "SELECT ShipperID, CompanyName, Phone FROM dbo.Shippers", _
      connection)

        ' Create a dependency and associate it with the SqlCommand.
        Dim dependency As New SqlDependency(command)
        ' Maintain the refernce in a class member.
        ' Subscribe to the SqlDependency event.
        AddHandler dependency.OnChange, AddressOf OnDependencyChange

        ' Execute the command.
        Using reader = command.ExecuteReader()
            ' Process the DataReader.
        End Using
    End Using
End Sub

' Handler method
Sub OnDependencyChange(ByVal sender As Object, _
    ByVal e As SqlNotificationEventArgs)
    ' Handle the event (for example, invalidate this cache entry).
End Sub

Sub Termination()
    ' Release the dependency
    SqlDependency.Stop(connectionString, queueName)
End Sub
```

```csharp
void Initialization()
{
    // Create a dependency connection.
    SqlDependency.Start(connectionString, queueName);
}

void SomeMethod()
{
    // Assume connection is an open SqlConnection.

    // Create a new SqlCommand object.
    using (SqlCommand command=new SqlCommand(
        "SELECT ShipperID, CompanyName, Phone FROM dbo.Shippers",
        connection))
    {

        // Create a dependency and associate it with the SqlCommand.
        SqlDependency dependency=new SqlDependency(command);
        // Maintain the reference in a class member.

        // Subscribe to the SqlDependency event.
        dependency.OnChange+=new
           OnChangeEventHandler(OnDependencyChange);

        // Execute the command.
        using (SqlDataReader reader = command.ExecuteReader())
        {
            // Process the DataReader.
        }
    }
}

// Handler method
void OnDependencyChange(object sender,
   SqlNotificationEventArgs e )
{
  // Handle the event (for example, invalidate this cache entry).
}

void Termination()
{
    // Release the dependency.
    SqlDependency.Stop(connectionString, queueName);
}
```

## <a name="see-also"></a>Veja também

- [Notificações de consulta no SQL Server](query-notifications-in-sql-server.md)
- [Visão geral do ADO.NET](../ado-net-overview.md)
