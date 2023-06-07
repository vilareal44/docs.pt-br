---
title: Nomes de classes, structs e interfaces
description: Use estas diretrizes para nomear classes, estruturas e interfaces como parte das diretrizes para criar bibliotecas que estendem e interajam com bibliotecas .NET.
ms.date: 10/22/2008
helpviewer_keywords:
- type names, guidelines
- classes [.NET Framework], names
- enumerations [.NET Framework], names
- names [.NET Framework], interfaces
- common type names
- names [.NET Framework], type names
- names [.NET Framework], classes
- interfaces [.NET Framework], names
- generic type parameters
ms.assetid: 87a4b0da-ed64-43b1-ac43-968576c444ce
ms.openlocfilehash: b9de9329cc8e1bfc47a46523c7119bb3b2c244d8
ms.sourcegitcommit: 33deec3e814238fb18a49b2a7e89278e27888291
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/02/2020
ms.locfileid: "84290208"
---
# <a name="names-of-classes-structs-and-interfaces"></a>Nomes de classes, structs e interfaces
As diretrizes de nomenclatura a seguir se aplicam à nomenclatura de tipo geral.

 ✔️ as classes de nome e structs com frases substantivas ou substantivo, usando PascalCasing.

 Isso distingue nomes de tipos de métodos, que são nomeados com frases de verbo.

 ✔️ as interfaces de nome com frases de adjetivo ou ocasionalmente com substantivos ou frases de substantivo.

 As frases de substantivo e substantivo devem ser usadas raramente e podem indicar que o tipo deve ser uma classe abstrata, e não uma interface.

 ❌Não dê aos nomes de classe um prefixo (por exemplo, "C").

 ✔️ Considere encerrar o nome de classes derivadas com o nome da classe base.

 Isso é muito legível e explica a relação claramente. Alguns exemplos disso no código são: `ArgumentOutOfRangeException` , que é um tipo de `Exception` , e `SerializableAttribute` , que é um tipo de `Attribute` . No entanto, é importante usar o julgamento razoável na aplicação dessa diretriz; por exemplo, a `Button` classe é um tipo de `Control` evento, embora `Control` não apareça em seu nome.

 ✔️ os nomes de interface de prefixo com a letra I, para indicar que o tipo é uma interface.

 Por exemplo, `IComponent` (substantivo descritivo), `ICustomAttributeProvider` (frase de substantivo) e `IPersistable` (adjetivo) são nomes de interface apropriados. Assim como ocorre com outros nomes de tipo, evite abreviações.

 ✔️ garantir que os nomes diferem somente pelo prefixo "I" no nome da interface quando você estiver definindo um par de interface – classe em que a classe é uma implementação padrão da interface.

## <a name="names-of-generic-type-parameters"></a>Nomes de parâmetros de tipo genérico
 Os genéricos foram adicionados ao .NET Framework 2,0. O recurso introduziu um novo tipo de identificador chamado *parâmetro de tipo*.

 ✔️ os parâmetros de tipo genérico de nome com nomes descritivos, a menos que um nome de letra única seja totalmente auto-explicativo e um nome descritivo não adicione valor.

 ✔️ Considere usar `T` como o nome do parâmetro de tipo para tipos com um parâmetro de tipo de letra única.

```csharp
public int IComparer<T> { ... }
public delegate bool Predicate<T>(T item);
public struct Nullable<T> where T:struct { ... }
```

 ✔️ os nomes de parâmetro de tipo descritivo de prefixo com `T` .

```csharp
public interface ISessionChannel<TSession> where TSession : ISession {
    TSession Session { get; }
}
```

 ✔️ CONSIDERAR a indicação de restrições colocadas em um parâmetro de tipo no nome do parâmetro.

 Por exemplo, um parâmetro restrito a `ISession` pode ser chamado `TSession` .

## <a name="names-of-common-types"></a>Nomes de tipos comuns
 ✔️ Siga as diretrizes descritas na tabela a seguir ao nomear tipos derivados ou implementando determinados tipos de .NET Framework.

|Tipo base|Diretriz de tipo derivado/implementando|
|---------------|------------------------------------------|
|`System.Attribute`|✔️ Adicionar o sufixo "Attribute" aos nomes das classes de atributo personalizadas.|
|`System.Delegate`|✔️ Adicionar o sufixo "EventHandler" aos nomes de delegados que são usados em eventos.<br /><br /> ✔️ Adicionar o sufixo "callback" a nomes de delegados diferentes daqueles usados como manipuladores de eventos.<br /><br /> ❌Não adicione o sufixo "delegate" a um delegado.|
|`System.EventArgs`|✔️ Adicionar o sufixo "EventArgs".|
|`System.Enum`|❌Não Derive dessa classe; em vez disso, use a palavra-chave com suporte no seu idioma; por exemplo, em C#, use a `enum` palavra-chave.<br /><br /> ❌Não adicione o sufixo "enum" ou "Flag".|
|`System.Exception`|✔️ Adicionar o sufixo "Exception".|
|`IDictionary` <br /> `IDictionary<TKey,TValue>`|✔️ Adicionar o sufixo "Dictionary". Observe que `IDictionary` é um tipo específico de coleção, mas essa diretriz tem precedência sobre a diretriz de coleções mais gerais a seguir.|
|`IEnumerable` <br /> `ICollection` <br /> `IList` <br /> `IEnumerable<T>` <br /> `ICollection<T>` <br /> `IList<T>`|✔️ Adicionar o sufixo "coleção".|
|`System.IO.Stream`|✔️ Adicionar o sufixo "Stream".|
|`CodeAccessPermission IPermission`|✔️ Adicionar o sufixo "permissão".|

## <a name="naming-enumerations"></a>Enumerações de nomenclatura
 Os nomes dos tipos de enumeração (também chamados de enums) em geral devem seguir as regras de nomenclatura de tipo padrão (PascalCasing, etc.). No entanto, há diretrizes adicionais que se aplicam especificamente a enums.

 ✔️ usar um nome de tipo singular para uma enumeração, a menos que seus valores sejam campos de bits.

 ✔️ usar um nome de tipo plural para uma enumeração com campos de bits como valores, também chamados de sinalizadores Enum.

 ❌Não use um sufixo "enum" em nomes de tipos de enumeração.

 ❌Não use os sufixos "Flag" ou "Flags" em nomes de tipo de enumeração.

 ❌Não use um prefixo em nomes de valores de enumeração (por exemplo, "AD" para enums ADO, "RTF" para enums de Rich Text, etc.).

 *Partes © 2005, 2009 Microsoft Corporation. Todos os direitos reservados.*

 *Reimpresso com permissão da Pearson Education, Inc. das [Diretrizes de Design do Framework: convenções, linguagens e padrões para bibliotecas do .NET reutilizável, 2ª edição](https://www.informit.com/store/framework-design-guidelines-conventions-idioms-and-9780321545619) por Krzysztof Cwalina e Brad Abrams, publicado em 22 de outubro de 2008 por Addison-Wesley Professional como parte da série de desenvolvimento do Microsoft Windows.*

## <a name="see-also"></a>Veja também

- [Diretrizes de design de estrutura](index.md)
- [Diretrizes de nomenclatura](naming-guidelines.md)
