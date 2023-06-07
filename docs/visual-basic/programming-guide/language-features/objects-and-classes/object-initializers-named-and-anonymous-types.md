---
title: 'Inicializadores de objeto: tipos nomeados e anônimos'
ms.date: 07/20/2015
f1_keywords:
- vb.ObjectInitializer
helpviewer_keywords:
- object initializers [Visual Basic]
- anonymous types [Visual Basic], object initializers
- initializing properties [Visual Basic]
- initializers [Visual Basic]
- named types [Visual Basic]
ms.assetid: e2df3807-a70f-49dd-ac94-f1e07f472b1b
ms.openlocfilehash: 5561812a53e2fe45c3ad4d12d0e18a8a1e948559
ms.sourcegitcommit: f8c270376ed905f6a8896ce0fe25b4f4b38ff498
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/04/2020
ms.locfileid: "84411760"
---
# <a name="object-initializers-named-and-anonymous-types-visual-basic"></a>Inicializadores de objeto: tipos nomeados e anônimos (Visual Basic)
Os inicializadores de objeto permitem especificar propriedades para um objeto complexo usando uma única expressão. Eles podem ser usados para criar instâncias de tipos nomeados e de tipos anônimos.  
  
## <a name="declarations"></a>Declarações  
 Declarações de instâncias de tipos nomeados e anônimos podem parecer quase idênticas, mas seus efeitos não são os mesmos. Cada categoria tem habilidades e restrições próprias. O exemplo a seguir mostra uma maneira conveniente de declarar e inicializar uma instância de uma classe nomeada, `Customer` , usando uma lista de inicializadores de objeto. Observe que o nome da classe é especificado após a palavra-chave `New` .  
  
 [!code-vb[VbVbalrObjectInit#1](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrObjectInit/VB/Class1.vb#1)]  
  
 Um tipo anônimo não tem nome utilizável. Portanto, uma instanciação de um tipo anônimo não pode incluir um nome de classe.  
  
 [!code-vb[VbVbalrObjectInit#2](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrObjectInit/VB/Class1.vb#2)]  
  
 Os requisitos e os resultados das duas declarações não são os mesmos. Para `namedCust` , uma `Customer` classe que tem uma `Name` Propriedade já deve existir e a declaração cria uma instância dessa classe. Para `anonymousCust` o, o compilador define uma nova classe que tem uma propriedade, uma cadeia `Name` de caracteres chamada e cria uma nova instância dessa classe.  
  
## <a name="named-types"></a>Tipos nomeados  
 Os inicializadores de objeto fornecem uma maneira simples de chamar o construtor de um tipo e, em seguida, definir os valores de algumas ou todas as propriedades em uma única instrução. O compilador invoca o construtor apropriado para a instrução: o construtor sem parâmetros se nenhum argumento for apresentado ou um construtor com parâmetros se um ou mais argumentos forem enviados. Depois disso, as propriedades especificadas são inicializadas na ordem em que são apresentadas na lista de inicializadores.  
  
 Cada inicialização na lista de inicializadores consiste na atribuição de um valor inicial a um membro da classe. Os nomes e tipos de dados dos membros são determinados quando a classe é definida. Nos exemplos a seguir, a `Customer` classe deve existir e deve ter membros chamados `Name` e `City` que pode aceitar valores de cadeia de caracteres.  
  
 [!code-vb[VbVbalrObjectInit#3](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrObjectInit/VB/Class1.vb#3)]  
  
 Como alternativa, você pode obter o mesmo resultado usando o seguinte código:  
  
 [!code-vb[VbVbalrObjectInit#4](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrObjectInit/VB/Class1.vb#4)]  
  
 Cada uma dessas declarações é equivalente ao exemplo a seguir, que cria um `Customer` objeto usando o construtor sem parâmetros e, em seguida, especifica os valores iniciais para as `Name` `City` Propriedades e usando uma `With` instrução.  
  
 [!code-vb[VbVbalrObjectInit#5](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrObjectInit/VB/Class1.vb#5)]  
  
 Se a `Customer` classe contiver um construtor com parâmetros que permita que você envie um valor para `Name` , por exemplo, você também pode declarar e inicializar um `Customer` objeto das seguintes maneiras:  
  
 [!code-vb[VbVbalrObjectInit#6](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrObjectInit/VB/Class1.vb#6)]  
  
 Você não precisa inicializar todas as propriedades, como mostra o código a seguir.  
  
 [!code-vb[VbVbalrObjectInit#7](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrObjectInit/VB/Class1.vb#7)]  
  
 No entanto, a lista de inicialização não pode ficar vazia. As propriedades não inicializadas retêm seus valores padrão.  
  
### <a name="type-inference-with-named-types"></a>Inferência de tipos com tipos nomeados  
 Você pode encurtar o código para a declaração de `cust1` combinando inicializadores de objeto e inferência de tipo local. Isso permite omitir a `As` cláusula na declaração da variável. O tipo de dados da variável é inferido a partir do tipo do objeto criado pela atribuição. No exemplo a seguir, o tipo de `cust6` é `Customer` .  
  
 [!code-vb[VbVbalrObjectInit#8](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrObjectInit/VB/Class1.vb#8)]  
  
### <a name="remarks-about-named-types"></a>Comentários sobre tipos nomeados  
  
- Um membro de classe não pode ser inicializado mais de uma vez na lista de inicializadores de objeto. A declaração de `cust7` causa um erro.  
  
     [!code-vb[VbVbalrObjectInit#9](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrObjectInit/VB/Class1.vb#9)]  
  
- Um membro pode ser usado para inicializar a si mesmo ou outro campo. Se um membro for acessado antes de ser inicializado, como na declaração a seguir para `cust8` , o valor padrão será usado. Lembre-se de que quando uma declaração que usa um inicializador de objeto é processada, a primeira coisa que acontece é que o construtor apropriado é invocado. Depois disso, os campos individuais na lista de inicializadores são inicializados. Nos exemplos a seguir, o valor padrão para `Name` é atribuído para `cust8` e um valor inicializado é atribuído em `cust9` .  
  
     [!code-vb[VbVbalrObjectInit#10](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrObjectInit/VB/Class1.vb#10)]  
  
     O exemplo a seguir usa o construtor com parâmetros de `cust3` e `cust4` para declarar e inicializar `cust10` e `cust11` .  
  
     [!code-vb[VbVbalrObjectInit#11](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrObjectInit/VB/Class1.vb#11)]  
  
- Os inicializadores de objeto podem ser aninhados. No exemplo a seguir, `AddressClass` é uma classe que tem duas propriedades `City` e `State` , e a `Customer` classe tem uma `Address` propriedade que é uma instância do `AddressClass` .  
  
     [!code-vb[VbVbalrObjectInit#12](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrObjectInit/VB/Class1.vb#12)]  
  
- A lista de inicialização não pode ficar vazia.  
  
- A instância que está sendo inicializada não pode ser do tipo Object.  
  
- Membros de classe que estão sendo inicializados não podem ser membros compartilhados, membros somente leitura, constantes ou chamadas de método.  
  
- Membros de classe que estão sendo inicializados não podem ser indexados ou qualificados. Os exemplos a seguir geram erros do compilador:  
  
     `'' Not valid.`  
  
     `' Dim c1 = New Customer With {.OrderNumbers(0) = 148662}`  
  
     `' Dim c2 = New Customer with {.Address.City = "Springfield"}`  
  
## <a name="anonymous-types"></a>Tipos anônimos  
 Os tipos anônimos usam inicializadores de objeto para criar instâncias de novos tipos que você não define explicitamente e nomear. Em vez disso, o compilador gera um tipo de acordo com as propriedades que você designa na lista de inicializadores de objeto. Como o nome do tipo não é especificado, ele é conhecido como um *tipo anônimo*. Por exemplo, compare a declaração a seguir para a anterior para `cust6` .  
  
 [!code-vb[VbVbalrObjectInit#13](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrObjectInit/VB/Class1.vb#13)]  
  
 A única diferença sintaticamente é que nenhum nome é especificado depois `New` para o tipo de dados. No entanto, o que acontece é bastante diferente. O compilador define um novo tipo anônimo que tem duas propriedades `Name` e `City` cria uma instância dela com os valores especificados. A inferência de tipos determina os tipos de `Name` e, `City` no exemplo, para serem cadeias de caracteres.  
  
> [!CAUTION]
> O nome do tipo anônimo é gerado pelo compilador e pode variar de compilação para compilação. Seu código não deve usar ou depender do nome de um tipo anônimo.  
  
 Como o nome do tipo não está disponível, você não pode usar uma `As` cláusula para declarar `cust13` . Seu tipo deve ser inferido. Sem usar a ligação tardia, isso limita o uso de tipos anônimos a variáveis locais.  
  
 Tipos anônimos fornecem suporte crítico para consultas LINQ. Para obter mais informações sobre o uso de tipos anônimos em consultas, consulte [tipos anônimos](anonymous-types.md) e [introdução ao LINQ no Visual Basic](../linq/introduction-to-linq.md).  
  
### <a name="remarks-about-anonymous-types"></a>Comentários sobre tipos anônimos  
  
- Normalmente, todas ou a maioria das propriedades em uma declaração de tipo anônimo serão Propriedades de chave, que são indicadas digitando-se a palavra-chave `Key` na frente do nome da propriedade.  
  
     [!code-vb[VbVbalrObjectInit#14](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrObjectInit/VB/Class1.vb#14)]  
  
     Para obter mais informações sobre as propriedades de chave, consulte [chave](../../../language-reference/modifiers/key.md).  
  
- Como tipos nomeados, listas de inicializadores para definições de tipo anônimo devem declarar pelo menos uma propriedade.  
  
     [!code-vb[VbVbalrObjectInit#2](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrObjectInit/VB/Class1.vb#2)]  
  
- Quando uma instância de um tipo anônimo é declarada, o compilador gera uma definição de tipo anônimo correspondente. Os nomes e tipos de dados das propriedades são obtidos da declaração de instância e são incluídos pelo compilador na definição. As propriedades não são nomeadas e definidas com antecedência, como seriam para um tipo nomeado. Seus tipos são inferidos. Você não pode especificar os tipos de dados das propriedades usando uma `As` cláusula.  
  
- Os tipos anônimos também podem estabelecer os nomes e valores de suas propriedades de várias maneiras. Por exemplo, uma propriedade de tipo anônimo pode pegar o nome e o valor de uma variável, ou o nome e o valor de uma propriedade de outro objeto.  
  
     [!code-vb[VbVbalrObjectInit#15](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrObjectInit/VB/Class1.vb#15)]  
  
     Para obter mais informações sobre as opções para definir propriedades em tipos anônimos, consulte [como: inferir nomes e tipos de propriedade em declarações de tipo anônimo](how-to-infer-property-names-and-types-in-anonymous-type-declarations.md).  
  
## <a name="see-also"></a>Confira também

- [Inferência de Tipo de Variável Local](../variables/local-type-inference.md)
- [Tipos anônimos](anonymous-types.md)
- [Introdução a LINQ no Visual Basic](../linq/introduction-to-linq.md)
- [Como inferir nomes e tipos de propriedade na declaração de tipo anônimo](how-to-infer-property-names-and-types-in-anonymous-type-declarations.md)
- [Chave](../../../language-reference/modifiers/key.md)
- [Como declarar um objeto usando um inicializador de objeto](how-to-declare-an-object-by-using-an-object-initializer.md)
