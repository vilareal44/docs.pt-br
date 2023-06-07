---
title: Nomes de membros de tipo
description: Conheça as diretrizes para nomear membros de tipo no .NET, como métodos, propriedades, eventos e campos.
ms.date: 10/22/2008
ms.technology: dotnet-standard
helpviewer_keywords:
- events [.NET Framework], names
- methods [.NET Framework], names
- type members
- properties [.NET Framework], names
- fields, names
- field names
- names [.NET Framework], type members
- members [.NET Framework], type
ms.assetid: af5a0903-36af-4c2a-b848-cf959affeaa5
ms.openlocfilehash: de613673989bd174ac80adda566d04600059642d
ms.sourcegitcommit: 7137e12f54c4e83a94ae43ec320f8cf59c1772ea
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/10/2020
ms.locfileid: "84662492"
---
# <a name="names-of-type-members"></a>Nomes de membros de tipo
Tipos são compostos de membros: métodos, propriedades, eventos, construtores e campos. As seções a seguir descrevem as diretrizes de nomenclatura de membros de tipo.

## <a name="names-of-methods"></a>Nomes de métodos
 Como os métodos são os meios para executar uma ação, as diretrizes de design exigem que os nomes de métodos sejam verbos ou frases verbais. Seguir essa diretriz também serve para distinguir os nomes de métodos de nomes de propriedades e tipos, que são substantivos ou frases adjetivas.

 ✔️ fornecem nomes de métodos que são verbos ou frases de verbo.

```csharp
public class String {
    public int CompareTo(...);
    public string[] Split(...);
    public string Trim();
}
```

## <a name="names-of-properties"></a>Nomes de propriedades
 Diferentemente de outros membros, as propriedades devem ser nomeadas com uma frase substantivada ou um adjetivo. Isso porque uma propriedade se refere a dados, e o nome da propriedade reflete isso. PascalCasing sempre é usado para nomes de propriedade.

 ✔️ as propriedades de nome usando um substantivo, uma frase de substantivo ou um adjetivo.

 ❌Não têm propriedades que correspondam ao nome dos métodos "Get", como no exemplo a seguir:

 `public string TextWriter { get {...} set {...} }` `public string GetTextWriter(int value) { ... }`

 Esse padrão geralmente indica que a propriedade deveria realmente ser um método.

 ✔️ as propriedades da coleção de nomes com uma frase plural que descreve os itens na coleção em vez de usar uma frase singular seguida de "List" ou "Collection".

 ✔️ as propriedades booleanas de nome com uma frase afirmativo ( `CanSeek` em vez de `CantSeek` ). Opcionalmente, você também pode prefixar propriedades booleanas com "is", "Can" ou "tem", mas apenas onde ele agrega valor.

 ✔️ Considere atribuir uma propriedade com o mesmo nome que o seu tipo.

 Por exemplo, a seguinte propriedade obtém e define corretamente um valor de enumeração denominado `Color`, portanto, a propriedade é chamada `Color`:

```csharp
public enum Color {...}
public class Control {
    public Color Color { get {...} set {...} }
}
```

## <a name="names-of-events"></a>Nomes de eventos
 Eventos sempre fazem referência a uma ação, seja uma ação que está acontecendo ou que já ocorreu. Portanto, assim como acontece com os métodos, os eventos são nomeados com verbos e o tempo verbal é usado para indicar o horário em que o evento é acionado.

 ✔️ eventos de nome com um verbo ou uma frase verbal.

 Os exemplos incluem `Clicked`, `Painting`, `DroppedDown`, etc.

 ✔️ fornecer nomes de eventos com um conceito de antes e depois, usando as dezenas e as últimas.

 Por exemplo, um evento de fechamento gerado antes de uma janela ser fechada seria chamado de `Closing`, e um gerado após a janela ser fechada seria chamado de `Closed`.

 ❌Não use prefixos "Before" ou "After" ou as pós-correções para indicar pré e eventos Post. Use os tempos verbais Pretérito e Presente conforme descrito.

 ✔️ manipuladores de eventos de nome (delegados usados como tipos de eventos) com o sufixo "EventHandler", conforme mostrado no exemplo a seguir:

 `public delegate void ClickedEventHandler(object sender, ClickedEventArgs e);`

 ✔️ usar dois parâmetros chamados `sender` e `e` em manipuladores de eventos.

 O parâmetro do remetente representa o objeto que acionou o evento. O parâmetro do remetente normalmente é do tipo `object`, mesmo se for possível empregar um tipo mais específico.

 ✔️ classes de argumento de evento de nome com o sufixo "EventArgs".

## <a name="names-of-fields"></a>Nomes de campos
 As diretrizes de nomenclatura de campo se aplicam a campos públicos e protegidos estáticos. Campos particulares e internos não são cobertos pelas diretrizes, e campos de instância pública ou protegida não são permitidos pelas [diretrizes de design de membro](member.md).

 ✔️ Use PascalCasing em nomes de campo.

 ✔️ os campos de nome usando um substantivo, uma frase de substantivo ou um adjetivo.

 ❌Não use um prefixo para nomes de campo.

 Por exemplo, não use "g_" ou "s_" para indicar campos estáticos.

 *Partes © 2005, 2009 Microsoft Corporation. Todos os direitos reservados.*

 *Reimpresso com permissão da Pearson Education, Inc. das [Diretrizes de Design do Framework: convenções, linguagens e padrões para bibliotecas do .NET reutilizável, 2ª edição](https://www.informit.com/store/framework-design-guidelines-conventions-idioms-and-9780321545619) por Krzysztof Cwalina e Brad Abrams, publicado em 22 de outubro de 2008 por Addison-Wesley Professional como parte da série de desenvolvimento do Microsoft Windows.*

## <a name="see-also"></a>Confira também

- [Diretrizes de design de estrutura](index.md)
- [Diretrizes de nomenclatura](naming-guidelines.md)
