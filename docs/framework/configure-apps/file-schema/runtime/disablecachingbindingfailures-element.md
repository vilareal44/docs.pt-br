---
title: Elemento <disableCachingBindingFailures>
ms.date: 03/30/2017
f1_keywords:
- http://schemas.microsoft.com/.NetConfiguration/v2.0#disableCachingBindingFailures
- http://schemas.microsoft.com/.NetConfiguration/v2.0#configuration/runtime/disableCachingBindingFailures
helpviewer_keywords:
- assemblies [.NET Framework],caching binding failures
- caching assembly binding failures
- <disableCachingBindingFailures> element
- disableCachingBindingFailures element
ms.assetid: bf598873-83b7-48de-8955-00b0504fbad0
ms.openlocfilehash: 23633cb282b8e59b4df4bcc2cd38717d805a207e
ms.sourcegitcommit: b16c00371ea06398859ecd157defc81301c9070f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/06/2020
ms.locfileid: "73117494"
---
# <a name="disablecachingbindingfailures-element"></a>Elemento \<disableCachingBindingFailures>
Especifica se é para desabilitar o cache de falhas de associação que ocorrem porque o assembly não foi encontrado pela investigação.  
  
[**\<configuration>**](../configuration-element.md)\
&nbsp;&nbsp;[**\<runtime>**](runtime-element.md)\
&nbsp;&nbsp;&nbsp;&nbsp;**\<disableCachingBindingFailures>**  
  
## <a name="syntax"></a>Sintaxe  
  
```xml  
<disableCachingBindingFailures enabled="0|1"/>  
```  
  
## <a name="attributes-and-elements"></a>Atributos e elementos  
 As seções a seguir descrevem atributos, elementos filho e elementos pai.  
  
### <a name="attributes"></a>Atributos  
  
|Atributo|Descrição|  
|---------------|-----------------|  
|Habilitado|Atributo obrigatório.<br /><br /> Especifica se é para desabilitar o cache de falhas de associação que ocorrem porque o assembly não foi encontrado pela investigação.|  
  
## <a name="enabled-attribute"></a>Atributo habilitado  
  
|Valor|Descrição|  
|-----------|-----------------|  
|0|Não desabilite o cache de falhas de associação que ocorrem porque o assembly não foi encontrado pela investigação. Esse é o comportamento de associação padrão a partir do .NET Framework versão 2,0.|  
|1|Desabilite o cache de falhas de associação que ocorrem porque o assembly não foi encontrado pela investigação. Essa configuração reverte para o comportamento de associação do .NET Framework versão 1,1.|  
  
### <a name="child-elements"></a>Elementos filho  
 Nenhum.  
  
### <a name="parent-elements"></a>Elementos pai  
  
|Elemento|Descrição|  
|-------------|-----------------|  
|`configuration`|O elemento raiz em cada arquivo de configuração usado pelos aplicativos do Common Language Runtime e .NET Framework.|  
|`runtime`|Contém informações sobre associação do assembly e coleta de lixo.|  
  
## <a name="remarks"></a>Comentários  
 A partir do .NET Framework versão 2,0, o comportamento padrão para carregar assemblies é armazenar em cache todas as falhas de associação e carregamento. Ou seja, se uma tentativa de carregar um assembly falhar, as solicitações subsequentes para carregar o mesmo assembly falharão imediatamente, sem nenhuma tentativa de localizar o assembly. Esse elemento desabilita esse comportamento padrão para falhas de associação que ocorrem porque o assembly não foi encontrado no caminho de investigação. Essas falhas geram <xref:System.IO.FileNotFoundException> .  
  
 Algumas falhas de ligação e carregamento não são afetadas por esse elemento e sempre são armazenadas em cache. Essas falhas ocorrem porque o assembly foi encontrado, mas não pôde ser carregado. Eles lançam <xref:System.BadImageFormatException> ou <xref:System.IO.FileLoadException> . A lista a seguir inclui alguns exemplos de tais falhas.  
  
- Se você tentar carregar um arquivo não for um assembly válido, as tentativas subsequentes de carregar o assembly falharão mesmo que o arquivo incorreto seja substituído pelo assembly correto.  
  
- Se você tentar carregar um assembly que está bloqueado pelo sistema de arquivos, as tentativas subsequentes de carregar o assembly falharão mesmo depois que o assembly for liberado pelo sistema de arquivos.  
  
- Se uma ou mais versões do assembly que você está tentando carregar estiver no caminho de investigação, mas a versão específica que você está solicitando não estiver entre elas, as tentativas subsequentes de carregar essa versão falharão mesmo que a versão correta seja movida para o caminho de investigação.  
  
## <a name="example"></a>Exemplo  
 O exemplo a seguir mostra como desabilitar o cache de falhas de associação de assembly que ocorrem porque o assembly não foi encontrado pela investigação.  
  
```xml  
<configuration>  
   <runtime>  
      <disableCachingBindingFailures enabled="1" />  
   </runtime>  
</configuration>  
```  
  
## <a name="see-also"></a>Confira também

- [Esquema de configurações do runtime](index.md)
- [Esquema de arquivos de configuração](../index.md)
- [Como o runtime localiza assemblies](../../../deployment/how-the-runtime-locates-assemblies.md)
