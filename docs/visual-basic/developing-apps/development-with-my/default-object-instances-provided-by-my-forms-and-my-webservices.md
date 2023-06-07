---
title: Instâncias de objeto padrão fornecidas por My.Forms e My.WebServices
ms.date: 07/20/2015
helpviewer_keywords:
- My.WebServices object [Visual Basic], developing applications
- My.Forms object [Visual Basic], developing applications
- rapid application development (RAD), My.Forms
- rapid application development (RAD), My.WebServices
ms.assetid: de930027-9108-4f0c-b97c-5e7db4d6ef79
ms.openlocfilehash: 141f2f5f98499498d3c6732f7ae8d0abe6259ed9
ms.sourcegitcommit: 45c8eed045779b70a47b23169897459d0323dc89
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/18/2020
ms.locfileid: "84990241"
---
# <a name="default-object-instances-provided-by-myforms-and-mywebservices-visual-basic"></a>Instâncias de objeto padrão fornecidas por My.Forms e My.WebServices (Visual Basic)

Os objetos [My. Forms](../../language-reference/objects/my-forms-object.md) e [My. WebServices](../../language-reference/objects/my-webservices-object.md) fornecem acesso a formulários, fontes de dados e Web Services XML usados pelo seu aplicativo. Eles fornecem acesso por meio de coleções de *instâncias padrão* de cada um desses objetos.  
  
## <a name="default-instances"></a>Instâncias padrão  

 Uma instância padrão é uma instância da classe fornecida pelo tempo de execução e não precisa ser declarada e instanciada usando as `Dim` `New` instruções e. O exemplo a seguir demonstra como você pode ter declarado e instanciado uma instância de uma <xref:System.Windows.Forms.Form> classe chamada `Form1` e como agora é possível obter uma instância padrão dessa <xref:System.Windows.Forms.Form> classe por meio de `My.Forms` .  
  
 [!code-vb[VbVbcnMy#5](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbcnMy/VB/Class1.vb#5)]  
  
 [!code-vb[VbVbcnMy#6](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbcnMy/VB/Class1.vb#6)]  
  
 O `My.Forms` objeto retorna uma coleção de instâncias padrão para cada `Form` classe que existe em seu projeto. Da mesma forma, `My.WebServices` o fornece uma instância padrão da classe proxy para cada serviço Web ao qual você criou uma referência em seu aplicativo.  
  
## <a name="see-also"></a>Confira também

- [Objeto My.Forms](../../language-reference/objects/my-forms-object.md)
- [Objeto My.WebServices](../../language-reference/objects/my-webservices-object.md)
- [Como My depende do tipo de projeto](how-my-depends-on-project-type.md)
