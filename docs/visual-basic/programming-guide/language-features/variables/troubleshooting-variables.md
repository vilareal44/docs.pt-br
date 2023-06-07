---
title: Solução de problemas de variáveis
ms.date: 07/20/2015
helpviewer_keywords:
- troubleshooting [Visual Basic], variables
- variables [Visual Basic], troubleshooting
ms.assetid: 928a2dc8-e565-4ae4-8ba3-80cc0cb50090
ms.openlocfilehash: 3efecf3883e5a1f49308af732a89ff66dc8b2421
ms.sourcegitcommit: f8c270376ed905f6a8896ce0fe25b4f4b38ff498
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/04/2020
ms.locfileid: "84410316"
---
# <a name="troubleshooting-variables-in-visual-basic"></a>Solucionando problemas de variáveis no Visual Basic
Esta página lista alguns problemas comuns que podem ocorrer ao trabalhar com variáveis no Visual Basic.  
  
## <a name="unable-to-access-members-of-an-object"></a>Não é possível acessar os membros de um objeto  
 Se o seu código tentar acessar uma propriedade ou um método em um objeto, haverá dois resultados de erro possíveis:  
  
- O compilador pode gerar uma mensagem de erro se você declarar a variável de objeto para ser de um tipo específico e, em seguida, referir-se a um membro não definido por esse tipo.  
  
- Um tempo de execução <xref:System.MemberAccessException> ocorre quando o objeto atribuído a uma variável de objeto não expõe o membro que seu código está tentando acessar. No caso de uma variável de [tipo de dados Object](../../../language-reference/data-types/object-data-type.md), você também poderá obter essa exceção se o membro não for `Public` . Isso ocorre porque a associação tardia permite acesso somente a `Public` Membros.  
  
 Quando a [instrução Option Strict](../../../language-reference/statements/option-strict-statement.md) define a verificação de tipo `On` , uma variável de objeto pode acessar somente os métodos e as propriedades da classe com a qual você o declara. O exemplo a seguir ilustra isto.  

 [!code-vb[VbVbalrVariables#2](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrVariables/VB/Class1.vb#2)]  
  
 Neste exemplo, `p` o pode usar somente os membros da <xref:System.Object> própria classe, que não incluem a `Left` propriedade. Por outro lado, `q` foi declarado como de tipo <xref:System.Windows.Forms.Label> , portanto, ele pode usar todos os métodos e propriedades da <xref:System.Windows.Forms.Label> classe no <xref:System.Windows.Forms> namespace.  
  
### <a name="correct-approach"></a>Abordagem correta  
 Para poder acessar todos os membros de um objeto de uma determinada classe, declare a variável de objeto para ser do tipo dessa classe quando possível. Se não for possível fazer isso, por exemplo, se você não souber o tipo de objeto em tempo de compilação, deverá definir `Option Strict` como `Off` e declarar a variável como sendo do [tipo de dados Object](../../../language-reference/data-types/object-data-type.md). Isso permite que os objetos de qualquer tipo sejam atribuídos à variável, e você deve tomar medidas para garantir que o objeto atribuído atualmente seja de um tipo aceitável. Você pode usar o [operador typeof](../../../language-reference/operators/typeof-operator.md) para fazer essa determinação.  
  
## <a name="other-components-cannot-access-your-variable"></a>Outros componentes não podem acessar sua variável  
 Os nomes de Visual Basic diferenciam *maiúsculas de minúsculas*. Se dois nomes forem diferentes somente no caso alfabético, o compilador os interpretará como o mesmo nome. Por exemplo, ele considera `ABC` e `abc` se refere ao mesmo elemento declarado.  
  
 No entanto, o Common Language Runtime (CLR) usa a associação que *diferencia maiúsculas de minúsculas* . Portanto, quando você produz um assembly ou uma DLL e o disponibiliza para outros assemblies, seus nomes não são mais sensíveis a maiúsculas e minúsculas. Por exemplo, se você definir uma classe com um elemento chamado `ABC` , e outros assemblies fizerem uso de sua classe por meio da Common Language Runtime, eles deverão se referir ao elemento como `ABC` . Se, posteriormente, você recompilar sua classe e alterar o nome do elemento para `abc` , os outros assemblies que usam sua classe não poderão mais acessar esse elemento. Portanto, quando você libera uma versão atualizada de um assembly, não deve alterar o caso alfabético de quaisquer elementos públicos.  
  
 Para obter mais informações, consulte [Common Language Runtime](../../../../standard/clr.md).  
  
### <a name="correct-approach"></a>Abordagem correta  
 Para permitir que outros componentes acessem suas variáveis, trate seus nomes como se eles diferenciassem maiúsculas de minúsculas. Quando você estiver testando sua classe ou módulo, verifique se outros assemblies estão sendo vinculados às variáveis que você espera. Depois de publicar um componente, não faça modificações em nomes de variáveis existentes, incluindo a alteração de seus casos.  
  
## <a name="wrong-variable-being-used"></a>Variável incorreta sendo usada  
 Quando você tem mais de uma variável com o mesmo nome, o compilador Visual Basic tenta resolver cada referência a esse nome. Se as variáveis tiverem escopo diferente, o compilador resolverá uma referência à declaração com o escopo mais estreito. Se eles tiverem o mesmo escopo, a resolução falhará e o compilador sinalizará um erro. Para obter mais informações, consulte [referências a elementos declarados](../declared-elements/references-to-declared-elements.md).  
  
### <a name="correct-approach"></a>Abordagem correta  
 Evite usar variáveis com o mesmo nome, mas com escopo diferente. Se você estiver usando outros assemblies ou projetos, evite usar quaisquer nomes definidos nesses componentes externos tanto quanto possível. Se você tiver mais de uma variável com o mesmo nome, certifique-se de qualificar todas as referências a ela. Para obter mais informações, consulte [referências a elementos declarados](../declared-elements/references-to-declared-elements.md).  
  
## <a name="see-also"></a>Confira também

- [Variáveis](index.md)
- [Declaração de Variável](variable-declaration.md)
- [Variáveis de Objeto](object-variables.md)
- [Declaração de Variável do Objeto](object-variable-declaration.md)
- [Como acessar membros de um objeto](how-to-access-members-of-an-object.md)
- [Valores de Variável de Objeto](object-variable-values.md)
- [Como determinar a que tipo uma variável de objeto se refere](how-to-determine-what-type-an-object-variable-refers-to.md)
- [Referências a elementos declarados](../declared-elements/references-to-declared-elements.md)
- [Nomes de elementos declarados](../declared-elements/declared-element-names.md)
