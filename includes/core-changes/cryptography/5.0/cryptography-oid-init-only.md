---
ms.openlocfilehash: cf51d5f6b3eee7189fd9613b3d780a829d197a04
ms.sourcegitcommit: 8bfeb5930ca48b2ee6053f16082dcaf24d46d221
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/18/2020
ms.locfileid: "88557992"
---
### <a name="systemsecuritycryptographyoid-is-functionally-init-only"></a>System. Security. Cryptography. OID é funcionalmente somente init

A <xref:System.Security.Cryptography.Oid?displayProperty=fullName> classe, que é usada para representar valores de identificador de objeto ASN. 1 e seus nomes "amigáveis", anteriormente era totalmente mutável. Essa imutabilidade costuma ser despercebida ou surgiu como uma surpresa. Os setters de propriedade agora lançam um <xref:System.PlatformNotSupportedException> quando você tenta alterar o valor depois que ele já foi atribuído.

#### <a name="change-description"></a>Descrição da alteração

Em versões anteriores, os setters de propriedade em <xref:System.Security.Cryptography.Oid> podem ser usados para alterar o valor das <xref:System.Security.Cryptography.Oid.FriendlyName> <xref:System.Security.Cryptography.Oid.Value> Propriedades e.

No .NET 5,0 e versões posteriores, os setters de propriedade só podem ser usados para inicializar o valor. Depois que a propriedade tiver um valor, de um construtor ou de uma chamada anterior para o setter de propriedade, o setter da propriedade sempre lançará um <xref:System.PlatformNotSupportedException> .

#### <a name="reason-for-change"></a>Motivo da alteração

Essa alteração permite a reutilização de <xref:System.Security.Cryptography.Oid> objetos como parte dos valores de retorno em APIs públicas para reduzir os perfis de alocação de objetos. Ele evita a necessidade de criar cópias "defensivas" temporárias quando <xref:System.Security.Cryptography.Oid> os valores são usados como entradas.

#### <a name="version-introduced"></a>Versão introduzida

5,0 Preview 8

#### <a name="recommended-action"></a>Ação recomendada

Evite usar os <xref:System.Security.Cryptography.Oid> setters de propriedade diferentes de para a inicialização do objeto. Para representar um novo valor, use uma nova instância em vez de alterar o valor em um objeto existente.

#### <a name="category"></a>Categoria

Criptografia

#### <a name="affected-apis"></a>APIs afetadas

- <xref:System.Security.Cryptography.Oid.FriendlyName?displayProperty=fullName>
- <xref:System.Security.Cryptography.Oid.Value?displayProperty=fullName>

<!--

#### Affected APIs

- `P:System.Security.Cryptography.Oid.FriendlyName`
- `P:System.Security.Cryptography.Oid.Value`

-->
