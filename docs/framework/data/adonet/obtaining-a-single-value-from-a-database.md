---
title: Obter um único valor de um banco de dados
description: Saiba como retornar um único valor em ADO.NET. Este exemplo de código retorna o valor da coluna de identidade para um registro inserido.
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
ms.assetid: b38526cd-a62a-48cb-822a-e91dfa68e02d
ms.openlocfilehash: a6f268f72f8b8a09ae48ba3cad6254323cb95a20
ms.sourcegitcommit: 33deec3e814238fb18a49b2a7e89278e27888291
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/02/2020
ms.locfileid: "84286696"
---
# <a name="obtaining-a-single-value-from-a-database"></a>Obter um único valor de um banco de dados
Talvez seja necessário retornar informações do banco de dados que seja simplesmente um único valor em vez de na forma de um fluxo de tabela ou data. Por exemplo, talvez você queira retornar o resultado de uma função de agregação, como COUNT ( \* ), Sum (Price) ou AVG (quantity). O objeto **Command** fornece a capacidade de retornar valores únicos usando o método **ExecuteScalar** . O método **ExecuteScalar** retorna, como um valor escalar, o valor da primeira coluna da primeira linha do conjunto de resultados.  
  
 O exemplo de código a seguir insere um novo valor no banco de dados usando um <xref:System.Data.SqlClient.SqlCommand> . O <xref:System.Data.SqlClient.SqlCommand.ExecuteScalar%2A> método é usado para retornar o valor da coluna de identidade para o registro inserido.  
  
 [!code-csharp[DataWorks SqlCommand.ExecuteScalar#1](../../../../samples/snippets/csharp/VS_Snippets_ADO.NET/DataWorks SqlCommand.ExecuteScalar/CS/source.cs#1)]
 [!code-vb[DataWorks SqlCommand.ExecuteScalar#1](../../../../samples/snippets/visualbasic/VS_Snippets_ADO.NET/DataWorks SqlCommand.ExecuteScalar/VB/source.vb#1)]  
  
## <a name="see-also"></a>Veja também

- [Comandos e parâmetros](commands-and-parameters.md)
- [Executando um comando](executing-a-command.md)
- [DbConnection, DbCommand e DbException](dbconnection-dbcommand-and-dbexception.md)
- [Visão geral do ADO.NET](ado-net-overview.md)
