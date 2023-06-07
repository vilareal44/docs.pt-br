---
title: Design de classe abstrata
ms.date: 10/22/2008
ms.technology: dotnet-standard
helpviewer_keywords:
- type design guidelines, abstract classes
- abstract classes, design guidelines
- class library design guidelines [.NET Framework], classes
- classes [.NET Framework], abstract
- classes [.NET Framework], design guidelines
- type design guidelines, classes
ms.assetid: d3646e6d-5c1f-4922-8fb0-ec5effb30d60
ms.openlocfilehash: e6a5923f293ed536fb272f6fe6c805067aede0ab
ms.sourcegitcommit: 33deec3e814238fb18a49b2a7e89278e27888291
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/02/2020
ms.locfileid: "84280771"
---
# <a name="abstract-class-design"></a>Design de classe abstrata

❌Não defina construtores internos públicos ou protegidos em tipos abstratos.

 Os construtores só devem ser públicos se os usuários precisarem criar instâncias do tipo. Como não é possível criar instâncias de um tipo abstrato, um tipo abstrato com um construtor público é projetado incorretamente e induzindo aos usuários.

 ✔️ definir um construtor protegido ou interno em classes abstratas.

 Um construtor protegido é mais comum e simplesmente permite que a classe base faça sua própria inicialização quando os subtipos são criados.

 Um construtor interno pode ser usado para limitar implementações concretas da classe abstrata ao assembly que define a classe.

 ✔️ fornece pelo menos um tipo concreto que herda de cada classe abstrata que você envia.

 Fazer isso ajuda a validar o design da classe abstrata. Por exemplo, <xref:System.IO.FileStream?displayProperty=nameWithType> é uma implementação da <xref:System.IO.Stream?displayProperty=nameWithType> classe abstract.

 *Partes © 2005, 2009 Microsoft Corporation. Todos os direitos reservados.*

 *Reimpresso com permissão da Pearson Education, Inc. das [Diretrizes de Design do Framework: convenções, linguagens e padrões para bibliotecas do .NET reutilizável, 2ª edição](https://www.informit.com/store/framework-design-guidelines-conventions-idioms-and-9780321545619) por Krzysztof Cwalina e Brad Abrams, publicado em 22 de outubro de 2008 por Addison-Wesley Professional como parte da série de desenvolvimento do Microsoft Windows.*

## <a name="see-also"></a>Veja também

- [Diretrizes de design de tipo](type.md)
- [Diretrizes de design de estrutura](index.md)
