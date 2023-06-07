---
title: Ferramenta de Análise de Composição (Mefx)
description: Leia sobre a ferramenta de análise de composição (Mefx), que analisa os arquivos DLL e EXE que contêm partes Managed Extensibility Frameworks (MEF) no .NET.
ms.date: 03/30/2017
helpviewer_keywords:
- Composition Analysis Tool [MEF]
- MEF, Composition Analysis Tool
- Mefx [MEF], Composition Analysis Tool
ms.assetid: c48a7f93-83bb-4a06-aea0-d8e7bd1502ad
ms.openlocfilehash: abb1459afc5aeb2d39ee553c62fe382bb7af58d5
ms.sourcegitcommit: 97ce5363efa88179dd76e09de0103a500ca9b659
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/13/2020
ms.locfileid: "86281271"
---
# <a name="composition-analysis-tool-mefx"></a>Ferramenta de Análise de Composição (Mefx)
A Ferramenta de Análise de Composição (Mefx) é um aplicativo de linha de comando que analisa arquivos de biblioteca (.dll) e de aplicativo (.exe) que contêm partes do MEF (Managed Extensibility Framework). A principal finalidade da Mefx é fornecer aos desenvolvedores uma maneira de diagnosticar falhas de composição em seus aplicativos MEF sem a necessidade de adicionar um código de rastreamento inconveniente ao próprio aplicativo. Ele também pode ser útil para ajudar a entender as partes de uma biblioteca fornecida por terceiros. Este tópico descreve como usar a Mefx e fornece uma referência para sua sintaxe.  
  
<a name="getting_mefx"></a>
## <a name="getting-mefx"></a>Obtendo a Mefx  
 A Mefx está disponível no GitHub em [Managed Extensibility Framework](https://github.com/MicrosoftArchive/mef/releases/tag/4.0). Basta baixar e descompactar a ferramenta.  
  
<a name="basic_syntax"></a>
## <a name="basic-syntax"></a>Sintaxe básica  
 A Mefx é invocada na linha de comando no seguinte formato:  
  
```console
mefx [files and directories] [action] [options]  
```  
  
 O primeiro conjunto de argumentos especifica os arquivos e os diretórios dos quais as partes devem ser carregadas para análise. Especifique um arquivo com a opção `/file:` e um diretório com a opção `/directory:`. Você pode especificar vários arquivos ou diretórios, conforme é mostrado no exemplo a seguir:  
  
```console  
mefx /file:MyAddIn.dll /directory:Program\AddIns [action...]  
```  
  
> [!NOTE]
> Cada arquivo .dll ou .exe deve ser carregado somente uma vez. Se algum arquivo for carregado várias vezes, a ferramenta poderá retornar informações incorretas.  
  
 Depois da lista de arquivos e diretórios, você deverá especificar um comando e as opções para esse comando.  
  
<a name="listing_available_parts"></a>
## <a name="listing-available-parts"></a>Listando as partes disponíveis  
 Use a ação `/parts` para listar todas as partes declaradas nos arquivos carregados. O resultado é uma lista simple de nomes de parte.  
  
```console
mefx /file:MyAddIn.dll /parts  
MyAddIn.AddIn  
MyAddIn.MemberPart  
```  
  
 Para obter mais informações sobre as partes, use a opção `/verbose`. Isso produzirá mais informações para todas as partes disponíveis. Para obter mais informações sobre uma única parte, use a ação `/type` em vez de `/parts`.  
  
```console  
mefx /file:MyAddIn.dll /type:MyAddIn.AddIn /verbose  
[Part] MyAddIn.MemberPart from: AssemblyCatalog (Assembly=" MyAddIn, Version=1.0.0.0, Culture=neutral, PublicKeyToken=null")  
  [Export] MyAddIn.MemberPart (ContractName=" MyAddIn.MemberPart")  
```  
  
<a name="listing_imports_and_exports"></a>
## <a name="listing-imports-and-exports"></a>Listando as importações e exportações  
 As ações `/imports` e `/exports` listarão todas as partes importadas e todas as partes exportadas, respectivamente. Você também pode listar as partes que importam ou exportam um tipo específico usando as ações `/importers` ou `/exporters`.  
  
```console  
mefx /file:MyAddIn.dll /importers:MyAddin.MemberPart  
MyAddin.AddIn  
```  
  
 Você também pode aplicar a opção `/verbose` a essas ações.  
  
<a name="finding_rejected_parts"></a>
## <a name="finding-rejected-parts"></a>Localizando as partes rejeitadas  
 Depois de carregar as partes disponíveis, a Mefx usa o mecanismo de composição do MEF para compô-las. As partes que não podem ser compostas com êxito são chamadas de *rejeitadas*. Para listar todas as partes rejeitadas, use a ação `/rejected`.  
  
 Você pode usar a opção `/verbose` com a ação `/rejected` para imprimir informações detalhadas sobre as partes rejeitadas. No exemplo a seguir, a DLL `ClassLibrary1` contém a parte `AddIn`, que importa as partes `MemberPart` e `ChainOne`. A `ChainOne` importa a `ChainTwo`, mas a `ChainTwo` não existe. Isso significa que a `ChainOne` foi rejeitada, o que faz com que a `AddIn` seja rejeitada.  
  
```console  
mefx /file:ClassLibrary1.dll /rejected /verbose  
```  
  
 O exemplo a seguir mostra a saída completa do comando anterior:  
  
```output
[Part] ClassLibrary1.AddIn from: AssemblyCatalog (Assembly="ClassLibrary1, Version=1.0.0.0, Culture=neutral, PublicKeyToken=null")  
  [Export] ClassLibrary1.AddIn (ContractName="ClassLibrary1.AddIn")  
  [Import] ClassLibrary1.AddIn.memberPart (ContractName="ClassLibrary1.MemberPart")  
    [SatisfiedBy] ClassLibrary1.MemberPart (ContractName="ClassLibrary1.MemberPart") from: ClassLibrary1.MemberPart from: AssemblyCatalog (Assembly="ClassLibrar  
y1, Version=1.0.0.0, Culture=neutral, PublicKeyToken=null")  
  [Import] ClassLibrary1.AddIn.chain (ContractName="ClassLibrary1.ChainOne")  
    [Exception] System.ComponentModel.Composition.ImportCardinalityMismatchException: No valid exports were found that match the constraint '((exportDefinition.ContractName == "ClassLibrary1.ChainOne") AndAlso (exportDefinition.Metadata.ContainsKey("ExportTypeIdentity") AndAlso "ClassLibrary1.ChainOne".Equals(exportDefinition.Metadata.get_Item("ExportTypeIdentity"))))', invalid exports may have been rejected.  
   at System.ComponentModel.Composition.Hosting.ExportProvider.GetExports(ImportDefinition definition, AtomicComposition atomicComposition)  
   at Microsoft.ComponentModel.Composition.Diagnostics.CompositionInfo.AnalyzeImportDefinition(ExportProvider host, IEnumerable`1 availableParts, ImportDefinition id)  
    [Unsuitable] ClassLibrary1.ChainOne (ContractName="ClassLibrary1.ChainOne")  
from: ClassLibrary1.ChainOne from: AssemblyCatalog (Assembly="ClassLibrary1, Version=1.0.0.0, Culture=neutral, PublicKeyToken=null")  
      [Because] PartDefinitionIsRejected, The part providing the export is rejected because of other issues.  
  
[Part] ClassLibrary1.ChainOne from: AssemblyCatalog (Assembly="ClassLibrary1, Version=1.0.0.0, Culture=neutral, PublicKeyToken=null")  
  [Primary Rejection]  
  [Export] ClassLibrary1.ChainOne (ContractName="ClassLibrary1.ChainOne")  
  [Import] ClassLibrary1.ChainOne.chain (ContractName="ClassLibrary1.ChainTwo")  
    [Exception] System.ComponentModel.Composition.ImportCardinalityMismatchException: No valid exports were found that match the constraint '((exportDefinition.ContractName == "ClassLibrary1.ChainTwo") AndAlso (exportDefinition.Metadata.ContainsKey("ExportTypeIdentity") AndAlso "ClassLibrary1.ChainTwo".Equals(exportDefinition.Metadata.get_Item("ExportTypeIdentity"))))', invalid exports may have been rejected.  
   at System.ComponentModel.Composition.Hosting.ExportProvider.GetExports(ImportDefinition definition, AtomicComposition atomicComposition)  
   at Microsoft.ComponentModel.Composition.Diagnostics.CompositionInfo.AnalyzeImportDefinition(ExportProvider host, IEnumerable`1 availableParts, ImportDefinition id)  
```  
  
 As informações interessantes estão contidas nos resultados de `[Exception]` e `[Unsuitable]`. O resultado de `[Exception]` fornece informações sobre por que uma parte foi rejeitada. O resultado `[Unsuitable]` indica por que uma parte com outro tipo de correspondência não pôde ser usada para preencher uma importação. Nesse caso, porque essa parte foi rejeitada devido à ausência de importações.  
  
<a name="analyzing_primary_causes"></a>
## <a name="analyzing-primary-causes"></a>Analisando a causa principal  
 Se várias partes estiverem vinculadas em uma cadeia longa de dependência, um problema envolvendo uma parte próxima à parte inferior poderá fazer com que a cadeia inteira seja rejeitada. O diagnóstico desses problemas pode ser difícil, pois a causa raiz da falha nem sempre é óbvia. Para ajudar a resolver o problema, você pode usar a ação `/causes`, que tenta localizar a causa raiz de qualquer rejeição em cascata.  
  
 O uso da ação `/causes` no exemplo anterior listaria apenas as informações para `ChainOne`, cuja importação não preenchida é a causa raiz da rejeição de `AddIn`. A ação `/causes` pode ser usada nas opções normal e `/verbose`.  
  
> [!NOTE]
> Na maioria dos casos, a Mefx poderá diagnosticar a causa raiz de uma falha em cascata. No entanto, nos casos em que as partes são adicionadas de forma programática a um contêiner, nos que envolvem contêineres hierárquicos ou nos que envolvem implementações de `ExportProvider` personalizadas, a Mefx não pode diagnosticar a causa. Em geral, esses casos descritos devem ser evitados sempre que possível, pois as falhas geralmente são difíceis de diagnosticar.  
  
<a name="white_lists"></a>
## <a name="white-lists"></a>Listas de permissões  
 A opção `/whitelist` permite que você especifique um arquivo de texto que lista as partes que devem ser rejeitadas. Assim, as rejeições inesperadas serão sinalizadas. Isso pode ser útil ao analisar uma biblioteca incompleta ou uma sub-biblioteca com algumas dependências faltando. A opção `/whitelist` pode ser aplicada às ações `/rejected` ou `/causes`.  
  
 Considere um arquivo chamado test.txt que contenha o texto "ClassLibrary1.ChainOne". Se você executar a ação `/rejected` com a opção `/whitelist` no exemplo anterior, ela produzirá a saída a seguir:  
  
```console
mefx /file:ClassLibrary1.dll /rejected /whitelist:test.txt  
[Unexpected] ClassLibrary1.AddIn  
ClassLibrary1.ChainOne  
```
