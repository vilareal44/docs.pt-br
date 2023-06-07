---
title: Elemento <shadowCopyVerifyByTimestamp>
ms.date: 03/30/2017
helpviewer_keywords:
- <shadowCopyTimeStampVerification> element
- shadowCopyTimeStampVerification element
ms.assetid: 2f1648e5-997b-435e-a4f9-d236c574c66c
ms.openlocfilehash: 160f14c856735e1ceac8635506aea52454faea43
ms.sourcegitcommit: b16c00371ea06398859ecd157defc81301c9070f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/06/2020
ms.locfileid: "73115735"
---
# <a name="shadowcopyverifybytimestamp-element"></a>Elemento \<shadowCopyVerifyByTimestamp>
Especifica se a cópia de sombra usa o comportamento de inicialização padrão introduzido no .NET Framework 4 ou reverte para o comportamento de inicialização de versões anteriores do .NET Framework.  
  
[**\<configuration>**](../configuration-element.md)\
&nbsp;&nbsp;[**\<runtime>**](runtime-element.md)\
&nbsp;&nbsp;&nbsp;&nbsp;**\<shadowCopyVerifyByTimestamp>**  
  
## <a name="syntax"></a>Sintaxe  
  
```xml  
<shadowCopyVerifyByTimestamp enabled="true|false" />  
```  
  
## <a name="attributes-and-elements"></a>Atributos e elementos  
 As seções a seguir descrevem atributos, elementos filho e elementos pai.  
  
### <a name="attributes"></a>Atributos  
  
|Atributo|Descrição|  
|---------------|-----------------|  
|Habilitado|Atributo obrigatório.<br /><br /> Especifica se os domínios de aplicativo que usam a cópia de sombra comparam carimbos de data/hora ao iniciar, para determinar se um assembly foi atualizado antes de copiar a sombra do assembly.|  
  
## <a name="enabled-attribute"></a>Atributo habilitado  
  
|Valor|Descrição|  
|-----------|-----------------|  
|true|Na inicialização, o copia somente os assemblies que foram atualizados desde a última vez que foram copiados para o diretório de cópia de sombra. Esse é o padrão para o .NET Framework 4.|  
|false|Reverte para o comportamento de inicialização de versões anteriores do .NET Framework, que era copiar todos os arquivos na inicialização.|  
  
### <a name="child-elements"></a>Elementos filho  
 Nenhum.  
  
### <a name="parent-elements"></a>Elementos pai  
  
|Elemento|Descrição|  
|-------------|-----------------|  
|`configuration`|O elemento raiz em cada arquivo de configuração usado pelos aplicativos do Common Language Runtime e .NET Framework.|  
|`runtime`|Contém informações sobre associação do assembly e coleta de lixo.|  
  
## <a name="remarks"></a>Comentários  
 A partir do .NET Framework 4, os assemblies serão copiados por sombra somente se os carimbos de data/hora indicarem que foram alterados desde que foram copiados pela última vez para o diretório de cópia de sombra. Isso melhora os tempos de inicialização para muitos aplicativos que usam cópia de sombra, conforme descrito em [assemblies de cópia de sombra](../../../app-domains/shadow-copy-assemblies.md). Aplicativos que têm uma alta porcentagem e frequência de atualizações de assembly podem não se beneficiar dessa alteração no comportamento. Nesse caso, você pode usar esse elemento para restaurar o comportamento de versões anteriores do .NET Framework.  
  
## <a name="example"></a>Exemplo  
 O exemplo a seguir mostra como desabilitar o comportamento de inicialização padrão da cópia de sombra no .NET Framework 4 e reverter para o comportamento de inicialização de versões anteriores do .NET Framework.  
  
```xml  
<configuration>  
   <runtime>  
      <shadowCopyVerifyByTimestamp enabled="false" />  
   </runtime>  
</configuration>  
```  
  
## <a name="see-also"></a>Confira também

- [Esquema de configurações do runtime](index.md)
- [Esquema de arquivos de configuração](../index.md)
- [Criando cópias de sombra de assemblies](../../../app-domains/shadow-copy-assemblies.md)
