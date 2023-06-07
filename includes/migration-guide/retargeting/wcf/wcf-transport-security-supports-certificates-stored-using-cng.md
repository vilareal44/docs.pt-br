---
ms.openlocfilehash: 5c09bee92f679cd7e7a95cd23d5ce0ca9b57170c
ms.sourcegitcommit: e02d17b2cf9c1258dadda4810a5e6072a0089aee
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/01/2020
ms.locfileid: "85614287"
---
### <a name="wcf-transport-security-supports-certificates-stored-using-cng"></a>Segurança de transporte do WCF é compatível com certificados armazenados usando CNG

#### <a name="details"></a>Detalhes

Começando com os aplicativos destinados ao .NET Framework 4.6.2, a segurança de transporte do WCF é compatível com certificados armazenados usando a CNG (Biblioteca de Criptografia do Windows). Esse suporte é limitado a certificados com uma chave pública que tenha um expoente não superior a 32 bits de comprimento. Quando o aplicativo é destinado ao .NET Framework 4.6.2, esse recurso é ativado por padrão. Em versões anteriores do .NET Framework, a tentativa de usar os certificados X509 com um provedor de armazenamento de chaves CSG gera uma exceção.

#### <a name="suggestion"></a>Sugestão

Os aplicativos destinados ao .NET Framework 4.6.1 e versões anteriores, mas que são executados no .NET Framework 4.6.2, podem habilitar a compatibilidade com os certificados CNG adicionando a seguinte linha à seção `<runtime>` do arquivo app.config ou web.config:

```xml
<runtime>
  <AppContextSwitchOverrides value="Switch.System.ServiceModel.DisableCngCertificates=false" />
</runtime>
```

Isso também pode ser feito de modo programático com o seguinte código:

```csharp
private const string DisableCngCertificates = @"Switch.System.ServiceModel.DisableCngCertificate";

AppContext.SetSwitch(disableCngCertificates, false);
```

```vb
Const DisableCngCertificates As String = "Switch.System.ServiceModel.DisableCngCertificates"
AppContext.SetSwitch(disableCngCertificates, False)
```

Observe que, por causa dessa alteração, qualquer código de tratamento de exceção que dependa da tentativa de iniciar a comunicação segura com um certificado CNG para falhar não será mais executado.

| Name    | Valor       |
|:--------|:------------|
| Escopo   | Secundária       |
| Versão | 4.6.2       |
| Type    | Redirecionando |
