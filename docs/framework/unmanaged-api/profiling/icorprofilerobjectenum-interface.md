---
title: Interface ICorProfilerObjectEnum
ms.date: 03/30/2017
api_name:
- ICorProfilerObjectEnum
api_location:
- mscorwks.dll
api_type:
- COM
f1_keywords:
- ICorProfilerObjectEnum
helpviewer_keywords:
- ICorProfilerObjectEnum interface [.NET Framework profiling]
ms.assetid: 13e1651c-9523-40ef-bfd7-87fb94519f8b
topic_type:
- apiref
ms.openlocfilehash: 5ebe99dd8d1d7ec73cd140991a4b13dfa381791d
ms.sourcegitcommit: da21fc5a8cce1e028575acf31974681a1bc5aeed
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/08/2020
ms.locfileid: "84494638"
---
# <a name="icorprofilerobjectenum-interface"></a>Interface ICorProfilerObjectEnum
Fornece métodos para iterar em sequência por meio de uma coleção de objetos congelados que são gerados pelo [NGen. exe (gerador de imagem nativa)](../../tools/ngen-exe-native-image-generator.md).  
  
## <a name="methods"></a>Métodos  
  
|Método|Descrição|  
|------------|-----------------|  
|[Método Clone](icorprofilerobjectenum-clone-method.md)|Obtém um ponteiro de interface para uma cópia desta `ICorProfilerObjectEnum` interface.|  
|[Método GetCount](icorprofilerobjectenum-getcount-method.md)|Obtém o número total de objetos congelados na coleção.|  
|[Método Next](icorprofilerobjectenum-next-method.md)|Obtém o número especificado de objetos contíguos de uma coleção sequencial de objetos, começando na posição atual do enumerador na sequência.|  
|[Método Reset](icorprofilerobjectenum-reset-method.md)|Move o cursor deste enumerador para a posição inicial da sequência.|  
|[Método Skip](icorprofilerobjectenum-skip-method.md)|Avança o cursor deste enumerador de sua posição atual para que o número especificado de elementos seja ignorado.|  
  
## <a name="remarks"></a>Comentários  
 A `ICorProfilerObjectEnum` interface é um enumerador. Ele permite que o destinatário de uma matriz Extraia elementos do remetente a uma taxa apropriada para o destinatário. Em outras palavras, o receptor é capaz de controlar explicitamente o fluxo de elementos de matriz, evitando assim os problemas relacionados à passagem de matrizes grandes como parâmetros de método.  
  
 Use [ICorProfilerInfo2:: EnumModuleFrozenObjects](icorprofilerinfo2-enummodulefrozenobjects-method.md) para obter um ponteiro para a `ICorProfilerObjectEnum` interface.  
  
## <a name="requirements"></a>Requisitos  
 **Plataformas:** confira [Requisitos do sistema](../../get-started/system-requirements.md).  
  
 **Cabeçalho:** CorProf. idl, CorProf. h  
  
 **Biblioteca:** CorGuids.lib  
  
 **.NET Framework versões:**[!INCLUDE[net_current_v20plus](../../../../includes/net-current-v20plus-md.md)]  
  
## <a name="see-also"></a>Confira também

- [Criação de perfil de interfaces](profiling-interfaces.md)
- [Método EnumModuleFrozenObjects](icorprofilerinfo2-enummodulefrozenobjects-method.md)
