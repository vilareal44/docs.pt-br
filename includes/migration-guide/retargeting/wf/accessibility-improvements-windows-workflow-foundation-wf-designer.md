---
ms.openlocfilehash: d420be76645fc71ac922542fa49f799a473e9a83
ms.sourcegitcommit: e02d17b2cf9c1258dadda4810a5e6072a0089aee
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/01/2020
ms.locfileid: "85614304"
---
### <a name="accessibility-improvements-in-windows-workflow-foundation-wf-workflow-designer"></a>Melhorias de acessibilidade no designer de fluxo de trabalho do Windows Workflow Foundation (WF)

#### <a name="details"></a>Detalhes

O designer de fluxo de trabalho do Windows Workflow Foundation (WF) está melhorando a forma como funciona com tecnologias de acessibilidade. essas melhorias incluem as seguintes alterações:

- A ordem de tabulação é alterada para esquerda para a direita e de cima para baixo em alguns controles:
- A janela de inicialização de correlação para definir dados de correlação para a atividade <xref:System.ServiceModel.Activities.InitializeCorrelation>
- A janela de definição de conteúdo para as atividades <xref:System.ServiceModel.Activities.Receive>, <xref:System.ServiceModel.Activities.Send>, <xref:System.ServiceModel.Activities.SendReply> e <xref:System.ServiceModel.Activities.ReceiveReply>
- Mais funções estão disponíveis por meio do teclado:
- Ao editar as propriedades de uma atividade, grupos de propriedades podem ser recolhidos pelo teclado na primeira vez em que são colocados em foco.
- Ícones de aviso agora são acessíveis por teclado.
- O botão Mais Propriedades na janela Propriedades agora pode ser acessado pelo teclado.
- Usuários do teclado agora podem acessar os itens de cabeçalho nos painéis de Argumentos e Variáveis do Designer de Fluxo de Trabalho.
- Melhor visibilidade de itens com foco, como ao:
- Adicionar linhas às grades de dados usadas pelo Designer de Fluxo de Trabalho e por designers de atividades.
- Percorrer campos nas atividades <xref:System.ServiceModel.Activities.ReceiveReply> e <xref:System.ServiceModel.Activities.SendReply>.
- Definir valores padrão para variáveis ou argumentos
- Agora, leitores de tela podem reconhecer corretamente:
- Pontos de interrupção definidos no Designer de Fluxo de Trabalho.
- As atividades <xref:System.Activities.Statements.FlowSwitch%601>, <xref:System.Activities.Statements.FlowDecision> e <xref:System.ServiceModel.Activities.CorrelationScope>.
- O conteúdo da atividade <xref:System.ServiceModel.Activities.Receive>.
- O Tipo de Destino da atividade <xref:System.Activities.Statements.InvokeMethod>.
- A caixa de combinação Exceção e a seção Finalmente na atividade <xref:System.Activities.Statements.TryCatch>.
- A caixa de combinação Tipo de Mensagem, o divisor na janela Adicionar Inicializadores de Correlação, a janela Definição de Conteúdo e a janela de Definição de CorrelatesOn nas atividades de mensagens (<xref:System.ServiceModel.Activities.Receive>, <xref:System.ServiceModel.Activities.Send>, <xref:System.ServiceModel.Activities.SendReply> e <xref:System.ServiceModel.Activities.ReceiveReply>).
- Transições de máquinas de estado e destinos de transição.
- Anotações e conectores em atividades <xref:System.Activities.Statements.FlowDecision>.
- Os menus de contexto (clique com o botão direito do mouse) para atividades.
- Os editores de valor de propriedade, o botão Limpar Pesquisa, os botões de classificação Por Categoria e Ordem Alfabética e a caixa de diálogo Editor de Expressão na grade de propriedades.
- A porcentagem de zoom no Designer de Fluxo de Trabalho.
- O separador nas atividades <xref:System.Activities.Statements.Parallel> e <xref:System.Activities.Statements.Pick>.
- A atividade <xref:System.Activities.Statements.InvokeDelegate>.
- A janela Selecionar Tipos para atividades de dicionário (`Microsoft.Activities.AddToDictionary&lt;TKey,TValue>`, `Microsoft.Activities.RemoveFromDictionary&lt;TKey,TValue>` etc.).
- A janela Procurar e Selecionar um Tipo .NET.
- Trilhas de navegação no Designer de Fluxo de Trabalho.
- Usuários que escolherem temas de Alto Contraste verão muitas melhorias na visibilidade do Designer de Fluxo de Trabalho e em seus controles, como melhores taxas de contraste entre elementos e caixas de seleção mais notáveis usadas para elementos de foco.

#### <a name="suggestion"></a>Sugestão

Se você tiver um aplicativo com um designer de fluxo de trabalho hospedado novamente, seu aplicativo poderá se beneficiar dessas alterações ao executar qualquer uma das seguintes ações:

- Recompilar o aplicativo para ser destinado ao .NET Framework 4.7.1. Essas alterações de acessibilidade são habilitadas por padrão.
- Se seu aplicativo for destinado ao .NET Framework 4.7 ou anterior, mas estiver em execução no .NET Framework 4.7.1, você poderá recusar esses comportamentos de acessibilidade herdados adicionando a seguinte [opção de AppContext](~/docs/framework/configure-apps/file-schema/runtime/appcontextswitchoverrides-element.md) à seção `<runtime>` do arquivo app.config e definindo-a como `false`, como mostra o exemplo a seguir.

```xml
<?xml version="1.0" encoding="utf-8"?>
<configuration>
  <startup>
    <supportedRuntime version="v4.0" sku=".NETFramework,Version=v4.7"/>
  </startup>
  <runtime>
    <!-- AppContextSwitchOverrides value attribute is in the form of 'key1=true/false;key2=true/false  -->
    <AppContextSwitchOverrides value="Switch.UseLegacyAccessibilityFeatures=false" />
  </runtime>
</configuration>
```

Aplicativos destinados ao .NET Framework 4.7.1 ou posterior que desejam preservar o comportamento de acessibilidade herdado pode aceitar o uso das funcionalidades de acessibilidade herdadas definindo explicitamente essa opção de AppContext como `true`.

| Name    | Valor       |
|:--------|:------------|
| Escopo   | Secundária       |
| Versão | 4.7.1       |
| Type    | Redirecionando |
