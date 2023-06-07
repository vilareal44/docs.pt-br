---
title: Propriedades autoimplementadas
ms.date: 07/20/2015
f1_keywords:
- vb.AutoProperty
- vb.AutoImplementedProperty
helpviewer_keywords:
- properties [Visual Basic], auto-implemented
- auto-implemented properties [Visual Basic]
ms.assetid: 5c669f0b-cf95-4b4e-ae84-9cc55212ca87
ms.openlocfilehash: d991a385e537c43daeb708e96e712acd92110379
ms.sourcegitcommit: f8c270376ed905f6a8896ce0fe25b4f4b38ff498
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/04/2020
ms.locfileid: "84403376"
---
# <a name="auto-implemented-properties-visual-basic"></a>Propriedades autoimplementadas (Visual Basic)
*As propriedades implementadas automaticamente* permitem que você especifique rapidamente uma propriedade de uma classe sem precisar gravar o código `Get` e `Set` a propriedade. Quando você escreve o código para uma propriedade implementada automaticamente, o compilador Visual Basic cria automaticamente um campo particular para armazenar a variável de propriedade, além de criar `Get` os `Set` procedimentos e associados.  
  
 Com propriedades implementadas automaticamente, uma propriedade, incluindo um valor padrão, pode ser declarada em uma única linha. O exemplo a seguir mostra três declarações de propriedade.  
  
 [!code-vb[VbVbalrAutoImplementedProperties#1](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/vbvbalrautoimplementedproperties/vb/module1.vb#1)]  
  
 Uma propriedade implementada automaticamente é equivalente a uma propriedade para a qual o valor da propriedade é armazenado em um campo particular. O exemplo de código a seguir mostra uma propriedade implementada automaticamente.  
  
 [!code-vb[VbVbalrAutoImplementedProperties#5](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/vbvbalrautoimplementedproperties/vb/module1.vb#5)]  
  
 O exemplo de código a seguir mostra o código equivalente para o exemplo de propriedade implementada automaticamente anterior.  
  
 [!code-vb[VbVbalrAutoImplementedProperties#2](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/vbvbalrautoimplementedproperties/vb/module1.vb#2)]  
  
 O código a seguir mostra a implementação de propriedades ReadOnly:  
  
```vb  
Class Customer  
   Public ReadOnly Property Tags As New List(Of String)  
   Public ReadOnly Property Name As String = ""  
   Public ReadOnly Property File As String  
  
   Sub New(file As String)  
      Me.File = file  
   End Sub  
End Class  
```  
  
 Você pode atribuir à propriedade com expressões de inicialização, conforme mostrado no exemplo, ou pode atribuir às propriedades no construtor do tipo recipiente.  Você pode atribuir aos campos de backup das propriedades ReadOnly a qualquer momento.  
  
## <a name="backing-field"></a>Campo de apoio  
 Quando você declara uma propriedade implementada automaticamente, Visual Basic cria automaticamente um campo privado oculto chamado *campo de backup* para conter o valor da propriedade. O nome do campo de backup é o nome da propriedade autoimplementada precedida por um sublinhado (_). Por exemplo, se você declarar uma propriedade implementada automaticamente chamada `ID` , o campo de backup será nomeado `_ID` . Se você incluir um membro de sua classe que também é nomeado `_ID` , produzirá um conflito de nomenclatura e Visual Basic relatará um erro do compilador.  
  
 O campo de backup também tem as seguintes características:  
  
- O modificador de acesso para o campo de backup é sempre `Private` , mesmo quando a própria propriedade tem um nível de acesso diferente, como `Public` .  
  
- Se a propriedade estiver marcada como `Shared` , o campo de backup também será compartilhado.  
  
- Os atributos especificados para a propriedade não se aplicam ao campo de backup.  
  
- O campo de backup pode ser acessado a partir do código dentro da classe e de ferramentas de depuração, como o janela Inspeção. No entanto, o campo de backup não aparece em uma lista de preenchimento de palavra do IntelliSense.  
  
## <a name="initializing-an-auto-implemented-property"></a>Inicializando uma propriedade implementada automaticamente  
 Qualquer expressão que possa ser usada para inicializar um campo é válida para inicializar uma propriedade implementada automaticamente. Quando você Inicializa uma propriedade implementada automaticamente, a expressão é avaliada e passada para o `Set` procedimento para a propriedade. Os exemplos de código a seguir mostram algumas propriedades implementadas automaticamente que incluem valores iniciais.  
  
 [!code-vb[VbVbalrAutoImplementedProperties#3](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/vbvbalrautoimplementedproperties/vb/module1.vb#3)]  
  
 Não é possível inicializar uma propriedade implementada automaticamente que seja membro de um `Interface` ou um que esteja marcado como `MustOverride` .  
  
 Ao declarar uma propriedade autoimplementada como membro de um `Structure` , você só poderá inicializar a propriedade autoimplementada se ela estiver marcada como `Shared` .  
  
 Quando você declara uma propriedade implementada automaticamente como uma matriz, não é possível especificar limites de matriz explícitos. No entanto, você pode fornecer um valor usando um inicializador de matriz, conforme mostrado nos exemplos a seguir.  
  
 [!code-vb[VbVbalrAutoImplementedProperties#4](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/vbvbalrautoimplementedproperties/vb/module1.vb#4)]  
  
## <a name="property-definitions-that-require-standard-syntax"></a>Definições de propriedade que exigem sintaxe padrão  
 As propriedades implementadas automaticamente são convenientes e dão suporte a muitos cenários de programação. No entanto, há situações em que você não pode usar uma propriedade autoimplementada e, em vez disso, usar a sintaxe de propriedade Standard ou *Expanded*.  
  
 Você precisa usar a sintaxe de definição de propriedade expandida se desejar fazer um dos seguintes:  
  
- Adicione código ao `Get` procedimento ou `Set` de uma propriedade, como código para validar os valores de entrada no `Set` procedimento. Por exemplo, talvez você queira verificar se uma cadeia de caracteres que representa um número de telefone contém o número necessário de numerais antes de definir o valor da propriedade.  
  
- Especifique a acessibilidade diferente para `Get` o `Set` procedimento e. Por exemplo, talvez você queira fazer o `Set` procedimento `Private` e o `Get` procedimento `Public` .  
  
- Crie propriedades que são `WriteOnly` .  
  
- Use propriedades parametrizadas (incluindo `Default` Propriedades). Você deve declarar uma propriedade expandida para especificar um parâmetro para a propriedade ou para especificar parâmetros adicionais para o `Set` procedimento.  
  
- Coloque um atributo no campo de apoio ou altere o nível de acesso do campo de backup.  
  
- Forneça comentários XML para o campo de backup.  
  
## <a name="expanding-an-auto-implemented-property"></a>Expandindo uma propriedade implementada automaticamente  
 Se você precisar converter uma propriedade autoimplementada em uma propriedade expandida que contenha `Get` um `Set` procedimento ou, o editor de código Visual Basic poderá gerar automaticamente os procedimentos e a `Get` instrução da `Set` `End Property` propriedade. O código será gerado se você colocar o cursor em uma linha em branco após a `Property` instrução, digite um `G` (para `Get` ) ou um `S` (para `Set` ) e pressione Enter. O editor de código Visual Basic gera automaticamente `Get` o `Set` procedimento ou para propriedades somente leitura e somente gravação quando você pressiona Enter no final de uma `Property` instrução.  
  
## <a name="see-also"></a>Confira também

- [Como declarar e chamar uma propriedade padrão no Visual Basic](./how-to-declare-and-call-a-default-property.md)
- [Como declarar uma propriedade com níveis de acesso mistos](./how-to-declare-a-property-with-mixed-access-levels.md)
- [Instrução Property](../../../language-reference/statements/property-statement.md)
- [Leitura](../../../language-reference/modifiers/readonly.md)
- [WriteOnly](../../../language-reference/modifiers/writeonly.md)
- [Objetos e classes](../objects-and-classes/index.md)
