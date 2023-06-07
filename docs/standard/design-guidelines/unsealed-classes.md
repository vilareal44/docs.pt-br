---
title: Classes não seladas
ms.date: 10/22/2008
ms.technology: dotnet-standard
helpviewer_keywords:
- classes [.NET Framework], unsealed
- unsealed classes
- inheritance, classes
ms.assetid: 9a3bd505-90f5-4053-9f0d-3cf5fa3d3ebf
ms.openlocfilehash: 8e332a6382cf644c82d5e26cf5234cea08dcc693
ms.sourcegitcommit: 33deec3e814238fb18a49b2a7e89278e27888291
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/02/2020
ms.locfileid: "84289545"
---
# <a name="unsealed-classes"></a>Classes não seladas
As classes seladas não podem ser herdadas de e impedem a extensibilidade. Por outro lado, as classes que podem ser herdadas são chamadas de classes sem lacre.

 ✔️ Considere o uso de classes sem lacre sem nenhum membro virtual ou protegido adicionado como uma ótima maneira de fornecer uma extensibilidade econômica, mas muito apreciada para uma estrutura.

 Os desenvolvedores geralmente desejam herdar de classes sem lacre para adicionar membros de conveniência, como construtores personalizados, novos métodos ou sobrecargas de método. Por exemplo, o `System.Messaging.MessageQueue` não é lacrado e, portanto, permite que os usuários criem filas personalizadas que são padronizadas para um determinado caminho de fila ou para adicionar métodos personalizados que simplificam a API para cenários específicos.

 As classes não são seladas por padrão na maioria das linguagens de programação, e esse também é o padrão recomendado para a maioria das classes em estruturas. A extensibilidade proporcionada por tipos sem lacre é muito apreciada pelos usuários do Framework e é bastante barata para fornecer devido a custos de teste relativamente baixos associados a tipos sem lacre.

 *Partes © 2005, 2009 Microsoft Corporation. Todos os direitos reservados.*

 *Reimpresso com permissão da Pearson Education, Inc. das [Diretrizes de Design do Framework: convenções, linguagens e padrões para bibliotecas do .NET reutilizável, 2ª edição](https://www.informit.com/store/framework-design-guidelines-conventions-idioms-and-9780321545619) por Krzysztof Cwalina e Brad Abrams, publicado em 22 de outubro de 2008 por Addison-Wesley Professional como parte da série de desenvolvimento do Microsoft Windows.*

## <a name="see-also"></a>Veja também

- [Diretrizes de design de estrutura](index.md)
- [Designer voltado para extensibilidade](designing-for-extensibility.md)
- [Selar](sealing.md)
