---
title: Elemento <UseSmallInternalThreadStacks>
ms.date: 03/30/2017
helpviewer_keywords:
- UseSmallInternalThreadStacks element
- <UseSmallInternalThreadStacks> element
ms.assetid: 1e3f6ec0-1cac-4e1c-9c81-17d948ae5874
ms.openlocfilehash: 2fd776ce8605e6dcf288dcb3852ded16638a1873
ms.sourcegitcommit: b16c00371ea06398859ecd157defc81301c9070f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/06/2020
ms.locfileid: "73114920"
---
# <a name="usesmallinternalthreadstacks-element"></a>Elemento \<UseSmallInternalThreadStacks>
Solicita que o Common Language Runtime (CLR) reduza o uso de memória especificando tamanhos de pilha explícitos ao criar determinados threads que ele usa internamente, em vez de usar o tamanho de pilha padrão para esses threads.  
  
[**\<configuration>**](../configuration-element.md)\
&nbsp;&nbsp;[**\<runtime>**](runtime-element.md)\
&nbsp;&nbsp;&nbsp;&nbsp;**\<UseSmallInternalThreadStacks>**  
  
## <a name="syntax"></a>Sintaxe  
  
```xml  
<UseSmallInternalThreadStacks enabled="true|false" />  
```  
  
## <a name="attributes-and-elements"></a>Atributos e elementos  
 As seções a seguir descrevem atributos, elementos filho e elementos pai.  
  
### <a name="attributes"></a>Atributos  
  
|Atributo|Descrição|  
|---------------|-----------------|  
|Habilitado|Atributo obrigatório.<br /><br /> Especifica se deve-se solicitar que o CLR use tamanhos de pilha explícitos em vez do tamanho de pilha padrão quando ele cria determinados threads que ele usa internamente. Os tamanhos de pilha explícitos são menores do que o tamanho de pilha padrão de 1 MB.|  
  
## <a name="enabled-attribute"></a>Atributo habilitado  
  
|Valor|Descrição|  
|-----------|-----------------|  
|true|Solicitar tamanhos de pilha explícitos.|  
|false|Use o tamanho de pilha padrão. Esse é o padrão para o .NET Framework 4.|  
  
### <a name="child-elements"></a>Elementos filho  
 Nenhum.  
  
### <a name="parent-elements"></a>Elementos pai  
  
|Elemento|Descrição|  
|-------------|-----------------|  
|`configuration`|O elemento raiz em cada arquivo de configuração usado pelos aplicativos do Common Language Runtime e .NET Framework.|  
|`runtime`|Contém informações sobre associação do assembly e coleta de lixo.|  
  
## <a name="remarks"></a>Comentários  
 Esse elemento de configuração é usado para solicitar o uso reduzido de memória virtual em um processo, pois os tamanhos de thread explícitos usados pelo CLR para seus threads internos, se a solicitação for respeitada, serão menores do que o tamanho padrão.  
  
> [!IMPORTANT]
> Esse elemento de configuração é uma solicitação para o CLR em vez de um requisito absoluto. No .NET Framework 4, a solicitação é respeitada apenas para a arquitetura x86. Esse elemento pode ser ignorado completamente em versões futuras do CLR, ou substituído por tamanhos de pilha explícitos que são sempre usados para threads internos selecionados.  
  
 A especificação desse elemento de configuração compensa a confiabilidade para uso menor de memória virtual se o CLR honrar a solicitação, porque tamanhos de pilha menores poderiam potencialmente causar estouros de pilha mais prováveis.  
  
## <a name="example"></a>Exemplo  
 O exemplo a seguir mostra como solicitar que o CLR use tamanhos de pilha explícitos para determinados threads que ele usa internamente.  
  
```xml  
<configuration>  
   <runtime>  
      <UseSmallInternalThreadStacks enabled="true" />  
   </runtime>  
</configuration>  
```  
  
## <a name="see-also"></a>Confira também

- [Esquema de configurações do runtime](index.md)
- [Esquema do arquivo de configuração](../index.md)
