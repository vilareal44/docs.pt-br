---
title: Elemento GCHeapCount
ms.date: 11/08/2019
helpviewer_keywords:
- gcHeapCount element
- <gcHeapCount> element
ms.openlocfilehash: 3d6cac4185af182758cb82e6bfd9d96ed24869b4
ms.sourcegitcommit: b16c00371ea06398859ecd157defc81301c9070f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/06/2020
ms.locfileid: "74283079"
---
# <a name="gcheapcount-element"></a>Elemento \<GCHeapCount>

Especifica o número de heaps/threads a serem usados para a coleta de lixo do servidor.

\<configuration>\
&nbsp;&nbsp;\<runtime>\
&nbsp;&nbsp;&nbsp;&nbsp;\<GCHeapCount>

## <a name="syntax"></a>Sintaxe

```xml
<GCHeapCount
   enabled="nn"/>
```

## <a name="attributes-and-elements"></a>Atributos e elementos

As seções a seguir descrevem atributos, elementos filho e elementos pai.

### <a name="attributes"></a>Atributos

|Atributo|Descrição|
|---------------|-----------------|
|`enabled`|Atributo obrigatório.<br /><br />Especifica o número de heaps a ser usado para a coleta de lixo do servidor. O número real de heaps é o mínimo do número de heaps que você especifica e o número de processadores que seu processo tem permissão para usar. |

#### <a name="enabled-attribute"></a>atributo habilitado

|Valor|Descrição|
|-----------|-----------------|
|`nn`|O número de heaps a ser usado para o GC do servidor.|

### <a name="child-elements"></a>Elementos filho

Nenhum.

### <a name="parent-elements"></a>Elementos pai

|Elemento|Descrição|
|-------------|-----------------|
|`configuration`|O elemento raiz em cada arquivo de configuração usado pelos aplicativos do Common Language Runtime e .NET Framework.|
|`runtime`|Contém informações sobre associação do assembly e coleta de lixo.|

## <a name="remarks"></a>Comentários

Por padrão, os threads GC do servidor são relacionados com sua respectiva CPU para que haja um heap de GC, um thread GC de servidor e um thread GC de servidor em segundo plano para cada processador. Começando com .NET Framework 4.6.2, você pode usar o elemento **GCHeapCount** para limitar o número de heaps usados pelo seu aplicativo para GC de servidor. Limitar o número de heaps usado para o servidor GC é particularmente útil para sistemas que executam várias instâncias de um aplicativo de servidor.

**GCHeapCount** normalmente é usado junto com dois outros sinalizadores:

- [GCNoAffinitize](gcnoaffinitize-element.md), que controla se threads GC do servidor/heaps são relacionados com CPUs.

- [GCHeapAffinitizeMask](gcheapaffinitizemask-element.md), que controla a afinidade de threads do GC/heaps com CPUs.

Se **GCHeapCount** for definido e **GCNoAffinitize** estiver desabilitado (sua configuração padrão), haverá uma afinidade entre os threads/pilhas do *NN* GC e os primeiros processadores *NN* . Você pode usar o elemento **GCHeapAffinitizeMask** para especificar quais processadores são usados pelos heaps de GC do servidor do processo. Caso contrário, se vários processos de servidor estiverem em execução em um sistema, o uso do processador será sobreposto.

Se **GCHeapCount** for definido e **GCNoAffinitize** estiver habilitado, o coletor de lixo limita o número de processadores usados para GC do servidor, mas não relacionar heaps e processadores do GC.

## <a name="example"></a>Exemplo

O exemplo a seguir indica que um aplicativo usa GC de servidor com 10 heaps/threads. Como você não quer que esses heaps se sobreponham com heaps de outros aplicativos em execução no sistema, use o **GCHeapAffinitizeMask** para especificar que o processo deve usar CPUs de 0 a 9.

```xml
<configuration>
   <runtime>
      <gcServer enabled="true"/>
      <GCHeapCount enabled="10"/>
      <GCHeapAffinitizeMask enabled="1023"/>
   </runtime>
</configuration>
```

O exemplo a seguir não relacionar threads GC do servidor e limita o número de heaps/threads do GC a 10.

```xml
<configuration>
   <runtime>
      <gcServer enabled="true"/>
      <GCHeapCount enabled="10"/>
      <GCNoAffinitize enabled="true"/>
   </runtime>
</configuration>
```

## <a name="see-also"></a>Confira também

- <xref:System.Runtime.GCSettings.IsServerGC%2A?displayProperty=nameWithType>
- [Elemento GCNoAffinitize](gcnoaffinitize-element.md)
- [Elemento GCHeapAffinitizeMask](gcheapaffinitizemask-element.md)
- [Conceitos básicos da coleta de lixo](../../../../standard/garbage-collection/fundamentals.md)
- [Esquema de configurações do runtime](index.md)
- [Esquema do arquivo de configuração](../index.md)
