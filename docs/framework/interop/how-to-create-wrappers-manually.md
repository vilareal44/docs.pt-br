---
title: 'Como: Criar wrappers manualmente'
description: Crie wrappers de tipos COM manualmente. Use um arquivo IDL existente ou biblioteca de tipos, ou crie declarações gerenciadas e exporte o assembly para uma biblioteca de tipos.
ms.date: 03/30/2017
helpviewer_keywords:
- wrappers, creating manually
ms.assetid: cc2a70d8-6a58-4071-a8cf-ce28c018c09b
ms.openlocfilehash: e562a7e963ff744bf9193821d54dd898db521464
ms.sourcegitcommit: e02d17b2cf9c1258dadda4810a5e6072a0089aee
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/01/2020
ms.locfileid: "85619579"
---
# <a name="how-to-create-wrappers-manually"></a>Como: Criar wrappers manualmente
Se você decidir declarar tipos COM manualmente no código-fonte gerenciado, o melhor lugar para começar é com um arquivo de linguagem IDL ou uma biblioteca de tipos existente. Quando você não tem o arquivo IDL ou não pode gerar um arquivo de biblioteca de tipos, pode simular os tipos COM criando declarações gerenciadas e exportando o assembly resultante para uma biblioteca de tipos.  
  
### <a name="to-simulate-com-types-from-managed-source"></a>Para simular tipos COM de uma fonte gerenciada  
  
1. Declare os tipos em uma linguagem que está em conformidade com o CLS (Common Language Specification) e compile o arquivo.  
  
2. Exporte o assembly que contém os tipos com o [Exportador de Biblioteca de Tipos (Tlbexp.exe)](../tools/tlbexp-exe-type-library-exporter.md).  
  
3. Use a biblioteca de tipos COM exportada como base para declarar tipos gerenciados orientados ao COM.  
  
### <a name="to-create-a-runtime-callable-wrapper-rcw"></a>Para criar um RCW (Runtime Callable Wrapper)  
  
1. Supondo que você tenha um arquivo IDL ou um arquivo de biblioteca de tipos, decida quais classes e interfaces você deseja incluir no RCW personalizado. Exclua todos os tipos que você não pretende usar direta ou indiretamente do aplicativo.  
  
2. Crie um arquivo de origem em uma linguagem em conformidade com CLS e declare os tipos. Consulte [Resumo da conversão de bibliotecas de tipos em assemblies](https://docs.microsoft.com/previous-versions/dotnet/netframework-4.0/k83zzh38(v=vs.100)) para obter uma descrição completa do processo de conversão de importação. Efetivamente, quando você cria um RCW (Runtime Callable Wrapper) personalizado, você está executando manualmente a atividade de conversão de tipo fornecida pelo [Importador da Biblioteca de Tipos (Tlbimp.exe)](../tools/tlbimp-exe-type-library-importer.md). O exemplo na próxima seção mostra os tipos em um arquivo IDL ou de biblioteca de tipos e seus tipos correspondentes no código C#.  
  
3. Quando as declarações forem concluídas, compile o arquivo como compilaria qualquer código-fonte gerenciado.  
  
4. Assim como ocorre com os tipos importados com Tlbimp.exe, alguns exigem informações adicionais, que podem ser adicionadas diretamente ao código. Para obter detalhes, consulte [Como editar assemblies de interoperabilidade](https://docs.microsoft.com/previous-versions/dotnet/netframework-4.0/8zbc969t(v=vs.100)).  
  
## <a name="example"></a>Exemplo  
 O código a seguir mostra um exemplo da interface `ISATest` e da classe `SATest` na IDL e os tipos correspondentes no código-fonte C#.  
  
 **Arquivo IDL ou de biblioteca de tipos**  
  
```cpp
 [  
object,  
uuid(40A8C65D-2448-447A-B786-64682CBEF133),  
dual,  
helpstring("ISATest Interface"),  
pointer_default(unique)  
 ]  
interface ISATest : IDispatch  
 {  
[id(1), helpstring("method InSArray")]
HRESULT InSArray([in] SAFEARRAY(int) *ppsa, [out,retval] int *pSum);  
 };  
 [  
uuid(116CCA1E-7E39-4515-9849-90790DA6431E),  
helpstring("SATest Class")  
 ]  
coclass SATest  
 {  
  [default] interface ISATest;  
 };  
```  
  
 **Wrapper no código-fonte gerenciado**  
  
```csharp  
using System;  
using System.Runtime.InteropServices;  
using System.Runtime.CompilerServices;  
  
[assembly:Guid("E4A992B8-6F5C-442C-96E7-C4778924C753")]  
[assembly:ImportedFromTypeLib("SAServerLib")]  
namespace SAServer  
{  
 [ComImport]  
 [Guid("40A8C65D-2448-447A-B786-64682CBEF133")]  
 [TypeLibType(TypeLibTypeFlags.FLicensed)]  
 public interface ISATest  
 {  
  [DispId(1)]  
  //[MethodImpl(MethodImplOptions.InternalCall,  
  // MethodCodeType=MethodCodeType.Runtime)]  
  int InSArray( [MarshalAs(UnmanagedType.SafeArray,  
      SafeArraySubType=VarEnum.VT_I4)] ref int[] param );  
 }
 [ComImport]  
 [Guid("116CCA1E-7E39-4515-9849-90790DA6431E")]  
 [ClassInterface(ClassInterfaceType.None)]  
 [TypeLibType(TypeLibTypeFlags.FCanCreate)]  
 public class SATest : ISATest  
 {  
  [DispId(1)]  
  [MethodImpl(MethodImplOptions.InternalCall,
  MethodCodeType=MethodCodeType.Runtime)]  
  extern int ISATest.InSArray( [MarshalAs(UnmanagedType.SafeArray,
  SafeArraySubType=VarEnum.VT_I4)] ref int[] param );  
 }  
}  
```  
  
## <a name="see-also"></a>Consulte também

- [Personalizando RCWs (Runtime Callable Wrappers)](https://docs.microsoft.com/previous-versions/dotnet/netframework-4.0/e753eftz(v=vs.100))
- [Tipos de dados COM](https://docs.microsoft.com/previous-versions/dotnet/netframework-4.0/sak564ww(v=vs.100))
- [Como editar assemblies de interoperabilidade](https://docs.microsoft.com/previous-versions/dotnet/netframework-4.0/8zbc969t(v=vs.100))
- [Resumo da conversão de bibliotecas de tipos em assemblies](https://docs.microsoft.com/previous-versions/dotnet/netframework-4.0/k83zzh38(v=vs.100))
- [Tlbimp.exe (tipo de importador de biblioteca de tipos)](../tools/tlbimp-exe-type-library-importer.md)
- [Tlbexp.exe (exportador de biblioteca de tipos)](../tools/tlbexp-exe-type-library-exporter.md)
