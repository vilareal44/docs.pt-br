---
title: Abstrações (tipos e interfaces abstratos)
ms.date: 10/22/2008
ms.technology: dotnet-standard
helpviewer_keywords:
- interfaces [.NET Framework], abstract
- abstract interfaces [.NET Framework]
- abstract types [.NET Framework]
- types [.NET Framework], abstract
ms.assetid: 0a632bc7-9b03-44ee-8842-c82f88672a45
ms.openlocfilehash: fd5b8fe10d0dcca5da3a2093f7be37f6d88b382a
ms.sourcegitcommit: 33deec3e814238fb18a49b2a7e89278e27888291
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/02/2020
ms.locfileid: "84280606"
---
# <a name="abstractions-abstract-types-and-interfaces"></a>Abstrações (tipos e interfaces abstratos)
Uma abstração é um tipo que descreve um contrato, mas não fornece uma implementação completa do contrato. As abstrações geralmente são implementadas como classes ou interfaces abstratas e são fornecidas com um conjunto bem definido de documentação de referência que descreve a semântica necessária dos tipos que implementam o contrato. Algumas das abstrações mais importantes no .NET Framework incluem <xref:System.IO.Stream> , <xref:System.Collections.Generic.IEnumerable%601> e <xref:System.Object> .

 Você pode estender estruturas implementando um tipo concreto que dá suporte ao contrato de uma abstração e usando esse tipo concreto com APIs de estrutura consumindo (operando em) a abstração.

 Uma abstração significativa e útil que é capaz de resistir ao teste de tempo é muito difícil de criar. A dificuldade principal é obter o conjunto certo de membros, não há mais nem menos. Se uma abstração tiver muitos membros, torna-se difícil ou até mesmo impossível de implementar. Se ele tiver poucos membros para a funcionalidade prometida, ele se tornará inútil em muitos cenários interessantes.

 Muitas abstrações em uma estrutura também afetam negativamente a usabilidade da estrutura. Geralmente, é muito difícil entender uma abstração sem entender como ela se encaixa na visão mais ampla das implementações concretas e das APIs que operam na abstração. Além disso, os nomes de abstrações e seus membros são necessariamente abstratos, o que geralmente os torna confusos e inacessívels sem primeiro compreender o contexto mais amplo de seu uso.

 No entanto, as abstrações fornecem extensibilidade extremamente poderosa que os outros mecanismos de extensibilidade geralmente não podem corresponder. Eles estão no núcleo de muitos padrões arquitetônicos, como plug-ins, inversão de controle (IoC), pipelines e assim por diante. Eles também são extremamente importantes para a capacidade de teste de estruturas. As boas abstrações possibilitam o stub de dependências pesadas para fins de teste de unidade. Em resumo, as abstrações são responsáveis pelas sofisticações buscadas das estruturas orientadas a objeto modernas.

 ❌Não forneça abstrações a menos que elas sejam testadas desenvolvendo várias implementações concretas e APIs que consomem as abstrações.

 ✔️ Escolha cuidadosamente entre uma classe abstrata e uma interface ao projetar uma abstração.

 ✔️ Considere fornecer testes de referência para implementações concretas de abstrações. Esses testes devem permitir que os usuários testem se suas implementações implementam corretamente o contrato.

 *Partes © 2005, 2009 Microsoft Corporation. Todos os direitos reservados.*

 *Reimpresso com permissão da Pearson Education, Inc. das [Diretrizes de Design do Framework: convenções, linguagens e padrões para bibliotecas do .NET reutilizável, 2ª edição](https://www.informit.com/store/framework-design-guidelines-conventions-idioms-and-9780321545619) por Krzysztof Cwalina e Brad Abrams, publicado em 22 de outubro de 2008 por Addison-Wesley Professional como parte da série de desenvolvimento do Microsoft Windows.*

## <a name="see-also"></a>Veja também

- [Diretrizes de design de estrutura](index.md)
- [Designer voltado para extensibilidade](designing-for-extensibility.md)
