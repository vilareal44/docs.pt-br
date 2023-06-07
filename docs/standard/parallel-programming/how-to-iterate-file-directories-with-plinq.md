---
title: 'Como: iterar diretórios de arquivos com PLINQ'
ms.date: 03/30/2017
ms.technology: dotnet-standard
helpviewer_keywords:
- PLINQ queries, how to iterate directories
ms.assetid: 354e8ce3-35c4-431c-99ca-7661d1f3901b
ms.openlocfilehash: abf31ea69af6a85140efb783959a9a586ef6a59e
ms.sourcegitcommit: 33deec3e814238fb18a49b2a7e89278e27888291
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/02/2020
ms.locfileid: "84277993"
---
# <a name="how-to-iterate-file-directories-with-plinq"></a>Como: iterar diretórios de arquivos com PLINQ

Este artigo mostra duas maneiras de paralelizar operações em diretórios de arquivos. A primeira consulta usa o método <xref:System.IO.Directory.GetFiles%2A> para preencher uma matriz de nomes de arquivo em um diretório e em todos os subdiretórios. Esse método pode introduzir a latência no início da operação, pois ela não retorna até que toda a matriz seja populada. No entanto, depois que a matriz é populada, o PLINQ pode processá-la em paralelo rapidamente.  
  
A segunda consulta usa os métodos estáticos <xref:System.IO.Directory.EnumerateDirectories%2A> e <xref:System.IO.DirectoryInfo.EnumerateFiles%2A> , que começam a retornar os resultados imediatamente. Essa abordagem pode ser mais rápida quando você está Iterando em árvores de diretório grande, mas o tempo de processamento comparado ao primeiro exemplo depende de muitos fatores.  
  
> [!NOTE]
> Esses exemplos destinam-se a demonstrar o uso e podem não ser executados mais rápido do que a consulta LINQ to Objects sequencial equivalente. Para saber mais sobre agilização, confira [Noções básicas sobre agilização no PLINQ](understanding-speedup-in-plinq.md).  
  
## <a name="getfiles-example"></a>Exemplo de GetFiles

 Este exemplo mostra como iterar em diretórios de arquivos em cenários simples quando você tem acesso a todos os diretórios na árvore, os tamanhos de arquivo não são grandes e os horários de acesso não são significativos. Essa abordagem envolve um período de latência no início, enquanto a matriz de nomes de arquivo está sendo construída.  
  
 [!code-csharp[PLINQ#33](../../../samples/snippets/csharp/VS_Snippets_Misc/plinq/cs/plinqfileiteration.cs#33)]  
  
## <a name="enumeratefiles-example"></a>Exemplo de EnumerateFiles

 Este exemplo mostra como iterar em diretórios de arquivos em cenários simples quando você tem acesso a todos os diretórios na árvore, os tamanhos de arquivo não são grandes e os horários de acesso não são significativos. Essa abordagem começa a produzir resultados mais rápido do que o exemplo anterior.  
  
 [!code-csharp[PLINQ#34](../../../samples/snippets/csharp/VS_Snippets_Misc/plinq/cs/plinqfileiteration.cs#34)]  
  
 Ao usar <xref:System.IO.Directory.GetFiles%2A>, verifique se você tem permissões suficientes em todos os diretórios na árvore. Caso contrário, uma exceção será lançada e nenhum resultado será retornado. Ao usar o <xref:System.IO.Directory.EnumerateDirectories%2A> em uma consulta PLINQ, é problemático tratar as exceções de E/S de uma maneira que permita que você continue a iteração. Se o seu código precisar manipular exceções de acesso não autorizado ou de E/S, considere a abordagem descrita em [Como iterar diretórios de arquivos com a classe paralela](how-to-iterate-file-directories-with-the-parallel-class.md).  
  
 Se a latência de E/S for um problema, por exemplo com E/S de arquivo em uma rede, considere o uso de uma das técnicas de E/S assíncronas descritas em [TPL e programação assíncrona de TPL e .NET Framework](tpl-and-traditional-async-programming.md) e nesta [postagem de blog ](https://devblogs.microsoft.com/pfxteam/parallel-extensions-and-io/).  
  
## <a name="see-also"></a>Veja também

- [LINQ paralelo (PLINQ)](introduction-to-plinq.md)
