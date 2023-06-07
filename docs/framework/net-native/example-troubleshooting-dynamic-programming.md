---
title: 'Exemplo: solução de problemas de programação dinâmica'
ms.date: 03/30/2017
ms.assetid: 42ed860a-a022-4682-8b7f-7c9870784671
ms.openlocfilehash: ff179854066d024a89cb5a84a19d0b9bb054d6e5
ms.sourcegitcommit: b16c00371ea06398859ecd157defc81301c9070f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/06/2020
ms.locfileid: "73128447"
---
# <a name="example-troubleshooting-dynamic-programming"></a>Exemplo: solução de problemas de programação dinâmica
> [!NOTE]
> Este tópico refere-se ao Developer Preview do .NET Nativo, que é um software em pré-lançamento. Você pode baixar a versão prévia do [site Microsoft Connect](https://go.microsoft.com/fwlink/?LinkId=394611) (registro obrigatório).  
  
 Nem todas as falhas de pesquisa de metadados em aplicativos desenvolvidos usando o .NET Native cadeia de ferramentas resultam em uma exceção.  Alguns podem manifestar-se de formas imprevisíveis em um aplicativo.  O exemplo a seguir mostra uma violação de acesso causada pela referência a um objeto nulo:  
  
```output
Access violation - code c0000005 (first chance)  
App!$3_App::Core::Util::NavigationArgs.Setup  
App!$3_App::Core::Util::NavigationArgs..ctor  
App!$0_App::Gibbon::Util::DesktopNavigationArgs..ctor  
App!$0_App::ViewModels::DesktopAppVM.NavigateToPage  
App!$3_App::Core::ViewModels::AppViewModel.NavigateToFirstPage  
App!$3_App::Core::ViewModels::AppViewModel::<HandleLaunch>d__a.MoveNext  
App!$43_System::Runtime::CompilerServices::AsyncMethodBuilderCore.CallMoveNext  
App!System::Action.InvokeClosedStaticThunk  
App!System::Action.Invoke  
App!$43_System::Threading::Tasks::AwaitTaskContinuation.InvokeAction  
App!$43_System::Threading::SendOrPostCallback.InvokeOpenStaticThunk  
[snip]  
```  
  
 Vamos tentar solucionar essa exceção usando a abordagem de três etapas descrita na seção “Resolver manualmente os metadados ausentes” da [Introdução](getting-started-with-net-native.md).  
  
## <a name="what-was-the-app-doing"></a>O que o aplicativo estava fazendo?  
 A primeira coisa a observar é o maquinário da palavra-chave do `async` na base da pilha.  Determinar o que o aplicativo fazia em método `async` pode ser problemático, porque a pilha perde o contexto da chamada de origem e executa o código `async` em um thread diferente. No entanto, podemos deduzir que o aplicativo está tentando carregar sua primeira página.  Na implementação de `NavigationArgs.Setup`, o seguinte código causou a violação de acesso:  
  
`AppViewModel.Current.LayoutVM.PageMap`  
  
 Nesta instância, a propriedade `LayoutVM` em `AppViewModel.Current` era **null**.  A ausência de metadados causou uma diferença de comportamento sutil e resultou em uma propriedade sendo não inicializada em vez do conjunto, como o aplicativo esperava.  Definir um ponto de interrupção no código onde `LayoutVM` deveria ter sido inicializado poderá esclarecer a situação.  No entanto, observe que o tipo do `LayoutVM` é `App.Core.ViewModels.Layout.LayoutApplicationVM`.  A única diretiva de metadados presente até agora no arquivo rd.xml é:  
  
```xml  
<Namespace Name="App.ViewModels" Browse="Required Public" Dynamic="Required Public" />  
```  
  
 O motivo mais provável da falha é que metadados estão faltando em `App.Core.ViewModels.Layout.LayoutApplicationVM` porque ela está em um namespace diferente.  
  
 Nesse caso, adicionar uma diretiva de runtime para `App.Core.ViewModels` resolveu o problema. A causa raiz foi uma chamada à API ao método <xref:System.Type.GetType%28System.String%29?displayProperty=nameWithType> que retornou **null** e o aplicativo ignorou silenciosamente o problema até que ocorreu uma falha.  
  
 Na programação dinâmica, uma prática recomendada ao usar APIs de reflexão em .NET Native é usar as <xref:System.Type.GetType%2A?displayProperty=nameWithType> sobrecargas que geram uma exceção em caso de falha.  
  
## <a name="is-this-an-isolated-case"></a>Esta é uma ocorrência isolada?  
 Outros problemas também podem surgir ao usar `App.Core.ViewModels`.  Você deve decidir se vale a pena identificar e corrigir cada exceção de metadados ausentes, ou economizar tempo e adicionar diretivas para uma classe maior de tipos.  Aqui, adicionar metadados `dynamic` a `App.Core.ViewModels` pode ser a melhor abordagem se o aumento de tamanho resultante do binário de saída não for um problema.  
  
## <a name="could-the-code-be-rewritten"></a>O código pode ser reescrito?  
 Se o aplicativo tivesse usado `typeof(LayoutApplicationVM)` em vez de `Type.GetType("LayoutApplicationVM")`, a cadeia de ferramentas poderia ter preservado os metadados `browse`.  No entanto, ele ainda não criaria os metadados de `invoke`, o que levaria a uma exceção [MissingMetadataException](missingmetadataexception-class-net-native.md) ao instanciar o tipo. Para evitar a exceção, ainda seria necessário adicionar uma diretiva de runtime para o namespace ou o tipo que especifica a política `dynamic`. Para obter informações sobre as diretivas de runtime, consulte a [Referência do arquivo de configuração das diretivas de runtime (rd.xml)](runtime-directives-rd-xml-configuration-file-reference.md).  
  
## <a name="see-also"></a>Confira também

- [Introdução](getting-started-with-net-native.md)
- [Exemplo: lidar com exceções ao associar dados](example-handling-exceptions-when-binding-data.md)
