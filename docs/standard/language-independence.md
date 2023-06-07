---
title: Componentes de independência de linguagem e componentes independentes da linguagem
description: Saiba como você pode desenvolver em uma das muitas linguagens com suporte no .NET, como C#, C++/CLI, F#, IronPython, VB, Visual COBOL e PowerShell.
ms.date: 07/22/2016
dev_langs:
- csharp
- vb
ms.technology: dotnet-standard
ms.assetid: 2dbed1bc-86f5-43cd-9a57-adbb1c5efba4
ms.openlocfilehash: 813558299b40e0b90e8047f22b788c8f1419eb5e
ms.sourcegitcommit: da21fc5a8cce1e028575acf31974681a1bc5aeed
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/08/2020
ms.locfileid: "84504648"
---
# <a name="language-independence-and-language-independent-components"></a>Componentes de independência de linguagem e componentes independentes da linguagem

O .NET é independente de linguagem. Isso significa que, como desenvolvedor, você pode desenvolver em uma das muitas linguagens direcionadas às implementações do .NET, como C#, F# e Visual Basic. É possível acessar tipos e membros de bibliotecas de classes desenvolvidas para implementações do .NET sem que seja necessário conhecer a linguagem em que foram originalmente escritas e sem precisar seguir as convenções da linguagem original. Se você for um desenvolvedor de componentes, seu componente poderá ser acessado por qualquer aplicativo .NET, independentemente de seu idioma.

> [!NOTE]
> Esta primeira parte deste artigo aborda a criação de componentes independentes de linguagem, ou seja, componentes que podem ser consumidos por aplicativos que são escritos em qualquer linguagem. Você também pode criar um único componente ou aplicativo de código-fonte gravado em várias linguagens; consulte [Interoperabilidade em qualquer idioma](#cross-language-interoperability) na segunda parte deste artigo.

Para interagir completamente com outros objetos gravados em qualquer linguagem, os objetos devem expor aos chamadores somente os recursos comuns a todas as linguagens. Esse conjunto comum de recursos é definido pela CLS (Common Language Specification), que é um conjunto de regras que se aplicam aos assemblies gerados. A Common Language Specification é definida na Partição I, cláusulas 7 a 11 do [Padrão ECMA-335: Common Language Infrastructure](https://www.ecma-international.org/publications/standards/Ecma-335.htm).

Se o componente estiver de acordo com a Common Language Specification, ele será compatível com a CLS e poderá ser acessado pelo código em assemblies gravados em qualquer linguagem de programação que dê suporte a CLS. É possível determinar se o componente está de acordo com a Common Language Specification no tempo de compilação aplicando o atributo [CLSCompliantAttribute](xref:System.CLSCompliantAttribute) ao código-fonte. Para obter mais informações, consulte [o atributo CLSCompliantAttribute](#the-clscompliantattribute-attribute).

## <a name="cls-compliance-rules"></a>Regras de conformidade com CLS

Esta seção discute as regras para criar um componente compatível com CLS. Para obter uma lista completa de regras, consulte Partição I, Cláusula 11 do [Padrão ECMA-335: Common Language Infrastructure](https://www.ecma-international.org/publications/standards/Ecma-335.htm).

> [!NOTE]
> A Common Language Specification aborda cada regra de conformidade com CLS à medida que se aplica a consumidores (desenvolvedores que estão acessando programaticamente um componente em conformidade com CLS), estruturas (desenvolvedores que estão usando um compilador de linguagem para criar bibliotecas em conformidade com CLS) e extensores (desenvolvedores que estão criando uma ferramenta, como um compilador de linguagem ou um analisador de código que cria componentes em conformidade com CLS). Este artigo enfoca as regras que se aplicam às estruturas. Entretanto, algumas das regras que se aplicam a extensores também podem ser aplicadas a assemblies criados usando [Reflection.Emit](xref:System.Reflection.Emit).

Para criar um componente independente de linguagem, você só precisa aplicar as regras de compatibilidade com CLS à interface pública do componente. A implementação privada não precisa estar de acordo com a especificação.

> [!IMPORTANT]
> As regras de conformidade com CLS só se aplicam à interface pública de um componente e não à implementação privada.

Por exemplo, inteiros sem sinal que não sejam [Byte](xref:System.Byte) não são compatíveis com CLS. Como a classe `Person` no exemplo a seguir expõe uma propriedade `Age` de tipo [UInt16](xref:System.UInt16), o código a seguir exibe um aviso do compilador.

```csharp
using System;

[assembly: CLSCompliant(true)]

public class Person
{
   private UInt16 personAge = 0;

   public UInt16 Age
   { get { return personAge; } }
}
// The attempt to compile the example displays the following compiler warning:
//    Public1.cs(10,18): warning CS3003: Type of 'Person.Age' is not CLS-compliant
```

```vb
<Assembly: CLSCompliant(True)>

Public Class Person
   Private personAge As UInt16

   Public ReadOnly Property Age As UInt16
      Get
         Return personAge
      End Get
   End Property
End Class
' The attempt to compile the example displays the following compiler warning:
'    Public1.vb(9) : warning BC40027: Return type of function 'Age' is not CLS-compliant.
'
'       Public ReadOnly Property Age As UInt16
'                                ~~~
```

É possível tornar a classe Pessoa compatível com CLS alterando o tipo de propriedade `Age` de `UInt16` para [Int16](xref:System.Int16), que é um inteiro com sinal de 16 bits compatível com CLS. Não é necessário alterar o tipo do campo `personAge` privado.

```csharp
using System;

[assembly: CLSCompliant(true)]

public class Person
{
   private Int16 personAge = 0;

   public Int16 Age
   { get { return personAge; } }
}
```

```vb
<Assembly: CLSCompliant(True)>

Public Class Person
   Private personAge As UInt16

   Public ReadOnly Property Age As Int16
      Get
         Return CType(personAge, Int16)
      End Get
   End Property
End Class
```

A interface pública de uma biblioteca consiste no seguinte:

* Definições de classes públicas.

* Definições dos membros públicos de classes públicas e definições de membros acessíveis para classes derivadas (ou seja, membros protegidos).

* Parâmetros e tipos de retorno de métodos públicos de classes públicas e parâmetros e tipos de retorno de métodos acessíveis para classes derivadas.

As regras de conformidade com CLS estão listadas na tabela a seguir. O texto das regras é tirado literalmente do [Padrão ECMA-335: Common Language Infrastructure](https://www.ecma-international.org/publications/standards/Ecma-335.htm), com direitos autorais de 2012 da Ecma International. Informações mais detalhadas sobre essas regras são encontradas nas seções a seguir.

Categoria | Consulte | Regra | Número da regra
-------- | --- | ---- | -----------
Acessibilidade | [Acessibilidade de membro](#member-accessibility) | A acessibilidade não deverá ser alterada ao substituir métodos herdados, exceto na substituição de um método herdado de um assembly diferente com acessibilidade `family-or-assembly`. Nesse caso, a substituição deverá ter a acessibilidade `family`. | 10
Acessibilidade | [Acessibilidade de membro](#member-accessibility) | A visibilidade e a acessibilidade de tipos e membros deverão ser de tal forma que os tipos na assinatura de qualquer membro sejam visíveis e acessíveis sempre que o próprio membro estiver visível e acessível. Por exemplo, um método público visível fora do assembly não deve ter um argumento cujo tipo seja visível somente dentro do assembly. A visibilidade e a acessibilidade dos tipos que compõem um tipo genérico instanciado usado na assinatura de qualquer membro deverão estar visíveis e acessíveis sempre que o próprio membro estiver visível e acessível. Por exemplo, um tipo genérico instanciado presente na assinatura de um membro visível fora do assembly não deverá ter um argumento genérico cujo tipo seja visível somente dentro do assembly. | 12
Matrizes | [Matrizes](#arrays) | As matrizes deverão ter elementos com um tipo compatível com CLS e todas as dimensões da matriz deverão ter limites inferiores iguais a zero. Se o item for uma matriz, o tipo do elemento da matriz será necessário para diferenciar as sobrecargas. Quando o sobrecarregamento é baseado em dois ou mais tipos de matriz, os tipos de elementos devem ser tipos nomeados. | 16
Atributos | [Atributos](#attributes) | Os atributos deverão ser do tipo [System.Attribute](xref:System.Attribute) ou de um tipo herdado dele. | 41
Atributos | [Atributos](#attributes) | A CLS só permite um subconjunto das codificações de atributos personalizados. Os únicos tipos que devem ser exibidos nessas codificações são (consulte a Partição IV) [System.Type](xref:System.Type), [System.String](xref:System.String), [System.Char](xref:System.Char), [System.Boolean](xref:System.Boolean), [System.Byte](xref:System.Byte), [System.Int16](xref:System.Int16), [System.Int32](xref:System.Int32), [System.Int64](xref:System.Int64), [System.Single](xref:System.Single), [System.Double](xref:System.Double) e qualquer tipo de enumeração com base em um tipo de inteiro básico compatível com CLS. | 34
Atributos | [Atributos](#attributes) | A CLS não permite modificadores obrigatórios visíveis publicamente (`modreq`, consulte a Partição II), mas permite modificadores opcionais (`modopt`, consulte a Partição II) que ela não entende. | 35
Construtores | [Construtores](#constructors) | Um construtor de objeto deverá chamar um construtor de instância de sua classe base antes de qualquer acesso aos dados da instância herdados. (Isso não se aplica a tipos de valor, que não precisam ter construtores.)  | 21
Construtores | [Construtores](#constructors) | Um construtor de objeto não deverá ser chamado, exceto como parte da criação de um objeto e um objeto não deve ser inicializado duas vezes. | 22
Enumerações | [Enumerações](#enumerations) | O tipo subjacente de um enum deverá ser um tipo de inteiro CLS interno, o nome do campo deverá ser "value__", e esse campo deverá ser marcado como `RTSpecialName`. |  7
Enumerações | [Enumerações](#enumerations) | Há dois tipos diferentes de enums, indicados pela presença ou pela ausência do atributo personalizado [System.FlagsAttribute](xref:System.FlagsAttribute) (consulte a Biblioteca da Partição IV). Um representa valores de inteiro nomeados; o outro representa sinalizadores de bit nomeados que podem ser combinados para gerar um valor sem nome. O valor de um `enum` não está limitado aos valores especificados. |  8
Enumerações | [Enumerações](#enumerations) | Campos estáticos de literais de um enum deverão ter o tipo do próprio enum. |  9
Eventos | [Eventos](#events) | Os métodos que implementam um evento deverão ser marcados como `SpecialName` nos metadados. |29
Eventos | [Eventos](#events) | A acessibilidade de um evento e de seus acessadores deverá ser idêntica. |30
Eventos | [Eventos](#events) | Os métodos `add` e `remove` de um evento deverão estar presentes ou ausentes. |31
Eventos | [Eventos](#events) | Os `add` `remove` métodos e para um evento devem receber um parâmetro cujo tipo define o tipo do evento e que deve ser derivado de [System. delegate](xref:System.Delegate). |32
Eventos | [Eventos](#events) | Os eventos deverão respeitar um padrão de nomenclatura específico. O atributo SpecialName mencionado na regra 29 da CLS deverá ser ignorado em comparações de nome apropriadas e respeitar as regras do identificador.  |33
Exceções | [Exceção](#exceptions) | Os atributos acionados deverão ser do tipo [System.Exception](xref:System.Exception) ou de um tipo herdado dele. Mesmo assim, os métodos compatíveis com CLS não precisam bloquear a propagação de outros tipos de exceção. | 40
Geral | [Regras de conformidade com CLS](#cls-compliance-rules) | As regras CLS só se aplicam a essas partes de um tipo acessíveis ou visíveis fora do assembly de definição. | 1
Geral | [Regras de conformidade com CLS](#cls-compliance-rules) | Membros de tipos sem conformidade com CLS não deverão ser marcados como em conformidade com CLS. | 2
Genéricos | [Tipos e membros genéricos](#generic-types-and-members) | Os tipos aninhados deverão ter, pelo menos, tantos parâmetros genéricos quanto o tipo delimitador. Os parâmetros genéricos em um tipo aninhado correspondem, por posição, aos parâmetros genéricos no tipo delimitador.  | 42
Genéricos | [Tipos e membros genéricos](#generic-types-and-members) | O nome de um tipo genérico deverá codificar o número de parâmetros de tipo declarados no tipo não aninhado ou recém-introduzidos no tipo, se aninhado, de acordo com as regras definidas anteriormente. | 43
Genéricos | [Tipos e membros genéricos](#generic-types-and-members) | Um tipo genérico deverá redeclarar restrições suficientes para assegurar que todas as restrições no tipo base ou nas interfaces sejam atendidas pelas restrições de tipo genérico. | 44
Genéricos | [Tipos e membros genéricos](#generic-types-and-members) | Tipos usados como restrições em parâmetros genéricos deverão ser compatíveis com CLS. | 45
Genéricos | [Tipos e membros genéricos](#generic-types-and-members) | A visibilidade e a acessibilidade de membros (incluindo tipos aninhados) em um tipo genérico instanciado deverão ser consideradas no escopo da instanciação específica, em vez da declaração de tipo genérico como um todo. Supondo isso, as regras de visibilidade e acessibilidade da regra 12 da CLS continuam sendo aplicáveis. | 46
Genéricos | [Tipos e membros genéricos](#generic-types-and-members) | Para cada método genérico abstrato ou virtual, deverá haver uma implementação concreta (não abstrata) padrão | 47
Interfaces | [Interfaces](#interfaces) | As interfaces em conformidade com CLS não deverão exigir a definição de métodos incompatíveis com CLS para implementá-los. | 18
Interfaces | [Interfaces](#interfaces) | As interfaces compatíveis com CLS não deverão definir métodos estáticos, nem devem definir campos. | 19
Membros | [Membros de tipo em geral](#type-members-in-general) | Campos e métodos estáticos globais não são compatíveis com CLS. | 36
Membros | -- | O valor de um estático literal é especificado por meio do uso de metadados de inicialização do campo. Um literal compatível com CLS deve ter um valor especificado em metadados de inicialização de campo que sejam exatamente do mesmo tipo que o literal (ou do tipo subjacente, se esse literal for um `enum`). | 13
Membros | [Membros de tipo em geral](#type-members-in-general) | A restrição vararg não faz parte da CLS e a única convenção de chamada com suporte pela CLS é a convenção de chamada gerenciada padrão. | 15
Convenções de nomenclatura | [Convenções de nomenclatura](#naming-conventions) | Os assemblies deverão seguir o Anexo 7 do Relatório Técnico 15 do Padrão Unicode 3.0 que controla o conjunto de caracteres permitidos para iniciar e serem incluídos em identificadores, disponíveis online em [Formulários de Normalização de Unicode](https://unicode.org/reports/tr15/). Os identificadores deverão estar no formato canônico definido pelo Formulário C de Normalização de Unicode. Para fins de CLS, dois identificadores serão iguais se os mapeamentos em minúsculas (conforme especificado pelos mapeamentos em minúsculas um para um, insensíveis a localidade Unicode) forem os mesmos. Ou seja, para dois identificadores serem considerados diferentes na CLS, eles deverão ser diferentes além de apenas maiúsculas e minúsculas. No entanto, para substituir uma definição herdada, a CLI exige que a codificação precisa da declaração original seja usada. | 4
Sobrecarga | [Convenções de nomenclatura](#naming-conventions) | Todos os nomes introduzidos em um escopo compatível com CLS deverão ser independentes e distintos do tipo, exceto quando os nomes forem idênticos e resolvidos por meio da sobrecarga. Ou seja, embora o CTS permita que um tipo single use o mesmo nome para um método e um campo, a CLS não permite. | 5
Sobrecarga | [Convenções de nomenclatura](#naming-conventions) | Campos e tipos aninhados deverão ser diferenciados apenas por comparação de identificador, mesmo que o CTS permita que assinaturas diferentes sejam distinguidas. Métodos, propriedades e eventos que têm o mesmo nome (por comparação de identificador) devem ser diferentes por mais do que apenas o tipo de retorno, exceto conforme especificado na regra CLS 39 | 6
Sobrecarga | [Sobrecargas](#overloads) | Somente propriedades e métodos podem ser sobrecarregados. | 37
Sobrecarga | [Sobrecargas](#overloads) |As propriedades e os métodos só podem ser sobrecarregados com base no número e nos tipos de seus parâmetros, exceto os operadores de conversão chamados `op_Implicit` e `op_Explicit`, que também podem ser sobrecarregados com base no tipo de retorno. | 38
Sobrecarga | -- | Se dois ou mais métodos em conformidade com CLS declarados em um tipo tiverem o mesmo nome e, para um conjunto específico de instanciações de tipo, tiverem os mesmos tipos de parâmetro e retorno, esses métodos deverão ser semanticamente equivalentes nessas instanciações de tipo. | 48
Propriedades | [Propriedades](#properties) | Os métodos que implementam os métodos getter e setter de uma propriedade deverão ser marcados como `SpecialName` nos metadados. | 24
Propriedades | [Propriedades](#properties) | Os acessadores de uma propriedade serão todos estáticos, todos serão virtuais ou todas as instâncias. | 26
Propriedades | [Propriedades](#properties) | O tipo de uma propriedade deverá ser o tipo de retorno do getter e o tipo do último argumento do setter. Os tipos dos parâmetros da propriedade deverão ser os tipos dos parâmetros do getter e os tipos de todos os parâmetros, menos o parâmetro final do setter. Todos esses tipos deverão ser compatíveis com CLS e não deverão ser ponteiros gerenciados (ou seja, não deverão ser passados por referência). | 27
Propriedades | [Propriedades](#properties) | As propriedades deverão seguir um padrão de nomenclatura específico. O atributo `SpecialName` mencionado na regra 24 da CLS deverá ser ignorado em comparações de nome apropriadas e respeitar as regras do identificador. Uma propriedade deverá ter um método getter, um método setter ou ambos. | 28
Conversão de tipos | [Conversão de tipo](#type-conversion) | Se op_Implicit ou op_Explicit for fornecido, um meio alternativo de coerção deverá ser fornecido. | 39
Tipos | [Tipos e assinaturas de membro de tipo](#types-and-type-member-signatures) | Tipos de valor demarcado não estão em conformidade com CLS. | 3
Tipos | [Tipos e assinaturas de membro de tipo](#types-and-type-member-signatures) | Todos os tipos exibidos em uma assinatura deverão ser compatíveis com CLS. Todos os tipos que compõem um tipo genérico instanciado deverão ser compatíveis com CLS. | 11
Tipos | [Tipos e assinaturas de membro de tipo](#types-and-type-member-signatures) | Referências com tipo não são compatíveis com CLS. | 14
Tipos | [Tipos e assinaturas de membro de tipo](#types-and-type-member-signatures) | Tipos de ponteiro não gerenciados não são compatíveis com CLS. | 17
Tipos | [Tipos e assinaturas de membro de tipo](#types-and-type-member-signatures) | Classes compatíveis com CLS, tipos de valor e interfaces não deverão exigir a implementação de membros incompatíveis com CLS | 20
Tipos | [Tipos e assinaturas de membro de tipo](#types-and-type-member-signatures) | [System. Object](xref:System.Object) é compatível com CLS. Qualquer outra classe compatível com CLS deverá herdar de uma classe compatível com CLS. | 23

Indexar em subseções:

* [Tipos e assinaturas de membro de tipo](#types-and-type-member-signatures)
* [Convenções de nomenclatura](#naming-conventions)
* [Conversão de tipo](#type-conversion)
* [Matrizes](#arrays)
* [Interfaces](#interfaces)
* [Enumerações](#enumerations)
* [Membros de tipo em geral](#type-members-in-general)
* [Acessibilidade de membro](#member-accessibility)
* [Tipos e membros genéricos](#generic-types-and-members)
* [Construtores](#constructors)
* [Propriedades](#properties)
* [Eventos](#events)
* [Sobrecargas](#overloads)
* [Exceção](#exceptions)
* [Atributos](#attributes)

### <a name="types-and-type-member-signatures"></a>Tipos e assinaturas de membro de tipo

O tipo [System.Object](xref:System.Object) é compatível com CLS e é o tipo base de todos os tipos de objeto no sistema de tipos do .NET Framework. A herança no .NET Framework é implícita (por exemplo, a classe [String](xref:System.String) herda implicitamente da classe `Object`) ou explícita (por exemplo, a classe [CultureNotFoundException](xref:System.Globalization.CultureNotFoundException) herda explicitamente da classe [ArgumentException](xref:System.ArgumentException), que herda explicitamente da classe [Exception](xref:System.Exception)). Para que um tipo derivado esteja em conformidade com CLS, seu tipo base também deverá estar em conformidade com CLS.

O exemplo a seguir mostra um tipo derivado cujo tipo de base não é compatível com CLS. Ele define uma classe `Counter` base que usa um inteiro de 32 bits sem sinal como um contador. Como a classe fornece funcionalidade de contador encapsulando um inteiro sem sinal, a classe é marcada como não compatível com CLS. Assim, uma classe derivada, `NonZeroCounter`, também não é compatível com CLS.

```csharp
using System;

[assembly: CLSCompliant(true)]

[CLSCompliant(false)]
public class Counter
{
   UInt32 ctr;

   public Counter()
   {
      ctr = 0;
   }

   protected Counter(UInt32 ctr)
   {
      this.ctr = ctr;
   }

   public override string ToString()
   {
      return $"{ctr}). ";
   }

   public UInt32 Value
   {
      get { return ctr; }
   }

   public void Increment()
   {
      ctr += (uint) 1;
   }
}

public class NonZeroCounter : Counter
{
   public NonZeroCounter(int startIndex) : this((uint) startIndex)
   {
   }

   private NonZeroCounter(UInt32 startIndex) : base(startIndex)
   {
   }
}
// Compilation produces a compiler warning like the following:
//    Type3.cs(37,14): warning CS3009: 'NonZeroCounter': base type 'Counter' is not
//            CLS-compliant
//    Type3.cs(7,14): (Location of symbol related to previous warning)
```

```vb
<Assembly: CLSCompliant(True)>

<CLSCompliant(False)> _
Public Class Counter
   Dim ctr As UInt32

   Public Sub New
      ctr = 0
   End Sub

   Protected Sub New(ctr As UInt32)
      ctr = ctr
   End Sub

   Public Overrides Function ToString() As String
      Return $"{ctr}). "
   End Function

   Public ReadOnly Property Value As UInt32
      Get
         Return ctr
      End Get
   End Property

   Public Sub Increment()
      ctr += CType(1, UInt32)
   End Sub
End Class

Public Class NonZeroCounter : Inherits Counter
   Public Sub New(startIndex As Integer)
      MyClass.New(CType(startIndex, UInt32))
   End Sub

   Private Sub New(startIndex As UInt32)
      MyBase.New(CType(startIndex, UInt32))
   End Sub
End Class
' Compilation produces a compiler warning like the following:
'    Type3.vb(34) : warning BC40026: 'NonZeroCounter' is not CLS-compliant
'    because it derives from 'Counter', which is not CLS-compliant.
'
'    Public Class NonZeroCounter : Inherits Counter
'                 ~~~~~~~~~~~~~~
```

Todos os tipos exibidos em assinaturas de membro, incluindo um tipo de retorno de método ou um tipo de propriedade, devem ser compatíveis com CLS. Além disso, para tipos genéricos:

* Todos os tipos que compõe um tipo genérico instanciado devem ser compatíveis com CLS.

* Todos os tipos usados como restrições em parâmetros genéricos devem ser compatíveis com CLS.

O [Common Type System](common-type-system.md) do .NET inclui vários tipos internos com suporte diretamente com o Common Language Runtime e codificados especialmente nos metadados de um assembly. Desses tipos intrínsecos, os tipos listados na tabela a seguir estão em conformidade com CLS.

Tipo em conformidade com CLS | Descrição
------------------ | -----------
[Minuciosa](xref:System.Byte) | Inteiro sem sinal de 8 bits
[Int16](xref:System.Int16) | Inteiro com sinal de 16 bits
[Int32](xref:System.Int32) | Inteiro com sinal de 32 bits
[Int64](xref:System.Int64) | Inteiro com sinal de 64 bits
[Exclusivo](xref:System.Single) | Valor do ponto flutuante de precisão simples
[Clique](xref:System.Double) | Valor de ponto flutuante de precisão dupla
[Boolean](xref:System.Boolean) | tipo de valor verdadeiro ou falso
[º](xref:System.Char) | unidade de código codificado UTF-16
[Decimal](xref:System.Decimal) | Número decimal de ponto não flutuante
[IntPtr](xref:System.IntPtr) | Ponteiro ou identificador de um tamanho definido por plataforma
[Cadeia de caracteres](xref:System.String) | Coleção de zero, um ou mais objetos Char

Os tipos intrínsecos listados na tabela a seguir não são compatíveis com CLS.

Tipo não compatível | Descrição | Alternativa em conformidade com CLS
------------------ | ----------- | -------------------------
[SByte](xref:System.SByte) | Tipo de dados inteiro com sinal de 8 bits | [Int16](xref:System.Int16)
[UInt16](xref:System.UInt16) | Inteiro sem sinal de 16 bits | [Int32](xref:System.Int32)
[UInt32](xref:System.UInt32) | Inteiro sem sinal de 32 bits | [Int64](xref:System.Int64)
[UInt64](xref:System.UInt64) | Inteiro sem sinal de 64 bits | [Int64](xref:System.Int64) (pode estourar), [BigInteger](xref:System.Numerics.BigInteger), ou[Double](xref:System.Double)
[UIntPtr](xref:System.UIntPtr) | Ponteiro ou identificador sem sinal | [IntPtr](xref:System.IntPtr)

A biblioteca de classes .NET Framework ou qualquer outra biblioteca de classes pode incluir outros tipos que não sejam compatíveis com CLS; por exemplo:

* Tipos de valor demarcado. O exemplo do C# a seguir cria uma classe que tem uma propriedade pública do tipo `int*` chamada `Value`. Como um `int*` é um tipo de valor demarcado, o compilador o sinaliza como incompatível com CLS.

```csharp
using System;

[assembly:CLSCompliant(true)]

public unsafe class TestClass
{
   private int* val;

   public TestClass(int number)
   {
      val = (int*) number;
   }

   public int* Value {
      get { return val; }
   }
}
// The compiler generates the following output when compiling this example:
//        warning CS3003: Type of 'TestClass.Value' is not CLS-compliant
```

* Referências de tipo, que são constructos especiais que contêm referência a um objeto e referência a um tipo.

Se um tipo não for compatível com CLS, você deverá aplicar o atributo [CLSCompliantAttribute](xref:System.CLSCompliantAttribute) com um parâmetro *isCompliant* com um valor de `false` a ele. Para obter mais informações, consulte a seção [CLSCompliantAttribute attribute](#the-clscompliantattribute-attribute).

O exemplo a seguir ilustra o problema de conformidade com CLS em uma assinatura do método e em uma instanciação de tipo genérico. Ele define uma classe `InvoiceItem` com uma propriedade do tipo [UInt32](xref:System.UInt32), uma propriedade do tipo [Nullable(Of UInt32)](xref:System.Nullable%601) e um construtor com parâmetros do tipo `UInt32` e `Nullable(Of UInt32)`. Você recebe quatro avisos do compilador ao tentar de compilar esse exemplo.

```csharp
using System;

[assembly: CLSCompliant(true)]

public class InvoiceItem
{
   private uint invId = 0;
   private uint itemId = 0;
   private Nullable<uint> qty;

   public InvoiceItem(uint sku, Nullable<uint> quantity)
   {
      itemId = sku;
      qty = quantity;
   }

   public Nullable<uint> Quantity
   {
      get { return qty; }
      set { qty = value; }
   }

   public uint InvoiceId
   {
      get { return invId; }
      set { invId = value; }
   }
}
// The attempt to compile the example displays the following output:
//    Type1.cs(13,23): warning CS3001: Argument type 'uint' is not CLS-compliant
//    Type1.cs(13,33): warning CS3001: Argument type 'uint?' is not CLS-compliant
//    Type1.cs(19,26): warning CS3003: Type of 'InvoiceItem.Quantity' is not CLS-compliant
//    Type1.cs(25,16): warning CS3003: Type of 'InvoiceItem.InvoiceId' is not CLS-compliant
```

```vb
<Assembly: CLSCompliant(True)>

Public Class InvoiceItem

   Private invId As UInteger = 0
   Private itemId As UInteger = 0
   Private qty AS Nullable(Of UInteger)

   Public Sub New(sku As UInteger, quantity As Nullable(Of UInteger))
      itemId = sku
      qty = quantity
   End Sub

   Public Property Quantity As Nullable(Of UInteger)
      Get
         Return qty
      End Get
      Set
         qty = value
      End Set
   End Property

   Public Property InvoiceId As UInteger
      Get
         Return invId
      End Get
      Set
         invId = value
      End Set
   End Property
End Class
' The attempt to compile the example displays output similar to the following:
'    Type1.vb(13) : warning BC40028: Type of parameter 'sku' is not CLS-compliant.
'
'       Public Sub New(sku As UInteger, quantity As Nullable(Of UInteger))
'                      ~~~
'    Type1.vb(13) : warning BC40041: Type 'UInteger' is not CLS-compliant.
'
'       Public Sub New(sku As UInteger, quantity As Nullable(Of UInteger))
'                                                               ~~~~~~~~
'    Type1.vb(18) : warning BC40041: Type 'UInteger' is not CLS-compliant.
'
'       Public Property Quantity As Nullable(Of UInteger)
'                                               ~~~~~~~~
'    Type1.vb(27) : warning BC40027: Return type of function 'InvoiceId' is not CLS-compliant.
'
'       Public Property InvoiceId As UInteger
```

Para eliminar os avisos do compilador, substitua os tipos não compatíveis com CLS na interface pública `InvoiceItem` por tipos compatíveis:

```csharp
using System;

[assembly: CLSCompliant(true)]

public class InvoiceItem
{
   private uint invId = 0;
   private uint itemId = 0;
   private Nullable<int> qty;

   public InvoiceItem(int sku, Nullable<int> quantity)
   {
      if (sku <= 0)
         throw new ArgumentOutOfRangeException("The item number is zero or negative.");
      itemId = (uint) sku;

      qty = quantity;
   }

   public Nullable<int> Quantity
   {
      get { return qty; }
      set { qty = value; }
   }

   public int InvoiceId
   {
      get { return (int) invId; }
      set {
         if (value <= 0)
            throw new ArgumentOutOfRangeException("The invoice number is zero or negative.");
         invId = (uint) value; }
   }
}
```

```vb
Assembly: CLSCompliant(True)>

Public Class InvoiceItem

   Private invId As UInteger = 0
   Private itemId As UInteger = 0
   Private qty AS Nullable(Of Integer)

   Public Sub New(sku As Integer, quantity As Nullable(Of Integer))
      If sku <= 0 Then
         Throw New ArgumentOutOfRangeException("The item number is zero or negative.")
      End If
      itemId = CUInt(sku)
      qty = quantity
   End Sub

   Public Property Quantity As Nullable(Of Integer)
      Get
         Return qty
      End Get
      Set
         qty = value
      End Set
   End Property

   Public Property InvoiceId As Integer
      Get
         Return CInt(invId)
      End Get
      Set
         invId = CUInt(value)
      End Set
   End Property
End Class
```

Além dos tipos específicos listados, algumas categorias de tipos não são compatíveis com CLS. Entre eles estão tipos de ponteiro não gerenciados e tipos de ponteiro de função. O exemplo a seguir gera um aviso do compilador porque ele usa um ponteiro para um inteiro a fim de criar uma matriz de inteiros.

```csharp
using System;

[assembly: CLSCompliant(true)]

public class ArrayHelper
{
   unsafe public static Array CreateInstance(Type type, int* ptr, int items)
   {
      Array arr = Array.CreateInstance(type, items);
      int* addr = ptr;
      for (int ctr = 0; ctr < items; ctr++) {
          int value = *addr;
          arr.SetValue(value, ctr);
          addr++;
      }
      return arr;
   }
}
// The attempt to compile this example displays the following output:
//    UnmanagedPtr1.cs(8,57): warning CS3001: Argument type 'int*' is not CLS-compliant
```

```csharp
using System;

[assembly: CLSCompliant(true)]

public class ArrayHelper
{
   unsafe public static Array CreateInstance(Type type, int* ptr, int items)
   {
      Array arr = Array.CreateInstance(type, items);
      int* addr = ptr;
      for (int ctr = 0; ctr < items; ctr++) {
          int value = *addr;
          arr.SetValue(value, ctr);
          addr++;
      }
      return arr;
   }
}
// The attempt to compile this example displays the following output:
//    UnmanagedPtr1.cs(8,57): warning CS3001: Argument type 'int*' is not CLS-compliant
```

Para classes abstratas em conformidade com CLS (ou seja, classes marcadas como `abstract` no C#), todos os membros da classe também devem estar em conformidade com CLS.

### <a name="naming-conventions"></a>Convenções de nomenclatura

Como algumas linguagens de programação não diferenciam maiúsculas de minúsculas, os identificadores (como nomes de namespaces, tipos e membros) devem se diferenciar além de maiúsculas e minúsculas. Dois identificadores serão considerados equivalentes se seus mapeamentos em minúsculas forem os mesmos. O exemplo do C# a seguir define duas classes públicas, `Person` e `person`. Como elas são diferentes apenas em maiúsculas e minúsculas, o compilador do C# as sinaliza como não compatíveis com CLS.

```csharp
using System;

[assembly: CLSCompliant(true)]

public class Person : person
{

}

public class person
{

}
// Compilation produces a compiler warning like the following:
//    Naming1.cs(11,14): warning CS3005: Identifier 'person' differing
//                       only in case is not CLS-compliant
//    Naming1.cs(6,14): (Location of symbol related to previous warning)
```

Os identificadores de linguagem de programação, como os nomes de namespaces, tipos e membros, devem estar em conformidade com o [padrão Unicode](https://unicode.org/reports/tr15/). Isso significa que:

* O primeiro caractere de um identificador pode ser qualquer letra maiúscula Unicode, letra minúscula, letra maiúscula do título, letra modificadora, outra letra ou o número da letra. Para obter informações sobre categorias de caractere Unicode, consulte a enumeração [System.Globalization.UnicodeCategory](xref:System.Globalization.UnicodeCategory).

* Os caracteres subsequentes podem ser de qualquer uma das categorias como o primeiro caractere e também podem incluir marcas sem espaçamento, marcas de espaçamento, números decimais, Pontuação do conector e códigos de formatação.

Antes de comparar identificadores, você deve filtrar códigos de formatação e converter os identificadores em Formulário C de Normalização de Unicode, porque um caractere único pode ser representado por várias unidades de código codificadas em UTF-16. As sequências de caracteres que produzem as mesmas unidades de código no Formulário C de Normalização de Unicode não são compatíveis com CLS. O exemplo a seguir define uma propriedade chamada `Å`, que consiste no caractere SÍMBOLO DE ANGSTROM (U+212B) e uma segunda propriedade chamada `Å`, que consiste no caractere LETRA LATINA MAIÚSCULA A COM ANEL SUPERIOR (U+00C5). O compilador do C# sinaliza o código-fonte como não compatível com CLS.

```csharp
public class Size
{
   private double a1;
   private double a2;

   public double Å
   {
       get { return a1; }
       set { a1 = value; }
   }

   public double Å
   {
       get { return a2; }
       set { a2 = value; }
   }
}
// Compilation produces a compiler warning like the following:
//    Naming2a.cs(16,18): warning CS3005: Identifier 'Size.Å' differing only in case is not
//            CLS-compliant
//    Naming2a.cs(10,18): (Location of symbol related to previous warning)
//    Naming2a.cs(18,8): warning CS3005: Identifier 'Size.Å.get' differing only in case is not
//            CLS-compliant
//    Naming2a.cs(12,8): (Location of symbol related to previous warning)
//    Naming2a.cs(19,8): warning CS3005: Identifier 'Size.Å.set' differing only in case is not
//            CLS-compliant
//    Naming2a.cs(13,8): (Location of symbol related to previous warning)
```

```vb
<Assembly: CLSCompliant(True)>
Public Class Size
   Private a1 As Double
   Private a2 As Double

   Public Property Å As Double
       Get
          Return a1
       End Get
       Set
          a1 = value
       End Set
   End Property

   Public Property Å As Double
       Get
          Return a2
       End Get
       Set
          a2 = value
       End Set
   End Property
End Class
' Compilation produces a compiler warning like the following:
'    Naming1.vb(9) : error BC30269: 'Public Property Å As Double' has multiple definitions
'     with identical signatures.
'
'       Public Property Å As Double
'                       ~
```

Os nomes de membro em um escopo específico (como os namespaces em um assembly, os tipos em um namespace ou os membros em um tipo) devem ser exclusivos, exceto os nomes resolvidos por meio de sobrecarga. Esse requisito é mais rígido do que o do Common Type System, que permite que vários membros em um escopo tenham nomes idênticos desde que sejam tipos diferentes de membros (por exemplo, um é um método e outro é um campo). Em particular, para membros de tipo:

* Campos e tipos aninhados são diferenciados apenas por nome.

* Métodos, propriedades e eventos que tenham o mesmo nome devem ser diferentes além apenas do tipo de retorno.

O exemplo a seguir ilustra o requisito de que nomes de membros devem ser exclusivos dentro de seu escopo. Ele define uma classe chamada `Converter` que inclui quatro membros chamados `Conversion`. Três são métodos e um é uma propriedade. O método que inclui um parâmetro `Int64` tem um nome exclusivo, mas os dois métodos com um parâmetro `Int32` não têm, porque o valor retornado não é considerado parte da assinatura de um membro. A propriedade `Conversion` também viola esse requisito porque as propriedades não podem ter o mesmo nome dos métodos sobrecarregados.

```csharp
using System;

[assembly: CLSCompliant(true)]

public class Converter
{
   public double Conversion(int number)
   {
      return (double) number;
   }

   public float Conversion(int number)
   {
      return (float) number;
   }

   public double Conversion(long number)
   {
      return (double) number;
   }

   public bool Conversion
   {
      get { return true; }
   }
}
// Compilation produces a compiler error like the following:
//    Naming3.cs(13,17): error CS0111: Type 'Converter' already defines a member called
//            'Conversion' with the same parameter types
//    Naming3.cs(8,18): (Location of symbol related to previous error)
//    Naming3.cs(23,16): error CS0102: The type 'Converter' already contains a definition for
//            'Conversion'
//    Naming3.cs(8,18): (Location of symbol related to previous error)
```

```vb
<Assembly: CLSCompliant(True)>

Public Class Converter
   Public Function Conversion(number As Integer) As Double
      Return CDbl(number)
   End Function

   Public Function Conversion(number As Integer) As Single
      Return CSng(number)
   End Function

   Public Function Conversion(number As Long) As Double
      Return CDbl(number)
   End Function

   Public ReadOnly Property Conversion As Boolean
      Get
         Return True
      End Get
   End Property
End Class
' Compilation produces a compiler error like the following:
'    Naming3.vb(8) : error BC30301: 'Public Function Conversion(number As Integer) As Double'
'                    and 'Public Function Conversion(number As Integer) As Single' cannot
'                    overload each other because they differ only by return types.
'
'       Public Function Conversion(number As Integer) As Double
'                       ~~~~~~~~~~
'    Naming3.vb(20) : error BC30260: 'Conversion' is already declared as 'Public Function
'                     Conversion(number As Integer) As Single' in this class.
'
'       Public ReadOnly Property Conversion As Boolean
'                                ~~~~~~~~~~
```

Linguagens individuais incluem palavras-chave exclusivas, portanto, linguagens que apontam para o Common Language Runtime também devem oferecer algum mecanismo para fazer referência a identificadores (como nomes de tipo) que coincidam com palavras-chave. Por exemplo, `case` é uma palavra-chave no C# e no Visual Basic. No entanto, o exemplo do Visual Basic a seguir pode remover a ambiguidade de uma classe chamada `case` da palavra-chave `case`, usando chaves de abertura e fechamento. Caso contrário, o exemplo produziria a mensagem de erro "A palavra-chave não é válida como um identificador", e não seria compilado.

```vb
Public Class [case]
   Private _id As Guid
   Private name As String

   Public Sub New(name As String)
      _id = Guid.NewGuid()
      Me.name = name
   End Sub

   Public ReadOnly Property ClientName As String
      Get
         Return name
      End Get
   End Property
End Class
```

O exemplo do C# a seguir pode criar uma instância da classe `case` usando o símbolo @ para remover a ambiguidade do identificador da palavra-chave da linguagem. Sem ele, o compilador do C# exibiria duas mensagens de erro, "Tipo esperado" e "'Maiúsculas e minúsculas' do termo de expressão inválido".

```csharp
using System;

public class Example
{
   public static void Main()
   {
      @case c = new @case("John");
      Console.WriteLine(c.ClientName);
   }
}
```

### <a name="type-conversion"></a>Conversão de tipos

A Common Language Specification define dois operadores de conversão:

* `op_Implicit`, que é usado para conversões de ampliação que não resultam em perda de dados ou precisão. Por exemplo, a estrutura [Decimal](xref:System.Decimal) inclui um operador `op_Implicit` sobrecarregado para converter valores de tipos integrais e valores [Char](xref:System.Char) em valores `Decimal`.

* `op_Explicit`, que é usado para conversões de redução que possam resultar em perda de magnitude (um valor é convertido em um valor com um intervalo menor) ou precisão. Por exemplo, a estrutura `Decimal` inclui um operador `op_Explicit` sobrecarregado para converter valores [Double](xref:System.Double) e [Single](xref:System.Single) em `Decimal` e para converter valores `Decimal` em valores inteiros, `Double`, `Single` e `Char`.

No entanto, nem todas as linguagens dão suporte à sobrecarga de operador ou à definição de operadores personalizados. Se optar por implementar esses operadores de conversão, você também deverá fornecer uma maneira alternativa para realizar a conversão. Recomendamos que você forneça métodos `From`Xxx e `To`Xxx.

O exemplo a seguir define conversões explícitas e implícitas em conformidade com CLS. Ele cria uma `UDouble` classe que representa um número de ponto flutuante de precisão dupla assinado. Ele fornece conversões implícitas de `UDouble` em `Double` e conversões explícitas de `UDouble` em `Single`, de `Double` em `UDouble` e de `Single` em `UDouble`. Ele também define um método `ToDouble` como uma alternativa ao operador de conversão implícita e os métodos `ToSingle`, `FromDouble` e `FromSingle` como alternativas aos operadores de conversão explícita.

```csharp
using System;

public struct UDouble
{
   private double number;

   public UDouble(double value)
   {
      if (value < 0)
         throw new InvalidCastException("A negative value cannot be converted to a UDouble.");

      number = value;
   }

   public UDouble(float value)
   {
      if (value < 0)
         throw new InvalidCastException("A negative value cannot be converted to a UDouble.");

      number = value;
   }

   public static readonly UDouble MinValue = (UDouble) 0.0;
   public static readonly UDouble MaxValue = (UDouble) Double.MaxValue;

   public static explicit operator Double(UDouble value)
   {
      return value.number;
   }

   public static implicit operator Single(UDouble value)
   {
      if (value.number > (double) Single.MaxValue)
         throw new InvalidCastException("A UDouble value is out of range of the Single type.");

      return (float) value.number;
   }

   public static explicit operator UDouble(double value)
   {
      if (value < 0)
         throw new InvalidCastException("A negative value cannot be converted to a UDouble.");

      return new UDouble(value);
   }

   public static implicit operator UDouble(float value)
   {
      if (value < 0)
         throw new InvalidCastException("A negative value cannot be converted to a UDouble.");

      return new UDouble(value);
   }

   public static Double ToDouble(UDouble value)
   {
      return (Double) value;
   }

   public static float ToSingle(UDouble value)
   {
      return (float) value;
   }

   public static UDouble FromDouble(double value)
   {
      return new UDouble(value);
   }

   public static UDouble FromSingle(float value)
   {
      return new UDouble(value);
   }
}
```

```vb
Public Structure UDouble
   Private number As Double

   Public Sub New(value As Double)
      If value < 0 Then
         Throw New InvalidCastException("A negative value cannot be converted to a UDouble.")
      End If
      number = value
   End Sub

   Public Sub New(value As Single)
      If value < 0 Then
         Throw New InvalidCastException("A negative value cannot be converted to a UDouble.")
      End If
      number = value
   End Sub

   Public Shared ReadOnly MinValue As UDouble = CType(0.0, UDouble)
   Public Shared ReadOnly MaxValue As UDouble = Double.MaxValue

   Public Shared Widening Operator CType(value As UDouble) As Double
      Return value.number
   End Operator

   Public Shared Narrowing Operator CType(value As UDouble) As Single
      If value.number > CDbl(Single.MaxValue) Then
         Throw New InvalidCastException("A UDouble value is out of range of the Single type.")
      End If
      Return CSng(value.number)
   End Operator

   Public Shared Narrowing Operator CType(value As Double) As UDouble
      If value < 0 Then
         Throw New InvalidCastException("A negative value cannot be converted to a UDouble.")
      End If
      Return New UDouble(value)
   End Operator

   Public Shared Narrowing Operator CType(value As Single) As UDouble
      If value < 0 Then
         Throw New InvalidCastException("A negative value cannot be converted to a UDouble.")
      End If
      Return New UDouble(value)
   End Operator

   Public Shared Function ToDouble(value As UDouble) As Double
      Return CType(value, Double)
   End Function

   Public Shared Function ToSingle(value As UDouble) As Single
      Return CType(value, Single)
   End Function

   Public Shared Function FromDouble(value As Double) As UDouble
      Return New UDouble(value)
   End Function

   Public Shared Function FromSingle(value As Single) As UDouble
      Return New UDouble(value)
   End Function
End Structure
```

### <a name="arrays"></a>Matrizes

As matrizes compatíveis com CLS estão em conformidade com as seguintes regras:

* Todas as dimensões de uma matriz devem ter um limite inferior igual a zero. O exemplo a seguir cria uma matriz não compatível com CLS com um limite inferior de um. Apesar da presença do atributo [CLSCompliantAttribute](xref:System.CLSCompliantAttribute) , o compilador não detecta que a matriz retornada pelo `Numbers.GetTenPrimes` método não é compatível com CLS.

  ```csharp
  [assembly: CLSCompliant(true)]

  public class Numbers
  {
    public static Array GetTenPrimes()
    {
        Array arr = Array.CreateInstance(typeof(Int32), new int[] {10}, new int[] {1});
        arr.SetValue(1, 1);
        arr.SetValue(2, 2);
        arr.SetValue(3, 3);
        arr.SetValue(5, 4);
        arr.SetValue(7, 5);
        arr.SetValue(11, 6);
        arr.SetValue(13, 7);
        arr.SetValue(17, 8);
        arr.SetValue(19, 9);
        arr.SetValue(23, 10);

        return arr;
    }
  }
  ```

  ```vb
  <Assembly: CLSCompliant(True)>

  Public Class Numbers
     Public Shared Function GetTenPrimes() As Array
        Dim arr As Array = Array.CreateInstance(GetType(Int32), {10}, {1})
        arr.SetValue(1, 1)
        arr.SetValue(2, 2)
        arr.SetValue(3, 3)
        arr.SetValue(5, 4)
        arr.SetValue(7, 5)
        arr.SetValue(11, 6)
        arr.SetValue(13, 7)
        arr.SetValue(17, 8)
        arr.SetValue(19, 9)
        arr.SetValue(23, 10)
        Return arr
     End Function
  End Class
  ```

* Todos os elementos de matriz devem consistir em tipos compatíveis com CLS. O exemplo a seguir define dois métodos que retornam matrizes não em conformidade com CLS. O primeiro retorna uma matriz de valores [UInt32](xref:System.UInt32). O segundo retorna uma matriz [Object](xref:System.Object) que inclui valores [Int32](xref:System.Int32) e `UInt32`. Embora o compilador identifique a primeira matriz como não em conformidade devido ao seu tipo `UInt32`, ele não reconhece que a segunda matriz inclui elementos não em conformidade com CLS.

  ```csharp
  using System;

  [assembly: CLSCompliant(true)]

  public class Numbers
  {
    public static UInt32[] GetTenPrimes()
    {
        uint[] arr = { 1u, 2u, 3u, 5u, 7u, 11u, 13u, 17u, 19u };
        return arr;
    }

    public static Object[] GetFivePrimes()
    {
        Object[] arr = { 1, 2, 3, 5u, 7u };
        return arr;
    }
  }
  // Compilation produces a compiler warning like the following:
  //    Array2.cs(8,27): warning CS3002: Return type of 'Numbers.GetTenPrimes()' is not
  //            CLS-compliant
  ```

  ```vb
  <Assembly: CLSCompliant(True)>

  Public Class Numbers
     Public Shared Function GetTenPrimes() As UInt32()
        Return { 1ui, 2ui, 3ui, 5ui, 7ui, 11ui, 13ui, 17ui, 19ui }
     End Function
     Public Shared Function GetFivePrimes() As Object()
        Dim arr() As Object = { 1, 2, 3, 5ui, 7ui }
        Return arr
     End Function
  End Class
  ' Compilation produces a compiler warning like the following:
  '    warning BC40027: Return type of function 'GetTenPrimes' is not CLS-compliant.
  ```

* A resolução de sobrecarga para métodos que tenham parâmetros de matriz se baseia no fato de que são matrizes e em seu tipo de elemento. Por esse motivo, a seguinte definição de um método `GetSquares` sobrecarregado é compatível com CLS.

  ```csharp
  using System;
  using System.Numerics;

  [assembly: CLSCompliant(true)]

  public class Numbers
  {
    public static byte[] GetSquares(byte[] numbers)
    {
        byte[] numbersOut = new byte[numbers.Length];
        for (int ctr = 0; ctr < numbers.Length; ctr++) {
            int square = ((int) numbers[ctr]) * ((int) numbers[ctr]);
            if (square <= Byte.MaxValue)
                numbersOut[ctr] = (byte) square;
            // If there's an overflow, assign MaxValue to the corresponding
            // element.
            else
                numbersOut[ctr] = Byte.MaxValue;

        }
        return numbersOut;
    }

    public static BigInteger[] GetSquares(BigInteger[] numbers)
  {
        BigInteger[] numbersOut = new BigInteger[numbers.Length];
        for (int ctr = 0; ctr < numbers.Length; ctr++)
            numbersOut[ctr] = numbers[ctr] * numbers[ctr];

       return numbersOut;
    }
  }
  ```

  ```vb
  Imports System.Numerics

  <Assembly: CLSCompliant(True)>

  Public Module Numbers
     Public Function GetSquares(numbers As Byte()) As Byte()
        Dim numbersOut(numbers.Length - 1) As Byte
        For ctr As Integer = 0 To numbers.Length - 1
           Dim square As Integer = (CInt(numbers(ctr)) * CInt(numbers(ctr)))
           If square <= Byte.MaxValue Then
              numbersOut(ctr) = CByte(square)
           ' If there's an overflow, assign MaxValue to the corresponding
           ' element.
           Else
              numbersOut(ctr) = Byte.MaxValue
           End If
        Next
        Return numbersOut
     End Function

     Public Function GetSquares(numbers As BigInteger()) As BigInteger()
         Dim numbersOut(numbers.Length - 1) As BigInteger
         For ctr As Integer = 0 To numbers.Length - 1
            numbersOut(ctr) = numbers(ctr) * numbers(ctr)
         Next
         Return numbersOut
     End Function
  End Module
  ```

### <a name="interfaces"></a>Interfaces

Interfaces compatíveis com CLS podem definir propriedades, eventos e métodos virtuais (métodos sem implementação). Uma interface compatível com CLS não pode ter nenhum dos seguintes itens:

* Métodos estáticos ou campos estáticos. O compilador do C# gerará erros de compilador se você definir um membro estático em uma interface.

* Campos. O compilador do C# gerará erros de compilador se você definir um campo em uma interface.

* Métodos que não são compatíveis com CLS. Por exemplo, a definição a seguir da interface inclui um método, `INumber.GetUnsigned`, que está marcado como não compatível com CLS. Este exemplo gera um aviso do compilador.

  ```csharp
  using System;

  [assembly:CLSCompliant(true)]

  public interface INumber
  {
      int Length();
      [CLSCompliant(false)] ulong GetUnsigned();
  }
  // Attempting to compile the example displays output like the following:
  //    Interface2.cs(8,32): warning CS3010: 'INumber.GetUnsigned()': CLS-compliant interfaces
  //            must have only CLS-compliant members
  ```

  ```vb
  <Assembly: CLSCompliant(True)>

  Public Interface INumber
    Function Length As Integer
      <CLSCompliant(False)> Function GetUnsigned As ULong
    End Interface
    ' Attempting to compile the example displays output like the following:
    '    Interface2.vb(9) : warning BC40033: Non CLS-compliant 'function' is not allowed in a
    '    CLS-compliant interface.
    '
    '       <CLSCompliant(False)> Function GetUnsigned As ULong
    '                                      ~~~~~~~~~~~
  ```

  Devido a essa regra, os tipos compatíveis com CLS não são necessários para implementar membros não compatíveis com CLS. Se uma estrutura compatível com CLS expuser uma classe que implementa uma interface não compatível com CLS, ela também deverá fornecer implementações concretas de todos os membros não compatíveis com CLS.

Compiladores de linguagem compatíveis com CLS também devem permitir que uma classe forneça implementações separadas dos membros com o mesmo nome e a assinatura em várias interfaces. O C# dá suporte a implementações explícitas de interface para fornecer implementações diferentes de métodos com nomes idênticos. O exemplo a seguir ilustra esse cenário, definindo uma classe `Temperature` que implementa as interfaces `ICelsius` e `IFahrenheit` como implementações explícitas de interface.

```csharp
using System;

[assembly: CLSCompliant(true)]

public interface IFahrenheit
{
   decimal GetTemperature();
}

public interface ICelsius
{
   decimal GetTemperature();
}

public class Temperature : ICelsius, IFahrenheit
{
   private decimal _value;

   public Temperature(decimal value)
   {
      // We assume that this is the Celsius value.
      _value = value;
   }

   decimal IFahrenheit.GetTemperature()
   {
      return _value * 9 / 5 + 32;
   }

   decimal ICelsius.GetTemperature()
   {
      return _value;
   }
}
public class Example
{
   public static void Main()
   {
      Temperature temp = new Temperature(100.0m);
      ICelsius cTemp = temp;
      IFahrenheit fTemp = temp;
      Console.WriteLine("Temperature in Celsius: {0} degrees",
                        cTemp.GetTemperature());
      Console.WriteLine("Temperature in Fahrenheit: {0} degrees",
                        fTemp.GetTemperature());
   }
}
// The example displays the following output:
//       Temperature in Celsius: 100.0 degrees
//       Temperature in Fahrenheit: 212.0 degrees
```

```vb
Assembly: CLSCompliant(True)>

Public Interface IFahrenheit
   Function GetTemperature() As Decimal
End Interface

Public Interface ICelsius
   Function GetTemperature() As Decimal
End Interface

Public Class Temperature : Implements ICelsius, IFahrenheit
   Private _value As Decimal

   Public Sub New(value As Decimal)
      ' We assume that this is the Celsius value.
      _value = value
   End Sub

   Public Function GetFahrenheit() As Decimal _
          Implements IFahrenheit.GetTemperature
      Return _value * 9 / 5 + 32
   End Function

   Public Function GetCelsius() As Decimal _
          Implements ICelsius.GetTemperature
      Return _value
   End Function
End Class

Module Example
   Public Sub Main()
      Dim temp As New Temperature(100.0d)
      Console.WriteLine("Temperature in Celsius: {0} degrees",
                        temp.GetCelsius())
      Console.WriteLine("Temperature in Fahrenheit: {0} degrees",
                        temp.GetFahrenheit())
   End Sub
End Module
' The example displays the following output:
'       Temperature in Celsius: 100.0 degrees
'       Temperature in Fahrenheit: 212.0 degrees
```

### <a name="enumerations"></a>Enumerações

Enumerações compatíveis com CLS devem seguir estas regras:

* O tipo subjacente da enumeração deve ser um inteiro intrínseco compatível com CLS ([Byte](xref:System.Byte), [Int16](xref:System.Int16), [Int32](xref:System.Int32) ou [Int64](xref:System.Int64)). Por exemplo, o código a seguir tenta definir uma enumeração cujo tipo subjacente é [UInt32](xref:System.UInt32) e gera um aviso do compilador.

    ```csharp
    using System;

    [assembly: CLSCompliant(true)]

    public enum Size : uint {
        Unspecified = 0,
        XSmall = 1,
        Small = 2,
        Medium = 3,
        Large = 4,
        XLarge = 5
    };

    public class Clothing
    {
        public string Name;
        public string Type;
        public string Size;
    }
    // The attempt to compile the example displays a compiler warning like the following:
    //    Enum3.cs(6,13): warning CS3009: 'Size': base type 'uint' is not CLS-compliant
    ```

    ```vb
    <Assembly: CLSCompliant(True)>

    Public Enum Size As UInt32
       Unspecified = 0
       XSmall = 1
       Small = 2
       Medium = 3
       Large = 4
       XLarge = 5
    End Enum

    Public Class Clothing
       Public Name As String
       Public Type As String
       Public Size As Size
    End Class
    ' The attempt to compile the example displays a compiler warning like the following:
    '    Enum3.vb(6) : warning BC40032: Underlying type 'UInt32' of Enum is not CLS-compliant.
    '
    '    Public Enum Size As UInt32
    '                ~~~~
    ```

* Um tipo de enumeração deve ter um campo de instância única chamado `Value__` que foi marcado com o atributo `FieldAttributes.RTSpecialName`. Isso permite que você referencie o valor do campo implicitamente.

* Uma enumeração inclui campos estáticos literais, cujos tipos correspondem ao tipo da própria enumeração. Por exemplo, se você definir uma enumeração `State` com valores de `State.On` e `State.Off`, `State.On` e `State.Off` serão campos literais estáticos cujo tipo será `State`.

* Há dois tipos de enumeração:

  * Uma enumeração que representa um conjunto de valores mutuamente excludentes, valores inteiros nomeados. Esse tipo de enumeração é indicado pela ausência do atributo personalizado [System.FlagsAttribute](xref:System.FlagsAttribute).

  * Uma enumeração que representa um conjunto de sinalizadores de bit que podem ser combinados para produzir um valor sem nome. Esse tipo de enumeração é indicado pela presença do atributo personalizado [System.FlagsAttribute](xref:System.FlagsAttribute).

Para obter mais informações, consulte a documentação da estrutura [Enum](xref:System.Enum).

* O valor de uma enumeração não está limitado ao intervalo de seus valores especificados. Em outras palavras, o intervalo de valores em uma enumeração é o intervalo de seu valor subjacente. Você pode usar o método `Enum.IsDefined` para determinar se um valor especificado é membro de uma enumeração.

### <a name="type-members-in-general"></a>Membros de tipo em geral

O Common Language Specification requer que todos os campos e métodos sejam acessados como membros de uma classe específica. Portanto, campos e métodos estáticos globais (ou seja, campos ou métodos estáticos que são definidos independentemente de um tipo) não são compatíveis com CLS. Se você tentar incluir um campo ou método global no código-fonte, os compiladores do C# gerarão um erro de compilador.

A Common Language Specification dá suporte somente à convenção de chamada gerenciada padrão. Ela não dá suporte a convenções e métodos de chamada não gerenciados com listas de argumentos de variável marcadas com a palavra-chave `varargs`. Para listas de argumentos de variável que são compatíveis com a convenção de chamada gerenciada padrão, use o atributo [ParamArrayAttribute](xref:System.ParamArrayAttribute) ou a implementação da linguagem individual, como a palavra-chave `params` em C# e a palavra-chave `ParamArray` em Visual Basic.

### <a name="member-accessibility"></a>Acessibilidade de membro

A substituição de um membro herdado não pode alterar a acessibilidade desse membro. Por exemplo, um método público em uma classe base não pode ser substituído por um método privado em uma classe derivada. Há uma exceção: um membro `protected internal` (em C#) ou `Protected Friend` (em Visual Basic) em um assembly que é substituído por um tipo em um assembly diferente.  Nesse caso, a acessibilidade da substituição é `Protected`.

O exemplo a seguir ilustra o erro que é gerado quando o atributo [CLSCompliantAttribute](xref:System.CLSCompliantAttribute) é definido como `true` e `Person`, que é uma classe derivada de `Animal`, tenta alterar a acessibilidade da propriedade `Species` de pública para privada. O exemplo será compilado com êxito se sua acessibilidade for alterada para pública.

```csharp
using System;

[assembly: CLSCompliant(true)]

public class Animal
{
   private string _species;

   public Animal(string species)
   {
      _species = species;
   }

   public virtual string Species
   {
      get { return _species; }
   }

   public override string ToString()
   {
      return _species;
   }
}

public class Human : Animal
{
   private string _name;

   public Human(string name) : base("Homo Sapiens")
   {
      _name = name;
   }

   public string Name
   {
      get { return _name; }
   }

   private override string Species
   {
      get { return base.Species; }
   }

   public override string ToString()
   {
      return _name;
   }
}

public class Example
{
   public static void Main()
   {
      Human p = new Human("John");
      Console.WriteLine(p.Species);
      Console.WriteLine(p.ToString());
   }
}
// The example displays the following output:
//    error CS0621: 'Human.Species': virtual or abstract members cannot be private
```

```vb
<Assembly: CLSCompliant(True)>

Public Class Animal
   Private _species As String

   Public Sub New(species As String)
      _species = species
   End Sub

   Public Overridable ReadOnly Property Species As String
      Get
         Return _species
      End Get
   End Property

   Public Overrides Function ToString() As String
      Return _species
   End Function
End Class

Public Class Human : Inherits Animal
   Private _name As String

   Public Sub New(name As String)
      MyBase.New("Homo Sapiens")
      _name = name
   End Sub

   Public ReadOnly Property Name As String
      Get
         Return _name
      End Get
   End Property

   Private Overrides ReadOnly Property Species As String
      Get
         Return MyBase.Species
      End Get
   End Property

   Public Overrides Function ToString() As String
      Return _name
   End Function
End Class

Public Module Example
   Public Sub Main()
      Dim p As New Human("John")
      Console.WriteLine(p.Species)
      Console.WriteLine(p.ToString())
   End Sub
End Module
' The example displays the following output:
'     'Private Overrides ReadOnly Property Species As String' cannot override
'     'Public Overridable ReadOnly Property Species As String' because
'      they have different access levels.
'
'         Private Overrides ReadOnly Property Species As String
```

Os tipos na assinatura de um membro deverão ser acessíveis sempre que o membro for acessível. Por exemplo, isso significa que um membro público não pode incluir um parâmetro cujo tipo seja privado, protegido ou interno. O exemplo a seguir ilustra o erro do compilador que resulta quando um construtor de classe `StringWrapper` expõe um valor de enumeração `StringOperationType` interno que determina como um valor de cadeia de caracteres deve ser encapsulado.

```csharp
using System;
using System.Text;

public class StringWrapper
{
   string internalString;
   StringBuilder internalSB = null;
   bool useSB = false;

   public StringWrapper(StringOperationType type)
   {
      if (type == StringOperationType.Normal) {
         useSB = false;
      }
      else {
         useSB = true;
         internalSB = new StringBuilder();
      }
   }

   // The remaining source code...
}

internal enum StringOperationType { Normal, Dynamic }
// The attempt to compile the example displays the following output:
//    error CS0051: Inconsistent accessibility: parameter type
//            'StringOperationType' is less accessible than method
//            'StringWrapper.StringWrapper(StringOperationType)'
```

```vb
Imports System.Text

<Assembly:CLSCompliant(True)>

Public Class StringWrapper

   Dim internalString As String
   Dim internalSB As StringBuilder = Nothing
   Dim useSB As Boolean = False

   Public Sub New(type As StringOperationType)
      If type = StringOperationType.Normal Then
         useSB = False
      Else
         internalSB = New StringBuilder()
         useSB = True
      End If
   End Sub

   ' The remaining source code...
End Class

Friend Enum StringOperationType As Integer
   Normal = 0
   Dynamic = 1
End Enum
' The attempt to compile the example displays the following output:
'    error BC30909: 'type' cannot expose type 'StringOperationType'
'     outside the project through class 'StringWrapper'.
'
'       Public Sub New(type As StringOperationType)
'                              ~~~~~~~~~~~~~~~~~~~
```

### <a name="generic-types-and-members"></a>Tipos e membros genéricos

Tipos aninhados sempre têm pelo menos o mesmo número de parâmetros genéricos do tipo delimitador. Eles correspondem por posição aos parâmetros genéricos no tipo delimitador. O tipo genérico também pode incluir novos parâmetros genéricos.

A relação entre os parâmetros de tipo genérico de um tipo de contenção e seus tipos aninhados pode ser ocultada pela sintaxe de linguagens individuais. No exemplo a seguir, um tipo genérico `Outer<T>` contém duas classes aninhadas, `Inner1A` e `Inner1B<U>`. As chamadas para o método `ToString`, que cada classe herda de `Object.ToString`, mostram que cada classe aninhada inclui parâmetros de tipo de sua classe de contenção.

```csharp
using System;

[assembly:CLSCompliant(true)]

public class Outer<T>
{
   T value;

   public Outer(T value)
   {
      this.value = value;
   }

   public class Inner1A : Outer<T>
   {
      public Inner1A(T value) : base(value)
      {  }
   }

   public class Inner1B<U> : Outer<T>
   {
      U value2;

      public Inner1B(T value1, U value2) : base(value1)
      {
         this.value2 = value2;
      }
   }
}

public class Example
{
   public static void Main()
   {
      var inst1 = new Outer<String>("This");
      Console.WriteLine(inst1);

      var inst2 = new Outer<String>.Inner1A("Another");
      Console.WriteLine(inst2);

      var inst3 = new Outer<String>.Inner1B<int>("That", 2);
      Console.WriteLine(inst3);
   }
}
// The example displays the following output:
//       Outer`1[System.String]
//       Outer`1+Inner1A[System.String]
//       Outer`1+Inner1B`1[System.String,System.Int32]
```

```vb
<Assembly:CLSCompliant(True)>

Public Class Outer(Of T)
   Dim value As T

   Public Sub New(value As T)
      Me.value = value
   End Sub

   Public Class Inner1A : Inherits Outer(Of T)
      Public Sub New(value As T)
         MyBase.New(value)
      End Sub
   End Class

   Public Class Inner1B(Of U) : Inherits Outer(Of T)
      Dim value2 As U

      Public Sub New(value1 As T, value2 As U)
         MyBase.New(value1)
         Me.value2 = value2
      End Sub
   End Class
End Class

Public Module Example
   Public Sub Main()
      Dim inst1 As New Outer(Of String)("This")
      Console.WriteLine(inst1)

      Dim inst2 As New Outer(Of String).Inner1A("Another")
      Console.WriteLine(inst2)

      Dim inst3 As New Outer(Of String).Inner1B(Of Integer)("That", 2)
      Console.WriteLine(inst3)
   End Sub
End Module
' The example displays the following output:
'       Outer`1[System.String]
'       Outer`1+Inner1A[System.String]
'       Outer`1+Inner1B`1[System.String,System.Int32]
```

Os nomes de tipo genérico são codificados no formato *name*'*n*, em que *name* é o nome do tipo, *`* é um literal de caractere e *n* é o número de parâmetros declarados no tipo ou, para tipos genéricos aninhados, o número de parâmetros de tipo recém-introduzidos. Essa codificação de nomes de tipo genéricos é principalmente de interesse de desenvolvedores que usam a reflexão para acessar tipos genéricos compatíveis com CLS em uma biblioteca.

Se as restrições forem aplicadas a um tipo genérico, qualquer tipo usado como restrição também deverá ser compatível com CLS. O exemplo a seguir define uma classe chamada `BaseClass` que não está em conformidade com CLS e uma classe genérica chamada `BaseCollection` cujo parâmetro de tipo deve derivar de `BaseClass`. Mas como `BaseClass` não é compatível com CLS, o compilador emite um aviso.

```csharp
using System;

[assembly:CLSCompliant(true)]

[CLSCompliant(false)] public class BaseClass
{}

public class BaseCollection<T> where T : BaseClass
{}
// Attempting to compile the example displays the following output:
//    warning CS3024: Constraint type 'BaseClass' is not CLS-compliant
```

```vb
Assembly: CLSCompliant(True)>

<CLSCompliant(False)> Public Class BaseClass
End Class

Public Class BaseCollection(Of T As BaseClass)
End Class
' Attempting to compile the example displays the following output:
'    warning BC40040: Generic parameter constraint type 'BaseClass' is not
'    CLS-compliant.
'
'    Public Class BaseCollection(Of T As BaseClass)
'                                        ~~~~~~~~~
```

Se um tipo genérico for derivado de um tipo de base genérico, ele deverá redeclarar todas as restrições, para assegurar que as restrições no tipo de base também sejam atendidas. O exemplo a seguir define um `Number<T>` que pode representar qualquer tipo numérico. Ele também define uma classe `FloatingPoint<T>` que representa um valor de ponto flutuante. No entanto, o código-fonte falha na compilação porque não aplica a restrição em `Number<T>` (esse T deve ser um tipo de valor) a `FloatingPoint<T>`.

```csharp
using System;

[assembly:CLSCompliant(true)]

public class Number<T> where T : struct
{
   // use Double as the underlying type, since its range is a superset of
   // the ranges of all numeric types except BigInteger.
   protected double number;

   public Number(T value)
   {
      try {
         this.number = Convert.ToDouble(value);
      }
      catch (OverflowException e) {
         throw new ArgumentException("value is too large.", e);
      }
      catch (InvalidCastException e) {
         throw new ArgumentException("The value parameter is not numeric.", e);
      }
   }

   public T Add(T value)
   {
      return (T) Convert.ChangeType(number + Convert.ToDouble(value), typeof(T));
   }

   public T Subtract(T value)
   {
      return (T) Convert.ChangeType(number - Convert.ToDouble(value), typeof(T));
   }
}

public class FloatingPoint<T> : Number<T>
{
   public FloatingPoint(T number) : base(number)
   {
      if (typeof(float) == number.GetType() ||
          typeof(double) == number.GetType() ||
          typeof(decimal) == number.GetType())
         this.number = Convert.ToDouble(number);
      else
         throw new ArgumentException("The number parameter is not a floating-point number.");
   }
}
// The attempt to compile the example displays the following output:
//       error CS0453: The type 'T' must be a non-nullable value type in
//               order to use it as parameter 'T' in the generic type or method 'Number<T>'
```

```vb
<Assembly:CLSCompliant(True)>

Public Class Number(Of T As Structure)
   ' Use Double as the underlying type, since its range is a superset of
   ' the ranges of all numeric types except BigInteger.
   Protected number As Double

   Public Sub New(value As T)
      Try
         Me.number = Convert.ToDouble(value)
      Catch e As OverflowException
         Throw New ArgumentException("value is too large.", e)
      Catch e As InvalidCastException
         Throw New ArgumentException("The value parameter is not numeric.", e)
      End Try
   End Sub

   Public Function Add(value As T) As T
      Return CType(Convert.ChangeType(number + Convert.ToDouble(value), GetType(T)), T)
   End Function

   Public Function Subtract(value As T) As T
      Return CType(Convert.ChangeType(number - Convert.ToDouble(value), GetType(T)), T)
   End Function
End Class

Public Class FloatingPoint(Of T) : Inherits Number(Of T)
   Public Sub New(number As T)
      MyBase.New(number)
      If TypeOf number Is Single Or
               TypeOf number Is Double Or
               TypeOf number Is Decimal Then
         Me.number = Convert.ToDouble(number)
      Else
         throw new ArgumentException("The number parameter is not a floating-point number.")
      End If
   End Sub
End Class
' The attempt to compile the example displays the following output:
'    error BC32105: Type argument 'T' does not satisfy the 'Structure'
'    constraint for type parameter 'T'.
'
'    Public Class FloatingPoint(Of T) : Inherits Number(Of T)
'                                                          ~
```

O exemplo será compilado com êxito se a restrição for adicionada à classe `FloatingPoint<T>`.

```csharp
using System;

[assembly:CLSCompliant(true)]

public class Number<T> where T : struct
{
   // use Double as the underlying type, since its range is a superset of
   // the ranges of all numeric types except BigInteger.
   protected double number;

   public Number(T value)
   {
      try {
         this.number = Convert.ToDouble(value);
      }
      catch (OverflowException e) {
         throw new ArgumentException("value is too large.", e);
      }
      catch (InvalidCastException e) {
         throw new ArgumentException("The value parameter is not numeric.", e);
      }
   }

   public T Add(T value)
   {
      return (T) Convert.ChangeType(number + Convert.ToDouble(value), typeof(T));
   }

   public T Subtract(T value)
   {
      return (T) Convert.ChangeType(number - Convert.ToDouble(value), typeof(T));
   }
}

public class FloatingPoint<T> : Number<T> where T : struct
{
   public FloatingPoint(T number) : base(number)
   {
      if (typeof(float) == number.GetType() ||
          typeof(double) == number.GetType() ||
          typeof(decimal) == number.GetType())
         this.number = Convert.ToDouble(number);
      else
         throw new ArgumentException("The number parameter is not a floating-point number.");
   }
}
```

```vb
<Assembly:CLSCompliant(True)>

Public Class Number(Of T As Structure)
   ' Use Double as the underlying type, since its range is a superset of
   ' the ranges of all numeric types except BigInteger.
   Protected number As Double

   Public Sub New(value As T)
      Try
         Me.number = Convert.ToDouble(value)
      Catch e As OverflowException
         Throw New ArgumentException("value is too large.", e)
      Catch e As InvalidCastException
         Throw New ArgumentException("The value parameter is not numeric.", e)
      End Try
   End Sub

   Public Function Add(value As T) As T
      Return CType(Convert.ChangeType(number + Convert.ToDouble(value), GetType(T)), T)
   End Function

   Public Function Subtract(value As T) As T
      Return CType(Convert.ChangeType(number - Convert.ToDouble(value), GetType(T)), T)
   End Function
End Class

Public Class FloatingPoint(Of T As Structure) : Inherits Number(Of T)
   Public Sub New(number As T)
      MyBase.New(number)
      If TypeOf number Is Single Or
               TypeOf number Is Double Or
               TypeOf number Is Decimal Then
         Me.number = Convert.ToDouble(number)
      Else
         throw new ArgumentException("The number parameter is not a floating-point number.")
      End If
   End Sub
End Class
```

A Common Language Specification impõe um modelo por instanciação conservador para tipos aninhados e membros protegidos. Tipos genéricos abertos não podem expor campos ou membros com assinaturas que contenham uma instanciação específica de um tipo genérico aninhado, protegido. Tipos não genéricos que estendam uma instanciação específica de uma interface ou classe base genérica não podem expor campos ou membros com assinaturas que contenham uma instanciação diferente de um tipo genérico aninhado e protegido.

O exemplo a seguir define um tipo genérico, `C1<T>`, e uma classe protegida, `C1<T>.N`. `C1<T>` possui dois métodos, `M1` e `M2`. No entanto, `M1` não é compatível com CLS porque tenta retornar um objeto `C1<int>.N` de `C1<T>`. Uma segunda classe, `C2`, é derivada de`C1<long>`. Tem dois métodos, `M3` e `M4`. `M3` não está em conformidade com CLS porque tenta retornar um `C1<int>.N` objeto de uma subclasse de `C1<long>`. Os compiladores de linguagem podem ser ainda mais restritivos. Neste exemplo, o Visual Basic exibe um erro ao tentar compilar `M4`.

```csharp
using System;

[assembly:CLSCompliant(true)]

public class C1<T>
{
   protected class N { }

   protected void M1(C1<int>.N n) { } // Not CLS-compliant - C1<int>.N not
                                      // accessible from within C1<T> in all
                                      // languages
   protected void M2(C1<T>.N n) { }   // CLS-compliant – C1<T>.N accessible
                                      // inside C1<T>
}

public class C2 : C1<long>
{
   protected void M3(C1<int>.N n) { }  // Not CLS-compliant – C1<int>.N is not
                                       // accessible in C2 (extends C1<long>)

   protected void M4(C1<long>.N n) { } // CLS-compliant, C1<long>.N is
                                       // accessible in C2 (extends C1<long>)
}
// Attempting to compile the example displays output like the following:
//       Generics4.cs(9,22): warning CS3001: Argument type 'C1<int>.N' is not CLS-compliant
//       Generics4.cs(18,22): warning CS3001: Argument type 'C1<int>.N' is not CLS-compliant
```

```vb
<Assembly:CLSCompliant(True)>

Public Class C1(Of T)
   Protected Class N
   End Class

   Protected Sub M1(n As C1(Of Integer).N)   ' Not CLS-compliant - C1<int>.N not
                                             ' accessible from within C1(Of T) in all
   End Sub                                   ' languages

   Protected Sub M2(n As C1(Of T).N)     ' CLS-compliant – C1(Of T).N accessible
   End Sub                               ' inside C1(Of T)
End Class

Public Class C2 : Inherits C1(Of Long)
   Protected Sub M3(n As C1(Of Integer).N)   ' Not CLS-compliant – C1(Of Integer).N is not
   End Sub                                   ' accessible in C2 (extends C1(Of Long))

   Protected Sub M4(n As C1(Of Long).N)
   End Sub
End Class
' Attempting to compile the example displays output like the following:
'    error BC30508: 'n' cannot expose type 'C1(Of Integer).N' in namespace
'    '<Default>' through class 'C1'.
'
'       Protected Sub M1(n As C1(Of Integer).N)   ' Not CLS-compliant - C1<int>.N not
'                             ~~~~~~~~~~~~~~~~
'    error BC30389: 'C1(Of T).N' is not accessible in this context because
'    it is 'Protected'.
'
'       Protected Sub M3(n As C1(Of Integer).N)   ' Not CLS-compliant - C1(Of Integer).N is not
'
'                             ~~~~~~~~~~~~~~~~
'
'    error BC30389: 'C1(Of T).N' is not accessible in this context because it is 'Protected'.
'
'       Protected Sub M4(n As C1(Of Long).N)
'                             ~~~~~~~~~~~~~
```

### <a name="constructors"></a>Construtores

Os construtores em classes compatíveis com CLS e em estruturas devem seguir estas regras:

* Um construtor de uma classe derivada deve chamar o construtor de instância de sua classe base antes de acessar quaisquer dados de instância herdados. Esse requisito deve-se ao fato de que construtores de classe base não são herdados por suas classes derivadas. Essa regra não se aplica a estruturas que não dão suporte a herança direta.

  Normalmente, os compiladores aplicam essa regra independentemente da conformidade com CLS, conforme mostrado no exemplo a seguir. Ele cria uma classe `Doctor` que é derivada de uma classe `Person`, mas a classe `Doctor` falha ao chamar o construtor de classe `Person` para inicializar campos herdados da instância.

    ```csharp
    using System;

    [assembly: CLSCompliant(true)]

    public class Person
    {
    private string fName, lName, _id;

    public Person(string firstName, string lastName, string id)
    {
        if (String.IsNullOrEmpty(firstName + lastName))
            throw new ArgumentNullException("Either a first name or a last name must be provided.");

        fName = firstName;
        lName = lastName;
        _id = id;
    }

    public string FirstName
    {
        get { return fName; }
    }

    public string LastName
    {
        get { return lName; }
    }

    public string Id
    {
        get { return _id; }
    }

    public override string ToString()
    {
        return String.Format("{0}{1}{2}", fName,
                            String.IsNullOrEmpty(fName) ?  "" : " ",
                            lName);
    }
    }

    public class Doctor : Person
    {
    public Doctor(string firstName, string lastName, string id)
    {
    }

    public override string ToString()
    {
        return "Dr. " + base.ToString();
    }
    }
    // Attempting to compile the example displays output like the following:
    //    ctor1.cs(45,11): error CS1729: 'Person' does not contain a constructor that takes 0
    //            arguments
    //    ctor1.cs(10,11): (Location of symbol related to previous error)
    ```

    ```vb
    <Assembly: CLSCompliant(True)>

    Public Class Person
       Private fName, lName, _id As String

       Public Sub New(firstName As String, lastName As String, id As String)
          If String.IsNullOrEmpty(firstName + lastName) Then
             Throw New ArgumentNullException("Either a first name or a last name must be provided.")
          End If

          fName = firstName
          lName = lastName
          _id = id
       End Sub

       Public ReadOnly Property FirstName As String
          Get
             Return fName
          End Get
       End Property

       Public ReadOnly Property LastName As String
          Get
             Return lName
          End Get
       End Property

       Public ReadOnly Property Id As String
          Get
             Return _id
          End Get
       End Property

       Public Overrides Function ToString() As String
          Return String.Format("{0}{1}{2}", fName,
                               If(String.IsNullOrEmpty(fName), "", " "),
                               lName)
       End Function
    End Class

    Public Class Doctor : Inherits Person
       Public Sub New(firstName As String, lastName As String, id As String)
       End Sub

       Public Overrides Function ToString() As String
          Return "Dr. " + MyBase.ToString()
       End Function
    End Class
    ' Attempting to compile the example displays output like the following:
    '    Ctor1.vb(46) : error BC30148: First statement of this 'Sub New' must be a call
    '    to 'MyBase.New' or 'MyClass.New' because base class 'Person' of 'Doctor' does
    '    not have an accessible 'Sub New' that can be called with no arguments.
    '
    '       Public Sub New()
    '                  ~~~
    ````

* Um construtor de objeto não pode ser chamado, exceto para criar um objeto. Além disso, um objeto não pode ser inicializado duas vezes. Por exemplo, isso significa que `Object.MemberwiseClone` não deve chamar construtores.

### <a name="properties"></a>Propriedades

As propriedades em tipos em conformidade com CLS devem seguir estas regras:

* Uma propriedade deve ter um setter, um getter ou ambos. Em um assembly, eles são implementados como métodos especiais, o que significa que eles serão exibidos como métodos separados (o getter é chamado de `get` \_ *PropertyName* e o setter é `set` \_ *PropertyName*) marcado como `SpecialName` nos metadados do assembly. Os compiladores do C# aplicam automaticamente essa regra, sem a necessidade de aplicar o atributo <xref:System.CLSCompliantAttribute>.

* Um tipo de propriedade é o tipo de retorno do getter da propriedade e o último argumento do setter. Esses tipos devem estar em conformidade com CLS e os argumentos não podem ser atribuídos à propriedade por referência (ou seja, não podem ser ponteiros gerenciados).

* Se uma propriedade tiver um getter e um setter, ambos deverão ser virtuais, estáticos ou instâncias. O compilador do C# impõe automaticamente essa regra por meio de sintaxe da definição de propriedade.

### <a name="events"></a>Eventos

Um evento é definido por seu nome e tipo. O tipo de evento é um delegado que é usado para indicar o evento. Por exemplo, o evento `DbConnection.StateChange` é do tipo `StateChangeEventHandler`. Além do evento em si, três métodos com nomes com base no nome do evento fornecem a implementação do evento e estão marcados como `SpecialName` nos metadados do assembly:

* Um método para adicionar um manipulador de eventos, chamado `add`_*EventName*. Por exemplo, o método de assinatura do evento para o evento `DbConnection.StateChange` é chamado `add_StateChange`.

* Um método para remover um manipulador de eventos, chamado `remove`_*EventName*. Por exemplo, o método de remoção para o evento `DbConnection.StateChange` é chamado `remove_StateChange`.

* Um método para indicar que o evento ocorreu, chamado `raise` \_ *EventName*.

> [!NOTE]
> A maioria das regras da Common Language Specification em relação a eventos é implementada por compiladores de linguagem e é transparente para desenvolvedores de componente.

Os métodos para adicionar, remover e acionar o evento devem ter a mesma acessibilidade. Eles também devem ser todos estáticos, instâncias ou virtuais. Os métodos para adicionar e remover um evento têm um parâmetro cujo tipo é o tipo de delegado do evento. Os métodos para adicionar e remover devem estar ambos presentes ou ausentes.

O exemplo a seguir define uma classe compatível com CLS chamada `Temperature` que acionará um evento `TemperatureChanged` se a mudança de temperatura entre as duas leituras for igual ou exceder o valor de limite. A classe `Temperature` define explicitamente um método `raise_TemperatureChanged` para executar seletivamente os manipuladores de eventos.

```csharp
using System;
using System.Collections;
using System.Collections.Generic;

[assembly: CLSCompliant(true)]

public class TemperatureChangedEventArgs : EventArgs
{
   private Decimal originalTemp;
   private Decimal newTemp;
   private DateTimeOffset when;

   public TemperatureChangedEventArgs(Decimal original, Decimal @new, DateTimeOffset time)
   {
      originalTemp = original;
      newTemp = @new;
      when = time;
   }

   public Decimal OldTemperature
   {
      get { return originalTemp; }
   }

   public Decimal CurrentTemperature
   {
      get { return newTemp; }
   }

   public DateTimeOffset Time
   {
      get { return when; }
   }
}

public delegate void TemperatureChanged(Object sender, TemperatureChangedEventArgs e);

public class Temperature
{
   private struct TemperatureInfo
   {
      public Decimal Temperature;
      public DateTimeOffset Recorded;
   }

   public event TemperatureChanged TemperatureChanged;

   private Decimal previous;
   private Decimal current;
   private Decimal tolerance;
   private List<TemperatureInfo> tis = new List<TemperatureInfo>();

   public Temperature(Decimal temperature, Decimal tolerance)
   {
      current = temperature;
      TemperatureInfo ti = new TemperatureInfo();
      ti.Temperature = temperature;
      tis.Add(ti);
      ti.Recorded = DateTimeOffset.UtcNow;
      this.tolerance = tolerance;
   }

   public Decimal CurrentTemperature
   {
      get { return current; }
      set {
         TemperatureInfo ti = new TemperatureInfo();
         ti.Temperature = value;
         ti.Recorded = DateTimeOffset.UtcNow;
         previous = current;
         current = value;
         if (Math.Abs(current - previous) >= tolerance)
            raise_TemperatureChanged(new TemperatureChangedEventArgs(previous, current, ti.Recorded));
      }
   }

   public void raise_TemperatureChanged(TemperatureChangedEventArgs eventArgs)
   {
      if (TemperatureChanged == null)
         return;

      foreach (TemperatureChanged d in TemperatureChanged.GetInvocationList()) {
         if (d.Method.Name.Contains("Duplicate"))
            Console.WriteLine("Duplicate event handler; event handler not executed.");
         else
            d.Invoke(this, eventArgs);
      }
   }
}

public class Example
{
   public Temperature temp;

   public static void Main()
   {
      Example ex = new Example();
   }

   public Example()
   {
      temp = new Temperature(65, 3);
      temp.TemperatureChanged += this.TemperatureNotification;
      RecordTemperatures();
      Example ex = new Example(temp);
      ex.RecordTemperatures();
   }

   public Example(Temperature t)
   {
      temp = t;
      RecordTemperatures();
   }

   public void RecordTemperatures()
   {
      temp.TemperatureChanged += this.DuplicateTemperatureNotification;
      temp.CurrentTemperature = 66;
      temp.CurrentTemperature = 63;
   }

   internal void TemperatureNotification(Object sender, TemperatureChangedEventArgs e)
   {
      Console.WriteLine("Notification 1: The temperature changed from {0} to {1}", e.OldTemperature, e.CurrentTemperature);
   }

   public void DuplicateTemperatureNotification(Object sender, TemperatureChangedEventArgs e)
   {
      Console.WriteLine("Notification 2: The temperature changed from {0} to {1}", e.OldTemperature, e.CurrentTemperature);
   }
}
```

```vb
Imports System.Collections
Imports System.Collections.Generic

<Assembly: CLSCompliant(True)>

Public Class TemperatureChangedEventArgs   : Inherits EventArgs
   Private originalTemp As Decimal
   Private newTemp As Decimal
   Private [when] As DateTimeOffset

   Public Sub New(original As Decimal, [new] As Decimal, [time] As DateTimeOffset)
      originalTemp = original
      newTemp = [new]
      [when] = [time]
   End Sub

   Public ReadOnly Property OldTemperature As Decimal
      Get
         Return originalTemp
      End Get
   End Property

   Public ReadOnly Property CurrentTemperature As Decimal
      Get
         Return newTemp
      End Get
   End Property

   Public ReadOnly Property [Time] As DateTimeOffset
      Get
         Return [when]
      End Get
   End Property
End Class

Public Delegate Sub TemperatureChanged(sender As Object, e As TemperatureChangedEventArgs)

Public Class Temperature
   Private Structure TemperatureInfo
      Dim Temperature As Decimal
      Dim Recorded As DateTimeOffset
   End Structure

   Public Event TemperatureChanged As TemperatureChanged

   Private previous As Decimal
   Private current As Decimal
   Private tolerance As Decimal
   Private tis As New List(Of TemperatureInfo)

   Public Sub New(temperature As Decimal, tolerance As Decimal)
      current = temperature
      Dim ti As New TemperatureInfo()
      ti.Temperature = temperature
      ti.Recorded = DateTimeOffset.UtcNow
      tis.Add(ti)
      Me.tolerance = tolerance
   End Sub

   Public Property CurrentTemperature As Decimal
      Get
         Return current
      End Get
      Set
         Dim ti As New TemperatureInfo
         ti.Temperature = value
         ti.Recorded = DateTimeOffset.UtcNow
         previous = current
         current = value
         If Math.Abs(current - previous) >= tolerance Then
            raise_TemperatureChanged(New TemperatureChangedEventArgs(previous, current, ti.Recorded))
         End If
      End Set
   End Property

   Public Sub raise_TemperatureChanged(eventArgs As TemperatureChangedEventArgs)
      If TemperatureChangedEvent Is Nothing Then Exit Sub

      Dim ListenerList() As System.Delegate = TemperatureChangedEvent.GetInvocationList()
      For Each d As TemperatureChanged In TemperatureChangedEvent.GetInvocationList()
         If d.Method.Name.Contains("Duplicate") Then
            Console.WriteLine("Duplicate event handler; event handler not executed.")
         Else
            d.Invoke(Me, eventArgs)
         End If
      Next
   End Sub
End Class

Public Class Example
   Public WithEvents temp As Temperature

   Public Shared Sub Main()
      Dim ex As New Example()
   End Sub

   Public Sub New()
      temp = New Temperature(65, 3)
      RecordTemperatures()
      Dim ex As New Example(temp)
      ex.RecordTemperatures()
   End Sub

   Public Sub New(t As Temperature)
      temp = t
      RecordTemperatures()
   End Sub

   Public Sub RecordTemperatures()
      temp.CurrentTemperature = 66
      temp.CurrentTemperature = 63

   End Sub

   Friend Shared Sub TemperatureNotification(sender As Object, e As TemperatureChangedEventArgs) _
          Handles temp.TemperatureChanged
      Console.WriteLine("Notification 1: The temperature changed from {0} to {1}", e.OldTemperature, e.CurrentTemperature)
   End Sub

   Friend Shared Sub DuplicateTemperatureNotification(sender As Object, e As TemperatureChangedEventArgs) _
          Handles temp.TemperatureChanged
      Console.WriteLine("Notification 2: The temperature changed from {0} to {1}", e.OldTemperature, e.CurrentTemperature)
   End Sub
End Class
```

### <a name="overloads"></a>Sobrecargas

A Common Language Specification impõe os seguintes requisitos em membros sobrecarregados:

* Os membros podem ser sobrecarregados com base no número de parâmetros e no tipo de qualquer parâmetro. A convenção de chamada, o tipo de retorno, os modificadores personalizados aplicados ao método ou seus parâmetros e se os parâmetros são passados por valor ou por referência não são considerados na diferenciação entre sobrecargas. Para obter um exemplo, consulte o código para o requisito de que os nomes devem ser exclusivos em um escopo na seção [Convenções de nomenclatura](#naming-conventions).

* Somente propriedades e métodos podem ser sobrecarregados. Campos e eventos não podem ser sobrecarregados.

* Métodos genéricos podem ser sobrecarregados com base no número de seus parâmetros genéricos.

> [!NOTE]
> Os operadores `op_Explicit` e `op_Implicit` são exceções à regra de que o valor retornado não é considerado parte de uma assinatura de método para resolução de sobrecarga. Esses dois operadores podem ser sobrecarregados com base nos parâmetros e valor retornado.

### <a name="exceptions"></a>Exceções

Objetos de exceção devem derivar de [System.Exception](xref:System.Exception) ou de outro tipo derivado de `System.Exception`. O exemplo a seguir ilustra o erro do compilador que resulta quando uma classe personalizada chamada `ErrorClass` é usada para tratamento de exceções.

```csharp
using System;

[assembly: CLSCompliant(true)]

public class ErrorClass
{
   string msg;

   public ErrorClass(string errorMessage)
   {
      msg = errorMessage;
   }

   public string Message
   {
      get { return msg; }
   }
}

public static class StringUtilities
{
   public static string[] SplitString(this string value, int index)
   {
      if (index < 0 | index > value.Length) {
         ErrorClass badIndex = new ErrorClass("The index is not within the string.");
         throw badIndex;
      }
      string[] retVal = { value.Substring(0, index - 1),
                          value.Substring(index) };
      return retVal;
   }
}
// Compilation produces a compiler error like the following:
//    Exceptions1.cs(26,16): error CS0155: The type caught or thrown must be derived from
//            System.Exception
```

```vb
Imports System.Runtime.CompilerServices

<Assembly: CLSCompliant(True)>

Public Class ErrorClass
   Dim msg As String

   Public Sub New(errorMessage As String)
      msg = errorMessage
   End Sub

   Public ReadOnly Property Message As String
      Get
         Return msg
      End Get
   End Property
End Class

Public Module StringUtilities
   <Extension()> Public Function SplitString(value As String, index As Integer) As String()
      If index < 0 Or index > value.Length Then
         Dim BadIndex As New ErrorClass("The index is not within the string.")
         Throw BadIndex
      End If
      Dim retVal() As String = { value.Substring(0, index - 1),
                                 value.Substring(index) }
      Return retVal
   End Function
End Module
' Compilation produces a compiler error like the following:
'    Exceptions1.vb(27) : error BC30665: 'Throw' operand must derive from 'System.Exception'.
'
'             Throw BadIndex
'             ~~~~~~~~~~~~~~
```

Para corrigir este erro, a classe `ErrorClass` deve herdar de `System.Exception`. Além disso, a propriedade Message deve ser substituída. O exemplo a seguir corrige esses erros para definir uma classe `ErrorClass` que seja compatível com CLS.

```csharp
using System;

[assembly: CLSCompliant(true)]

public class ErrorClass : Exception
{
   string msg;

   public ErrorClass(string errorMessage)
   {
      msg = errorMessage;
   }

   public override string Message
   {
      get { return msg; }
   }
}

public static class StringUtilities
{
   public static string[] SplitString(this string value, int index)
   {
      if (index < 0 | index > value.Length) {
         ErrorClass badIndex = new ErrorClass("The index is not within the string.");
         throw badIndex;
      }
      string[] retVal = { value.Substring(0, index - 1),
                          value.Substring(index) };
      return retVal;
   }
}
```

```vb
Imports System.Runtime.CompilerServices

<Assembly: CLSCompliant(True)>

Public Class ErrorClass : Inherits Exception
   Dim msg As String

   Public Sub New(errorMessage As String)
      msg = errorMessage
   End Sub

   Public Overrides ReadOnly Property Message As String
      Get
         Return msg
      End Get
   End Property
End Class

Public Module StringUtilities
   <Extension()> Public Function SplitString(value As String, index As Integer) As String()
      If index < 0 Or index > value.Length Then
         Dim BadIndex As New ErrorClass("The index is not within the string.")
         Throw BadIndex
      End If
      Dim retVal() As String = { value.Substring(0, index - 1),
                                 value.Substring(index) }
      Return retVal
   End Function
End Module
```

### <a name="attributes"></a>Atributos

Em assemblies do .NET Framework, atributos personalizados fornecem um mecanismo extensível para armazenamento de atributos personalizados e recuperação de metadados sobre objetos de programação, como assemblies, tipos, membros e parâmetros de método. Atributos personalizados devem derivar de [System.Attribute](xref:System.Attribute) ou de um tipo derivado de `System.Attribute`.

O exemplo a seguir viola essa regra. Ele define uma classe `NumericAttribute` que não deriva de `System.Attribute`. Um erro de compilador é resultado somente quando o atributo não compatível com CLS é aplicado, não quando a classe é definida.

```csharp
using System;

[assembly: CLSCompliant(true)]

[AttributeUsageAttribute(AttributeTargets.Class | AttributeTargets.Struct)]
public class NumericAttribute
{
   private bool _isNumeric;

   public NumericAttribute(bool isNumeric)
   {
      _isNumeric = isNumeric;
   }

   public bool IsNumeric
   {
      get { return _isNumeric; }
   }
}

[Numeric(true)] public struct UDouble
{
   double Value;
}
// Compilation produces a compiler error like the following:
//    Attribute1.cs(22,2): error CS0616: 'NumericAttribute' is not an attribute class
//    Attribute1.cs(7,14): (Location of symbol related to previous error)
```

```vb
<Assembly: CLSCompliant(True)>

<AttributeUsageAttribute(AttributeTargets.Class Or AttributeTargets.Struct)> _
Public Class NumericAttribute
   Private _isNumeric As Boolean

   Public Sub New(isNumeric As Boolean)
      _isNumeric = isNumeric
   End Sub

   Public ReadOnly Property IsNumeric As Boolean
      Get
         Return _isNumeric
      End Get
   End Property
End Class

<Numeric(True)> Public Structure UDouble
   Dim Value As Double
End Structure
' Compilation produces a compiler error like the following:
'    error BC31504: 'NumericAttribute' cannot be used as an attribute because it
'    does not inherit from 'System.Attribute'.
'
'    <Numeric(True)> Public Structure UDouble
'     ~~~~~~~~~~~~~
```

O construtor ou as propriedades de um atributo compatível com CLS podem expor somente os seguintes tipos:

* [Boolean](xref:System.Boolean)

* [Minuciosa](xref:System.Byte)

* [º](xref:System.Char)

* [Clique](xref:System.Double)

* [Int16](xref:System.Int16)

* [Int32](xref:System.Int32)

* [Int64](xref:System.Int64)

* [Exclusivo](xref:System.Single)

* [Cadeia de caracteres](xref:System.String)

* [Tipo](xref:System.Type)

* Qualquer tipo de enumeração cujo tipo subjacente seja `Byte`, `Int16`, `Int32` ou `Int64`.

O exemplo a seguir define uma classe `DescriptionAttribute` que deriva de [Atributo](xref:System.Attribute). O construtor de classe tem um parâmetro do tipo `Descriptor`, portanto a classe não é compatível com CLS. O compilador C# emite um aviso, mas é compilado com êxito.

```csharp
using System;

[assembly:CLSCompliantAttribute(true)]

public enum DescriptorType { type, member };

public class Descriptor
{
   public DescriptorType Type;
   public String Description;
}

[AttributeUsage(AttributeTargets.All)]
public class DescriptionAttribute : Attribute
{
   private Descriptor desc;

   public DescriptionAttribute(Descriptor d)
   {
      desc = d;
   }

   public Descriptor Descriptor
   { get { return desc; } }
}
// Attempting to compile the example displays output like the following:
//       warning CS3015: 'DescriptionAttribute' has no accessible
//               constructors which use only CLS-compliant types
```

```vb
<Assembly:CLSCompliantAttribute(True)>

Public Enum DescriptorType As Integer
   Type = 0
   Member = 1
End Enum

Public Class Descriptor
   Public Type As DescriptorType
   Public Description As String
End Class

<AttributeUsage(AttributeTargets.All)> _
Public Class DescriptionAttribute : Inherits Attribute
   Private desc As Descriptor

   Public Sub New(d As Descriptor)
      desc = d
   End Sub

   Public ReadOnly Property Descriptor As Descriptor
      Get
         Return desc
      End Get
   End Property
End Class
```

## <a name="the-clscompliantattribute-attribute"></a>O atributo CLSCompliantAttribute

O atributo [CLSCompliantAttribute](xref:System.CLSCompliantAttribute) é usado para indicar se um elemento do programa está em conformidade com a Common Language Specification. O construtor `CLSCompliantAttribute.CLSCompliantAttribute(Boolean)` inclui um único parâmetro necessário, *isCompliant*, que indica se o elemento de programa é compatível com CLS.

No tempo de compilação, o compilador detecta os elementos não compatíveis que provavelmente são compatíveis com CLS e emite um aviso. O compilador não emite avisos para tipos ou membros explicitamente declarados como não compatíveis.

Os desenvolvedores de componentes podem usar o atributo `CLSCompliantAttribute` de duas maneiras:

* Para definir as partes da interface pública exposta por um componente que são compatíveis com CLS e as partes que não são compatíveis com CLS. Quando o atributo é usado para marcar elementos de programa específicos como em conformidade com CLS, seu uso garante que os elementos sejam acessíveis em todas as linguagens e ferramentas direcionadas ao .NET Framework.

* Para garantir que a interface pública da biblioteca de componentes exponha apenas elementos de programa que são compatíveis com CLS. Se os elementos não forem compatíveis com CLS, os compiladores geralmente emitirão um aviso.

> [!WARNING]
> Em alguns casos, os compiladores de linguagem aplicam as regras de compatibilidade com CLS independentemente do uso ou não do atributo `CLSCompliantAttribute`. Por exemplo, a definição de um membro `*static` em uma interface viola uma regra CLS. No entanto, se você definir um membro `*static` em uma interface, o compilador do C# exibirá uma mensagem de erro e falhará ao compilar o aplicativo.

O atributo `CLSCompliantAttribute` é marcado com um atributo [AttributeUsageAttribute](xref:System.AttributeUsageAttribute) que tem um valor de `AttributeTargets.All`. Esse valor permite que você aplique o atributo `CLSCompliantAttribute` a qualquer elemento de programa, incluindo assemblies, módulos, tipos (classes, estruturas, interfaces, enumerações e delegados), membros de tipo (construtores, métodos, propriedades, campos e eventos), parâmetros, parâmetros genéricos e valores de retorno. No entanto, na prática, você deve aplicar o atributo somente a assemblies, tipos e membros de tipo. Caso contrário, os compiladores ignoram o atributo e continuam gerando avisos do compilador sempre que encontrarem um parâmetro não compatível, parâmetro genérico ou valor retornado na interface pública da biblioteca.

O valor do atributo `CLSCompliantAttribute` é herdado pelos elementos contidos no programa. Por exemplo, se um assembly for marcado como compatível com CLS, seus tipos também serão compatíveis com CLS. Se um tipo for marcado como em conformidade com CLS, seus membros e tipos aninhados também tem conformidade com CLS.

Você pode substituir explicitamente a compatibilidade herdada aplicando o atributo `CLSCompliantAttribute` a um elemento contido no programa. Por exemplo, é possível usar o atributo `CLSCompliantAttribute` com um valor *isCompliant* de `false` para definir um tipo não compatível em um assembly compatível e é possível usar o atributo com um valor *isComplian* de `true` para definir um tipo compatível em um assembly não compatível. Você também pode definir membros não compatíveis em um tipo compatível. No entanto, um tipo não compatível não pode ter membros compatíveis. Portanto, você não pode usar o atributo com um valor *isCompliant* de `true` para substituir a herança de um tipo não compatível.

Ao desenvolver componentes, você sempre deve usar o atributo `CLSCompliantAttribute` para indicar se o assembly, seus tipos e membros são compatíveis com CLS.

Para criar componentes compatíveis com CLS:

1. Use `CLSCompliantAttribute` para marcar o assembly como compatível com CLS.

2. Marque qualquer tipo exposto publicamente no assembly que não esteja em conformidade com CLS como não em conformidade.

3. Marque qualquer membro publicamente exposto em tipos compatíveis com CLS como não compatíveis.

4. Forneça uma alternativa em conformidade com CLS para membros não em conformidade com CLS.

Se você marcou com êxito todos os tipos e membros não em conformidade, o compilador não deverá emitir avisos de não conformidade. Entretanto, você deve indicar quais membros não são compatíveis com CLS e listar suas alternativas compatíveis com CLS na documentação do produto.

O exemplo a seguir usa o atributo `CLSCompliantAttribute` para definir um assembly compatível com CLS e um tipo, `CharacterUtilities`, que tem dois membros não compatíveis com CLS. Como ambos os membros são marcados com o atributo `CLSCompliant(false)`, o compilador não produz avisos. A classe também fornece uma alternativa compatível com CLS para ambos os métodos. Normalmente, nós adicionaríamos apenas duas sobrecargas ao método `ToUTF16` para fornecer alternativas compatíveis com CLS. Entretanto, como os métodos não podem ser sobrecarregados com base no valor retornado, os nomes dos métodos em conformidade com CLS são diferentes dos nomes dos métodos não em conformidade.

```csharp
using System;
using System.Text;

[assembly:CLSCompliant(true)]

public class CharacterUtilities
{
   [CLSCompliant(false)] public static ushort ToUTF16(String s)
   {
      s = s.Normalize(NormalizationForm.FormC);
      return Convert.ToUInt16(s[0]);
   }

   [CLSCompliant(false)] public static ushort ToUTF16(Char ch)
   {
      return Convert.ToUInt16(ch);
   }

   // CLS-compliant alternative for ToUTF16(String).
   public static int ToUTF16CodeUnit(String s)
   {
      s = s.Normalize(NormalizationForm.FormC);
      return (int) Convert.ToUInt16(s[0]);
   }

   // CLS-compliant alternative for ToUTF16(Char).
   public static int ToUTF16CodeUnit(Char ch)
   {
      return Convert.ToInt32(ch);
   }

   public bool HasMultipleRepresentations(String s)
   {
      String s1 = s.Normalize(NormalizationForm.FormC);
      return s.Equals(s1);
   }

   public int GetUnicodeCodePoint(Char ch)
   {
      if (Char.IsSurrogate(ch))
         throw new ArgumentException("ch cannot be a high or low surrogate.");

      return Char.ConvertToUtf32(ch.ToString(), 0);
   }

   public int GetUnicodeCodePoint(Char[] chars)
   {
      if (chars.Length > 2)
         throw new ArgumentException("The array has too many characters.");

      if (chars.Length == 2) {
         if (! Char.IsSurrogatePair(chars[0], chars[1]))
            throw new ArgumentException("The array must contain a low and a high surrogate.");
         else
            return Char.ConvertToUtf32(chars[0], chars[1]);
      }
      else {
         return Char.ConvertToUtf32(chars.ToString(), 0);
      }
   }
}
```

```vb
Imports System.Text

<Assembly:CLSCompliant(True)>

Public Class CharacterUtilities
   <CLSCompliant(False)> Public Shared Function ToUTF16(s As String) As UShort
      s = s.Normalize(NormalizationForm.FormC)
      Return Convert.ToUInt16(s(0))
   End Function

   <CLSCompliant(False)> Public Shared Function ToUTF16(ch As Char) As UShort
      Return Convert.ToUInt16(ch)
   End Function

   ' CLS-compliant alternative for ToUTF16(String).
   Public Shared Function ToUTF16CodeUnit(s As String) As Integer
      s = s.Normalize(NormalizationForm.FormC)
      Return CInt(Convert.ToInt16(s(0)))
   End Function

   ' CLS-compliant alternative for ToUTF16(Char).
   Public Shared Function ToUTF16CodeUnit(ch As Char) As Integer
      Return Convert.ToInt32(ch)
   End Function

   Public Function HasMultipleRepresentations(s As String) As Boolean
      Dim s1 As String = s.Normalize(NormalizationForm.FormC)
      Return s.Equals(s1)
   End Function

   Public Function GetUnicodeCodePoint(ch As Char) As Integer
      If Char.IsSurrogate(ch) Then
         Throw New ArgumentException("ch cannot be a high or low surrogate.")
      End If
      Return Char.ConvertToUtf32(ch.ToString(), 0)
   End Function

   Public Function GetUnicodeCodePoint(chars() As Char) As Integer
      If chars.Length > 2 Then
         Throw New ArgumentException("The array has too many characters.")
      End If
      If chars.Length = 2 Then
         If Not Char.IsSurrogatePair(chars(0), chars(1)) Then
            Throw New ArgumentException("The array must contain a low and a high surrogate.")
         Else
            Return Char.ConvertToUtf32(chars(0), chars(1))
         End If
      Else
         Return Char.ConvertToUtf32(chars.ToString(), 0)
      End If
   End Function
End Class
```

Se você estiver desenvolvendo um aplicativo em vez de uma biblioteca (ou seja, se não estiver expondo tipos ou membros que possam ser consumidos por outros desenvolvedores de aplicativos), a conformidade com CLS dos elementos do programa que seu aplicativo consome só serão de interesse se sua linguagem não der suporte a eles. Nesse caso, seu compilador de linguagem gerará um erro quando você tentar usar um elemento não compatível com CLS.

## <a name="cross-language-interoperability"></a>Interoperabilidade entre linguagens

A independência de linguagem tem vários significados possíveis. Um significado envolve o consumo contínuo de tipos gravados em uma linguagem de um aplicativo gravado em outra linguagem. Um segundo significado, que é o enfoque deste artigo, envolve combinar o código gravado em várias linguagens em um único assembly do .NET Framework.

O exemplo a seguir ilustra a interoperabilidade em qualquer idioma com a criação de uma biblioteca de classes chamada Utilities.dll que inclui duas classes, `NumericLib` e `StringLib`. A classe `NumericLib` é gravada em C#, e a classe `StringLib` é gravada em Visual Basic. Aqui está o código-fonte de `StringUtil.vb`, que inclui um único membro, `ToTitleCase`, em sua classe `StringLib`.

```vb
Imports System.Collections.Generic
Imports System.Runtime.CompilerServices

Public Module StringLib
   Private exclusions As List(Of String)

   Sub New()
      Dim words() As String = { "a", "an", "and", "of", "the" }
      exclusions = New List(Of String)
      exclusions.AddRange(words)
   End Sub

   <Extension()> _
   Public Function ToTitleCase(title As String) As String
      Dim words() As String = title.Split()
      Dim result As String = String.Empty

      For ctr As Integer = 0 To words.Length - 1
         Dim word As String = words(ctr)
         If ctr = 0 OrElse Not exclusions.Contains(word.ToLower()) Then
            result += word.Substring(0, 1).ToUpper() + _
                      word.Substring(1).ToLower()
         Else
            result += word.ToLower()
         End If
         If ctr <= words.Length - 1 Then
            result += " "
         End If
      Next
      Return result
   End Function
End Module
```

Aqui está o código-fonte de NumberUtil.cs, que define uma classe `NumericLib` com dois membros, `IsEven` e `NearZero`.

```csharp
using System;

public static class NumericLib
{
   public static bool IsEven(this IConvertible number)
   {
      if (number is Byte ||
          number is SByte ||
          number is Int16 ||
          number is UInt16 ||
          number is Int32 ||
          number is UInt32 ||
          number is Int64)
         return ((long) number) % 2 == 0;
      else if (number is UInt64)
         return ((ulong) number) %2 == 0;
      else
         throw new NotSupportedException("IsEven called for a non-integer value.");
   }

   public static bool NearZero(double number)
   {
      return number < .00001;
   }
}
```

Para empacotar as duas classes em um único assembly, você deve compilá-las em módulos. Para compilar o arquivo de código-fonte do Visual Basic em um módulo, use este comando:

```console
vbc /t:module StringUtil.vb
```

Para compilar o arquivo de código-fonte do C# em um módulo, use este comando:

```console
csc /t:module NumberUtil.cs
```

Então você usa a ferramenta Link (Link.exe) para compilar os dois módulos em um assembly:

```console
link numberutil.netmodule stringutil.netmodule /out:UtilityLib.dll /dll
```

O exemplo a seguir chama os métodos `NumericLib.NearZero` e `StringLib.ToTitleCase`. O código de Visual Basic e o código C# são capazes de acessar os métodos em ambas as classes.

```csharp
using System;

public class Example
{
   public static void Main()
   {
      Double dbl = 0.0 - Double.Epsilon;
      Console.WriteLine(NumericLib.NearZero(dbl));

      string s = "war and peace";
      Console.WriteLine(s.ToTitleCase());
   }
}
// The example displays the following output:
//       True
//       War and Peace
```

```vb
Module Example
   Public Sub Main()
      Dim dbl As Double = 0.0 - Double.Epsilon
      Console.WriteLine(NumericLib.NearZero(dbl))

      Dim s As String = "war and peace"
      Console.WriteLine(s.ToTitleCase())
   End Sub
End Module
' The example displays the following output:
'       True
'       War and Peace
```

Para compilar o código do Visual Basic, use este comando:

```console
vbc example.vb /r:UtilityLib.dll
```

Para compilar usando C#, altere o nome do compilador de vbc para csc e altere a extensão do arquivo de .vb para .cs:

```console
csc example.cs /r:UtilityLib.dll
```
