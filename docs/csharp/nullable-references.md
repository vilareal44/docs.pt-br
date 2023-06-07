---
title: Tipos de referência anuláveis
description: Este artigo fornece uma visão geral dos tipos de referência anuláveis, adicionados ao C# 8,0. Você aprenderá como o recurso fornece segurança com relação a exceções de referência nula para projetos novos e existentes.
ms.technology: csharp-null-safety
ms.date: 04/21/2020
ms.openlocfilehash: 6d068760805a21e41712a4f70735bef41ce2052f
ms.sourcegitcommit: b16c00371ea06398859ecd157defc81301c9070f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/05/2020
ms.locfileid: "84446666"
---
# <a name="nullable-reference-types"></a>Tipos de referência anuláveis

O C# 8.0 apresenta **tipos de referência que permitem valor nulo** e **tipos de referência que não permitem valor nulo** que permitem que você crie importantes instruções sobre as propriedades para variáveis de tipo de referência:

- **Uma referência não deve ser nula**. Quando variáveis não devem ser nulas, o compilador impõe regras que garantem que é seguro desreferenciar essas variáveis sem primeiro verificar se ela não é nula:
  - A variável deve ser inicializada para um valor não nulo.
  - A variável nunca pode receber o valor `null`.
- **Uma referência pode ser nula**. Quando variáveis puderem ser nulas, o compilador imporá diferentes regras para garantir que você verificou corretamente se há uma referência nula:
  - a variável só poderá ser desreferenciada quando o compilador puder garantir que o valor não é nulo.
  - Essas variáveis podem ser inicializadas com o valor `null` padrão e receber o valor `null` em outro código.

Esse novo recurso fornece benefícios significativos em relação ao tratamento de variáveis de referência em versões anteriores do C#, em que a intenção de design não pode ser determinada a partir da declaração de variável. O compilador não forneceu segurança com relação a exceções de referência nula para tipos de referência:

- **Uma referência pode ser nula**. O compilador não emite avisos quando um tipo de referência é inicializado como nulo ou posteriormente atribuído a NULL. O compilador emite avisos quando essas variáveis são desreferenciadas sem verificações nulas.
- **Uma referência é considerada não nula**. O compilador não emite nenhum aviso quando os tipos de referência são desreferenciados. O compilador emite avisos se uma variável é definida como uma expressão que pode ser nula.

Esses avisos são emitidos no momento da compilação. O compilador não adiciona nenhuma verificação nula ou outras construções de tempo de execução em um contexto anulável. Em tempo de execução, uma referência anulável e uma referência não anulável são equivalentes.

Com a adição de tipos de referência que permitem valor nulo, é possível declarar sua intenção mais claramente. O valor `null` é a maneira correta de representar que uma variável não se refere a um valor. Não use esse recurso para remover todos os valores `null` do seu código. Em vez disso, você deve declarar sua intenção par ao compilador e para outros desenvolvedores que leem seu código. Ao declarar sua intenção, o compilador informa quando você escreve um código inconsistente com essa intenção.

Um **tipo de referência que permite valor nulo** é indicado usando a mesma sintaxe que [tipos de valor que permitem valor nulo](language-reference/builtin-types/nullable-value-types.md): um `?` é acrescentado ao tipo da variável. Por exemplo, a seguinte declaração de variável representa uma variável de cadeia de caracteres que permite valor nulo, `name`:

```csharp
string? name;
```

Qualquer variável em que o `?` não seja acrescentado ao nome do tipo é um **tipo de referência não anulável**. Isso inclui todas as variáveis de tipo de referência no código existente quando você habilitou esse recurso.

O compilador usa uma análise estática para determinar se uma referência que permite valor nulo é conhecida como não nula. O compilador informa se você desreferencia uma referência quer permite valor nulo quando ela pode ser nula. Você pode substituir esse comportamento usando o [operador NULL-tolerante](language-reference/operators/null-forgiving.md) `!` seguindo um nome de variável. Por exemplo, se você sabe que a variável `name` não é nula, mas o compilador emite um aviso, é possível escrever o seguinte código para substituir a análise do compilador:

```csharp
name!.Length;
```

## <a name="nullability-of-types"></a>Possibilidade de nulidade de tipos

Qualquer tipo de referência pode ter uma das quatro *nulidades*, que descreve quando os avisos são gerados:

- Não *nulo*: nulo não pode ser atribuído a variáveis deste tipo. As variáveis desse tipo não precisam ser verificadas quanto à nulidade antes de desreferenciar.
- *Nullable*: NULL pode ser atribuído a variáveis desse tipo. Desreferenciar variáveis desse tipo sem verificar primeiro se há `null` gera um aviso.
- *Alheios*: alheios é o estado de pré-C # 8,0. As variáveis desse tipo podem ser desreferenciadas ou atribuídas sem avisos.
- *Desconhecido*: desconhecido é geralmente para parâmetros de tipo em que as restrições não dizem ao compilador que o tipo deve ser *anulável* ou não *nulo*.

A nulidade de um tipo em uma declaração de variável é controlada pelo *contexto que permite valor nulo* no qual a variável é declarada.

## <a name="nullable-contexts"></a>Contextos que permitem valor nulo

Contextos que permitem valor nulo habilitam o controle refinado para a maneira como o compilador interpreta variáveis de tipo de referência. O **contexto de anotação anulável** de qualquer linha de origem determinada é habilitado ou desabilitado. Você pode considerar o compilador pré-C # 8,0 como compilar todo o seu código em um contexto anulável desabilitado: qualquer tipo de referência pode ser nulo. O **contexto de avisos anuláveis** também pode ser habilitado ou desabilitado. O contexto de avisos que permite valor nulo especifica os avisos gerados pelo compilador usando sua análise de fluxo.

O contexto de anotação anulável e o contexto de aviso anulável podem ser definidos para um projeto usando o `Nullable` elemento em seu arquivo *. csproj* . Esse elemento configura como o compilador interpreta a nulidade de tipos e quais avisos são gerados. As configurações válidas são:

- `enable`: O contexto de anotação anulável está **habilitado**. O contexto de aviso que permite valor nulo está **habilitado**.
  - Variáveis de um tipo de referência, `string` por exemplo, não permitem valor nulo.  Todos os avisos de nulidade estão habilitados.
- `warnings`: O contexto de anotação anulável está **desabilitado**. O contexto de aviso que permite valor nulo está **habilitado**.
  - As variáveis de um tipo de referência são óbvias. Todos os avisos de nulidade estão habilitados.
- `annotations`: O contexto de anotação anulável está **habilitado**. O contexto de aviso que permite valor nulo está **desabilitado**.
  - Variáveis de um tipo de referência, String, por exemplo, são não anuláveis. Todos os avisos de nulidade estão desabilitados.
- `disable`: O contexto de anotação anulável está **desabilitado**. O contexto de aviso que permite valor nulo está **desabilitado**.
  - As variáveis de um tipo de referência são alheias, como as versões anteriores do C#. Todos os avisos de nulidade estão desabilitados.

**Exemplo**:

```xml
<Nullable>enable</Nullable>
```

Também é possível usar diretivas para definir esses mesmos contextos em qualquer lugar no seu projeto:

- `#nullable enable`: Define o contexto de anotação anulável e o contexto de aviso anulável como **habilitado**.
- `#nullable disable`: Define o contexto de anotação anulável e o contexto de aviso anulável como **desabilitado**.
- `#nullable restore`: Restaura o contexto de anotação anulável e o contexto de aviso anulável para as configurações do projeto.
- `#nullable disable warnings`: Defina o contexto de aviso anulável como **desabilitado**.
- `#nullable enable warnings`: Defina o contexto de aviso anulável como **habilitado**.
- `#nullable restore warnings`: Restaura o contexto de aviso anulável para as configurações do projeto.
- `#nullable disable annotations`: Defina o contexto de anotação anulável como **desabilitado**.
- `#nullable enable annotations`: Defina o contexto de anotação anulável como **habilitado**.
- `#nullable restore annotations`: Restaura o contexto de aviso de anotação para as configurações do projeto.

Por padrão, os contextos de anotação e de aviso anuláveis são **desabilitados**, incluindo novos projetos. Isso significa que o código existente é compilado sem alterações e sem gerar nenhum aviso novo.

Essas opções fornecem duas estratégias distintas para [atualizar uma base de código existente](nullable-migration-strategies.md) para usar tipos de referência anuláveis.

## <a name="nullable-annotation-context"></a>Contexto de anotação que permite valor nulo

O compilador usa as seguintes regras em um contexto de anotação que permite valor nulo desabilitado:

- Não é possível declarar referências que permitem valor nulo em um contexto desabilitado.
- Todas as variáveis de referência podem ser atribuídas a um valor nulo.
- Nenhum aviso é gerado quando uma variável de um tipo de referência é desreferenciado.
- O operador tolerante a nulo pode não ser usado em um contexto desabilitado.

O comportamento é o mesmo que o das versões anteriores de C#.

O compilador usa as seguintes regras em um contexto de anotação que permite valor nulo habilitado:

- Qualquer variável de um tipo de referência é uma **referência que não permite valor nulo**.
- Qualquer referência que não permite valor nulo pode ser desreferenciada com segurança.
- Qualquer tipo de referência que permite valor nulo (indicado pelo `?` após o tipo na declaração de variável) pode ser nulo. A análise estática determina se o valor é conhecido como não nulo quando é desreferenciado. Caso contrário, o compilador avisa.
- É possível usar o operador tolerante a nulo para declarar que uma referência que permite valor nulo não é nula.

Em um contexto de anotação que permite valor nulo habilitado, o caractere `?` acrescentado a um tipo de referência declara um **tipo de referência que permite valor nulo**. O **operador NULL-tolerante** `!` pode ser anexado a uma expressão para declarar que a expressão não é nula.

## <a name="nullable-warning-context"></a>Contexto de aviso que permite valor nulo

O contexto de aviso que permite valor nulo é diferente do contexto de anotação que permite valor nulo. Os avisos podem ser habilitados mesmo quando as novas anotações estão desabilitadas. O compilador usa a análise de fluxo estática para determinar o **estado nulo** de qualquer referência. O estado nulo é **não nulo** ou **talvez nulo** quando o *contexto de aviso que permite valor nulo* não está **desabilitado**. Se você desreferenciar uma referência quando o compilador tiver determinado que ela é **talvez nula**, o compilador avisará. O estado de uma referência é **talvez nulo**, a menos que o compilador possa determinar uma das duas condições:

1. A variável foi atribuída definitivamente a um valor não nulo.
1. A variável ou expressão foi marcada novamente como nula antes de desreferenciá-la.

O compilador gera avisos ao desreferenciar uma variável ou expressão que **talvez seja nula** em um contexto de aviso anulável. Além disso, o compilador gera avisos quando um tipo de referência não nula é atribuído a uma variável ou expressão **nula** em um contexto de anotação anulável habilitado.

## <a name="attributes-describe-apis"></a>Atributos descrevem APIs

Você adiciona atributos a APIs que fornecem ao compilador mais informações sobre quando argumentos ou valores de retorno podem ou não ser nulos. Você pode aprender mais sobre esses atributos em nosso artigo na referência de linguagem que abrange os [atributos anuláveis](language-reference/attributes/nullable-analysis.md). Esses atributos estão sendo adicionados às bibliotecas do .NET em versões atuais e futuras. As APIs usadas com mais frequência estão sendo atualizadas primeiro.

## <a name="see-also"></a>Confira também

- [Especificação de tipos de referência Nullable de rascunho](~/_csharplang/proposals/csharp-8.0/nullable-reference-types-specification.md)
- [Introdução ao tutorial de referências que permitem valor nulo](tutorials/nullable-reference-types.md)
- [Migrar uma base de código para referências que permitem valor nulo](tutorials/upgrade-to-nullable-references.md)
- [-Nullable (opção de compilador C#)](language-reference/compiler-options/nullable-compiler-option.md)
