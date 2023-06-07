---
title: Reflexão e .NET Nativo
ms.date: 03/30/2017
ms.assetid: 91c9eae4-c641-476c-a06e-d7ce39709763
ms.openlocfilehash: 65921377be9b8bf1c2d147b384c85cbd037d15f2
ms.sourcegitcommit: b16c00371ea06398859ecd157defc81301c9070f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/06/2020
ms.locfileid: "73128180"
---
# <a name="reflection-and-net-native"></a>Reflexão e .NET Nativo
No .NET Framework, o desenvolvimento gerenciado oferece suporte à metaprogramação por meio da API de reflexão. A reflexão permite inspecionar objetos em um aplicativo, chamar métodos em objetos descobertos por meio de inspeção, gerar novos tipos no tempo de execução e oferece suporte a muitos outros cenários de código dinâmico. Ele também oferece suporte à serialização e desserialização, o que permite que os valores do campo do objeto sejam mantidos e restaurados posteriormente. Todos esses cenários exigem o compilador do .NET Framework JIT (just-in-time) para gerar código nativo com base em metadados disponíveis.  
  
 O tempo de execução de .NET Native não inclui um compilador JIT. Como resultado, todo o código nativo necessário deve ser gerado com antecedência. Um conjunto de heurística é usado para determinar qual código deve ser gerado, mas esses heurística não pode abranger todos os cenários de metaprogramação possíveis.  Portanto, você deve fornecer dicas para esses cenários metaprogramação usando [diretivas de runtime](runtime-directives-rd-xml-configuration-file-reference.md). Se o código de implementação ou de metadados necessário não está disponível em tempo de execução, seu aplicativo gerará uma exceção [MissingMetadataException](missingmetadataexception-class-net-native.md), [MissingRuntimeArtifactException](missingruntimeartifactexception-class-net-native.md) ou [MissingInteropDataException](missinginteropdataexception-class-net-native.md). Estão disponíveis duas soluções de problemas que gerarão a entrada apropriada para seu arquivo de diretivas de runtime, a qual elimina a exceção:  
  
- A [solução de problemas MissingMetadataException](https://dotnet.github.io/native/troubleshooter/type.html) para tipos.  
  
- A [solução de problemas MissingMetadataException](https://dotnet.github.io/native/troubleshooter/method.html) para métodos.  
  
> [!NOTE]
> Para obter uma visão geral do processo de compilação do .NET Native que fornece informações de contexto sobre a necessidade de ter um arquivo das diretivas de runtime, consulte [.NET Native e compilação](net-native-and-compilation.md).  
  
 Além disso, .NET Native não permite que você reflita sobre membros privados da biblioteca de classes .NET Framework. Por exemplo, uma chamada para a propriedade <xref:System.Reflection.TypeInfo.DeclaredFields%2A?displayProperty=nameWithType> para recuperar os campos de um tipo de biblioteca de classes do .NET Framework retorna somente campos públicos ou protegidos.  
  
 Os tópicos a seguir fornecem a documentação conceitual e de referência necessária para oferecer suporte à reflexão e serialização nos seus aplicativos:  
  
- [APIs que dependem de reflexão](apis-that-rely-on-reflection.md)  
  
- [Referência da API de Reflexão](net-native-reflection-api-reference.md)  
  
- [Referência do arquivo de configuração de diretivas do runtime (rd.xml)](runtime-directives-rd-xml-configuration-file-reference.md)  
  
## <a name="see-also"></a>Confira também

- [Compilando aplicativos com o .NET Nativo](index.md)
- [Compilação e .NET nativo](net-native-and-compilation.md)
