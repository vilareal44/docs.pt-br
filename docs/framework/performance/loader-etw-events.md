---
title: Eventos ETW de carregador
description: Examine os eventos ETW do carregador, que incluem eventos de domínio de aplicativo, eventos de assembly do carregador CLR, eventos de módulo, eventos de módulo de domínio CLR e eventos de intervalo de módulo.
ms.date: 03/30/2017
helpviewer_keywords:
- loader events [.NET Framework]
- ETW, loader events (CLR)
ms.assetid: cb403cc6-56f8-4609-b467-cdfa09f07909
ms.openlocfilehash: 8220e8e773409be76bc7522d57551f1bddb90e5d
ms.sourcegitcommit: cf5a800a33de64d0aad6d115ffcc935f32375164
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/20/2020
ms.locfileid: "86474352"
---
# <a name="loader-etw-events"></a>Eventos ETW de carregador
 Esses eventos coletam informações relacionadas ao carregamento e descarregamento de domínios do aplicativo, assemblies e módulos.  
  
 Todos os eventos de carregador são gerados sob a palavra-chave `LoaderKeyword` (0x8). Os eventos `DCStart` e `DCEnd` são gerados sob `LoaderRundownKeyword` (0x8) com `StartRundown`/`EndRundown` habilitado. (Para obter mais informações, consulte [Palavras-chaves e níveis CLR ETW](clr-etw-keywords-and-levels.md).)  

## <a name="application-domain-events"></a>Eventos de domínio do aplicativo
 A tabela a seguir mostra a palavra-chave e o nível.  
  
|Palavra-chave para acionar o evento|Evento|Nível|  
|-----------------------------------|-----------|-----------|  
|`LoaderKeyword` (0x8)|`AppDomainLoad_V1` e `AppDomainUnLoad_V1`|Informativo (4)|  
|`LoaderRundownKeyword` (0x8) +<br /><br /> `StartRundownKeyword`|`AppDomainDCStart_V1`|Informativo (4)|  
|`LoaderRundownKeyword` (0x8) +<br /><br /> `EndRundownKeyword`|`AppDomainDCEnd_V1`|Informativo (4)|  
  
 A tabela a seguir mostra as informações do evento.  
  
|Evento|ID do evento|Descrição|  
|-----------|--------------|-----------------|  
|`AppDomainLoad_V1` (registrado para todos os domínios do aplicativo)|156|Gerado sempre que um domínio do aplicativo é criado durante o tempo de vida de um processo.|  
|`AppDomainUnLoad_V1`|157|Gerado sempre que um domínio do aplicativo é destruído durante o tempo de vida de um processo.|  
|`AppDomainDCStart_V1`|157|Enumera os domínios de aplicativo durante um encerramento inicial.|  
|`AppDomainDCEnd_V1`|158|Enumera os domínios de aplicativo durante um encerramento final.|  
  
 A tabela a seguir mostra os dados do evento.  
  
|Nome do campo|Tipo de dados|Descrição|  
|----------------|---------------|-----------------|  
|AppDomainID|win:UInt64|O identificador exclusivo de um domínio do aplicativo.|  
|AppDomainFlags|win:UInt32|0x1: domínio padrão.<br /><br /> 0x2: executável.<br /><br /> 0x4: domínio do aplicativo, 28-31 bits: política de compartilhamento desse domínio.<br /><br /> 0: um domínio compartilhado.|  
|AppDomainName|win:UnicodeString|Nome amigável de domínio do aplicativo. Pode ser alterado durante o tempo de vida do processo.|  
|AppDomainIndex|Win:UInt32|O índice desse domínio do aplicativo.|  
|ClrInstanceID|win:UInt16|ID exclusiva da instância do CLR ou do CoreCLR.|  

## <a name="clr-loader-assembly-events"></a>Eventos de assembly do carregador CLR  
 A tabela a seguir mostra a palavra-chave e o nível.  
  
|Palavra-chave para acionar o evento|Evento|Nível|  
|-----------------------------------|-----------|-----------|  
|`LoaderKeyword` (0x8)|`AssemblyLoad` e `AssemblyUnload`|Informativo (4)|  
|`LoaderRundownKeyword` (0x8) +<br /><br /> `StartRundownKeyword`|`AssemblyDCStart`|Informativo (4)|  
|`LoaderRundownKeyword` (0x8) +<br /><br /> `EndRundownKeyword`|`AssemblyDCEnd`|Informativo (4)|  
  
 A tabela a seguir mostra as informações do evento.  
  
|Evento|ID do evento|Descrição|  
|-----------|--------------|-----------------|  
|`AssemblyLoad_V1`|154|Gerado quando um assembly é carregado.|  
|`AssemblyUnload_V1`|155|Gerado quando um assembly é descarregado.|  
|`AssemblyDCStart_V1`|155|Enumera assemblies durante um encerramento inicial.|  
|`AssemblyDCEnd_V1`|156|Enumera assemblies durante um encerramento final.|  
  
 A tabela a seguir mostra os dados do evento.  
  
|Nome do campo|Tipo de dados|Descrição|  
|----------------|---------------|-----------------|  
|AssemblyID|win:UInt64|ID exclusiva para o assembly.|  
|AppDomainID|win:UInt64|ID do domínio desse assembly.|  
|BindingID|win:UInt64|ID que identifica exclusivamente a associação do assembly.|  
|AssemblyFlags|win:UInt32|0x1: assembly de domínio neutro.<br /><br /> 0x2: assembly dinâmico.<br /><br /> 0x4: o assembly tem uma imagem nativa.<br /><br /> 0x8: assembly de coleção.|  
|AssemblyName|win:UnicodeString|O nome totalmente qualificado do assembly.|  
|ClrInstanceID|win:UInt16|ID exclusiva da instância do CLR ou do CoreCLR.|

## <a name="module-events"></a>Eventos de módulo
 A tabela a seguir mostra a palavra-chave e o nível.  
  
|Palavra-chave para acionar o evento|Evento|Nível|  
|-----------------------------------|-----------|-----------|  
|`LoaderKeyword` (0x8)|`ModuleLoad_V2` e `ModuleUnload_V2`|Informativo (4)|  
|`LoaderRundownKeyword` (0x8) +<br /><br /> `StartRundownKeyword`|`ModuleDCStart_V2`|Informativo (4)|  
|`LoaderRundownKeyword` (0x8) +<br /><br /> `EndRundownKeyword`|`ModuleDCEnd_V2`|Informativo (4)|  
||||  
  
 A tabela a seguir mostra as informações do evento.  
  
|Evento|ID do evento|Descrição|  
|-----------|--------------|-----------------|  
|`ModuleLoad_V2`|152|Gerado quando um módulo é carregado durante o tempo de vida de um processo.|  
|`ModuleUnload_V2`|153|Gerado quando um módulo é descarregado durante o tempo de vida de um processo.|  
|`ModuleDCStart_V2`|153|Enumera módulos durante um encerramento inicial.|  
|`ModuleDCEnd_V2`|154|Enumera módulos durante um encerramento final.|  
  
 A tabela a seguir mostra os dados do evento.  
  
|Nome do campo|Tipo de dados|Descrição|  
|----------------|---------------|-----------------|  
|ModuleID|win:UInt64|ID exclusiva para o módulo.|  
|AssemblyID|win:UInt64|ID do assembly em que esse módulo reside.|  
|ModuleFlags|win:UInt32|0x1: módulo de domínio neutro.<br /><br /> 0x2: o módulo tem uma imagem nativa.<br /><br /> 0x4: módulo dinâmico.<br /><br /> 0x8: módulo de manifesto.|  
|Reserved1|win:UInt32|Campo reservado.|  
|ModuleILPath|win:UnicodeString|O caminho da imagem MSIL (Microsoft Intermediate Language) para o módulo ou o nome de módulo dinâmico se ele é um assembly dinâmico (terminado em nulo).|  
|ModuleNativePath|win:UnicodeString|Caminho da imagem nativa do módulo, se presente (terminado em nulo).|  
|ClrInstanceID|win:UInt16|ID exclusiva da instância do CLR ou do CoreCLR.|  
|ManagedPdbSignature|win:GUID|Assinatura GUID do banco de dados de programa (PDB) gerenciado que corresponde a esse módulo. (Consulte os comentários.)|  
|ManagedPdbAge|win:UInt32|Número de idade escrito para o PDB gerenciado que corresponde a esse módulo. (Consulte os comentários.)|  
|ManagedPdbBuildPath|win:UnicodeString|Caminho para o local em que o PDB gerenciado que corresponde a esse módulo foi criado. Em alguns casos, isso pode ser apenas um nome de arquivo. (Consulte os comentários.)|  
|NativePdbSignature|win:GUID|Assinatura GUID do gerador de imagem nativa (NGen) PDB que corresponde a esse módulo, se aplicável. (Consulte os comentários.)|  
|NativePdbAge|win:UInt32|Número de idade escrito para o PDB do NGen que corresponde a esse módulo, se aplicável. (Consulte os comentários.)|  
|NativePdbBuildPath|win:UnicodeString|Caminho para o local em que o PDB do NGen que corresponde a esse módulo foi criado, se aplicável. Em alguns casos, isso pode ser apenas um nome de arquivo. (Consulte os comentários.)|  
  
### <a name="remarks"></a>Comentários  
  
- Os campos que têm "Pdb" em seus nomes podem ser usados por ferramentas de criação de perfil para localizar PDBs que correspondem os módulos que foram carregados durante a sessão de criação de perfil. Os valores desses campos correspondem aos dados gravados nas seções IMAGE_DIRECTORY_ENTRY_DEBUG do módulo normalmente usado por depuradores para ajudar a localizar PDBs que correspondem aos módulos carregados.  
  
- Os nomes de campo que começam com "ManagedPdb" referem-se ao PDB gerenciado correspondente ao módulo MSIL que foi gerado pelo compilador gerenciado (por exemplo, o compilador de C# ou de Visual Basic). Esse PDB usa o formato de PDB gerenciado e descreve como os elementos do código-fonte gerenciado original, tais como arquivos, números de linha e nomes de símbolo, são mapeados para elementos MSIL que são compilados no módulo MSIL.  
  
- Os nomes de campo que começam com "NativePdb" se referem ao PDB NGen gerado chamando `NGEN createPDB`. Esse PDB usa o formato de PDB nativo e descreve como os elementos do código-fonte gerenciado original, tais como arquivos, números de linha e nomes de símbolo, são mapeados para elementos nativos que são compilados no módulo NGen.  

## <a name="clr-domain-module-events"></a>Eventos de módulo de domínio CLR
 A tabela a seguir mostra a palavra-chave e o nível.  
  
|Palavra-chave para acionar o evento|Evento|Nível|  
|-----------------------------------|-----------|-----------|  
|`LoaderKeyword` (0x8)|`DomainModuleLoad_V1`|Informativo (4)|  
|`LoaderRundownKeyword` (0x8) +<br /><br /> `StartRundownKeyword`|`DomainModuleDCStart_V1`|Informativo (4)|  
|`LoaderRundownKeyword` (0x8) +<br /><br /> `EndRundownKeyword`|`DomainModuleDCEnd_V1`|Informativo (4)|  
  
 A tabela a seguir mostra as informações do evento.  
  
|Evento|ID do evento|Descrição|  
|-----------|--------------|-----------------|  
|`DomainModuleLoad_V1`|151|Gerado quando um módulo é carregado para um domínio do aplicativo.|  
|`DomainModuleDCStart_V1`|151|Enumera os módulos carregados para um domínio do aplicativo durante um encerramento inicial e é registrado para todos os domínios de aplicativo.|  
|`DomainModuleDCEnd_V1`|152|Enumera os módulos carregados para um domínio do aplicativo durante um encerramento final e é registrado para todos os domínios de aplicativo.|  
  
 A tabela a seguir mostra os dados do evento.  
  
|Nome do campo|Tipo de dados|Descrição|  
|----------------|---------------|-----------------|  
|ModuleID|win:UInt64|Identifica o assembly ao qual este módulo pertence.|  
|AssemblyID|win:UInt64|ID do assembly em que esse módulo reside.|  
|AppDomainID|win:UInt64|ID do domínio do aplicativo no qual esse módulo é usado.|  
|ModuleFlags|win:UInt32|0x1: módulo de domínio neutro.<br /><br /> 0x2: o módulo tem uma imagem nativa.<br /><br /> 0x4: módulo dinâmico.<br /><br /> 0x8: módulo de manifesto.|  
|Reserved1|win:UInt32|Campo reservado.|  
|ModuleILPath|win:UnicodeString|O caminho da imagem MSIL para o módulo ou o nome de módulo dinâmico se ele é um assembly dinâmico (terminado em nulo).|  
|ModuleNativePath|win:UnicodeString|Caminho da imagem nativa do módulo, se presente (terminado em nulo).|  
|ClrInstanceID|win:UInt16|ID exclusiva da instância do CLR ou do CoreCLR.|  

## <a name="module-range-events"></a>Eventos de intervalo de módulo
 A tabela a seguir mostra a palavra-chave e o nível.  
  
|Palavra-chave para acionar o evento|Evento|Nível|  
|-----------------------------------|-----------|-----------|  
|`PerfTrackKeyWord`)|`ModuleRange`|Informativo (4)|  
|`PerfTrackKeyWord`|`ModuleRangeDCStart`|Informativo (4)|  
|`PerfTrackKeyWord`|`ModuleRangeDCEnd`|Informativo (4)|  
  
 A tabela a seguir mostra as informações do evento.  
  
|Evento|ID do evento|Descrição|  
|-----------|--------------|-----------------|  
|`ModuleRange`|158|Esse evento está presente se uma imagem carregada do gerador de imagem nativa (NGen) foi otimizada com IBC e contém informações sobre as seções quentes da imagem NGen.|  
|`ModuleRangeDCStart`|160|Um evento `ModuleRange` disparado no início de um encerramento.|  
|`ModuleRangeDCEnd`|161|Um evento `ModuleRange` disparado no fim de um encerramento.|  
  
 A tabela a seguir mostra os dados do evento.  
  
|Nome do campo|Tipo de dados|Descrição|  
|----------------|---------------|-----------------|  
|ClrInstanceID|win:UInt16|Identifica de modo exclusivo uma instância específica do CLR em um processo se várias instâncias do CLR estão carregadas.|  
|ModuleID|win:UInt64|Identifica o assembly ao qual este módulo pertence.|  
|RangeBegin|win:UInt32|O deslocamento do módulo que representa o início do intervalo para o tipo de intervalo especificado.|  
|RangeSize|win:UInt32|O tamanho do intervalo especificado em bytes.|  
|RangeType|win:UInt32|Um único valor, 0x4, para representar intervalos IBC frios. Esse campo poderá representar mais valores no futuro.|  
|RangeSize1|win:UInt32|0 indica dados inválidos.|  
|RangeBegin2|win:UnicodeString||  
  
### <a name="remarks"></a>Comentários  
 Se uma imagem NGen carregada em um processo do .NET Framework foi otimizada com IBC, o evento `ModuleRange` que contém os intervalos de acessados na imagem NGen é registrado junto com os respectivos `moduleID` e `ClrInstanceID`.  Se a imagem NGen não for otimizada com IBC, esse evento não será registrado. Para determinar o nome do módulo, esse evento deve ser agrupado com os eventos ETW da carga do módulo.  
  
 O tamanho de carga para esse evento é variável. O campo `Count` indica o número de deslocamentos de intervalo contidos no evento.  Esse evento deve ser agrupado com o evento `IStart` do Windows para determinar os intervalos reais. O evento de Carregamento de Imagem do Windows é registrado sempre que uma imagem é carregada e contém o endereço virtual da imagem carregada.  
  
 Eventos de intervalo do módulo são acionados em qualquer nível de ETW maior ou igual a 4 e são classificados como eventos informativos.  
  
## <a name="see-also"></a>Consulte também

- [Eventos ETW no CLR](clr-etw-events.md)
