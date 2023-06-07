---
ms.openlocfilehash: 67e3ff5000ebd38064ed8a57e4fe561afa31f8d8
ms.sourcegitcommit: e02d17b2cf9c1258dadda4810a5e6072a0089aee
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/01/2020
ms.locfileid: "85614282"
---
### <a name="long-path-support"></a>Suporte a caminho longo

#### <a name="details"></a>Detalhes

A partir dos aplicativos destinados ao .NET Framework 4.6.2, caminhos longos (de até 32K caracteres) têm suporte e a limitação de 260 caracteres (ou `MAX_PATH`) para o tamanho dos caminhos foi removida. Para aplicativos recompilados para serem destinados ao .NET Framework 4.6.2, os caminhos de código que lançavam um <xref:System.IO.PathTooLongException?displayProperty=fullName> porque um caminho ultrapassava 260 caracteres agora lançam um <xref:System.IO.PathTooLongException?displayProperty=fullName> apenas sob as seguintes condições:

- O tamanho do caminho é superior a <xref:System.Int16.MaxValue> (32.767) caracteres.
- O sistema operacional retorna `COR_E_PATHTOOLONG` ou seu equivalente.
Para aplicativos destinados ao .NET Framework 4.6.1 e versões anteriores, a runtime gera automaticamente um <xref:System.IO.PathTooLongException?displayProperty=fullName> sempre que um caminho ultrapassa 260 caracteres.

#### <a name="suggestion"></a>Sugestão

Para aplicativos destinados ao .NET Framework 4.6.2, é possível recusar o suporte para caminhos longos se ele não for desejável adicionando o seguinte à seção `<runtime>` do arquivo `app.config`:

```xml
<runtime>
  <AppContextSwitchOverrides value="Switch.System.IO.BlockLongPaths=true" />
</runtime>
```

Para aplicativos destinados a versões anteriores do .NET Framework mas executados no .NET Framework 4.6.2 ou posterior, é possível aceitar o suporte para caminhos longos adicionando o seguinte à seção `<runtime>` do arquivo `app.config`:

```xml
<runtime>
  <AppContextSwitchOverrides value="Switch.System.IO.BlockLongPaths=false" />
</runtime>
```

| Name    | Valor       |
|:--------|:------------|
| Escopo   | Secundária       |
| Versão | 4.6.2       |
| Type    | Redirecionando |
