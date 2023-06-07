---
title: 'Mitigação: renderização de janela WPF'
description: Saiba mais sobre o impacto e a mitigação da renderização de janelas do WPF no .NET Framework 4,6 em execução no Windows 8 ou posterior.
ms.date: 03/30/2017
ms.assetid: 28ed6bf8-141b-4b73-a4e3-44a99fae5084
ms.openlocfilehash: d624478d17a4076107438f71b0a86eeb6d9f3ea4
ms.sourcegitcommit: cf5a800a33de64d0aad6d115ffcc935f32375164
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/20/2020
ms.locfileid: "86475327"
---
# <a name="mitigation-wpf-window-rendering"></a>Mitigação: renderização de janela WPF

No .NET Framework 4.6 em execução no Windows 8 e superior, a janela inteira é renderizada sem distorção quando ela se estende para fora da exibição única em um cenário de vários monitores.

## <a name="impact"></a>Impacto

Em geral, a renderização de uma janela inteira em vários monitores sem distorção é o comportamento esperado. No entanto, no Windows 7 e versões anteriores, as janelas do WPF são cortadas quando elas ultrapassam uma exibição única, pois renderizar uma parte da janela no segundo monitor tem um impacto significativo de desempenho.

O impacto preciso da renderização das janelas do WPF em monitores no Windows 8 e superior não é precisamente quantificável, pois depende de um grande número de fatores. Em alguns casos, ele ainda pode produzir um impacto indesejável no desempenho, especialmente para os usuários que executam aplicativos de uso intensivo de gráficos e que têm janelas que transpõem monitores. Em outros casos, você pode simplesmente querer um comportamento consistente nas versões do .NET Framework.

## <a name="mitigation"></a>Atenuação

Você pode desabilitar essa alteração e reverter para o comportamento anterior de distorção de uma janela do WPF quando ela ultrapassa uma exibição única. Há duas maneiras de fazer isso:

- Ao adicionar o elemento `<EnableMultiMonitorDisplayClipping>` à seção `<appSettings>` do seu arquivo de configuração de aplicativo, você pode desabilitar ou habilitar esse comportamento em aplicativos que são executados no Windows 8 ou posterior. Por exemplo, a seguinte seção de configuração desabilita a renderização sem distorção:

  ```xml
  <appSettings>
      <add key="EnableMultiMonitorDisplayClipping" value="true"/>
    </appSettings>
  ```

  A definição de configuração do `<EnableMultiMonitorDisplayClipping>` pode ter um dos dois valores:

  - `true`, para habilitar a distorção das janelas e monitorar limites durante a renderização.

  - `false`, para desabilitar a distorção das janelas e monitorar limites durante a renderização.

- Definindo a propriedade <xref:System.Windows.CoreCompatibilityPreferences.EnableMultiMonitorDisplayClipping%2A> como `true` na inicialização do aplicativo.

## <a name="see-also"></a>Consulte também

- [Compatibilidade de aplicativos](application-compatibility.md)
