---
title: Objeto My.Forms
ms.date: 07/20/2015
f1_keywords:
- My.Forms
- My.MyProject.Forms
helpviewer_keywords:
- My.Forms object
ms.assetid: f6bff4e6-6769-4294-956b-037aa6106d2a
ms.openlocfilehash: 001f6fbfae2467ea0af5e98ca041b694d1e7b8f9
ms.sourcegitcommit: f8c270376ed905f6a8896ce0fe25b4f4b38ff498
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/04/2020
ms.locfileid: "84372434"
---
# <a name="myforms-object"></a>Objeto My.Forms

Fornece propriedades para acessar uma instância de cada formulário do Windows declarado no projeto atual.

## <a name="remarks"></a>Comentários

O `My.Forms` objeto fornece uma instância de cada formulário no projeto atual. O nome da propriedade é o mesmo que o nome do formulário que a propriedade acessa.

Você pode acessar os formulários fornecidos pelo `My.Forms` objeto usando o nome do formulário, sem qualificação. Como o nome da propriedade é igual ao nome do tipo do formulário, isso permite que você acesse um formulário como se ele tivesse uma instância padrão. Por exemplo, `My.Forms.Form1.Show` equivale a `Form1.Show`.

O `My.Forms` objeto expõe apenas os formulários associados ao projeto atual. Ele não fornece acesso a formulários declarados em DLLs referenciadas. Para acessar um formulário que uma DLL fornece, você deve usar o nome qualificado do formulário, escrito como *DllName*. *FormName*.

Você pode usar a <xref:Microsoft.VisualBasic.ApplicationServices.WindowsFormsApplicationBase.OpenForms%2A> propriedade para obter uma coleção de todos os formulários abertos do aplicativo.

O objeto e suas propriedades estão disponíveis apenas para aplicativos do Windows.

## <a name="properties"></a>Propriedades

Cada propriedade do `My.Forms` objeto fornece acesso a uma instância de um formulário no projeto atual. O nome da propriedade é o mesmo que o nome do formulário que a propriedade acessa, e o tipo de propriedade é o mesmo que o tipo do formulário.

> [!NOTE]
> Se houver uma colisão de nomes, o nome da propriedade para acessar um formulário será *RootNamespace*_*namespace* \_ *FormName*. Por exemplo, considere dois formulários chamados `Form1.` se um desses formulários estiver no namespace raiz `WindowsApplication1` e no namespace `Namespace1` , você acessaria esse formulário por meio do `My.Forms.WindowsApplication1_Namespace1_Form1` .

O `My.Forms` objeto fornece acesso à instância do formulário principal do aplicativo que foi criado na inicialização. Para todos os outros formulários, o `My.Forms` objeto cria uma nova instância do formulário quando ele é acessado e armazena-o. As tentativas subsequentes de acessar essa propriedade retornam essa instância do formulário.

Você pode descartar um formulário atribuindo `Nothing` à propriedade para esse formulário. O setter da propriedade chama o <xref:System.Windows.Forms.Form.Close%2A> método do formulário e, em seguida, atribui `Nothing` ao valor armazenado. Se você atribuir qualquer valor diferente de `Nothing` à propriedade, o setter lançará uma <xref:System.ArgumentException> exceção.

Você pode testar se uma propriedade do `My.Forms` objeto armazena uma instância do formulário usando o `Is` `IsNot` operador OR. Você pode usar esses operadores para verificar se o valor da propriedade é `Nothing` .

> [!NOTE]
> Normalmente, o `Is` operador OR precisa `IsNot` ler o valor da propriedade para executar a comparação. No entanto, se a propriedade atualmente armazena `Nothing` , a propriedade cria uma nova instância do formulário e, em seguida, retorna essa instância. No entanto, o compilador de Visual Basic trata as propriedades do `My.Forms` objeto de forma diferente e permite que o `Is` `IsNot` operador OR Verifique o status da propriedade sem alterar seu valor.

## <a name="example"></a>Exemplo

Este exemplo altera o título do formulário padrão `SidebarMenu` .

[!code-vb[VbVbalrMyForms#2](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrMyForms/VB/Class1.vb#2)]

Para que este exemplo funcione, seu projeto deve ter um formulário chamado `SidebarMenu` .

Esse código só funcionará em um projeto de aplicativo do Windows.

## <a name="requirements"></a>Requisitos

### <a name="availability-by-project-type"></a>Disponibilidade por tipo de projeto

|Tipo de projeto|Disponível|
|---|---|
|Aplicativo do Windows|**Sim**|
|Biblioteca de Classes|Não|
|Aplicativo do Console|Não|
|Biblioteca de controle do Windows|Não|
|Biblioteca de controle da Web|Não|
|Windows Service|Não|
|Site|Não|

## <a name="see-also"></a>Confira também

- <xref:Microsoft.VisualBasic.ApplicationServices.WindowsFormsApplicationBase.OpenForms%2A>
- <xref:System.Windows.Forms.Form>
- <xref:System.Windows.Forms.Form.Close%2A>
- [Objetos](index.md)
- [Operador Is](../operators/is-operator.md)
- [Operador IsNot](../operators/isnot-operator.md)
- [Como acessar formulários de aplicativo](../../developing-apps/programming/accessing-application-forms.md)
