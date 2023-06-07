---
title: Elemento <forcePerformanceCounterUniqueSharedMemoryReads>
ms.date: 03/30/2017
helpviewer_keywords:
- forcePerformanceCounterUniqueSharedMemoryReads element
- <forcePerformanceCounterUniqueSharedMemoryReads> element
ms.assetid: 91149858-4810-4f65-9b48-468488172c9b
ms.openlocfilehash: 742b444c445ba67d6977b622e615a6a8f591826e
ms.sourcegitcommit: b16c00371ea06398859ecd157defc81301c9070f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/06/2020
ms.locfileid: "79154134"
---
# <a name="forceperformancecounteruniquesharedmemoryreads-element"></a>Elemento \<forcePerformanceCounterUniqueSharedMemoryReads>
Especifica se o PerfCounter.dll usa a configuração de registro CategoryOptions em um aplicativo do .NET Framework versão 1.1 para determinar se é preciso carregar dados do contador de desempenho da memória global ou da memória compartilhada especifica da categoria.  
  
[**\<configuration>**](../configuration-element.md)\
&nbsp;&nbsp;[**\<runtime>**](runtime-element.md)\
&nbsp;&nbsp;&nbsp;&nbsp;**\<forcePerformanceCounterUniqueSharedMemoryReads>**  
  
## <a name="syntax"></a>Sintaxe  
  
```xml  
<forcePerformanceCounterUniqueSharedMemoryReads
enabled="true|false"/>  
```  
  
## <a name="attributes-and-elements"></a>Atributos e elementos  
 As seções a seguir descrevem atributos, elementos filho e elementos pai.  
  
### <a name="attributes"></a>Atributos  
  
|Atributo|Descrição|  
|---------------|-----------------|  
|`enabled`|Atributo obrigatório.<br /><br /> Indica se PerfCounter. dll usa a configuração do registro CategoryOptions para determinar se os dados do contador de desempenho devem ser carregados de uma memória compartilhada específica da categoria ou da memória global.|  
  
## <a name="enabled-attribute"></a>Atributo habilitado  
  
|Valor|Descrição|  
|-----------|-----------------|  
|`false`|PerfCounter. dll não usa a configuração de registro CategoryOptions, que é o padrão.|  
|`true`|PerfCounter. dll usa a configuração do registro CategoryOptions.|  
  
### <a name="child-elements"></a>Elementos filho  
 Nenhum.  
  
### <a name="parent-elements"></a>Elementos pai  
  
|Elemento|Descrição|  
|-------------|-----------------|  
|`configuration`|O elemento raiz em cada arquivo de configuração usado pelos aplicativos do Common Language Runtime e .NET Framework.|  
|`runtime`|Contém informações sobre associação do assembly e coleta de lixo.|  
  
## <a name="remarks"></a>Comentários  
 Nas versões do .NET Framework antes do .NET Framework 4, a versão de PerfCounter. dll que foi carregada corresponde ao tempo de execução que foi carregado no processo. Se um computador tiver o .NET Framework versão 1,1 e o .NET Framework 2,0 instalados, um aplicativo .NET Framework 1,1 carregará a versão .NET Framework 1,1 do PerfCounter. dll. A partir do .NET Framework 4, a versão mais recente instalada do PerfCounter. dll é carregada. Isso significa que um aplicativo .NET Framework 1,1 carregará a versão .NET Framework 4 do PerfCounter. dll se o .NET Framework 4 estiver instalado no computador.  
  
 Começando com o .NET Framework 4, ao consumir contadores de desempenho, PerfCounter. dll verifica a entrada de registro CategoryOptions para cada provedor para determinar se ele deve ser lido de uma memória compartilhada específica da categoria ou da memória compartilhada global. O .NET Framework 1,1 PerfCounter. dll não lê essa entrada de registro, pois não está ciente da memória compartilhada específica da categoria; Ele sempre lê da memória compartilhada global.  
  
 Para compatibilidade com versões anteriores, o .NET Framework 4 PerfCounter. dll não verifica a entrada de registro CategoryOptions quando executado em um aplicativo .NET Framework 1,1. Ele simplesmente usa a memória compartilhada global, assim como o .NET Framework 1,1 PerfCounter. dll. No entanto, você pode instruir o .NET Framework 4 PerfCounter. dll para verificar a configuração do registro habilitando o `<forcePerformanceCounterUniqueSharedMemoryReads>` elemento.  
  
> [!NOTE]
> Habilitar o `<forcePerformanceCounterUniqueSharedMemoryReads>` elemento não garante que a memória compartilhada específica da categoria será usada. A configuração habilitada `true` só faz com que PerfCounter. dll referencie a configuração do registro CategoryOptions. A configuração padrão para CategoryOptions é usar a memória compartilhada específica da categoria; no entanto, você pode alterar CategoryOptions para indicar que a memória compartilhada global deve ser usada.  
  
 A chave do registro que contém a configuração CategoryOptions é HKEY_LOCAL_MACHINE \System\CurrentControlSet\Services \\<NomeDaCategoria \> \Performance. Por padrão, CategoryOptions é definido como 3, o que instrui o PerfCounter. dll a usar a memória compartilhada específica da categoria. Se CategoryOptions for definido como 0, PerfCounter. dll usará a memória compartilhada global. Os dados da instância serão reutilizados somente se o nome da instância que está sendo criada for idêntico à instância que está sendo reutilizada. Todas as versões poderão gravar na categoria. Se CategoryOptions for definido como 1, a memória compartilhada global será usada, mas os dados da instância poderão ser reutilizados se o nome da categoria tiver o mesmo comprimento que a categoria que está sendo reutilizada.  
  
 As configurações 0 e 1 podem levar a vazamentos de memória e o preenchimento da memória do contador de desempenho.  
  
## <a name="example"></a>Exemplo  
 O exemplo a seguir mostra como especificar que PerfCounter. dll deve fazer referência à entrada de registro CategoryOptions para determinar se ela deve usar a memória compartilhada específica da categoria.  
  
```xml  
<configuration>  
  <runtime>  
    <forcePerformanceCounterUniqueSharedMemoryReads enabled="true"/>  
  </runtime>  
</configuration>  
```  
  
## <a name="see-also"></a>Confira também

- [Esquema de configurações do runtime](index.md)
- [Esquema do arquivo de configuração](../index.md)
