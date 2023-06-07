---
title: Elemento <NetFx40_LegacySecurityPolicy>
ms.date: 03/30/2017
helpviewer_keywords:
- <NetFx40_LegacySecurityPolicy> element
- NetFx40_LegacySecurityPolicy element
ms.assetid: 07132b9c-4a72-4710-99d7-e702405e02d4
ms.openlocfilehash: d5192eb56bb8b640544bdc52a0bb9d8a5277efef
ms.sourcegitcommit: b16c00371ea06398859ecd157defc81301c9070f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/06/2020
ms.locfileid: "73116246"
---
# <a name="netfx40_legacysecuritypolicy-element"></a>Elemento \<NetFx40_LegacySecurityPolicy>

Especifica se o runtime usa a política de CAS (Segurança de Acesso do Código) herdada.

[**\<configuration>**](../configuration-element.md)\
&nbsp;&nbsp;[**\<runtime>**](runtime-element.md)\
&nbsp;&nbsp;&nbsp;&nbsp;**\<NetFx40_LegacySecurityPolicy>**  

## <a name="syntax"></a>Sintaxe

```xml
<NetFx40_LegacySecurityPolicy
   enabled="true|false"/>
```

## <a name="attributes-and-elements"></a>Atributos e elementos

As seções a seguir descrevem atributos, elementos filho e elementos pai.

### <a name="attributes"></a>Atributos

|Atributo|Descrição|
|---------------|-----------------|
|`enabled`|Atributo obrigatório.<br /><br /> Especifica se o tempo de execução usa a política CAS herdada.|

## <a name="enabled-attribute"></a>Atributo habilitado

|Valor|Descrição|
|-----------|-----------------|
|`false`|O tempo de execução não usa a política CAS herdada. Este é o padrão.|
|`true`|O tempo de execução usa a política CAS herdada.|

### <a name="child-elements"></a>Elementos filho

Nenhum.

### <a name="parent-elements"></a>Elementos pai

|Elemento|Descrição|
|-------------|-----------------|
|`configuration`|O elemento raiz em cada arquivo de configuração usado pelos aplicativos do Common Language Runtime e .NET Framework.|
|`runtime`|Contém informações sobre opções de inicialização do runtime.|

## <a name="remarks"></a>Comentários

No .NET Framework versão 3,5 e versões anteriores, a política de CAS está sempre em vigor. No .NET Framework 4, a política de CAS deve ser habilitada.

A política de CAS é específica da versão. As políticas de CAS personalizadas que existem em versões anteriores do .NET Framework devem ser reespecificadas no .NET Framework 4.

Aplicar o `<NetFx40_LegacySecurityPolicy>` elemento a um assembly .NET Framework 4 não afeta o [código de segurança transparente](../../../misc/security-transparent-code.md); as regras de transparência ainda se aplicam.

> [!IMPORTANT]
> A aplicação do `<NetFx40_LegacySecurityPolicy>` elemento pode resultar em penalidades de desempenho significativas para assemblies de imagem nativa criados pelo [gerador de imagem nativa (NGen. exe)](../../../tools/ngen-exe-native-image-generator.md) que não estão instalados no [cache de assembly global](../../../app-domains/gac.md). A degradação do desempenho é causada pela incapacidade do tempo de execução de carregar os assemblies como imagens nativas quando o atributo é aplicado, resultando no carregamento de assemblies just-in-time.

> [!NOTE]
> Se você especificar uma versão de .NET Framework de destino que seja anterior à .NET Framework 4 nas configurações do projeto para seu projeto do Visual Studio, a política de CAS será habilitada, incluindo as políticas CAS personalizadas especificadas para essa versão. No entanto, você não poderá usar novos .NET Framework 4 tipos e membros. Você também pode especificar uma versão anterior do .NET Framework usando o [ \<supportedRuntime> elemento](../startup/supportedruntime-element.md) no esquema de configurações de inicialização no arquivo de [configuração do aplicativo](../../index.md).

> [!NOTE]
> A sintaxe do arquivo de configuração diferencia maiúsculas de minúsculas. Você deve usar a sintaxe conforme fornecido nas seções sintaxe e exemplo.

## <a name="configuration-file"></a>Arquivo de configuração

Esse elemento só pode ser usado no arquivo de configuração do aplicativo.

## <a name="example"></a>Exemplo

O exemplo a seguir mostra como habilitar a política CAS herdada para um aplicativo.

```xml
<configuration>
   <runtime>
      <NetFx40_LegacySecurityPolicy enabled="true"/>
   </runtime>
</configuration>
```

## <a name="see-also"></a>Confira também

- [Esquema de configurações do runtime](index.md)
- [Esquema do arquivo de configuração](../index.md)
