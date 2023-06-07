---
title: Escolher entre Classe e Struct
description: Saiba como decidir se deseja criar um tipo como uma classe ou criar um tipo como uma estrutura. Entenda como tipos de referência e tipos de valor diferem no .NET.
ms.date: 10/22/2008
ms.technology: dotnet-standard
helpviewer_keywords:
- class library design guidelines [.NET Framework], structures
- class library design guidelines [.NET Framework], classes
- structures [.NET Framework], vs. classes
- classes [.NET Framework], design guidelines
- type design guidelines, structures
- structures [.NET Framework], design guidelines
- classes [.NET Framework], vs. structures
- type design guidelines, classes
ms.assetid: f8b8ec9b-0ba7-4dea-aadf-a93395cd804f
ms.openlocfilehash: 9d757e77292c1226fbe2328cce082033ae8f7003
ms.sourcegitcommit: 7137e12f54c4e83a94ae43ec320f8cf59c1772ea
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/10/2020
ms.locfileid: "84662596"
---
# <a name="choosing-between-class-and-struct"></a>Escolher entre Classe e Struct
Uma das decisões básicas de design que cada designer de estrutura enfrenta é se deve criar um tipo como uma classe (um tipo de referência) ou como uma struct (um tipo de valor). Uma boa compreensão das diferenças no comportamento dos tipos de referência e dos tipos de valor é crucial para fazer essa escolha.

 A primeira diferença entre os tipos de referência e os tipos de valor que consideraremos é que os tipos de referência são alocados no heap e no lixo coletado, enquanto os tipos de valor são alocados na pilha ou embutidos em tipos contendo e desalocados quando a pilha se desenrola ou quando seu tipo recipiente é desalocado. Portanto, alocações e desalocações de tipos de valor são em geral mais baratas do que as alocações e desalocações de tipos de referência.

 Em seguida, as matrizes dos tipos de referência são alocadas fora de linha, o que significa que os elementos da matriz são apenas referências a instâncias do tipo de referência que residem no heap. Matrizes de tipo de valor são alocadas embutidas, o que significa que os elementos da matriz são as instâncias reais do tipo de valor. Portanto, alocações e desalocações de matrizes de tipo de valor são muito mais baratas do que as alocações e desalocações de matrizes de tipo de referência. Além disso, na maioria das vezes, as matrizes de tipo de valor apresentam uma localidade muito melhor de referência.

 A próxima diferença está relacionada ao uso de memória. Os tipos de valor ficam na caixa quando são convertidos em um tipo de referência ou em uma das interfaces que implementam. Eles ficam inativos ao converter de volta para o tipo de valor. Como as caixas são objetos alocados no heap e são coletadas com lixo, muitas boxing e unboxing podem ter um impacto negativo sobre o heap, o coletor de lixo e, por fim, o desempenho do aplicativo.  Por outro lado, nenhuma Boxing ocorre conforme os tipos de referência são convertidos. (Para obter mais informações, consulte [boxing e unboxing](../../csharp/programming-guide/types/boxing-and-unboxing.md)).

 Em seguida, atribuições de tipo de referência copiam a referência, enquanto atribuições de tipo de valor copiam o valor inteiro. Portanto, as atribuições de tipos de referência grandes são mais baratas do que as atribuições de tipos de valor grande.

 Por fim, os tipos de referência são passados por referência, enquanto os tipos de valor são passados por valor. As alterações em uma instância de um tipo de referência afetam todas as referências que apontam para a instância. As instâncias de tipo de valor são copiadas quando são passadas por valor. Quando uma instância de um tipo de valor é alterada, é claro que não afeta nenhuma de suas cópias. Como as cópias não são criadas explicitamente pelo usuário, mas são criadas implicitamente quando argumentos são passados ou valores de retorno são retornados, os tipos de valor que podem ser alterados podem ser confusos para muitos usuários. Portanto, os tipos de valor devem ser imutáveis.

 Como regra geral, a maioria dos tipos em uma estrutura deve ser classes. No entanto, há algumas situações em que as características de um tipo de valor tornam mais apropriado o uso de structs.

 ✔️ CONSIDERAR a definição de uma struct em vez de uma classe se as instâncias do tipo forem pequenas e geralmente de curta duração ou se forem normalmente inseridas em outros objetos.

 ❌Evite definir uma struct, a menos que o tipo tenha todas as seguintes características:

- Ele representa logicamente um único valor, semelhante a tipos primitivos ( `int` , `double` , etc.).

- Ele tem um tamanho de instância inferior a 16 bytes.

- É imutável.

- Ele não precisará ser emoldurado com frequência.

 Em todos os outros casos, você deve definir seus tipos como classes.

 *Partes © 2005, 2009 Microsoft Corporation. Todos os direitos reservados.*

 *Reimpresso com permissão da Pearson Education, Inc. das [Diretrizes de Design do Framework: convenções, linguagens e padrões para bibliotecas do .NET reutilizável, 2ª edição](https://www.informit.com/store/framework-design-guidelines-conventions-idioms-and-9780321545619) por Krzysztof Cwalina e Brad Abrams, publicado em 22 de outubro de 2008 por Addison-Wesley Professional como parte da série de desenvolvimento do Microsoft Windows.*

## <a name="see-also"></a>Confira também

- [Diretrizes de design de tipo](type.md)
- [Diretrizes de design de estrutura](index.md)
