---
title: Design de parâmetro
ms.date: 10/22/2008
ms.technology: dotnet-standard
helpviewer_keywords:
- member design guidelines [.NET Framework], parameters
- members [.NET Framework], parameters
- names [.NET Framework], parameters
- parameters, design guidelines
- reserved parameters
ms.assetid: 3f33bf46-4a7b-43b3-bb78-1ffebe0dcfa6
ms.openlocfilehash: e0bc52f5679a7771d5690be9f903e677ce611605
ms.sourcegitcommit: e02d17b2cf9c1258dadda4810a5e6072a0089aee
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/01/2020
ms.locfileid: "85621581"
---
# <a name="parameter-design"></a>Design de parâmetro

Esta seção fornece diretrizes amplas sobre o design de parâmetros, incluindo seções com diretrizes para verificar argumentos. Além disso, você deve consultar as diretrizes descritas em [parâmetros de nomenclatura](naming-parameters.md).

 ✔️ usar o tipo de parâmetro menos derivado que fornece a funcionalidade exigida pelo membro.

 Por exemplo, suponha que você queira criar um método que enumere uma coleção e imprima cada item no console. Esse método deve ser adotado <xref:System.Collections.IEnumerable> como o parâmetro, não <xref:System.Collections.ArrayList> ou <xref:System.Collections.IList> , por exemplo.

 ❌Não use parâmetros reservados.

 Se mais entradas para um membro forem necessárias em alguma versão futura, uma nova sobrecarga poderá ser adicionada.

 ❌Não têm métodos expostos publicamente que usam ponteiros, matrizes de ponteiros ou matrizes multidimensionais como parâmetros.

 Ponteiros e matrizes multidimensionais são relativamente difíceis de usar corretamente. Em quase todos os casos, as APIs podem ser reprojetadas para evitar a criação desses tipos como parâmetros.

 ✔️ FAZER todos os `out` parâmetros após todos os parâmetros por valor e `ref` (excluindo matrizes de parâmetro), mesmo que resulte em uma inconsistência na ordenação de parâmetros entre sobrecargas (consulte [sobrecarga de membros](member-overloading.md)).

 Os `out` parâmetros podem ser vistos como valores de retorno extras e agrupá-los juntos torna a assinatura do método mais fácil de entender.

 ✔️ são consistentes na nomenclatura de parâmetros ao substituir membros ou implementar membros de interface.

 Isso comunica melhor a relação entre os métodos.

### <a name="choosing-between-enum-and-boolean-parameters"></a>Escolhendo entre parâmetros de enumeração e boolianos  
 ✔️ usar enums se um membro, caso contrário, teria dois ou mais parâmetros boolianos.

 ❌Não use boolianos, a menos que esteja absolutamente certo de que nunca haverá necessidade de mais de dois valores.

 As enumerações oferecem algum espaço para a adição futura de valores, mas você deve estar ciente de todas as implicações da adição de valores a enums, que são descritas em [design de enumeração](enum.md).

 ✔️ Considere usar boolianos para parâmetros de construtor que são verdadeiramente valores de dois Estados e são simplesmente usados para inicializar propriedades booleanas.

### <a name="validating-arguments"></a>Validando argumentos
 ✔️ validar argumentos passados para membros públicos, protegidos ou explicitamente implementados. Throw <xref:System.ArgumentException?displayProperty=nameWithType> , ou uma de suas subclasses, se a validação falhar.

 Observe que a validação real não precisa necessariamente acontecer no próprio membro público ou protegido. Isso pode acontecer em um nível inferior em alguma rotina privada ou interna. O ponto principal é que toda a área de superfície exposta aos usuários finais verifica os argumentos.

 ✔️ gerar <xref:System.ArgumentNullException> se um argumento nulo for passado e o membro não oferecer suporte a argumentos nulos.

 ✔️ validar os parâmetros de enumeração.

 Não assuma que os argumentos de enumeração estarão no intervalo definido pela enumeração. O CLR permite a conversão de qualquer valor inteiro em um valor de enumeração, mesmo se o valor não estiver definido na enumeração.

 ❌Não use <xref:System.Enum.IsDefined%2A?displayProperty=nameWithType> para verificações de intervalo de enumeração.

 ✔️ estar ciente de que argumentos mutáveis podem ter sido alterados depois de serem validados.

 Se o membro for sensível à segurança, você será incentivado a fazer uma cópia e, em seguida, validar e processar o argumento.

### <a name="parameter-passing"></a>Passagem de parâmetro
 Da perspectiva de um designer de estrutura, há três grupos principais de parâmetros: parâmetros por valor, `ref` parâmetros e `out` parâmetros.

 Quando um argumento é passado por um parâmetro por valor, o membro recebe uma cópia do argumento real passado. Se o argumento for um tipo de valor, uma cópia do argumento será colocada na pilha. Se o argumento for um tipo de referência, uma cópia da referência será colocada na pilha. As linguagens CLR mais populares, como C#, VB.NET e C++, assumem como padrão a passagem de parâmetros por valor.

 Quando um argumento é passado por um `ref` parâmetro, o membro recebe uma referência para o argumento real passado. Se o argumento for um tipo de valor, uma referência ao argumento será colocada na pilha. Se o argumento for um tipo de referência, uma referência à referência será colocada na pilha. `Ref`os parâmetros podem ser usados para permitir que o membro modifique os argumentos passados pelo chamador.

 `Out`os parâmetros são semelhantes aos `ref` parâmetros, com algumas pequenas diferenças. O parâmetro é inicialmente considerado não atribuído e não pode ser lido no corpo do membro antes de ser atribuído a um valor. Além disso, o parâmetro deve ser atribuído a um valor antes que o membro seja retornado.

 ❌Evite usar `out` `ref` parâmetros ou.

 O uso de `out` `ref` parâmetros ou requer experiência com ponteiros, compreendendo como tipos de valor e tipos de referência diferem e manipulando métodos com vários valores de retorno. Além disso, a diferença entre os `out` `ref` parâmetros e não é amplamente compreendida. Os arquitetos de estrutura que projetam para um público geral não devem esperar que os usuários façam o mestre de trabalho com os `out` `ref` parâmetros ou.

 ❌Não passe tipos de referência por referência.

 Há algumas exceções limitadas à regra, como um método que pode ser usado para alternar referências.

### <a name="members-with-variable-number-of-parameters"></a>Membros com número variável de parâmetros
 Os membros que podem obter um número variável de argumentos são expressos fornecendo um parâmetro de matriz. Por exemplo, <xref:System.String> o fornece o seguinte método:

```csharp
public class String {
    public static string Format(string format, object[] parameters);
}
```

 Um usuário pode então chamar o <xref:System.String.Format%2A?displayProperty=nameWithType> método, da seguinte maneira:

 `String.Format("File {0} not found in {1}",new object[]{filename,directory});`

 Adicionar a palavra-chave Parameters do C# a um parâmetro de matriz altera o parâmetro para o chamado parâmetro de matriz params e fornece um atalho para a criação de uma matriz temporária.

```csharp
public class String {
    public static string Format(string format, params object[] parameters);
}
```

 Fazer isso permite que o usuário chame o método passando os elementos da matriz diretamente na lista de argumentos.

 `String.Format("File {0} not found in {1}",filename,directory);`

 Observe que a palavra-chave params pode ser adicionada somente ao último parâmetro na lista de parâmetros.

 ✔️ Considere adicionar a palavra-chave params aos parâmetros de matriz se você espera que os usuários finais passem matrizes com um pequeno número de elementos. Se for esperado que muitos elementos sejam passados em cenários comuns, os usuários provavelmente não passarão esses elementos embutidos de qualquer forma e, portanto, a palavra-chave params não será necessária.

 ❌Evite usar matrizes params se o chamador quase sempre tiver a entrada já em uma matriz.

 Por exemplo, os membros com parâmetros de matriz de bytes quase nunca seriam chamados passando bytes individuais. Por esse motivo, os parâmetros de matriz de bytes no .NET Framework não usam a palavra-chave params.

 ❌Não use matrizes params se a matriz for modificada pelo membro usando o parâmetro de matriz params.

 Por causa do fato de que muitos compiladores transformam os argumentos para o membro em uma matriz temporária no site de chamada, a matriz pode ser um objeto temporário e, portanto, qualquer modificação na matriz será perdida.

 ✔️ Considere usar a palavra-chave params em uma sobrecarga simples, mesmo se uma sobrecarga mais complexa não puder usá-la.

 Pergunte-se se os usuários teriam valor com a matriz params em uma sobrecarga, mesmo que não estivesse em todas as sobrecargas.

 ✔️ Tente ordenar os parâmetros para tornar possível usar a palavra-chave params.

 ✔️ Considere fornecer sobrecargas especiais e caminhos de código para chamadas com um pequeno número de argumentos em APIs extremamente sensíveis ao desempenho.

 Isso possibilita evitar a criação de objetos de matriz quando a API é chamada com um pequeno número de argumentos. Formate os nomes dos parâmetros por meio de uma forma singular do parâmetro de matriz e adicionando um sufixo numérico.

 Você só deve fazer isso se for para o caso especial do caminho de código inteiro, não apenas criar uma matriz e chamar o método mais geral.

 ✔️ estar ciente de que NULL pode ser passado como um argumento de matriz params.

 Você deve validar que a matriz não é nula antes do processamento.

 ❌Não use os `varargs` métodos, caso contrário, conhecidos como reticências.

 Algumas linguagens CLR, como C++, dão suporte a uma convenção alternativa para passar listas de parâmetros de variáveis chamados `varargs` métodos. A Convenção não deve ser usada em estruturas, pois não é compatível com CLS.

### <a name="pointer-parameters"></a>Parâmetros do ponteiro
 Em geral, os ponteiros não devem aparecer na área de superfície pública de uma estrutura de código gerenciada bem projetada. Na maioria das vezes, os ponteiros devem ser encapsulados. No entanto, em alguns casos, os ponteiros são necessários para motivos de interoperabilidade e o uso de ponteiros nesses casos é apropriado.

 ✔️ fornecem uma alternativa para qualquer membro que usa um argumento de ponteiro, porque os ponteiros não são compatíveis com CLS.

 ❌Evite fazer uma verificação cara de argumento dos argumentos de ponteiro.

 ✔️ seguir Convenções comuns relacionadas a ponteiros ao criar membros com ponteiros.

 Por exemplo, não há necessidade de passar o índice inicial, porque a aritmética simples de ponteiro pode ser usada para atingir o mesmo resultado.

 *Partes &copy; 2005, 2009 Microsoft Corporation. Todos os direitos reservados.*

 *Reimpresso com permissão da Pearson Education, Inc. das [Diretrizes de Design do Framework: convenções, linguagens e padrões para bibliotecas do .NET reutilizável, 2ª edição](https://www.informit.com/store/framework-design-guidelines-conventions-idioms-and-9780321545619) por Krzysztof Cwalina e Brad Abrams, publicado em 22 de outubro de 2008 por Addison-Wesley Professional como parte da série de desenvolvimento do Microsoft Windows.*

## <a name="see-also"></a>Consulte também

- [Diretrizes de design de membro](member.md)
- [Diretrizes de design de estrutura](index.md)
