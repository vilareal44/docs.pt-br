---
title: Interface IMetaDataEmit
ms.date: 03/30/2017
api_name:
- IMetaDataEmit
api_location:
- mscoree.dll
api_type:
- COM
f1_keywords:
- IMetaDataEmit
helpviewer_keywords:
- IMetaDataEmit interface [.NET Framework metadata]
ms.assetid: 3b48fd47-7397-4e2c-8bec-8157aa08978c
topic_type:
- apiref
ms.openlocfilehash: a2c2512abc28f0140fc261c5136c7e1255db96de
ms.sourcegitcommit: 03fec33630b46e78d5e81e91b40518f32c4bd7b5
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/27/2020
ms.locfileid: "84009204"
---
# <a name="imetadataemit-interface"></a>Interface IMetaDataEmit
Fornece métodos para criar, modificar e salvar metadados sobre o assembly no escopo definido no momento. Os metadados podem ser armazenados na memória ou salvos em disco.  
  
## <a name="methods"></a>Métodos  
  
|Método|Descrição|  
|------------|-----------------|  
|[Método ApplyEditAndContinue](imetadataemit-applyeditandcontinue-method.md)|Atualiza o escopo do assembly atual com as alterações feitas no especificado `pImport` .|  
|[Método DefineCustomAttribute](imetadataemit-definecustomattribute-method.md)|Cria uma definição para um atributo personalizado com a assinatura de metadados especificada, a ser anexada ao objeto especificado e Obtém um token para essa definição de atributo personalizado.|  
|[Método DefineEvent](imetadataemit-defineevent-method.md)|Cria uma definição para um evento com a assinatura de metadados especificada e Obtém um token para essa definição de evento.|  
|[Método DefineField](imetadataemit-definefield-method.md)|Cria uma definição para um campo com a assinatura de metadados especificada e Obtém um token para essa definição de campo.|  
|[Método DefineImportMember](imetadataemit-defineimportmember-method.md)|Cria uma definição para um membro de um tipo que é definido em um módulo fora do escopo atual e Obtém um token para essa definição de referência.|  
|[Método DefineImportType](imetadataemit-defineimporttype-method.md)|Cria uma definição para uma referência a um tipo que é definido em um módulo fora do escopo atual e Obtém um token para essa definição de referência.|  
|[Método DefineMemberRef](imetadataemit-definememberref-method.md)|Cria uma definição para uma referência a um membro de um módulo fora do escopo atual e Obtém um token para essa definição de referência.|  
|[Método DefineMethod](imetadataemit-definemethod-method.md)|Cria uma definição para um método com a assinatura especificada e retorna um token para essa definição de método.|  
|[Método DefineMethodImpl](imetadataemit-definemethodimpl-method.md)|Cria uma definição para implementação de um método herdado de uma interface e retorna um token para essa definição de implementação de método.|  
|[Método DefineModuleRef](imetadataemit-definemoduleref-method.md)|Cria a assinatura de metadados para um módulo com o nome especificado.|  
|[Método DefineNestedType](imetadataemit-definenestedtype-method.md)|Cria a assinatura de metadados de uma definição de tipo e retorna um `mdTypeDef` token para esse tipo, especificando também que o tipo definido é um membro do tipo referenciado por `tdEncloser` .|  
|[Método DefineParam](imetadataemit-defineparam-method.md)|Cria uma definição de parâmetro com a assinatura especificada para o método referenciado pelo token especificado e Obtém um token para essa definição de parâmetro.|  
|[Método DefinePermissionSet](imetadataemit-definepermissionset-method.md)|Cria uma definição para um conjunto de permissões com a assinatura de metadados especificada e Obtém um token para essa definição de conjunto de permissões.|  
|[Método DefinePinvokeMap](imetadataemit-definepinvokemap-method.md)|Define os recursos da assinatura PInvoke do método referenciado pelo token especificado.|  
|[Método DefineProperty](imetadataemit-defineproperty-method.md)|Cria uma definição de propriedade para o tipo especificado, com os `get` `set` acessadores de método e especificados, e Obtém um token para essa definição de propriedade.|  
|[Método DefineSecurityAttributeSet](imetadataemit-definesecurityattributeset-method.md)|Cria um conjunto de permissões de segurança para anexar ao objeto referenciado pelo token especificado.|  
|[Método DefineTypeDef](imetadataemit-definetypedef-method.md)|Cria uma definição de tipo para um tipo de Common Language Runtime e Obtém um token de metadados para essa definição de tipo.|  
|[Método DefineTypeRefByName](imetadataemit-definetyperefbyname-method.md)|Obtém um token de metadados para um tipo que é definido em outro módulo fora do escopo atual.|  
|[Método DefineUserString](imetadataemit-defineuserstring-method.md)|Obtém um token de metadados para a cadeia de caracteres literal especificada.|  
|[Método DeleteClassLayout](imetadataemit-deleteclasslayout-method.md)|Destrói a assinatura de metadados de layout de classe para o tipo referenciado pelo token especificado.|  
|[Método DeleteFieldMarshal](imetadataemit-deletefieldmarshal-method.md)|Destrói a assinatura de metadados de marshaling do PInvoke para o objeto referenciado pelo token especificado.|  
|[Método DeletePinvokeMap](imetadataemit-deletepinvokemap-method.md)|Destrói os metadados de mapeamento do PInvoke para o objeto referenciado pelo token especificado.|  
|[Método DeleteToken](imetadataemit-deletetoken-method.md)|Exclui o token especificado do escopo de metadados atual.|  
|[Método GetSaveSize](imetadataemit-getsavesize-method.md)|Obtém o tamanho binário estimado do assembly no escopo atual.|  
|[Método GetTokenFromSig](imetadataemit-gettokenfromsig-method.md)|Obtém um token para a assinatura de metadados especificada.|  
|[Método GetTokenFromTypeSpec](imetadataemit-gettokenfromtypespec-method.md)|Obtém um token de metadados para o tipo com a assinatura de metadados especificada.|  
|[Método de Mesclagem](imetadataemit-merge-method.md)|Adiciona o escopo importado especificado à lista de escopos a serem mesclados.|  
|[Método MergeEnd](imetadataemit-mergeend-method.md)|Mescla no escopo atual todos os escopos de metadados especificados por uma ou mais chamadas anteriores para `IMetaDataEmit::Merge` .|  
|[Método Save](imetadataemit-save-method.md)|Salva todos os metadados no escopo atual para o arquivo no endereço especificado.|  
|[Método SaveToMemory](imetadataemit-savetomemory-method.md)|Salva todos os metadados no escopo atual para a área especificada da memória.|  
|[Método SaveToStream](imetadataemit-savetostream-method.md)|Salva todos os metadados no escopo atual para o especificado `IStream` .|  
|[Método SetClassLayout](imetadataemit-setclasslayout-method.md)|Define ou atualiza a assinatura de layout de classe de um tipo definido por uma chamada anterior para `IMetaDataEmit::DefineTypeDef` .|  
|[Método SetCustomAttributeValue](imetadataemit-setcustomattributevalue-method.md)|Define ou atualiza o valor de um atributo personalizado definido por uma chamada anterior para `IMetaDataEmit::DefineCustomAttribute` .|  
|[Método SetEventProps](imetadataemit-seteventprops-method.md)|Define ou atualiza o recurso especificado de um evento definido por uma chamada anterior para `IMetaDataEmit::DefineEvent` .|  
|[Método SetFieldMarshal](imetadataemit-setfieldmarshal-method.md)|Define as informações de marshaling do PInvoke para o campo, o retorno do método ou o parâmetro do método referenciado pelo token especificado.|  
|[Método SetFieldProps](imetadataemit-setfieldprops-method.md)|Define ou atualiza o valor padrão para o campo referenciado pelo token do campo especificado.|  
|[Método SetFieldRVA](imetadataemit-setfieldrva-method.md)|Define um valor de variável global para o endereço virtual relativo do campo referenciado pelo token especificado.|  
|[Método SetHandler](imetadataemit-sethandler-method.md)|Define o método referenciado pelo `IUnknown` ponteiro especificado como um retorno de chamada de notificação para remapeamentos de token.|  
|[Método SetMethodImplFlags](imetadataemit-setmethodimplflags-method.md)|Define ou atualiza a assinatura de metadados da implementação do método herdado referenciado pelo token especificado.|  
|[Método SetMethodProps](imetadataemit-setmethodprops-method.md)|Define ou atualiza o recurso, armazenado no endereço virtual relativo especificado, de um método definido por uma chamada anterior para `IMetaDataEmit::DefineMethod` .|  
|[Método SetModuleProps](imetadataemit-setmoduleprops-method.md)|Atualiza referências a um módulo definido por uma chamada anterior para `IMetaDataEmit::DefineModuleRef` .|  
|[Método SetParamProps](imetadataemit-setparamprops-method.md)|Define ou altera recursos de um parâmetro de método que foi definido por uma chamada anterior para `IMetaDataEmit::DefineParam` .|  
|[Método SetParent](imetadataemit-setparent-method.md)|Estabelece que o membro especificado, conforme definido por uma chamada anterior para `IMetaDataEmit::DefineMemberRef` , é um membro do tipo especificado, conforme definido por uma chamada anterior para `IMetaDataEmit::DefineTypeDef` .|  
|[Método SetPermissionSetProps](imetadataemit-setpermissionsetprops-method.md)|Define ou atualiza recursos da assinatura de metadados de um conjunto de permissões definido por uma chamada anterior para `IMetaDataEmit::DefinePermissionSet` .|  
|[Método SetPinvokeMap](imetadataemit-setpinvokemap-method.md)|Define ou altera recursos da assinatura do PInvoke de um método, conforme definido por uma chamada anterior para `IMetaDataEmit::DefinePinvokeMap` .|  
|[Método SetPropertyProps](imetadataemit-setpropertyprops-method.md)|Define os recursos armazenados em metadados para uma propriedade definida por uma chamada anterior para `IMetaDataEmit::DefineProperty` .|  
|[Método SetRVA](imetadataemit-setrva-method.md)|Define o endereço virtual relativo do método especificado.|  
|[Método SetTypeDefProps](imetadataemit-settypedefprops-method.md)|Define recursos de um tipo definido por uma chamada anterior para `IMetaDataEmit::DefineTypeDef` .|  
|[Método TranslateSigWithScope](imetadataemit-translatesigwithscope-method.md)|Importa um assembly para o escopo atual e obtém uma nova assinatura de metadados para o escopo mesclado.|  
  
## <a name="requirements"></a>Requisitos  
 **Plataformas:** confira [Requisitos do sistema](../../get-started/system-requirements.md).  
  
 **Cabeçalho:** Cor. h  
  
 **Biblioteca:** Usado como um recurso em MsCorEE. dll  
  
 **.NET Framework versões:**[!INCLUDE[net_current_v10plus](../../../../includes/net-current-v10plus-md.md)]  
  
## <a name="see-also"></a>Confira também

- [Interfaces de metadados](metadata-interfaces.md)
- [Interface IMetaDataEmit2](imetadataemit2-interface.md)
