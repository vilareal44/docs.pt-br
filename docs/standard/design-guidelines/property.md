---
title: Design de propriedade
ms.date: 10/22/2008
ms.technology: dotnet-standard
helpviewer_keywords:
- member design guidelines, properties
- properties [.NET Framework], design guidelines
ms.assetid: 127cbc0c-cbed-48fd-9c89-7c5d4f98f163
ms.openlocfilehash: c49b42ab369ace582c76d7f326da309415e8c45b
ms.sourcegitcommit: 33deec3e814238fb18a49b2a7e89278e27888291
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/02/2020
ms.locfileid: "84291936"
---
# <a name="property-design"></a>Design de propriedade
Embora as propriedades sejam tecnicamente muito semelhantes aos métodos, elas são muito diferentes em termos de seus cenários de uso. Eles devem ser vistos como campos inteligentes. Eles têm a sintaxe de chamada de campos e a flexibilidade dos métodos.

 ✔️ criar propriedades somente obtenção se o chamador não puder alterar o valor da propriedade.

 Tenha em mente que, se o tipo da propriedade for um tipo de referência mutável, o valor da propriedade poderá ser alterado mesmo se a propriedade for somente obtenção.

 ❌Não forneça Propriedades de conjunto ou propriedades com o setter com acessibilidade mais ampla do que o getter.

 Por exemplo, não use Propriedades com um setter público e um getter protegido.

 Se o getter da propriedade não puder ser fornecido, implemente a funcionalidade como um método em vez disso. Considere iniciar o nome do método com `Set` e seguir com o que você teria chamado de propriedade. Por exemplo, <xref:System.AppDomain> tem um método chamado `SetCachePath` em vez de ter uma propriedade somente Set chamada `CachePath` .

 ✔️ fornecem valores padrão razoáveis para todas as propriedades, garantindo que os padrões não resultem em uma brecha de segurança ou em um código extremamente ineficiente.

 ✔️ permitir que as propriedades sejam definidas em qualquer ordem, mesmo se isso resultar em um estado temporário inválido do objeto.

 É comum que duas ou mais propriedades sejam interrelacionadas a um ponto em que alguns valores de uma propriedade possam ser inválidos, dado os valores de outras propriedades no mesmo objeto. Nesses casos, as exceções resultantes do estado inválido devem ser adiadas até que as propriedades inter-relacionadas sejam realmente usadas juntas pelo objeto.

 ✔️ preservar o valor anterior se um setter de propriedade lançar uma exceção.

 ❌Evite lançar exceções de getters de propriedade.

 Os getters de propriedade devem ser operações simples e não devem ter nenhuma condição. Se um getter puder gerar uma exceção, ele provavelmente deve ser reprojetado para ser um método. Observe que essa regra não se aplica aos indexadores, onde podemos esperar exceções como resultado da validação dos argumentos.

### <a name="indexed-property-design"></a>Design de propriedade indexada
 Uma propriedade indexada é uma propriedade especial que pode ter parâmetros e pode ser chamada com sintaxe especial semelhante à indexação de matriz.

 As propriedades indexadas são conhecidas como indexadores. Os indexadores devem ser usados somente em APIs que fornecem acesso a itens em uma coleção lógica. Por exemplo, uma string é uma coleção de caracteres e o indexador em <xref:System.String?displayProperty=nameWithType> foi adicionado para acessar seus caracteres.

 ✔️ Considere o uso de indexadores para fornecer acesso aos dados armazenados em uma matriz interna.

 ✔️ Considere fornecer indexadores em tipos que representam coleções de itens.

 ❌Evite usar propriedades indexadas com mais de um parâmetro.

 Se o design exigir vários parâmetros, reconsidere se a propriedade representa realmente um acessador para uma coleção lógica. Se não tiver, use métodos em vez disso. Considere iniciar o nome do método com `Get` ou `Set` .

 ❌Evite indexadores com tipos de parâmetro diferentes de <xref:System.Int32?displayProperty=nameWithType> , <xref:System.Int64?displayProperty=nameWithType> , <xref:System.String?displayProperty=nameWithType> , <xref:System.Object?displayProperty=nameWithType> ou uma enumeração.

 Se o design exigir outros tipos de parâmetros, reavalie fortemente se a API representa realmente um acessador para uma coleção lógica. Se não tiver, use um método. Considere iniciar o nome do método com `Get` ou `Set` .

 ✔️ Use o nome `Item` para propriedades indexadas, a menos que haja um nome obviamente melhor (por exemplo, consulte a <xref:System.String.Chars%2A> propriedade em `System.String` ).

 Em C#, os indexadores são por item nomeado padrão. O <xref:System.Runtime.CompilerServices.IndexerNameAttribute> pode ser usado para personalizar esse nome.

 ❌Não forneça um indexador e métodos que sejam semanticamente equivalentes.

 ❌Não forneça mais de uma família de indexadores sobrecarregados em um tipo.

 Isso é imposto pelo compilador C#.

 ❌Não use Propriedades indexadas não padrão.

 Isso é imposto pelo compilador C#.

### <a name="property-change-notification-events"></a>Eventos de notificação de alteração de propriedade
 Às vezes, é útil fornecer um evento notificando o usuário sobre as alterações em um valor de propriedade. Por exemplo, `System.Windows.Forms.Control` gera um `TextChanged` evento depois que o valor de sua `Text` propriedade é alterado.

 ✔️ Considere gerar eventos de notificação de alteração quando os valores de propriedade em APIs de alto nível (geralmente componentes do designer) forem modificados.

 Se houver um bom cenário para que um usuário saiba quando uma propriedade de um objeto está sendo alterada, o objeto deverá gerar um evento de notificação de alteração para a propriedade.

 No entanto, é improvável que haja sobrecarga para gerar tais eventos para APIs de nível baixo, como tipos de base ou coleções. Por exemplo, <xref:System.Collections.Generic.List%601> não geraria esses eventos quando um novo item for adicionado à lista e a `Count` propriedade for alterada.

 ✔️ Considere gerar eventos de notificação de alteração quando o valor de uma propriedade for alterado por meio de forças externas.

 Se um valor de propriedade for alterado por meio de alguma força externa (de uma maneira diferente de chamar métodos no objeto), os eventos de geração indicam ao desenvolvedor que o valor está mudando e foi alterado. Um bom exemplo é a `Text` propriedade de um controle de caixa de texto. Quando o usuário digita o texto em um `TextBox` , o valor da propriedade é alterado automaticamente.

 *Partes © 2005, 2009 Microsoft Corporation. Todos os direitos reservados.*

 *Reimpresso com permissão da Pearson Education, Inc. das [Diretrizes de Design do Framework: convenções, linguagens e padrões para bibliotecas do .NET reutilizável, 2ª edição](https://www.informit.com/store/framework-design-guidelines-conventions-idioms-and-9780321545619) por Krzysztof Cwalina e Brad Abrams, publicado em 22 de outubro de 2008 por Addison-Wesley Professional como parte da série de desenvolvimento do Microsoft Windows.*

## <a name="see-also"></a>Veja também

- [Diretrizes de design de membro](member.md)
- [Diretrizes de design de estrutura](index.md)
