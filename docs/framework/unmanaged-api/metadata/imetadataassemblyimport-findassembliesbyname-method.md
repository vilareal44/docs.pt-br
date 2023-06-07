---
title: Método IMetaDataAssemblyImport::FindAssembliesByName
ms.date: 03/30/2017
api_name:
- IMetaDataAssemblyImport.FindAssembliesByName
api_location:
- mscoree.dll
api_type:
- COM
f1_keywords:
- IMetaDataAssemblyImport::FindAssembliesByName
helpviewer_keywords:
- FindAssembliesByName method [.NET Framework metadata]
- IMetaDataAssemblyImport::FindAssembliesByName method [.NET Framework metadata]
ms.assetid: 4db97cf9-e4c1-4233-8efa-cbdc0e14a8e4
topic_type:
- apiref
ms.openlocfilehash: d2693a94f02214df6d7265b26e3d70d91adcf8a7
ms.sourcegitcommit: da21fc5a8cce1e028575acf31974681a1bc5aeed
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/08/2020
ms.locfileid: "84503829"
---
# <a name="imetadataassemblyimportfindassembliesbyname-method"></a>Método IMetaDataAssemblyImport::FindAssembliesByName
Obtém uma matriz de assemblies com o `szAssemblyName` parâmetro especificado, usando as regras padrão empregadas pelo Common Language Runtime (CLR) para resolver referências.  
  
## <a name="syntax"></a>Sintaxe  
  
```cpp  
HRESULT FindAssembliesByName (  
    [in]  LPCWSTR     szAppBase,
    [in]  LPCWSTR     szPrivateBin,
    [in]  LPCWSTR     szAssemblyName,
    [out] IUnknown    *ppIUnk[],
    [in]  ULONG       cMax,
    [out] ULONG       *pcAssemblies  
);  
```  
  
## <a name="parameters"></a>Parâmetros  
 `szAppBase`  
 no O diretório raiz no qual Pesquisar o assembly fornecido. Se esse valor for definido como `null` , o `FindAssembliesByName` irá procurar somente no cache de assembly global do assembly.  
  
 `szPrivateBin`  
 no Uma lista de subdiretórios delimitados por ponto-e-vírgula (por exemplo, "bin; BIN2"), sob o diretório raiz, no qual Pesquisar o assembly. Esses diretórios são investigados além daqueles especificados nas regras de investigação padrão.  
  
 `szAssemblyName`  
 no O nome do assembly a ser localizado. O formato dessa cadeia de caracteres é definido na página de referência de classe para <xref:System.Reflection.AssemblyName> .  
  
 `ppIUnk`  
 no Uma matriz do tipo [IUnknown](/cpp/atl/iunknown) na qual colocar os `IMetadataAssemblyImport` ponteiros de interface.  
  
 `cMax`  
 fora O número máximo de ponteiros de interface que podem ser colocados em `ppIUnk` .  
  
 `pcAssemblies`  
 fora O número de ponteiros de interface retornados. Ou seja, o número de ponteiros de interface inseridos na verdade `ppIUnk` .  
  
## <a name="return-value"></a>Valor Retornado  
  
|HRESULT|Descrição|  
|-------------|-----------------|  
|`S_OK`|`FindAssembliesByName`retornado com êxito.|  
|`S_FALSE`|Não há assemblies.|  
  
## <a name="remarks"></a>Comentários  
 Dado um nome de assembly, o `FindAssembliesByName` método localiza o assembly seguindo as regras padrão para resolver referências de assembly. (Para obter mais informações, consulte [como o tempo de execução localiza assemblies](../../deployment/how-the-runtime-locates-assemblies.md).) `FindAssembliesByName`permite que o chamador configure vários aspectos do contexto do resolvedor de assembly, como base do aplicativo e caminho de pesquisa particular.  
  
 O `FindAssembliesByName` método requer que o CLR seja inicializado no processo para invocar a lógica de resolução do assembly. Portanto, você deve chamar o [codeinitialize](../hosting/coinitializeee-function.md) (passando COINITEE_DEFAULT) antes de chamar `FindAssembliesByName` e, em seguida, seguir com uma chamada para [CoUninitializeCor](../hosting/couninitializecor-function.md).  
  
 `FindAssembliesByName`Retorna um ponteiro [IMetaDataImport](imetadataimport-interface.md) para o arquivo que contém o manifesto do assembly para o nome do assembly que é passado. Se o nome do assembly fornecido não estiver totalmente especificado (por exemplo, se não incluir uma versão), vários assemblies poderão ser retornados.  
  
 `FindAssembliesByName`é comumente usado por um compilador que tenta localizar um assembly referenciado no momento da compilação.  
  
## <a name="requirements"></a>Requisitos  
 **Plataformas:** confira [Requisitos do sistema](../../get-started/system-requirements.md).  
  
 **Cabeçalho:** Cor. h  
  
 **Biblioteca:** Usado como um recurso em MsCorEE. dll  
  
 **.NET Framework versões:**[!INCLUDE[net_current_v10plus](../../../../includes/net-current-v10plus-md.md)]  
  
## <a name="see-also"></a>Confira também

- [Como o runtime localiza assemblies](../../deployment/how-the-runtime-locates-assemblies.md)
- [Interface IMetaDataAssemblyImport](imetadataassemblyimport-interface.md)
