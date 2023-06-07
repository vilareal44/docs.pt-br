---
title: Armazenando em cache em clientes de automação de interface do usuário
description: Obtenha detalhes sobre o cache em clientes de automação da interface do usuário no .NET. O Caching é definido como a busca prévia de dados.
ms.date: 03/30/2017
helpviewer_keywords:
- UI Automation caching in clients
- caching, UI Automation clients
ms.assetid: 94c15031-4975-43cc-bcd5-c9439ed21c9c
ms.openlocfilehash: 4fbb4acabebea54015b11cefdf8a37c7e2dc93f5
ms.sourcegitcommit: 87cfeb69226fef01acb17c56c86f978f4f4a13db
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/24/2020
ms.locfileid: "87168258"
---
# <a name="caching-in-ui-automation-clients"></a>Armazenando em cache em clientes de automação de interface do usuário
> [!NOTE]
> Esta documentação destina-se a desenvolvedores do .NET Framework que querem usar as classes da [!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)] gerenciadas definidas no namespace <xref:System.Windows.Automation>. Para obter as informações mais recentes sobre a [!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)], consulte [Windows Automation API: UI Automation](/windows/win32/winauto/entry-uiauto-win32) (API de Automação do Windows: Automação da Interface do Usuário).  
  
 Este tópico apresenta o cache de [!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)] Propriedades e padrões de controle.  
  
 No [!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)] , o Caching significa Pre-busca de dados. Os dados podem então ser acessados sem mais comunicação entre processos. O cache é normalmente usado por aplicativos cliente de automação da interface do usuário para recuperar propriedades e padrões de controle em massa. As informações são recuperadas do cache conforme necessário. O aplicativo atualiza o cache periodicamente, geralmente em resposta a eventos, indicando que algo no [!INCLUDE[TLA#tla_ui](../../../includes/tlasharptla-ui-md.md)] foi alterado.  
  
 Os benefícios do cache são mais perceptíveis com controles de Windows Presentation Foundation (WPF) e controles personalizados que têm provedores de automação de interface do usuário do lado do servidor. Há menos benefícios ao acessar provedores do lado do cliente, como os provedores padrão para controles do Win32.  
  
 O cache ocorre quando o aplicativo ativa um <xref:System.Windows.Automation.CacheRequest> e, em seguida, usa qualquer método ou propriedade que retorna um <xref:System.Windows.Automation.AutomationElement> ; por exemplo, <xref:System.Windows.Automation.AutomationElement.FindFirst%2A> , <xref:System.Windows.Automation.AutomationElement.FindAll%2A> . Os métodos da <xref:System.Windows.Automation.TreeWalker> classe são uma exceção; o Caching só será feito se um <xref:System.Windows.Automation.CacheRequest> for especificado como um parâmetro (por exemplo, <xref:System.Windows.Automation.TreeWalker.GetFirstChild%28System.Windows.Automation.AutomationElement%2CSystem.Windows.Automation.CacheRequest%29?displayProperty=nameWithType> .  
  
 O Caching também ocorre quando você assina um evento enquanto um <xref:System.Windows.Automation.CacheRequest> está ativo. O <xref:System.Windows.Automation.AutomationElement> passado para o manipulador de eventos como a origem de um evento contém as propriedades e os padrões armazenados em cache especificados pelo original <xref:System.Windows.Automation.CacheRequest> . Qualquer alteração feita no <xref:System.Windows.Automation.CacheRequest> depois que você assinar o evento não terá nenhum efeito.  
  
 As [!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)] Propriedades e os padrões de controle de um elemento podem ser armazenados em cache.  
  
<a name="Options_for_Caching"></a>
## <a name="options-for-caching"></a>Opções de cache  
 O <xref:System.Windows.Automation.CacheRequest> especifica as opções a seguir para o Caching.  
  
<a name="Properties_to_Cache"></a>
### <a name="properties-to-cache"></a>Propriedades para armazenar em cache  
 Você pode especificar propriedades para cache chamando <xref:System.Windows.Automation.CacheRequest.Add%28System.Windows.Automation.AutomationProperty%29> para cada propriedade antes de ativar a solicitação.  
  
<a name="Control_Patterns_to_Cache"></a>
### <a name="control-patterns-to-cache"></a>Padrões de controle para cache  
 Você pode especificar padrões de controle para cache chamando <xref:System.Windows.Automation.CacheRequest.Add%28System.Windows.Automation.AutomationPattern%29> para cada padrão antes de ativar a solicitação. Quando um padrão é armazenado em cache, suas propriedades não são armazenadas em cache automaticamente; Você deve especificar as propriedades que deseja armazenar em cache usando <xref:System.Windows.Automation.CacheRequest.Add%2A?displayProperty=nameWithType> .  
  
<a name="Scope_of_the_Caching"></a>
### <a name="scope-and-filtering-of-caching"></a>Escopo e filtragem de cache  
 Você pode especificar os elementos cujas propriedades e padrões você deseja armazenar em cache definindo a <xref:System.Windows.Automation.CacheRequest.TreeScope%2A?displayProperty=nameWithType> propriedade antes de ativar a solicitação. O escopo é relativo aos elementos que são recuperados enquanto a solicitação está ativa. Por exemplo, se você definir apenas <xref:System.Windows.Automation.TreeScope.Children> e, em seguida, recuperar um <xref:System.Windows.Automation.AutomationElement> , as propriedades e os padrões de filhos desse elemento serão armazenados em cache, mas não os do próprio elemento. Para garantir que o cache seja feito para o próprio elemento recuperado, você deve incluir <xref:System.Windows.Automation.TreeScope.Element> na <xref:System.Windows.Automation.CacheRequest.TreeScope%2A> propriedade. Não é possível definir o escopo como <xref:System.Windows.Automation.TreeScope.Parent> ou <xref:System.Windows.Automation.TreeScope.Ancestors> . No entanto, um elemento pai pode ser armazenado em cache quando um elemento filho é armazenado em cache; consulte Recuperando filhos e pais em cache neste tópico.  
  
 A extensão do cache também é afetada pela <xref:System.Windows.Automation.CacheRequest.TreeFilter%2A?displayProperty=nameWithType> propriedade. Por padrão, o cache é executado somente para elementos que aparecem na exibição de controle da [!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)] árvore. No entanto, você pode alterar essa propriedade para aplicar o cache a todos os elementos ou somente a elementos que aparecem na exibição de conteúdo.  
  
<a name="Strength_of_the_Element_References"></a>
### <a name="strength-of-the-element-references"></a>Força das referências de elemento  
 Quando você recupera um <xref:System.Windows.Automation.AutomationElement> , por padrão, você tem acesso a todas as propriedades e padrões desse elemento, incluindo aqueles que não foram armazenados em cache. No entanto, para maior eficiência, você pode especificar que a referência ao elemento se refere somente a dados armazenados em cache, definindo a <xref:System.Windows.Automation.CacheRequest.AutomationElementMode%2A> propriedade de <xref:System.Windows.Automation.CacheRequest> como <xref:System.Windows.Automation.AutomationElementMode.None> . Nesse caso, você não tem acesso a nenhuma propriedade não armazenada em cache e padrões de elementos recuperados. Isso significa que você não pode acessar as propriedades por meio da propriedade ou de <xref:System.Windows.Automation.AutomationElement.GetCurrentPropertyValue%2A> `Current` <xref:System.Windows.Automation.AutomationElement> qualquer padrão de controle; nem pode recuperar um padrão usando <xref:System.Windows.Automation.AutomationElement.GetCurrentPattern%2A> ou <xref:System.Windows.Automation.AutomationElement.TryGetCurrentPattern%2A> . Em padrões armazenados em cache, você pode chamar métodos que recuperam Propriedades de matriz, como <xref:System.Windows.Automation.SelectionPattern.SelectionPatternInformation.GetSelection%2A?displayProperty=nameWithType> , mas não qualquer, que executam ações no controle, como <xref:System.Windows.Automation.InvokePattern.Invoke%2A?displayProperty=nameWithType> .  
  
 Um exemplo de um aplicativo que pode não precisar de referências completas a objetos é um leitor de tela, que buscaria as <xref:System.Windows.Automation.AutomationElement.AutomationElementInformation.Name%2A> <xref:System.Windows.Automation.AutomationElement.AutomationElementInformation.ControlType%2A> Propriedades e dos elementos em uma janela, mas não precisaria dos <xref:System.Windows.Automation.AutomationElement> próprios objetos.  
  
<a name="Activating_the_CacheRequest"></a>
## <a name="activating-the-cacherequest"></a>Ativando o CacheRequest  
 O cache é executado somente quando <xref:System.Windows.Automation.AutomationElement> os objetos são recuperados enquanto um <xref:System.Windows.Automation.CacheRequest> está ativo para o thread atual. Há duas maneiras de ativar um <xref:System.Windows.Automation.CacheRequest> .  
  
 A maneira usual é chamar <xref:System.Windows.Automation.CacheRequest.Activate%2A> . Esse método retorna um objeto que implementa <xref:System.IDisposable> . A solicitação permanece ativa contanto que o <xref:System.IDisposable> objeto exista. A maneira mais fácil de controlar o tempo de vida do objeto é colocar a chamada dentro de um `using` bloco (C#) ou `Using` (Visual Basic). Isso garante que a solicitação será exibida da pilha, mesmo que uma exceção seja gerada.  
  
 Outra maneira, que é útil quando você deseja aninhar solicitações de cache, é chamar <xref:System.Windows.Automation.CacheRequest.Push%2A> . Isso coloca a solicitação em uma pilha e a ativa. A solicitação permanece ativa até ser removida da pilha pelo <xref:System.Windows.Automation.CacheRequest.Pop%2A> . A solicitação se tornará temporariamente inativa se outra solicitação for enviada para a pilha; somente a solicitação superior na pilha está ativa.  
  
<a name="Retrieving_Cached_Properties"></a>
## <a name="retrieving-cached-properties"></a>Recuperando propriedades armazenadas em cache  
 Você pode recuperar as propriedades armazenadas em cache de um elemento por meio dos métodos e propriedades a seguir.  
  
- <xref:System.Windows.Automation.AutomationElement.GetCachedPropertyValue%2A>  
  
- <xref:System.Windows.Automation.AutomationElement.Cached%2A>  
  
 Uma exceção será gerada se a propriedade solicitada não estiver no cache.  
  
 <xref:System.Windows.Automation.AutomationElement.Cached%2A>, like <xref:System.Windows.Automation.AutomationElement.Current%2A> , expõe propriedades individuais como membros de uma estrutura. No entanto, você não precisa recuperar essa estrutura; Você pode acessar as propriedades individuais diretamente. Por exemplo, a <xref:System.Windows.Automation.AutomationElement.AutomationElementInformation.Name%2A> propriedade pode ser Obtida de `element.Cached.Name` , onde `element` é um <xref:System.Windows.Automation.AutomationElement> .  
  
<a name="Retrieving_Cached_Control_Patterns"></a>
## <a name="retrieving-cached-control-patterns"></a>Recuperando padrões de controle armazenados em cache  
 Você pode recuperar os padrões de controle em cache de um elemento por meio dos métodos a seguir.  
  
- <xref:System.Windows.Automation.AutomationElement.GetCachedPattern%2A>  
  
- <xref:System.Windows.Automation.AutomationElement.TryGetCachedPattern%2A>  
  
 Se o padrão não estiver no cache, o <xref:System.Windows.Automation.AutomationElement.GetCachedPattern%2A> gerará uma exceção e <xref:System.Windows.Automation.AutomationElement.TryGetCachedPattern%2A> retornará `false` .  
  
 Você pode recuperar as propriedades armazenadas em cache de um padrão de controle usando a `Cached` Propriedade do objeto Pattern. Você também pode recuperar os valores atuais por meio da `Current` propriedade, mas somente se <xref:System.Windows.Automation.AutomationElementMode.None> não tiver sido especificado quando a <xref:System.Windows.Automation.AutomationElement> foi recuperada. ( <xref:System.Windows.Automation.AutomationElementMode.Full> é o valor padrão, e isso permite o acesso aos valores atuais.)  
  
<a name="Retrieving_Cached_Children_and_Parents"></a>
## <a name="retrieving-cached-children-and-parents"></a>Recuperando filhos e pais armazenados em cache  
 Quando você recupera um <xref:System.Windows.Automation.AutomationElement> e solicita o Caching para filhos desse elemento por meio da <xref:System.Windows.Automation.CacheRequest.TreeScope%2A> propriedade da solicitação, ele é posteriormente possível obter os elementos filho da <xref:System.Windows.Automation.AutomationElement.CachedChildren%2A> Propriedade do elemento que você recuperou.  
  
 Se <xref:System.Windows.Automation.TreeScope.Element> foi incluído no escopo da solicitação de cache, o elemento raiz da solicitação estará posteriormente disponível na <xref:System.Windows.Automation.AutomationElement.CachedParent%2A> propriedade de qualquer um dos elementos filho.  
  
> [!NOTE]
> Você não pode armazenar em cache pais ou ancestrais do elemento raiz da solicitação.  
  
<a name="Updating_the_Cache"></a>
## <a name="updating-the-cache"></a>Atualizando o cache  
 O cache é válido somente quando nada é alterado no [!INCLUDE[TLA2#tla_ui](../../../includes/tla2sharptla-ui-md.md)] . Seu aplicativo é responsável por atualizar o cache, normalmente em resposta a eventos.  
  
 Se você se inscrever em um evento enquanto um <xref:System.Windows.Automation.CacheRequest> estiver ativo, você obtém um <xref:System.Windows.Automation.AutomationElement> com um cache atualizado como a origem do evento sempre que seu delegado de manipulador de eventos é chamado. Você também pode atualizar informações armazenadas em cache para um elemento chamando <xref:System.Windows.Automation.AutomationElement.GetUpdatedCache%2A> . Você pode passar o original <xref:System.Windows.Automation.CacheRequest> para atualizar todas as informações que foram anteriormente armazenadas em cache.  
  
 A atualização do cache não altera as propriedades de nenhuma <xref:System.Windows.Automation.AutomationElement> referência existente.  
  
## <a name="see-also"></a>Confira também

- [Automação de Eventos de Interface de Usuário para Clientes.](ui-automation-events-for-clients.md)
- [Usar armazenamento em cache em automação de interface do usuário](use-caching-in-ui-automation.md)
- [Exemplo de FetchTimer](https://docs.microsoft.com/previous-versions/dotnet/netframework-3.5/ms771456(v=vs.90))
