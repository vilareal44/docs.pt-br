---
title: Conversão de tipos no .NET Framework
description: Leia sobre conversão de tipo no .NET, que cria um valor em um novo tipo equivalente ao valor do tipo antigo, mas pode não manter a identidade original.
ms.date: 03/30/2017
ms.technology: dotnet-standard
dev_langs:
- csharp
- vb
helpviewer_keywords:
- widening conversions
- explicit conversions
- narrowing conversions
- type conversion, about type conversion
- type conversion
- converting types
- narrowing coercion
- Explicit operator
- IConvertible interface
- base types, converting
- op_Implicit method
- widening coercion
- op_Explicit method
- Convert class
- implicit conversions
- Implicit operator
- data types [.NET Framework], converting
ms.assetid: ba36154f-064c-47d3-9f05-72f93a7ca96d
ms.openlocfilehash: 11345081610459dbf053d846aa04369301010732
ms.sourcegitcommit: 5fd4696a3e5791b2a8c449ccffda87f2cc2d4894
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/15/2020
ms.locfileid: "84769217"
---
# <a name="type-conversion-in-the-net-framework"></a>Conversão de tipos no .NET Framework
Cada valor tem um tipo associado, o qual define atributos como a quantidade de espaço alocado para o valor, o intervalo de valores possíveis que ele pode assumir e os membros que ele disponibiliza. Muitos valores podem ser expressos por mais de um tipo. Por exemplo, o valor 4 pode ser expresso como um número inteiro ou como um valor de ponto flutuante. A conversão de tipo cria um valor em um novo tipo que é equivalente ao valor de um tipo antigo, mas que não necessariamente preserva a identidade (ou o valor exato) do objeto original.  
  
 O .NET Framework dá suporte automaticamente às seguintes conversões:  
  
- Conversão de uma classe derivada em uma classe base. Isso significa, por exemplo, que uma instância de qualquer classe ou estrutura pode ser convertida em uma instância de <xref:System.Object>.  Essa conversão não exige um operador de conversão.  
  
- Conversão de uma classe base para a classe derivada original. No C#, essa conversão exige um operador de conversão. No Visual Basic, ela exige o operador `CType` quando a `Option Strict` está ativada.  
  
- Conversão de um tipo que implementa uma interface para um objeto de interface que representa essa interface. Essa conversão não exige um operador de conversão.  
  
- Conversão de um objeto de interface de volta para o tipo original que implementa essa interface.  No C#, essa conversão exige um operador de conversão. No Visual Basic, ela exige o operador `CType` quando a `Option Strict` está ativada.  
  
 Além dessas conversões automáticas, o .NET Framework fornece várias funcionalidades que oferecem suporte à conversão personalizada de tipo. Entre elas estão as seguintes:  
  
- O operador `Implicit`, que define as conversões de ampliação disponíveis entre tipos. Para obter mais informações, consulte a seção [conversão implícita com o operador implícito](#implicit-conversion-with-the-implicit-operator) .  
  
- O operador `Explicit`, que define as conversões de redução disponíveis entre tipos. Para obter mais informações, consulte a [conversão explícita com a seção operador explícita](#explicit-conversion-with-the-explicit-operator) .  
  
- A interface <xref:System.IConvertible>, que define conversões para cada um dos tipos de dados base do .NET Framework. Para obter mais informações, consulte a seção [A interface IConvertible](#the-iconvertible-interface).  
  
- A classe <xref:System.Convert>, que fornece um conjunto de métodos que implementam os métodos da interface <xref:System.IConvertible>. Para obter mais informações, consulte a seção [A classe Convert](#the-convert-class).  
  
- A classe <xref:System.ComponentModel.TypeConverter>, que é uma classe base que pode ser estendida para oferecer suporte à conversão de um tipo especificado para qualquer outro tipo. Para obter mais informações, consulte a seção [A classe TypeConverter](#the-typeconverter-class).  

## <a name="implicit-conversion-with-the-implicit-operator"></a>Conversão implícita com o operador implícito  
 As conversões de ampliação envolvem a criação de um novo valor do valor de um tipo existente que tem um intervalo mais restritivo ou uma lista de membros mais restrita que o tipo de destino. As conversões de ampliação não podem resultar na perda de dados (mas podem resultar em uma perda de precisão). Como os dados não podem ser perdidos, os compiladores podem tratar a conversão de forma implícita ou transparente, sem exigir o uso de um método de conversão explícita ou de um operador de conversão.  
  
> [!NOTE]
> Embora o código que executa uma conversão implícita possa chamar um método de conversão ou usar um operador de conversão, seu uso não é exigido pelos compiladores que oferecem suporte a conversões implícitas.  
  
 Por exemplo, o tipo <xref:System.Decimal> oferece suporte a conversões implícitas de valores <xref:System.Byte>, <xref:System.Char>, <xref:System.Int16>, <xref:System.Int32>, <xref:System.Int64>, <xref:System.SByte>, <xref:System.UInt16>, <xref:System.UInt32> e <xref:System.UInt64>. O exemplo a seguir ilustra algumas dessas conversões implícitas ao atribuir valores a uma variável <xref:System.Decimal>.  
  
 [!code-csharp[Conceptual.Conversion#1](../../../samples/snippets/csharp/VS_Snippets_CLR/conceptual.conversion/cs/implicit1.cs#1)]
 [!code-vb[Conceptual.Conversion#1](../../../samples/snippets/visualbasic/VS_Snippets_CLR/conceptual.conversion/vb/implicit1.vb#1)]  
  
 Se o compilador de uma linguagem específica oferecer suporte a operadores personalizados, você também poderá definir conversões implícitas em seus próprios tipos personalizados. O exemplo a seguir oferece uma implementação parcial de um tipo de dados de byte assinado chamado `ByteWithSign` que usa a representação de sinal e magnitude. Ele oferece suporte à conversão implícita dos valores <xref:System.Byte> e <xref:System.SByte> para valores `ByteWithSign`.  
  
 [!code-csharp[Conceptual.Conversion#2](../../../samples/snippets/csharp/VS_Snippets_CLR/conceptual.conversion/cs/implicit1.cs#2)]
 [!code-vb[Conceptual.Conversion#2](../../../samples/snippets/visualbasic/VS_Snippets_CLR/conceptual.conversion/vb/implicit1.vb#2)]  
  
 O código cliente pode então declarar uma variável `ByteWithSign` e atribuir a ela valores <xref:System.Byte> e <xref:System.SByte> sem executar nenhuma conversão explícita ou usar operadores de conversão, conforme mostrado no exemplo a seguir.  
  
 [!code-csharp[Conceptual.Conversion#3](../../../samples/snippets/csharp/VS_Snippets_CLR/conceptual.conversion/cs/implicit1.cs#3)]
 [!code-vb[Conceptual.Conversion#3](../../../samples/snippets/visualbasic/VS_Snippets_CLR/conceptual.conversion/vb/implicit1.vb#3)]  

## <a name="explicit-conversion-with-the-explicit-operator"></a>Conversão explícita com o operador explícito  
 As conversões de redução envolvem a criação de um novo valor com o valor de um tipo existente que tem um intervalo mais amplo ou uma lista de membros maior do que o tipo de destino. Como uma conversão de redução pode resultar em uma perda de dados, os compiladores geralmente exigem que a conversão se torne explícita por meio da chamada de um método de conversão ou de um operador de conversão. Ou seja, a conversão deve ser tratada explicitamente no código do desenvolvedor.  
  
> [!NOTE]
> A finalidade principal de requerer um método de conversão ou de conversão do operador para reduzir conversões é fazer com que o desenvolvedor saiba sobre a possibilidade de perda de dados ou de <xref:System.OverflowException> para que ela possa ser tratada no código. No entanto, alguns compiladores podem amenizar esse requisito. Por exemplo, no Visual Basic, se a `Option Strict` estiver desativada (sua configuração padrão), o compilador do Visual Basic tentará executar as conversões de redução de forma implícita.  
  
 Por exemplo, os tipos de dados <xref:System.UInt32>, <xref:System.Int64> e <xref:System.UInt64> possuem intervalos que excedem o do tipo de dados <xref:System.Int32>, conforme mostrado na tabela a seguir.  
  
|Tipo|Comparação com intervalo de Int32|  
|----------|------------------------------------|  
|<xref:System.Int64>|<xref:System.Int64.MaxValue?displayProperty=nameWithType> é maior que <xref:System.Int32.MaxValue?displayProperty=nameWithType>, e <xref:System.Int64.MinValue?displayProperty=nameWithType> é menor que (possui um intervalo negativo maior que) <xref:System.Int32.MinValue?displayProperty=nameWithType>.|  
|<xref:System.UInt32>|<xref:System.UInt32.MaxValue?displayProperty=nameWithType> é maior que <xref:System.Int32.MaxValue?displayProperty=nameWithType>.|  
|<xref:System.UInt64>|<xref:System.UInt64.MaxValue?displayProperty=nameWithType> é maior que <xref:System.Int32.MaxValue?displayProperty=nameWithType>.|  
  
 Para tratar essas conversões redutoras, o .NET Framework permite que os tipos definam um operador `Explicit`. Os compiladores de linguagens individuais podem então implementar esse operador que usa sua própria sintaxe, ou um membro da classe <xref:System.Convert> pode ser chamado para executar a conversão. (Para obter mais informações sobre a <xref:System.Convert> classe, consulte [a classe Convert](#the-convert-class) mais adiante neste tópico.) O exemplo a seguir ilustra o uso de recursos de linguagem para manipular a conversão explícita desses valores inteiros potencialmente fora do intervalo para <xref:System.Int32> valores.  
  
 [!code-csharp[Conceptual.Conversion#4](../../../samples/snippets/csharp/VS_Snippets_CLR/conceptual.conversion/cs/explicit1.cs#4)]
 [!code-vb[Conceptual.Conversion#4](../../../samples/snippets/visualbasic/VS_Snippets_CLR/conceptual.conversion/vb/explicit1.vb#4)]  
  
 As conversões explícitas podem produzir resultados diferentes em linguagens diferentes, e esses resultados podem diferir do valor retornado pelo método <xref:System.Convert> correspondente. Por exemplo, se o valor 12,63251 de <xref:System.Double> for convertido em <xref:System.Int32>, o método `CInt` do Visual Basic e o método <xref:System.Convert.ToInt32%28System.Double%29?displayProperty=nameWithType> do .NET Framework arredondam o valor de <xref:System.Double> para retornar um valor 13, mas o operador `(int)` do C# trunca <xref:System.Double> para retornar um valor 12. Da mesma forma, o operador `(int)` do C# não oferece suporte à conversão de booliano para inteiro, mas o método `CInt` do Visual Basic converte um valor `true` para -1. Por outro lado, o método <xref:System.Convert.ToInt32%28System.Boolean%29?displayProperty=nameWithType> converte um valor `true` para 1.  
  
 A maioria dos compiladores permite que as conversões explícitas sejam executadas de forma verificada ou não verificada. Quando uma conversão verificada é executada, uma <xref:System.OverflowException> é gerada quando o valor do tipo a ser convertido está fora do intervalo do tipo de destino. Quando uma conversão não verificada é executada nas mesmas condições, a conversão pode não gerar uma exceção, mas o comportamento exato se tornará indefinido e um valor incorreto poderá ser produzido.  
  
> [!NOTE]
> No C#, as conversões verificadas podem ser executadas usando a palavra-chave `checked` juntamente com um operador de conversão ou por meio da especificação da opção do compilador `/checked+`. Por outro lado, as conversões não verificadas podem ser executadas usando a palavra-chave `unchecked` juntamente com o operador de conversão ou especificando a opção do compilador `/checked-`. Por padrão, as conversões explícitas são do tipo não verificadas. No Visual Basic, as conversões verificadas podem ser executadas desmarcando-se a caixa de seleção **Remover Verificação de Estouro de Inteiro** na caixa de diálogo **Configurações Avançadas do Compilador** do projeto ou ao especificar a opção `/removeintchecks-` do compilador. Do mesmo modo, as conversões não verificadas podem ser executadas desmarcando-se a caixa de seleção **Remover Verificação de Estouro de Inteiro** na caixa de diálogo **Configurações Avançadas do Compilador** do projeto ou ao especificar a opção `/removeintchecks+` do compilador. Por padrão, as conversões explícitas são do tipo verificadas.  
  
 O exemplo em C# a seguir usa as palavras-chave `checked` e de `unchecked` para ilustrar a diferença no comportamento quando um valor fora do intervalo de <xref:System.Byte> é convertido em <xref:System.Byte>. A conversão verificada gera uma exceção, mas a conversão não verificada atribui <xref:System.Byte.MaxValue?displayProperty=nameWithType> à variável <xref:System.Byte>.  
  
 [!code-csharp[Conceptual.Conversion#12](../../../samples/snippets/csharp/VS_Snippets_CLR/conceptual.conversion/cs/explicit1.cs#12)]  
  
 Se o compilador de uma linguagem específica der suporte a operadores sobrecarregados personalizados, você também poderá definir as conversões explícitas em seus próprios tipos personalizados. O exemplo a seguir oferece uma implementação parcial de um tipo de dados de byte assinado chamado `ByteWithSign` que usa a representação de sinal e magnitude. Ele oferece suporte à conversão explícita dos valores <xref:System.Int32> e <xref:System.UInt32> para valores `ByteWithSign`.  
  
 [!code-csharp[Conceptual.Conversion#5](../../../samples/snippets/csharp/VS_Snippets_CLR/conceptual.conversion/cs/explicit1.cs#5)]
 [!code-vb[Conceptual.Conversion#5](../../../samples/snippets/visualbasic/VS_Snippets_CLR/conceptual.conversion/vb/explicit1.vb#5)]  
  
 O código cliente pode então declarar uma variável `ByteWithSign` e atribuir a ela valores <xref:System.Int32> e <xref:System.UInt32> quando as atribuições incluem um operador de conversão ou um método de conversão, conforme mostrado no exemplo a seguir.  
  
 [!code-csharp[Conceptual.Conversion#6](../../../samples/snippets/csharp/VS_Snippets_CLR/conceptual.conversion/cs/explicit1.cs#6)]
 [!code-vb[Conceptual.Conversion#6](../../../samples/snippets/visualbasic/VS_Snippets_CLR/conceptual.conversion/vb/explicit1.vb#6)]  

## <a name="the-iconvertible-interface"></a>Interface IConvertible  
 Para dar suporte à conversão de qualquer tipo em um tipo base Common Language Runtime base, o .NET Framework oferece a interface <xref:System.IConvertible>. O tipo de implementação é necessário para fornecer o seguinte:  
  
- Um método que retorne o <xref:System.TypeCode> do tipo implementando.  
  
- Métodos para converter o tipo implementando em cada tipo base do Common Language Runtime (<xref:System.Boolean>, <xref:System.Byte>, <xref:System.DateTime>, <xref:System.Decimal>, <xref:System.Double> e assim por diante).  
  
- Um método de conversão generalizado para converter uma instância do tipo de implementação em outro tipo especificado. As conversões sem suporte devem gerar uma <xref:System.InvalidCastException>.  
  
 Cada tipo base do Common Language Runtime (ou seja, <xref:System.Boolean>, <xref:System.Byte>, <xref:System.Char>, <xref:System.DateTime>, <xref:System.Decimal>, <xref:System.Double>, <xref:System.Int16>, <xref:System.Int32>, <xref:System.Int64>, <xref:System.SByte>, <xref:System.Single>, <xref:System.String>, <xref:System.UInt16>, <xref:System.UInt32>, e <xref:System.UInt64>), bem como os tipos <xref:System.DBNull> e <xref:System.Enum>, implementam a interface <xref:System.IConvertible>. No entanto, essas são implementações de interfaces explícitas; o método de conversão pode ser chamado somente com uma variável da interface <xref:System.IConvertible>, conforme mostrado no exemplo a seguir. Este exemplo converte um valor <xref:System.Int32> para seu valor <xref:System.Char> equivalente.  
  
 [!code-csharp[Conceptual.Conversion#7](../../../samples/snippets/csharp/VS_Snippets_CLR/conceptual.conversion/cs/iconvertible1.cs#7)]
 [!code-vb[Conceptual.Conversion#7](../../../samples/snippets/visualbasic/VS_Snippets_CLR/conceptual.conversion/vb/iconvertible1.vb#7)]  
  
 O requisito para chamar o método de conversão em sua interface, em vez de no tipo de implementação, torna as implementações de interfaces explícitas relativamente dispendiosas. Em vez disso, recomendamos chamar o membro apropriado da classe <xref:System.Convert> para fazer a conversão entre tipos base do Common Language Runtime. Para obter mais informações, consulte a próxima seção, [a classe Convert](#the-convert-class).  
  
> [!NOTE]
> Além da interface <xref:System.IConvertible> e da classe <xref:System.Convert> fornecidas pelo .NET Framework, as linguagens individuais também podem oferecer formas de executar conversões. Por exemplo, o C# usa operadores de conversão e Visual Basic usa funções de conversão implementadas no compilador, como `CType`,`CInt` e `DirectCast`.  
  
 Na maior parte, a interface <xref:System.IConvertible> é criada para dar suporte à conversão entre os tipos base no .NET Framework. No entanto, a interface também pode ser implementada por um tipo personalizado para oferecer suporte à conversão desse tipo em outros tipos personalizados. Para obter mais informações, consulte a seção [conversões personalizadas com o método altertype](#custom-conversions-with-the-changetype-method) mais adiante neste tópico.

## <a name="the-convert-class"></a>Classe Convert
 Embora a implementação da interface <xref:System.IConvertible> de cada tipo de base possa ser chamada para executar uma conversão de tipo, chamar os métodos da classe <xref:System.Convert?displayProperty=nameWithType> é a maneira recomendada independente de linguagem de converter de um tipo base para outro. Além disso, o método <xref:System.Convert.ChangeType%28System.Object%2CSystem.Type%2CSystem.IFormatProvider%29?displayProperty=nameWithType> pode ser usado para converter de um tipo personalizado especificado para outro tipo.  
  
### <a name="conversions-between-base-types"></a>Conversões entre tipos base  
 A classe <xref:System.Convert> fornece uma maneira independente de linguagem para realizar conversões entre tipos base e está disponível para todas as linguagens que visam o Common Language Runtime. Ele fornece um conjunto completo de métodos para ampliar e reduzir conversões, e gera uma <xref:System.InvalidCastException> para as conversões que não têm suporte (como a conversão de um valor <xref:System.DateTime> para um valor inteiro). As conversões de redução são executadas em um contexto verificado, e uma <xref:System.OverflowException> será gerada se a conversão falhar.  
  
> [!IMPORTANT]
> Como a classe <xref:System.Convert> inclui métodos para converter de/para cada tipo base, ela elimina a necessidade de chamar a implementação de interface explicita <xref:System.IConvertible> de cada tipo base.  
  
 O exemplo a seguir ilustra o uso da classe <xref:System.Convert?displayProperty=nameWithType> para executar várias conversões de ampliação e redução entre os tipos base do .NET Framework.  
  
 [!code-csharp[Conceptual.Conversion#8](../../../samples/snippets/csharp/VS_Snippets_CLR/conceptual.conversion/cs/convert1.cs#8)]
 [!code-vb[Conceptual.Conversion#8](../../../samples/snippets/visualbasic/VS_Snippets_CLR/conceptual.conversion/vb/convert1.vb#8)]  
  
 Em alguns casos, particularmente na conversão de/para valores de ponto flutuante, uma conversão pode envolver uma perda de precisão, mesmo que não gere uma <xref:System.OverflowException>. O exemplo a seguir ilustra essa perda de precisão. No primeiro caso, um valor <xref:System.Decimal> tem menos precisão (menos dígitos significativos) quando é convertida em <xref:System.Double>. No segundo caso, um valor <xref:System.Double> é arredondado de 42,72 para 43 para concluir a conversão.  
  
 [!code-csharp[Conceptual.Conversion#9](../../../samples/snippets/csharp/VS_Snippets_CLR/conceptual.conversion/cs/convert1.cs#9)]
 [!code-vb[Conceptual.Conversion#9](../../../samples/snippets/visualbasic/VS_Snippets_CLR/conceptual.conversion/vb/convert1.vb#9)]  
  
 Para uma tabela que lista as conversões de ampliação e redução aceitas pela classe <xref:System.Convert>, confira [Tabelas de conversão de tipos](conversion-tables.md).  

### <a name="custom-conversions-with-the-changetype-method"></a>Conversões personalizadas com o método ChangeType  
 Além de oferecer suporte a conversões para cada um dos tipos base, a classe <xref:System.Convert> pode ser usada para converter um tipo personalizado em um ou mais tipos predefinidos. Essa conversão é executada pelo método <xref:System.Convert.ChangeType%28System.Object%2CSystem.Type%2CSystem.IFormatProvider%29?displayProperty=nameWithType>, que por sua vez, envolve uma chamada ao método <xref:System.IConvertible.ToType%2A?displayProperty=nameWithType> do parâmetro `value`. Isso significa que o objeto representado pelo parâmetro `value` deve fornecer uma implementação da interface <xref:System.IConvertible>.  
  
> [!NOTE]
> Como os métodos <xref:System.Convert.ChangeType%28System.Object%2CSystem.Type%29?displayProperty=nameWithType> e <xref:System.Convert.ChangeType%28System.Object%2CSystem.Type%2CSystem.IFormatProvider%29?displayProperty=nameWithType> usam um objeto <xref:System.Type> para especificar o tipo de destino no qual `value` é convertido, eles podem ser usados para executar uma conversão dinâmica para um objeto cujo tipo não é conhecido em tempo de compilação. No entanto, observe que a implementação <xref:System.IConvertible> de `value` deve oferecer suporte a essa conversão.  
  
 O exemplo a seguir ilustra uma possível implementação da interface <xref:System.IConvertible> que permite que um objeto `TemperatureCelsius` seja convertido para um objeto `TemperatureFahrenheit` e vice-versa. O exemplo define uma classe base, `Temperature`, a qual implementa a interface <xref:System.IConvertible> e substitui o método <xref:System.Object.ToString%2A?displayProperty=nameWithType>. As classes `TemperatureCelsius` e `TemperatureFahrenheit` derivadas substituem os métodos `ToType` e `ToString` da classe base.  
  
 [!code-csharp[Conceptual.Conversion#10](../../../samples/snippets/csharp/VS_Snippets_CLR/conceptual.conversion/cs/iconvertible2.cs#10)]
 [!code-vb[Conceptual.Conversion#10](../../../samples/snippets/visualbasic/VS_Snippets_CLR/conceptual.conversion/vb/iconvertible2.vb#10)]  
  
 O exemplo a seguir ilustra várias chamadas às implementações de <xref:System.IConvertible> para converter objetos `TemperatureCelsius` para objetos `TemperatureFahrenheit` e vice-versa.  
  
 [!code-csharp[Conceptual.Conversion#11](../../../samples/snippets/csharp/VS_Snippets_CLR/conceptual.conversion/cs/iconvertible2.cs#11)]
 [!code-vb[Conceptual.Conversion#11](../../../samples/snippets/visualbasic/VS_Snippets_CLR/conceptual.conversion/vb/iconvertible2.vb#11)]  

## <a name="the-typeconverter-class"></a>Classe TypeConverter  
 O .NET Framework também permite que você defina um conversor de tipo para um tipo personalizado ao estender a classe <xref:System.ComponentModel.TypeConverter?displayProperty=nameWithType> e associar o conversor de tipo ao tipo por meio de um atributo <xref:System.ComponentModel.TypeConverterAttribute?displayProperty=nameWithType>. A tabela a seguir realça as diferenças entre esta abordagem e a implementação da interface <xref:System.IConvertible> para um tipo personalizado.  
  
> [!NOTE]
> O suporte no tempo de design somente poderá ser fornecido para um tipo personalizado se houver um conversor de tipo definido para ele.  
  
|Conversão usando TypeConverter|Conversão usando IConvertible|  
|------------------------------------|-----------------------------------|  
|É implementado para um tipo personalizado por meio da derivação de uma classe separada de <xref:System.ComponentModel.TypeConverter>. Essa classe derivada é associada ao tipo personalizado aplicando-se um atributo <xref:System.ComponentModel.TypeConverterAttribute>.|É implementada por um tipo personalizado para executar a conversão. Um usuário do tipo invoca um método de conversão <xref:System.IConvertible> no tipo.|  
|Pode ser usada tanto no tempo de design quanto no tempo de execução.|Somente pode ser usada no tempo de execução.|  
|Usa reflexão; consequentemente, é mais lento do que a conversão habilitada por <xref:System.IConvertible>.|Não usa reflexão.|  
|Permite conversões de tipo bidirecionais do tipo personalizado para outros tipos de dados e de outros tipos de dados para o tipo personalizado. Por exemplo, um <xref:System.ComponentModel.TypeConverter> definido para `MyType` permite conversões de `MyType` para <xref:System.String> e de <xref:System.String> para `MyType`.|Permite a conversão de um tipo personalizado em outros tipos de dados, mas não de outros tipos de dados em um tipo personalizado.|  
  
 Para obter mais informações sobre como usar conversores de tipo para executar conversões, consulte <xref:System.ComponentModel.TypeConverter?displayProperty=nameWithType>.  
  
## <a name="see-also"></a>Veja também

- <xref:System.Convert?displayProperty=nameWithType>
- <xref:System.IConvertible>
- [Tabelas de conversão de tipo](conversion-tables.md)
