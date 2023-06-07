---
title: Reflexão no .NET Framework para aplicativos da Windows Store
description: Use a reflexão no .NET para aplicativos da Windows Store. Há um conjunto de tipos de reflexo e membros a serem usados em aplicativos da Windows Store, que estão disponíveis para o .NET completo.
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
helpviewer_keywords:
- reflection, Windows Store apps
- .NET for Windows Store apps, TypeInfo class
ms.assetid: 0d07090c-9b47-4ecc-81d1-29d539603c9b
ms.openlocfilehash: 86bb633e97f0f88dc2a2a042b6897695a258cc3c
ms.sourcegitcommit: 3d84eac0818099c9949035feb96bbe0346358504
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/21/2020
ms.locfileid: "86865262"
---
# <a name="reflection-in-the-net-framework-for-windows-store-apps"></a>Reflexão no .NET Framework para aplicativos da Windows Store

A partir do .NET Framework 4,5, o .NET Framework inclui um conjunto de tipos de reflexo e membros para uso em aplicativos da loja do Windows 8. x. Esses tipos e membros estão disponíveis no .NET Framework completo, bem como no .NET para aplicativos da Windows Store. Este documento explica as principais diferenças entre eles e seus equivalentes no .NET Framework 4 e em versões anteriores.  
  
 Se você estiver criando um aplicativo da loja do Windows 8. x, deverá usar os tipos de reflexo e os membros no .NET para aplicativos da loja do Windows 8. x. Esses membros e tipos também estão disponíveis, mas não são obrigatórios, para uso em aplicativos da área de trabalho, portanto você pode usar o mesmo código para os dois tipos de aplicativos.  
  
## <a name="typeinfo-and-assembly-loading"></a>TypeInfo e Carregamento do Assembly  
 No .NET para aplicativos da loja do Windows 8. x, a <xref:System.Reflection.TypeInfo> classe contém algumas das funcionalidades da classe .NET Framework 4 <xref:System.Type> . Um objeto <xref:System.Type> representa uma referência a uma definição de tipo, enquanto um objeto <xref:System.Reflection.TypeInfo> representa a definição de tipo em si. Isso permite que você manipule objetos <xref:System.Type> sem necessariamente exigir que o runtime carregue o assembly que eles referenciam. Obter o objeto <xref:System.Reflection.TypeInfo> associado força o assembly a ser carregado.  
  
 <xref:System.Reflection.TypeInfo>contém muitos dos membros disponíveis em <xref:System.Type> , e muitas das propriedades de reflexão no .net para aplicativos da loja do Windows 8. x retornam coleções de <xref:System.Reflection.TypeInfo> objetos. Para obter um objeto <xref:System.Reflection.TypeInfo> de um objeto <xref:System.Type>, use o método <xref:System.Reflection.IReflectableType.GetTypeInfo%2A>.  
  
## <a name="query-methods"></a>Métodos de consulta  
 No .NET para aplicativos da loja do Windows 8. x, você usa as propriedades de reflexão que retornam <xref:System.Collections.Generic.IEnumerable%601> coleções em vez de métodos que retornam matrizes. Os contextos de reflexão podem implementar passagens lentas dessas coleções para grandes tipos ou assemblies.  
  
 As propriedades de reflexão retornam apenas os métodos declarados em um objeto específico em vez de percorrer a árvore de herança. Além disso, elas não usam parâmetros <xref:System.Reflection.BindingFlags> para filtragem. Em vez disso, a filtragem ocorre no código do usuário, usando consultas LINQ nas coleções retornadas. Para objetos de reflexão que se originam com o runtime (por exemplo, como resultado de `typeof(Object)`), a passagem pela árvore de herança é melhor realizada usando os métodos auxiliares da classe <xref:System.Reflection.RuntimeReflectionExtensions>. Os consumidores de objetos de contextos de reflexão personalizados não podem usar esses métodos e devem percorrer a árvore de herança sozinhos.  
  
## <a name="restrictions"></a>Restrições  
 Em um aplicativo da loja do Windows 8. x, o acesso a alguns tipos de .NET Framework e membros é restrito. Por exemplo, você não pode chamar .NET Framework métodos que não estão incluídos no .NET para aplicativos da loja do Windows 8. x, usando um <xref:System.Reflection.MethodInfo> objeto. Além disso, determinados tipos e membros que não são considerados seguros no contexto de um aplicativo da loja do Windows 8. x são bloqueados, como são <xref:System.Runtime.InteropServices.Marshal> e <xref:System.Runtime.InteropServices.WindowsRuntime.WindowsRuntimeMarshal> Membros. Essa restrição afeta apenas os tipos e membros do .NET Framework. Você pode chamar seu código ou o código de terceiros como faria normalmente.  
  
## <a name="example"></a>Exemplo  
 Este exemplo usa os tipos de reflexo e os membros no .NET para aplicativos da loja do Windows 8. x para recuperar os métodos e as propriedades do <xref:System.Globalization.Calendar> tipo, incluindo métodos herdados e propriedades. Para executar esse código, Cole-o no arquivo de código de uma página de armazenamento do Windows 8. x que contenha um <xref:Windows.UI.Xaml.Controls.TextBlock?displayProperty=nameWithType> controle chamado `textblock1` em um projeto chamado Reflection. Se você colar esse código dentro de um projeto com um nome diferente, certifique-se de alterar o nome do namespace para corresponder ao seu projeto.  
  
 [!code-csharp[System.ReflectionWinStoreApp#1](../../../samples/snippets/csharp/VS_Snippets_CLR_System/system.reflectionwinstoreapp/cs/mainpage.xaml.cs#1)]
 [!code-vb[System.ReflectionWinStoreApp#1](../../../samples/snippets/visualbasic/VS_Snippets_CLR_System/system.reflectionwinstoreapp/vb/mainpage.xaml.vb#1)]  
  
## <a name="see-also"></a>Veja também

- [Reflexão](reflection.md)
