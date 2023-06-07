---
title: Métodos de extensão – Guia de Programação em C#
description: Os métodos de extensão no C# permitem que você adicione métodos a tipos existentes sem criar um novo tipo derivado, recompilar ou modificar o tipo original.
ms.date: 03/19/2020
helpviewer_keywords:
- methods [C#], adding to existing types
- extension methods [C#]
- methods [C#], extension
ms.assetid: 175ce3ff-9bbf-4e64-8421-faeb81a0bb51
ms.openlocfilehash: 116087ac1aab57f2869b05f436801c7861c56eca
ms.sourcegitcommit: 7476c20d2f911a834a00b8a7f5e8926bae6804d9
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/11/2020
ms.locfileid: "88063699"
---
# <a name="extension-methods-c-programming-guide"></a>Métodos de extensão (Guia de Programação em C#)

Os métodos de extensão permitem que você "adicione" tipos existentes sem criar um novo tipo derivado, recompilar ou, caso contrário, modificar o tipo original. Os métodos de extensão são métodos estáticos, mas são chamados como se fossem métodos de instância no tipo estendido. Para o código de cliente escrito em C#, F # e Visual Basic, não há nenhuma diferença aparente entre chamar um método de extensão e os métodos definidos em um tipo.

Os métodos de extensão mais comuns são os operadores de consulta padrão do LINQ que adicionam a funcionalidade de consulta aos <xref:System.Collections.IEnumerable?displayProperty=nameWithType> tipos existentes e <xref:System.Collections.Generic.IEnumerable%601?displayProperty=nameWithType> . Para usar os operadores de consulta padrão, traga-os primeiro ao escopo com uma diretiva `using System.Linq`. Em seguida, qualquer tipo que implemente <xref:System.Collections.Generic.IEnumerable%601> parece ter métodos de instância como <xref:System.Linq.Enumerable.GroupBy%2A>, <xref:System.Linq.Enumerable.OrderBy%2A>, <xref:System.Linq.Enumerable.Average%2A> e assim por diante. Você pode exibir esses métodos adicionais no preenchimento de declaração do IntelliSense ao digitar "ponto" após uma instância de um tipo <xref:System.Collections.Generic.IEnumerable%601> como <xref:System.Collections.Generic.List%601> ou <xref:System.Array>.

### <a name="orderby-example"></a>Exemplo de OrderBy

O exemplo a seguir mostra como chamar o método de consulta padrão `OrderBy` em qualquer matriz de inteiros. A expressão entre parênteses é uma expressão lambda. Muitos operadores de consulta padrão usam expressões lambda como parâmetros, mas isso não é um requisito para métodos de extensão. Para obter mais informações, consulte [Expressões Lambda](../../language-reference/operators/lambda-expressions.md).

[!code-csharp[csProgGuideExtensionMethods#3](~/samples/snippets/csharp/VS_Snippets_VBCSharp/csProgGuideExtensionMethods/cs/extensionmethods.cs#3)]

Os métodos de extensão são definidos como estáticos, mas são chamados usando a sintaxe do método de instância. Seu primeiro parâmetro especifica em qual tipo o método opera. O parâmetro é precedido por [este](../../language-reference/keywords/this.md) modificador. Os métodos de extensão só estarão no escopo quando você importar explicitamente o namespace para seu código-fonte com uma diretiva `using`.

O exemplo a seguir mostra um método de extensão definido para a classe <xref:System.String?displayProperty=nameWithType>. Ele é definido dentro de uma classe estática não aninhada não genérica:

[!code-csharp[csProgGuideExtensionMethods#4](~/samples/snippets/csharp/VS_Snippets_VBCSharp/csProgGuideExtensionMethods/cs/extensionmethods.cs#4)]

O método de extensão `WordCount` pode ser colocado no escopo com esta diretiva `using`:

```csharp
using ExtensionMethods;
```

E pode ser chamado a partir de um aplicativo usando esta sintaxe:

```csharp
string s = "Hello Extension Methods";
int i = s.WordCount();
```

Você invoca o método de extensão em seu código com a sintaxe do método de instância. A IL (linguagem intermediária) gerada pelo compilador converte seu código em uma chamada no método estático. O princípio do encapsulamento não está realmente violado. Os métodos de extensão não podem acessar variáveis privadas no tipo que estão sendo estendidas.

Para obter mais informações, consulte [como implementar e chamar um método de extensão personalizado](./how-to-implement-and-call-a-custom-extension-method.md).

Em geral, você provavelmente estará chamando métodos de extensão muito mais frequentemente do que implementar seus próprios. Como os métodos de extensão são chamados com a sintaxe do método de instância, nenhum conhecimento especial é necessário para usá-los no código do cliente. Para habilitar métodos de extensão para um tipo específico, apenas adicione uma diretiva `using` para o namespace no qual os métodos estão definidos. Por exemplo, para usar os operadores de consulta padrão, adicione esta diretiva `using` ao seu código:

```csharp
using System.Linq;
```

(Você também pode precisar adicionar uma referência a System.Core.dll.) Você observará que os operadores de consulta padrão agora aparecem no IntelliSense como métodos adicionais disponíveis para a maioria dos <xref:System.Collections.Generic.IEnumerable%601> tipos.

## <a name="binding-extension-methods-at-compile-time"></a>Associando Métodos de Extensão no Momento da Compilação

Você pode usar métodos de extensão para estender uma classe ou interface, mas não os substituir. Um método de extensão com o mesmo nome e assinatura que um método de interface ou classe nunca será chamado. No tempo de compilação, os métodos de extensão sempre têm menos prioridade que os métodos de instância definidos no próprio tipo. Em outras palavras, se um tipo possuir um método chamado `Process(int i)` e se você tiver um método de extensão com a mesma assinatura, o compilador sempre se associará ao método de instância. Quando o compilador encontra uma invocação de método, primeiro ele procura uma correspondência nos métodos de instância do tipo. Se nenhuma correspondência for encontrada, ele irá procurar todos os métodos de extensão definidos para o tipo e associará o primeiro método de extensão que encontrar. O exemplo a seguir demonstra como o compilador determina a qual método de extensão ou método de instância associar.

## <a name="example"></a>Exemplo

O exemplo a seguir demonstra as regras que o compilador C# segue ao determinar se deve associar uma chamada de método a um método de instância no tipo ou a um método de extensão. A classe estática `Extensions` contém métodos de extensão definidos para qualquer tipo que implementa `IMyInterface`. As classes `A`, `B` e `C` implementam a interface.

O método de extensão `MethodB` nunca é chamado porque seu nome e assinatura são exatamente iguais aos métodos já implementados pelas classes.

Quando o compilador não consegue localizar um método de instância com uma assinatura correspondente, ele se associará a um método de extensão correspondente, se houver.

[!code-csharp[csProgGuideExtensionMethods#5](~/samples/snippets/csharp/VS_Snippets_VBCSharp/csProgGuideExtensionMethods/cs/extensionmethods.cs#5)]

## <a name="common-usage-patterns"></a>Padrões de uso comuns

### <a name="collection-functionality"></a>Funcionalidade de coleção

No passado, era comum criar "classes de coleção" que implementavam a <xref:System.Collections.Generic.IEnumerable%601?displayProperty=nameWithType> interface para um determinado tipo e uma funcionalidade contida que atuava em coleções desse tipo. Embora não haja nada de errado ao criar esse tipo de objeto de coleção, a mesma funcionalidade pode ser obtida usando uma extensão no <xref:System.Collections.Generic.IEnumerable%601?displayProperty=nameWithType> . As extensões têm a vantagem de permitir que a funcionalidade seja chamada de qualquer coleção, como uma <xref:System.Array?displayProperty=nameWithType> ou <xref:System.Collections.Generic.List%601?displayProperty=nameWithType> implementada <xref:System.Collections.Generic.IEnumerable%601?displayProperty=nameWithType> nesse tipo. Um exemplo disso é usar uma matriz de Int32, que pode ser encontrada [anteriormente neste artigo](#orderby-example).

### <a name="layer-specific-functionality"></a>Funcionalidade específica de camada

Ao usar uma arquitetura de cebola ou outro design de aplicativo em camadas, é comum ter um conjunto de entidades de domínio ou Transferência de Dados objetos que podem ser usados para se comunicar entre os limites do aplicativo. Esses objetos geralmente não contêm nenhuma funcionalidade ou apenas funcionalidade mínima que se aplica a todas as camadas do aplicativo. Os métodos de extensão podem ser usados para adicionar funcionalidade específica a cada camada de aplicativo sem carregar o objeto com métodos não necessários ou desejados em outras camadas.

```csharp
public class DomainEntity
{
    public int Id { get; set; }
    public string FirstName { get; set; }
    public string LastName { get; set; }
}

static class DomainEntityExtensions
{
    static string FullName(this DomainEntity value)
        => $"{value.FirstName} {value.LastName}";
}
```

### <a name="extending-predefined-types"></a>Estendendo tipos predefinidos

Em vez de criar novos objetos quando a funcionalidade reutilizável precisa ser criada, muitas vezes podemos estender um tipo existente, como um tipo .NET ou CLR. Por exemplo, se não usarmos métodos de extensão, podemos criar uma `Engine` classe or `Query` para fazer o trabalho de executar uma consulta em um SQL Server que pode ser chamado de vários locais em nosso código. No entanto, podemos estender a <xref:System.Data.SqlClient.SqlConnection?displayProperty=nameWithType> classe usando métodos de extensão para executar essa consulta em qualquer lugar em que tenhamos uma conexão com um SQL Server. Outros exemplos podem ser adicionar funcionalidade comum à <xref:System.String?displayProperty=nameWithType> classe, estender os recursos de processamento de dados dos <xref:System.IO.File?displayProperty=nameWithType> objetos e e <xref:System.IO.Stream?displayProperty=nameWithType> <xref:System.Exception?displayProperty=nameWithType> objetos para a funcionalidade de tratamento de erros específica. Esses tipos de casos de uso são limitados apenas por sua imaginação e bom sentido.

A extensão de tipos predefinidos pode ser difícil com `struct` tipos porque eles são passados por valor para métodos. Isso significa que qualquer alteração na estrutura é feita em uma cópia da estrutura. Essas alterações não serão visíveis depois que o método de extensão sair. A partir do C# 7,2, você pode adicionar o `ref` modificador ao primeiro argumento de um método de extensão. Adicionar o `ref` modificador significa que o primeiro argumento é passado por referência. Isso permite que você escreva métodos de extensão que alteram o estado da estrutura que está sendo estendida.

## <a name="general-guidelines"></a>Diretrizes gerais

Embora ainda seja considerado preferível adicionar funcionalidade modificando o código de um objeto ou derivando um novo tipo sempre que for razoável e possível fazer isso, os métodos de extensão se tornaram uma opção crucial para a criação de funcionalidade reutilizável em todo o ecossistema do .NET. Para as ocasiões em que a fonte original não está sob seu controle, quando um objeto derivado é inadequado ou impossível, ou quando a funcionalidade não deve ser exposta além do escopo aplicável, os métodos de extensão são uma opção excelente.

Para obter mais informações sobre tipos derivados, consulte [herança](./inheritance.md).

Ao usar um método de extensão para estender um tipo cujo código-fonte você não está controlando, você corre o risco de que uma alteração na implementação do tipo cause a interrupção do método de extensão.

Se você implementar métodos de extensão para um determinado tipo, lembre-se das seguintes considerações:

- Um método de extensão nunca será chamado se possuir a mesma assinatura que um método definido no tipo.
- Os métodos de extensão são trazidos para o escopo no nível do namespace. Por exemplo, se você tiver várias classes estáticas que contêm métodos de extensão em um único namespace chamado `Extensions` , elas serão colocadas no escopo pela `using Extensions;` diretiva.

Para uma biblioteca de classes que você implemente, não use métodos de extensão para evitar incrementar o número de versão de um assembly. Se você quiser adicionar uma funcionalidade significativa a uma biblioteca para a qual você possui o código-fonte, siga as diretrizes do .NET para o controle de versão do assembly. Para obter mais informações, consulte [Controle de versão do assembly](../../../standard/assembly/versioning.md).

## <a name="see-also"></a>Consulte também

- [Guia de programação C#](../index.md)
- [Exemplos de programação paralela (incluem vários métodos de extensão de exemplo)](/samples/browse/?products=dotnet-core%2Cdotnet-standard&term=parallel)
- [Expressões lambda](../../language-reference/operators/lambda-expressions.md)
- [Visão geral de operadores de consulta padrão](../concepts/linq/standard-query-operators-overview.md)
- [Regras de conversão para parâmetros de instância e seu impacto](https://docs.microsoft.com/archive/blogs/sreekarc/conversion-rules-for-instance-parameters-and-their-impact)
- [Interoperabilidade de métodos de extensão entre linguagens](https://docs.microsoft.com/archive/blogs/sreekarc/extension-methods-interoperability-between-languages)
- [Métodos de extensão e representantes via currying](https://docs.microsoft.com/archive/blogs/sreekarc/extension-methods-and-curried-delegates)
- [Associação do método de extensão e relatório de erros](https://docs.microsoft.com/archive/blogs/sreekarc/extension-method-binding-and-error-reporting)
