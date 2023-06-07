---
title: Como fornecer ajuda em um aplicativo do Windows
ms.date: 03/30/2017
helpviewer_keywords:
- Help [Windows Forms], Windows applications
- HTML Help [Windows Forms], Windows Forms
- Windows applications [Windows Forms], providing Help
- HelpProvider component [Windows Forms]
- forms [Windows Forms], providing Help
ms.assetid: 7c4e5cec-2bd2-4f0b-8d75-c2b88929bd61
ms.openlocfilehash: 405de333ce936a864047e827e60f56a930059e26
ms.sourcegitcommit: ee5b798427f81237a3c23d1fd81fff7fdc21e8d3
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/28/2020
ms.locfileid: "84143570"
---
# <a name="how-to-provide-help-in-a-windows-application"></a>Como fornecer ajuda em um aplicativo do Windows

Você pode fazer uso do <xref:System.Windows.Forms.HelpProvider> componente para anexar tópicos de ajuda dentro de um arquivo de ajuda a controles específicos em Windows Forms. O arquivo de Ajuda pode ser HTML ou HTMLHelp 1.x ou formato maior.

## <a name="provide-help"></a>Fornecer ajuda

1. No Visual Studio, na **caixa de ferramentas**, arraste um <xref:System.Windows.Forms.HelpProvider> componente para o formulário.

     O componente ficará na bandeja na parte inferior do Designer de Formulários do Windows.

2. Na janela **Propriedades** , defina a <xref:System.Windows.Forms.HelpProvider.HelpNamespace%2A> propriedade para o arquivo de ajuda. chm,. Col ou. htm.

3. Selecione outro controle que você tem em seu formulário e, na janela **Propriedades** , defina a <xref:System.Windows.Forms.HelpProvider.SetHelpKeyword%2A> propriedade.

     Essa é a cadeia de caracteres passada pelo <xref:System.Windows.Forms.HelpProvider> componente para o arquivo de ajuda para Convoque o tópico apropriado da ajuda.

4. Na janela **Propriedades** , defina a <xref:System.Windows.Forms.HelpProvider.SetHelpNavigator%2A> propriedade como um valor da <xref:System.Windows.Forms.HelpNavigator> enumeração.

     Isso determina o modo como a propriedade **HelpKeyword** é passada para o sistema de Ajuda. A tabela a seguir mostra as possíveis configurações e suas descrições.

    |Nome do membro|Descrição|
    |-----------------|-----------------|
    |AssociateIndex|Especifica que o índice para um tópico especificado é realizado na URL especificada.|
    |Localizar|Especifica que a página de pesquisa de uma URL especificada é exibida.|
    |Índice|Especifica que o índice de uma URL especificada é exibido.|
    |KeywordIndex|Especifica uma palavra-chave para pesquisar e a ação a ser tomada na URL especificada.|
    |TableOfContents|Especifica que o sumário do arquivo de Ajuda HTML 1.0 é exibido.|
    |Tópico|Especifica que o tópico referenciado por uma URL especificada é exibido.|

 Em tempo de execução, pressionar F1 quando o controle — para o qual você definiu as propriedades **HelpKeyword** e **HelpNavigator** — tem foco abrirá o arquivo de ajuda associado a esse <xref:System.Windows.Forms.HelpProvider> componente.

 Atualmente, a propriedade **HelpNamespace** dá suporte a arquivos de Ajuda nos três formatos a seguir: HTMLHelp 1.x, HTMLHelp 2.0 e HTML. Assim, você pode definir a propriedade **HelpNamespace** como um `http://` endereço, como uma página da Web. Se isso for feito, abrirá o navegador padrão para a página da Web com a cadeia de caracteres especificada na propriedade **HelpKeyword** usada como âncora. A âncora é usada para ir para uma parte específica de uma página HTML.

> [!IMPORTANT]
> Tenha cuidado para verificar as informações que são enviadas de um cliente antes de usá-las em seu aplicativo. Usuários mal-intencionados podem tentar enviar ou injetar script executável, instruções SQL ou outro código. Antes de exibir a entrada do usuário, armazená-la em um banco de dados ou trabalhar com ela, verifique se ela não contém informações potencialmente não seguras. Uma maneira comum de verificar é usar uma expressão regular para procurar palavras-chave como "SCRIPT" ao receber entrada de um usuário.

Você também pode usar o <xref:System.Windows.Forms.HelpProvider> componente para mostrar a ajuda pop-up, mesmo que você tenha configurado para exibir os arquivos de ajuda para os controles em seu Windows Forms. Para obter mais informações, consulte [Como exibir Ajuda em pop-up](how-to-display-pop-up-help.md).

## <a name="see-also"></a>Consulte também

- [Como Exibir Ajuda Pop-up](how-to-display-pop-up-help.md)
- [Ajuda de Controle Usando ToolTips](control-help-using-tooltips.md)
- [Integrando a Ajuda do Usuário nos Windows Forms](integrating-user-help-in-windows-forms.md)
- [Windows Forms](../index.md)
