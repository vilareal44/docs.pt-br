---
ms.openlocfilehash: 418bcdca1e9a325894891d7b0e080ce035e2d1b4
ms.sourcegitcommit: e02d17b2cf9c1258dadda4810a5e6072a0089aee
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/01/2020
ms.locfileid: "85614277"
---
### <a name="aspnet-accessibility-improvements-in-net-framework-471"></a>Melhorias de acessibilidade do ASP.NET no .NET Framework 4.7.1

#### <a name="details"></a>Detalhes

A partir do .NET Framework 4.7.1, o ASP.NET melhorou o funcionamento dos controles da Web ASP.NET com a tecnologia de acessibilidade no Visual Studio para dar melhor suporte a clientes do ASP.NET.  Isso inclui as seguintes alterações:

- Alterações para implementar padrões de acessibilidade de interface do usuário ausentes nos controles, como a caixa de diálogo Adicionar Campo no assistente de Exibição de Detalhes ou a caixa de diálogo Configurar ListView do assistente ListView.
- Alterações para melhorar a exibição no modo de Alto Contraste, como o Editor de Campos do Pager de Dados.
- Alterações para melhorar as experiências de navegação por teclado para controles como a caixa de diálogo Campos no assistente Editar Campos do Pager do controle DataPager, a caixa de diálogo Configurar ObjectContext ou a caixa de diálogo Configurar Seleção de Dados do assistente Configurar Fonte de Dados.

#### <a name="suggestion"></a>Sugestão

**Como aceitar ou recusar essas alterações** Para se beneficiar dessas alterações, o Designer do Visual Studio deverá ser executado no .NET Framework 4.7.1 ou posterior. O aplicativo Web pode se beneficiar dessas alterações de uma das seguintes maneiras:

- Instalar o Visual Studio 2017 15.3 ou posterior, que dá suporte a novas funcionalidades de acessibilidade com a seguinte opção de AppContext por padrão.
- Recusar os comportamentos de acessibilidade herdados adicionando a opção de AppContext `Switch.UseLegacyAccessibilityFeatures` à seção `<runtime>` do arquivo devenv.exe.config e configurando-a como `false`, como mostra o exemplo a seguir.

```xml
<?xml version="1.0" encoding="utf-8"?>
<configuration>
<runtime>
...
<!-- AppContextSwitchOverrides value attribute is in the form of 'key1=true/false;key2=true/false'  -->
<AppContextSwitchOverrides value="Switch.UseLegacyAccessibilityFeatures=false" />
...
</runtime>
</configuration>
```

Aplicativos destinados ao .NET Framework 4.7.1 ou posterior que desejam preservar o comportamento de acessibilidade herdado pode aceitar o uso das funcionalidades de acessibilidade herdadas definindo explicitamente essa opção de AppContext como `true`.

| Name    | Valor       |
|:--------|:------------|
| Escopo   | Secundária       |
| Versão | 4.7.1       |
| Type    | Redirecionando |
