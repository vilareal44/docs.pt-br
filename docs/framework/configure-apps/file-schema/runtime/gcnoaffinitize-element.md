---
title: Elemento GCNoAffinitize
ms.date: 11/08/2019
helpviewer_keywords:
- gcNoAffinitize element
- <gcNoAffinitize> element
ms.openlocfilehash: 16d6e5adefe2b632d7251669650058d7df7cea70
ms.sourcegitcommit: b16c00371ea06398859ecd157defc81301c9070f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/06/2020
ms.locfileid: "84004732"
---
# <a name="gcnoaffinitize-element"></a>Elemento \<GCNoAffinitize>

Especifica se os threads GC do servidor relacionar ou não devem ser usados com CPUs.

\<configuration>\
&nbsp;&nbsp;\<runtime>\
&nbsp;&nbsp;&nbsp;&nbsp;\<GCNoAffinitize>

## <a name="syntax"></a>Sintaxe

```xml
<GCNoAffinitize
   enabled="true|false"/>
```

## <a name="attributes-and-elements"></a>Atributos e elementos

As seções a seguir descrevem atributos, elementos filho e elementos pai.

### <a name="attributes"></a>Atributos

|Atributo|Descrição|
|---------------|-----------------|
|`enabled`|Atributo obrigatório.<br /><br />Especifica se segmentos/heaps GC do servidor são relacionados com os processadores disponíveis no computador.|

#### <a name="enabled-attribute"></a>atributo habilitado

|Valor|Descrição|
|-----------|-----------------|
|`false`|Threads GC do affinitizes Server com CPUs. Este é o padrão.|
|`true`|Não relacionar threads GC do servidor com CPUs.|

### <a name="child-elements"></a>Elementos filho

Nenhum.

### <a name="parent-elements"></a>Elementos pai

|Elemento|Descrição|
|-------------|-----------------|
|`configuration`|O elemento raiz em cada arquivo de configuração usado pelos aplicativos do Common Language Runtime e .NET Framework.|
|`runtime`|Contém informações sobre associação do assembly e coleta de lixo.|

## <a name="remarks"></a>Comentários

Por padrão, os threads GC do servidor são relacionados com suas respectivas CPUs. Cada um dos processadores disponíveis do sistema tem seu próprio heap e thread de GC. Normalmente, essa é a configuração preferida, pois otimiza o uso do cache. Começando com .NET Framework 4.6.2, definindo o atributo **GCNoAffinitize** do elemento GCNoAffinitize `enabled` como `true` , você pode especificar que os threads e as CPUs do servidor GC não devem estar rigidamente acoplados.

Você pode especificar o elemento de configuração **GCNoAffinitize** sozinho para não relacionar THREADs GC de servidor com CPUs. Você também pode usá-lo junto com o elemento [GCHeapCount](gcheapcount-element.md) para controlar o número de heaps e threads de GC usados por um aplicativo.

Se o `enabled` atributo do elemento **GCNoAffinitize** for `false` (seu valor padrão), você também poderá usar o elemento [GCHeapCount](gcheapcount-element.md) para especificar o número de threads e heaps do GC, juntamente com o elemento [GCHeapAffinitizeMask](gcheapaffinitizemask-element.md) para especificar os processadores aos quais os threads e heaps do GC são relacionados.

## <a name="example"></a>Exemplo

O exemplo a seguir não relacionar os threads GC do servidor:

```xml
<configuration>
   <runtime>
      <gcServer enabled="true"/>
      <GCNoAffinitize enabled="true"/>
   </runtime>
</configuration>
```

O exemplo a seguir não relacionar threads GC do servidor e limita o número de heaps/threads do GC a 10:

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
- [Elemento GCHeapAffinitizeMask](gcheapaffinitizemask-element.md)
- [Elemento GCHeapCount](gcheapcount-element.md)
- [Conceitos básicos da coleta de lixo](../../../../standard/garbage-collection/fundamentals.md)
- [Esquema de configurações do runtime](index.md)
- [Esquema do arquivo de configuração](../index.md)
