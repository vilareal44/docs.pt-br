---
title: Elemento <NetFx45_CultureAwareComparerGetHashCode_LongStrings>
ms.date: 03/30/2017
helpviewer_keywords:
- NetFx45_CultureAwareComparerGetHashCode_LongStrings element
- <NetFx45_CultureAwareComparerGetHashCode_LongStrings> element
- GetHashCode method
- hash codes, calculating
ms.assetid: 3a5f38d1-ebc8-44de-aaeb-2929f6e6b48f
ms.openlocfilehash: 413eb6c6e61b509135601c65cf045eabd849e8b3
ms.sourcegitcommit: b16c00371ea06398859ecd157defc81301c9070f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/06/2020
ms.locfileid: "74802118"
---
# <a name="netfx45_cultureawarecomparergethashcode_longstrings-element"></a>Elemento \<NetFx45_CultureAwareComparerGetHashCode_LongStrings>

Especifica se o runtime usa uma quantidade fixa de memória para calcular códigos hash para o método <xref:System.StringComparer.GetHashCode%2A?displayProperty=nameWithType>.

[**\<configuration>**](../configuration-element.md)\
&nbsp;&nbsp;[**\<runtime>**](runtime-element.md)\
&nbsp;&nbsp;&nbsp;&nbsp;**\<NetFx45_CultureAwareComparerGetHashCode_LongStrings>**  

## <a name="syntax"></a>Sintaxe

```xml
<NetFx45_CultureAwareComparerGetHashCode_LongStrings enabled="0|1">
```

## <a name="attributes-and-elements"></a>Atributos e elementos

As seções a seguir descrevem atributos, elementos filho e elementos pai.

### <a name="attributes"></a>Atributos

|Atributo|Descrição|
|---------------|-----------------|
|`enabled`|Atributo obrigatório.<br /><br /> Especifica se o Common Language Runtime atribui uma quantidade fixa de memória ao calcular códigos hash.|

## <a name="enabled-attribute"></a>Atributo habilitado

|Valor|Descrição|
|-----------|-----------------|
|0|O Common Language Runtime aloca uma quantidade variável de memória para o método <xref:System.StringComparer.GetHashCode%2A?displayProperty=nameWithType> para calcular códigos hash. Este é o padrão.|
|1|O Common Language Runtime aloca uma quantidade fixa de memória para o método <xref:System.StringComparer.GetHashCode%2A?displayProperty=nameWithType> para calcular códigos hash.|

### <a name="child-elements"></a>Elementos filho

Nenhum.

### <a name="parent-elements"></a>Elementos pai

|Elemento|Descrição|
|-------------|-----------------|
|`configuration`|O elemento raiz em cada arquivo de configuração usado pelos aplicativos do Common Language Runtime e .NET Framework.|
|`runtime`|Contém informações sobre opções de inicialização do runtime.|

## <a name="remarks"></a>Comentários

Por padrão, o Common Language Runtime aloca uma quantidade variável de memória para o método <xref:System.StringComparer.GetHashCode%2A?displayProperty=nameWithType> e um <xref:System.ArgumentException> pode ser disparado quando o método tentar calcular o código hash de cadeias de caracteres muito grandes (com comprimentos superiores a vários milhões de caracteres). Ao adicionar esse elemento a um arquivo de configuração do aplicativo e definir seu atributo `enabled` como "1", você poderá especificar que o método <xref:System.StringComparer.GetHashCode%2A?displayProperty=nameWithType> use um algoritmo alternativo que aloque uma quantidade fixa de memória para o cálculo de códigos hash.

> [!IMPORTANT]
> O `<NetFx45_CultureAwareComparerGetHashCode_LongStrings>` elemento não é usado no Windows 8 e versões posteriores.

## <a name="see-also"></a>Confira também

- <xref:System.StringComparer.GetHashCode%2A?displayProperty=nameWithType>
- [Esquema de configurações do runtime](index.md)
- [Esquema do arquivo de configuração](../index.md)
