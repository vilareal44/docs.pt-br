---
title: Argumentos nomeados e opcionais – Guia de Programação em C#
description: Argumentos nomeados em C# especificam argumentos por nome, não posição. Argumentos opcionais podem ser omitidos.
ms.date: 07/20/2015
f1_keywords:
- namedParameter_CSharpKeyword
- cs_namedParameter
helpviewer_keywords:
- parameters [C#], named
- named arguments [C#]
- arguments [C#], named
- optional arguments [C#]
- arguments [C#], optional
- parameters [C#], optional
- named and optional arguments [C#]
ms.assetid: 839c960c-c2dc-4d05-af4d-ca5428e54008
ms.openlocfilehash: 46b9dc23644e68aea2767f2b990fe7f243a4f357
ms.sourcegitcommit: 3d84eac0818099c9949035feb96bbe0346358504
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/21/2020
ms.locfileid: "86864976"
---
# <a name="named-and-optional-arguments-c-programming-guide"></a>Argumentos nomeados e opcionais (Guia de Programação em C#)
O C# 4 apresenta argumentos nomeados e opcionais. *Argumentos nomeados* permitem especificar um argumento para um parâmetro específico associando o argumento ao nome do parâmetro e não com à posição do parâmetro na lista de parâmetros. *Argumentos opcionais* permitem omitir argumentos para alguns parâmetros. Ambas as técnicas podem ser usadas com os métodos, indexadores, construtores e delegados.  
  
 Quando você usa argumentos nomeados e opcionais, os argumentos são avaliados na ordem em que aparecem na lista de argumentos e não na lista de parâmetros.  
  
 Os parâmetros nomeados e opcionais, quando usados em conjunto, permitem que você forneça argumentos para apenas alguns parâmetros de uma lista de parâmetros opcionais. Essa capacidade facilita bastante a chamadas para interfaces COM como as APIs de Automação do Microsoft Office.  
  
## <a name="named-arguments"></a>Argumentos nomeados  
 Os argumentos nomeados liberam você da necessidade de lembrar ou procurar a ordem dos parâmetros nas listas de parâmetros de métodos chamados. O parâmetro para cada argumento pode ser especificado pelo nome do parâmetro. Por exemplo, uma função que imprime detalhes de pedidos (como o nome do vendedor, nome do produto e número do pedido) pode ser chamada da maneira padrão, por meio do envio de argumentos por posição, na ordem definida pela função.
  
 `PrintOrderDetails("Gift Shop", 31, "Red Mug");`
  
 Se não se lembrar da ordem dos parâmetros, mas souber os nomes, você poderá enviar os argumentos em qualquer ordem.  
  
 `PrintOrderDetails(orderNum: 31, productName: "Red Mug", sellerName: "Gift Shop");`
  
 `PrintOrderDetails(productName: "Red Mug", sellerName: "Gift Shop", orderNum: 31);`
  
 Os argumentos nomeados também melhoram a legibilidade do código identificando o que cada argumento representa. No método de exemplo abaixo, o `sellerName` não pode ser nulo ou um espaço em branco. Como `sellerName` e `productName` são tipos de cadeia de caracteres, em vez de enviar argumentos por posição, é melhor usar argumentos nomeados para remover a ambiguidade dos dois e reduzir a confusão para qualquer pessoa que leia o código.
  
 Os argumentos nomeados, quando usados com argumentos posicionais, são válidos, desde que

- não sejam seguidos por argumentos posicionais ou,

 `PrintOrderDetails("Gift Shop", 31, productName: "Red Mug");`

- _começando com o C# 7.2_, sejam usados na posição correta. No exemplo a seguir, o parâmetro `orderNum` está na posição correta, mas não está explicitamente nomeado.

 `PrintOrderDetails(sellerName: "Gift Shop", 31, productName: "Red Mug");`
  
 Argumentos posicionais que seguem os argumentos nomeados fora de ordem são inválidos.

 ```csharp
 // This generates CS1738: Named argument specifications must appear after all fixed arguments have been specified.
 PrintOrderDetails(productName: "Red Mug", 31, "Gift Shop");
 ```
  
## <a name="example"></a>Exemplo  
 O código a seguir implementa os exemplos desta seção, juntamente com outros exemplos.  
  
 [!code-csharp[csProgGuideNamedAndOptional#1](~/samples/snippets/csharp/VS_Snippets_VBCSharp/csprogguidenamedandoptional/cs/program.cs#1)]  
  
## <a name="optional-arguments"></a>Argumentos opcionais  
 A definição de um método, construtor, indexador ou delegado pode especificar que seus parâmetros são obrigatórios ou que são opcionais. Qualquer chamada deve fornecer argumentos para todos os parâmetros necessários, mas pode omitir argumentos para parâmetros opcionais.  
  
 Cada parâmetro opcional tem um valor padrão como parte de sua definição. Se nenhum argumento é enviado para esse parâmetro, o valor padrão é usado. Um valor padrão deve ser um dos seguintes tipos de expressões:  
  
- uma expressão de constante;  
  
- uma expressão da forma `new ValType()`, em que `ValType` é um tipo de valor, como um [enum](../../language-reference/builtin-types/enum.md) ou um [struct](../../language-reference/builtin-types/struct.md);  
  
- uma expressão da forma [default(ValType)](../../language-reference/operators/default.md), em que `ValType` é um tipo de valor.  
  
 Os parâmetros opcionais são definidos no final da lista de parâmetros, depois de todos os parâmetros obrigatórios. Se o chamador fornecer um argumento para qualquer um de uma sucessão de parâmetros opcionais, ele deverá fornecer argumentos para todos os parâmetros opcionais anteriores. Não há suporte para intervalos separados por vírgula na lista de argumentos. Por exemplo, no código a seguir, método de instância `ExampleMethod` está definido com um parâmetro obrigatório e dois opcionais.  
  
 [!code-csharp[csProgGuideNamedAndOptional#15](~/samples/snippets/csharp/VS_Snippets_VBCSharp/csprogguidenamedandoptional/cs/optional.cs#15)]  
  
 A chamada para `ExampleMethod` a seguir causa um erro do compilador, porque um argumento é fornecido para o terceiro parâmetro, mas não para o segundo.  
  
 `//anExample.ExampleMethod(3, ,4);`  
  
 No entanto, se você souber o nome do terceiro parâmetro, poderá usar um argumento nomeado para realizar a tarefa.  
  
 `anExample.ExampleMethod(3, optionalint: 4);`  
  
 O IntelliSense usa colchetes para indicar parâmetros opcionais, conforme mostrado na seguinte ilustração:  
  
 ![Captura de tela mostrando as Informações Rápidas do IntelliSense para o método ExampleMethod.](./media/named-and-optional-arguments/optional-examplemethod-parameters.png)  
  
> [!NOTE]
> Você também pode declarar parâmetros opcionais usando a classe <xref:System.Runtime.InteropServices.OptionalAttribute> do .NET. Os parâmetros `OptionalAttribute` não exigem um valor padrão.  
  
## <a name="example"></a>Exemplo  
 No exemplo a seguir, o construtor para `ExampleClass` tem um parâmetro, que é opcional. O método de instância `ExampleMethod` tem um parâmetro obrigatório, `required` e dois parâmetros opcionais, `optionalstr` e `optionalint`. O código em `Main` mostra as diferentes maneiras em que o construtor e o método podem ser invocados.  
  
 [!code-csharp[csProgGuideNamedAndOptional#2](~/samples/snippets/csharp/VS_Snippets_VBCSharp/csprogguidenamedandoptional/cs/optional.cs#2)]  
  
## <a name="com-interfaces"></a>Interfaces COM  
 Os argumentos nomeados e opcionais, juntamente com suporte para objetos dinâmicos e outros aprimoramentos, aprimoram enormemente a interoperabilidade com APIs COM, como APIs de Automação do Office.  
  
 Por exemplo, o método <xref:Microsoft.Office.Interop.Excel.Range.AutoFormat%2A> na interface <xref:Microsoft.Office.Interop.Excel.Range> do Microsoft Office Excel tem sete parâmetros, todos opcionais. Esses parâmetros são mostrados na seguinte ilustração:  
  
 ![Captura de tela mostrando as Informações Rápidas do IntelliSense para o método AutoFormat.](./media/named-and-optional-arguments/autoformat-method-parameters.png)  
  
 No C# 3.0 e versões anteriores, é necessário um argumento para cada parâmetro, como mostrado no exemplo a seguir.  
  
 [!code-csharp[csProgGuideNamedAndOptional#3](~/samples/snippets/csharp/VS_Snippets_VBCSharp/csprogguidenamedandoptional/cs/namedandoptcom.cs#3)]  
  
 No entanto, você pode simplificar muito a chamada para `AutoFormat` usando argumentos nomeados e opcionais, introduzidos no C# 4.0. Os argumentos nomeados e opcionais permitem que você omita o argumento para um parâmetro opcional se não desejar alterar o valor padrão do parâmetro. Na chamada a seguir, um valor é especificado para apenas um dos sete parâmetros.  
  
 [!code-csharp[csProgGuideNamedAndOptional#13](~/samples/snippets/csharp/VS_Snippets_VBCSharp/csprogguidenamedandoptional/cs/namedandoptcom.cs#13)]  
  
 Para obter mais informações e exemplos, consulte [como usar argumentos nomeados e opcionais na programação do Office](./how-to-use-named-and-optional-arguments-in-office-programming.md) e [como acessar objetos de interoperabilidade do Office usando recursos do C#](../interop/how-to-access-office-onterop-objects.md).  
  
## <a name="overload-resolution"></a>Resolução de sobrecarga  
 O uso de argumentos nomeados e opcionais afeta a resolução de sobrecarga das seguintes maneiras:  
  
- Um método, indexador ou construtor é um candidato para a execução se cada um dos parâmetros é opcional ou corresponde, por nome ou posição, a um único argumento na instrução de chamada e esse argumento pode ser convertido para o tipo do parâmetro.  
  
- Se mais de um candidato for encontrado, as regras de resolução de sobrecarga de conversões preferenciais serão aplicadas aos argumentos que são especificados explicitamente. Os argumentos omitidos para parâmetros opcionais são ignorados.  
  
- Se dois candidatos são considerados igualmente bons, a preferência vai para um candidato que não tem parâmetros opcionais para os quais argumentos foram omitidos na chamada. Esta é uma consequência da preferência geral na resolução de sobrecarga de candidatos que têm menos parâmetros.  
  
## <a name="c-language-specification"></a>Especificação da Linguagem C#  
 [!INCLUDE[CSharplangspec](~/includes/csharplangspec-md.md)]  
  
## <a name="see-also"></a>Veja também

- [Como usar argumentos nomeados e opcionais na programação do Office](./how-to-use-named-and-optional-arguments-in-office-programming.md)
- [Usando o tipo dynamic](../types/using-type-dynamic.md)
- [Usando construtores](./using-constructors.md)
- [Usando indexadores](../indexers/using-indexers.md)
