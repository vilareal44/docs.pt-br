---
ms.openlocfilehash: 0024b2a53444319788b8cdd312d537f994070b5e
ms.sourcegitcommit: e02d17b2cf9c1258dadda4810a5e6072a0089aee
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/01/2020
ms.locfileid: "85614290"
---
### <a name="sslstream-supports-tls-alerts"></a>SslStream dá suporte a Alertas de TLS

#### <a name="details"></a>Detalhes

Após um handshake de TLS com falha, um <xref:System.IO.IOException?displayProperty=fullName> com uma exceção <xref:System.ComponentModel.Win32Exception?displayProperty=fullName> interna será lançado pela primeira operação de Leitura/Gravação de E/S. O <xref:System.ComponentModel.Win32Exception.NativeErrorCode?displayProperty=fullName> código para o <xref:System.ComponentModel.Win32Exception?displayProperty=fullName> pode ser mapeado para o alerta TLS da parte remota usando os [códigos de erro Schannel para alertas TLS e SSL](https://docs.microsoft.com/windows/desktop/SecAuthN/schannel-error-codes-for-tls-and-ssl-alerts). Para obter mais informações, consulte [RFC 2246: seção 7.2.2 Error Alerts](https://tools.ietf.org/html/rfc2246#section-7.2.2). <br/>O comportamento no .NET Framework 4.6.2 e nas versões anteriores é que o canal de transporte (normalmente a conexão TCP) atingirá o tempo limite durante a gravação ou a leitura se a outra parte falhar no handshake e rejeitar a conexão logo depois.

#### <a name="suggestion"></a>Sugestão

Os aplicativos que chamam APIs de E/S de rede, como <xref:System.IO.Stream.Read(System.Byte[],System.Int32,System.Int32)>/<xref:System.IO.Stream.Write(System.Byte[],System.Int32,System.Int32)>, devem tratar <xref:System.IO.IOException> ou <xref:System.TimeoutException?displayProperty=fullName>.<br/>O recurso de Alertas TLS é habilitado por padrão, começando com o .NET Framework 4.7. Os aplicativos direcionados às versões 4.0 a 4.6.2 do .NET Framework em execução no .NET Framework 4.7 ou em um sistema superior terão o recurso desabilitado para preservar a compatibilidade. <br/>A API de configuração a seguir está disponível para habilitar ou desabilitar o recurso para os aplicativos do .NET Framework 4.6 e posteriores em execução no .NET Framework 4.7 ou posterior.

- Programaticamente: deve ser a primeira coisa que o aplicativo faz, pois ServicePointManager será inicializado apenas uma vez:

    ```csharp
    AppContext.SetSwitch("TestSwitch.LocalAppContext.DisableCaching", true);

    // Set to 'false' to enable the feature in .NET Framework 4.6 - 4.6.2.
    AppContext.SetSwitch("Switch.System.Net.DontEnableTlsAlerts", true);
    ```

- AppConfig:

    ```xml
    <runtime>
      <AppContextSwitchOverrides value="Switch.System.Net.DontEnableTlsAlerts=true"/>
      <!-- Set to 'false' to enable the feature in .NET Framework 4.6 - 4.6.2. -->
    </runtime>
    ```

- Chave do registro (global do computador): defina o valor como `false` para habilitar o recurso no .NET Framework 4,6-4.6.2.

    ```ini
    Key: HKLM\SOFTWARE\Wow6432Node\Microsoft\.NETFramework\AppContext\Switch.System.Net.DontEnableTlsAlerts
    - Type: String
    - Value: "true"
    ```

| Name    | Valor       |
|:--------|:------------|
| Escopo   | Microsoft Edge        |
| Versão | 4.7         |
| Type    | Redirecionando |

#### <a name="affected-apis"></a>APIs afetadas

- <xref:System.Net.Security.SslStream?displayProperty=nameWithType>
- <xref:System.Net.WebRequest?displayProperty=nameWithType>
- <xref:System.Net.HttpWebRequest?displayProperty=nameWithType>
- <xref:System.Net.FtpWebRequest?displayProperty=nameWithType>
- <xref:System.Net.Mail.SmtpClient?displayProperty=nameWithType>
- <xref:System.Net.Http?displayProperty=nameWithType>
