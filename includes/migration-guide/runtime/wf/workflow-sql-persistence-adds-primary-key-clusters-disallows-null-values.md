---
ms.openlocfilehash: 809ca85b347fabc44573e2e0c5a43261d68590d3
ms.sourcegitcommit: e02d17b2cf9c1258dadda4810a5e6072a0089aee
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/01/2020
ms.locfileid: "85621023"
---
### <a name="workflow-sql-persistence-adds-primary-key-clusters-and-disallows-null-values-in-some-columns"></a>Persistência do fluxo de trabalho SQL adiciona clusters de chave primária e não permite valores nulos em algumas colunas

#### <a name="details"></a>Detalhes

A partir do .NET Framework 4.7, as tabelas criadas para a SWIS (SQL Workflow Instance Store) pelo script SqlWorkflowInstanceStoreSchema.sql usarão chaves primárias em clusters. Por isso, as identidades não são compatíveis com valores <code>null</code>. A operação da SWIS não é afetada por essa alteração. As atualizações foram feitas serem compatíveis com replicação transacional do SQL Server.

#### <a name="suggestion"></a>Sugestão

O arquivo SQL SqlWorkflowInstanceStoreSchemaUpgrade.sql deve ser aplicado a instalações existentes para usar essa alteração. Novas instalações de banco de dados serão alteradas automaticamente.

| Name    | Valor       |
|:--------|:------------|
| Escopo   |Microsoft Edge|
|Versão|4.7|
|Type|Runtime|
