---
ms.openlocfilehash: f907cb1939e111c586d68243e6b8a8e291974e36
ms.sourcegitcommit: e02d17b2cf9c1258dadda4810a5e6072a0089aee
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/01/2020
ms.locfileid: "85615599"
---
### <a name="wpf-layout-rounding-of-margins-has-changed"></a>Arredondamento de layout do WPF foi alterado

#### <a name="details"></a>Detalhes

A maneira como as margens são arredondadas, bem como as bordas e a tela de fundo dentro delas, foi alterada. Como resultado dessa alteração:

- A largura ou altura dos elementos pode aumentar ou reduzir em um pixel no máximo.
- O posicionamento de um objeto pode ser movido até um pixel, no máximo.
- Os elementos centralizados podem estar vertical ou horizontalmente fora do centro em, no máximo, um pixel.
Por padrão, esse novo layout é habilitado somente para aplicativos que se destinam ao .NET Framework 4.6.

#### <a name="suggestion"></a>Sugestão

Uma vez que essa modificação tende a eliminar a distorção da direita ou da parte inferior dos controles do WPF em DPIs altos, os aplicativos que de destinam a versões anteriores do .NET Framework, mas estão sendo executados no .NET Framework 4.6, podem aderir a esse novo comportamento adicionando a seguinte linha à seção `<runtime>` do arquivo app.config:

<pre><code class="lang-xml">&lt;AppContextSwitchOverrides value=&quot;Switch.MS.Internal.DoNotApplyLayoutRoundingToMarginsAndBorderThickness=false&quot; /&gt;&#39;&#13;&#10;</code></pre>

Os aplicativos que são direcionados ao .NET Framework 4.6, mas querem que os controles do WPF sejam renderizados usando o algoritmo de layout anterior podem fazer isso adicionando a seguinte linha à seção `<runtime>` do arquivo app.config:

<pre><code class="lang-xml">&lt;AppContextSwitchOverrides value=&quot;Switch.MS.Internal.DoNotApplyLayoutRoundingToMarginsAndBorderThickness=true&quot; /&gt;&#39;.&#13;&#10;</code></pre>

| Name    | Valor       |
|:--------|:------------|
| Escopo   | Secundária       |
| Versão | 4.6         |
| Type    | Redirecionando |
