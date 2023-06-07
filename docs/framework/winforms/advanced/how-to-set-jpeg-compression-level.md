---
title: Como definir o nível de compactação de JPEG
description: Saiba como ajustar a qualidade de uma imagem JPEG modificando seu nível de compactação em Windows Forms.
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
helpviewer_keywords:
- images [Windows Forms], changing encoder parameters
- JPEG images [Windows Forms], setting quality level
ms.assetid: 4b9a74e3-9504-43c1-9f28-ace651d0772e
ms.openlocfilehash: 1f6a96e8a05fff40eb08da0ce318faa86a06cc3a
ms.sourcegitcommit: e02d17b2cf9c1258dadda4810a5e6072a0089aee
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/01/2020
ms.locfileid: "85618708"
---
# <a name="how-to-set-jpeg-compression-level"></a>Como definir o nível de compactação de JPEG
É possível modificar os parâmetros de uma imagem ao salvar a imagem em disco para minimizar o tamanho do arquivo ou melhorar sua qualidade. Você pode ajustar a qualidade de uma imagem JPEG modificando seu nível de compactação. Para especificar o nível de compactação ao salvar uma imagem JPEG, você deve criar um <xref:System.Drawing.Imaging.EncoderParameters> objeto e passá-lo para o <xref:System.Drawing.Image.Save%2A> método da <xref:System.Drawing.Image> classe. Inicialize o <xref:System.Drawing.Imaging.EncoderParameters> objeto para que ele tenha uma matriz que consiste em um <xref:System.Drawing.Imaging.EncoderParameter> . Ao criar o <xref:System.Drawing.Imaging.EncoderParameter> , especifique o <xref:System.Drawing.Imaging.Encoder.Quality> codificador e o nível de compactação desejado.  
  
## <a name="example"></a>Exemplo  
 O código de exemplo a seguir cria um <xref:System.Drawing.Imaging.EncoderParameter> objeto e salva três imagens JPEG. Cada imagem JPEG é salva com um nível de qualidade diferente, modificando o `long` valor passado para o <xref:System.Drawing.Imaging.EncoderParameter> Construtor. Um nível de qualidade de 0 corresponde a uma compactação maior e um nível de qualidade de 100 corresponde a uma compactação menor.  
  
```csharp  
private void VaryQualityLevel()  
    {  
        // Get a bitmap. The using statement ensures objects  
        // are automatically disposed from memory after use.  
        using (Bitmap bmp1 = new Bitmap(@"C:\TestPhoto.jpg"))  
        {  
            ImageCodecInfo jpgEncoder = GetEncoder(ImageFormat.Jpeg);  
  
            // Create an Encoder object based on the GUID  
            // for the Quality parameter category.  
            System.Drawing.Imaging.Encoder myEncoder =  
                System.Drawing.Imaging.Encoder.Quality;  
  
            // Create an EncoderParameters object.  
            // An EncoderParameters object has an array of EncoderParameter  
            // objects. In this case, there is only one  
            // EncoderParameter object in the array.  
            EncoderParameters myEncoderParameters = new EncoderParameters(1);  
  
            EncoderParameter myEncoderParameter = new EncoderParameter(myEncoder, 50L);  
            myEncoderParameters.Param[0] = myEncoderParameter;  
            bmp1.Save(@"c:\TestPhotoQualityFifty.jpg", jpgEncoder, myEncoderParameters);  
  
            myEncoderParameter = new EncoderParameter(myEncoder, 100L);  
            myEncoderParameters.Param[0] = myEncoderParameter;  
            bmp1.Save(@"C:\TestPhotoQualityHundred.jpg", jpgEncoder, myEncoderParameters);  
  
            // Save the bitmap as a JPG file with zero quality level compression.  
            myEncoderParameter = new EncoderParameter(myEncoder, 0L);  
            myEncoderParameters.Param[0] = myEncoderParameter;  
            bmp1.Save(@"C:\TestPhotoQualityZero.jpg", jpgEncoder, myEncoderParameters);  
        }  
    }  
```  
  
```vb  
Private Sub VaryQualityLevel()  
    ' Get a bitmap. The Using statement ensures objects  
    ' are automatically disposed from memory after use.  
    Using bmp1 As New Bitmap("C:\test\TestPhoto.jpg")  
        Dim jpgEncoder As ImageCodecInfo = GetEncoder(ImageFormat.Jpeg)  
  
        ' Create an Encoder object based on the GUID  
        ' for the Quality parameter category.  
        Dim myEncoder As System.Drawing.Imaging.Encoder = System.Drawing.Imaging.Encoder.Quality  
  
        ' Create an EncoderParameters object.  
        ' An EncoderParameters object has an array of EncoderParameter  
        ' objects. In this case, there is only one  
        ' EncoderParameter object in the array.  
        Dim myEncoderParameters As New EncoderParameters(1)  
  
        Dim myEncoderParameter As New EncoderParameter(myEncoder, 50L)  
        myEncoderParameters.Param(0) = myEncoderParameter  
        bmp1.Save("c:\test\TestPhotoQualityFifty.jpg", jpgEncoder, myEncoderParameters)  
  
        myEncoderParameter = New EncoderParameter(myEncoder, 100L)  
        myEncoderParameters.Param(0) = myEncoderParameter  
        bmp1.Save("C:\test\TestPhotoQualityHundred.jpg", jpgEncoder, myEncoderParameters)  
  
        ' Save the bitmap as a JPG file with zero quality level compression.  
        myEncoderParameter = New EncoderParameter(myEncoder, 0L)  
        myEncoderParameters.Param(0) = myEncoderParameter  
        bmp1.Save("C:\test\TestPhotoQualityZero.jpg", jpgEncoder, myEncoderParameters)  
    End Using  
End Sub  
```  
  
```csharp  
private ImageCodecInfo GetEncoder(ImageFormat format)  
{  
    ImageCodecInfo[] codecs = ImageCodecInfo.GetImageDecoders();  
    foreach (ImageCodecInfo codec in codecs)  
    {  
        if (codec.FormatID == format.Guid)  
        {  
            return codec;  
        }  
    }  
    return null;  
}  
```  
  
```vb  
Private Function GetEncoder(ByVal format As ImageFormat) As ImageCodecInfo  
  
    Dim codecs As ImageCodecInfo() = ImageCodecInfo.GetImageDecoders()  
    Dim codec As ImageCodecInfo  
    For Each codec In codecs  
        If codec.FormatID = format.Guid Then  
            Return codec  
        End If  
    Next codec  
    Return Nothing  
  
End Function  
```  
  
## <a name="compiling-the-code"></a>Compilando o código  
 Este exemplo requer:  
  
- Um aplicativo Windows Forms.  
  
- A <xref:System.Windows.Forms.PaintEventArgs> , que é um parâmetro de <xref:System.Windows.Forms.PaintEventHandler> .  
  
- Um arquivo de imagem chamado `TestPhoto.jpg` e localizado em **c:\\**.  
  
## <a name="see-also"></a>Consulte também

- [Como determinar os parâmetros compatíveis com um codificador](how-to-determine-the-parameters-supported-by-an-encoder.md)
- [Tipos de Bitmaps](types-of-bitmaps.md)
- [Usando Codificadores e Decodificadores de Imagem no GDI+ Gerenciado](using-image-encoders-and-decoders-in-managed-gdi.md)
