---
ms.openlocfilehash: 0b62eff8d70873293aafa539f40bf032672df57a
ms.sourcegitcommit: e02d17b2cf9c1258dadda4810a5e6072a0089aee
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/01/2020
ms.locfileid: "85617777"
---
### <a name="managed-cryptography-classes-do-not-throw-a-cryptographyexception-in-fips-mode"></a>Classes de criptografia gerenciadas não geram uma CryptographyException no modo FIPS

#### <a name="details"></a>Detalhes

No .NET Framework 4.7.2 e em versões anteriores, as classes de provedor criptográfico gerenciadas, como <xref:System.Security.Cryptography.SHA256Managed>, geram uma <xref:System.Security.Cryptography.CryptographicException> quando as bibliotecas criptográficas do sistema estão configuradas no modo FIPS. Essas exceções são geradas porque as versões gerenciadas não foram submetidas à certificação 140-2 do padrão FIPS para bloquear algoritmos criptográficos que não foram considerados aprovados com base nas regras do FIPS.  Como poucos desenvolvedores têm computadores de desenvolvimento no modo FIPS, essas exceções são frequentemente geradas apenas em sistemas de produção. Aplicativos que direcionam o .NET Framework 4.8 e versões posteriores mudam automaticamente para a política flexibilizada mais recente para que um <xref:System.Security.Cryptography.CryptographicException> não seja mais gerado por padrão nesses casos. Em vez disso, as classes de criptografia gerenciadas redirecionam as operações criptográficas para uma biblioteca de criptografia do sistema. Essa alteração na política elimina efetivamente uma diferença potencialmente confusa entre os ambientes do desenvolvedor e os ambientes de produção, fazendo com que componentes nativos e componentes gerenciados operem sob a mesma política criptográfica.

#### <a name="suggestion"></a>Sugestão

Se esse comportamento for indesejável, será possível recusá-lo e restaurar o anterior para que um <xref:System.Security.Cryptography.CryptographicException> seja gerado em modo FIPS adicionando a configuração a seguir [AppContextSwitchOverrides](~/docs/framework/configure-apps/file-schema/runtime/appcontextswitchoverrides-element.md) à seção [\<runtime>](~/docs/framework/configure-apps/file-schema/runtime/runtime-element.md) do arquivo de configuração de aplicativo:

```xml
<runtime>
  <AppContextSwitchOverrides value="Switch.System.Security.Cryptography.UseLegacyFipsThrow=true" />
</runtime>
```

Se seu aplicativo direcionar o .NET Framework 4.7.2 ou versões anteriores, também será possível aceitar essa alteração adicionando a seguinte configuração [AppContextSwitchOverrides](~/docs/framework/configure-apps/file-schema/runtime/appcontextswitchoverrides-element.md) à seção [\<runtime>](~/docs/framework/configure-apps/file-schema/runtime/runtime-element.md) do arquivo de configuração de aplicativo:

```xml
<runtime>
  <AppContextSwitchOverrides value="Switch.System.Security.Cryptography.UseLegacyFipsThrow=false" />
</runtime>
```

| Name    | Valor       |
|:--------|:------------|
| Escopo   | Microsoft Edge        |
| Versão | 4.8         |
| Type    | Redirecionando |

#### <a name="affected-apis"></a>APIs afetadas

- <xref:System.Security.Cryptography.AesManaged?displayProperty=nameWithType>
- <xref:System.Security.Cryptography.MD5Cng?displayProperty=nameWithType>
- <xref:System.Security.Cryptography.MD5CryptoServiceProvider?displayProperty=nameWithType>
- <xref:System.Security.Cryptography.RC2CryptoServiceProvider?displayProperty=nameWithType>
- <xref:System.Security.Cryptography.RijndaelManaged?displayProperty=nameWithType>
- <xref:System.Security.Cryptography.RIPEMD160Managed?displayProperty=nameWithType>
- <xref:System.Security.Cryptography.SHA1Managed?displayProperty=nameWithType>
- <xref:System.Security.Cryptography.SHA256Managed?displayProperty=nameWithType>
