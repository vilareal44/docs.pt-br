---
title: Elemento <performanceCounters>
ms.date: 03/30/2017
f1_keywords:
- http://schemas.microsoft.com/.NetConfiguration/v2.0#configuration/system.diagnostics/performanceCounters
- http://schemas.microsoft.com/.NetConfiguration/v2.0#performanceCounters
helpviewer_keywords:
- performanceCounters element
- <performanceCounters> element
ms.assetid: a71f605b-c7d9-4501-a5c3-abcbb964a43f
ms.openlocfilehash: f52fdb2d5b0b7911de63f96663e70735d2f2496c
ms.sourcegitcommit: b16c00371ea06398859ecd157defc81301c9070f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/06/2020
ms.locfileid: "71697148"
---
# <a name="performancecounters-element"></a>Elemento \<performanceCounters>

Especifica o tamanho da memória global compartilhada por contadores de desempenho.

[**\<configuration>**](../configuration-element.md)  
&nbsp;&nbsp;[**\<system.diagnostics>**](system-diagnostics-element.md)  
&nbsp;&nbsp;&nbsp;&nbsp;**\<performanceCounters>**  

## <a name="syntax"></a>Sintaxe

```xml
<performanceCounters filemappingsize="524288" />
```

## <a name="attributes-and-elements"></a>Atributos e elementos

As seções a seguir descrevem atributos, elementos filho e elementos pai.

### <a name="attributes"></a>Atributos

|Atributo|Descrição|
|---------------|-----------------|
|filemappingsize|Atributo obrigatório.<br /><br /> Especifica o tamanho, em bytes, da memória global compartilhada pelos contadores de desempenho. O padrão é 524288.|

### <a name="child-elements"></a>Elementos filho

Nenhum.

### <a name="parent-elements"></a>Elementos pai

|Elemento|Descrição|
|-------------|-----------------|
|`Configuration`|O elemento raiz em cada arquivo de configuração usado pelos aplicativos do Common Language Runtime e .NET Framework.|
|`system.diagnostics`|Especifica o elemento raiz para a seção de configuração ASP.NET.|

## <a name="remarks"></a>Comentários

Os contadores de desempenho usam um arquivo mapeado de memória ou memória compartilhada para publicar dados de desempenho.  O tamanho da memória compartilhada determina quantas instâncias podem ser usadas de uma vez.  Há dois tipos de memória compartilhada: memória compartilhada global e memória compartilhada separada.  A memória compartilhada global é usada por todas as categorias de contador de desempenho instaladas com as versões 1,0 ou 1,1 do .NET Framework.  As categorias de contador de desempenho instaladas com o .NET Framework versão 2,0 usam memória compartilhada separada, com cada categoria de contador de desempenho com sua própria memória.

O tamanho da memória compartilhada global só pode ser definido com um arquivo de configuração.  O tamanho padrão é 524.288 bSim, o tamanho máximo é de 33.554.432 bytes e o tamanho mínimo é de 32.768 bytes.  Como a memória compartilhada global é compartilhada por todos os processos e categorias, o primeiro criador especifica o tamanho.  Se você definir o tamanho em seu arquivo de configuração de aplicativo, esse tamanho será usado somente se seu aplicativo for o primeiro aplicativo que faz com que os contadores de desempenho sejam executados.  Portanto, o local correto para especificar o `filemappingsize` valor é o arquivo Machine. config.  A memória na memória compartilhada global não pode ser liberada por contadores de desempenho individuais, portanto, eventualmente, a memória compartilhada global será esgotada se um grande número de instâncias de contador de desempenho com nomes diferentes for criado.

Para o tamanho da memória compartilhada separada, o valor DWORD FileMappingSize na chave do registro HKEY_LOCAL_MACHINE \SYSTEM\CurrentControlSet\Services \\ *\<category name>* \Performance é referenciado primeiro, seguido pelo valor especificado para a memória compartilhada global no arquivo de configuração. Se o valor de FileMappingSize não existir, o tamanho da memória compartilhada separada será definido como um quarto (1/4) a configuração global no arquivo de configuração.

## <a name="see-also"></a>Confira também

- <xref:System.Diagnostics.PerformanceCounter>
- <xref:System.Diagnostics.PerformanceCounterCategory>
- <xref:System.Diagnostics.PerformanceCounter.InstanceLifetime%2A>
- <xref:System.Diagnostics.PerformanceCounterInstanceLifetime>
