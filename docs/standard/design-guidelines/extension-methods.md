---
title: Métodos de Extensão
ms.date: 10/22/2008
ms.technology: dotnet-standard
ms.assetid: 5de945cb-88f4-49d7-b0e6-f098300cf357
ms.openlocfilehash: 55e6a816bbec401fdb061a3394635378b2f37424
ms.sourcegitcommit: e02d17b2cf9c1258dadda4810a5e6072a0089aee
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/01/2020
ms.locfileid: "85621594"
---
# <a name="extension-methods"></a>Métodos de Extensão
Os métodos de extensão são um recurso de linguagem que permite que métodos estáticos sejam chamados usando a sintaxe de chamada de método de instância. Esses métodos devem ter pelo menos um parâmetro, que representa a instância em que o método deve operar.

 A classe que define esses métodos de extensão é conhecida como a classe "patrocinador" e deve ser declarada como estática. Para usar métodos de extensão, é necessário importar o namespace que define a classe patrocinador.

 ❌Evite frivolously a definição de métodos de extensão, especialmente em tipos que não são de sua propriedade.

 Se você tiver o código-fonte próprio de um tipo, considere o uso de métodos de instância regular. Se você não for o proprietário e quiser adicionar um método, tenha muito cuidado. O uso liberal de métodos de extensão tem o potencial de desobstruir APIs de tipos que não foram projetados para ter esses métodos.

 ✔️ Considere o uso de métodos de extensão em qualquer um dos seguintes cenários:

- Para fornecer a funcionalidade auxiliar relevante para cada implementação de uma interface, se dito, a funcionalidade pode ser escrita em termos da interface principal. Isso ocorre porque as implementações concretas não podem ser atribuídas de outra forma a interfaces. Por exemplo, os `LINQ to Objects` operadores são implementados como métodos de extensão para todos os <xref:System.Collections.Generic.IEnumerable%601> tipos. Portanto, qualquer `IEnumerable<>` implementação é habilitada automaticamente por LINQ.

- Quando um método de instância introduzir uma dependência em algum tipo, mas essa dependência quebraria as regras de gerenciamento de dependência. Por exemplo, uma dependência de <xref:System.String> to <xref:System.Uri?displayProperty=nameWithType> é provavelmente não desejável e, portanto, `String.ToUri()` o método de instância retornado `System.Uri` seria o design errado de uma perspectiva de gerenciamento de dependência. Um método de extensão estático `Uri.ToUri(this string str)` retornado `System.Uri` seria um design muito melhor.

 ❌Evite definir métodos de extensão no <xref:System.Object?displayProperty=nameWithType> .

 Os usuários do VB não poderão chamar esses métodos em referências de objeto usando a sintaxe do método de extensão. O VB não dá suporte à chamada de tais métodos porque, no VB, a declaração de uma referência como Object força todas as invocações de método nele para serem associadas tardias (o membro real chamado é determinado em tempo de execução), enquanto as associações aos métodos de extensão são determinadas em tempo de compilação (limite inicial).

 Observe que a diretriz se aplica a outros idiomas em que o mesmo comportamento de ligação está presente, ou onde os métodos de extensão não têm suporte.

 ❌Não coloque os métodos de extensão no mesmo namespace que o tipo estendido, a menos que ele seja para adicionar métodos a interfaces ou para o gerenciamento de dependência.

 ❌Evite definir dois ou mais métodos de extensão com a mesma assinatura, mesmo que eles residam em namespaces diferentes.

 ✔️ Considere definir métodos de extensão no mesmo namespace que o tipo estendido se o tipo é uma interface e se os métodos de extensão devem ser usados na maioria ou em todos os casos.

 ❌Não defina métodos de extensão que implementam um recurso em namespaces normalmente associados a outros recursos. Em vez disso, defina-os no namespace associado ao recurso ao qual eles pertencem.

 ❌Evite a nomenclatura genérica de namespaces dedicados a métodos de extensão (por exemplo, "extensões"). Em vez disso, use um nome descritivo (por exemplo, "roteamento").

 *Partes &copy; 2005, 2009 Microsoft Corporation. Todos os direitos reservados.*

 *Reimpresso com permissão da Pearson Education, Inc. das [Diretrizes de Design do Framework: convenções, linguagens e padrões para bibliotecas do .NET reutilizável, 2ª edição](https://www.informit.com/store/framework-design-guidelines-conventions-idioms-and-9780321545619) por Krzysztof Cwalina e Brad Abrams, publicado em 22 de outubro de 2008 por Addison-Wesley Professional como parte da série de desenvolvimento do Microsoft Windows.*

## <a name="see-also"></a>Consulte também

- [Diretrizes de design de membro](member.md)
- [Diretrizes de design de estrutura](index.md)
