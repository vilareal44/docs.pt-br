---
title: Criação de perfil de interfaces
ms.date: 04/10/2018
helpviewer_keywords:
- unmanaged interfaces [.NET Framework], profiling
- profiling interfaces [.NET Framework]
- interfaces [.NET Framework profiling]
ms.assetid: d9303db8-e881-4217-91b7-8c7573c8ef9e
ms.openlocfilehash: f073794b4fdf89f289b70fed9967ee37b5f4e133
ms.sourcegitcommit: da21fc5a8cce1e028575acf31974681a1bc5aeed
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/08/2020
ms.locfileid: "84494027"
---
# <a name="profiling-interfaces"></a>Criação de perfil de interfaces
Esta seção descreve as interfaces não gerenciadas que permitem criar o perfil de um programa que está sendo executado no CLR (Common Language Runtime).  
  
## <a name="in-this-section"></a>Nesta seção  
 [Interface ICLRProfiling](iclrprofiling-interface.md)  
 Fornece o método [AttachProfiler](iclrprofiling-attachprofiler-method.md) , que permite que um criador de perfil anexe a um processo em execução.  
  
 [Interface ICorProfilerAssemblyReferenceProvider](icorprofilerassemblyreferenceprovider-interface.md)  
 Permite que o criador de perfil informe o CLR de referências de assembly que o criador de perfil adicionará ao retorno de chamada [ICorProfilerCallback:: ModuleLoadFinished](icorprofilercallback-moduleloadfinished-method.md) .  
  
 [Interface ICorProfilerCallback](icorprofilercallback-interface.md)  
 Fornece métodos usados pelo CLR para notificar um criador de perfis de código quando ocorrerem os eventos assinados pelo criador de perfis.  
  
 [Interface ICorProfilerCallback2](icorprofilercallback2-interface.md)  
 Estende a interface `ICorProfilerCallback` com retornos de chamadas com suporte no .NET Framework 2.0 e versões posteriores.  
  
 [Interface ICorProfilerCallback3](icorprofilercallback3-interface.md)  
 Fornece métodos de retorno de chamada que o CLR usa para comunicar anexação e desanexação de informações de estado ao criador de perfis.  
  
 [Interface ICorProfilerCallback4](icorprofilercallback4-interface.md)  
 Fornece métodos de retorno de chamada que o CLR usa para comunicar informações ao criador de perfis.  
  
 [Interface ICorProfilerCallback5](icorprofilercallback5-interface.md)  
 Fornece um método que identifica o fechamento transitivo de objetos referenciados por raízes de coleta de lixo.  
  
 [Interface ICorProfilerCallback6](icorprofilercallback6-interface.md)  
 Fornece um método de retorno de chamada que o CLR usa para notificar um criador de perfis de que um assembly está carregando.  
  
 [Interface ICorProfilerCallback7](icorprofilercallback7-interface.md)  
 Fornece um método de retorno de chamada que o Common Language Runtime usa para notificar o criador de perfil de que o fluxo de símbolos associado a um módulo na memória é atualizado.  

[Interface ICorProfilerCallback8](icorprofilercallback8-interface.md)  
Fornece métodos de retorno de chamada que o Common Language Runtime usa para notificar o criador de perfil de que a compilação JIT de um método dinâmico foi iniciada e concluída.

[Interface ICorProfilerCallback9](icorprofilercallback9-interface.md)  
Fornece um método de retorno de chamada que o Common Language Runtime usa para notificar o criador de perfil de que um método dinâmico é lixo coletado e subsequentemente descarregado.

 [Interface ICorProfilerFunctionControl](icorprofilerfunctioncontrol-interface.md)  
 Fornece métodos que permitem um criador de perfis de código se comunicar com o CLR para controlar como o compilador JIT deve gerar código ao recompilar um método específico.  
  
 [Interface ICorProfilerFunctionEnum](icorprofilerfunctionenum-interface.md)  
 Fornece métodos para iterar de forma sequencial por meio de uma coleção de funções no CLR.  
  
 [Interface ICorProfilerInfo](icorprofilerinfo-interface.md)  
 Fornece métodos para uso por criadores de perfis de código para se comunicar com o CLR para controlar o monitoramento de eventos e solicitar informações.  
  
 [Interface ICorProfilerInfo2](icorprofilerinfo2-interface.md)  
 Estende a interface de `ICorProfilerInfo` com métodos que têm suporte no .NET Framework 2.0 e em versões posteriores.  
  
 [Interface ICorProfilerInfo3](icorprofilerinfo3-interface.md)  
 Estende a `ICorProfilerInfo2` interface com métodos com suporte no .NET Framework 4 e versões posteriores.  
  
 [Interface ICorProfilerInfo4](icorprofilerinfo4-interface.md)  
 Fornece métodos que os criadores de perfis de código usam para se comunicar com o CLR para controlar o monitoramento de eventos e solicitar informações.  
  
 [Interface ICorProfilerInfo5](icorprofilerinfo5-interface.md)  
 Fornece métodos para uso por criadores de perfis de código para se comunicar com o CLR para controlar o monitoramento de eventos.  
  
 [Interface ICorProfilerInfo6](icorprofilerinfo6-interface.md)  
 Fornece um enumerador para todos os métodos que pertencem a um determinado módulo NGen e que são embutidos no corpo de um determinado método.  
  
 [Interface ICorProfilerInfo7](icorprofilerinfo7-interface.md)  
 Fornece um método para aplicar metadados definidos recentemente a um módulo e que fornece acesso a um fluxo de símbolo na memória.  
  
 [Interface ICorProfilerModuleEnum](icorprofilermoduleenum-interface.md)  
 Fornece métodos para iterar de forma sequencial por meio de uma coleção de módulos carregados pelo aplicativo ou pelo criador de perfis.  
  
 [Interface ICorProfilerObjectEnum](icorprofilerobjectenum-interface.md)  
 Fornece métodos para iterar em sequência por meio de uma coleção de objetos congelados que são gerados pelo [NGen. exe (gerador de imagem nativa)](../../tools/ngen-exe-native-image-generator.md).  
  
 [Interface ICorProfilerThreadEnum](icorprofilerthreadenum-interface.md)  
 Fornece métodos para iterar de forma sequencial por meio de uma coleção de threads no CLR.  
  
 [Interface IMethodMalloc](imethodmalloc-interface.md)  
 Fornece o método de [alocação](imethodmalloc-alloc-method.md) para alocar memória para um novo corpo de função da MSIL (Microsoft Intermediate Language).  
  
## <a name="related-sections"></a>Seções relacionadas  
 [Visão geral da criação de perfil](profiling-overview.md)  
  
 [Criando perfil de funções estáticas globais](profiling-global-static-functions.md)  
  
 [Criando perfil de enumerações](profiling-enumerations.md)  
  
 [Estruturas de criação de perfil](profiling-structures.md)
