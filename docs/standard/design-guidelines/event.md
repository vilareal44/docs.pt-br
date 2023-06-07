---
title: Design de eventos
ms.date: 10/22/2008
ms.technology: dotnet-standard
helpviewer_keywords:
- pre-events
- events [.NET Framework], design guidelines
- member design guidelines, events
- event handlers [.NET Framework], event design guidelines
- post-events
- signatures, event handling
ms.assetid: 67b3c6e2-6a8f-480d-a78f-ebeeaca1b95a
ms.openlocfilehash: 852c99b1a41691911f7ae82d3b8104526811757d
ms.sourcegitcommit: 33deec3e814238fb18a49b2a7e89278e27888291
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/02/2020
ms.locfileid: "84289818"
---
# <a name="event-design"></a>Design de eventos
Os eventos são a forma mais comumente usada de retornos de chamada (construções que permitem que a estrutura chame no código do usuário). Outros mecanismos de retorno de chamada incluem membros que utilizam delegados, membros virtuais e plug-ins baseados em interface. os dados de estudos de usabilidade indicam que a maioria dos desenvolvedores é mais confortável usando eventos do que usar os outros mecanismos de retorno de chamada. Os eventos são perfeitamente integrados ao Visual Studio e a várias linguagens.

 É importante observar que há dois grupos de eventos: eventos gerados antes que um estado do sistema seja alterado, chamado pré-eventos e eventos gerados depois de um Estado ser alterado, chamado post-events. Um exemplo de um pré-evento seria `Form.Closing` , que é gerado antes de um formulário ser fechado. Um exemplo de um post-Event seria `Form.Closed` , que é gerado depois que um formulário é fechado.

 ✔️ usar o termo "Raise" para eventos em vez de "Fire" ou "Trigger".

 ✔️ usar <xref:System.EventHandler%601?displayProperty=nameWithType> em vez de criar manualmente novos delegados para serem usados como manipuladores de eventos.

 ✔️ Considere o uso de uma subclasse de <xref:System.EventArgs> como o argumento do evento, a menos que você tenha certeza absoluta de que o evento nunca precisará transportar dados para o método de manipulação de eventos; nesse caso, você pode usar o `EventArgs` tipo diretamente.

 Se você enviar uma API usando `EventArgs` diretamente, nunca poderá adicionar dados a serem transportados com o evento sem perder a compatibilidade. Se você usar uma subclasse, mesmo que inicialmente completamente vazia, você poderá adicionar propriedades à subclasse quando necessário.

 ✔️ usar um método virtual protegido para gerar cada evento. Isso só é aplicável a eventos não estáticos em classes sem lacre, não a structs, a classes seladas ou a eventos estáticos.

 A finalidade do método é fornecer uma maneira para uma classe derivada manipular o evento usando uma substituição. A substituição é uma maneira mais flexível, mais rápida e natural de lidar com eventos de classe base em classes derivadas. Por convenção, o nome do método deve começar com "on" e ser seguido do nome do evento.

 A classe derivada pode optar por não chamar a implementação base do método em sua substituição. Esteja preparado para isso por não incluir nenhum processamento no método necessário para que a classe base funcione corretamente.

 ✔️ executar um parâmetro para o método protegido que gera um evento.

 O parâmetro deve ser nomeado `e` e deve ser digitado como a classe de argumento de evento.

 ❌Não passe NULL como o remetente ao gerar um evento não estático.

 ✔️ passar NULL como o remetente ao gerar um evento estático.

 ❌Não passe nulo como o parâmetro de dados de evento ao gerar um evento.

 Você deve passar `EventArgs.Empty` se não quiser passar dados para o método de manipulação de eventos. Os desenvolvedores esperam que esse parâmetro não seja nulo.

 ✔️ Considere a geração de eventos que o usuário final pode cancelar. Isso se aplica apenas a eventos anteriores.

 Use <xref:System.ComponentModel.CancelEventArgs?displayProperty=nameWithType> ou sua subclasse como o argumento de evento para permitir que o usuário final cancele eventos.

### <a name="custom-event-handler-design"></a>Design do manipulador de eventos personalizado
 Há casos em que `EventHandler<T>` não podem ser usados, como quando a estrutura precisa trabalhar com versões anteriores do CLR, que não davam suporte a genéricos. Nesses casos, talvez seja necessário criar e desenvolver um delegado de manipulador de eventos personalizado.

 ✔️ usar um tipo de retorno de void para manipuladores de eventos.

 Um manipulador de eventos pode invocar vários métodos de manipulação de eventos, possivelmente em vários objetos. Se os métodos de manipulação de eventos tivessem permissão para retornar um valor, haveria vários valores de retorno para cada invocação de evento.

 ✔️ usar `object` como o tipo do primeiro parâmetro do manipulador de eventos e chamá-lo `sender` .

 ✔️ Use <xref:System.EventArgs?displayProperty=nameWithType> ou sua subclasse como o tipo do segundo parâmetro do manipulador de eventos e chame-o `e` .

 ❌Não têm mais de dois parâmetros em manipuladores de eventos.

 *Partes © 2005, 2009 Microsoft Corporation. Todos os direitos reservados.*

 *Reimpresso com permissão da Pearson Education, Inc. das [Diretrizes de Design do Framework: convenções, linguagens e padrões para bibliotecas do .NET reutilizável, 2ª edição](https://www.informit.com/store/framework-design-guidelines-conventions-idioms-and-9780321545619) por Krzysztof Cwalina e Brad Abrams, publicado em 22 de outubro de 2008 por Addison-Wesley Professional como parte da série de desenvolvimento do Microsoft Windows.*

## <a name="see-also"></a>Veja também

- [Diretrizes de design de membro](member.md)
- [Diretrizes de design de estrutura](index.md)
