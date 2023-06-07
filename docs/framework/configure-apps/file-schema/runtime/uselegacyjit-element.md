---
title: Elemento <useLegacyJit>
ms.date: 04/26/2017
ms.assetid: c2cf97f0-9262-4f1f-a754-5568b51110ad
ms.openlocfilehash: a126b8c0050a8d1fd96a3d090f9b018a9faa07a7
ms.sourcegitcommit: b16c00371ea06398859ecd157defc81301c9070f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/06/2020
ms.locfileid: "73968853"
---
# <a name="uselegacyjit-element"></a>Elemento \<useLegacyJit>

Determina se o Common Language Runtime usa o compilador JIT de 64 bits herdado para uma compilação just-in-time.  
  
[**\<configuration>**](../configuration-element.md)\
&nbsp;&nbsp;[**\<runtime>**](runtime-element.md)\
&nbsp;&nbsp;&nbsp;&nbsp;**\<useLegacyJit>**  
  
## <a name="syntax"></a>Sintaxe  
  
```xml
<useLegacyJit enabled=0|1 />
```

O nome do elemento diferencia `useLegacyJit` maiúsculas de minúsculas.
  
## <a name="attributes-and-elements"></a>Atributos e elementos

As seções a seguir descrevem atributos, elementos filho e elementos pai.  
  
### <a name="attributes"></a>Atributos  
  
| Atributo | Descrição                                                                                   |  
| --------- | --------------------------------------------------------------------------------------------- |  
| `enabled` | Atributo obrigatório.<br><br>Especifica se o tempo de execução usa o compilador JIT de 64 bits herdado. |  
  
### <a name="enabled-attribute"></a>atributo habilitado  
  
| Valor | Descrição                                                                                                         |  
| ----- | ------------------------------------------------------------------------------------------------------------------- |  
| 0     | O Common Language Runtime usa o novo compilador JIT de 64 bits incluído no .NET Framework 4,6 e versões posteriores. |  
| 1     | O Common Language Runtime usa o compilador JIT de 64 bits mais antigo.                                                     |  
  
### <a name="child-elements"></a>Elementos filho

Nenhum
  
### <a name="parent-elements"></a>Elementos pai  
  
| Elemento         | Descrição                                                                                                       |  
| --------------- | ----------------------------------------------------------------------------------------------------------------- |  
| `configuration` | O elemento raiz em cada arquivo de configuração usado pelos aplicativos do Common Language Runtime e .NET Framework. |  
| `runtime`       | Contém informações sobre opções de inicialização do runtime.                                                        |  
  
## <a name="remarks"></a>Comentários  

A partir do .NET Framework 4,6, o Common Language Runtime usa um novo compilador de 64 bits para compilação JIT (just-in-time) por padrão. Em alguns casos, isso pode resultar em uma diferença no comportamento do código do aplicativo que foi compilado por JIT pela versão anterior do compilador JIT de 64 bits. Ao definir o `enabled` atributo do `<useLegacyJit>` elemento como `1` , você pode desabilitar o novo compilador JIT de 64 bits e, em vez disso, compilar seu aplicativo usando o compilador JIT de 64 bits herdado.  
  
> [!NOTE]
> O `<useLegacyJit>` elemento afeta apenas a compilação JIT de 64 bits. A compilação com o compilador JIT de 32 bits não é afetada.  
  
Em vez de usar uma configuração de arquivo de configuração, você pode habilitar o compilador JIT de 64 bits herdado de duas outras maneiras:  
  
- Definindo uma variável de ambiente

  Defina a `COMPLUS_useLegacyJit` variável de ambiente como `0` (use o novo compilador jit de 64 bits) ou `1` (use o compilador JIT de 64 bits mais antigo):
  
  ```env  
  COMPLUS_useLegacyJit=0|1  
  ```  
  
  A variável de ambiente tem *escopo global*, o que significa que ele afeta todos os aplicativos executados no computador. Se definido, ele pode ser substituído pela configuração do arquivo de configuração do aplicativo. O nome da variável de ambiente não diferencia maiúsculas de minúsculas.
  
- Adicionando uma chave do registro

  Você pode habilitar o compilador JIT de 64 bits herdado adicionando um `REG_DWORD` valor à `HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\.NETFramework` `HKEY_CURRENT_USER\SOFTWARE\Microsoft\.NETFramework` chave ou no registro. O valor é nomeado `useLegacyJit` . Se o valor for 0, o novo compilador será usado. Se o valor for 1, o compilador JIT de 64 bits herdado será habilitado. O nome do valor do registro não diferencia maiúsculas de minúsculas.
  
  Adicionar o valor à `HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\.NETFramework` chave afeta todos os aplicativos em execução no computador. Adicionar o valor à `HKEY_CURRENT_USER\SOFTWARE\Microsoft\.NETFramework` chave afeta todos os aplicativos executados pelo usuário atual. Se um computador estiver configurado com várias contas de usuário, somente os aplicativos executados pelo usuário atual serão afetados, a menos que o valor seja adicionado às chaves do registro para outros usuários também. A adição do `<useLegacyJit>` elemento a um arquivo de configuração substitui as configurações do registro, se estiverem presentes.  
  
## <a name="example"></a>Exemplo  

O arquivo de configuração a seguir desabilita a compilação com o novo compilador JIT de 64 bits e, em vez disso, usa o compilador JIT de 64 bits herdado.  
  
```xml  
<?xml version ="1.0"?>  
<configuration>  
  <runtime>  
    <useLegacyJit enabled="1" />  
  </runtime>  
</configuration>  
```  
  
## <a name="see-also"></a>Confira também

- [\<runtime>Elementos](runtime-element.md)
- [\<configuration>Elementos](../configuration-element.md)
- [Mitigação: novo compilador JIT de 64 bits](../../../migration-guide/mitigation-new-64-bit-jit-compiler.md)
