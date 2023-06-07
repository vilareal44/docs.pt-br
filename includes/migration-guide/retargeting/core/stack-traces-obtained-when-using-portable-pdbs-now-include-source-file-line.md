---
ms.openlocfilehash: c7500550cd9714a9788a7dea68af04823f000f7f
ms.sourcegitcommit: e02d17b2cf9c1258dadda4810a5e6072a0089aee
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/01/2020
ms.locfileid: "85614296"
---
### <a name="stack-traces-obtained-when-using-portable-pdbs-now-include-source-file-and-line-information-if-requested"></a>Os rastreamentos de pilha obtidos ao usar PDBs portáteis agora incluem informações de arquivo de origem e de linha, quando solicitadas

#### <a name="details"></a>Detalhes

Começando com o .NET Framework 4.7.2, os rastreamentos de pilha obtidos ao usar PDBs portáteis incluem informações de arquivo de origem e de linha, quando solicitadas. Nas versões anteriores do .NET Framework 4.7.2, as informações de arquivo de origem e de linha não estariam disponíveis ao usar PDBs portáteis, mesmo quando solicitadas explicitamente.

#### <a name="suggestion"></a>Sugestão

Para os aplicativos direcionados ao .NET Framework 4.7.2, você pode recusar as informações de arquivo de origem e de linha ao usar PDBs portáteis, caso não as deseje, adicionando o seguinte à seção `<runtime>` do arquivo `app.config`:

```xml
<runtime>
  <AppContextSwitchOverrides value="Switch.System.Diagnostics.IgnorePortablePDBsInStackTraces=true" />
</runtime>
```

Para aplicativos direcionados a versões anteriores do .NET Framework mas executados no .NET Framework 4.7.2 ou posterior, você pode aceitar as informações de arquivo de origem e de linha ao usar PDBs portáteis adicionando o seguinte à seção `<runtime>` do arquivo `app.config`:

```xml
<runtime>
  <AppContextSwitchOverrides value="Switch.System.Diagnostics.IgnorePortablePDBsInStackTraces=false" />
</runtime>
```

| Name    | Valor       |
|:--------|:------------|
| Escopo   | Microsoft Edge        |
| Versão | 4.7.2       |
| Type    | Redirecionando |

#### <a name="affected-apis"></a>APIs afetadas

- <xref:System.Diagnostics.StackTrace.%23ctor(System.Boolean)>
- <xref:System.Diagnostics.StackTrace.%23ctor(System.Exception,System.Boolean)>
- <xref:System.Diagnostics.StackTrace.%23ctor(System.Exception,System.Int32,System.Boolean)>
