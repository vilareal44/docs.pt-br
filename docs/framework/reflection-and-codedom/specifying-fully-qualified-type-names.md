---
title: Especificando nomes de tipo totalmente qualificados
description: Para obter uma entrada válida para operações de reflexão, use nomes de tipo totalmente qualificado, que têm especificações de nome de assembly, especificações de namespace e nomes de tipo.
ms.date: 02/21/2019
helpviewer_keywords:
- names [.NET Framework], fully qualified type names
- reflection, fully qualified type names
- names [.NET Framework], assemblies
- tokens
- BNF
- assemblies [.NET Framework], names
- languages, grammar
- fully qualified type names
- type names
- special characters
- IDENTIFIER
ms.assetid: d90b1e39-9115-4f2a-81c0-05e7e74e5580
ms.openlocfilehash: ff33b6abd31a82c6b80aa794564c5c48648cde63
ms.sourcegitcommit: 3d84eac0818099c9949035feb96bbe0346358504
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/21/2020
ms.locfileid: "86865223"
---
# <a name="specifying-fully-qualified-type-names"></a>Especificar nomes de tipo totalmente qualificado

Você deve especificar nomes de tipo para ter uma entrada válida para várias operações de reflexão. Um nome de tipo totalmente qualificado consiste em uma especificação de nome de assembly, uma especificação de namespace e um nome de tipo. Especificações de nome de tipo são usadas por métodos como <xref:System.Type.GetType%2A?displayProperty=nameWithType>, <xref:System.Reflection.Module.GetType%2A?displayProperty=nameWithType>, <xref:System.Reflection.Emit.ModuleBuilder.GetType%2A?displayProperty=nameWithType> e <xref:System.Reflection.Assembly.GetType%2A?displayProperty=nameWithType>.

## <a name="grammar-for-type-names"></a>Gramática para nomes de tipo

 A gramática define a sintaxe de linguagens formais. A tabela a seguir lista as regras lexicais que descrevem como reconhecer uma entrada válida. Terminais (elementos que não poder ser mais reduzidos) são mostrados em letras maiúsculas. Não terminais (elementos que ainda podem ser reduzidos) são mostrados em cadeias de caracteres combinando maiúsculas e minúsculas ou entre aspas simples, porém a aspa simples (') não faz parte da sintaxe em si. O caractere de barra vertical (& #124;) indica que as regras que têm sub-regras.

<!-- markdownlint-disable MD010 -->
```antlr
TypeSpec
    : ReferenceTypeSpec
    | SimpleTypeSpec
    ;

ReferenceTypeSpec
    : SimpleTypeSpec '&'
    ;

SimpleTypeSpec
    : PointerTypeSpec
    | GenericTypeSpec
    | TypeName
    ;

GenericTypeSpec
   : SimpleTypeSpec ` NUMBER

PointerTypeSpec
    : SimpleTypeSpec '*'
    ;

ArrayTypeSpec
    : SimpleTypeSpec '[ReflectionDimension]'
    | SimpleTypeSpec '[ReflectionEmitDimension]'
    ;

ReflectionDimension
    : '*'
    | ReflectionDimension ',' ReflectionDimension
    | NOTOKEN
    ;

ReflectionEmitDimension
    : '*'
    | Number '..' Number
    | Number '…'
    | ReflectionDimension ',' ReflectionDimension
    | NOTOKEN
    ;

Number
    : [0-9]+
    ;

TypeName
    : NamespaceTypeName
    | NamespaceTypeName ',' AssemblyNameSpec
    ;

NamespaceTypeName
    : NestedTypeName
    | NamespaceSpec '.' NestedTypeName
    ;

NestedTypeName
    : IDENTIFIER
    | NestedTypeName '+' IDENTIFIER
    ;

NamespaceSpec
    : IDENTIFIER
    | NamespaceSpec '.' IDENTIFIER
    ;

AssemblyNameSpec
    : IDENTIFIER
    | IDENTIFIER ',' AssemblyProperties
    ;

AssemblyProperties
    : AssemblyProperty
    | AssemblyProperties ',' AssemblyProperty
    ;

AssemblyProperty
    : AssemblyPropertyName '=' AssemblyPropertyValue
    ;
```
<!-- markdownlint-enable MD010 -->

## <a name="specifying-special-characters"></a>Especificar caracteres especiais

Em um nome de tipo, IDENTIFIER é qualquer nome válido determinado pelas regras de uma linguagem.

Use a barra invertida (\\) como um caractere de escape para separar os seguintes tokens quando usados como parte do IDENTIFIER.

|Token|Significado|
|-----------|-------------|
|\\,|Separador de assembly.|
|\\+|Separador de tipo aninhado.|
|\\&|Tipo de referência.|
|\\*|Tipo do ponteiro.|
|\\[|Delimitador de dimensão da matriz.|
|\\]|Delimitador de dimensão da matriz.|
|\\.|Use a barra invertida antes de um ponto somente se ele for usado em uma especificação de matriz. Os pontos em NamespaceSpec não usam a barra invertida.|
|\\\|Barra invertida quando for necessária como uma cadeia de caracteres literal.|

Observe que, em todos os componentes de TypeSpec, exceto AssemblyNameSpec, os espaços são relevantes. No AssemblyNameSpec, os espaços antes do separador ',' são relevantes, mas espaços depois do separador ',' são ignorados.

Classes de reflexão, como <xref:System.Type.FullName%2A?displayProperty=nameWithType>, retornam o nome danificado, de forma que o nome retornado pode ser usado em uma chamada para <xref:System.Type.GetType%2A>, como em `MyType.GetType(myType.FullName)`.

Por exemplo, o nome totalmente qualificado para um tipo pode ser `Ozzy.OutBack.Kangaroo+Wallaby,MyAssembly`.

Se o namespace fosse `Ozzy.Out+Back`, o sinal de adição deve ser precedido por uma barra invertida. Caso contrário, o analisador o interpretaria como um separador de aninhamento. A reflexão emite essa cadeia de caracteres como `Ozzy.Out\+Back.Kangaroo+Wallaby,MyAssembly`.

## <a name="specifying-assembly-names"></a>Especificar nomes de assembly

A informação mínima necessária em uma especificação de nome do assembly é o nome textual (IDENTIFIER) do assembly. Você pode seguir o IDENTIFIER de uma lista separada por vírgulas de pares propriedade/valor, conforme descrito na tabela a seguir. A nomenclatura do IDENTIFIER deve seguir as regras de nomenclatura de arquivo. O IDENTIFIER não diferencia maiúsculas de minúsculas.

|Nome da propriedade|Descrição|Valores permitidos|
|-------------------|-----------------|----------------------|
|**Versão**|Número de versão do assembly|*Major.Minor.Build.Revision*, em que *Major*, *Minor*, *Build* e *Revision* são inteiro entre 0 e 65535, inclusive.|
|**PublicKey**|Chave pública completa|O valor da cadeia de caracteres da chave pública completa em formato hexadecimal. Especifique uma referência nula (**Nothing** no Visual Basic) para indicar explicitamente um assembly particular.|
|**PublicKeyToken**|Token de chave pública (hash de 8 bytes da chave pública completa)|Valor da cadeia de caracteres do token de chave pública em formato hexadecimal. Especifique uma referência nula (**Nothing** no Visual Basic) para indicar explicitamente um assembly particular.|
|**Cultura**|Cultura do assembly|A cultura do assembly no formato RFC-1766 ou “neutra” para assemblies independente de linguagem (não satélite).|
|**Custom**|BLOB (objeto binário grande) personalizado. No momento, isso é usado apenas em assemblies gerados pelo [Ngen (Gerador de Imagens Nativas)](../tools/ngen-exe-native-image-generator.md).|A cadeia de caracteres personalizada usada pela ferramenta do Gerador de Imagens Nativas para notificar o cache de assembly que o assembly que está sendo instalado é uma imagem nativa e, portanto, deve ser instalada no cache de imagens nativas. Também chamado de cadeia de caracteres zap.|

A exemplo a seguir mostra um **AssemblyName** para um assembly de nome simples com cultura padrão.

```csharp
com.microsoft.crypto, Culture=""
```

O exemplo a seguir mostra uma referência totalmente especificada para um assembly de nome forte com a cultura “en”.

```csharp
com.microsoft.crypto, Culture=en, PublicKeyToken=a5d015c7d5a0b012,
    Version=1.0.0.0
```

Todos os exemplos a seguir mostram **AssemblyName** parcialmente especificado, que pode ser atendido por um assembly de nome forte ou simples.

```csharp
com.microsoft.crypto
com.microsoft.crypto, Culture=""
com.microsoft.crypto, Culture=en
```

Todos os exemplos a seguir mostram um **AssemblyName** parcialmente especificado, que deve ser atendido por um assembly de nome simples.

```csharp
com.microsoft.crypto, Culture="", PublicKeyToken=null
com.microsoft.crypto, Culture=en, PublicKeyToken=null
```

Todos os exemplos a seguir mostram um **AssemblyName** parcialmente especificado, que deve ser atendido por um assembly de nome forte.

```csharp
com.microsoft.crypto, Culture="", PublicKeyToken=a5d015c7d5a0b012
com.microsoft.crypto, Culture=en, PublicKeyToken=a5d015c7d5a0b012,
    Version=1.0.0.0
```

## <a name="specifying-generic-types"></a>Especificar tipos genéricos

SimpleTypeSpec\`NÚMERO representa um tipo genérico aberto com 1 a *n* parâmetros de tipo genérico. Por exemplo, para obter referência à lista de tipos genéricos abertos \<T> ou à lista de tipos genéricos fechados \<String> , use ``Type.GetType("System.Collections.Generic.List`1")`` para obter uma referência ao dicionário de tipo genérico \<TKey,TValue> , use ``Type.GetType("System.Collections.Generic.Dictionary`2")`` .

## <a name="specifying-pointers"></a>Especificar ponteiros

SimpleTypeSpec* representa um ponteiro não gerenciado. Por exemplo, para obter um ponteiro para o tipo MyType, use `Type.GetType("MyType*")`. Para obter um ponteiro para o tipo MyType, use `Type.GetType("MyType**")`.

## <a name="specifying-references"></a>Especificar referências

SimpleTypeSpec & representa um ponteiro ou referência gerenciado. Por exemplo, para obter uma referência ao tipo MyType, use `Type.GetType("MyType &")`. Observe que, ao contrário dos ponteiros, as referências são limitadas a um nível.

## <a name="specifying-arrays"></a>Especificar matrizes

Na Gramática BNF, ReflectionEmitDimension só se aplica às definições de tipo incompletas recuperadas usando <xref:System.Reflection.Emit.ModuleBuilder.GetType%2A?displayProperty=nameWithType>. As definições de tipo incompletas são objetos <xref:System.Reflection.Emit.TypeBuilder> construídos usando <xref:System.Reflection.Emit?displayProperty=nameWithType>, mas no qual <xref:System.Reflection.Emit.TypeBuilder.CreateType%2A?displayProperty=nameWithType> não foi chamado. É possível usar ReflectionDimension para recuperar qualquer definição de tipo que foi concluída, ou seja, um tipo que foi carregado.

Matrizes são acessadas na reflexão ao especificar a classificação da matriz:

- `Type.GetType("MyArray[]")` obtém uma matriz de dimensão única com o limite inferior 0.

- `Type.GetType("MyArray[*]")` obtém uma matriz de dimensão única com limite inferior desconhecido.
- `Type.GetType("MyArray[][]")` recebe uma matriz bidimensional.

- `Type.GetType("MyArray[*,*]")` e `Type.GetType("MyArray[,]")` obtém uma matriz bidimensional retangular com limites inferiores desconhecidos.

Observe que, do ponto de vista do runtime, `MyArray[] != MyArray[*]`, mas para matrizes multidimensionais, as duas notações são equivalentes. Ou seja, `Type.GetType("MyArray [,]") == Type.GetType("MyArray[*,*]")` é avaliada como **true**.

Para o o **. GetType**, `MyArray[0..5]` indica uma matriz de dimensão única com tamanho 6, menor limite 0. `MyArray[4…]` indica uma matriz de dimensão única de tamanho desconhecido e limite inferior 4.

## <a name="see-also"></a>Veja também

- <xref:System.Reflection.AssemblyName>
- <xref:System.Reflection.Emit.ModuleBuilder>
- <xref:System.Reflection.Emit.TypeBuilder>
- <xref:System.Type.FullName%2A?displayProperty=nameWithType>
- <xref:System.Type.GetType%2A?displayProperty=nameWithType>
- <xref:System.Type.AssemblyQualifiedName%2A?displayProperty=nameWithType>
- [Exibindo informações de tipo](viewing-type-information.md)
