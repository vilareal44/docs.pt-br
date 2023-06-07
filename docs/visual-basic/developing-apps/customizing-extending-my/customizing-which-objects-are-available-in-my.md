---
title: Como personalizar quais objetos estão disponíveis em My
ms.date: 07/20/2015
helpviewer_keywords:
- My namespace [Visual Basic], customizing
- My namespace
ms.assetid: 4e8279c2-ed5b-4681-8903-8a6671874000
ms.openlocfilehash: 5245c129281bc8c7c1c6fe9215a221889380a901
ms.sourcegitcommit: f8c270376ed905f6a8896ce0fe25b4f4b38ff498
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/04/2020
ms.locfileid: "84410212"
---
# <a name="customizing-which-objects-are-available-in-my-visual-basic"></a>Personalizando quais objetos estão disponíveis em My (Visual Basic)

Este tópico descreve como você pode controlar quais `My` objetos são habilitados definindo a constante de `_MYTYPE` compilação condicional do projeto. O IDE (ambiente de desenvolvimento integrado) do Visual Studio mantém a `_MYTYPE` constante de compilação condicional de um projeto em sincronia com o tipo do projeto.  
  
## <a name="predefined-_mytype-values"></a>Valores de com MyType predefinidos \_  

Você deve usar a `/define` opção do compilador para definir a `_MYTYPE` constante de compilação condicional. Ao especificar seu próprio valor para a `_MYTYPE` constante, você deve colocar o valor da cadeia de caracteres nas sequências de barra invertida/aspas ( \\ "). Por exemplo, você pode usar:  
  
```console  
/define:_MYTYPE=\"WindowsForms\"  
```  
  
 Esta tabela mostra o que a `_MYTYPE` constante de compilação condicional está definida para vários tipos de projeto.  
  
|Tipo de projeto|\_Valor de com MyType|  
|------------------|--------------------|  
|Biblioteca de Classes|“Windows”|  
|Aplicativo do Console|MMC|  
|Web|Site|  
|Biblioteca de controle da Web|WebControl|  
|Aplicativo do Windows|WindowsForms|  
|Aplicativo do Windows, ao iniciar com personalizado`Sub Main`|"WindowsFormsWithCustomSubMain"|  
|Biblioteca de controle do Windows|“Windows”|  
|Windows Service|MMC|  
|Vazio|Esvaziá|  
  
> [!NOTE]
> Todas as comparações de cadeia de caracteres de compilação condicional diferenciam maiúsculas de minúsculas, independentemente de como a `Option Compare` instrução é definida.  
  
## <a name="dependent-_my-compilation-constants"></a>\_Minhas constantes de compilação dependentes  

A `_MYTYPE` constante de compilação condicional, por sua vez, controla os valores de várias outras `_MY` constantes de compilação:  
  
|\_COM MyType|\_MYAPPLICATIONTYPE|\_Mycomputertype|\_MYFORMS|\_Myusertype|\_Meus WEBSERVICEs|  
|--------------|-------------------------|----------------------|---------------|------------------|---------------------|  
|MMC|MMC|“Windows”|Indefinido|“Windows”|VERDADEIRO|  
|Personalizar|Indefinido|Indefinido|Indefinido|Indefinido|Indefinido|  
|Esvaziá|Indefinido|Indefinido|Indefinido|Indefinido|Indefinido|  
|Site|Indefinido|Site|FALSO|Site|FALSO|  
|WebControl|Indefinido|Site|FALSO|Site|VERDADEIRO|  
|"Windows" ou ""|“Windows”|“Windows”|Indefinido|“Windows”|VERDADEIRO|  
|WindowsForms|WindowsForms|“Windows”|VERDADEIRO|“Windows”|VERDADEIRO|  
|"WindowsFormsWithCustomSubMain"|MMC|“Windows”|VERDADEIRO|“Windows”|VERDADEIRO|  
  
 Por padrão, constantes de compilação condicional indefinidas são resolvidas para `FALSE` . Você pode especificar valores para as constantes indefinidas ao compilar seu projeto para substituir o comportamento padrão.  
  
> [!NOTE]
> Quando `_MYTYPE` é definido como "Custom", o projeto contém o `My` namespace, mas não contém nenhum objeto. No entanto, `_MYTYPE` a definição de "Empty" impede que o compilador adicione o `My` namespace e seus objetos.  
  
 Esta tabela descreve os efeitos dos valores predefinidos das `_MY` constantes de compilação.  
  
|Constante|Significado|  
|--------------|-------------|  
|`_MYAPPLICATIONTYPE`|Habilita `My.Application` , se a constante for "console", Windows, "ou" WindowsForms ":<br /><br /> -A versão do "console" deriva de <xref:Microsoft.VisualBasic.ApplicationServices.ConsoleApplicationBase> . e tem menos membros do que a versão "Windows".<br />-A versão "Windows" deriva de <xref:Microsoft.VisualBasic.ApplicationServices.ApplicationBase> . e tem menos membros do que a versão "WindowsForms".<br />-A versão "WindowsForms" de `My.Application` é derivada de <xref:Microsoft.VisualBasic.ApplicationServices.WindowsFormsApplicationBase> . Se a `TARGET` constante for definida como "winexe", a classe incluirá um `Sub Main` método.|  
|`_MYCOMPUTERTYPE`|Habilita `My.Computer` , se a constante for "Web" ou "Windows":<br /><br /> -A versão "Web" deriva de <xref:Microsoft.VisualBasic.Devices.ServerComputer> e tem menos membros do que a versão "Windows".<br />-A versão do "Windows" de `My.Computer` é derivada de <xref:Microsoft.VisualBasic.Devices.Computer> .|  
|`_MYFORMS`|Habilita `My.Forms` , se a constante for `TRUE` .|  
|`_MYUSERTYPE`|Habilita `My.User` , se a constante for "Web" ou "Windows":<br /><br /> -A versão "Web" do `My.User` está associada à identidade do usuário da solicitação HTTP atual.<br />-A versão "Windows" do `My.User` está associada à entidade de segurança atual do thread.|  
|`_MYWEBSERVICES`|Habilita `My.WebServices` , se a constante for `TRUE` .|  
|`_MYTYPE`|Habilita `My.Log` , `My.Request` e `My.Response` , se a constante for "Web".|  
  
## <a name="see-also"></a>Confira também

- <xref:Microsoft.VisualBasic.ApplicationServices.ApplicationBase>
- <xref:Microsoft.VisualBasic.Devices.Computer>
- <xref:Microsoft.VisualBasic.Logging.Log>
- <xref:Microsoft.VisualBasic.ApplicationServices.User>
- [Como My depende do tipo de projeto](../development-with-my/how-my-depends-on-project-type.md)
- [Compilação condicional](../../programming-guide/program-structure/conditional-compilation.md)
- [-definir (Visual Basic)](../../reference/command-line-compiler/define.md)
- [Objeto My.Forms](../../language-reference/objects/my-forms-object.md)
- [Objeto My.Request](../../language-reference/objects/my-request-object.md)
- [Objeto My.Response](../../language-reference/objects/my-response-object.md)
- [Objeto My.WebServices](../../language-reference/objects/my-webservices-object.md)
