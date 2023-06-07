---
title: Elemento gcConcurrent
ms.date: 03/30/2017
f1_keywords:
- http://schemas.microsoft.com/.NetConfiguration/v2.0#configuration/runtime/gcConcurrent
- http://schemas.microsoft.com/.NetConfiguration/v2.0#gcConcurrent
helpviewer_keywords:
- container tags, <gcConcurrent> element
- gcConcurrent element
- <gcConcurrent> element
ms.assetid: 503f55ba-26ed-45ac-a2ea-caf994da04cd
ms.openlocfilehash: 249518ae7477d284d50f9010757db83b7752c657
ms.sourcegitcommit: b16c00371ea06398859ecd157defc81301c9070f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/06/2020
ms.locfileid: "82102899"
---
# <a name="gcconcurrent-element"></a>Elemento \<gcConcurrent>

Especifica se o Common Language Runtime executa a coleta de lixo em um thread separado.

[\<configuration>](../configuration-element.md)\
&nbsp;&nbsp;[\<runtime>](runtime-element.md)\
&nbsp;&nbsp;&nbsp;&nbsp;\<gcConcurrent>

## <a name="syntax"></a>Sintaxe

```xml
<gcConcurrent
   enabled="true|false"/>
```

## <a name="attributes-and-elements"></a>Atributos e elementos

As seções a seguir descrevem atributos, elementos filho e elementos pai.

### <a name="attributes"></a>Atributos

|Atributo|Descrição|
|---------------|-----------------|
|`enabled`|Atributo obrigatório.<br /><br />Especifica se o runtime executa a coleta de lixo simultaneamente.|

#### <a name="enabled-attribute"></a>atributo habilitado

|Valor|Descrição|
|-----------|-----------------|
|`false`|Não executa a coleta de lixo simultaneamente.|
|`true`|Executa a coleta de lixo simultaneamente. Este é o padrão.|

### <a name="child-elements"></a>Elementos filho

Nenhum.

### <a name="parent-elements"></a>Elementos pai

|Elemento|Descrição|
|-------------|-----------------|
|`configuration`|O elemento raiz em cada arquivo de configuração usado pelos aplicativos do Common Language Runtime e .NET Framework.|
|`runtime`|Contém informações sobre associação do assembly e coleta de lixo.|

## <a name="remarks"></a>Comentários

Antes do .NET Framework 4, a coleta de lixo da estação de trabalho oferecia suporte à coleta de lixo simultânea, que realizou a coleta de lixo em segundo plano em um thread separado. No .NET Framework 4, a coleta de lixo simultânea foi substituída pelo GC em segundo plano, que também executa a coleta de lixo em segundo plano em um thread separado. A partir do .NET Framework 4,5, a coleção em segundo plano tornou-se disponível na coleta de lixo do servidor. O elemento **gcConcurrent** controla se o tempo de execução executa uma coleta de lixo simultânea ou em segundo plano, se estiver disponível, ou se ele executará a coleta de lixo em primeiro plano.

### <a name="to-disable-background-garbage-collection"></a>Para desabilitar a coleta de lixo em segundo plano

> [!WARNING]
> A partir do .NET Framework 4, a coleta de lixo simultânea é substituída pela coleta de lixo em segundo plano. Os termos *simultâneos* e de *plano de fundo* são usados de maneira intercambiável na documentação do .NET Framework. Para desabilitar a coleta de lixo em segundo plano, use o elemento **gcConcurrent** , conforme discutido neste artigo.

Por padrão, o tempo de execução usa a coleta de lixo simultânea ou em segundo plano, que é otimizada para latência. Se seu aplicativo envolver uma interação pesada do usuário, deixe a coleta de lixo simultânea habilitada para minimizar o tempo de pausa do aplicativo para executar a coleta de lixo. Se você definir o `enabled` atributo do elemento **gcConcurrent** como `false` , o tempo de execução usará a coleta de lixo não simultânea, que é otimizada para taxa de transferência.

O seguinte arquivo de configuração desabilita a coleta de lixo em segundo plano:

```xml
<configuration>
   <runtime>
      <gcConcurrent enabled="false"/>
   </runtime>
</configuration>
```

Se houver uma configuração **gcConcurrentSetting** no arquivo de configuração da máquina, ele definirá o valor padrão para todos os aplicativos .NET Framework. A configuração do arquivo de configuração do computador substitui a configuração do arquivo de configuração do aplicativo.

Para obter mais informações sobre a coleta de lixo simultânea e em segundo plano, consulte [coleta de lixo em segundo plano](../../../../standard/garbage-collection/background-gc.md).

## <a name="example"></a>Exemplo

O exemplo a seguir habilita a coleta de lixo em segundo plano:

```xml
<configuration>
   <runtime>
      <gcConcurrent enabled="true"/>
   </runtime>
</configuration>
```

## <a name="see-also"></a>Confira também

- [Esquema de configurações do runtime](index.md)
- [Esquema de arquivos de configuração](../index.md)
- [Noções básicas sobre a coleta de lixo](../../../../standard/garbage-collection/fundamentals.md)
