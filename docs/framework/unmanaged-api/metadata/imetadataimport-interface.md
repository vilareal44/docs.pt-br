---
title: Interface IMetaDataImport
ms.date: 03/30/2017
api_name:
- IMetaDataImport
api_location:
- mscoree.dll
api_type:
- COM
f1_keywords:
- IMetaDataImport
helpviewer_keywords:
- IMetaDataImport interface [.NET Framework metadata]
ms.assetid: 0adbbd35-5e8d-4fec-8268-dc70a07c5975
topic_type:
- apiref
ms.openlocfilehash: 02d1ea1ef12fa158ce7ec94aeca4356ac54d4e5f
ms.sourcegitcommit: da21fc5a8cce1e028575acf31974681a1bc5aeed
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/08/2020
ms.locfileid: "84503478"
---
# <a name="imetadataimport-interface"></a>Interface IMetaDataImport
Fornece métodos para importar e manipular metadados existentes de um arquivo executável portátil (PE) ou outra fonte, como uma biblioteca de tipos ou um binário de metadados de tempo de execução autônomo.  
  
## <a name="methods"></a>Métodos  
  
|Método|Descrição|  
|------------|-----------------|  
|[Método CloseEnum](imetadataimport-closeenum-method.md)|Fecha o enumerador com o identificador especificado.|  
|[Método CountEnum](imetadataimport-countenum-method.md)|Obtém o número de elementos no enumerador com o identificador especificado.|  
|[Método EnumCustomAttributes](imetadataimport-enumcustomattributes-method.md)|Enumera uma lista de tokens de definição de atributo personalizados associados ao tipo ou membro especificado.|  
|[Método EnumEvents](imetadataimport-enumevents-method.md)|Enumera os tokens de definição de evento para o token de TypeDef especificado.|  
|[Método EnumFields](imetadataimport-enumfields-method.md)|Enumera os tokens FieldDef para o tipo referenciado pelo token de TypeDef especificado.|  
|[Método EnumFieldsWithName](imetadataimport-enumfieldswithname-method.md)|Enumera os tokens FieldDef do tipo especificado com o nome especificado.|  
|[Método EnumInterfaceImpls](imetadataimport-enuminterfaceimpls-method.md)|Enumera tokens MethodDef que representam implementações de interface.|  
|[Método EnumMemberRefs](imetadataimport-enummemberrefs-method.md)|Enumera os tokens de MemberRef que representam os membros do tipo especificado.|  
|[Método EnumMembers](imetadataimport-enummembers-method.md)|Enumera os tokens MemberDef que representam os membros do tipo especificado.|  
|[Método EnumMembersWithName](imetadataimport-enummemberswithname-method.md)|Enumera os tokens MemberDef que representam os membros do tipo especificado com o nome especificado.|  
|[Método EnumMethodImpls](imetadataimport-enummethodimpls-method.md)|Enumera os tokens MethodBody e MethodDeclaration que representam métodos do tipo especificado.|  
|[Método EnumMethods](imetadataimport-enummethods-method.md)|Enumera tokens MethodDef que representam métodos do tipo especificado.|  
|[Método EnumMethodSemantics](imetadataimport-enummethodsemantics-method.md)|Enumera as propriedades e os eventos de alteração de propriedade aos quais o método especificado está relacionado.|  
|[Método EnumMethodsWithName](imetadataimport-enummethodswithname-method.md)|Enumera os métodos que têm o nome especificado e que são definidos pelo tipo referenciado pelo token de TypeDef especificado.|  
|[Método EnumModuleRefs](imetadataimport-enummodulerefs-method.md)|Enumera tokens ModuleRef que representam módulos importados.|  
|[Método EnumParams](imetadataimport-enumparams-method.md)|Enumera os tokens ParamDef que representam os parâmetros do método referenciado pelo token MethodDef especificado.|  
|[Método EnumPermissionSets](imetadataimport-enumpermissionsets-method.md)|Enumera permissões para os objetos em um escopo de metadados especificado.|  
|[Método EnumProperties](imetadataimport-enumproperties-method.md)|Enumera os tokens PropertyDef que representam as propriedades do tipo referenciado pelo token de TypeDef especificado.|  
|[Método EnumSignatures](imetadataimport-enumsignatures-method.md)|Enumera os tokens de assinatura que representam assinaturas autônomas no escopo atual.|  
|[Método EnumTypeDefs](imetadataimport-enumtypedefs-method.md)|Enumera os tokens de TypeDef que representam todos os tipos no escopo atual.|  
|[Método EnumTypeRefs](imetadataimport-enumtyperefs-method.md)|Enumera tokens TypeRef definidos no escopo de metadados atual.|  
|[Método EnumTypeSpecs](imetadataimport-enumtypespecs-method.md)|Enumera os tokens de TypeSpec definidos no escopo de metadados atual.|  
|[Método EnumUnresolvedMethods](imetadataimport-enumunresolvedmethods-method.md)|Enumera os tokens MemberDef que representam os métodos não resolvidos no escopo de metadados atual.|  
|[Método EnumUserStrings](imetadataimport-enumuserstrings-method.md)|Enumera os tokens de cadeia de caracteres que representam cadeias embutidas em código no escopo de metadados atual.|  
|[Método FindField](imetadataimport-findfield-method.md)|Obtém o token FieldDef para o campo que é um membro do tipo especificado e tem o nome e a assinatura de metadados especificados.|  
|[Método FindMember](imetadataimport-findmember-method.md)|Obtém um ponteiro para o token MemberDef para o membro definido pelo tipo especificado com o nome especificado e a assinatura de metadados.|  
|[Método FindMemberRef](imetadataimport-findmemberref-method.md)|Obtém um ponteiro para o token de MemberRef do membro definido pelo tipo especificado com o nome e a assinatura de metadados especificados.|  
|[Método FindMethod](imetadataimport-findmethod-method.md)|Obtém um ponteiro para o token MethodDef para o método definido pelo tipo especificado com o nome especificado e a assinatura de metadados.|  
|[Método FindTypeDefByName](imetadataimport-findtypedefbyname-method.md)|Obtém um ponteiro para o token de metadados de TypeDef para o tipo com o nome especificado.|  
|[Método FindTypeRef](imetadataimport-findtyperef-method.md)|Obtém um ponteiro para o token de metadados TypeRef que faz referência ao tipo no escopo de pesquisa especificado com o nome especificado.|  
|[Método GetClassLayout](imetadataimport-getclasslayout-method.md)|Obtém informações de layout para a classe referenciada pelo token de TypeDef especificado.|  
|[Método GetCustomAttributeByName](imetadataimport-getcustomattributebyname-method.md)|Obtém o valor do atributo personalizado, dado seu nome.|  
|[Método GetCustomAttributeProps](imetadataimport-getcustomattributeprops-method.md)|Obtém o valor do atributo personalizado, dado seu token de metadados.|  
|[Método GetEventProps](imetadataimport-geteventprops-method.md)|Obtém informações de metadados (incluindo o tipo declarativo, os métodos Add e remove para delegados e quaisquer sinalizadores e outros dados associados) para o evento representado pelo token de evento especificado.|  
|[Método GetFieldMarshal](imetadataimport-getfieldmarshal-method.md)|Obtém um ponteiro para o tipo nativo não gerenciado do campo representado pelo token de metadados do campo especificado.|  
|[Método GetFieldProps](imetadataimport-getfieldprops-method.md)|Obtém os metadados associados ao campo referenciado pelo token FieldDef especificado.|  
|[Método GetInterfaceImplProps](imetadataimport-getinterfaceimplprops-method.md)|Obtém um ponteiro para os tokens de metadados para o tipo que implementa o método especificado e para a interface que declara esse método.|  
|[Método GetMemberProps](imetadataimport-getmemberprops-method.md)|Obtém informações de metadados (incluindo o nome, a assinatura binária e o endereço virtual relativo) do membro de tipo referenciado pelo token de metadados especificado.|  
|[Método GetMemberRefProps](imetadataimport-getmemberrefprops-method.md)|Obtém os metadados associados ao membro referenciado pelo token especificado.|  
|[Método GetMethodProps](imetadataimport-getmethodprops-method.md)|Obtém os metadados associados ao método referenciado pelo token MethodDef especificado.|  
|[Método GetMethodSemantics](imetadataimport-getmethodsemantics-method.md)|Obtém um ponteiro para a relação entre o método referenciado pelo token MethodDef especificado e a propriedade emparelhada e o evento referenciado pelo token EventProp especificado.|  
|[Método GetModuleFromScope](imetadataimport-getmodulefromscope-method.md)|Obtém um ponteiro para o token de metadados do módulo referenciado no escopo de metadados atual.|  
|[Método GetModuleRefProps](imetadataimport-getmodulerefprops-method.md)|Obtém o nome do módulo referenciado pelo token de metadados especificado.|  
|[Método GetNameFromToken](imetadataimport-getnamefromtoken-method.md)|Obtém o nome UTF-8 do objeto referenciado pelo token de metadados especificado.|  
|[Método GetNativeCallConvFromSig](imetadataimport-getnativecallconvfromsig-method.md)|Obtém a Convenção de chamada nativa para o método representado pelo ponteiro de assinatura especificado.|  
|[Método GetNestedClassProps](imetadataimport-getnestedclassprops-method.md)|Obtém o token de TypeDef para o tipo pai delimitador do tipo aninhado especificado.|  
|[Método GetParamForMethodIndex](imetadataimport-getparamformethodindex-method.md)|Obtém um ponteiro para o token que representa o parâmetro na posição ordinal especificada na sequência de parâmetros do método para o método representado pelo token MethodDef especificado.|  
|[Método GetParamProps](imetadataimport-getparamprops-method.md)|Obtém valores de metadados para o parâmetro referenciado pelo token ParamDef especificado.|  
|[Método GetPermissionSetProps](imetadataimport-getpermissionsetprops-method.md)|Obtém os metadados associados ao System. Security. PermissionSet representado pelo token de permissão especificado.|  
|[GetPinvokeMap](imetadataimport-getpinvokemap-method.md)|Obtém um token ModuleRef para representar o assembly de destino de uma chamada PInvoke.|  
|[Método GetPropertyProps](imetadataimport-getpropertyprops-method.md)|Obtém os metadados associados à propriedade representada pelo token especificado.|  
|[Método GetRVA](imetadataimport-getrva-method.md)|Obtém o deslocamento do endereço virtual relativo do objeto de código representado pelo token especificado.|  
|[Método GetScopeProps](imetadataimport-getscopeprops-method.md)|Obtém o nome e, opcionalmente, o identificador de versão do assembly ou módulo no escopo de metadados atual.|  
|[Método GetSigFromToken](imetadataimport-getsigfromtoken-method.md)|Obtém a assinatura de metadados binários associada ao token especificado.|  
|[Método GetTypeDefProps](imetadataimport-gettypedefprops-method.md)|Retorna informações de metadados para o tipo representado pelo token de TypeDef especificado.|  
|[Método GetTypeRefProps](imetadataimport-gettyperefprops-method.md)|Obtém os metadados associados ao tipo referenciado pelo token TypeRef especificado.|  
|[Método GetTypeSpecFromToken](imetadataimport-gettypespecfromtoken-method.md)|Obtém a assinatura de metadados binários da especificação de tipo representada pelo token especificado.|  
|[Método GetUserString](imetadataimport-getuserstring-method.md)|Obtém a cadeia de caracteres literal representada pelo token de metadados especificado.|  
|[Método IsGlobal](imetadataimport-isglobal-method.md)|Obtém um valor que indica se o campo, o método ou o tipo representado pelo token de metadados especificado tem escopo global.|  
|[Método IsValidToken](imetadataimport-isvalidtoken-method.md)|Obtém um valor que indica se o token especificado contém uma referência válida a um objeto de código.|  
|[Método ResetEnum](imetadataimport-resetenum-method.md)|Redefine o enumerador especificado para a posição especificada.|  
|[Método ResolveTypeRef](imetadataimport-resolvetyperef-method.md)|Obtém informações de tipo para o tipo referenciado pelo token TypeRef especificado.|  
  
## <a name="remarks"></a>Comentários  
 O design da `IMetaDataImport` interface destina-se principalmente a ser usado por ferramentas e serviços que irão importar informações de tipo (por exemplo, ferramentas de desenvolvimento) ou gerenciar componentes implantados (por exemplo, serviços de resolução/ativação). Os métodos no se `IMetaDataImport` encaixam nas seguintes categorias de tarefas:  
  
- Enumerando coleções de itens no escopo de metadados.  
  
- Encontrar um item que tenha um conjunto específico de características.  
  
- Obtendo propriedades de um item especificado.  
  
- Os métodos get são projetados especificamente para retornar propriedades de valor único de um item de metadados. Quando a propriedade é uma referência a outro item, um token para esse item é retornado. Qualquer tipo de entrada de ponteiro pode ser nulo para indicar que o valor específico não está sendo solicitado. Para obter propriedades que são essencialmente objetos de coleção (por exemplo, a coleção de interfaces que uma classe implementa), use os métodos de enumeração.  
  
## <a name="requirements"></a>Requisitos  
 **Plataformas:** confira [Requisitos do sistema](../../get-started/system-requirements.md).  
  
 **Cabeçalho:** Cor. h  
  
 **Biblioteca:** Usado como um recurso em MsCorEE. dll  
  
 **.NET Framework versões:**[!INCLUDE[net_current_v10plus](../../../../includes/net-current-v10plus-md.md)]  
  
## <a name="see-also"></a>Confira também

- [Interfaces de metadados](metadata-interfaces.md)
- [Interface IMetaDataImport2](imetadataimport2-interface.md)
