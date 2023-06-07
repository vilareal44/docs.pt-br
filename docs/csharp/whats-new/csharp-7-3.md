---
title: Novidades no C# 7.3
description: Uma visão geral dos novos recursos no C# 7.3
ms.date: 05/16/2018
ms.openlocfilehash: cd8f554516fb5078d9d2ed1eec787f36e8f4c7a7
ms.sourcegitcommit: cb27c01a8b0b4630148374638aff4e2221f90b22
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/09/2020
ms.locfileid: "86174751"
---
# <a name="whats-new-in-c-73"></a>Novidades no C# 7.3

Há dois temas principais para a versão C# 7.3. Um tema fornece recursos que permitem que o código seguro tenha o mesmo desempenho que o código não seguro. O segundo tema fornece melhorias incrementais aos recursos existentes. Além disso, novas opções do compilador foram adicionadas nesta versão.

Os novos recursos a seguir são compatíveis com o tema de melhor desempenho para código seguro:

- Você pode acessar campos fixos sem fixação.
- Você pode reatribuir variáveis locais `ref`.
- Você pode usar inicializadores em matrizes `stackalloc`.
- Você pode usar instruções `fixed` com qualquer tipo compatível com um padrão.
- Você pode usar restrições genéricas adicionais.

Os seguintes recursos e aprimoramentos foram feitos nos recursos existentes:

- Você pode testar `==` e `!=` com tipos de tupla.
- Você pode usar variáveis de expressão em mais locais.
- Você pode anexar atributos ao campo de suporte de propriedades autoimplementadas.
- A resolução de métodos quando os argumentos se diferenciam por `in` foi aprimorada.
- A resolução de sobrecarga agora tem menos casos ambíguos.

As novas opções do compilador são:

- `-publicsign` para habilitar a assinatura de Software de código aberto (OSS) de assemblies.
- `-pathmap` para fornecer um mapeamento para diretórios de origem.

O restante deste artigo fornece detalhes e links para saber mais sobre cada uma das melhorias. Você pode explorar esses recursos em seu ambiente usando a ferramenta global `dotnet try`:

1. Instale a ferramenta global [dotnet-try](https://github.com/dotnet/try/blob/master/README.md#setup).
1. Clone o repositório [dotnet/try-samples](https://github.com/dotnet/try-samples).
1. Definir o diretório atual do subdiretório *csharp7* para o repositório *try-samples*.
1. Execute `dotnet try`.

## <a name="enabling-more-efficient-safe-code"></a>Como habilitar código seguro mais eficiente

Você deve ser capaz de escrever um código C# de forma segurança que seja executado tão bem quanto o código não seguro. O código seguro evita classes de erros, como estouros de buffer, ponteiros perdidos e outros erros de acesso à memória. Esses novos recursos expandem as capacidades do código seguro verificável. Tente escrever mais partes do seu código usando construções seguras. Esses recursos tornam isso mais fácil.

### <a name="indexing-fixed-fields-does-not-require-pinning"></a>A indexação de campos `fixed` não requer fixação

Considere este struct:

```csharp
unsafe struct S
{
    public fixed int myFixedField[10];
}
```

Em versões anteriores do C#, era preciso fixar uma variável para acessar um dos números inteiros que fazem parte de `myFixedField`. Agora, o código a seguir compila sem fixar a variável `p` dentro de uma instrução `fixed` separada:

```csharp
class C
{
    static S s = new S();

    unsafe public void M()
    {
        int p = s.myFixedField[5];
    }
}
```

A variável `p` acessa um elemento em `myFixedField`. Não é necessário declarar uma variável `int*` separada. Você ainda pode precisar de um contexto `unsafe`. Em versões anteriores do C#, é necessário declarar um segundo ponteiro fixo:

```csharp
class C
{
    static S s = new S();

    unsafe public void M()
    {
        fixed (int* ptr = s.myFixedField)
        {
            int p = ptr[5];
        }
    }
}
```

Para obter mais informações, consulte o artigo na [ `fixed` instrução](../language-reference/keywords/fixed-statement.md).

### <a name="ref-local-variables-may-be-reassigned"></a>É possível reatribuir variáveis locais `ref`

Agora, as variáveis locais `ref` podem ser reatribuídas para se referir a diferentes instâncias depois de serem inicializadas. O comando a seguir agora compila:

```csharp
ref VeryLargeStruct refLocal = ref veryLargeStruct; // initialization
refLocal = ref anotherVeryLargeStruct; // reassigned, refLocal refers to different storage.
```

Para obter mais informações, consulte o artigo sobre [ `ref` retornos e `ref` locais](../programming-guide/classes-and-structs/ref-returns.md)e o artigo sobre [`foreach`](../language-reference/keywords/foreach-in.md) .

### <a name="stackalloc-arrays-support-initializers"></a>Matrizes `stackalloc` são compatíveis com inicializadores

Você conseguiu especificar os valores dos elementos em uma matriz ao inicializá-la:

```csharp
var arr = new int[3] {1, 2, 3};
var arr2 = new int[] {1, 2, 3};
```

Agora, essa mesma sintaxe pode ser aplicada a matrizes declaradas com `stackalloc`:

```csharp
int* pArr = stackalloc int[3] {1, 2, 3};
int* pArr2 = stackalloc int[] {1, 2, 3};
Span<int> arr = stackalloc [] {1, 2, 3};
```

Para obter mais informações, consulte o artigo do [ `stackalloc` operador](../language-reference/operators/stackalloc.md) .

### <a name="more-types-support-the-fixed-statement"></a>Mais tipos são compatíveis com a instrução `fixed`

A instrução `fixed` era compatível com um conjunto limitado de tipos. A partir do C# 7.3, qualquer tipo que contenha um método `GetPinnableReference()` que retorna `ref T` ou `ref readonly T` pode ser `fixed`. A adição desse recurso significa que `fixed` pode ser usado com <xref:System.Span%601?displayProperty=nameWithType> e tipos relacionados.

Para obter mais informações, consulte o artigo da [ `fixed` instrução](../language-reference/keywords/fixed-statement.md) na referência de linguagem.

### <a name="enhanced-generic-constraints"></a>Restrições genéricas aprimoradas

Agora é possível especificar o tipo <xref:System.Enum?displayProperty=nameWithType> ou <xref:System.Delegate?displayProperty=nameWithType> como restrições de classe base para um parâmetro de tipo.

Você também pode usar a nova `unmanaged` restrição para especificar que um parâmetro de tipo deve ser um tipo não- [gerenciado](../language-reference/builtin-types/unmanaged-types.md)não anulável.

Para obter mais informações, consulte os artigos sobre [ `where` restrições genéricas](../language-reference/keywords/where-generic-type-constraint.md) e [restrições em parâmetros de tipo](../programming-guide/generics/constraints-on-type-parameters.md).

Adicionar essas restrições a tipos existentes é uma [alteração incompatível](version-update-considerations.md#incompatible-changes). Tipos genéricos fechados podem não atender mais a essas novas restrições.

## <a name="make-existing-features-better"></a>Melhorias nos recursos existentes

O segundo tema fornece melhorias aos recursos da linguagem. Esses recursos melhoram a produtividade ao escrever em C#.

### <a name="tuples-support--and-"></a>As tuplas são compatíveis com `==` e `!=`

Os tipos de tupla de C# agora são compatíveis com `==` e `!=`. Para obter mais informações, consulte a seção de [igualdade de tupla](../language-reference/builtin-types/value-tuples.md#tuple-equality) do artigo [tipos de tupla](../language-reference/builtin-types/value-tuples.md) .

### <a name="attach-attributes-to-the-backing-fields-for-auto-implemented-properties"></a>Anexar atributos aos campos de suporte de propriedades autoimplementadas

Agora há suporte para esta sintaxe:

```csharp
[field: SomeThingAboutFieldAttribute]
public int SomeProperty { get; set; }
```

O atributo `SomeThingAboutFieldAttribute` é aplicado ao campo de suporte gerado pelo compilador para `SomeProperty`. Para saber mais, confira [atributos](../programming-guide/concepts/attributes/index.md) no guia de programação em C#.

### <a name="in-method-overload-resolution-tiebreaker"></a>Resolução de ambiguidade de sobrecarga com o método `in`

Quando o modificador de argumento `in` era adicionado, estes dois métodos causariam ambiguidade:

```csharp
static void M(S arg);
static void M(in S arg);
```

Agora, a sobrecarga por valor (primeira no exemplo anterior) é melhor do que a versão da referência somente leitura. Para chamar a versão com o argumento de referência somente leitura, inclua o modificador `in` ao chamar o método.

> [!NOTE]
> Isso foi implementado como uma correção de bug. Não é mais ambíguo nem mesmo com a versão da linguagem definida como "7.2".

Para obter mais informações, consulte o artigo sobre o [ `in` modificador de parâmetro](../language-reference/keywords/in-parameter-modifier.md).

### <a name="extend-expression-variables-in-initializers"></a>Estender variáveis de expressão em inicializadores

A sintaxe adicionada no C# 7.0 para permitir declarações de variável `out` foi estendida para incluir inicializadores de campo, inicializadores de propriedade, inicializadores de construtor e cláusulas de consulta. Ela permite código como no seguinte exemplo:

```csharp
public class B
{
   public B(int i, out int j)
   {
      j = i;
   }
}

public class D : B
{
   public D(int i) : base(i, out var j)
   {
      Console.WriteLine($"The value of 'j' is {j}");
   }
}
```

### <a name="improved-overload-candidates"></a>Candidatos de sobrecarga aprimorados

Em cada versão, as regras de resolução de sobrecarga são atualizadas para resolver situações em que as chamadas de método ambíguas têm uma opção "óbvia". Esta versão adiciona três novas regras para ajudar o compilador a escolher a opção óbvia:

1. Quando um grupo de métodos contém membros de instância e estáticos, o compilador descartará os membros da instância se o método tiver sido chamado sem um receptor ou contexto de instância. O compilador descartará os membros estáticos se o método tiver sido chamado com um receptor de instância. Quando não há receptor, o compilador inclui apenas membros estáticos em um contexto estático, caso contrário, membros estáticos e de instância. Quando o receptor é ambiguamente uma instância ou um tipo, o compilador inclui ambos. Um contexto estático, em que não é possível usar um receptor de instância `this` implícito, inclui o corpo de membros em que nenhum `this` é definido, como membros estáticos, bem como locais onde `this` não pode ser usado, como inicializadores de campo e inicializadores de construtor.
1. Quando um grupo de métodos contém alguns métodos genéricos cujos argumentos de tipo não satisfazem suas restrições, esses membros são removidos do conjunto de candidatos.
1. Para uma conversão de grupo de métodos, os métodos candidatos cujo tipo de retorno não corresponda ao tipo de retorno do delegado são removidos do conjunto.

Você notará essa mudança somente porque encontrará menos erros de compilador para sobrecargas de métodos ambíguos quando tiver certeza de qual método é melhor.

## <a name="new-compiler-options"></a>Novas opções do compilador

Novas opções do compilador são compatíveis com novos cenários de build e DevOps para programas C#.

### <a name="public-or-open-source-signing"></a>Assinatura pública ou de código aberto

A opção de compilador `-publicsign` instrui o compilador a assinar o assembly usando uma chave pública. O assembly é marcado como assinado, mas a assinatura é obtida da chave pública. Essa opção permite criar crie assemblies assinados a partir de projetos de código aberto usando uma chave pública.

Para saber mais, confira o artigo [Opção do compilador -publicsign](../language-reference/compiler-options/publicsign-compiler-option.md).

### <a name="pathmap"></a>pathmap

A opção de compilador `-pathmap` instrui o compilador a substituir os caminhos de origem do ambiente de build com caminhos de origem mapeados. A opção `-pathmap` controla o caminho de origem gravado pelo compilador para arquivos PDB ou para o <xref:System.Runtime.CompilerServices.CallerFilePathAttribute>.

Para saber mais, confira o artigo [Opção do compilador -pathmap](../language-reference/compiler-options/pathmap-compiler-option.md).
