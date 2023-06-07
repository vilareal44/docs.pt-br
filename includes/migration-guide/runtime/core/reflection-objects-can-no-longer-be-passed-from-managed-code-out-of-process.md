---
ms.openlocfilehash: a54b17b2002bd0f85b8b47c5e37e040470d6c494
ms.sourcegitcommit: e02d17b2cf9c1258dadda4810a5e6072a0089aee
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/01/2020
ms.locfileid: "85621037"
---
### <a name="reflection-objects-can-no-longer-be-passed-from-managed-code-to-out-of-process-dcom-clients"></a>Os objetos Reflection não podem mais ser passados de código gerenciado para clientes DCOM fora do processo

#### <a name="details"></a>Detalhes

Os objetos Reflection não podem mais ser passados de código gerenciado para clientes DCOM fora do processo. Os seguintes tipos são afetados:<ul><li><xref:System.Reflection.Assembly?displayProperty=fullName></li><li><xref:System.Reflection.MemberInfo?displayProperty=fullName> (e seus tipos derivados, incluindo <xref:System.Reflection.FieldInfo?displayProperty=fullName>, <xref:System.Reflection.MethodInfo?displayProperty=fullName>, <xref:System.Type?displayProperty=fullName> e <xref:System.Reflection.TypeInfo?displayProperty=fullName>)</li><li><xref:System.Reflection.MethodBody?displayProperty=fullName></li><li><xref:System.Reflection.Module?displayProperty=fullName></li><li><xref:System.Reflection.ParameterInfo?displayProperty=fullName>.</li></ul>Chamadas a <code>IMarshal</code> para o retorno do objeto <code>E_NOINTERFACE</code>.

#### <a name="suggestion"></a>Sugestão

Atualize o código de marshaling para trabalhar com objetos de não reflexão

| Name    | Valor       |
|:--------|:------------|
| Escopo   |Secundária|
|Versão|4.6|
|Type|Runtime|
