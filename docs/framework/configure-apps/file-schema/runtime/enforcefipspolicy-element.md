---
title: Elemento <enforceFIPSPolicy>
ms.date: 03/30/2017
helpviewer_keywords:
- enforceFIPSPolicy element
- FIPS
- <enforceFIPSPolicy> element
- Federal Information Processing Standards (FIPS)
ms.assetid: c35509c4-35cf-43c0-bb47-75e4208aa24e
ms.openlocfilehash: 0d6dd291a24928487a040c0427f058dee80bf836
ms.sourcegitcommit: b16c00371ea06398859ecd157defc81301c9070f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/06/2020
ms.locfileid: "73117382"
---
# <a name="enforcefipspolicy-element"></a>Elemento \<enforceFIPSPolicy>
Especifica se deve-se impor um requisito de configuração do computador em que os algoritmos de criptografia devem estar em conformidade com o FIPS (padrão norte-americano de processamento de informações).  
  
[**\<configuration>**](../configuration-element.md)\
&nbsp;&nbsp;[**\<runtime>**](runtime-element.md)\
&nbsp;&nbsp;&nbsp;&nbsp;**\<enforceFIPSPolicy>**  
  
## <a name="syntax"></a>Sintaxe  
  
```xml  
<enforceFIPSPolicy enabled="true|false" />  
```  
  
## <a name="attributes-and-elements"></a>Atributos e elementos  
 As seções a seguir descrevem atributos, elementos filho e elementos pai.  
  
### <a name="attributes"></a>Atributos  
  
|Atributo|Descrição|  
|---------------|-----------------|  
|Habilitado|Atributo obrigatório.<br /><br /> Especifica se é necessário habilitar a imposição de um requisito de configuração de computador que os algoritmos criptográficos devem ser compatíveis com o FIPS.|  
  
## <a name="enabled-attribute"></a>Atributo habilitado  
  
|Valor|Descrição|  
|-----------|-----------------|  
|`true`|Se o computador estiver configurado para exigir que os algoritmos criptográficos sejam compatíveis com FIPS, esse requisito será imposto. Se uma classe implementa um algoritmo que não está em conformidade com o FIPS, os construtores ou `Create` métodos dessa classe lançam exceções quando são executados nesse computador. Este é o padrão.|  
|`false`|Os algoritmos de criptografia usados pelo aplicativo não precisam ser compatíveis com o FIPS, independentemente da configuração do computador.|  
  
### <a name="child-elements"></a>Elementos filho  
 Nenhum.  
  
### <a name="parent-elements"></a>Elementos pai  
  
|Elemento|Descrição|  
|-------------|-----------------|  
|`configuration`|O elemento raiz em cada arquivo de configuração usado pelos aplicativos do Common Language Runtime e .NET Framework.|  
|`runtime`|Contém informações sobre associação do assembly e coleta de lixo.|  
  
## <a name="remarks"></a>Comentários  
 A partir do .NET Framework 2,0, a criação de classes que implementam algoritmos de criptografia é controlada pela configuração do computador. Se o computador estiver configurado para exigir que os algoritmos sejam compatíveis com o FIPS e uma classe implementar um algoritmo que não seja compatível com o FIPS, qualquer tentativa de criar uma instância dessa classe gerará uma exceção. Os construtores geram uma <xref:System.InvalidOperationException> exceção e os `Create` métodos geram uma exceção <xref:System.Reflection.TargetInvocationException> com uma <xref:System.InvalidOperationException> exceção interna.  
  
 Se seu aplicativo for executado em computadores cujas configurações exigem conformidade com o FIPS e seu aplicativo usa um algoritmo que não é compatível com o FIPS, você pode usar esse elemento em seu arquivo de configuração para impedir que o Common Language Runtime (CLR) imponha a conformidade com o FIPS. Esse elemento foi introduzido no .NET Framework 2,0 Service Pack 1.  
  
## <a name="example"></a>Exemplo  
 O exemplo a seguir mostra como impedir que o CLR imponha a conformidade com o FIPS.  
  
```xml  
<configuration>  
    <runtime>  
        <enforceFIPSPolicy enabled="false"/>  
    </runtime>  
</configuration>  
```  
  
## <a name="see-also"></a>Confira também

- [Esquema de configurações do runtime](index.md)
- [Esquema de arquivos de configuração](../index.md)
- [Modelo de criptografia](../../../../standard/security/cryptography-model.md)
