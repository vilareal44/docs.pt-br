---
title: Definições de configuração do coletor de lixo
description: Saiba mais sobre as configurações de tempo de execução para configurar como o coletor de lixo gerencia a memória para aplicativos .NET Core.
ms.date: 07/10/2020
ms.topic: reference
ms.openlocfilehash: 91d155b638c7e69b3d2c0216266a7c0c0410db4c
ms.sourcegitcommit: ef50c99928183a0bba75e07b9f22895cd4c480f8
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/07/2020
ms.locfileid: "87915988"
---
# <a name="run-time-configuration-options-for-garbage-collection"></a>Opções de configuração de tempo de execução para coleta de lixo

Esta página contém informações sobre as configurações de coletor de lixo (GC) que podem ser alteradas em tempo de execução. Se você estiver tentando atingir o desempenho máximo de um aplicativo em execução, considere o uso dessas configurações. No entanto, os padrões fornecem um desempenho ideal para a maioria dos aplicativos em situações típicas.

As configurações são organizadas em grupos nesta página. As configurações em cada grupo são comumente usadas em conjunto entre si para obter um resultado específico.

> [!NOTE]
>
> - Essas configurações também podem ser alteradas dinamicamente pelo aplicativo enquanto ele está em execução, portanto, as configurações de tempo de execução definidas podem ser substituídas.
> - Algumas configurações, como [nível de latência](../../standard/garbage-collection/latency.md), normalmente são definidas somente por meio da API em tempo de design. Essas configurações são omitidas desta página.
> - Para valores numéricos, use a notação decimal para configurações no *runtimeconfig.jsem* notação de arquivo e hexadecimal para configurações de variável de ambiente. Para valores hexadecimais, você pode especificá-los com ou sem o prefixo "0x".

## <a name="flavors-of-garbage-collection"></a>Tipos de coleta de lixo

Os dois tipos principais de coleta de lixo são estação de trabalho GC e GC de servidor. Para obter mais informações sobre as diferenças entre as duas, consulte [estação de trabalho e coleta de lixo do servidor](../../standard/garbage-collection/workstation-server-gc.md).

Os subtipos de coleta de lixo são em segundo plano e não simultâneos.

Use as seguintes configurações para selecionar tipos de coleta de lixo:

- [Estação de trabalho vs. servidor GC](#workstation-vs-server)
- [GC em segundo plano](#background-gc)

### <a name="workstation-vs-server"></a>Estação de trabalho versus servidor

- Define se o aplicativo usa a coleta de lixo da estação de trabalho ou a coleta de lixo do servidor.
- Padrão: coleta de lixo da estação de trabalho. Isso é equivalente a definir o valor como `false` .

| | Nome da configuração | Valores | Versão introduzida |
| - | - | - | - |
| **runtimeconfig.jsem** | `System.GC.Server` | `false`-Estação de trabalho<br/>`true`-servidor | .NET Core 1.0 |
| **Propriedade do MSBuild** | `ServerGarbageCollection` | `false`-Estação de trabalho<br/>`true`-servidor | .NET Core 1.0 |
| **Variável de ambiente** | `COMPlus_gcServer` | `0`-Estação de trabalho<br/>`1`-servidor | .NET Core 1.0 |
| **app.config para .NET Framework** | [GCServer](../../framework/configure-apps/file-schema/runtime/gcserver-element.md) | `false`-Estação de trabalho<br/>`true`-servidor |  |

#### <a name="examples"></a>Exemplos

*runtimeconfig.jsno* arquivo:

```json
{
   "runtimeOptions": {
      "configProperties": {
         "System.GC.Server": true
      }
   }
}
```

Arquivo de projeto:

```xml
<Project Sdk="Microsoft.NET.Sdk">

  <PropertyGroup>
    <ServerGarbageCollection>true</ServerGarbageCollection>
  </PropertyGroup>

</Project>
```

### <a name="background-gc"></a>GC em segundo plano

- Configura se a coleta de lixo em segundo plano (simultânea) está habilitada.
- Padrão: usar GC em segundo plano. Isso é equivalente a definir o valor como `true` .
- Para obter mais informações, consulte [coleta de lixo em segundo plano](../../standard/garbage-collection/background-gc.md).

| | Nome da configuração | Valores | Versão introduzida |
| - | - | - | - |
| **runtimeconfig.jsem** | `System.GC.Concurrent` | `true`-GC em segundo plano<br/>`false`-GC não simultâneo | .NET Core 1.0 |
| **Propriedade do MSBuild** | `ConcurrentGarbageCollection` | `true`-GC em segundo plano<br/>`false`-GC não simultâneo | .NET Core 1.0 |
| **Variável de ambiente** | `COMPlus_gcConcurrent` | `1`-GC em segundo plano<br/>`0`-GC não simultâneo | .NET Core 1.0 |
| **app.config para .NET Framework** | [gcConcurrent](../../framework/configure-apps/file-schema/runtime/gcconcurrent-element.md) | `true`-GC em segundo plano<br/>`false`-GC não simultâneo |  |

#### <a name="examples"></a>Exemplos

*runtimeconfig.jsno* arquivo:

```json
{
   "runtimeOptions": {
      "configProperties": {
         "System.GC.Concurrent": false
      }
   }
}
```

Arquivo de projeto:

```xml
<Project Sdk="Microsoft.NET.Sdk">

  <PropertyGroup>
    <ConcurrentGarbageCollection>false</ConcurrentGarbageCollection>
  </PropertyGroup>

</Project>
```

## <a name="manage-resource-usage"></a>Gerenciar o uso de recursos

Use as configurações a seguir para gerenciar a memória e o uso do processador do coletor de lixo:

- [Relacionar](#affinitize)
- [Máscara de relacionar](#affinitize-mask)
- [Intervalos de relacionar](#affinitize-ranges)
- [Grupos de CPU](#cpu-groups)
- [Contagem de heaps](#heap-count)
- [Limite de heap](#heap-limit)
- [Percentual de limite de heap](#heap-limit-percent)
- [Porcentagem de memória alta](#high-memory-percent)
- [Limites por objeto-heap](#per-object-heap-limits)
- [Porcentagem de limite por objeto-heap](#per-object-heap-limit-percents)
- [Reter VM](#retain-vm)

Para obter mais informações sobre algumas dessas configurações, consulte o meio-termo entre a entrada de blog do [GC de estação de trabalho e servidor](https://devblogs.microsoft.com/dotnet/middle-ground-between-server-and-workstation-gc/) .

### <a name="heap-count"></a>Contagem de heaps

- Limita o número de heaps criados pelo coletor de lixo.
- Aplica-se somente à coleta de lixo do servidor.
- Se a [afinidade do processador GC](#affinitize) estiver habilitada, que é o padrão, a configuração de contagem de heap affinitizes `n` heaps/threads do GC para os primeiros `n` processadores. (Use a [máscara relacionar](#affinitize-mask) ou as configurações de [intervalos de relacionar](#affinitize-ranges) para especificar exatamente quais processadores relacionar.)
- Se a [afinidade do processador GC](#affinitize) estiver desabilitada, essa configuração limitará o número de heaps do GC.
- Para obter mais informações, consulte o [GCHeapCount comentários](../../framework/configure-apps/file-schema/runtime/gcheapcount-element.md#remarks).

| | Nome da configuração | Valores | Versão introduzida |
| - | - | - | - |
| **runtimeconfig.jsem** | `System.GC.HeapCount` | *valor decimal* | .NET Core 3.0 |
| **Variável de ambiente** | `COMPlus_GCHeapCount` | *valor hexadecimal* | .NET Core 3.0 |
| **app.config para .NET Framework** | [GCHeapCount](../../framework/configure-apps/file-schema/runtime/gcheapcount-element.md) | *valor decimal* | .NET Framework 4.6.2 |

Exemplo:

```json
{
   "runtimeOptions": {
      "configProperties": {
         "System.GC.HeapCount": 16
      }
   }
}
```

> [!TIP]
> Se você estiver definindo a opção em *runtimeconfig.jsem*, especifique um valor decimal. Se você estiver definindo a opção como uma variável de ambiente, especifique um valor hexadecimal. Por exemplo, para limitar o número de heaps a 16, os valores seriam 16 para o arquivo JSON e 0x10 ou 10 para a variável de ambiente.

### <a name="affinitize-mask"></a>Máscara de relacionar

- Especifica os processadores exatos que os threads do coletor de lixo devem usar.
- Se a [afinidade do processador GC](#affinitize) estiver desabilitada, essa configuração será ignorada.
- Aplica-se somente à coleta de lixo do servidor.
- O valor é uma máscara de bits que define os processadores que estão disponíveis para o processo. Por exemplo, um valor decimal de 1023 (ou um valor hexadecimal de 0x3FF ou 3FF se você estiver usando a variável de ambiente) é 0011 1111 1111 em notação binária. Isso especifica que os primeiros 10 processadores devem ser usados. Para especificar os 10 próximos processadores, ou seja, os processadores 10-19, especifique um valor decimal de 1047552 (ou um valor hexadecimal de 0xFFC00 ou FFC00), que é equivalente a um valor binário de 1111 1111 1100 0000 0000.

| | Nome da configuração | Valores | Versão introduzida |
| - | - | - | - |
| **runtimeconfig.jsem** | `System.GC.HeapAffinitizeMask` | *valor decimal* | .NET Core 3.0 |
| **Variável de ambiente** | `COMPlus_GCHeapAffinitizeMask` | *valor hexadecimal* | .NET Core 3.0 |
| **app.config para .NET Framework** | [GCHeapAffinitizeMask](../../framework/configure-apps/file-schema/runtime/gcheapaffinitizemask-element.md) | *valor decimal* | .NET Framework 4.6.2 |

Exemplo:

```json
{
   "runtimeOptions": {
      "configProperties": {
         "System.GC.HeapAffinitizeMask": 1023
      }
   }
}
```

### <a name="affinitize-ranges"></a>Intervalos de relacionar

- Especifica a lista de processadores a serem usados para threads do coletor de lixo.
- Essa configuração é semelhante a [System. GC. HeapAffinitizeMask](#affinitize-mask), exceto pelo fato de que ela permite que você especifique mais de 64 processadores.
- Para sistemas operacionais Windows, Prefixe o número do processador ou o intervalo com o [grupo de CPU](/windows/win32/procthread/processor-groups)correspondente, por exemplo, "0:1-10, 0:12, 1:50-52, 1:70".
- Se a [afinidade do processador GC](#affinitize) estiver desabilitada, essa configuração será ignorada.
- Aplica-se somente à coleta de lixo do servidor.
- Para obter mais informações, consulte [tornando a configuração de CPU melhor para GC em computadores com > 64 CPUs](https://devblogs.microsoft.com/dotnet/making-cpu-configuration-better-for-gc-on-machines-with-64-cpus/) no blog do Maoni Stephens '.

| | Nome da configuração | Valores | Versão introduzida |
| - | - | - | - |
| **runtimeconfig.jsem** | `System.GC.GCHeapAffinitizeRanges` | Lista separada por vírgulas de números de processador ou intervalos de números de processador.<br/>Exemplo de UNIX: "1-10, 12, 50-52, 70"<br/>Exemplo do Windows: "0:1-10, 0:12, 1:50-52, 1:70" | .NET Core 3.0 |
| **Variável de ambiente** | `COMPlus_GCHeapAffinitizeRanges` | Lista separada por vírgulas de números de processador ou intervalos de números de processador.<br/>Exemplo de UNIX: "1-10, 12, 50-52, 70"<br/>Exemplo do Windows: "0:1-10, 0:12, 1:50-52, 1:70" | .NET Core 3.0 |

Exemplo:

```json
{
   "runtimeOptions": {
      "configProperties": {
         "System.GC.GCHeapAffinitizeRanges": "0:1-10,0:12,1:50-52,1:70"
      }
   }
}
```

### <a name="cpu-groups"></a>Grupos de CPU

- Configura se o coletor de lixo usa [grupos de CPU](/windows/win32/procthread/processor-groups) ou não.

  Quando um computador com Windows de 64 bits tem vários grupos de CPU, ou seja, há mais de 64 processadores, a habilitação desse elemento estende a coleta de lixo em todos os grupos de CPU. O coletor de lixo usa todos os núcleos para criar e balancear heaps.

- Aplica-se à coleta de lixo do servidor somente em sistemas de operação do Windows de 64 bits.
- Padrão: o GC não se estende por grupos de CPU. Isso é equivalente a definir o valor como `0` .
- Para obter mais informações, consulte [tornando a configuração de CPU melhor para GC em computadores com > 64 CPUs](https://devblogs.microsoft.com/dotnet/making-cpu-configuration-better-for-gc-on-machines-with-64-cpus/) no blog do Maoni Stephens '.

| | Nome da configuração | Valores | Versão introduzida |
| - | - | - | - |
| **runtimeconfig.jsem** | `System.GC.CpuGroup` | `0`-desabilitado<br/>`1`-habilitado | .NET 5,0 |
| **Variável de ambiente** | `COMPlus_GCCpuGroup` | `0`-desabilitado<br/>`1`-habilitado | .NET Core 1.0 |
| **app.config para .NET Framework** | [GCCpuGroup](../../framework/configure-apps/file-schema/runtime/gccpugroup-element.md) | `false`-desabilitado<br/>`true`-habilitado |  |

> [!NOTE]
> Para configurar o Common Language Runtime (CLR) para também distribuir threads do pool de threads em todos os grupos de CPU, habilite a opção de [elemento Thread_UseAllCpuGroups](../../framework/configure-apps/file-schema/runtime/thread-useallcpugroups-element.md) . Para aplicativos .NET Core, você pode habilitar essa opção definindo o valor da variável de `COMPlus_Thread_UseAllCpuGroups` ambiente como `1` .

### <a name="affinitize"></a>Relacionar

- Especifica se os threads de coleta de lixo devem ser *relacionardos* com processadores. Para relacionar um thread de GC significa que ele só pode ser executado em sua CPU específica. Um heap é criado para cada thread do GC.
- Aplica-se somente à coleta de lixo do servidor.
- Padrão: relacionar os threads de coleta de lixo com processadores. Isso é equivalente a definir o valor como `false` .

| | Nome da configuração | Valores | Versão introduzida |
| - | - | - | - |
| **runtimeconfig.jsem** | `System.GC.NoAffinitize` | `false`-relacionar<br/>`true`-Não relacionar | .NET Core 3.0 |
| **Variável de ambiente** | `COMPlus_GCNoAffinitize` | `0`-relacionar<br/>`1`-Não relacionar | .NET Core 3.0 |
| **app.config para .NET Framework** | [GCNoAffinitize](../../framework/configure-apps/file-schema/runtime/gcnoaffinitize-element.md) | `false`-relacionar<br/>`true`-Não relacionar | .NET Framework 4.6.2 |

Exemplo:

```json
{
   "runtimeOptions": {
      "configProperties": {
         "System.GC.NoAffinitize": true
      }
   }
}
```

### <a name="heap-limit"></a>Limite de heap

- Especifica o tamanho máximo de confirmação, em bytes, para o heap de GC e a escrituração de GC.
- Essa configuração se aplica somente a computadores de 64 bits.
- Essa configuração será ignorada se os [limites por objeto-heap](#per-object-heap-limits) estiverem configurados.
- O valor padrão, que se aplica apenas em determinados casos, é o maior de 20 MB ou 75% do limite de memória no contêiner. O valor padrão se aplica se:

  - O processo está sendo executado dentro de um contêiner que tem um limite de memória especificado.
  - [System. GC. HeapHardLimitPercent](#heap-limit-percent) não está definido.

| | Nome da configuração | Valores | Versão introduzida |
| - | - | - | - |
| **runtimeconfig.jsem** | `System.GC.HeapHardLimit` | *valor decimal* | .NET Core 3.0 |
| **Variável de ambiente** | `COMPlus_GCHeapHardLimit` | *valor hexadecimal* | .NET Core 3.0 |

Exemplo:

```json
{
   "runtimeOptions": {
      "configProperties": {
         "System.GC.HeapHardLimit": 209715200
      }
   }
}
```

> [!TIP]
> Se você estiver definindo a opção em *runtimeconfig.jsem*, especifique um valor decimal. Se você estiver definindo a opção como uma variável de ambiente, especifique um valor hexadecimal. Por exemplo, para especificar um limite rígido de heap de 200 mebibytes (MiB), os valores seriam 209715200 para o arquivo JSON e 0xC800000 ou C800000 para a variável de ambiente.

### <a name="heap-limit-percent"></a>Percentual de limite de heap

- Especifica o uso de heap de GC permitido como uma porcentagem da memória física total.
- Se [System. GC. HeapHardLimit](#heap-limit) também for definido, essa configuração será ignorada.
- Essa configuração se aplica somente a computadores de 64 bits.
- Se o processo estiver sendo executado dentro de um contêiner que tem um limite de memória especificado, a porcentagem será calculada como uma porcentagem desse limite de memória.
- Essa configuração será ignorada se os [limites por objeto-heap](#per-object-heap-limits) estiverem configurados.
- O valor padrão, que se aplica apenas em determinados casos, é o menor de 20 MB ou 75% do limite de memória no contêiner. O valor padrão se aplica se:

  - O processo está sendo executado dentro de um contêiner que tem um limite de memória especificado.
  - [System. GC. HeapHardLimit](#heap-limit) não está definido.

| | Nome da configuração | Valores | Versão introduzida |
| - | - | - | - |
| **runtimeconfig.jsem** | `System.GC.HeapHardLimitPercent` | *valor decimal* | .NET Core 3.0 |
| **Variável de ambiente** | `COMPlus_GCHeapHardLimitPercent` | *valor hexadecimal* | .NET Core 3.0 |

Exemplo:

```json
{
   "runtimeOptions": {
      "configProperties": {
         "System.GC.HeapHardLimitPercent": 30
      }
   }
}
```

> [!TIP]
> Se você estiver definindo a opção em *runtimeconfig.jsem*, especifique um valor decimal. Se você estiver definindo a opção como uma variável de ambiente, especifique um valor hexadecimal. Por exemplo, para limitar o uso de heap a 30%, os valores seriam 30 para o arquivo JSON e 0x1E ou 1E para a variável de ambiente.

### <a name="per-object-heap-limits"></a>Limites por objeto-heap

Você pode especificar o uso de heap permitido do GC em uma base por objeto-heap. Os heaps diferentes são o LOH (heap de objeto grande), SOH (heap de objeto pequeno) e POH (heap de objeto fixado).

- Se você especificar um valor para qualquer uma das `COMPLUS_GCHeapHardLimitSOH` `COMPLUS_GCHeapHardLimitLOH` configurações, ou `COMPLUS_GCHeapHardLimitPOH` , também deverá especificar um valor para `COMPLUS_GCHeapHardLimitSOH` e `COMPLUS_GCHeapHardLimitLOH` . Se você não fizer isso, o tempo de execução falhará ao inicializar.
- O valor padrão de `COMPLUS_GCHeapHardLimitPOH` é 0. `COMPLUS_GCHeapHardLimitSOH`e `COMPLUS_GCHeapHardLimitLOH` não têm valores padrão.

| | Nome da configuração | Valores | Versão introduzida |
| - | - | - | - |
| **runtimeconfig.jsem** | `System.GC.HeapHardLimitSOH` | *valor decimal* | .NET 5,0 |
| **Variável de ambiente** | `COMPLUS_GCHeapHardLimitSOH` | *valor hexadecimal* | .NET 5,0 |

| | Nome da configuração | Valores | Versão introduzida |
| - | - | - | - |
| **runtimeconfig.jsem** | `System.GC.HeapHardLimitLOH` | *valor decimal* | .NET 5,0 |
| **Variável de ambiente** | `COMPLUS_GCHeapHardLimitLOH` | *valor hexadecimal* | .NET 5,0 |

| | Nome da configuração | Valores | Versão introduzida |
| - | - | - | - |
| **runtimeconfig.jsem** | `System.GC.HeapHardLimitPOH` | *valor decimal* | .NET 5,0 |
| **Variável de ambiente** | `COMPLUS_GCHeapHardLimitPOH` | *valor hexadecimal* | .NET 5,0 |

> [!TIP]
> Se você estiver definindo a opção em *runtimeconfig.jsem*, especifique um valor decimal. Se você estiver definindo a opção como uma variável de ambiente, especifique um valor hexadecimal. Por exemplo, para especificar um limite rígido de heap de 200 mebibytes (MiB), os valores seriam 209715200 para o arquivo JSON e 0xC800000 ou C800000 para a variável de ambiente.

### <a name="per-object-heap-limit-percents"></a>Porcentagem de limite por objeto-heap

Você pode especificar o uso de heap permitido do GC em uma base por objeto-heap. Os heaps diferentes são o LOH (heap de objeto grande), SOH (heap de objeto pequeno) e POH (heap de objeto fixado).

- Se você especificar um valor para qualquer uma das `COMPLUS_GCHeapHardLimitSOHPercent` `COMPLUS_GCHeapHardLimitLOHPercent` configurações, ou `COMPLUS_GCHeapHardLimitPOHPercent` , também deverá especificar um valor para `COMPLUS_GCHeapHardLimitSOHPercent` e `COMPLUS_GCHeapHardLimitLOHPercent` . Se você não fizer isso, o tempo de execução falhará ao inicializar.
- Essas configurações serão ignoradas se `COMPLUS_GCHeapHardLimitSOH` , `COMPLUS_GCHeapHardLimitLOH` e `COMPLUS_GCHeapHardLimitPOH` forem especificadas.
- Um valor de 1 significa que o GC usa 1% da memória física total para esse heap de objeto.
- Cada valor deve ser maior que zero e menor que 100. Além disso, a soma dos três valores percentuais deve ser menor que 100. Caso contrário, o tempo de execução não será inicializado.

| | Nome da configuração | Valores | Versão introduzida |
| - | - | - | - |
| **runtimeconfig.jsem** | `System.GC.HeapHardLimitSOHPercent` | *valor decimal* | .NET 5,0 |
| **Variável de ambiente** | `COMPLUS_GCHeapHardLimitSOHPercent` | *valor hexadecimal* | .NET 5,0 |

| | Nome da configuração | Valores | Versão introduzida |
| - | - | - | - |
| **runtimeconfig.jsem** | `System.GC.HeapHardLimitLOHPercent` | *valor decimal* | .NET 5,0 |
| **Variável de ambiente** | `COMPLUS_GCHeapHardLimitLOHPercent` | *valor hexadecimal* | .NET 5,0 |

| | Nome da configuração | Valores | Versão introduzida |
| - | - | - | - |
| **runtimeconfig.jsem** | `System.GC.HeapHardLimitPOHPercent` | *valor decimal* | .NET 5,0 |
| **Variável de ambiente** | `COMPLUS_GCHeapHardLimitPOHPercent` | *valor hexadecimal* | .NET 5,0 |

> [!TIP]
> Se você estiver definindo a opção em *runtimeconfig.jsem*, especifique um valor decimal. Se você estiver definindo a opção como uma variável de ambiente, especifique um valor hexadecimal. Por exemplo, para limitar o uso de heap a 30%, os valores seriam 30 para o arquivo JSON e 0x1E ou 1E para a variável de ambiente.

### <a name="high-memory-percent"></a>Porcentagem de memória alta

A carga de memória é indicada pela porcentagem de memória física em uso. Por padrão, quando a carga de memória física atinge **90%**, a coleta de lixo torna-se mais agressiva sobre a realização de coleções de lixo completas e compactadas para evitar a paginação. Quando a carga de memória está abaixo de 90%, o GC favorece coleções em segundo plano para coleções de lixo completas, que têm pausas mais curtas, mas não reduzem o tamanho total do heap por muito tempo. Em computadores com uma quantidade significativa de memória (80 GB ou mais), o limite de carga padrão é entre 90% e 97%.

O limite de carga de memória alta pode ser ajustado pela `COMPlus_GCHighMemPercent` variável de ambiente ou `System.GC.HighMemoryPercent` definição de configuração JSON. Considere ajustar o limite se você quiser controlar o tamanho do heap. Por exemplo, para o processo dominante em um computador com 64 GB de memória, é razoável que o GC comece a reagir quando há 10% da memória disponível. Mas, para processos menores, por exemplo, um processo que consome apenas 1GB de memória, o GC pode ser executado com menos de 10% da memória disponível. Para esses processos menores, considere definir o limite mais alto. Por outro lado, se você quiser que processos maiores tenham tamanhos de heap menores (mesmo quando houver muita memória física disponível), reduzir esse limite é uma maneira eficaz para que o GC reaja mais cedo para compactar o heap.

> [!NOTE]
> Para processos em execução em um contêiner, o GC considera a memória física com base no limite do contêiner.

| | Nome da configuração | Valores | Versão introduzida |
| - | - | - | - |
| **runtimeconfig.jsem** | `System.GC.HighMemoryPercent` | *valor decimal* | .NET 5,0 |
| **Variável de ambiente** | `COMPlus_GCHighMemPercent` | *valor hexadecimal* | |

> [!TIP]
> Se você estiver definindo a opção em *runtimeconfig.jsem*, especifique um valor decimal. Se você estiver definindo a opção como uma variável de ambiente, especifique um valor hexadecimal. Por exemplo, para definir o limite de memória alto para 75%, os valores seriam 75 para o arquivo JSON e 0x4B ou 4B para a variável de ambiente.

### <a name="retain-vm"></a>Reter VM

- Configura se OS segmentos que devem ser excluídos são colocados em uma lista de espera para uso futuro ou são liberados de volta para o sistema operacional (SO).
- Padrão: libera segmentos de volta para o sistema operacional. Isso é equivalente a definir o valor como `false` .

| | Nome da configuração | Valores | Versão introduzida |
| - | - | - | - |
| **runtimeconfig.jsem** | `System.GC.RetainVM` | `false`-versão para o sistema operacional<br/>`true`-colocar em espera | .NET Core 1.0 |
| **Propriedade do MSBuild** | `RetainVMGarbageCollection` | `false`-versão para o sistema operacional<br/>`true`-colocar em espera | .NET Core 1.0 |
| **Variável de ambiente** | `COMPlus_GCRetainVM` | `0`-versão para o sistema operacional<br/>`1`-colocar em espera | .NET Core 1.0 |

#### <a name="examples"></a>Exemplos

*runtimeconfig.jsno* arquivo:

```json
{
   "runtimeOptions": {
      "configProperties": {
         "System.GC.RetainVM": true
      }
   }
}
```

Arquivo de projeto:

```xml
<Project Sdk="Microsoft.NET.Sdk">

  <PropertyGroup>
    <RetainVMGarbageCollection>true</RetainVMGarbageCollection>
  </PropertyGroup>

</Project>
```

## <a name="large-pages"></a>Páginas grandes

- Especifica se páginas grandes devem ser usadas quando um limite rígido de heap for definido.
- Padrão: não use páginas grandes quando um limite rígido de heap for definido. Isso é equivalente a definir o valor como `0` .
- Essa é uma configuração experimental.

| | Nome da configuração | Valores | Versão introduzida |
| - | - | - | - |
| **runtimeconfig.jsem** | N/D | N/D | N/D |
| **Variável de ambiente** | `COMPlus_GCLargePages` | `0`-desabilitado<br/>`1`-habilitado | .NET Core 3.0 |

## <a name="allow-large-objects"></a>Permitir objetos grandes

- Configura o suporte ao coletor de lixo em plataformas de 64 bits para matrizes com mais de 2 gigabytes (GB) no tamanho total.
- Padrão: o GC dá suporte a matrizes maiores que 2 GB. Isso é equivalente a definir o valor como `1` .
- Essa opção pode se tornar obsoleta em uma versão futura do .NET.

| | Nome da configuração | Valores | Versão introduzida |
| - | - | - | - |
| **runtimeconfig.jsem** | N/D | N/D | N/D |
| **Variável de ambiente** | `COMPlus_gcAllowVeryLargeObjects` | `1`-habilitado<br/> `0`-desabilitado | .NET Core 1.0 |
| **app.config para .NET Framework** | [gcAllowVeryLargeObjects](../../framework/configure-apps/file-schema/runtime/gcallowverylargeobjects-element.md) | `1`-habilitado<br/> `0`-desabilitado | .NET Framework 4.5 |

## <a name="large-object-heap-threshold"></a>Limite de heap de objeto grande

- Especifica o tamanho do limite, em bytes, que faz com que os objetos vá para o LOH (heap de objeto grande).
- O limite padrão é de 85.000 bytes.
- O valor especificado deve ser maior que o limite padrão.

| | Nome da configuração | Valores | Versão introduzida |
| - | - | - | - |
| **runtimeconfig.jsem** | `System.GC.LOHThreshold` | *valor decimal* | .NET Core 1.0 |
| **Variável de ambiente** | `COMPlus_GCLOHThreshold` | *valor hexadecimal* | .NET Core 1.0 |
| **app.config para .NET Framework** | [GCLOHThreshold](../../framework/configure-apps/file-schema/runtime/gclohthreshold-element.md) | *valor decimal* | .NET Framework 4.8 |

Exemplo:

```json
{
   "runtimeOptions": {
      "configProperties": {
         "System.GC.LOHThreshold": 120000
      }
   }
}
```

> [!TIP]
> Se você estiver definindo a opção em *runtimeconfig.jsem*, especifique um valor decimal. Se você estiver definindo a opção como uma variável de ambiente, especifique um valor hexadecimal. Por exemplo, para definir um tamanho de limite de 120.000 bytes, os valores seriam 120000 para o arquivo JSON e 0x1D4C0 ou 1D4C0 para a variável de ambiente.

## <a name="standalone-gc"></a>GC autônomo

- Especifica um caminho para a biblioteca que contém o coletor de lixo que o tempo de execução pretende carregar.
- Para obter mais informações, consulte [design de carregador de GC autônomo](https://github.com/dotnet/runtime/blob/master/docs/design/features/standalone-gc-loading.md).

| | Nome da configuração | Valores | Versão introduzida |
| - | - | - | - |
| **runtimeconfig.jsem** | N/D | N/D | N/D |
| **Variável de ambiente** | `COMPlus_GCName` | *string_path* | .NET Core 2.0 |
