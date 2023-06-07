---
title: Design de interface
ms.date: 10/22/2008
ms.technology: dotnet-standard
helpviewer_keywords:
- interfaces [.NET Framework], design guidelines
- type design guidelines, interfaces
- class library design guidelines [.NET Framework], interfaces
ms.assetid: a016bd18-6710-4358-9438-9f190a295392
ms.openlocfilehash: f589d47d5b945179430275598996b2fb77e92848
ms.sourcegitcommit: 33deec3e814238fb18a49b2a7e89278e27888291
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/02/2020
ms.locfileid: "84289025"
---
# <a name="interface-design"></a>Design de interface
Embora a maioria das APIs seja melhor modelada com classes e estruturas, há casos em que as interfaces são mais apropriadas ou são a única opção.

 O CLR não dá suporte a várias heranças (ou seja, classes CLR não podem herdar de mais de uma classe base), mas permite que os tipos implementem uma ou mais interfaces além de herdar de uma classe base. Portanto, as interfaces são geralmente usadas para atingir o efeito de várias heranças. Por exemplo, <xref:System.IDisposable> é uma interface que permite que os tipos ofereçam suporte a Disposability independente de qualquer outra hierarquia de herança na qual desejam participar.

 A outra situação na qual a definição de uma interface é apropriada é a criação de uma interface comum que pode ser suportada por vários tipos, incluindo alguns tipos de valor. Tipos de valor não podem herdar de tipos diferentes de <xref:System.ValueType> , mas podem implementar interfaces, portanto, usar uma interface é a única opção para fornecer um tipo de base comum.

 ✔️ definir uma interface se precisar que uma API comum seja suportada por um conjunto de tipos que inclui tipos de valor.

 ✔️ Considere definir uma interface se precisar dar suporte à sua funcionalidade em tipos que já herdam de algum outro tipo.

 ❌Evite usar interfaces de marcador (interfaces sem membros).

 Se você precisar marcar uma classe como tendo uma característica específica (marcador), em geral, use um atributo personalizado em vez de uma interface.

 ✔️ fornecer pelo menos um tipo que seja uma implementação de uma interface.

 Isso ajuda a validar o design da interface. Por exemplo, <xref:System.Collections.Generic.List%601> é uma implementação da <xref:System.Collections.Generic.IList%601> interface.

 ✔️ fornece pelo menos uma API que consome cada interface que você define (um método que usa a interface como um parâmetro ou uma propriedade digitada como a interface).

 Isso ajuda a validar o design da interface. Por exemplo, <xref:System.Collections.Generic.List%601.Sort%2A?displayProperty=nameWithType> consome a <xref:System.Collections.Generic.IComparer%601?displayProperty=nameWithType> interface.

 ❌Não adicione membros a uma interface que foi enviada anteriormente.

 Isso interromperia as implementações da interface. Você deve criar uma nova interface para evitar problemas de controle de versão.

 Exceto pelas situações descritas nestas diretrizes, você deve, em geral, escolher classes em vez de interfaces em projetando bibliotecas reutilizáveis de código gerenciado.

 *Partes © 2005, 2009 Microsoft Corporation. Todos os direitos reservados.*

 *Reimpresso com permissão da Pearson Education, Inc. das [Diretrizes de Design do Framework: convenções, linguagens e padrões para bibliotecas do .NET reutilizável, 2ª edição](https://www.informit.com/store/framework-design-guidelines-conventions-idioms-and-9780321545619) por Krzysztof Cwalina e Brad Abrams, publicado em 22 de outubro de 2008 por Addison-Wesley Professional como parte da série de desenvolvimento do Microsoft Windows.*

## <a name="see-also"></a>Veja também

- [Diretrizes de design de tipo](type.md)
- [Diretrizes de design de estrutura](index.md)
