---
title: Como My depende do tipo de projeto
ms.date: 07/20/2015
helpviewer_keywords:
- _MYTYPE
ms.assetid: c188b38e-bd9d-4121-9983-41ea6a94d28e
ms.openlocfilehash: 740185d8030c09e8813bc7680b451f6326588593
ms.sourcegitcommit: f8c270376ed905f6a8896ce0fe25b4f4b38ff498
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/04/2020
ms.locfileid: "84411708"
---
# <a name="how-my-depends-on-project-type-visual-basic"></a>Como My depende do tipo de projeto (Visual Basic)

`My`expõe somente os objetos exigidos por um tipo de projeto específico. Por exemplo, o `My.Forms` objeto está disponível em um aplicativo Windows Forms, mas não está disponível em um aplicativo de console. Este tópico descreve quais `My` objetos estão disponíveis em diferentes tipos de projeto.  
  
## <a name="my-in-windows-applications-and-web-sites"></a>Meu nos aplicativos e sites do Windows  

 `My`expõe somente objetos que são úteis no tipo de projeto atual; Ele suprime objetos que não são aplicáveis. Por exemplo, a imagem a seguir mostra o `My` modelo de objeto em um projeto Windows Forms.  
  
 ![Diagrama que mostra o modelo de objeto My em um aplicativo Windows Forms.](./media/how-my-depends-on-project-type/my-object-model-windows-forms.png)  
  
 Em um projeto de site da Web, `My` o expõe objetos que são relevantes para um desenvolvedor da Web (como os `My.Request` `My.Response` objetos e) ao mesmo tempo em que suprimim objetos que não são relevantes (como o `My.Forms` objeto). A imagem a seguir mostra o `My` modelo de objeto em um projeto de site da Web:  
  
 ![Diagrama que mostra o modelo de objeto My em um aplicativo Web.](./media/how-my-depends-on-project-type/my-object-model-web.png)  
  
## <a name="project-details"></a>Detalhes do projeto  

 A tabela a seguir mostra quais `My` objetos são habilitados por padrão para oito tipos de projeto: aplicativo do Windows, biblioteca de classes, aplicativo de console, biblioteca de controle do Windows, biblioteca de controle da Web, serviço do Windows, vazio e site.  
  
 Há três versões do `My.Application` objeto, duas versões do `My.Computer` objeto e duas versões do `My.User` objeto; os detalhes sobre essas versões são fornecidos nas notas de rodapé após a tabela.  
  
|Meu objeto|Aplicativo do Windows|Biblioteca de Classes|Aplicativo do Console|Biblioteca de controle do Windows|Biblioteca de controle da Web|Windows Service|Vazio|Site|  
|---|---|---|---|---|---|---|---|---|  
|`My.Application`|**Sim** <sup>1</sup>|**Sim** <sup>2</sup>|**Sim** <sup>3</sup>|**Sim** <sup>2</sup>|Não|**Sim** <sup>3</sup>|Não|Não|  
|`My.Computer`|**Sim** <sup>4</sup>|**Sim** <sup>4</sup>|**Sim** <sup>4</sup>|**Sim** <sup>4</sup>|**Sim** <sup>5</sup>|**Sim** <sup>4</sup>|Não|**Sim** <sup>5</sup>|  
|`My.Forms`|**Sim**|Não|Não|**Sim**|Não|Não|Não|Não|  
|`My.Log`|Não|Não|Não|Não|Não|Não|Não|**Sim**|  
|`My.Request`|Não|Não|Não|Não|Não|Não|Não|**Sim**|  
|`My.Resources`|**Sim**|**Sim**|**Sim**|**Sim**|**Sim**|**Sim**|Não|Não|  
|`My.Response`|Não|Não|Não|Não|Não|Não|Não|**Sim**|  
|`My.Settings`|**Sim**|**Sim**|**Sim**|**Sim**|**Sim**|**Sim**|Não|Não|  
|`My.User`|**Sim** <sup>6</sup>|**Sim** <sup>6</sup>|**Sim** <sup>6</sup>|**Sim** <sup>6</sup>|**Sim** <sup>7</sup>|**Sim** <sup>6</sup>|Não|**Sim** <sup>7</sup>|  
|`My.WebServices`|**Sim**|**Sim**|**Sim**|**Sim**|**Sim**|**Sim**|Não|Não|  
  
 <sup>1</sup> Windows Forms versão do `My.Application` . Deriva da versão do console (consulte a observação 3); Adiciona suporte para interação com as janelas do aplicativo e fornece o modelo de aplicativo Visual Basic.  
  
 <sup>2</sup> versão da biblioteca do `My.Application` . Fornece a funcionalidade básica necessária para um aplicativo: fornece membros para gravar no log do aplicativo e acessar informações do aplicativo.  
  
 <sup>3</sup> versão do console do `My.Application` . Deriva da versão da biblioteca (consulte a observação 2) e adiciona membros adicionais para acessar os argumentos de linha de comando do aplicativo e as informações de implantação do ClickOnce.  
  
 <sup>4</sup> versão do Windows do `My.Computer` . Deriva da versão do servidor (consulte a observação 5) e fornece acesso a objetos úteis em um computador cliente, como o teclado, a tela e o mouse.  
  
 <sup>5</sup> versão do servidor do `My.Computer` . Fornece informações básicas sobre o computador, como o nome, o acesso ao relógio e assim por diante.  
  
 <sup>6</sup> versão do Windows do `My.User` . Esse objeto é associado à identidade atual do thread.  
  
 <sup>7</sup> versão da Web do `My.User` . Esse objeto é associado à identidade do usuário da solicitação HTTP atual do aplicativo.  
  
## <a name="see-also"></a>Confira também

- <xref:Microsoft.VisualBasic.ApplicationServices.ApplicationBase>
- <xref:Microsoft.VisualBasic.Devices.Computer>
- <xref:Microsoft.VisualBasic.Logging.Log>
- <xref:Microsoft.VisualBasic.ApplicationServices.User>
- [Como personalizar quais objetos estão disponíveis em My](../customizing-extending-my/customizing-which-objects-are-available-in-my.md)
- [Compilação condicional](../../programming-guide/program-structure/conditional-compilation.md)
- [-definir (Visual Basic)](../../reference/command-line-compiler/define.md)
- [Objeto My.Forms](../../language-reference/objects/my-forms-object.md)
- [Objeto My.Request](../../language-reference/objects/my-request-object.md)
- [Objeto My.Response](../../language-reference/objects/my-response-object.md)
- [Objeto My.WebServices](../../language-reference/objects/my-webservices-object.md)
