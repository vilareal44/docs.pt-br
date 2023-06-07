---
title: Elemento GCLOHThreshold
ms.date: 11/20/2019
helpviewer_keywords:
- GCLOHThreshold element
- <GCLOHThreshold> element
ms.openlocfilehash: d72dc9d27984f60dfb6296217263ce8b176093c6
ms.sourcegitcommit: b16c00371ea06398859ecd157defc81301c9070f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/06/2020
ms.locfileid: "74451216"
---
# <a name="gclohthreshold-element"></a>Elemento GCLOHThreshold

Especifica o tamanho do limite, em bytes, que faz com que o coletor de lixo coloque objetos na LOH (Large Object heap).

[\<configuration>](../configuration-element.md)\
&nbsp;&nbsp;[\<runtime>](runtime-element.md)\
&nbsp;&nbsp;&nbsp;&nbsp;\<GCLOHThreshold>

## <a name="syntax"></a>Sintaxe

```xml
<GCLOHThreshold
   enabled="nnnn"/>
```

## <a name="attributes"></a>Atributos

|Atributo|Descrição|
|---------------|-----------------|
|`enabled`|Atributo obrigatório.<br /><br />Especifica o tamanho do limite que faz com que os objetos vá para a heap de objeto grande.|

### <a name="enabled-attribute"></a>atributo habilitado

|Valor|Descrição|
|-----------|-----------------|
|`nnnn`|O tamanho do limite, em bytes, que faz com que os objetos vá para a heap de objeto grande.|

## <a name="child-elements"></a>Elementos filho

Nenhum.

## <a name="parent-elements"></a>Elementos pai

|Elemento|Descrição|
|-------------|-----------------|
|`configuration`|O elemento raiz em cada arquivo de configuração usado pelos aplicativos do Common Language Runtime e .NET Framework.|
|`runtime`|Contém informações sobre associação do assembly e coleta de lixo.|

## <a name="remarks"></a>Comentários

Essa configuração foi introduzida no .NET Framework 4,8.

## <a name="see-also"></a>Confira também

- [Esquema de configurações de tempo de execução](index.md)
- [Esquema do arquivo de configuração](../index.md)
- [Conceitos básicos da coleta de lixo](../../../../standard/garbage-collection/fundamentals.md)
- [Opções de configuração de tempo de execução do NET Core para GC](../../../../core/run-time-config/garbage-collector.md)
