---
ms.openlocfilehash: 04c4fb4c8e9da8c58a5e26f78a7b13aa6a0df4a0
ms.sourcegitcommit: e02d17b2cf9c1258dadda4810a5e6072a0089aee
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/01/2020
ms.locfileid: "85614278"
---
### <a name="changes-in-path-normalization"></a>Alterações na normalização de caminho

#### <a name="details"></a>Detalhes

A partir de aplicativos destinados ao .NET Framework 4.6.2, a maneira como o runtime normaliza caminhos foi alterada. Normalizar um caminho envolve modificar a cadeia de caracteres que identifica um arquivo ou caminho para que ela corresponda a um caminho válido no sistema operacional de destino. Normalmente, a normalização envolve:

- Padronização de separadores de diretório e componente.
- Aplicação do diretório atual a um caminho relativo.
- Avaliação do diretório relativo (.) ou do diretório pai (..) em um caminho.
- Remoção de determinados caracteres.
A partir dos aplicativos destinados ao .NET Framework 4.6.2, as seguintes alterações na normalização do caminho são habilitadas por padrão:
  - O runtime atende à função [GetFullPathName](https://docs.microsoft.com/windows/desktop/api/fileapi/nf-fileapi-getfullpathnamew) do sistema operacional para normalizar caminhos.
- A normalização não envolve mais a remoção do fim dos segmentos do diretório (como um espaço no fim do nome de um diretório).
- Suporte à sintaxe do caminho do dispositivo em confiança total, incluindo `\\.\` e, para APIs de E/S de arquivo em mscorlib.dll, `\\?\`.
- O runtime não valida caminhos de sintaxe do dispositivo.
- Há suporte ao uso da sintaxe de dispositivo para acessar fluxos de dados alternados.
Essas alterações devem melhorar o desempenho e ao mesmo tempo permitir que os métodos acessem caminhos anteriormente inacessíveis. Os aplicativos direcionados ao .NET Framework 4.6.1 e versões anteriores, mas que são executados no .NET Framework 4.6.2 ou posteriores, não são afetados por essa alteração.

#### <a name="suggestion"></a>Sugestão

Aplicativos destinados ao .NET Framework 4.6.2 ou posteriores podem recusar essa alteração e usar a normalização herdada adicionando o seguinte à seção `<runtime>` do arquivo de configuração de aplicativo:

```xml
<runtime>
  <AppContextSwitchOverrides value="Switch.System.IO.UseLegacyPathHandling=true" />
</runtime>
```

Aplicativos destinados ao .NET Framework 4.6.1 ou anteriores, mas que são executados no .NET Framework 4.6.2 ou posteriores podem habilitar as alterações na normalização de caminho adicionando a seguinte linha à seção `<runtime>` do arquivo de configuração de aplicativo:

```xml
<runtime>
  <AppContextSwitchOverrides value="Switch.System.IO.UseLegacyPathHandling=false" />
</runtime>
```

| Name    | Valor       |
|:--------|:------------|
| Escopo   | Secundária       |
| Versão | 4.6.2       |
| Type    | Redirecionando |
