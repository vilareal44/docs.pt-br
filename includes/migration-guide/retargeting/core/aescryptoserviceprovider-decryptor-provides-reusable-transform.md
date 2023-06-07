---
ms.openlocfilehash: 36a9db601f7637185bf48dfcbe2233b4489fcdcf
ms.sourcegitcommit: e02d17b2cf9c1258dadda4810a5e6072a0089aee
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/01/2020
ms.locfileid: "85614273"
---
### <a name="aescryptoserviceprovider-decryptor-provides-a-reusable-transform"></a>Descriptografador AesCryptoServiceProvider fornece uma transformação reutilizável

#### <a name="details"></a>Detalhes

Começando com aplicativos destinados ao .NET Framework 4.6.2, o descriptografador <xref:System.Security.Cryptography.AesCryptoServiceProvider> fornece uma transformação reutilizável. Após uma chamada para <xref:System.Security.Cryptography.CryptoAPITransform.TransformFinalBlock(System.Byte[],System.Int32,System.Int32)?displayProperty=fullName>, a transformação é reinicializada e pode ser reutilizada. Para aplicativos destinados a versões anteriores do .NET Framework, tentar reutilizar o descriptografador chamando <xref:System.Security.Cryptography.CryptoAPITransform.TransformBlock(System.Byte[],System.Int32,System.Int32,System.Byte[],System.Int32)?displayProperty=fullName> após uma chamada para <xref:System.Security.Cryptography.CryptoAPITransform.TransformFinalBlock(System.Byte[],System.Int32,System.Int32)?displayProperty=fullName> gera um <xref:System.Security.Cryptography.CryptographicException> ou produz dados corrompidos.

#### <a name="suggestion"></a>Sugestão

O impacto dessa alteração deve ser mínimo, uma vez que se trata do comportamento esperado. Aplicativos que dependem do comportamento anterior podem optar por não usá-lo adicionando a seguinte definição de configuração à seção `<runtime>` do arquivo de configuração do aplicativo:

```xml
<runtime>
<AppContextSwitchOverrides value="Switch.System.Security.Cryptography.AesCryptoServiceProvider.DontCorrectlyResetDecryptor=true"/>
</runtime>
```

Além disso, aplicativos destinados a uma versão anterior do .NET Framework, mas em execução em uma versão do .NET Framework a partir do .NET Framework 4.6.2 podem aceitá-lo adicionando a seguinte definição de configuração à seção `<runtime>` do arquivo de configuração do aplicativo:

```xml
<runtime>
<AppContextSwitchOverrides value="Switch.System.Security.Cryptography.AesCryptoServiceProvider.DontCorrectlyResetDecryptor=false"/>
</runtime>
```

| Name    | Valor       |
|:--------|:------------|
| Escopo   | Secundária       |
| Versão | 4.6.2       |
| Type    | Redirecionando |

#### <a name="affected-apis"></a>APIs afetadas

- <xref:System.Security.Cryptography.AesCryptoServiceProvider.CreateDecryptor?displayProperty=nameWithType>
