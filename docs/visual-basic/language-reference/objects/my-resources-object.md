---
title: Objeto My.Resources
ms.date: 07/20/2015
f1_keywords:
- My.Resources
- My.Resources.MyResources.ResourceManager
- My.Resources.MyResources.Culture
helpviewer_keywords:
- My.Resources object
ms.assetid: 34c3f2dc-7b87-432c-9d5f-17ea666bb266
ms.openlocfilehash: 2b7c82c31d2e31ccbf573cf1dfa138af2d99ce29
ms.sourcegitcommit: f8c270376ed905f6a8896ce0fe25b4f4b38ff498
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/04/2020
ms.locfileid: "84372447"
---
# <a name="myresources-object"></a>Objeto My.Resources
Fornece propriedades e classes para acessar os recursos do aplicativo.  
  
## <a name="remarks"></a>Comentários  
 O `My.Resources` objeto fornece acesso aos recursos do aplicativo e permite que você recupere dinamicamente os recursos para seu aplicativo. Para obter mais informações, consulte [Gerenciando recursos de aplicativos (.net)](/visualstudio/ide/managing-application-resources-dotnet).  
  
 O `My.Resources` objeto expõe apenas recursos globais. Ele não fornece acesso aos arquivos de recursos associados aos formulários. Você deve acessar os recursos do formulário no formulário.  
  
 Você pode acessar os arquivos de recursos específicos da cultura do aplicativo a partir do `My.Resources` objeto. Por padrão, o `My.Resources` objeto pesquisa os recursos do arquivo de recursos que corresponde à cultura na <xref:Microsoft.VisualBasic.ApplicationServices.ApplicationBase.UICulture%2A> propriedade. No entanto, você pode substituir esse comportamento e especificar uma cultura específica a ser usada para os recursos. Para saber mais, veja [Recursos em aplicativos da área de trabalho](../../../framework/resources/index.md).  
  
## <a name="properties"></a>Propriedades  
 As propriedades do `My.Resources` objeto fornecem acesso somente leitura aos recursos do aplicativo. Para adicionar ou remover recursos, use o **Designer de projeto**. Você pode acessar os recursos adicionados por meio do **Designer de projeto** usando `My.Resources.` *resourceName*.  
  
 Você também pode adicionar ou remover arquivos de recursos selecionando seu projeto no **Gerenciador de soluções** e clicando em **Adicionar novo item** ou **Adicionar item existente** no menu **projeto** . Você pode acessar recursos adicionados dessa maneira usando `My.Resources.` *resourceFileName* `.` *resourceName*.  
  
 Cada recurso tem um nome, categoria e valor, e essas configurações de recurso determinam como a propriedade para acessar o recurso aparece no `My.Resources` objeto. Para recursos adicionados no **Designer de projeto**:  
  
- O nome determina o nome da propriedade,  
  
- Os dados de recurso são o valor da propriedade,  
  
- A categoria determina o tipo da propriedade:  
  
|Categoria|Tipo de dados de propriedade|  
|---|---|  
|**Cadeias de caracteres**|[Cadeia de caracteres](../data-types/string-data-type.md)|  
|**Imagens**|<xref:System.Drawing.Bitmap>|  
|**Ícones**|<xref:System.Drawing.Icon>|  
|**Sonoro**|<xref:System.IO.UnmanagedMemoryStream><br /><br /> A <xref:System.IO.UnmanagedMemoryStream> classe deriva da <xref:System.IO.Stream> classe, portanto, ela pode ser usada com métodos que usam fluxos, como o <xref:Microsoft.VisualBasic.Devices.Audio.Play%2A> método.|  
|**Arquivos**|-   [Cadeia de caracteres](../data-types/string-data-type.md) para arquivos de texto.<br />-   <xref:System.Drawing.Bitmap>para arquivos de imagem.<br />-   <xref:System.Drawing.Icon>para arquivos de ícone.<br />-   <xref:System.IO.UnmanagedMemoryStream>para arquivos de som.|  
|**Outros**|Determinado pelas informações na coluna **Type** do designer.|  
  
## <a name="classes"></a>Classes  
 O `My.Resources` objeto expõe cada arquivo de recurso como uma classe com propriedades compartilhadas. O nome da classe é o mesmo que o nome do arquivo de recurso. Conforme descrito na seção anterior, os recursos em um arquivo de recurso são expostos como propriedades na classe.  
  
## <a name="example"></a>Exemplo  
 Este exemplo define o título de um formulário para o recurso de cadeia de caracteres chamado `Form1Title` no arquivo de recurso do aplicativo. Para que o exemplo funcione, o aplicativo deve ter uma cadeia de caracteres chamada `Form1Title` em seu arquivo de recurso.  
  
 [!code-vb[VbVbalrMyResources#1](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrMyResources/VB/Form1.vb#1)]  
  
## <a name="example"></a>Exemplo  
 Este exemplo define o ícone do formulário para o ícone chamado `Form1Icon` que é armazenado no arquivo de recurso do aplicativo. Para que o exemplo funcione, o aplicativo deve ter um ícone chamado `Form1Icon` em seu arquivo de recurso.  
  
 [!code-vb[VbVbalrMyResources#2](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrMyResources/VB/Form1.vb#2)]  
  
## <a name="example"></a>Exemplo  
 Este exemplo define a imagem de plano de fundo de um formulário para o recurso de imagem chamado `Form1Background` , que está no arquivo de recurso de aplicativo. Para que este exemplo funcione, o aplicativo deve ter um recurso de imagem chamado `Form1Background` em seu arquivo de recurso.  
  
 [!code-vb[VbVbalrMyResources#3](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrMyResources/VB/Form1.vb#3)]  
  
## <a name="example"></a>Exemplo  
 Este exemplo reproduz o som que é armazenado como um recurso de áudio chamado `Form1Greeting` no arquivo de recurso do aplicativo. Para que o exemplo funcione, o aplicativo deve ter um recurso de áudio chamado `Form1Greeting` em seu arquivo de recurso. O `My.Computer.Audio.Play` método está disponível somente para aplicativos Windows Forms.  
  
 [!code-vb[VbVbalrMyResources#4](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrMyResources/VB/Form1.vb#4)]  
  
## <a name="example"></a>Exemplo  
 Este exemplo recupera a versão de cultura francesa de um recurso de cadeia de caracteres do aplicativo. O recurso é nomeado `Message` . Para alterar a cultura usada pelo `My.Resources` objeto, o exemplo usa <xref:Microsoft.VisualBasic.ApplicationServices.ApplicationBase.ChangeUICulture%2A> .  
  
 Para que este exemplo funcione, o aplicativo deve ter uma cadeia de caracteres chamada `Message` em seu arquivo de recurso e o aplicativo deve ter a versão de cultura francesa desse arquivo de recurso, Resources.fr-fr. resx. Se o aplicativo não tiver a versão de cultura francesa do arquivo de recurso, o `My.Resource` objeto recuperará o recurso do arquivo de recurso de cultura padrão.  
  
 [!code-vb[VbVbalrMyResources#10](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrMyResources/VB/Form1.vb#10)]  
  
## <a name="see-also"></a>Confira também

- [Gerenciando recursos de aplicativo (.NET)](/visualstudio/ide/managing-application-resources-dotnet)
- [Recursos em aplicativos da área de trabalho](../../../framework/resources/index.md)
