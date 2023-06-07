---
ms.openlocfilehash: 8b0edd9a49ca431355ab4f57fa041c5d1756d7eb
ms.sourcegitcommit: c37e8d4642fef647ebab0e1c618ecc29ddfe2a0f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/06/2020
ms.locfileid: "87855654"
---
> [!CAUTION]
> CAS (segurança de acesso a código) e código parcialmente confiável
>
> O .NET Framework fornece um mecanismo para a imposição de níveis variáveis de confiança em códigos diferentes em execução no mesmo aplicativo chamado CAS (Segurança de Acesso do Código).
>
> **Não há suporte para CAS no .NET Core, no .NET 5 ou em versões posteriores. Não há suporte para CAS em versões do C# posteriores a 7,0.**
>
> As CAS no .NET Framework não devem ser usadas como um mecanismo para impor limites de segurança com base na origem do código ou em outros aspectos de identidade. O CAS e o código Transparent Security não têm suporte como um limite de segurança com código parcialmente confiável, especialmente código de origem desconhecida. Não aconselhamos carregar e executar códigos de origens desconhecidas sem a adoção de medidas de segurança alternativas no local.
>
> Essa política é aplicável à todas as versões do .NET Framework, mas não é aplicável ao .NET Framework incluído no Silverlight.
