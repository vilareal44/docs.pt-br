---
title: Como inserir linhas no banco de dados
description: Aprenda a inserir linhas em um banco de dados adicionando LINQ to SQL objetos a uma coleção relacionada à tabela. LINQ to SQL traduz as adições aos comandos SQL INSERT.
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
ms.assetid: 44d99680-69c7-4879-a732-f6771b334211
ms.openlocfilehash: 39eee6edf59d2adb7de41efd88899050fbe69fd8
ms.sourcegitcommit: 33deec3e814238fb18a49b2a7e89278e27888291
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/02/2020
ms.locfileid: "84286346"
---
# <a name="how-to-insert-rows-into-the-database"></a>Como inserir linhas no banco de dados

Você insere linhas em um banco de dados adicionando objetos à [!INCLUDE[vbtecdlinq](../../../../../../includes/vbtecdlinq-md.md)] <xref:System.Data.Linq.Table%601> coleção associada e, em seguida, enviando as alterações ao banco de dados. [!INCLUDE[vbtecdlinq](../../../../../../includes/vbtecdlinq-md.md)]traduz as alterações nos comandos SQL apropriados `INSERT` .

> [!NOTE]
> Você pode substituir os métodos padrão do [!INCLUDE[vbtecdlinq](../../../../../../includes/vbtecdlinq-md.md)] para as operações de banco de dados `Insert`, `Update` e `Delete`. Para obter mais informações, consulte [Personalizando as operações de inserção, atualização e exclusão](customizing-insert-update-and-delete-operations.md).
>
> Os desenvolvedores que usam o Visual Studio podem usar o Object Relational Designer para desenvolver procedimentos armazenados para a mesma finalidade.

As etapas a seguir presumem que um <xref:System.Data.Linq.DataContext> válido conecta você ao banco de dados Northwind. Para obter mais informações, consulte [como conectar-se a um banco de dados](how-to-connect-to-a-database.md).

### <a name="to-insert-a-row-into-the-database"></a>Para inserir uma linha no banco de dados

1. Crie um novo objeto que inclui os dados da coluna a serem enviados.

2. Adicione o novo objeto à [!INCLUDE[vbtecdlinq](../../../../../../includes/vbtecdlinq-md.md)] `Table` coleção associada à tabela de destino no banco de dados.

3. Envie a alteração para o banco de dados.

## <a name="example"></a>Exemplo

O exemplo de código a seguir cria um novo objeto do tipo `Order` e preenche-o com valores apropriados. Ele em seguida adiciona o novo objeto à coleção `Order`. Finalmente, ele envia a alteração para o banco de dados como uma nova linha na tabela `Orders`.

[!code-csharp[System.Data.Linq.Table#1](../../../../../../samples/snippets/csharp/VS_Snippets_Data/system.data.linq.table/cs/program.cs#1)]
[!code-vb[System.Data.Linq.Table#1](../../../../../../samples/snippets/visualbasic/VS_Snippets_Data/system.data.linq.table/vb/module1.vb#1)]

## <a name="see-also"></a>Veja também

- [Como gerenciar conflitos de alteração](how-to-manage-change-conflicts.md)
- [Métodos DataContext (O/R Designer)](/visualstudio/data-tools/datacontext-methods-o-r-designer)
- [Como atribuir procedimentos armazenados para executar atualizações, inserções e exclusões (Designer Relacional de Objetos)](/visualstudio/data-tools/how-to-assign-stored-procedures-to-perform-updates-inserts-and-deletes-o-r-designer)
- [Fazendo e enviando alterações de dados](making-and-submitting-data-changes.md)
