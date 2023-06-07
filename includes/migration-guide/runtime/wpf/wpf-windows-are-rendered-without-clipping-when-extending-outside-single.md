---
ms.openlocfilehash: d276e2bbf24c8b2389a0a8078c62c327c3729d50
ms.sourcegitcommit: e02d17b2cf9c1258dadda4810a5e6072a0089aee
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/01/2020
ms.locfileid: "85621041"
---
### <a name="wpf-windows-are-rendered-without-clipping-when-extending-outside-a-single-monitor"></a>Janelas do WPF são renderizadas sem distorção ao se estender para fora de um único monitor

#### <a name="details"></a>Detalhes

No .NET Framework 4.6 em execução no Windows 8 e superior, a janela inteira é renderizada sem distorção quando ela se estende para fora da exibição única em um cenário de vários monitores. Isso é diferente de versões anteriores do .NET Framework, que distorceriam janelas do WPF que se estendiam para fora de uma única exibição.

#### <a name="suggestion"></a>Sugestão

Esse comportamento (com distorção ou não) pode ser definido explicitamente usando o elemento <code>&lt;EnableMultiMonitorDisplayClipping&gt;</code> em <code>&lt;appSettings&gt;</code> no arquivo de configuração do aplicativo ou definindo a propriedade <code>EnableMultiMonitorDisplayClipping</code> durante a inicialização do aplicativo.

| Name    | Valor       |
|:--------|:------------|
| Escopo   |Secundária|
|Versão|4.6|
|Type|Runtime|
