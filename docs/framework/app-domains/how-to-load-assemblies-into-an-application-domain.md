---
title: 'Como: Carregar assemblies em um domínio do aplicativo'
description: Saiba como carregar assemblies em um domínio de aplicativo no .NET. A maneira recomendada é usar o método de carga estático (ou compartilhado) no System. Reflection. assembly.
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
- cpp
helpviewer_keywords:
- application domains, loading assemblies
- loading assemblies
ms.assetid: 1432aa2d-bd83-4346-bf3b-a1b7920e2aa9
ms.openlocfilehash: c91c70625b79002e9297755dfdfac8aa6e283208
ms.sourcegitcommit: 1c37a894c923bea021a3cc38ce7cba946357bbe1
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/19/2020
ms.locfileid: "85104749"
---
# <a name="how-to-load-assemblies-into-an-application-domain"></a>Como: Carregar assemblies em um domínio do aplicativo
Há várias maneiras de carregar um assembly em um domínio de aplicativo. A maneira recomendada é usar o método <xref:System.Reflection.Assembly.Load%2A>`static` (`Shared` no Visual Basic) da classe <xref:System.Reflection.Assembly?displayProperty=nameWithType>. Outras maneiras que os assemblies podem ser carregados incluem:  
  
- O método <xref:System.Reflection.Assembly.LoadFrom%2A> da classe <xref:System.Reflection.Assembly> carrega um assembly dado seu local do arquivo. O carregamento de assemblies com esse método usa um contexto de carga diferente.  
  
- Os métodos <xref:System.Reflection.Assembly.ReflectionOnlyLoad%2A> e <xref:System.Reflection.Assembly.ReflectionOnlyLoadFrom%2A> carregam um assembly no contexto de somente reflexão. Os assemblies carregados neste contexto podem ser examinados, mas não executados, permitindo o exame dos assemblies que visam outras plataformas. Consulte [Como carregar assemblies no contexto de somente para reflexão](../reflection-and-codedom/how-to-load-assemblies-into-the-reflection-only-context.md).  
  
> [!NOTE]
> O contexto de somente reflexão é uma novidade no .NET Framework versão 2.0.  
  
- Métodos como <xref:System.AppDomain.CreateInstance%2A> e <xref:System.AppDomain.CreateInstanceAndUnwrap%2A> da classe <xref:System.AppDomain> podem carregar assemblies em um domínio de aplicativo.  
  
- O método <xref:System.Type.GetType%2A> da classe <xref:System.Type> pode carregar assemblies.  
  
- O método <xref:System.AppDomain.Load%2A> da classe <xref:System.AppDomain?displayProperty=nameWithType> pode carregar assemblies, mas ele é usado principalmente para a interoperabilidade COM. Ele não deve ser usado para carregar assemblies em um domínio do aplicativo diferente do domínio do aplicativo do qual ele foi chamado.  
  
> [!NOTE]
> A partir do .NET Framework versão 2.0, o runtime não carregará um assembly que foi compilado com uma versão do .NET Framework que tenha um número de versão mais alto do que o runtime atualmente carregado. Isso vale para a combinação dos componentes principal e secundário do número de versão.  
  
 Você pode especificar a maneira como o JIT (Just-In-Time) compilou o código dos assemblies carregados é compartilhado entre os domínios do aplicativo. Para obter mais informações, consulte [domínios de aplicativo e assemblies](application-domains.md#application-domains-and-assemblies).  
  
## <a name="example"></a>Exemplo  
 O código a seguir carrega um assembly chamado "example.exe" ou "example.dll" para o domínio de aplicativo atual, obtém um tipo chamado `Example` do assembly, obtém um método sem parâmetros chamado `MethodA` para esse tipo e executa o método. Para encontrar uma discussão completa sobre como obter as informações de um assembly carregado, consulte [Dynamically Loading and Using Types](../reflection-and-codedom/dynamically-loading-and-using-types.md) (Carregando e usando tipos dinamicamente).  
  
 [!code-cpp[System.AppDomain.Load#2](../../../samples/snippets/cpp/VS_Snippets_CLR_System/system.appdomain.load/cpp/source2.cpp#2)]
 [!code-csharp[System.AppDomain.Load#2](../../../samples/snippets/csharp/VS_Snippets_CLR_System/system.appdomain.load/cs/source2.cs#2)]
 [!code-vb[System.AppDomain.Load#2](../../../samples/snippets/visualbasic/VS_Snippets_CLR_System/system.appdomain.load/vb/source2.vb#2)]  
  
## <a name="see-also"></a>Veja também

- <xref:System.Reflection.Assembly.ReflectionOnlyLoad%2A>
- [Programação com domínios do aplicativo](application-domains.md#programming-with-application-domains)
- [Reflexão](../reflection-and-codedom/reflection.md)
- [Usando domínios do aplicativo](use.md)
- [Como: Carregar assemblies no contexto somente de reflexão](../reflection-and-codedom/how-to-load-assemblies-into-the-reflection-only-context.md)
- [Domínios de aplicativo e assemblies](application-domains.md#application-domains-and-assemblies)
