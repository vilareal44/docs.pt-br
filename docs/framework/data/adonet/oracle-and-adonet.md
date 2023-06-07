---
title: Oracle e ADO.NET
description: Saiba mais sobre os recursos e comportamentos do .NET Framework Provedor de Dados para Oracle, que fornece acesso a um banco de dados Oracle usando a interface de chamada Oracle.
titleSuffix: ''
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
ms.assetid: 8ee8e389-53cf-45cf-80bd-1df63ef34f2e
ms.openlocfilehash: 8757352a7444fad802ea88ba58e0fe643c86cbb8
ms.sourcegitcommit: 33deec3e814238fb18a49b2a7e89278e27888291
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/02/2020
ms.locfileid: "84286683"
---
# <a name="oracle-and-adonet"></a>Oracle e ADO.NET
> [!NOTE]
> Os tipos em <xref:System.Data.OracleClient> são preteridos. Os tipos permanecem com suporte na versão atual do .NET Framework, mas serão removidos em uma versão futura. A Microsoft recomenda que você use um provedor Oracle de terceiros.  
  
 Esta seção descreve os recursos e comportamentos específicos para o .NET Framework Provedor de Dados para Oracle.  
  
 O .NET Framework Provedor de Dados para Oracle fornece acesso a um banco de dados Oracle usando a interface de chamada Oracle (OCI), conforme fornecido pelo software cliente Oracle. A funcionalidade do provedor de dados foi projetada para ser semelhante à do .NET Framework provedores de dados para SQL Server, OLE DB e ODBC.  
  
 Para usar o .NET Framework Provedor de Dados para Oracle, um aplicativo deve fazer referência ao <xref:System.Data.OracleClient> namespace da seguinte maneira:  
  
```vb  
Imports System.Data.OracleClient  
```  
  
```csharp  
using System.Data.OracleClient;  
```  
  
 Você também deve incluir uma referência à DLL ao compilar seu código. Por exemplo, se você estiver criando um programa C#, sua linha de comando deverá incluir:  
  
```console
csc /r:System.Data.OracleClient.dll  
```  
  
## <a name="in-this-section"></a>Nesta seção  
 [Requisitos do sistema](system-requirements-for-the-dotnet-data-provider-for-oracle.md)  
 Descreve os requisitos para usar o .NET Framework Provedor de Dados para Oracle e descreve uma série de problemas que você deve saber ao usá-lo.  
  
 [Oracle BFILEs](oracle-bfiles.md)  
 Descreve a classe <xref:System.Data.OracleClient.OracleBFile>, que é usada no trabalho com o tipo de dados Oracle BFILE.  
  
 [LOBs do Oracle](oracle-lobs.md)  
 Descreve a classe <xref:System.Data.OracleClient.OracleLob>, que é usada para no trabalho com tipos de dados Oracle LOB.  
  
 [REF CURSORs do Oracle](oracle-ref-cursors.md)  
 Descreve o suporte para o tipo de dados REF CURSOR Oracle.  
  
 [OracleTypes](oracletypes.md)  
 Descreve estruturas que você pode usar para trabalhar com tipos de dados Oracle, incluindo <xref:System.Data.OracleClient.OracleNumber> e <xref:System.Data.OracleClient.OracleString>.  
  
 [Sequências da Oracle](oracle-sequences.md)  
 Descreve o suporte para recuperar os principais valores de sequência Oracle gerados pelo servidor.  
  
 [Mapeamentos de tipo de dados Oracle](oracle-data-type-mappings.md)  
 Lista tipos de dados Oracle e seus mapeamentos para <xref:System.Data.OracleClient.OracleDataReader>.  
  
 [Transações distribuídas do Oracle](oracle-distributed-transactions.md)  
 Descreve como o objeto <xref:System.Data.OracleClient.OracleConnection> automaticamente se inscreve em uma transação distribuída existente caso determine que uma transação está ativa.  
  
## <a name="related-sections"></a>Seções relacionadas  
 [Protegendo aplicativos ADO.NET](securing-ado-net-applications.md)  
 Descreve práticas seguras de codificação ao usar o ADO.NET.  
  
 [DataSets, DataTables e DataViews](./dataset-datatable-dataview/index.md)  
 Descreve como criar e usar `DataSets`, `DataSets` tipados, `DataTables` e `DataViews`.  
  
 [Retrieving and Modifying Data in ADO.NET](retrieving-and-modifying-data.md) (Recuperando e modificando dados no ADO.NET)  
 Descreve como trabalhar com dados no ADO.NET.  
  
 [SQL Server e ADO.NET](./sql/index.md)  
 Descreve como trabalhar com recursos e funcionalidades que são específicos ao SQL Server.  
  
 [DbProviderFactories](dbproviderfactories.md)  
 Descreve classes genéricas que permitem que você grave código independente de provedor em ADO.NET.  
  
## <a name="see-also"></a>Veja também

- [ADO.NET](index.md)
- [Visão geral do ADO.NET](ado-net-overview.md)
