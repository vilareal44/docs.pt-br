---
title: Design de Struct
ms.date: 10/22/2008
ms.technology: dotnet-standard
helpviewer_keywords:
- class library design guidelines [.NET Framework], structures
- deallocating structures
- allocating structures
- value types, structures
- structure design
- type design guidelines, structures
- structures [.NET Framework], design guidelines
ms.assetid: 1f48b2d8-608c-4be6-9ba4-d8f203ed9f9f
ms.openlocfilehash: c6ac53014e048da3a90dd7b8e961176f61e90355
ms.sourcegitcommit: 33deec3e814238fb18a49b2a7e89278e27888291
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/02/2020
ms.locfileid: "84290805"
---
# <a name="struct-design"></a>Design de Struct
O tipo de valor de uso geral é geralmente referido como struct, sua palavra-chave C#. Esta seção fornece diretrizes para o design geral de struct.

 ❌Não forneça um construtor sem parâmetros para uma struct.

 Seguir essa diretriz permite que matrizes de structs sejam criadas sem a necessidade de executar o Construtor em cada item da matriz. Observe que o C# não permite que structs tenham construtores sem parâmetros.

 ❌Não defina tipos de valores mutáveis.

 Os tipos de valores mutáveis têm vários problemas. Por exemplo, quando um getter de propriedade retorna um tipo de valor, o chamador recebe uma cópia. Como a cópia é criada implicitamente, os desenvolvedores podem não estar cientes de que estão modificando a cópia e não o valor original. Além disso, algumas linguagens (linguagens dinâmicas, em particular) têm problemas ao usar tipos de valor mutável porque até mesmo variáveis locais, quando desreferenciadas, fazem com que uma cópia seja feita.

 ✔️ garantir que um estado em que todos os dados da instância esteja definido como zero, falso ou nulo (conforme apropriado) seja válido.

 Isso impede a criação acidental de instâncias inválidas quando uma matriz de structs é criada.

 ✔️ implementar <xref:System.IEquatable%601> em tipos de valor.

 O <xref:System.Object.Equals%2A?displayProperty=nameWithType> método em tipos de valor causa boxing, e sua implementação padrão não é muito eficiente, pois usa reflexão. <xref:System.IEquatable%601.Equals%2A>pode ter um desempenho muito melhor e pode ser implementado para que não cause boxing.

 ❌Não estenda explicitamente <xref:System.ValueType> . Na verdade, a maioria dos idiomas impede isso.

 Em geral, as estruturas podem ser muito úteis, mas devem ser usadas somente para valores pequenos, únicos e imutáveis que não serão emoldurados com frequência.

 *Partes © 2005, 2009 Microsoft Corporation. Todos os direitos reservados.*

 *Reimpresso com permissão da Pearson Education, Inc. das [Diretrizes de Design do Framework: convenções, linguagens e padrões para bibliotecas do .NET reutilizável, 2ª edição](https://www.informit.com/store/framework-design-guidelines-conventions-idioms-and-9780321545619) por Krzysztof Cwalina e Brad Abrams, publicado em 22 de outubro de 2008 por Addison-Wesley Professional como parte da série de desenvolvimento do Microsoft Windows.*

## <a name="see-also"></a>Veja também

- [Diretrizes de design de tipo](type.md)
- [Diretrizes de design de estrutura](index.md)
- [Escolher entre Classe e Struct](choosing-between-class-and-struct.md)
