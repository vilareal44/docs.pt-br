---
title: Nomes de namespaces
description: Use estas diretrizes para nomear namespaces como parte das diretrizes para criar bibliotecas que estendem e interajam com bibliotecas .NET.
ms.date: 10/22/2008
helpviewer_keywords:
- names [.NET Framework], conflicts
- names [.NET Framework], namespaces
- type names, conflicts
- namespaces [.NET Framework], names
- names [.NET Framework], type names
ms.assetid: a49058d2-0276-43a7-9502-04adddf857b2
ms.openlocfilehash: e435e0281165b4a9f12bbccbeb10401b57375dcb
ms.sourcegitcommit: 33deec3e814238fb18a49b2a7e89278e27888291
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/02/2020
ms.locfileid: "84290195"
---
# <a name="names-of-namespaces"></a>Nomes de namespaces
Assim como acontece com outras diretrizes de nomenclatura, a meta ao nomear namespaces é criar uma clareza suficiente para o programador usando a estrutura para saber imediatamente qual é a probabilidade do conteúdo do namespace. O modelo a seguir especifica a regra geral para nomes de namespaces:

 `<Company>.(<Product>|<Technology>)[.<Feature>][.<Subnamespace>]`

 Os exemplos são os seguintes:

 `Fabrikam.Math` `Litware.Security`

 ✔️ os nomes de namespace de prefixo com um nome de empresa para impedir que os namespaces de diferentes empresas tenham o mesmo nome.

 ✔️ usar um nome de produto estável e independente de versão no segundo nível de um nome de namespace.

 ❌Não use hierarquias organizacionais como base para nomes em hierarquias de namespace, pois os nomes de grupo nas empresas tendem a ser de curta duração. Organize a hierarquia de namespaces em relação a grupos de tecnologias relacionadas.

 ✔️ usar PascalCasing e separar componentes de namespace com pontos (por exemplo, `Microsoft.Office.PowerPoint` ). Se sua marca emprega um uso não tradicional, você deve seguir a capitalização definida por sua marca, mesmo que ela se desvie da capitalização normal do namespace.

 ✔️ Considere o uso de nomes de namespace do plural quando apropriado.

 Por exemplo, use `System.Collections` ao invés de `System.Collection`. No entanto, os nomes e acrônimos de marca são exceções a essa regra. Por exemplo, use `System.IO` ao invés de `System.IOs`.

 ❌Não use o mesmo nome para um namespace e um tipo nesse namespace.

 Por exemplo, não use `Debug` como um nome de namespace e, em seguida, forneça uma classe chamada `Debug` no mesmo namespace. Vários compiladores exigem que esses tipos sejam totalmente qualificados.

### <a name="namespaces-and-type-name-conflicts"></a>Namespaces e conflitos de nome de tipo
 ❌Não introduza nomes de tipos genéricos, como,, `Element` `Node` `Log` e `Message` .

 Há uma probabilidade muito alta de fazer isso resultará em conflitos de nome de tipo em cenários comuns. Você deve qualificar os nomes de tipo genérico ( `FormElement` , `XmlNode` , `EventLog` , `SoapMessage` ).

 Há diretrizes específicas para evitar conflitos de nome de tipo para diferentes categorias de namespaces.

- **Namespaces de modelo de aplicativo**

     Os namespaces que pertencem a um único modelo de aplicativo geralmente são usados juntos, mas quase nunca são usados com namespaces de outros modelos de aplicativo. Por exemplo, o <xref:System.Windows.Forms?displayProperty=nameWithType> namespace é muito raramente usado junto com o <xref:System.Web.UI?displayProperty=nameWithType> namespace. Veja a seguir uma lista de grupos de namespace de modelo de aplicativo conhecidos:

     `System.Windows*` `System.Web.UI*`

     ❌Não atribua o mesmo nome a tipos em namespaces em um único modelo de aplicativo.

     Por exemplo, não adicione um tipo chamado `Page` ao <xref:System.Web.UI.Adapters?displayProperty=nameWithType> namespace, pois o <xref:System.Web.UI?displayProperty=nameWithType> namespace já contém um tipo chamado `Page` .

- **Namespaces de infraestrutura**

     Esse grupo contém namespaces que raramente são importados durante o desenvolvimento de aplicativos comuns. Por exemplo, os `.Design` namespaces são usados principalmente ao desenvolver ferramentas de programação. Evitar conflitos com tipos nesses namespaces não é crítico.

- **Principais namespaces**

     Os namespaces de núcleo incluem todos os `System` namespaces, excluindo namespaces dos modelos de aplicativo e os namespaces de infraestrutura. Os namespaces de núcleo incluem, entre outros,,,, `System` `System.IO` `System.Xml` e `System.Net` .

     ❌Não forneça nomes de tipos que possam entrar em conflito com qualquer tipo nos namespaces de núcleo.

     Por exemplo, nunca use `Stream` como um nome de tipo. Ele estaria em conflito com <xref:System.IO.Stream?displayProperty=nameWithType> um tipo muito usado.

- **Grupos de namespace de tecnologia**

     Essa categoria inclui todos os namespaces com os mesmos primeiros dois nós de namespace `(<Company>.<Technology>*` ), como `Microsoft.Build.Utilities` e `Microsoft.Build.Tasks` . É importante que os tipos que pertencem a uma única tecnologia não entrem em conflito entre si.

     ❌Não atribua nomes de tipos que possam entrar em conflito com outros tipos em uma única tecnologia.

     ❌Não introduza o nome do tipo em conflito entre os tipos nos namespaces de tecnologia e um namespace de modelo de aplicativo (a menos que a tecnologia não se destinar a ser usada com o modelo de aplicativo).

 *Partes © 2005, 2009 Microsoft Corporation. Todos os direitos reservados.*

 *Reimpresso com permissão da Pearson Education, Inc. das [Diretrizes de Design do Framework: convenções, linguagens e padrões para bibliotecas do .NET reutilizável, 2ª edição](https://www.informit.com/store/framework-design-guidelines-conventions-idioms-and-9780321545619) por Krzysztof Cwalina e Brad Abrams, publicado em 22 de outubro de 2008 por Addison-Wesley Professional como parte da série de desenvolvimento do Microsoft Windows.*

## <a name="see-also"></a>Veja também

- [Diretrizes de design de estrutura](index.md)
- [Diretrizes de nomenclatura](naming-guidelines.md)
