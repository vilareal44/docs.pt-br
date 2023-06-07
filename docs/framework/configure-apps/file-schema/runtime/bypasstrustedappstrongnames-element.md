---
title: Elemento <bypassTrustedAppStrongNames>
ms.date: 03/30/2017
helpviewer_keywords:
- strong-name bypass feature
- bypassTrustedAppStrongNames element
- strong-named assemblies, loading into trusted application domains
- <bypassTrustedAppStrongNames> element
ms.assetid: 71b2ebf6-3843-41e2-ad52-ffa5cd083a40
ms.openlocfilehash: 96361a6742d1d2f76cb237344189d3277d7c8069
ms.sourcegitcommit: b16c00371ea06398859ecd157defc81301c9070f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/06/2020
ms.locfileid: "73739088"
---
# <a name="bypasstrustedappstrongnames-element"></a>Elemento \<bypassTrustedAppStrongNames>

Especifica se deve ignorar a validação de nomes fortes em assemblies de confiança total que são carregados em uma confiança total <xref:System.AppDomain> .

[**\<configuration>**](../configuration-element.md)\
&nbsp;&nbsp;[**\<runtime>**](runtime-element.md)\
&nbsp;&nbsp;&nbsp;&nbsp;**\<bypassTrustedAppStrongNames>**

## <a name="syntax"></a>Sintaxe

```xml
<bypassTrustedAppStrongNames
   enabled="true|false"/>
```

## <a name="attributes-and-elements"></a>Atributos e elementos

As seções a seguir descrevem atributos, elementos filho e elementos pai.

### <a name="attributes"></a>Atributos

|Atributo|Descrição|
|---------------|-----------------|
|`enabled`|Atributo obrigatório.<br /><br /> Especifica se o recurso de bypass que evita a validação de nomes fortes para assemblies de confiança total está habilitado. Quando esse recurso é habilitado, nomes fortes não são validados quanto à exatidão quando o assembly é carregado. O padrão é `true`.|

## <a name="enabled-attribute"></a>Atributo habilitado

|Valor|Descrição|
|-----------|-----------------|
|`true`|As assinaturas de nome forte em assemblies de confiança total não são validadas quando os assemblies são carregados em uma confiança total <xref:System.AppDomain> . Este é o padrão.|
|`false`|As assinaturas de nome forte em assemblies de confiança total são validadas quando os assemblies são carregados em uma confiança total <xref:System.AppDomain> . A assinatura de nome forte é verificada apenas quanto à exatidão da assinatura; Ela não é comparada com outro nome forte para uma correspondência.|

### <a name="child-elements"></a>Elementos filho

Nenhum.

### <a name="parent-elements"></a>Elementos pai

|Elemento|Descrição|
|-------------|-----------------|
|`configuration`|O elemento raiz em cada arquivo de configuração usado pelos aplicativos do Common Language Runtime e .NET Framework.|
|`runtime`|Contém informações sobre associação do assembly e coleta de lixo.|

## <a name="remarks"></a>Comentários

O recurso de bypass de nome forte evita a sobrecarga de verificação de assinatura de nome forte de assemblies de confiança total.

O recurso de desvio se aplica a qualquer assembly que está assinado com um nome forte e que tem as seguintes características:

- Totalmente confiável sem a <xref:System.Security.Policy.StrongName> evidência (por exemplo, tem `MyComputer` evidências de zona).

- Carregado em um <xref:System.AppDomain> totalmente confiável.

- Carregado de um local sob a propriedade <xref:System.AppDomainSetup.ApplicationBase%2A> desse <xref:System.AppDomain>.

- Não assinado com atraso.

> [!NOTE]
> Se o recurso de bypass tiver sido desativado para todos os aplicativos no computador usando uma chave do registro, essa configuração do arquivo de configuração não terá nenhum efeito. Para obter mais informações, consulte [como: desabilitar o recurso de bypass de nome forte](../../../../standard/assembly/disable-strong-name-bypass-feature.md).

## <a name="example"></a>Exemplo

O exemplo a seguir mostra como especificar o comportamento que valida a assinatura de nome forte em assemblies de confiança total.

```xml
<configuration>
   <runtime>
      <bypassTrustedAppStrongNames enabled="false"/>
   </runtime>
</configuration>
```

## <a name="see-also"></a>Confira também

- [Esquema de configurações do runtime](index.md)
- [Esquema de arquivos de configuração](../index.md)
- [Como desabilitar o recurso de bypass de nome forte](../../../../standard/assembly/disable-strong-name-bypass-feature.md)
