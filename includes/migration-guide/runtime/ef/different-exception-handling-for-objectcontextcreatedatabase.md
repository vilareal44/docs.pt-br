---
ms.openlocfilehash: 687118157020ede200f97a0125c4740a06bf4b5e
ms.sourcegitcommit: e02d17b2cf9c1258dadda4810a5e6072a0089aee
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/01/2020
ms.locfileid: "85619795"
---
### <a name="different-exception-handling-for-objectcontextcreatedatabase-and-dbproviderservicescreatedatabase-methods"></a>Tratamento de exceção diferente para os métodos ObjectContext.CreateDatabase e DbProviderServices.CreateDatabase

#### <a name="details"></a>Detalhes

Começando com o .NET Framework 4.5, se houver falha na criação do banco de dados, os métodos <code>CreateDatabase</code> tentarão remover o banco de dados vazio. Se essa operação for bem-sucedida, a <xref:System.Data.SqlClient.SqlException?displayProperty=fullName> original será propagada (em vez da <xref:System.InvalidOperationException?displayProperty=fullName> que sempre era gerada no .NET Framework 4.0)

#### <a name="suggestion"></a>Sugestão

Ao capturar um <xref:System.InvalidOperationException?displayProperty=fullName> durante a execução de <xref:System.Data.Objects.ObjectContext.CreateDatabase> ou <xref:System.Data.Common.DbProviderServices.CreateDatabase(System.Data.Common.DbConnection,System.Nullable{System.Int32},System.Data.Metadata.Edm.StoreItemCollection)>, agora, SQLExceptions também devem ser capturadas.

| Name    | Valor       |
|:--------|:------------|
| Escopo   |Secundária|
|Versão|4.5|
|Type|Runtime

#### <a name="affected-apis"></a>APIs afetadas

-<xref:System.Data.Objects.ObjectContext.CreateDatabase?displayProperty=nameWithType></li><li><xref:System.Data.Common.DbProviderServices.CreateDatabase(System.Data.Common.DbConnection,System.Nullable{System.Int32},System.Data.Metadata.Edm.StoreItemCollection)?displayProperty=nameWithType></li></ul>|
