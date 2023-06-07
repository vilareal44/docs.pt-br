---
ms.openlocfilehash: ccd056f23d26e6cce4cc691542784792bffe9fc6
ms.sourcegitcommit: 8bfeb5930ca48b2ee6053f16082dcaf24d46d221
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/18/2020
ms.locfileid: "88558161"
---
### <a name="environmentosversion-returns-the-correct-operating-system-version"></a>Environment. OSVersion retorna a versão correta do sistema operacional

<xref:System.Environment.OSVersion?displayProperty=nameWithType> Retorna a versão real do sistema operacional (SO) em vez de, por exemplo, o sistema operacional selecionado para compatibilidade de aplicativos.

#### <a name="change-description"></a>Descrição da alteração

Nas versões anteriores do .NET, <xref:System.Environment.OSVersion?displayProperty=nameWithType> retorna uma versão do sistema operacional que pode estar incorreta quando um aplicativo é executado no modo de compatibilidade do Windows. Para obter mais informações, consulte [comentários da função GetVersionExA](/windows/win32/api/sysinfoapi/nf-sysinfoapi-getversionexa#remarks).

A partir do .NET 5,0, <xref:System.Environment.OSVersion?displayProperty=nameWithType> retorna a versão real do sistema operacional.

#### <a name="reason-for-change"></a>Motivo da alteração

Os usuários dessa propriedade esperam que ele retorne a versão real do sistema operacional. A maioria dos aplicativos .NET não especifica sua versão com suporte no manifesto do aplicativo e, portanto, obtém a versão padrão com suporte do host dotnet. Como resultado, o Shim de compatibilidade raramente é significativo para o aplicativo em execução. Quando o Windows libera uma nova versão e um host dotnet mais antigo ainda está em uso, esses aplicativos podem obter uma versão incorreta do sistema operacional. Retornar a versão real é mais embutido com as expectativas dos desenvolvedores desta API.

#### <a name="version-introduced"></a>Versão introduzida

5.0

#### <a name="recommended-action"></a>Ação recomendada

Revise e teste qualquer código que use <xref:System.Environment.OSVersion?displayProperty=nameWithType> para garantir que ele se comporta conforme o desejado.

#### <a name="category"></a>Categoria

Bibliotecas principais do .NET

#### <a name="affected-apis"></a>APIs afetadas

- <xref:System.Environment.OSVersion?displayProperty=fullName>

<!--

#### Affected APIs

- `P:System.Environment.OSVersion`

-->
