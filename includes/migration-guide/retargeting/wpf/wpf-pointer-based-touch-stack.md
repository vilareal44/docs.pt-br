---
ms.openlocfilehash: eb89cbc72d8b09fd1aa5bc775a6c539eb208fa70
ms.sourcegitcommit: e02d17b2cf9c1258dadda4810a5e6072a0089aee
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/01/2020
ms.locfileid: "85614310"
---
### <a name="wpf-pointer-based-touch-stack"></a>Pilha de toque baseada em ponteiro do WPF

#### <a name="details"></a>Detalhes

Essa alteração possibilita habilitar uma pilha de toque/caneta do WPF baseada em WM_POINTER opcional.  Desenvolvedores que não habilitarem explicitamente essa opção não deverão ver alterações no comportamento de toque/caneta do WPF. Problemas conhecidos atuais com a pilha de toque/caneta baseada em WM_POINTER opcional:

- Não há suporte para escrita à tinta em tempo real.
- Embora os plug-ins de caneta e escrita à tinta ainda funcionem, eles serão processados no thread da interface do usuário, o que pode levar a um desempenho ruim.
- Alterações de comportamento devido a alterações na promoção de eventos de toque/caneta para eventos de mouse.
- A manipulação pode se comportar de maneira diferente
- Arrastar/soltar não mostrará comentários apropriados para entrada por toque
- Isso não afeta entrada de caneta
- Arrastar/soltar não pode mais ser iniciado em eventos de toque/caneta
- Isso pode travar o aplicativo até que a entrada do mouse seja detectada.
- Em vez disso, os desenvolvedores devem iniciar a ação de arrastar e soltar usando eventos de mouse.

#### <a name="suggestion"></a>Sugestão

Desenvolvedores que desejam habilitar essa pilha podem adicionar/mesclar o seguinte ao arquivo app.config do aplicativo:

<pre><code class="lang-xml">&lt;configuration&gt;&#13;&#10;&lt;runtime&gt;&#13;&#10;&lt;AppContextSwitchOverrides value=&quot;Switch.System.Windows.Input.Stylus.EnablePointerSupport=true&quot;/&gt;&#13;&#10;&lt;/runtime&gt;&#13;&#10;&lt;/configuration&gt;&#13;&#10;</code></pre>

Remover isso ou definir o valor como false desabilita essa pilha opcional. Observe que essa pilha está disponível somente na Atualização do Windows 10 para Criadores e superiores.

| Name    | Valor       |
|:--------|:------------|
| Escopo   | Microsoft Edge        |
| Versão | 4.7         |
| Type    | Redirecionando |
