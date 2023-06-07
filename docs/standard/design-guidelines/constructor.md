---
title: Design do construtor
ms.date: 10/22/2008
ms.technology: dotnet-standard
helpviewer_keywords:
- member design guidelines, constructors
- constructors, design guidelines
- instance constructors
- type constructors
- virtual members
- constructors, types
- parameterless constructors
- static constructors
ms.assetid: b4496afe-5fa7-4bb0-85ca-70b0ef21e6fc
ms.openlocfilehash: a258bebac57258cc1e8fbe2d6b5ccce88cb28872
ms.sourcegitcommit: 33deec3e814238fb18a49b2a7e89278e27888291
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/02/2020
ms.locfileid: "84280342"
---
# <a name="constructor-design"></a>Design do construtor

Há dois tipos de construtores: construtores de tipo e construtores de instância.

Construtores de tipo são estáticos e são executados pelo CLR antes de o tipo ser usado. Os construtores de instância são executados quando uma instância de um tipo é criada.

Construtores de tipo não podem usar parâmetros. Construtores de instância podem. Os construtores de instância que não usam parâmetros geralmente são chamados de construtores sem parâmetros.

Os construtores são a maneira mais natural de criar instâncias de um tipo. A maioria dos desenvolvedores pesquisará e tentará usar um Construtor antes que eles considerem maneiras alternativas de criar instâncias (como métodos de fábrica).

✔️ Considere fornecer construtores simples, ideais, padrão.

Um Construtor simples tem um número muito pequeno de parâmetros e todos os parâmetros são primitivos ou enums. Esses construtores simples aumentam a usabilidade da estrutura.

✔️ Considere o uso de um método de fábrica estático em vez de um construtor se a semântica da operação desejada não for mapeada diretamente para a construção de uma nova instância ou se as diretrizes de design de Construtor a seguir não se sentirem naturais.

✔️ Use parâmetros de construtor como atalhos para definir propriedades principais.

Não deve haver nenhuma diferença na semântica entre o uso do construtor vazio seguido por alguns conjuntos de propriedades e o uso de um construtor com vários argumentos.

✔️ usar o mesmo nome para parâmetros de construtor e uma propriedade se os parâmetros do Construtor forem usados para simplesmente definir a propriedade.

A única diferença entre esses parâmetros e as propriedades deve estar entre maiúsculas e minúsculas.

✔️ EXECUTAR o mínimo de trabalho no construtor.

Os construtores não devem fazer muito trabalho além de capturar os parâmetros do construtor. O custo de qualquer outro processamento deve ser atrasado até que seja necessário.

✔️ gerar exceções de construtores de instância, se apropriado.

✔️ declarar explicitamente o construtor público sem parâmetros em classes, se esse construtor for necessário.

Se você não declarar explicitamente nenhum construtor em um tipo, muitas linguagens (como C#) adicionarão automaticamente um construtor público sem parâmetros. (Classes abstratas obtêm um construtor protegido.)

Adicionar um construtor com parâmetros a uma classe impede que o compilador adicione o construtor sem parâmetros. Isso geralmente causa alterações de interrupção acidentais.

❌Evite definir construtores sem parâmetros explicitamente em structs.

Isso torna a criação de matriz mais rápida, porque se o construtor sem parâmetros não estiver definido, ele não precisará ser executado em todos os slots na matriz. Observe que muitos compiladores, incluindo C#, não permitem que structs tenham construtores sem parâmetros por esse motivo.

❌Evite chamar Membros virtuais em um objeto dentro de seu construtor.

Chamar um membro virtual fará com que a substituição mais derivada seja chamada, mesmo que o construtor do tipo mais derivado ainda não tenha sido totalmente executado.

## <a name="type-constructor-guidelines"></a>Diretrizes de construtor de tipo

✔️ tornar os construtores estáticos privados.

Um construtor estático, também chamado de construtor de classe, é usado para inicializar um tipo. O CLR chama o construtor estático antes que a primeira instância do tipo seja criada ou quaisquer membros estáticos nesse tipo sejam chamados. O usuário não tem controle sobre quando o construtor estático é chamado. Se um construtor estático não for privado, ele poderá ser chamado por um código diferente do CLR. Dependendo das operações executadas no construtor, isso pode causar um comportamento inesperado. O compilador C# força os construtores estáticos a serem privados.

❌Não lance exceções de construtores estáticos.

Se uma exceção for gerada de um construtor de tipo, o tipo não poderá ser usado no domínio do aplicativo atual.

✔️ Considere a inicialização de campos estáticos embutidos em vez de usar construtores estáticos explicitamente, pois o tempo de execução é capaz de otimizar o desempenho dos tipos que não têm um construtor estático explicitamente definido.

*Partes © 2005, 2009 Microsoft Corporation. Todos os direitos reservados.*

*Reimpresso com permissão da Pearson Education, Inc. das [Diretrizes de Design do Framework: convenções, linguagens e padrões para bibliotecas do .NET reutilizável, 2ª edição](https://www.informit.com/store/framework-design-guidelines-conventions-idioms-and-9780321545619) por Krzysztof Cwalina e Brad Abrams, publicado em 22 de outubro de 2008 por Addison-Wesley Professional como parte da série de desenvolvimento do Microsoft Windows.*

## <a name="see-also"></a>Veja também

- [Diretrizes de design de membro](member.md)
- [Diretrizes de design de estrutura](index.md)
