---
title: Elemento <legacyCorruptedStateExceptionsPolicy>
ms.date: 03/30/2017
helpviewer_keywords:
- <legacyCorruptedStateExceptionsPolicy> element
- legacyCorruptedStateExceptionsPolicy element
ms.assetid: e0a55ddc-bfa8-4f3e-ac14-d1fc3330e4bb
ms.openlocfilehash: d1d29a37999a01f3e370897a1052f4f94435a218
ms.sourcegitcommit: b16c00371ea06398859ecd157defc81301c9070f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/06/2020
ms.locfileid: "73116454"
---
# <a name="legacycorruptedstateexceptionspolicy-element"></a>Elemento \<legacyCorruptedStateExceptionsPolicy>
Especifica se o Common Language Runtime permite que o código gerenciado Capture violações de acesso e outras exceções de estado corrompidas.  
  
[**\<configuration>**](../configuration-element.md)\
&nbsp;&nbsp;[**\<runtime>**](runtime-element.md)\
&nbsp;&nbsp;&nbsp;&nbsp;**\<legacyCorruptedStateExceptionsPolicy>**  
  
## <a name="syntax"></a>Sintaxe  
  
```xml  
<legacyCorruptedStateExceptionsPolicy enabled="true|false"/>  
```  
  
## <a name="attributes-and-elements"></a>Atributos e elementos  
 As seções a seguir descrevem atributos, elementos filho e elementos pai.  
  
### <a name="attributes"></a>Atributos  
  
|Atributo|Descrição|  
|---------------|-----------------|  
|`enabled`|Atributo obrigatório.<br /><br /> Especifica que o aplicativo irá detectar falhas de exceção de estado corrompido, como violações de acesso.|  
  
## <a name="enabled-attribute"></a>Atributo habilitado  
  
|Valor|Descrição|  
|-----------|-----------------|  
|`false`|O aplicativo não detectará falhas de exceção de estado corrompido, como violações de acesso. Este é o padrão.|  
|`true`|O aplicativo detectará falhas de exceção de estado corrompido, como violações de acesso.|  
  
### <a name="child-elements"></a>Elementos filho  
 Nenhum.  
  
### <a name="parent-elements"></a>Elementos pai  
  
|Elemento|Descrição|  
|-------------|-----------------|  
|`configuration`|O elemento raiz em cada arquivo de configuração usado pelos aplicativos do Common Language Runtime e .NET Framework.|  
|`runtime`|Contém informações sobre associação do assembly e coleta de lixo.|  
  
## <a name="remarks"></a>Comentários  
 No .NET Framework versão 3,5 e anteriores, o Common Language Runtime permitia código gerenciado para capturar exceções que foram geradas por Estados de processo corrompidos. Uma violação de acesso é um exemplo desse tipo de exceção.  
  
 A partir do .NET Framework 4, o código gerenciado não captura mais esses tipos de exceções em `catch` blocos. No entanto, você pode substituir essa alteração e manter a manipulação de exceções de estado corrompidas de duas maneiras:  
  
- Defina o `<legacyCorruptedStateExceptionsPolicy>` atributo do elemento `enabled` como `true` . Essa configuração é aplicada processwide e afeta todos os métodos.  
  
 -ou-  
  
- Aplique o <xref:System.Runtime.ExceptionServices.HandleProcessCorruptedStateExceptionsAttribute?displayProperty=nameWithType> atributo ao método que contém o bloco de exceções `catch` .  
  
 Este elemento de configuração está disponível apenas no .NET Framework 4 e posterior.  
  
## <a name="example"></a>Exemplo  
 O exemplo a seguir mostra como especificar que o aplicativo deve reverter para o comportamento antes do .NET Framework 4 e capturar todas as falhas de exceção de estado corrompido.  
  
```xml  
<configuration>  
   <runtime>  
      <legacyCorruptedStateExceptionsPolicy enabled="true" />  
   </runtime>  
</configuration>  
```  
  
## <a name="see-also"></a>Confira também

- <xref:System.Runtime.ExceptionServices.HandleProcessCorruptedStateExceptionsAttribute>
- [Esquema de configurações do runtime](index.md)
- [Esquema do arquivo de configuração](../index.md)
