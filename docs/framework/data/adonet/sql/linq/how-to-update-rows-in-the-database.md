---
title: Como atualizar linhas no banco de dados
description: Saiba como atualizar linhas em um banco de dados modificando LINQ to SQL objetos em uma coleção relacionada à tabela. LINQ to SQL traduz as adições aos comandos SQL UPDATE.
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
ms.assetid: a2b5c90f-6cc3-4128-bfab-1db488d5af26
ms.openlocfilehash: f25efb91fb5a83fb1c7c109bd018c8210edaec8b
ms.sourcegitcommit: 33deec3e814238fb18a49b2a7e89278e27888291
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/02/2020
ms.locfileid: "84286333"
---
# <a name="how-to-update-rows-in-the-database"></a>Como atualizar linhas no banco de dados

Você pode atualizar linhas em um banco de dados modificando valores de membro dos objetos associados à [!INCLUDE[vbtecdlinq](../../../../../../includes/vbtecdlinq-md.md)] <xref:System.Data.Linq.Table%601> coleção e, em seguida, enviando as alterações para o banco de dados. [!INCLUDE[vbtecdlinq](../../../../../../includes/vbtecdlinq-md.md)]traduz as alterações nos comandos SQL apropriados `UPDATE` .

> [!NOTE]
> Você pode substituir os métodos padrão do [!INCLUDE[vbtecdlinq](../../../../../../includes/vbtecdlinq-md.md)] para as operações de banco de dados `Insert`, `Update` e `Delete`. Para obter mais informações, consulte [Personalizando as operações de inserção, atualização e exclusão](customizing-insert-update-and-delete-operations.md).
>
> Os desenvolvedores que usam o Visual Studio podem usar o Object Relational Designer para desenvolver procedimentos armazenados para a mesma finalidade.

As etapas a seguir presumem que um <xref:System.Data.Linq.DataContext> válido conecta você ao banco de dados Northwind. Para obter mais informações, consulte [como conectar-se a um banco de dados](how-to-connect-to-a-database.md).

### <a name="to-update-a-row-in-the-database"></a>Para atualizar uma linha no banco de dados

1. Consulte a linha ser atualizada no banco de dados.

2. Faça as alterações desejadas nos valores dos membros no objeto [!INCLUDE[vbtecdlinq](../../../../../../includes/vbtecdlinq-md.md)] resultante.

3. Envie as alterações ao banco de dados.

## <a name="example"></a>Exemplo

O exemplo a seguir consulta a ordem #11000 no banco de dados e modifica os valores de `ShipName` e de `ShipVia` no objeto `Order` resultante. Finalmente, as alterações nesses valores membro são enviadas ao banco de dados como alterações nas colunas `ShipName` e `ShipVia`.

[!code-csharp[System.Data.Linq.Table#2](../../../../../../samples/snippets/csharp/VS_Snippets_Data/system.data.linq.table/cs/program.cs#2)]
[!code-vb[System.Data.Linq.Table#2](../../../../../../samples/snippets/visualbasic/VS_Snippets_Data/system.data.linq.table/vb/module1.vb#2)]

## <a name="see-also"></a>Veja também

- [Como gerenciar conflitos de alteração](how-to-manage-change-conflicts.md)
- [Como atribuir procedimentos armazenados para executar atualizações, inserções e exclusões (Designer Relacional de Objetos)](/visualstudio/data-tools/how-to-assign-stored-procedures-to-perform-updates-inserts-and-deletes-o-r-designer)
- [Fazendo e enviando alterações de dados](making-and-submitting-data-changes.md)
