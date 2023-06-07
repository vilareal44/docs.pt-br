---
ms.openlocfilehash: c462c7b4ec8423ce8fd331d3cd31154283cf1f1d
ms.sourcegitcommit: e02d17b2cf9c1258dadda4810a5e6072a0089aee
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/01/2020
ms.locfileid: "85619786"
---
### <a name="marshalsizeof-and-marshalptrtostructure-overloads-break-dynamic-code"></a>As sobrecargas Marshal.SizeOf e Marshal.PtrToStructure interrompem o código dinâmico

#### <a name="details"></a>Detalhes

A partir do .NET Framework 4.5.1, a associação dinâmica aos métodos <xref:System.Runtime.InteropServices.Marshal.SizeOf%60%601>, <xref:System.Runtime.InteropServices.Marshal.SizeOf%60%601(%60%600)>, <xref:System.Runtime.InteropServices.Marshal.PtrToStructure(System.IntPtr,System.Object)>, <xref:System.Runtime.InteropServices.Marshal.PtrToStructure(System.IntPtr,System.Type)>, <xref:System.Runtime.InteropServices.Marshal.PtrToStructure%60%601(System.IntPtr)> ou <xref:System.Runtime.InteropServices.Marshal.PtrToStructure%60%601(System.IntPtr,%60%600)> (por meio do Windows PowerShell, IronPython ou da palavra-chave dinâmica do C#, por exemplo) pode resultar em <code>MethodInvocationExceptions</code>, pois novas sobrecargas desses métodos que foram adicionadas podem ser ambíguas para os mecanismos de script.

#### <a name="suggestion"></a>Sugestão

Atualize os scripts para indicar claramente qual sobrecarga deve ser usada. Normalmente, isso pode ser feito convertendo explicitamente os parâmetros de tipo dos métodos como <xref:System.Type>. Clique [neste link](https://support.microsoft.com/kb/2909958/) para obter mais detalhes e exemplos de como contornar o problema.

| Name    | Valor       |
|:--------|:------------|
| Escopo   |Secundária|
|Versão|4.5.1|
|Type|Runtime|
