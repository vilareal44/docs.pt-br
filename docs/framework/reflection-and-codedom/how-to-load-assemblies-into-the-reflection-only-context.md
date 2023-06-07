---
title: 'Como: Carregar assemblies no contexto somente de reflexão'
description: Exiba um exemplo de como carregar assemblies no contexto somente de reflexão no .NET. Examine os assemblies compilados para outras plataformas ou versões do .NET.
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
- cpp
helpviewer_keywords:
- reflection, reflection-only loader context
- assemblies [.NET Framework], loading for reflection
- attributes [.NET Framework], retrieving
- assemblies [.NET Framework], reflection-only loader context
- reflection-only loader context
ms.assetid: 9818b660-52f5-423d-a9af-e75163aa7068
ms.openlocfilehash: 92f847f6c61ba39bf8621af6080baccfdabe514a
ms.sourcegitcommit: 3d84eac0818099c9949035feb96bbe0346358504
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/21/2020
ms.locfileid: "86865067"
---
# <a name="how-to-load-assemblies-into-the-reflection-only-context"></a>Como: Carregar assemblies no contexto somente de reflexão

O contexto de carregamento de somente reflexão permite que você examine assemblies compilados para outras plataformas ou para outras versões do .NET Framework. O código carregado neste contexto pode apenas ser examinado, ele não pode ser executado. Isso significa que os objetos não podem ser criados, pois os construtores não podem ser executados. Como o código não pode ser executado, as dependências não são carregadas automaticamente. Se precisar examiná-las, você mesmo deverá carregá-las.

## <a name="to-load-an-assembly-into-the-reflection-only-load-context"></a>Para carregar um assembly no contexto de carregamento de somente reflexão

1. Use a sobrecarga do método <xref:System.Reflection.Assembly.ReflectionOnlyLoad%28System.String%29> para carregar o assembly dado seu nome de exibição ou o método <xref:System.Reflection.Assembly.ReflectionOnlyLoadFrom%2A> para carregar o assembly dado seu caminho. Se o assembly for uma imagem binária, use a sobrecarga do método <xref:System.Reflection.Assembly.ReflectionOnlyLoad%28System.Byte%5B%5D%29>.

    > [!NOTE]
    > Você não pode usar o contexto de somente reflexão para carregar uma versão de mscorlib.dll de uma versão do .NET Framework diferente da versão no contexto de execução.

2. Se o assembly tiver dependências, o método <xref:System.Reflection.Assembly.ReflectionOnlyLoad%2A> não as carregará. Se precisar examiná-las, você mesmo deverá carregá-las.

3. Determine se um assembly é carregado no contexto de somente reflexão usando a propriedade <xref:System.Reflection.Assembly.ReflectionOnly%2A> do assembly.

4. Se os atributos tiverem sido aplicados ao assembly ou aos tipos no assembly, examine esses atributos usando a classe <xref:System.Reflection.CustomAttributeData> para garantir que não seja feita nenhuma tentativa de executar o código no contexto de somente reflexão. Use a sobrecarga adequada do método <xref:System.Reflection.CustomAttributeData.GetCustomAttributes%2A?displayProperty=nameWithType> para obter objetos <xref:System.Reflection.CustomAttributeData> que representam os atributos aplicados a um assembly, membro, módulo ou parâmetro.

    > [!NOTE]
    > Os atributos aplicados ao assembly ou ao seu conteúdo podem ser definidos no assembly ou em outro assembly carregado no contexto de somente reflexão. Não há nenhuma maneira de dizer antecipadamente onde os atributos são definidos.

## <a name="example"></a>Exemplo

O exemplo de código a seguir mostra como examinar os atributos aplicados a um assembly carregado no contexto de somente reflexão.

O exemplo de código define um atributo personalizado com dois construtores e uma propriedade. O atributo é aplicado ao assembly, a um tipo declarado no assembly, para um método do tipo e a um parâmetro do método. Quando executado, o próprio assembly faz seu carregamento no contexto de somente reflexão e exibe as informações sobre os atributos personalizados que foram aplicados a ele e aos tipos e os membros que ele contém.

> [!NOTE]
> Para simplificar o exemplo de código, o próprio assembly faz seu carregamento e análise. Normalmente, você não esperaria encontrar o mesmo assembly carregado no contexto de execução e no contexto de somente reflexão.

[!code-cpp[CustomAttributeData#1](../../../samples/snippets/cpp/VS_Snippets_CLR/CustomAttributeData/CPP/source.cpp#1)]
[!code-csharp[CustomAttributeData#1](../../../samples/snippets/csharp/VS_Snippets_CLR/CustomAttributeData/CS/source.cs#1)]
[!code-vb[CustomAttributeData#1](../../../samples/snippets/visualbasic/VS_Snippets_CLR/CustomAttributeData/VB/source.vb#1)]

## <a name="see-also"></a>Confira também

- <xref:System.Reflection.Assembly.ReflectionOnlyLoad%2A>
- <xref:System.Reflection.Assembly.ReflectionOnly%2A>
- <xref:System.Reflection.CustomAttributeData>
