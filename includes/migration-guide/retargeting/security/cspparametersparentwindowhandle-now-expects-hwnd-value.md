---
ms.openlocfilehash: 4b5c886ad35afbbf0a68e03b3174ab9ea1f5524f
ms.sourcegitcommit: e02d17b2cf9c1258dadda4810a5e6072a0089aee
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/01/2020
ms.locfileid: "85614291"
---
### <a name="cspparametersparentwindowhandle-now-expects-hwnd-value"></a>CspParameters.ParentWindowHandle agora espera o valor HWND

#### <a name="details"></a>Detalhes

O valor <xref:System.Security.Cryptography.CspParameters.ParentWindowHandle>, introduzido no .NET Framework 2.0, permite que um aplicativo registre um valor de identificador de janela pai, de modo que qualquer interface do usuário necessária para acessar a chave (por exemplo, uma caixa de diálogo de solicitação de PIN ou de consentimento) seja aberta como um filho modal para a janela especificada. A partir dos aplicativos destinados ao .NET Framework 4.7, um aplicativo do Windows Forms pode definir a propriedade <xref:System.Security.Cryptography.CspParameters.ParentWindowHandle> com código como o seguinte:

```csharp
cspParameters.ParentWindowHandle = form.Handle;
```

Em versões anteriores do .NET Framework, esperava-se que o valor fosse um <xref:System.IntPtr?displayProperty=fullName> representando um local na memória em que o valor de [HWND](https://docs.microsoft.com/windows/desktop/WinProg/windows-data-types#HWND) residia. Definir a propriedade como form.Handle no Windows 7 e em versões anteriores não tinha efeito. No Windows 8 e em versões posteriores, isso resulta em &quot;<xref:System.Security.Cryptography.CryptographicException?displayProperty=fullName>: O parâmetro está incorreto.&quot;

#### <a name="suggestion"></a>Sugestão

Os aplicativos direcionados ao .NET Framework 4.7 ou posterior que desejam registrar um relacionamento de janela pai são incentivados a usar o formato simplificado:

```csharp
cspParameters.ParentWindowHandle = form.Handle;
```

Usuários que identificaram que o valor correto a ser passado era o endereço de um local da memória que mantinha o valor de `form.Handle` podem recusar essa alteração de comportamento definindo o comutador AppContext `Switch.System.Security.Cryptography.DoNotAddrOfCspParentWindowHandle` como `true`:

- Configurando de forma programática as opções de compatibilidade em AppContext, conforme explicado [aqui](https://devblogs.microsoft.com/dotnet/net-announcements-at-build-2015/#dotnet46).
- Adicionando a seguinte linha na seção `<runtime>` do arquivo app.config:

```xml
<runtime>
 <AppContextSwitchOverrides value="Switch.System.Security.Cryptography.DoNotAddrOfCspParentWindowHandle=true"/>
</runtime>
```

Por outro lado, usuários que quiserem aceitar o novo comportamento no runtime do .NET Framework 4.7 quando o aplicativo for carregado em versões anteriores do .NET Framework podem definir a opção de AppContext como `false`.

| Name    | Valor       |
|:--------|:------------|
| Escopo   | Secundária       |
| Versão | 4.7         |
| Type    | Redirecionando |

#### <a name="affected-apis"></a>APIs afetadas

- <xref:System.Security.Cryptography.CspParameters.ParentWindowHandle?displayProperty=nameWithType>
