---
title: Método IMetaDataEmit::GetSaveSize
ms.date: 03/30/2017
api_name:
- IMetaDataEmit.GetSaveSize
api_location:
- mscoree.dll
api_type:
- COM
f1_keywords:
- IMetaDataEmit::GetSaveSize
helpviewer_keywords:
- IMetaDataEmit::GetSaveSize method [.NET Framework metadata]
- GetSaveSize method [.NET Framework metadata]
ms.assetid: 8aea2e2c-23a3-4cda-9a06-e19f97383830
topic_type:
- apiref
ms.openlocfilehash: 0a283c837e23ab1aafd3545df1dfe8a267de0557
ms.sourcegitcommit: da21fc5a8cce1e028575acf31974681a1bc5aeed
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/08/2020
ms.locfileid: "84501281"
---
# <a name="imetadataemitgetsavesize-method"></a>Método IMetaDataEmit::GetSaveSize
Obtém o tamanho binário estimado do assembly e seus metadados no escopo atual.  
  
## <a name="syntax"></a>Sintaxe  
  
```cpp  
HRESULT GetSaveSize (  
    [in]  CorSaveSize fSave,  
    [out] DWORD       *pdwSaveSize  
);  
```  
  
## <a name="parameters"></a>Parâmetros  
 `fSave`  
 no Um valor da enumeração [CorSaveSize](corsavesize-enumeration.md) que especifica se um tamanho preciso ou aproximado deve ser obtido. Somente três valores são válidos: cssAccurate, cssQuick e cssDiscardTransientCAs:  
  
- cssAccurate retorna o tamanho exato da gravação, mas leva mais tempo para calcular.  
  
- cssQuick retorna um tamanho, preenchido para segurança, mas leva menos tempo para calcular.  
  
- cssDiscardTransientCAs informa `GetSaveSize` que pode gerar atributos personalizados descartados.  
  
 `pdwSaveSize`  
 fora Um ponteiro para o tamanho necessário para salvar o arquivo.  
  
## <a name="remarks"></a>Comentários  
 `GetSaveSize`Calcula o espaço necessário, em bytes, para salvar o assembly e todos os seus metadados no escopo atual. (Uma chamada para o método [IMetaDataEmit:: SaveToStream](imetadataemit-savetostream-method.md) emitiria esse número de bytes.)  
  
 Se o chamador implementar a interface [IMapToken](imaptoken-interface.md) (por meio de [IMetaDataEmit:: SetHandler](imetadataemit-sethandler-method.md) ou [IMetaDataEmit:: Merge](imetadataemit-merge-method.md)), o `GetSaveSize` executará duas passagens sobre os metadados para otimizá-la e compactá-la. Caso contrário, nenhuma otimização será executada.  
  
 Se a otimização for executada, a primeira passagem simplesmente classificará as estruturas de metadados para ajustar o desempenho das pesquisas de tempo de importação. Em geral, essa etapa resulta na movimentação de registros, com o efeito colateral de que os tokens retidos pela ferramenta para referência futura são invalidados. No entanto, os metadados não informam o chamador dessas alterações de token até depois da segunda passagem. Na segunda passagem, são executadas várias otimizações que se destinam a reduzir o tamanho geral dos metadados, como a otimização de ausência (Associação antecipada) `mdTypeRef` e `mdMemberRef` os tokens quando a referência é para um tipo ou membro declarado no escopo de metadados atual. Nessa passagem, ocorre outra rodada de mapeamento de token. Após essa passagem, o mecanismo de metadados notifica o chamador, por meio `IMapToken` de sua interface, de quaisquer valores de token alterados.  
  
## <a name="requirements"></a>Requisitos  
 **Plataformas:** confira [Requisitos do sistema](../../get-started/system-requirements.md).  
  
 **Cabeçalho:** Cor. h  
  
 **Biblioteca:** Usado como um recurso em MSCorEE. dll  
  
 **.NET Framework versões:**[!INCLUDE[net_current_v11plus](../../../../includes/net-current-v11plus-md.md)]  
  
## <a name="see-also"></a>Confira também

- [Interface IMetaDataEmit](imetadataemit-interface.md)
- [Interface IMetaDataEmit2](imetadataemit2-interface.md)
