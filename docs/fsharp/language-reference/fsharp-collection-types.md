---
title: Tipos de coleção
description: 'Saiba mais sobre os tipos de coleção F # e como eles diferem dos tipos de coleção .NET.'
ms.date: 08/14/2020
ms.openlocfilehash: 197ba754d632051b5a0bf9c8364d45a1fb932f48
ms.sourcegitcommit: 0100be20fcf23f61dab672deced70059ed71bb2e
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/17/2020
ms.locfileid: "88267276"
---
# <a name="f-collection-types"></a>Tipos de coleção de F#

Ao examinar este tópico, você pode determinar qual tipo de coleção F # melhor atende a uma necessidade específica. Esses tipos de coleção diferem dos tipos de coleção no .NET, como aqueles no `System.Collections.Generic` namespace, nos quais os tipos de coleção F # são projetados de uma perspectiva de programação funcional em vez de uma perspectiva orientada a objeto. Mais especificamente, somente a coleção de matrizes tem elementos mutáveis. Portanto, ao modificar uma coleção, você cria uma instância da coleção modificada em vez de alterar a coleção original.

Os tipos de coleção também diferem no tipo de estrutura de dados em que os objetos são armazenados. Estruturas de dados como tabelas de hash, listas vinculadas e matrizes têm características de desempenho diferentes e um conjunto diferente de operações disponíveis.

## <a name="f-collection-types"></a>Tipos de coleção de F#

A tabela a seguir mostra os tipos de coleção F #.

|Type|Descrição|Links Relacionados|
|----|-----------|-------------|
|[Lista](https://msdn.microsoft.com/library/c627b668-477b-4409-91ed-06d7f1b3e4a7)|Uma série ordenada e imutável de elementos do mesmo tipo. Implementado como uma lista vinculada.|[Listas](lists.md)<br /><br />[Módulo List](https://msdn.microsoft.com/library/a2264ba3-2d45-40dd-9040-4f7aa2ad9788)|
|[Matriz](https://msdn.microsoft.com/library/0cda8040-9396-40dd-8dcd-cf48542165a1)|Uma coleção de tamanho fixo, baseada em zero, mutável de elementos de dados consecutivos que são do mesmo tipo.|[matrizes](arrays.md)<br /><br />[Módulo Array](https://msdn.microsoft.com/library/0cda8040-9396-40dd-8dcd-cf48542165a1)<br /><br />[Módulo Array2D](https://msdn.microsoft.com/library/ae1a9746-7817-4430-bcdb-a79c2411bbd3)<br /><br />[Módulo Array3D](https://msdn.microsoft.com/library/c8355e2d-add8-48a4-8aa6-1c57ae74c560)|
|[Seq](https://msdn.microsoft.com/library/2f0c87c6-8a0d-4d33-92a6-10d1d037ce75)|Uma série lógica de elementos que são todos de um tipo. As sequências são particularmente úteis quando você tem uma coleção de dados grande e ordenada, mas não espera necessariamente usar todos os elementos. Os elementos de sequência individuais são computados somente conforme necessário, portanto, uma sequência pode ter um desempenho melhor do que uma lista se nem todos os elementos forem usados. As sequências são representadas pelo `seq<'T>` tipo, que é um alias para `IEnumerable<T>` . Portanto, qualquer tipo de .NET Framework que implemente `System.Collections.Generic.IEnumerable<'T>` possa ser usado como uma sequência.|[Sequências](sequences.md)<br /><br />[Módulo Seq](https://msdn.microsoft.com/library/54e8f059-ca52-4632-9ae9-49685ee9b684)|
|[Map](https://msdn.microsoft.com/library/975316ea-55e3-4987-9994-90897ad45664)|Um dicionário imutável de elementos. Os elementos são acessados por chave.|[Módulo de mapa](https://msdn.microsoft.com/library/bfe61ead-f16c-416f-af98-56dbcbe23e4f)|
|[Configurar](https://msdn.microsoft.com/library/50cebdce-0cd7-4c5c-8ebc-f3a9e90b38d8)|Um conjunto imutável baseado em árvores binárias, em que Comparison é a função de comparação estrutural F #, que potencialmente usa implementações da `System.IComparable` interface em valores de chave.|[Definir módulo](https://msdn.microsoft.com/library/61efa732-d55d-4c32-993f-628e2f98e6a0)|

### <a name="table-of-functions"></a>Tabela de funções

Esta seção compara as funções que estão disponíveis em tipos de coleção F #. A complexidade computacional da função é determinada, em que N é o tamanho da primeira coleção e M é o tamanho da segunda coleção, se houver. Um traço (-) indica que essa função não está disponível na coleção. Como as sequências são avaliadas lentamente, uma função como Seq. Distinct pode ser O (1) porque ela retorna imediatamente, embora ainda afete o desempenho da sequência quando enumerada.

|Função|Array|Lista|Sequência|Mapeamento|Definir|Descrição|
|--------|-----|----|--------|---|---|-----------|
|acrescentar|O (N)|O (N)|O (N)|-|-|Retorna uma nova coleção que contém os elementos da primeira coleção seguida por elementos da segunda coleção.|
|add|-|-|-|O (log (N))|O (log (N))|Retorna uma nova coleção com o elemento adicionado.|
|média|O (N)|O (N)|O (N)|-|-|Retorna a média dos elementos na coleção.|
|averageBy|O (N)|O (N)|O (N)|-|-|Retorna a média dos resultados da função fornecida aplicada a cada elemento.|
|blit|O (N)|-|-|-|-|Copia uma seção de uma matriz.|
|cache|-|-|O (N)|-|-|Computa e armazena os elementos de uma sequência.|
|cast|-|-|O (N)|-|-|Converte os elementos no tipo especificado.|
|choose|O (N)|O (N)|O (N)|-|-|Aplica a função fornecida `f` a cada elemento `x` da lista. Retorna a lista que contém os resultados de cada elemento em que a função retorna `Some(f(x))` .|
|collect|O (N)|O (N)|O (N)|-|-|Aplica a função fornecida a cada elemento da coleção, concatena todos os resultados e retorna a lista combinada.|
|compareWith|-|-|O (N)|-|-|Compara duas sequências usando a função de comparação fornecida, elemento por elemento.|
|concat|O (N)|O (N)|O (N)|-|-|Combina a enumeração de enumerações fornecida como uma única Enumeração concatenada.|
|contains|-|-|-|-|O (log (N))|Retornará true se o conjunto contiver o elemento especificado.|
|containsKey|-|-|-|O (log (N))|-|Testa se um elemento está no domínio de um mapa.|
|count|-|-|-|-|O (N)|Retorna o número de elementos no conjunto.|
|countBy|-|-|O (N)|-|-|Aplica uma função de geração de chaves a cada elemento de uma sequência e retorna uma sequência que produz chaves exclusivas e seu número de ocorrências na sequência original.|
|copy|O (N)|-|O (N)|-|-|Copia a coleção.|
|create|O (N)|-|-|-|-|Cria uma matriz de elementos inteiros que são inicialmente o valor determinado.|
|atrasar|-|-|O (1)|-|-|Retorna uma sequência que é criada a partir da especificação atrasada especificada de uma sequência.|
|diferença|-|-|-|-|O (M \* log (N))|Retorna um novo conjunto com os elementos do segundo conjunto removido do primeiro conjunto.|
|distinct|||O (1)\*|||Retorna uma sequência que não contém entradas duplicadas de acordo com as comparações de hash genérico e de igualdade nas entradas. Se um elemento ocorrer várias vezes na sequência, as ocorrências posteriores serão descartadas.|
|distinctBy|||O (1)\*|||Retorna uma sequência que não contém entradas duplicadas de acordo com as comparações de hash genérico e de igualdade nas chaves que a função de geração de chave fornecida retorna. Se um elemento ocorrer várias vezes na sequência, as ocorrências posteriores serão descartadas.|
|vazio|O (1)|O (1)|O (1)|O (1)|O (1)|Cria uma coleção vazia.|
|exists|O (N)|O (N)|O (N)|O (log (N))|O (log (N))|Testa se qualquer elemento da sequência atende ao predicado fornecido.|
|exists2|O (mín. (N, M))|-|O (mín. (N, M))|||Testa se qualquer par de elementos correspondentes das sequências de entrada atende ao predicado fornecido.|
|fill|O (N)|||||Define um intervalo de elementos da matriz para o valor especificado.|
|filtro|O (N)|O (N)|O (N)|O (N)|O (N)|Retorna uma nova coleção que contém somente os elementos da coleção para a qual o predicado fornecido retorna `true` .|
|localizar|O (N)|O (N)|O (N)|O (log (N))|-|Retorna o primeiro elemento para o qual a função fornecida retorna `true` . Retorna `System.Collections.Generic.KeyNotFoundException` se nenhum elemento desse tipo existir.|
|findIndex|O (N)|O (N)|O (N)|-|-|Retorna o índice do primeiro elemento na matriz que satisfaz o predicado fornecido. Gera `System.Collections.Generic.KeyNotFoundException` se nenhum elemento satisfizer o predicado.|
|findKey|-|-|-|O (log (N))|-|Avalia a função em cada mapeamento na coleção e retorna a chave para o primeiro mapeamento onde a função retorna `true` . Se nenhum elemento desse tipo existir, essa função gerará `System.Collections.Generic.KeyNotFoundException` .|
|dobrar|O (N)|O (N)|O (N)|O (N)|O (N)|Aplica uma função a cada elemento da coleção, segmentando um argumento acumulador por meio da computação. Se a função de entrada for f e os elementos forem i0... No, essa função computa f (... (f s i0)...) no.|
|fold2|O (N)|O (N)|-|-|-|Aplica uma função aos elementos correspondentes de duas coleções, segmentando um argumento acumulador por meio da computação. As coleções devem ter tamanhos idênticos. Se a função de entrada for f e os elementos forem i0... iN e J0... jN, essa função computa f (... (f s i0 j0)...) Em jN.|
|foldBack|O (N)|O (N)|-|O (N)|O (N)|Aplica uma função a cada elemento da coleção, segmentando um argumento acumulador por meio da computação. Se a função de entrada for f e os elementos forem i0... No, essa função computa o f i0 (... (f em s)).|
|foldBack2|O (N)|O (N)|-|-|-|Aplica uma função aos elementos correspondentes de duas coleções, segmentando um argumento acumulador por meio da computação. As coleções devem ter tamanhos idênticos. Se a função de entrada for f e os elementos forem i0... iN e J0... jN, essa função computa f i0 j0 (... (f em jN s)).|
|ForAll|O (N)|O (N)|O (N)|O (N)|O (N)|Testa se todos os elementos da coleção atendem ao predicado fornecido.|
|forall2|O (N)|O (N)|O (N)|-|-|Testa se todos os elementos correspondentes da coleção atendem ao determinado predicado fornecido.|
|Get/enésimo|O (1)|O (N)|O (N)|-|-|Retorna um elemento da coleção de acordo com seu índice.|
|head|-|O (1)|O (1)|-|-|Retorna o primeiro elemento da coleção.|
|init|O (N)|O (N)|O (1)|-|-|Cria uma coleção de acordo com a dimensão e uma função de gerador para computar os elementos.|
|initInfinite|-|-|O (1)|-|-|Gera uma sequência que, quando iterada, retorna elementos sucessivos chamando a função fornecida.|
|intersect|-|-|-|-|O (log (N) \* log (M))|Computa a interseção de dois conjuntos.|
|intersectMany|-|-|-|-|O (N1 \* N2...)|Computa a interseção de uma sequência de conjuntos. A sequência não deve estar vazia.|
|isEmpty|O (1)|O (1)|O (1)|O (1)|-|Retorna `true` se a coleção está vazia.|
|isProperSubset|-|-|-|-|O (M \* log (N))|Retorna `true` se todos os elementos do primeiro conjunto estiverem no segundo conjunto e pelo menos um elemento do segundo conjunto não estiver no primeiro conjunto.|
|isProperSuperset|-|-|-|-|O (M \* log (N))|Retorna `true` se todos os elementos do segundo conjunto estiverem no primeiro conjunto e pelo menos um elemento do primeiro conjunto não estiver no segundo conjunto.|
|subconjunto|-|-|-|-|O (M \* log (N))|Retorna `true` se todos os elementos do primeiro conjunto estiverem no segundo conjunto.|
|isSuperset|-|-|-|-|O (M \* log (N))|Retorna `true` se todos os elementos do segundo conjunto estiverem no primeiro conjunto.|
|iter|O (N)|O (N)|O (N)|O (N)|O (N)|Aplica a função fornecida a cada elemento da coleção.|
|iteri|O (N)|O (N)|O (N)|-|-|Aplica a função fornecida a cada elemento da coleção. O inteiro que é passado para a função indica o índice do elemento.|
|iteri2|O (N)|O (N)|-|-|-|Aplica a função fornecida a um par de elementos que são desenhados de índices correspondentes em duas matrizes. O inteiro que é passado para a função indica o índice dos elementos. As duas matrizes devem ter o mesmo comprimento.|
|iter2|O (N)|O (N)|O (N)|-|-|Aplica a função fornecida a um par de elementos que são desenhados de índices correspondentes em duas matrizes. As duas matrizes devem ter o mesmo comprimento.|
|last|O (1)|O (N)|O (N)|-|-|Retorna o último item na coleção aplicável.|
|comprimento|O (1)|O (N)|O (N)|-|-|Retorna o número de elementos na coleção.|
|mapa|O (N)|O (N)|O (1)|-|-|Cria uma coleção cujos elementos são os resultados da aplicação da função determinada em cada elemento da matriz.|
|map2|O (N)|O (N)|O (1)|-|-|Cria uma coleção cujos elementos são os resultados da aplicação da função fornecida aos elementos correspondentes das duas coleções emparelhadas. As duas matrizes de entrada devem ter o mesmo comprimento.|
|map3|-|O (N)|-|-|-|Cria uma coleção cujos elementos são os resultados da aplicação da função fornecida aos elementos correspondentes das três coleções simultaneamente.|
|MAPI|O (N)|O (N)|O (N)|-|-|Cria uma matriz cujos elementos são os resultados da aplicação da função determinada em cada elemento da matriz. O índice de inteiro que é passado para a função indica o índice do elemento que está sendo transformado.|
|mapi2|O (N)|O (N)|-|-|-|Cria uma coleção cujos elementos são os resultados da aplicação da função fornecida aos elementos correspondentes das duas coleções emparelhadas, também passando o índice dos elementos. As duas matrizes de entrada devem ter o mesmo comprimento.|
|max|O (N)|O (N)|O (N)|-|-|Retorna o maior elemento na coleção, comparado usando o operador [Max](https://msdn.microsoft.com/library/9a988328-00e9-467b-8dfa-e7a6990f6cce) .|
|maxBy|O (N)|O (N)|O (N)|-|-|Retorna o maior elemento na coleção, comparado usando [Max](https://msdn.microsoft.com/library/9a988328-00e9-467b-8dfa-e7a6990f6cce) no resultado da função.|
|maxElement|-|-|-|-|O (log (N))|Retorna o elemento maior no conjunto de acordo com a ordenação usada para o conjunto.|
|min|O (N)|O (N)|O (N)|-|-|Retorna o elemento mínimo na coleção, comparada usando o operador [min](https://msdn.microsoft.com/library/adea4fd7-bfad-4834-989c-7878aca81fed) .|
|minBy|O (N)|O (N)|O (N)|-|-|Retorna o elemento mínimo na coleção, comparada usando o operador [min](https://msdn.microsoft.com/library/adea4fd7-bfad-4834-989c-7878aca81fed) no resultado da função.|
|minElement|-|-|-|-|O (log (N))|Retorna o elemento mais baixo no conjunto de acordo com a ordenação usada para o conjunto.|
|ofArray|-|O (N)|O (1)|O (N)|O (N)|Cria uma coleção que contém os mesmos elementos que a matriz especificada.|
|ofList|O (N)|-|O (1)|O (N)|O (N)|Cria uma coleção que contém os mesmos elementos da lista fornecida.|
|ofSeq|O (N)|O (N)|-|O (N)|O (N)|Cria uma coleção que contém os mesmos elementos que a sequência fornecida.|
|emparelha|-|-|O (N)|-|-|Retorna uma sequência de cada elemento na sequência de entrada e seu predecessor, exceto o primeiro elemento, que é retornado somente como o predecessor do segundo elemento.|
|partition|O (N)|O (N)|-|O (N)|O (N)|Divide a coleção em duas coleções. A primeira coleção contém os elementos para os quais o predicado fornecido retorna `true` e a segunda coleção contém os elementos para os quais o predicado específico retorna `false` .|
|permute|O (N)|O (N)|-|-|-|Retorna uma matriz com todos os elementos sem mudo de acordo com a permutação especificada.|
|dique|O (N)|O (N)|O (N)|O (log (N))|-|Aplica a função fornecida a elementos sucessivos, retornando o primeiro resultado em que a função retorna alguns. Se a função nunca retornar algum, `System.Collections.Generic.KeyNotFoundException` será gerado.|
|readonly|-|-|O (N)|-|-|Cria um objeto de sequência que delega para o objeto de sequência fornecido. Essa operação garante que uma conversão de tipo não possa redescobrir e mutar a sequência original. Por exemplo, se uma matriz for fornecida, a sequência retornada retornará os elementos da matriz, mas não será possível converter o objeto de sequência retornado em uma matriz.|
|reduce|O (N)|O (N)|O (N)|-|-|Aplica uma função a cada elemento da coleção, segmentando um argumento acumulador por meio da computação. Essa função começa aplicando a função aos dois primeiros elementos, passa esse resultado para a função junto com o terceiro elemento e assim por diante. A função retorna o resultado final.|
|reduceBack|O (N)|O (N)|-|-|-|Aplica uma função a cada elemento da coleção, segmentando um argumento acumulador por meio da computação. Se a função de entrada for f e os elementos forem i0... No, essa função computa o f i0 (... (f iN-1 iN)).|
|remove|-|-|-|O (log (N))|O (log (N))|Remove um elemento do domínio do mapa. Nenhuma exceção será gerada se o elemento não estiver presente.|
|replicar|-|O (N)|-|-|-|Cria uma lista de um comprimento especificado com cada elemento definido para o valor especificado.|
|Rev|O (N)|O (N)|-|-|-|Retorna uma nova lista com os elementos na ordem inversa.|
|scanner|O (N)|O (N)|O (N)|-|-|Aplica uma função a cada elemento da coleção, segmentando um argumento acumulador por meio da computação. Essa operação aplica a função ao segundo argumento e ao primeiro elemento da lista. Em seguida, a operação passa esse resultado para a função junto com o segundo elemento e assim por diante. Por fim, a operação retorna a lista de resultados intermediários e o resultado final.|
|scanBack|O (N)|O (N)|-|-|-|Se assemelha à operação foldBack, mas retorna os resultados intermediários e finais.|
|singleton|-|-|O (1)|-|O (1)|Retorna uma sequência que gera apenas um item.|
|set|O (1)|-|-|-|-|Define um elemento de uma matriz para o valor especificado.|
|skip|-|-|O (N)|-|-|Retorna uma sequência que ignora N elementos da sequência subjacente e, em seguida, gera os elementos restantes da sequência.|
|skipWhile|-|-|O (N)|-|-|Retorna uma sequência que, quando iterada, ignora os elementos da sequência subjacente, enquanto o predicado fornecido retorna `true` e, em seguida, gera os elementos restantes da sequência.|
|sort|O (N \* log (n)) média<br /><br />O (N ^ 2) pior caso|O (N \* log (n))|O (N \* log (n))|-|-|Classifica a coleção por valor do elemento. Os elementos são comparados usando [Compare](https://msdn.microsoft.com/library/295e1320-0955-4c3d-ac31-288fa80a658c).|
|sortBy|O (N \* log (n)) média<br /><br />O (N ^ 2) pior caso|O (N \* log (n))|O (N \* log (n))|-|-|Classifica a lista determinada usando as chaves fornecidas pela projeção fornecida. As chaves são comparadas usando [Compare](https://msdn.microsoft.com/library/295e1320-0955-4c3d-ac31-288fa80a658c).|
|sortInPlace|O (N \* log (n)) média<br /><br />O (N ^ 2) pior caso|-|-|-|-|Classifica os elementos de uma matriz, modificando-os no local e usando a função de comparação fornecida. Os elementos são comparados usando [Compare](https://msdn.microsoft.com/library/295e1320-0955-4c3d-ac31-288fa80a658c).|
|sortInPlaceBy|O (N \* log (n)) média<br /><br />O (N ^ 2) pior caso|-|-|-|-|Classifica os elementos de uma matriz, modificando-os no local e usando a projeção fornecida para as chaves. Os elementos são comparados usando [Compare](https://msdn.microsoft.com/library/295e1320-0955-4c3d-ac31-288fa80a658c).|
|sortInPlaceWith|O (N \* log (n)) média<br /><br />O (N ^ 2) pior caso|-|-|-|-|Classifica os elementos de uma matriz, modificando-os no local e usando a função de comparação fornecida como a ordem.|
|sortWith|O (N \* log (n)) média<br /><br />O (N ^ 2) pior caso|O (N \* log (n))|-|-|-|Classifica os elementos de uma coleção, usando a função de comparação fornecida como a ordem e retornando uma nova coleção.|
|sub|O (N)|-|-|-|-|Cria uma matriz que contém o subintervalo fornecido, que é especificado pelo índice inicial e pelo comprimento.|
|Sum|O (N)|O (N)|O (N)|-|-|Retorna a soma dos elementos na coleção.|
|sumBy|O (N)|O (N)|O (N)|-|-|Retorna a soma dos resultados gerados pela aplicação da função a cada elemento da coleção.|
|engloba|-|O (1)|-|-|-|Retorna a lista sem seu primeiro elemento.|
|take|-|-|O (N)|-|-|Retorna os elementos da sequência até uma contagem especificada.|
|takeWhile|-|-|O (1)|-|-|Retorna uma sequência que, quando iterada, gera elementos da sequência subjacente, enquanto o predicado fornecido retorna `true` e, em seguida, não retorna mais nenhum elemento.|
|toArray|-|O (N)|O (N)|O (N)|O (N)|Cria uma matriz a partir da coleção fornecida.|
|toList|O (N)|-|O (N)|O (N)|O (N)|Cria uma lista a partir da coleção fornecida.|
|toSeq|O (1)|O (1)|-|O (1)|O (1)|Cria uma sequência a partir da coleção fornecida.|
|truncate|-|-|O (1)|-|-|Retorna uma sequência que, quando enumerada, não retorna mais de N elementos.|
|tryFind|O (N)|O (N)|O (N)|O (log (N))|-|Procura um elemento que satisfaça um determinado predicado.|
|tryFindIndex|O (N)|O (N)|O (N)|-|-|Procura o primeiro elemento que satisfaça um determinado predicado e retorna o índice do elemento correspondente, ou `None` se nenhum elemento desse tipo existir.|
|tryFindKey|-|-|-|O (log (N))|-|Retorna a chave do primeiro mapeamento na coleção que satisfaz o predicado fornecido ou retorna `None` se nenhum elemento desse tipo existe.|
|tryPick|O (N)|O (N)|O (N)|O (log (N))|-|Aplica a função fornecida a elementos sucessivos, retornando o primeiro resultado em que a função retorna `Some` para algum valor. Se nenhum elemento desse tipo existir, a operação retornará `None` .|
|desdobrar|-|-|O (N)|-|-|Retorna uma sequência que contém os elementos que o cálculo fornecido gera.|
|union|-|-|-|-|O (M \* log (N))|Computa a União dos dois conjuntos.|
|unionMany|-|-|-|-|O (N1 \* N2...)|Computa a União de uma sequência de conjuntos.|
|unzip|O (N)|O (N)|O (N)|-|-|Divide uma lista de pares em duas listas.|
|unzip3|O (N)|O (N)|O (N)|-|-|Divide uma lista de percorridas em três listas.|
|em janelas|-|-|O (N)|-|-|Retorna uma sequência que produz janelas deslizantes contendo elementos que são desenhados da sequência de entrada. Cada janela é retornada como uma matriz nova.|
|zip|O (N)|O (N)|O (N)|-|-|Combina as duas coleções em uma lista de pares. As duas listas devem ter comprimentos iguais.|
|zip3|O (N)|O (N)|O (N)|-|-|Combina as três coleções em uma lista de corridas. As listas devem ter comprimentos iguais.|

## <a name="see-also"></a>Confira também

- [Tipos F#](fsharp-types.md)
- [Referência de linguagem F #](index.md)
