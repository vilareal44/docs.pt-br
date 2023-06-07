---
ms.openlocfilehash: f78d15338aa49de5b729aca12964924a0df00ec6
ms.sourcegitcommit: e02d17b2cf9c1258dadda4810a5e6072a0089aee
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/01/2020
ms.locfileid: "85614301"
---
### <a name="improved-accessibility-for-some-net-sdk-tools"></a>Acessibilidade aprimorada para algumas ferramentas do SDK do .NET

#### <a name="details"></a>Detalhes

No SDK do .NET Framework 4.7.1, as ferramentas SvcConfigEditor.exe e SvcTraceViewer.exe foram melhoradas com a correção de vários problemas de acessibilidade. A maioria eram problemas pequenos como um nome que não está definido ou certos padrões de automação de interface do usuário implementados incorretamente. Embora muitos usuários não saibam esses valores incorretos, os clientes que usam tecnologias assistenciais como leitores de tela acharão essas ferramentas de SDK mais acessíveis. Certamente, essas correções alteram alguns comportamentos anteriores, como a ordem do foco do teclado. Para obter todas as correções de acessibilidade nessas ferramentas, é possível adicionar o seguinte ao arquivo app.config:

```xml
<runtime>
  <AppContextSwitchOverrides value="Switch.UseLegacyAccessibilityFeatures=false"/>
</runtime>
```

| Name    | Valor       |
|:--------|:------------|
| Escopo   | Microsoft Edge        |
| Versão | 4.7.1       |
| Type    | Redirecionando |
