---
title: Membros virtuais
ms.date: 10/22/2008
ms.technology: dotnet-standard
helpviewer_keywords:
- overridable members
- virtual members
- members [.NET Framework], virtual
ms.assetid: 8ff4eb97-0364-43ec-8a02-934b5cd94d19
ms.openlocfilehash: 918208bb44f84988b7fe903c589e82c7bf1f59e3
ms.sourcegitcommit: e02d17b2cf9c1258dadda4810a5e6072a0089aee
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/01/2020
ms.locfileid: "85620762"
---
# <a name="virtual-members"></a>Membros virtuais
Os membros virtuais podem ser substituídos, alterando assim o comportamento da subclasse. Eles são bastante semelhantes aos retornos de chamada em termos da extensibilidade que eles fornecem, mas são melhores em termos de desempenho de execução e consumo de memória. Além disso, os membros virtuais sentem-se mais naturais em cenários que exigem a criação de um tipo especial de um Type (especialização) existente.

 Os membros virtuais têm um desempenho melhor do que retornos de chamada e eventos, mas não têm melhor desempenho do que métodos não virtuais.

 A principal desvantagem dos membros virtuais é que o comportamento de um membro virtual só pode ser modificado no momento da compilação. O comportamento de um retorno de chamada pode ser modificado em tempo de execução.

 Membros virtuais, como retornos de chamada (e talvez mais de retornos de chamada), são caros de projetar, testar e manter, pois qualquer chamada para um membro virtual pode ser substituída de maneiras imprevisíveis e pode executar código arbitrário. Além disso, geralmente é necessário muito mais esforço para definir claramente o contrato de membros virtuais, de modo que o custo de criá-los e documentá-los é maior.

 ❌Não torne os membros virtuais, a menos que você tenha um bom motivo para fazer isso e esteja ciente de todos os custos relacionados à criação, ao teste e à manutenção de membros virtuais.

 Os membros virtuais são menos tolerante em termos de alterações que podem ser feitas a eles sem a necessidade de perder a compatibilidade. Além disso, eles são mais lentos que os membros não virtuais, principalmente porque as chamadas para membros virtuais não são embutidas.

 ✔️ Considere limitar a extensibilidade apenas ao que é absolutamente necessário.

 ✔️ preferir a acessibilidade protegida sobre acessibilidade pública para membros virtuais. Os membros públicos devem fornecer extensibilidade (se necessário) chamando um membro virtual protegido.

 Os membros públicos de uma classe devem fornecer o conjunto certo de funcionalidade para consumidores diretos dessa classe. Os membros virtuais são projetados para serem substituídos em subclasses e a acessibilidade protegida é uma ótima maneira de definir o escopo de todos os pontos de extensibilidade virtual para onde eles podem ser usados.

 *Partes &copy; 2005, 2009 Microsoft Corporation. Todos os direitos reservados.*

 *Reimpresso com permissão da Pearson Education, Inc. das [Diretrizes de Design do Framework: convenções, linguagens e padrões para bibliotecas do .NET reutilizável, 2ª edição](https://www.informit.com/store/framework-design-guidelines-conventions-idioms-and-9780321545619) por Krzysztof Cwalina e Brad Abrams, publicado em 22 de outubro de 2008 por Addison-Wesley Professional como parte da série de desenvolvimento do Microsoft Windows.*

## <a name="see-also"></a>Consulte também

- [Diretrizes de design de estrutura](index.md)
- [Designer voltado para extensibilidade](designing-for-extensibility.md)
