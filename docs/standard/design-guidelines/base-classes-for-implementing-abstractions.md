---
title: Classes base para implementar abstrações
ms.date: 10/22/2008
ms.technology: dotnet-standard
helpviewer_keywords:
- abstractions [.NET Framework]
- base classes, abstractions
ms.assetid: 37a2d9a4-9721-482a-a40f-eee2c1d97875
ms.openlocfilehash: 6af63373b7cbb571265f14ac36028953525fcc7f
ms.sourcegitcommit: 33deec3e814238fb18a49b2a7e89278e27888291
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/02/2020
ms.locfileid: "84280563"
---
# <a name="base-classes-for-implementing-abstractions"></a>Classes base para implementar abstrações
Estritamente falando, uma classe se torna uma classe base quando outra classe é derivada dela. No entanto, para a finalidade desta seção, uma classe base é uma classe projetada principalmente para fornecer uma abstração comum ou para outras classes reutilizarem algumas implementações padrão por meio de herança. As classes base geralmente ficam no meio das hierarquias de herança, entre uma abstração na raiz de uma hierarquia e várias implementações personalizadas na parte inferior.

 Eles servem como auxiliares de implementação para implementar abstrações. Por exemplo, uma das abstrações da estrutura para coleções ordenadas de itens é a <xref:System.Collections.Generic.IList%601> interface. <xref:System.Collections.Generic.IList%601>A implementação não é trivial e, portanto, a estrutura fornece várias classes base, como <xref:System.Collections.ObjectModel.Collection%601> e <xref:System.Collections.ObjectModel.KeyedCollection%602> , que servem como auxiliares para implementar coleções personalizadas.

 As classes base geralmente não são adequadas para servir como abstrações por si mesmas, pois tendem a conter muita implementação. Por exemplo, a `Collection<T>` classe base contém muitas implementações relacionadas ao fato de que ela implementa a interface não genérica `IList` (para integrar melhor com coleções não genéricas) e ao fato de ser uma coleção de itens armazenados na memória em um de seus campos.

 Conforme discutido anteriormente, as classes base podem fornecer ajuda inestimável para os usuários que precisam implementar abstrações, mas ao mesmo tempo elas podem ser uma responsabilidade significativa. Eles adicionam área de superfície e aumentam a profundidade das hierarquias de herança e, portanto, complicam conceitualmente a estrutura. Portanto, as classes base só devem ser usadas se fornecerem valor significativo para os usuários da estrutura. Eles devem ser evitados se fornecerem valor apenas para os implementadores da estrutura; nesse caso, a delegação para uma implementação interna em vez da herança de uma classe base deve ser fortemente considerada.

 ✔️ Considere tornar as classes base abstratas mesmo que elas não contenham membros abstratos. Isso se comunica claramente com os usuários dos quais a classe é projetada exclusivamente para serem herdados.

 ✔️ Considere colocar classes base em um namespace separado dos tipos de cenário principal. Por definição, as classes base são destinadas a cenários de extensibilidade avançada e, portanto, não são interessantes para a maioria dos usuários.

 ❌Evite nomear classes base com um sufixo "base" se a classe for destinada a uso em APIs públicas.

 *Partes © 2005, 2009 Microsoft Corporation. Todos os direitos reservados.*

 *Reimpresso com permissão da Pearson Education, Inc. das [Diretrizes de Design do Framework: convenções, linguagens e padrões para bibliotecas do .NET reutilizável, 2ª edição](https://www.informit.com/store/framework-design-guidelines-conventions-idioms-and-9780321545619) por Krzysztof Cwalina e Brad Abrams, publicado em 22 de outubro de 2008 por Addison-Wesley Professional como parte da série de desenvolvimento do Microsoft Windows.*

## <a name="see-also"></a>Veja também

- [Diretrizes de design de estrutura](index.md)
- [Designer voltado para extensibilidade](designing-for-extensibility.md)
