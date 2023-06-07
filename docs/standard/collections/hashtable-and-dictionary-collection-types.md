---
title: Tipos de coleção Hashtable e Dictionary
ms.date: 03/30/2017
ms.technology: dotnet-standard
helpviewer_keywords:
- Hashtable class, grouping data in collections
- Hashtable collection type
- hash tables
- grouping data in collections, Hashtable collection type
- hash function
- collections [.NET Framework], Hashtable collection type
ms.assetid: bfc20837-3d02-4fc7-8a8f-c5215b6b7913
ms.openlocfilehash: b228f5db16ba01969b77d601becfb94ac0506d1e
ms.sourcegitcommit: 33deec3e814238fb18a49b2a7e89278e27888291
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/02/2020
ms.locfileid: "84287959"
---
# <a name="hashtable-and-dictionary-collection-types"></a>Tipos de coleção Hashtable e Dictionary
A classe <xref:System.Collections.Hashtable?displayProperty=nameWithType>, e as classes genéricas <xref:System.Collections.Generic.Dictionary%602?displayProperty=nameWithType> e <xref:System.Collections.Concurrent.ConcurrentDictionary%602?displayProperty=nameWithType>, implementam a interface <xref:System.Collections.IDictionary?displayProperty=nameWithType>. A classe genérica <xref:System.Collections.Generic.Dictionary%602> também implementa a interface genérica <xref:System.Collections.Generic.IDictionary%602>. Portanto, cada elemento nessas coleções é um par chave-valor.  
  
 Um <xref:System.Collections.Hashtable> objeto consiste em buckets que contêm os elementos da coleção. Um bucket é um subgrupo virtual de elementos dentro de <xref:System.Collections.Hashtable>, o que torna a pesquisa e a recuperação mais fáceis e rápidas do que na maioria das coleções. Cada bucket é associado um código hash, que é gerado usando uma função de hash e tem como base a chave do elemento.  
  
 A classe <xref:System.Collections.Generic.HashSet%601> genérica é uma coleção não ordenada com a finalidade de conter elementos exclusivos.  
  
 Uma função de hash é um algoritmo que retorna um código hash numérico com base em uma chave. A chave é o valor da alguma propriedade do objeto sendo armazenado. Uma função de hash deve sempre retornar o mesmo código hash para a mesma chave. É possível para uma função de hash gerar o mesmo código hash para duas chaves diferentes, mas uma função de hash que gera um código hash exclusivo para cada chave específica resulta em um melhor desempenho ao recuperar os elementos da tabela de hash.  
  
 Cada objeto que é usado como um elemento em um <xref:System.Collections.Hashtable> deve ser capaz de gerar um código hash para si mesmo usando uma implementação do método <xref:System.Object.GetHashCode%2A>. No entanto, você também pode especificar uma função de hash para todos os elementos em uma <xref:System.Collections.Hashtable> usando um construtor <xref:System.Collections.Hashtable> que aceita uma implementação <xref:System.Collections.IHashCodeProvider> como um de seus parâmetros.  
  
 Quando um objeto é adicionado a um <xref:System.Collections.Hashtable>, ele é armazenado no compartimento de memória que está associado ao código hash que corresponde ao código hash do objeto. Quando um valor está sendo procurado no <xref:System.Collections.Hashtable>, o código hash é gerado para esse valor e o bucket associado a esse código hash é pesquisado.  
  
 Por exemplo, uma função de hash para uma cadeia de caracteres pode pegar os códigos ASCII de cada caractere na cadeia e adicioná-los em conjunto para gerar um código hash. A cadeia de caracteres "piquenique" teria um código hash diferente daquele da cadeia de caracteres "cesta"; portanto, as cadeias de caracteres "piquenique" e "cesta" ficariam em buckets diferentes. Por outro lado, "stressed" e "desserts" teriam o mesmo código hash e ficariam no mesmo bucket.  
  
 As classes <xref:System.Collections.Generic.Dictionary%602> e <xref:System.Collections.Concurrent.ConcurrentDictionary%602> têm a mesma funcionalidade que a classe <xref:System.Collections.Hashtable>. Um <xref:System.Collections.Generic.Dictionary%602> de um tipo específico (diferente de <xref:System.Object>) fornece desempenho melhor do que um <xref:System.Collections.Hashtable> para tipos de valor. Isso ocorre porque os elementos de <xref:System.Collections.Hashtable> são do tipo <xref:System.Object>; portanto, conversões boxing e unboxing normalmente ocorrem quando você armazena ou recupera um tipo de valor. A classe <xref:System.Collections.Concurrent.ConcurrentDictionary%602> deve ser usada quando vários threads podem estar acessando a coleção simultaneamente.  
  
## <a name="see-also"></a>Veja também

- <xref:System.Collections.Hashtable>
- <xref:System.Collections.IDictionary>
- <xref:System.Collections.IHashCodeProvider>
- <xref:System.Collections.Generic.Dictionary%602>
- <xref:System.Collections.Generic.IDictionary%602?displayProperty=nameWithType>
- <xref:System.Collections.Concurrent.ConcurrentDictionary%602?displayProperty=nameWithType>
- [Tipos de coleção comumente usados](commonly-used-collection-types.md)
