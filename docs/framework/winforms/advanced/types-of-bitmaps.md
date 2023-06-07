---
title: Tipos de bitmaps
description: Saiba mais sobre os tipos de bitmap e os formatos de arquivo de gráficos GDI+ com suporte, incluindo BMP, JPG, GIF, PNG e TIFF.
ms.date: 03/30/2017
helpviewer_keywords:
- jpeg files
- TIFF files
- gif files
- Graphics Interchange Format
- Joint Photographic Experts Group
- .jpeg files
- .gif files
- graphics [Windows Forms], file formats
- images [Windows Forms], file formats
- Portable Network Graphics
- .bmp files
- EXIF file format
- PNG files
- pictures [Windows Forms], file formats
- Tag Image File Format
- bitmaps [Windows Forms], file format
- Exchangeable Image File
ms.assetid: 6be085a2-2c13-47c8-b80a-c18b32777d8d
ms.openlocfilehash: 09b74ef476467b0bba5aac1f58db278b3898ef17
ms.sourcegitcommit: cb27c01a8b0b4630148374638aff4e2221f90b22
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/09/2020
ms.locfileid: "86174673"
---
# <a name="types-of-bitmaps"></a>Tipos de bitmaps
Um bitmap é uma matriz de bits que especifica a cor de cada pixel em uma matriz retangular de pixels. O número de bits dedicados a um pixel individual determina o número de cores que podem ser atribuídos a esse pixel. Por exemplo, se cada pixel é representado por 4 bits, em seguida, um determinado pixel pode ser atribuído um dos 16 cores diferentes (2^4 = 16). A tabela a seguir mostra alguns exemplos do número de cores que podem ser atribuídos a um pixel representado por um determinado número de bits.  
  
|Bits por Pixel|Número de cores que podem ser atribuídos a um pixel|  
|--------------------|------------------------------------------------------|  
|1|2^1 = 2|  
|2|2^2 = 4|  
|4|2^4 = 16|  
|8|2^8 = 256|  
|16|2^16 = 65.536|  
|24|2^24 = 16,777,216|  
  
 Arquivos de disco que armazenam os bitmaps geralmente contêm um ou mais blocos de informações que armazenam informações como o número de bits por pixel, o número de pixels em cada linha e o número de linhas na matriz. Esse arquivo também pode conter uma tabela de cores (às vezes chamada de uma paleta de cores). Uma tabela de cores mapeia os números no bitmap para cores específicas. A ilustração a seguir mostra uma imagem ampliada juntamente com sua tabela de cores e bitmap. Cada pixel é representado por um número de 4 bits, portanto, existem 2^4 = 16 cores na tabela de cores. Cada cor na tabela é representada por um número de 24 bits: 8 bits para vermelho, 8 bits para verde e 8 bits para azul. Os números são exibidos em formato hexadecimal (base 16): um = 10, B = 11, C = 12, D = 13, E = 14, F = 15.  
  
 ![Exemplo de bitmap](./media/aboutgdip03-art01.gif "AboutGdip03_Art01")  
  
 Examinar o pixel na linha 3, coluna 5 da imagem. O número correspondente no bitmap é 1. A tabela de cores nos informa que 1 representa a cor vermelha, portanto o pixel é vermelho. Todas as entradas na linha superior do bitmap são 3. A tabela de cores nos informa que 3 representa azul, portanto, todos os pixels na linha superior da imagem são azuis.  
  
> [!NOTE]
> Alguns bitmaps são armazenados no formato de baixo para cima; os números na primeira linha do bitmap correspondem aos pixels na linha inferior da imagem.  
  
 Um bitmap que armazena os índices em uma tabela de cores é chamado um bitmap indexado por paleta. Alguns bitmaps não tiveram necessidade de uma tabela de cores. Por exemplo, se um bitmap usa 24 bits por pixel, esse bitmap pode armazenar as cores ao invés de indexar em uma tabela de cores. A ilustração a seguir mostra um bitmap que armazena as cores diretamente (24 bits por pixel) em vez de usar uma tabela de cores. A ilustração também mostra uma exibição ampliada da imagem correspondente. No bitmap, FFFFFF representa branco, FF0000 representa vermelho, 00FF00 representa verde e 0000FF representa azul.  
  
 ![Exemplo de bitmap](./media/aboutgdip03-art02.gif "AboutGdip03_Art02")  
  
## <a name="graphics-file-formats"></a>Formatos de arquivos gráficos  
 Há muitos formatos padrão para salvar os bitmaps nos arquivos do disco. O GDI+ dá suporte aos formatos de arquivo gráficos descritos nos parágrafos a seguir.  
  
### <a name="bmp"></a>BMP  
 BMP é um formato padrão usado pelo Windows para armazenar imagens independente de dispositivo e aplicativo. O número de bits por pixel (1, 4, 8, 15, 24, 32 ou 64) para um determinado arquivo BMP é especificado no cabeçalho do arquivo. Arquivos BMP com 24 bits por pixel são comuns. Arquivos BMP geralmente não são compactados e, portanto, não são adequados para a transferência pela Internet.  
  
### <a name="graphics-interchange-format-gif"></a>GIF (Graphics Interchange Format)  
 GIF é um formato comum para imagens que aparecem nas páginas da Web. GIFs funcionam bem para desenhos de linha, figuras com blocos de cor sólida e imagens com limites nítidas entre as cores. GIFs são compactados, mas nenhuma informação é perdida no processo de compactação; uma imagem descompactada é exatamente a mesma que a original. Uma cor em uma imagem GIF pode ser designada como transparente, para que tenha a cor da tela de fundo de qualquer página da Web que exibe a imagem. Uma sequência de imagens GIF pode ser armazenada em um único arquivo para formar um GIF animado. GIFs armazenam no máximo 8 bits por pixel, portanto, estão limitados a 256 cores.  
  
### <a name="joint-photographic-experts-group-jpeg"></a>Joint Photographic Experts Group (JPEG)  
 JPEG é um esquema de compactação que funciona bem para cenários naturais, como fotografias digitalizadas. Algumas informações são perdidas no processo de compactação, mas geralmente a perda é imperceptível ao olho humano. JPEGs armazenam 24 bits por pixel, para que sejam capazes de exibir mais de 16 milhões de cores. JPEGs não dão suporte a transparências ou animações.  
  
 O nível de compactação de imagens JPEG é configurável, mas níveis mais altos de compactação (arquivos menores) resultam em mais perda de informações. Uma taxa de compactação de 20:1 geralmente produz uma imagem que o olho humano tem dificuldade de distinguir do original. A ilustração a seguir mostra uma imagem BMP e duas imagens JPEG compactadas dessa imagem BMP. O primeiro JPEG tem uma taxa de compactação de 4:1 e o segundo JPEG tem uma taxa de compactação de 8:1.  
  
 ![Amostras de filetype](./media/aboutgdip03-art03.gif "AboutGdip03_Art03")  
  
 Compactação de JPEG não funcionam bem para desenhos de linha, blocos de cor sólida e limites nítidos. A ilustração a seguir mostra um BMP junto com dois JPEGs e GIF. Os JPEGs e GIF foram compactados do BMP. A taxa de compactação é 4:1 para o GIF, 4:1 para o JPEG menor e 8:3 para o JPEG maior. Observe que o GIF mantém os limites nítidos ao longo das linhas, mas os JPEGs tendem a desfocar os limites.  
  
 ![Arquivo](./media/aboutgdip03-art03a.gif "AboutGdip03_Art03A")  
  
 JPEG é um esquema de compactação, não é um formato de arquivo. JPEG File Interchange Format (JFIF) é um formato de arquivo normalmente usado para armazenar e transferir imagens que foram compactadas de acordo com o esquema JPEG. Arquivos JFIF exibidos por navegadores da Web usam a extensão .jpg.  
  
### <a name="exchangeable-image-file-exif"></a>Exchangeable Image File (EXIF)  
 EXIF é um formato de arquivo usado para fotografias capturadas por câmeras digitais. Um arquivo EXIF contém uma imagem que é compactada de acordo com a especificação de JPEG. Um arquivo EXIF também contém informações sobre a fotografia (tirada, obturador velocidade, tempo de exposição e assim por diante) e informações sobre a câmera (fabricante, modelo e assim por diante).  
  
### <a name="portable-network-graphics-png"></a>PNG (formato PNG)  
 O formato PNG mantém muitas das vantagens do formato GIF, mas também fornece recursos além dos do GIF. Como arquivos GIF, PNG são compactados sem perda de informações. Arquivos PNG podem armazenar cores com 8, 24, 48 bits por pixel e escalas de cinza com 1, 2, 4, 8 ou 16 bits por pixel. Por outro lado, arquivos GIF podem usar apenas 1, 2, 4 ou 8 bits por pixel. Um arquivo PNG também pode armazenar um valor alfa para cada pixel, que especifica o grau ao qual a cor desse pixel é combinada com a cor da tela de fundo.  
  
 PNG aprimora o GIF em sua capacidade de exibir progressivamente uma imagem (ou seja exibir melhores e melhor aproximações da imagem como ela chega em uma conexão de rede). Arquivos PNG podem conter informações de correção de cor e de gama para que as imagens podem ser processadas com precisão em uma variedade de dispositivos de vídeo.  
  
### <a name="tag-image-file-format-tiff"></a>Formato Tag Image File (TIFF)  
 TIFF é um formato flexível e extensível que tem suporte em uma ampla variedade de plataformas e aplicativos de processamento de imagem. Arquivos TIFF podem armazenar imagens com um número arbitrário de bits por pixel e podem empregar uma variedade de algoritmos de compactação. Várias imagens podem ser armazenadas em um único arquivo TIFF de várias páginas. Informações relacionadas à imagem (marca de scanner, computador host, tipo de compactação, orientação, amostras por pixel e assim por diante) podem ser armazenadas no arquivo e organizadas com o uso de marcas. O formato TIFF pode ser estendido conforme necessário para a aprovação e a adição de novas marcas.  
  
## <a name="see-also"></a>Veja também

- <xref:System.Drawing.Image?displayProperty=nameWithType>
- <xref:System.Drawing.Bitmap?displayProperty=nameWithType>
- <xref:System.Drawing.Imaging.PixelFormat?displayProperty=nameWithType>
- [Imagens, bitmaps e metaarquivos](images-bitmaps-and-metafiles.md)
- [Trabalhando com Imagens, Bitmaps, Ícones e Metarquivos](working-with-images-bitmaps-icons-and-metafiles.md)
