---
title: Elemento gcServer
ms.date: 03/30/2017
f1_keywords:
- http://schemas.microsoft.com/.NetConfiguration/v2.0#configuration/runtime/gcServer
- http://schemas.microsoft.com/.NetConfiguration/v2.0#gcServer
helpviewer_keywords:
- gcServer element
- <gcServer> element
ms.assetid: 8d25b80e-2581-4803-bd87-a59528e3cb03
ms.openlocfilehash: 8eab5e36bab90510aff4f1a3e15328197ac59ed7
ms.sourcegitcommit: b16c00371ea06398859ecd157defc81301c9070f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/06/2020
ms.locfileid: "73968941"
---
# <a name="gcserver-element"></a>Elemento \<gcServer>

Especifica se o Common Language Runtime executa a coleta de lixo do servidor.

[\<configuration>](../configuration-element.md)\
&nbsp;&nbsp;[\<runtime>](runtime-element.md)\
&nbsp;&nbsp;&nbsp;&nbsp;\<gcServer>

## <a name="syntax"></a>Sintaxe

```xml
<gcServer
   enabled="true|false"/>
```

## <a name="attributes-and-elements"></a>Atributos e elementos

As seções a seguir descrevem atributos, elementos filho e elementos pai.

### <a name="attributes"></a>Atributos

|Atributo|Descrição|
|---------------|-----------------|
|`enabled`|Atributo obrigatório.<br /><br />Especifica se o tempo de execução executa a coleta de lixo do servidor.|

#### <a name="enabled-attribute"></a>atributo habilitado

|Valor|Descrição|
|-----------|-----------------|
|`false`|Não executa a coleta de lixo do servidor. Este é o padrão.|
|`true`|Executa a coleta de lixo do servidor.|

### <a name="child-elements"></a>Elementos filho

Nenhum.

### <a name="parent-elements"></a>Elementos pai

|Elemento|Descrição|
|-------------|-----------------|
|`configuration`|O elemento raiz em cada arquivo de configuração usado pelos aplicativos do Common Language Runtime e .NET Framework.|
|`runtime`|Contém informações sobre associação do assembly e coleta de lixo.|

## <a name="remarks"></a>Comentários

O Common Language Runtime (CLR) dá suporte a dois tipos de coleta de lixo: coleta de lixo da estação de trabalho, que está disponível em todos os sistemas e coleta de lixo do servidor, que está disponível em sistemas de multiprocessadores. Use o elemento **gcServer** para controlar o tipo de coleta de lixo que o CLR executa. Use a <xref:System.Runtime.GCSettings.IsServerGC%2A?displayProperty=nameWithType> propriedade para determinar se a coleta de lixo do servidor está habilitada.

Para computadores com um único processador, a coleta de lixo da estação de trabalho padrão deve ser a opção mais rápida. A estação de trabalho ou o servidor pode ser usado para computadores com dois processadores. A coleta de lixo do servidor deve ser a opção mais rápida para mais de dois processadores. Normalmente, os sistemas de servidores multiprocessadores desabilitam o GC do servidor e usam o GC da estação de trabalho quando muitas instâncias de um aplicativo de servidor são executadas no mesmo computador.

Esse elemento só pode ser usado no arquivo de configuração do aplicativo; Ela será ignorada se estiver no arquivo de configuração da máquina.

> [!NOTE]
> No .NET Framework 4 e versões anteriores, a coleta de lixo simultânea não está disponível quando a coleta de lixo do servidor está habilitada. A partir do .NET Framework 4,5, a coleta de lixo do servidor é simultânea. Para usar a coleta de lixo do servidor não simultânea, defina o elemento **gcServer** como `true` e o [elemento gcConcurrent](gcconcurrent-element.md) como `false` .

A partir do .NET Framework 4.6.2, você também pode usar os seguintes elementos para configurar o GC do servidor:

- [GCNoAffinitize](gcnoaffinitize-element.md), que especifica se há uma afinidade entre os processadores e heaps do GC do servidor. Por padrão, há um heap GC de servidor para cada processador.

- [GCHeapCount](gcheapcount-element.md), que limita o número de heaps usados por um processo.

- [GCHeapAffinitizeMask](gcheapaffinitizemask-element.md), que define a afinidade entre as pilhas de GC do servidor disponíveis e os processadores individuais.

## <a name="example"></a>Exemplo

O exemplo a seguir habilita a coleta de lixo do servidor:

```xml
<configuration>
   <runtime>
      <gcServer enabled="true"/>
   </runtime>
</configuration>
```

## <a name="see-also"></a>Confira também

- <xref:System.Runtime.GCSettings.IsServerGC%2A?displayProperty=nameWithType>
- [Esquema de configurações do runtime](index.md)
- [Esquema de arquivos de configuração](../index.md)
- [Para desabilitar a coleta de lixo simultânea](gcconcurrent-element.md#to-disable-background-garbage-collection)
