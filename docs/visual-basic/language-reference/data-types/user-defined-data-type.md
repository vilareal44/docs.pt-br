---
title: Tipo de dados definido pelo usuário
ms.date: 07/20/2015
f1_keywords:
- UserDefined
- UDT
- vb.UDT
- User-Defined
- vb.UserDefined
- vb.User-Defined
helpviewer_keywords:
- user-defined data types [Visual Basic], Visual Basic
- user-defined types
- structures [Visual Basic], as user-defined data types
- user-defined types [Visual Basic], Visual Basic
- user-defined types [Visual Basic], structure declaration
- user-defined types [Visual Basic], structures in Visual Basic
- user-defined data types [Visual Basic], structure declaration
- data types [Visual Basic], assigning
- Structure statement [Visual Basic]
- data types [Visual Basic], user-defined
- user-defined data types [Visual Basic], structures in Visual Basic
- user-defined data types
- types [Visual Basic], user-defined
ms.assetid: be913dca-a364-4a51-96a1-549a1b390b0a
ms.openlocfilehash: fbd9536a54d7fb471d6cb2e130b14a84e40a4940
ms.sourcegitcommit: f8c270376ed905f6a8896ce0fe25b4f4b38ff498
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/04/2020
ms.locfileid: "84415486"
---
# <a name="user-defined-data-type"></a>Tipo de dados definido pelo usuário

Contém dados em um formato que você define. A `Structure` instrução define o formato.

As versões anteriores do Visual Basic dão suporte ao tipo definido pelo usuário (UDT). A versão atual expande o UDT para uma *estrutura*. Uma estrutura é uma concatenação de um ou mais *Membros* de vários tipos de dados. Visual Basic trata uma estrutura como uma única unidade, embora você também possa acessar seus membros individualmente.

## <a name="remarks"></a>Comentários

Defina e use um tipo de dados de estrutura quando for necessário combinar vários tipos de dados em uma única unidade ou quando nenhum dos tipos de dados elementares atender às suas necessidades.

O valor padrão de um tipo de dados de estrutura consiste na combinação dos valores padrão de cada um de seus membros.

## <a name="declaration-format"></a>Formato de declaração

Uma declaração de estrutura começa com a [instrução Structure](../statements/structure-statement.md) e termina com a `End Structure` instrução. A `Structure` instrução fornece o nome da estrutura, que também é o identificador do tipo de dados que a estrutura está definindo. Outras partes do código podem usar esse identificador para declarar variáveis, parâmetros e valores de retorno de função para serem do tipo de dados dessa estrutura.

As declarações entre as `Structure` `End Structure` instruções e definem os membros da estrutura.

## <a name="member-access-levels"></a>Níveis de acesso de membro

Você deve declarar todos os membros usando uma [instrução Dim](../statements/dim-statement.md) ou uma instrução que especifique o nível de acesso, como [Public](../modifiers/public.md), [Friend](../modifiers/friend.md)ou [Private](../modifiers/private.md). Se você usar uma `Dim` instrução, o nível de acesso padrão será público.

## <a name="programming-tips"></a>Dicas de programação

- **Consumo de memória.** Assim como acontece com todos os tipos de dados compostos, você não pode calcular com segurança o consumo total de memória de uma estrutura adicionando as alocações de armazenamento nominal de seus membros. Além disso, você não pode supor com segurança que a ordem de armazenamento na memória é igual à sua ordem de declaração. Se você precisar controlar o layout de armazenamento de uma estrutura, poderá aplicar o <xref:System.Runtime.InteropServices.StructLayoutAttribute> atributo à `Structure` instrução.

- **Considerações sobre interoperabilidade.** Se você estiver fazendo a interface com componentes não escritos para o .NET Framework, por exemplo, automação ou objetos COM, tenha em mente que os tipos definidos pelo usuário em outros ambientes não são compatíveis com Visual Basic tipos de estrutura.

- **Ampliação.** Não há conversão automática de ou para nenhum tipo de dados de estrutura. Você pode definir operadores de conversão em sua estrutura usando a [instrução Operator](../statements/operator-statement.md)e pode declarar cada operador de conversão como `Widening` ou `Narrowing` .

- **Digite os caracteres.** Os tipos de dados de estrutura não têm nenhum caractere de tipo literal ou caractere de tipo de identificador.

- **Tipo de estrutura.** Não há nenhum tipo correspondente no .NET Framework. Todas as estruturas herdam da classe .NET Framework <xref:System.ValueType?displayProperty=nameWithType> , mas nenhuma estrutura individual corresponde a <xref:System.ValueType?displayProperty=nameWithType> .

## <a name="example"></a>Exemplo

O paradigma a seguir mostra o contorno da declaração de uma estrutura.

```vb
[Public | Protected | Friend | Protected Friend | Private] Structure structname
    {Dim | Public | Friend | Private} member1 As datatype1
    ' ...
    {Dim | Public | Friend | Private} memberN As datatypeN
End Structure
```

## <a name="see-also"></a>Confira também

- <xref:System.ValueType>
- <xref:System.Runtime.InteropServices.StructLayoutAttribute>
- [Tipos de dados](index.md)
- [Funções de conversão do tipo](../functions/type-conversion-functions.md)
- [Resumo da Conversão](../keywords/conversion-summary.md)
- [Instrução Structure](../statements/structure-statement.md)
- [Widening](../modifiers/widening.md)
- [Narrowing](../modifiers/narrowing.md)
- [Estruturas](../../programming-guide/language-features/data-types/structures.md)
- [Uso eficiente de tipos de dados](../../programming-guide/language-features/data-types/efficient-use-of-data-types.md)
