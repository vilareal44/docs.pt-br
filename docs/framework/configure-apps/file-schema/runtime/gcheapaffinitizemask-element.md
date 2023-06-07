---
title: Elemento GCHeapAffinitizeMask
ms.date: 11/08/2019
helpviewer_keywords:
- gcHeapCount element
- <gcHeapCount> element
ms.openlocfilehash: 09d6523fb10692dd3617a3827d5bccf112bc632b
ms.sourcegitcommit: b16c00371ea06398859ecd157defc81301c9070f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/06/2020
ms.locfileid: "73978378"
---
# <a name="gcheapaffinitizemask-element"></a>Elemento \<GCHeapAffinitizeMask>

Define a afinidade entre heaps de GC e processadores individuais.

\<configuration>\
&nbsp;&nbsp;\<runtime>\
&nbsp;&nbsp;&nbsp;&nbsp;\<GCHeapAffinitizeMask>

## <a name="syntax"></a>Sintaxe

```xml
<GCHeapAffinitizeMask
   enabled="nnnn"/>
```

## <a name="attributes-and-elements"></a>Atributos e elementos

As seções a seguir descrevem atributos, elementos filho e elementos pai.

### <a name="attributes"></a>Atributos

|Atributo|Descrição|
|---------------|-----------------|
|`enabled`|Atributo obrigatório.<br /><br />Especifica a afinidade entre heaps de GC e processadores individuais. |

#### <a name="enabled-attribute"></a>atributo habilitado

|Valor|Descrição|
|-----------|-----------------|
|`nnnn`|Um valor decimal que forma um bitmask que define a afinidade entre heaps de GC de servidor e processadores individuais. |

### <a name="child-elements"></a>Elementos filho

Nenhum.

### <a name="parent-elements"></a>Elementos pai

|Elemento|Descrição|
|-------------|-----------------|
|`configuration`|O elemento raiz em cada arquivo de configuração usado pelos aplicativos do Common Language Runtime e .NET Framework.|
|`runtime`|Contém informações sobre associação do assembly e coleta de lixo.|

## <a name="remarks"></a>Comentários

Por padrão, os threads GC do servidor são relacionados com sua respectiva CPU para que haja um heap de GC, um thread GC de servidor e um thread GC de servidor em segundo plano para cada processador. A partir do .NET Framework 4.6.2, você pode usar o elemento **GCHeapAffinitizeMask** para controlar a afinidade entre heaps e processadores do GC do servidor quando o número de heaps é limitado pelo elemento **GCHeapCount** .

**GCHeapAffinitizeMask** normalmente é usado junto com dois outros sinalizadores:

- [GCNoAffinitize](gcnoaffinitize-element.md), que controla se threads GC do servidor/heaps são relacionados com CPUs. O `enabled` atributo do elemento [GCNoAffinitize](gcnoaffinitize-element.md) deve ser `false` (seu valor padrão) para que a configuração **GCHeapAffinitizeMask** seja usada.

- [GCHeapCount](gcheapcount-element.md), que limita o número de heaps usados pelo processo para GC do servidor. Por padrão, há um heap para cada processador.

**nnnn** é uma máscara de bits expressa como um valor decimal. O bit 0 do byte 0 representa o processador 0, o bit 1 do byte 0 representa o processador 1 e assim por diante. Por exemplo:

```xml
<GCHeapAffinitizeMask enabled="1023"/>
```

Um valor de 1023 é 0x3FF ou 0011 1111 1111b. O processo usa 10 processadores, do processador 0 até o processador 9.

## <a name="example"></a>Exemplo

O exemplo a seguir indica que um aplicativo usa GC de servidor com 10 heaps/threads. Como você não quer que esses heaps se sobreponham com heaps de outros aplicativos em execução no sistema, use **GCHeapAffinitizeMask** para especificar que o processo deve usar CPUs de 0 a 9.

```xml
<configuration>
   <runtime>
      <gcServer enabled="true"/>
      <GCHeapCount enabled="10"/>
      <GCHeapAffinitizeMask enabled="1023"/>
   </runtime>
</configuration>
```

## <a name="see-also"></a>Confira também

- <xref:System.Runtime.GCSettings.IsServerGC%2A?displayProperty=nameWithType>
- [Elemento GCNoAffinitize](gcnoaffinitize-element.md)
- [Elemento GCHeapCount](gcheapcount-element.md)
- [Conceitos básicos da coleta de lixo](../../../../standard/garbage-collection/fundamentals.md)
- [Esquema de configurações do runtime](index.md)
- [Esquema do arquivo de configuração](../index.md)
