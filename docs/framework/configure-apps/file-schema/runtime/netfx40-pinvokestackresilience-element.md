---
title: Elemento <NetFx40_PInvokeStackResilience>
ms.date: 03/30/2017
helpviewer_keywords:
- <NetFx40_PInvokeStackResilience> element
- NetFx40_PInvokeStackResilience element
ms.assetid: 39fb1588-72a4-4479-af74-0605233b68bd
ms.openlocfilehash: 86f50aafe0b21d5080288e09ac7118ca1e4c939a
ms.sourcegitcommit: b16c00371ea06398859ecd157defc81301c9070f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/06/2020
ms.locfileid: "73116155"
---
# <a name="netfx40_pinvokestackresilience-element"></a>Elemento \<NetFx40_PInvokeStackResilience>

Especifica se o runtime corrige automaticamente declarações de invocação de plataforma incorretas em runtime, às custas de transições mais lentas entre o código gerenciado e não gerenciado.

[**\<configuration>**](../configuration-element.md)\
&nbsp;&nbsp;[**\<runtime>**](runtime-element.md)\
&nbsp;&nbsp;&nbsp;&nbsp;**\<NetFx40_PInvokeStackResilience>**  

## <a name="syntax"></a>Sintaxe

```xml
<NetFx40_PInvokeStackResilience  enabled="1|0"/>
```

## <a name="attributes-and-elements"></a>Atributos e elementos

As seções a seguir descrevem atributos, elementos filho e elementos pai.

### <a name="attributes"></a>Atributos

|Atributo|Descrição|
|---------------|-----------------|
|`enabled`|Atributo obrigatório.<br /><br /> Especifica se o tempo de execução detecta declarações incorretas de invocação de plataforma e corrige automaticamente a pilha em tempo de execução em plataformas de 32 bits.|

## <a name="enabled-attribute"></a>Atributo habilitado

|Valor|Descrição|
|-----------|-----------------|
|`0`|O tempo de execução usa a arquitetura de marshaling de interoperabilidade mais rápida introduzida no .NET Framework 4, que não detecta e corrige declarações de invocação de plataforma incorretas. Este é o padrão.|
|`1`|O tempo de execução usa transições mais lentas que detectam e corrigem declarações de invocação de plataforma incorretas.|

### <a name="child-elements"></a>Elementos filho

Nenhum.

### <a name="parent-elements"></a>Elementos pai

|Elemento|Descrição|
|-------------|-----------------|
|`configuration`|O elemento raiz em cada arquivo de configuração usado pelos aplicativos do Common Language Runtime e .NET Framework.|
|`runtime`|Contém informações sobre opções de inicialização do runtime.|

## <a name="remarks"></a>Comentários

Esse elemento permite que você negocie o marshaling de interoperabilidade mais rápido para resiliência em tempo de execução contra declarações de invocação de plataforma incorretas.

A partir do .NET Framework 4, uma arquitetura de marshaling de interoperabilidade simplificada fornece uma melhoria significativa de desempenho para transições de código gerenciado para código não gerenciado. Em versões anteriores do .NET Framework, a camada de marshaling detectou declarações incorretas de invocação de plataforma em plataformas de 32 bits e corrigiu a pilha automaticamente. A nova arquitetura de marshaling elimina essa etapa. Como resultado, as transições são muito rápidas, mas uma declaração de invocação de plataforma incorreta pode causar uma falha do programa.

Para facilitar a detecção de declarações incorretas durante o desenvolvimento, a experiência de depuração do Visual Studio foi aprimorada. O MDA (Assistente de depuração gerenciada) [pInvokeStackImbalance](../../../debug-trace-profile/pinvokestackimbalance-mda.md) notifica sobre declarações de invocação de plataforma incorretas quando seu aplicativo está em execução com o depurador anexado.

Para abordar cenários em que seu aplicativo usa componentes que você não pode recompilar e que têm declarações de invocação de plataforma incorretas, você pode usar o `NetFx40_PInvokeStackResilience` elemento. Adicionar esse elemento ao arquivo de configuração do aplicativo com `enabled="1"` optas por um modo de compatibilidade com o comportamento de versões anteriores do .NET Framework, às custas de transições mais lentas. Os assemblies que foram compilados em relação às versões anteriores do .NET Framework são automaticamente optados por esse modo de compatibilidade e não precisam desse elemento.

## <a name="configuration-file"></a>Arquivo de configuração

Esse elemento só pode ser usado no arquivo de configuração do aplicativo.

## <a name="example"></a>Exemplo

O exemplo a seguir mostra como aceitar maior resiliência contra declarações de invocação de plataforma incorretas para um aplicativo, ao custo de transições mais lentas entre códigos gerenciados e não gerenciados.

```xml
<configuration>
   <runtime>
      <NetFx40_PInvokeStackResilience enabled="1"/>
   </runtime>
</configuration>
```

## <a name="see-also"></a>Confira também

- [Esquema de configurações do runtime](index.md)
- [Esquema de arquivos de configuração](../index.md)
- [pInvokeStackImbalance](../../../debug-trace-profile/pinvokestackimbalance-mda.md)
