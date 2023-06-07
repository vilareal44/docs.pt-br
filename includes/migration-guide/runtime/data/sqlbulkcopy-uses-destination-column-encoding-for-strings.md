---
ms.openlocfilehash: 1329a86db4227f75dfba7c50bbbdc2fc23099528
ms.sourcegitcommit: e02d17b2cf9c1258dadda4810a5e6072a0089aee
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/01/2020
ms.locfileid: "85619790"
---
### <a name="sqlbulkcopy-uses-destination-column-encoding-for-strings"></a>SqlBulkCopy usa a codificação de coluna de destino para cadeias de caracteres

#### <a name="details"></a>Detalhes

Quando dados são inseridos em uma coluna, o <xref:System.Data.SqlClient.SqlBulkCopy?displayProperty=fullName> usa a codificação da coluna de destino em vez da codificação padrão para os tipos <code>VARCHAR</code> e <code>CHAR</code>. Essa alteração elimina a possibilidade de corrompimento de dados causada pelo uso da codificação padrão quando a coluna de destino não usa a codificação padrão. Em casos raros, um aplicativo existente pode gerar uma exceção SqlException quando a mudança na codificação produz dados que são grandes demais para caber na coluna de destino.

#### <a name="suggestion"></a>Sugestão

Espere que <xref:System.Data.SqlClient.SqlBulkCopy?displayProperty=fullName> não corrompa os dados devido a diferenças de codificação. Se as cadeias de caracteres perto do limite de tamanho da coluna de destino estiverem sendo copiadas, talvez seja necessário codificar previamente os dados (a serem copiados para verificar se os dados serão ajustados na coluna de destino) ou capturar <xref:System.Data.SqlClient.SqlException?displayProperty=fullName>s.

| Name    | Valor       |
|:--------|:------------|
| Escopo   |Microsoft Edge|
|Versão|4.5|
|Type|Runtime

#### <a name="affected-apis"></a>APIs afetadas

-<xref:System.Data.SqlClient.SqlBulkCopy?displayProperty=nameWithType></li><li><xref:System.Data.SqlClient.SqlBulkCopy.%23ctor(System.Data.SqlClient.SqlConnection)></li></ul>|
