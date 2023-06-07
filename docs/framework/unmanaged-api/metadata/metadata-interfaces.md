---
title: Interfaces de metadados
ms.date: 03/30/2017
helpviewer_keywords:
- unmanaged interfaces [.NET Framework], metadata
- metadata interfaces [.NET Framework]
- interfaces (.NET Framework metadata]
ms.assetid: f5cdac93-a28c-48ef-8a19-5773376e9e7c
ms.openlocfilehash: 4d947388afb8d7f8f935ae3b8e8aff81efaf2ee4
ms.sourcegitcommit: da21fc5a8cce1e028575acf31974681a1bc5aeed
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/08/2020
ms.locfileid: "84489580"
---
# <a name="metadata-interfaces"></a>Interfaces de metadados
Esta seção descreve as interfaces não gerenciadas que fornecem acesso aos metadados expostos pelo .NET Framework tipos, métodos, campos e assim por diante.  
  
## <a name="in-this-section"></a>Nesta seção  
 [Interface ICeeGen](iceegen-interface.md)  
 Fornece métodos para a compilação dinâmica de código.  
  
 [Interface IHostFilter](ihostfilter-interface.md)  
 Fornece um método para o host de tempo de execução marcar tokens de metadados para processamento.  
  
 [Interface IMapToken](imaptoken-interface.md)  
 Fornece recursos de mapeamento entre assinaturas de metadados importadas e emitidas.  
  
 [Interface IMetaDataAssemblyEmit](imetadataassemblyemit-interface.md)  
 Fornece métodos que dão suporte ao modelo de autodescrição usado pelo Common Language Runtime (CLR) para resolver e consumir recursos.  
  
 [Interface IMetaDataAssemblyImport](imetadataassemblyimport-interface.md)  
 Fornece métodos para acessar e examinar o conteúdo de um manifesto do assembly.  
  
 [Interface IMetaDataConverter](imetadataconverter-interface.md)  
 Fornece métodos para mapear bibliotecas de tipos para suas assinaturas de metadados e para converter de uma para a outra.  
  
 [Interface IMetaDataDispenser](imetadatadispenser-interface.md)  
 `IMetaDataDispenser` é obsoleto. Use `IMetaDataDispenserEx` em vez disso.  
  
 [Interface IMetaDataDispenserEx](imetadatadispenserex-interface.md)  
 Fornece métodos que mapeiam áreas de memória para criar ou modificar metadados.  
  
 [Interface IMetaDataEmit](imetadataemit-interface.md)  
 Fornece métodos para criar, modificar e armazenar metadados sobre o assembly no escopo definido no momento.  
  
 [Interface IMetaDataEmit2](imetadataemit2-interface.md)  
 Fornece métodos para definir e modificar as assinaturas de metadados de métodos e construtores com parâmetros do tipo <xref:System.Type?displayProperty=nameWithType> .  
  
 [Interface IMetaDataError](imetadataerror-interface.md)  
 Fornece um mecanismo de retorno de chamada para relatar erros durante a resolução da assinatura de metadados para um assembly.  
  
 [Interface IMetaDataFilter](imetadatafilter-interface.md)  
 Fornece métodos para marcação e filtragem de tokens de metadados para evitar ações repetidas que já foram realizadas.  
  
 [Interface IMetaDataImport](imetadataimport-interface.md)  
 Fornece métodos para importar e manipular tipos de outros assemblies.  
  
 [Interface IMetaDataImport2](imetadataimport2-interface.md)  
 Estende- `IMetaDataImport` se para fornecer a capacidade de trabalhar com tipos genéricos.  
  
 [Interface IMetaDataInfo](imetadatainfo-interface.md)  
 Fornece um método que obtém informações sobre o mapeamento de metadados de um arquivo em disco para a memória.  
  
 [Interface IMetaDataTables](imetadatatables-interface.md)  
 Fornece métodos para o armazenamento e a recuperação de informações de metadados em tabelas.  
  
 [Interface IMetaDataTables2](imetadatatables2-interface.md)  
 Estende- `IMetaDataTables` se para incluir métodos para trabalhar com fluxos de metadados.  
  
 [Interface IMetaDataValidate](imetadatavalidate-interface.md)  
 Fornece métodos a serem usados para a validação de assinaturas de metadados.  
  
## <a name="related-sections"></a>Seções relacionadas  
 [Funções estáticas globais de metadados](metadata-global-static-functions.md)  
  
 [Enumerações de metadados](metadata-enumerations.md)  
  
 [Estruturas de metadados](metadata-structures.md)  
  
 [Uniões de metadados](metadata-unions.md)
