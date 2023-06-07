---
title: Seção de configuração do Windows Forms
ms.date: 04/07/2017
ms.assetid: 6eb142d5-fc98-40e2-9d90-84733f2a27ba
ms.openlocfilehash: 4de61ae3cb5eb8a3fc226881e2b7f842030dfddf
ms.sourcegitcommit: b16c00371ea06398859ecd157defc81301c9070f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/06/2020
ms.locfileid: "79151826"
---
# <a name="windows-forms-configuration-section"></a>Seção de configuração do Windows Forms
As definições de configuração do Windows Forms permitem que um aplicativo do Windows Forms armazene e recupere informações sobre configurações personalizadas de aplicativo, como suporte a vários monitores, suporte ao DPI e outras configurações predefinidas.

As definições de configuração de aplicativo do Windows Forms são armazenadas em um elemento `System.Windows.Forms.ApplicationConfigurationSection` do arquivo de configuração de aplicativo.

## <a name="syntax"></a>Sintaxe

```xml
<configuration>
  <System.Windows.Forms.ApplicationConfigurationSection>
  ...
  </System.Windows.Forms.ApplicationConfigurationSection>
</configuration>
```

## <a name="attributes-and-elements"></a>Atributos e elementos

As seções a seguir descrevem atributos, elementos filho e elementos pai.

### <a name="attributes"></a>Atributos

Nenhum.

### <a name="child-elements"></a>Elementos filho

Elemento  |Descrição |
---------|---------|
[`<add>`](windows-forms-add-configuration-element.md) | Adiciona uma chave de definição de configuração com um valor especificado |

### <a name="parent-elements"></a>Elementos pai

Elemento  |Descrição |
---------|---------|
[\<configuration>](../configuration-element.md) | O elemento raiz em cada arquivo de configuração usado pelos aplicativos do Common Language Runtime e Windows Forms |

## <a name="remarks"></a>Comentários

A partir do .NET Framework 4.7, o elemento `<System.Windows.Forms.ApplicationConfigurationSection>` permite configurar os aplicativos do Windows Forms para aproveitar os recursos adicionados em versões recentes do .NET Framework.

O `<System.Windows.Forms.ApplicationConfigurationSection>` elemento pode incluir um ou mais [`<add>`](windows-forms-add-configuration-element.md) elementos filho, cada um deles define uma definição de configuração específica.

## <a name="see-also"></a>Confira também

- [Esquema de arquivos de configuração](../index.md)
- [Suporte a alto DPI no Windows Forms](../../../winforms/high-dpi-support-in-windows-forms.md)
