---
title: Documentar o código com XML
ms.date: 07/20/2015
helpviewer_keywords:
- XML [Visual Basic], documenting code
- XML comments, Visual Basic
- Visual Basic code, documenting with XML
ms.assetid: a0d35dc7-c5f9-4d74-92ff-a1c6f28d5235
ms.openlocfilehash: f391fb909cfe4de8f27afb24d6db389e2c8cdfae
ms.sourcegitcommit: cdb295dd1db589ce5169ac9ff096f01fd0c2da9d
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/09/2020
ms.locfileid: "84590923"
---
# <a name="document-your-code-with-xml-visual-basic"></a>Documente seu código com XML (Visual Basic)

No Visual Basic, você pode documentar seu código usando XML.

## <a name="xml-documentation-comments"></a>Comentários da documentação XML

Visual Basic fornece uma maneira fácil de criar automaticamente a documentação XML para projetos. Você pode gerar automaticamente um esqueleto XML para seus tipos e membros e, em seguida, fornecer resumos, documentação descritiva para cada parâmetro e outros comentários. Com a configuração apropriada, a documentação XML é emitida automaticamente em um arquivo XML com o mesmo nome de arquivo raiz do seu projeto. Para obter mais informações, consulte [-Doc](../../reference/command-line-compiler/doc.md).

O arquivo XML pode ser consumido ou manipulado de outra forma como XML. Esse arquivo está localizado no mesmo diretório que o arquivo. exe ou. dll de saída do seu projeto.

A documentação XML começa com `'''` . O processamento desses comentários tem algumas restrições:

- A documentação deve ser em XML bem formado. Se o XML não estiver bem formado, um aviso será gerado e o arquivo de documentação conterá um comentário dizendo que foi encontrado um erro.

- Os desenvolvedores são livres para criar seu próprio conjunto de marcas. Há um conjunto recomendado de marcas (consulte [marcas de comentário XML](../../language-reference/xmldoc/index.md)). Algumas das marcas recomendadas têm significado especial:

  - A \<param> marca é usada para descrever os parâmetros. Se ela for usada, o compilador verificará se o parâmetro existe e se todos os parâmetros são descritos na documentação. Se a verificação falhar, o compilador emitirá um aviso.

  - O atributo `cref` pode ser anexado a qualquer marca para fornecer uma referência a um elemento de código. O compilador verifica se esse elemento de código existe. Se a verificação falhar, o compilador emitirá um aviso. O compilador também respeita quaisquer `Imports` instruções ao procurar um tipo descrito no `cref` atributo.

  - A \<summary> marca é usada pelo IntelliSense no Visual Studio para exibir informações adicionais sobre um tipo ou membro.

## <a name="related-sections"></a>Seções relacionadas

Para obter detalhes sobre como criar um arquivo XML com comentários de documentação, consulte os seguintes tópicos:

- [-doc](../../reference/command-line-compiler/doc.md)

- [Marcações de Comentário XML](../../language-reference/xmldoc/index.md)

- [Processando o arquivo XML](processing-the-xml-file.md)

- [Como criar documentação XML](how-to-create-xml-documentation.md)

- [Ferramentas XML no Visual Studio](/visualstudio/xml-tools/xml-tools-in-visual-studio)

## <a name="see-also"></a>Confira também

- [Desenvolvendo aplicativos com o Visual Basic](../../developing-apps/index.md)
- [Guia de programação do Visual Basic](../index.md)
