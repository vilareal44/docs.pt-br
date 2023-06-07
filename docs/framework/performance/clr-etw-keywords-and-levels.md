---
title: Palavras-chave e níveis ETW no CLR
description: Examine as palavras-chave e os níveis do ETW (rastreamento de eventos) Common Language Runtime (CLR). As palavras-chave do ETW do CLR de evento habilitam a filtragem de eventos por categoria.
ms.date: 03/30/2017
helpviewer_keywords:
- CLR ETW keywords
- CLR ETW levels
- ETW, CLR keywords
- ETW, CLR levels
ms.assetid: fdf5856d-516b-4042-849d-911c4518a6cb
ms.openlocfilehash: dfbe047640a3a640cf37adeea6fa3656cfd9ec6d
ms.sourcegitcommit: 0fa2b7b658bf137e813a7f4d09589d64c148ebf5
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/14/2020
ms.locfileid: "86309671"
---
# <a name="clr-etw-keywords-and-levels"></a>Palavras-chave e níveis ETW no CLR
 Os eventos ETW (rastreamento de eventos para Windows) pode ser filtrado por categoria e nível. As [palavras-chave CLR ETW](#clr-etw-keywords) do evento permitem a filtragem de eventos por categoria; elas são usadas em combinações para os provedores de runtime e de encerramento. Os [níveis de evento](#etw-event-levels) são identificados por sinalizadores.  
  
## <a name="clr-etw-keywords"></a>Palavras-chave CLR ETW  
 As palavras-chave são sinalizadores que podem ser combinados para gerar valores. Na prática, você usa os valores hexadecimais das palavras-chave, em vez dos nomes da palavra-chave, ao chamar os utilitários de linha de comando.  
  
 Essas palavras-chave são descritas nas seguintes tabelas:  
  
- [Palavras-chave de tempo de execução do ETW do CLR](#runtime)  
  
- [Palavras-chave de encerramento CLR ETW](#rundown)  
  
- [Combinações de palavras-chave para a resolução de símbolo do provedor de runtime](#runtime_combo)  
  
- [Combinações de palavras-chave para a resolução de símbolo do provedor de encerramento](#rundown_combo)  
  
<a name="runtime"></a>
### <a name="clr-etw-runtime-keywords"></a>Palavras-Chave de Runtime CLR ETW  
 A tabela a seguir lista as palavras-chave de runtime CLR ETW, seus valores e sua finalidade de uso.  
  
|Nome da palavra-chave de runtime|Valor|Finalidade|  
|--------------------------|-----------|-------------|  
|`GCKeyword`|0x00000001|Habilita a coleta de [eventos de coleta de lixo](garbage-collection-etw-events.md).|  
|`LoaderKeyword`|0x00000008|Habilita a coleta de [eventos de carregador](loader-etw-events.md).|  
|`JITKeyword`|0x00000010|Habilita a coleta de [eventos JIT (Just-In-Time)](jit-tracing-etw-events.md).|  
|`NGenKeyword`|0x00000020|Habilita a coleta de eventos de métodos de imagem nativa (métodos processados pelo Gerador de Imagem Nativa, Ngen.exe); usado com `StartEnumerationKeyword` e `EndEnumerationKeyword`. Essa palavra-chave tem uma alta sobrecarga. Ela gera eventos para cada método dentro de cada módulo do NGen carregado. Sempre que possível, em vez de usar essa palavra-chave, recomendamos o uso dos PDBs (bancos de dados do programa) gerados pelas ferramentas de criação de perfil para recuperar informações sobre os métodos dos módulos do NGen. Consulte também `OverrideAndSuppressNGenEventsKeyword` mais adiante nesta tabela.|  
|`StartEnumerationKeyword`|0x00000040|Habilita a enumeração de todos os métodos no runtime; usado em conjunto com `NGenKeyword`.|  
|`EndEnumerationKeyword`|0x00000080|Habilita a enumeração de todos os métodos destruídos no runtime; usado em conjunto com `JITKeyword` e `NGenKeyword`.|  
|`SecurityKeyword`|0x00000400|Habilita a coleta de [eventos de segurança](security-etw-events.md).|  
|`AppDomainResourceManagementKeyword`|0x00000800|Habilita a coleção de eventos de monitoramento de recursos no nível de um domínio do aplicativo.|  
|`JITTracingKeyword`|0x00001000|Habilita a coleta de [eventos de rastreamento JIT](jit-tracing-etw-events.md).|  
|`InteropKeyword`|0x00002000|Habilita a coleta de [eventos de interoperabilidade](interop-etw-events.md).|  
|`ContentionKeyword`|0x00004000|Habilita a coleta de [eventos de contenção](contention-etw-events.md).|  
|`ExceptionKeyword`|0x00008000|Habilita a coleta de [eventos de exceção](exception-thrown-v1-etw-event.md).|  
|`ThreadingKeyword`|0x00010000|Habilita a coleção de [eventos de pool de threads](thread-pool-etw-events.md).|  
|`OverrideAndSuppressNGenEventsKeyword`|0x00040000|(Disponível no .NET Framework 4,5 e posterior.) Suprime a palavra-chave de alta sobrecarga `NGenKeyword` e impede a geração de eventos para métodos que estão dentro de módulos NGen. A partir do .NET Framework 4,5, as ferramentas de criação de perfil devem usar `OverrideAndSuppressNGenEventsKeyword` e `NGenKeyword` em conjunto para suprimir a geração de eventos para métodos em módulos NGen. Isso permite que a ferramenta de criação de perfil use os PDBs mais eficientes do NGen para obter informações sobre os métodos nos módulos do NGen. O CLR no .NET Framework 4 e versões anteriores não dá suporte à criação de PDBs do NGen. Nessas versões anteriores, o CLR não reconhecerá `OverrideAndSuppressNGenEventsKeyword` e processará `NGenKeyword` para gerar eventos para os métodos nos módulos do NGen.|  
|`PerfTrackKeyWord`|0x2000000|Habilita a coleta dos eventos `ModuleLoad` e `ModuleRange`.|  
|`StackKeyword`|0x40000000|Habilita a coleta de [eventos de rastreamento de pilha](stack-etw-event.md) do CLR.|  
  
<a name="rundown"></a>
### <a name="clr-etw-rundown-keywords"></a>Palavras-Chave de Encerramento CLR ETW  
 A tabela a seguir lista as palavras-chave de encerramento CLR ETW, seus valores e sua finalidade de uso.  
  
|Nome da palavra-chave de encerramento|Valor|Finalidade|  
|--------------------------|-----------|-------------|  
|`LoaderRundownKeyword`|0x00000008|Habilita a coleta de eventos de carregador quando usado com `StartRundownKeyword` e `EndRundownKeyword`.|  
|`JitRundownKeyword`|0x00000010|Habilita a coleta dos eventos `DCStart` e `DCEnd` do método para métodos compilados pelo JIT quando usado com `StartRundownKeyword` e `EndRundownKeyword`.|  
|`NGenRundownKeyword`|0x00000020|Habilita a coleta dos eventos `DCStart` e `DCEnd` do método para métodos de imagem nativa do NGen quando usado com `StartRundownKeyword` e `EndRundownKeyword`. Essa palavra-chave tem uma alta sobrecarga. Ela gera eventos para cada método dentro de cada módulo do NGen carregado. Sempre que possível, em vez de usar essa palavra-chave, recomendamos o uso dos PDBs (bancos de dados do programa) gerados pelas ferramentas de criação de perfil para recuperar informações sobre os métodos dos módulos do NGen. Consulte também `OverrideAndSuppressNGenEventsRundownKeyword` mais adiante nesta tabela.|  
|`StartRundownKeyword`|0x00000040|Habilita a enumeração de estado do sistema durante um encerramento inicial.|  
|`EndRundownKeyword`|0x00000100|Habilita a enumeração de estado do sistema durante um encerramento final.|  
|`AppDomainResourceManagementRundownKeyword`|0x00000800|Habilita a coleta de eventos de monitoramento de recursos no nível de um <xref:System.AppDomain> quando usado com `StartRundownKeyword` ou `EndRundownKeyword`.|  
|`ThreadingKeyword`|0x00010000|Habilita a coleta de eventos de pool de threads.|  
|`OverrideAndSuppressNGenEventsRundownKeyword`|0x00040000|(Disponível no .NET Framework 4,5 e posterior.) Suprime a palavra-chave de alta sobrecarga `NGenRundownKeyword` e impede a geração de eventos para métodos que estão dentro de módulos NGen. A partir do .NET Framework 4,5, as ferramentas de criação de perfil devem usar `OverrideAndSuppressNGenEventsRundownKeyword` e `NGenRundownKeyword` em conjunto para suprimir a geração de eventos para métodos em módulos NGen. Isso permite que a ferramenta de criação de perfil use os PDBs mais eficientes do NGen para obter informações sobre os métodos nos módulos do NGen. O CLR no .NET Framework 4 e versões anteriores não dá suporte à criação de PDBs do NGen. Nessas versões anteriores, o CLR não reconhecerá `OverrideAndSuppressNGenEventsRundownKeyword` e processará `NGenRundownKeyword` para gerar eventos para os métodos nos módulos do NGen.|  
|`PerfTrackKeyWord`|0x2000000|Habilita a coleta dos eventos `ModuleDCStart`, `ModuleDCEnd`, `ModuleRangeDCStart` e `ModuleRangeDCEnd`.|
  
<a name="runtime_combo"></a>
### <a name="keyword-combinations-for-symbol-resolution-for-the-runtime-provider"></a>Combinações de palavras-chave para a resolução de símbolo do provedor de runtime  
  
|Palavras-chave e sinalizadores|Domínio do aplicativo, assembly, eventos de carregamento/descarregamento do módulo|Eventos de carregamento/descarregamento do método (exceto eventos dinâmicos)|Eventos de carregamento/destruição de método dinâmico|  
|------------------------|--------------------------------------------------------------|----------------------------------------------------------|-----------------------------------------|  
|`LoaderKeyword`|Eventos de carregamento e descarregamento.|Nenhum.|Nenhum.|  
|`JITKeyword`<br /><br /> (+ `StartEnumerationKeyword` não adiciona nada)|Nenhum.|Eventos de carregamento.|Eventos de carregamento e descarregamento.|  
|`JITKeyword` +<br /><br /> `EndEnumerationKeyword`|Nenhum.|Eventos de carregamento e descarregamento.|Eventos de carregamento e descarregamento.|  
|`NGenKeyword`|Nenhum.|Nenhum.|Não aplicável.|  
|`NGenKeyword` +<br /><br /> `StartEnumerationKeyword`|Nenhum.|Eventos de carregamento.|Não aplicável.|  
|`NGenKeyword` +<br /><br /> `EndEnumerationKeyword`|Nenhum.|Eventos de descarregamento.|Não aplicável.|  
  
<a name="rundown_combo"></a>
### <a name="keyword-combinations-for-symbol-resolution-for-the-rundown-provider"></a>Combinações de palavras-chave para a resolução de símbolo do provedor de encerramento  
  
|Palavras-chave e sinalizadores|Domínio do aplicativo, assembly, eventos de DCStart/DCEnd do módulo|Eventos de DCStart/DCEnd do módulo (incluindo eventos de método dinâmico)|  
|------------------------|----------------------------------------------------------------|----------------------------------------------------------------------|  
|`LoaderRundownKeyword` +<br /><br /> `StartRundownKeyword`|Eventos `DCStart`.|Nenhum.|  
|`LoaderRundownKeyword` +<br /><br /> `EndRundownKeyword`|Eventos `DCEnd`.|Nenhum.|  
|`JITKeyword` +<br /><br /> `StartRundownKeyword`|Nenhum.|Eventos `DCStart`.|  
|`JITKeyword` +<br /><br /> `EndRundownKeyword`|Nenhum.|Eventos `DCEnd`.|  
|`NGenKeyword` +<br /><br /> `StartRundownKeyword`|Nenhum.|Eventos `DCStart`.|  
|`NGenKeyword` +<br /><br /> `EndRundownKeyword`|Nenhum.|Eventos `DCEnd`.|  

## <a name="etw-event-levels"></a>Níveis de eventos ETW  
 Os eventos ETW também podem ser filtrados por nível. Se o nível for definido como 0x5, eventos de todos os níveis serão acionados, incluindo 0x5 e abaixo (que são eventos que pertencem às categorias habilitadas por meio de palavras-chave). Se o nível for definido como 0x2, somente os eventos que pertencem ao nível 0x2 e abaixo serão acionados.  
  
 Os níveis têm os seguintes significados:  
  
 0x5 – Detalhado  
  
 0x4 – Informativo  
  
 0x3 – Aviso  
  
 0x2 – Erro  
  
 0x1 – Crítico  
  
 0x0 – LogAlways  
  
## <a name="see-also"></a>Confira também

- [Provedores ETW no CLR](clr-etw-providers.md)
- [Eventos ETW no CLR](clr-etw-events.md)
- [Eventos ETW no Common Language Runtime](etw-events-in-the-common-language-runtime.md)
