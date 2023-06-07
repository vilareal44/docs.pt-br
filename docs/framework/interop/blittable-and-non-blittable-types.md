---
title: Tipos blittable e não blittable
description: Saiba mais sobre tipos blittable e não blittable. Os tipos de dados blittable são normalmente representados em memória gerenciada e não gerenciada e não precisam de manipulação especial.
ms.date: 03/30/2017
helpviewer_keywords:
- interop marshaling, blittable types
- blittable types, interop marshaling
ms.assetid: d03b050e-2916-49a0-99ba-f19316e5c1b3
ms.openlocfilehash: 68f4197a2710b6825c83bbc51daaf8f6b5a2c81f
ms.sourcegitcommit: e02d17b2cf9c1258dadda4810a5e6072a0089aee
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/01/2020
ms.locfileid: "85621529"
---
# <a name="blittable-and-non-blittable-types"></a>Tipos blittable e não blittable
A maioria dos tipos de dados tem uma representação comum na memória gerenciada e não gerenciada e não exige manipulação especial pelo marshaler de interoperabilidade. Esses tipos são chamados *tipos blittable* porque não exigem conversão quando são passados entre o código gerenciado e não gerenciado.  
  
 Estruturas retornadas de chamadas de invocação de plataforma devem ser tipos blittable. A invocação de plataforma não dá suporte a estruturas não blittable como tipos de retorno.  
  
 Os seguintes tipos do namespace <xref:System> são tipos blittable:  
  
- <xref:System.Byte?displayProperty=nameWithType>  
  
- <xref:System.SByte?displayProperty=nameWithType>  
  
- <xref:System.Int16?displayProperty=nameWithType>  
  
- <xref:System.UInt16?displayProperty=nameWithType>  
  
- <xref:System.Int32?displayProperty=nameWithType>  
  
- <xref:System.UInt32?displayProperty=nameWithType>  
  
- <xref:System.Int64?displayProperty=nameWithType>  
  
- <xref:System.UInt64?displayProperty=nameWithType>  
  
- <xref:System.IntPtr?displayProperty=nameWithType>  
  
- <xref:System.UIntPtr?displayProperty=nameWithType>  
  
- <xref:System.Single?displayProperty=nameWithType>  
  
- <xref:System.Double?displayProperty=nameWithType>  
  
 Os seguintes tipos complexos também são tipos blittable:  
  
- Matrizes unidimensionais de tipos blittable, como uma matriz de inteiros. No entanto, um tipo que contém uma matriz variável de tipos blittable não é blittable em si.  
  
- Tipos de valor formatados que contêm somente tipos blittable (e classes, se elas tiverem o marshaling realizado como tipos formatados). Para obter mais informações sobre os tipos de valor formatados, consulte [Marshaling padrão para tipos de valor](default-marshaling-behavior.md#default-marshaling-for-value-types).  
  
 Referências de objeto não são blittable. Isso inclui uma matriz de referências a objetos que são blittable por si mesmos. Por exemplo, é possível definir uma estrutura blittable, mas não é possível definir um tipo blittable que contém uma matriz de referências a essas estruturas.  
  
 Como uma otimização, as matrizes de tipos blittable e as classes que contêm somente membros blittable são [fixadas](copying-and-pinning.md), em vez de copiadas durante o marshaling. Esses tipos podem parecer como se tivessem o marshaling realizado como parâmetros de Entrada/Saída quando o chamador e o receptor estão no mesmo apartment. No entanto, esses tipos, na verdade, têm o marshaling realizado como parâmetros de Entrada e é necessário aplicar os atributos <xref:System.Runtime.InteropServices.InAttribute> e <xref:System.Runtime.InteropServices.OutAttribute> de você desejar realizar marshaling do argumento como um parâmetro de Entrada/Saída.  
  
 Alguns tipos de dados gerenciados exigem uma representação diferente em um ambiente não gerenciado. Esses tipos de dados não blittable devem ser convertidos em um formato que pode ter o marshaling realizado. Por exemplo, cadeias de caracteres gerenciadas não são tipos blittable porque devem ser convertidas em objetos de cadeia de caracteres antes que possam ter o marshaling realizado.  
  
 A tabela a seguir lista tipos não blittable do namespace <xref:System>. [Representantes](default-marshaling-behavior.md#default-marshaling-for-delegates), que são estruturas de dados que se referem a um método estático ou a uma instância de classe, também não são blittable.  
  
|Tipo não bittable|Descrição|  
|-------------------------|-----------------|  
|[System.Array](default-marshaling-for-arrays.md)|Converte em uma matriz C-style ou em um `SAFEARRAY`.|  
|[System.Boolean](https://docs.microsoft.com/previous-versions/dotnet/netframework-4.0/t2t3725f(v=vs.100))|Converte em um valor de 1, 2 ou 4 bytes com `true` como 1 ou -1.|  
|[System. Char](https://docs.microsoft.com/previous-versions/dotnet/netframework-4.0/6tyybbf2(v=vs.100))|Converte em um caractere Unicode ou ANSI.|  
|[System.Class](https://docs.microsoft.com/previous-versions/dotnet/netframework-4.0/s0968xy8(v=vs.100))|Converte em uma interface de classe.|  
|[System. Object](default-marshaling-for-objects.md)|Converte em uma variante ou uma interface.|  
|[System.Mdarray](default-marshaling-for-arrays.md)|Converte em uma matriz C-style ou em um `SAFEARRAY`.|  
|[System.String](default-marshaling-for-strings.md)|Converte em uma cadeia de caracteres que termina em uma referência nula ou em um BSTR.|  
|[System.Valuetype](https://docs.microsoft.com/previous-versions/dotnet/netframework-4.0/0t2cwe11(v=vs.100))|Converte em uma estrutura com um layout de memória fixo.|  
|[System.Szarray](default-marshaling-for-arrays.md)|Converte em uma matriz C-style ou em um `SAFEARRAY`.|  
  
 Há suporte para tipos de objeto e de classe apenas na interoperabilidade COM. Para tipos correspondentes no Visual Basic, C# e C++, consulte a [Visão geral da biblioteca de classes](../../standard/class-library-overview.md).  
  
## <a name="see-also"></a>Consulte também

- [Comportamento de marshaling padrão](default-marshaling-behavior.md)
