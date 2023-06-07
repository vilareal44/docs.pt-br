---
title: Considerações sobre versão e atualização para os desenvolvedores de C#
description: A introdução de novos recursos de linguagem de programação em sua biblioteca pode afetar o código que faz uso dela.
ms.topic: reference
ms.date: 09/19/2018
ms.openlocfilehash: f7db7c79792d04bcf592bc1858e1f0f05cb34402
ms.sourcegitcommit: 0100be20fcf23f61dab672deced70059ed71bb2e
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/17/2020
ms.locfileid: "88268121"
---
# <a name="version-and-update-considerations-for-c-developers"></a>Considerações sobre versão e atualização para os desenvolvedores de C#

A compatibilidade é uma meta muito importante quando novos recursos são adicionados à linguagem C#. Em quase todos os casos, o código existente pode ser recompilado com uma nova versão do compilador sem nenhum problema.

Mais cuidado pode ser necessário quando você adota novos recursos de linguagem de programação em uma biblioteca. Você poderá estar criando uma nova biblioteca com recursos encontrados na versão mais recente, e precisará garantir que os aplicativos criados com versões anteriores do compilador possam usá-la. Ou você pode estar atualizando uma biblioteca existente e muitos dos seus usuários talvez não tenham atualizado as versões ainda. Ao tomar decisões sobre a adoção de novos recursos, você precisará considerar duas variações de compatibilidade: compatível com a origem e compatível com binário.

## <a name="binary-compatible-changes"></a>Alterações compatíveis com binário

Alterações à sua biblioteca são **compatíveis com binário** quando sua biblioteca atualizada pode ser usada sem recompilar os aplicativos e bibliotecas que fazem uso dela. Não é necessário recompilar assemblies dependentes, tampouco é necessária qualquer alteração de código-fonte. Alterações compatíveis com binário também alterações compatíveis com a origem.

## <a name="source-compatible-changes"></a>Alterações compatíveis com a origem

Alterações à sua biblioteca são **compatíveis com a origem** quando os aplicativos e bibliotecas que usam sua biblioteca não exigem alterações de código-fonte, mas a origem deve ser recompilada em relação à nova versão para funcionar corretamente.

## <a name="incompatible-changes"></a>Alterações incompatíveis

Se uma alteração não for nem **compatível com a origem** nem **compatível com binário**, alterações de código-fonte, juntamente com a recompilação, serão necessárias nas bibliotecas e aplicativos dependentes.

## <a name="evaluate-your-library"></a>Avaliar a biblioteca

Esses conceitos de compatibilidade afetam as declarações públicas e protegidas para sua biblioteca, mas não a respectiva implementação interna. A adoção de quaisquer novos recursos internamente é sempre **compatível com binário**.  

Alterações **compatíveis com binário** oferecem uma nova sintaxe que gera o mesmo código compilado para declarações públicas que a sintaxe antiga. Por exemplo, a alteração de um método para um membro de corpo da expressão é uma alteração **compatível com binário**:

Código original:

```csharp
public double CalculateSquare(double value)
{
    return value * value;
}
```

Novo código:

```csharp
public double CalculateSquare(double value) => value * value;
```

Alterações **compatíveis com a origem** introduzem sintaxe que altera o código compilado para um membro público, mas de uma forma compatível com sites de chamada já existentes. Por exemplo, alterar a assinatura de um método de um parâmetro por valor para um `in` por referência é compatível com a origem, mas não é compatível com binário:

Código original:

```csharp
public double CalculateSquare(double value) => value * value;
```

Novo código:

```csharp
public double CalculateSquare(in double value) => value * value;
```

Os artigos [Novidades](index.md) apontam que a introdução de um recurso que afeta as declarações públicas é compatível com a origem ou compatível com binário.
