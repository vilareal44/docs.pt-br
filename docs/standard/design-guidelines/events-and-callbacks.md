---
title: Eventos e retornos de chamada
ms.date: 10/22/2008
ms.technology: dotnet-standard
helpviewer_keywords:
- events [.NET Framework], extensibility
- methods [.NET Framework], callback
- callback methods
- callbacks
ms.assetid: 48b55c60-495f-4089-9396-97f9122bba7c
ms.openlocfilehash: 4000944c3b913f71bc18462cea9062e9237ae53f
ms.sourcegitcommit: e02d17b2cf9c1258dadda4810a5e6072a0089aee
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/01/2020
ms.locfileid: "85619527"
---
# <a name="events-and-callbacks"></a>Eventos e retornos de chamada
Os retornos de chamada são pontos de extensibilidade que permitem que uma estrutura chame de volta no código do usuário por meio de um delegado. Esses delegados são geralmente passados para a estrutura por meio de um parâmetro de um método.

 Os eventos são um caso especial de retornos de chamada que dão suporte a sintaxe conveniente e consistente para fornecer o delegado (um manipulador de eventos). Além disso, a conclusão e os designers de instruções do Visual Studio fornecem ajuda no uso de APIs baseadas em eventos. (Consulte [design de eventos](event.md).)

 ✔️ Considere o uso de retornos de chamada para permitir que os usuários forneçam código personalizado a ser executado pela estrutura.

 ✔️ Considere o uso de eventos para permitir que os usuários personalizem o comportamento de uma estrutura sem a necessidade de entender o design orientado a objeto.

 ✔️ preferem eventos em retornos de chamada simples, pois eles são mais familiares para uma gama mais ampla de desenvolvedores e são integrados com a conclusão da instrução do Visual Studio.

 ❌Evite usar retornos de chamada em APIs sensíveis ao desempenho.

 ✔️ usar os novos `Func<...>` tipos, `Action<...>` , ou `Expression<...>` em vez de delegados personalizados, ao definir APIs com retornos de chamada.

 `Func<...>`e `Action<...>` representam delegados genéricos. `Expression<...>`representa definições de função que podem ser compiladas e, posteriormente, invocadas em tempo de execução, mas também podem ser serializadas e passadas para processos remotos.

 ✔️ medir e entender as implicações de desempenho do uso do `Expression<...>` , em vez de usar o `Func<...>` e os `Action<...>` delegados.

 `Expression<...>`os tipos estão na maioria dos casos logicamente equivalentes a `Func<...>` e `Action<...>` delegados. A principal diferença entre eles é que os delegados se destinam a serem usados em cenários de processo local; as expressões são destinadas a casos em que é benéfico e possível avaliar a expressão em um computador ou processo remoto.

 ✔️ entender que ao chamar um delegado, você está executando código arbitrário e pode ter repercussões de segurança, exatidão e compatibilidade.

 *Partes &copy; 2005, 2009 Microsoft Corporation. Todos os direitos reservados.*

 *Reimpresso com permissão da Pearson Education, Inc. das [Diretrizes de Design do Framework: convenções, linguagens e padrões para bibliotecas do .NET reutilizável, 2ª edição](https://www.informit.com/store/framework-design-guidelines-conventions-idioms-and-9780321545619) por Krzysztof Cwalina e Brad Abrams, publicado em 22 de outubro de 2008 por Addison-Wesley Professional como parte da série de desenvolvimento do Microsoft Windows.*

## <a name="see-also"></a>Consulte também

- [Designer voltado para extensibilidade](designing-for-extensibility.md)
- [Diretrizes de design de estrutura](index.md)
