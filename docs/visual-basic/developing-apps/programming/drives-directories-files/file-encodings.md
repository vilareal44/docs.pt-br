---
title: Codificações de arquivo
ms.date: 07/20/2015
helpviewer_keywords:
- character encodings
- files [Visual Basic], encoding
- Unicode, file encoding
- file encoding
ms.assetid: ea2c5f5f-bbb1-4150-9928-b9951fa6bc57
ms.openlocfilehash: f906b2f2d747a7950c70a24549bbf5423e5b87b4
ms.sourcegitcommit: f8c270376ed905f6a8896ce0fe25b4f4b38ff498
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/04/2020
ms.locfileid: "84401739"
---
# <a name="file-encodings-visual-basic"></a>Codificações de arquivo (Visual Basic)

As codificações de arquivo, também conhecidas como codificações de caracteres, especificam como representar caracteres no processamento de texto. Uma codificação pode ser preferível em relação a outra em termos de quais caracteres de idioma podem ou não ser manipulados, embora o Unicode geralmente seja o ideal.

Durante a leitura ou gravação em arquivos, combinar incorretamente as codificações de arquivo pode resultar em exceções ou resultados incorretos.

## <a name="types-of-encodings"></a>Tipos de Codificações

O Unicode é a codificação preferencial ao trabalhar com arquivos. O Unicode é um padrão de codificação de caracteres global que usa valores de código de 16 bits para representar todos os caracteres utilizados na computação moderna, incluindo símbolos técnicos e caracteres especiais usados em publicações.

Os padrões de codificação de caracteres anteriores consistiam em conjuntos de caracteres tradicionais, como o conjunto de caracteres ANSI do Windows que usa valores de código de 8 bits ou combinações de valores de 8 bits, a fim de representar os caracteres usados em um idioma ou região geográfica específicos.

## <a name="encoding-class"></a>Classe de Codificação

A classe <xref:System.Text.Encoding> representa uma codificação de caracteres. Esta tabela lista os tipos de codificações disponíveis e descreve cada uma.

|Nome|Descrição|
|---|---|
|<xref:System.Text.ASCIIEncoding>|Representa uma codificação de caracteres ASCII de caracteres Unicode.|
|<xref:System.Text.UnicodeEncoding>|Representa uma codificação de caracteres Unicode UTF-16.|
|<xref:System.Text.UTF32Encoding>|Representa uma codificação de caracteres Unicode UTF-32.|
|<xref:System.Text.UTF7Encoding>|Representa uma codificação de caracteres Unicode UTF-7.|
|<xref:System.Text.UTF8Encoding>|Representa uma codificação de caracteres Unicode UTF-8. |

## <a name="see-also"></a>Confira também

- [Ler arquivos](reading-from-files.md)
- [Gravar em arquivos](writing-to-files.md)
