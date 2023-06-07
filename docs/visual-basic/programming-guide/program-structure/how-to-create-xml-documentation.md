---
title: Como criar documentação XML
ms.date: 07/20/2015
helpviewer_keywords:
- XML comments
- XML documentation [Visual Basic], creating
ms.assetid: 27b5b06c-09b9-496a-8245-f9542d846230
ms.openlocfilehash: 1421cc85beba42b3cf3656c34b1d02347fbaf164
ms.sourcegitcommit: f8c270376ed905f6a8896ce0fe25b4f4b38ff498
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/04/2020
ms.locfileid: "84403233"
---
# <a name="how-to-create-xml-documentation-in-visual-basic"></a>Como criar documentação XML no Visual Basic

Este exemplo mostra como adicionar comentários de documentação XML ao seu código.

[!INCLUDE[note_settings_general](~/includes/note-settings-general-md.md)]

## <a name="to-create-xml-documentation-for-a-type-or-member"></a>Para criar a documentação XML para um tipo ou membro

1. No **Editor de código**, posicione o cursor na linha acima do tipo ou do membro para o qual você deseja criar a documentação.

2. Tipo `'''` (três aspas simples).

    Um esqueleto XML para o tipo ou membro é adicionado no **Editor de código**.

3. Adicione informações descritivas entre as marcas apropriadas.

    > [!NOTE]
    > Se você adicionar linhas adicionais no bloco de documentação XML, cada linha deverá começar com `'''` .

4. Adicione um código adicional que use o tipo ou o membro com os novos comentários de documentação XML.

    O IntelliSense exibe o texto da \<summary> marca para o tipo ou o membro.

5. Compile o código para gerar um arquivo XML contendo os comentários da documentação. Para obter mais informações, consulte [-Doc](../../reference/command-line-compiler/doc.md).

## <a name="see-also"></a>Confira também

- [Documentando o código com XML](documenting-your-code-with-xml.md)
- [Marcações de Comentário XML](../../language-reference/xmldoc/index.md)
- [-doc](../../reference/command-line-compiler/doc.md)
