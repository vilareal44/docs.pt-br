---
ms.openlocfilehash: 207dba9327cfd6debd15c5573697f8950b6c2c95
ms.sourcegitcommit: cb27c01a8b0b4630148374638aff4e2221f90b22
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/09/2020
ms.locfileid: "86218133"
---
### <a name="tls-1x-by-default-passes-the-sch_send_aux_record-flag-to-the-underlying-schannel-api"></a>TLS 1.x por padrão passa o sinalizador SCH_SEND_AUX_RECORD para a API do SCHANNEL subjacente

#### <a name="details"></a>Detalhes

Ao usar TLS 1.x, o .NET Framework depende da API de SCHANNEL do Windows subjacente. A partir do .NET Framework 4.6, o sinalizador [SCH_SEND_AUX_RECORD](https://docs.microsoft.com/windows/win32/api/schannel/ns-schannel-schannel_cred) é passado por padrão para SCHANNEL. Isso faz com que SCHANNEL divida os dados a serem criptografados em dois registros separados, o primeiro como um byte único e o segundo como <em>n</em>-1 bytes. Em casos raros, isso interrompe a comunicação entre clientes e servidores existentes que supõem que os dados residem em um único registro.

#### <a name="suggestion"></a>Sugestão

Se essa alteração interromper a comunicação com um servidor existente, você poderá desabilitá-la enviando o sinalizador [SCH_SEND_AUX_RECORD](https://docs.microsoft.com/windows/win32/api/schannel/ns-schannel-schannel_cred), e restaurar o comportamento anterior de não dividir dados em registros separados adicionando a seguinte opção ao elemento [`<AppContextSwitchOverrides>`](~/docs/framework/configure-apps/file-schema/runtime/appcontextswitchoverrides-element.md) na seção [`<runtime>`](~/docs/framework/configure-apps/file-schema/runtime/runtime-element.md) do arquivo de configuração de aplicativo:

```xml
<runtime>
  <AppContextSwitchOverrides value="Switch.System.Net.DontEnableSchSendAuxRecord=true" />
</runtime>
```

> [!IMPORTANT]
> Essa configuração é fornecida apenas para compatibilidade com versões anteriores. Caso contrário, seu uso não é recomendado.

| Nome    | Valor       |
|:--------|:------------|
| Escopo   | Edge        |
| Versão | 4.6         |
| Type    | Redirecionando |

#### <a name="affected-apis"></a>APIs afetadas

- <xref:System.Net.Security.SslStream?displayProperty=nameWithType>
- <xref:System.Net.ServicePointManager?displayProperty=nameWithType>
- <xref:System.Net.Http.HttpClient?displayProperty=nameWithType>
- <xref:System.Net.Mail.SmtpClient?displayProperty=nameWithType>
- <xref:System.Net.HttpWebRequest?displayProperty=nameWithType>
- <xref:System.Net.FtpWebRequest?displayProperty=nameWithType>
