---
title: Como desserializar um objeto usando XmlSerializer
description: Saiba como desserializar um objeto. O formato de transporte determina se um objeto de fluxo ou de arquivo deve ser criado.
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
helpviewer_keywords:
- deserializing objects
- objects, deserializing steps
ms.assetid: 287129c8-035a-4fea-b7b3-4790057ca076
ms.openlocfilehash: e08ae0d77539219223650fd3bcbd1bcee4df2739
ms.sourcegitcommit: b16c00371ea06398859ecd157defc81301c9070f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/06/2020
ms.locfileid: "83379110"
---
# <a name="how-to-deserialize-an-object-using-xmlserializer"></a>Como desserializar um objeto usando XmlSerializer

Quando você desserializar um objeto, o formato do transporte determina se você criará um fluxo ou objeto de arquivo. Após o formato do transporte ser determinado, você poderá chamar os métodos <xref:System.Xml.Serialization.XmlSerializer.Serialize%2A> ou <xref:System.Xml.Serialization.XmlSerializer.Deserialize%2A>, conforme o necessário.

## <a name="to-deserialize-an-object"></a>Para desserializar um objeto

1. Construa um <xref:System.Xml.Serialization.XmlSerializer> usando o tipo do objeto para desserializar.

1. Chame o método <xref:System.Xml.Serialization.XmlSerializer.Deserialize%2A> para gerar uma réplica do objeto. Ao desserializar, você deve converter o objeto retornado para o tipo do original, conforme mostrado no exemplo a seguir, que desserializa o objeto de um arquivo (embora ele também possa ser desserializado de um fluxo).

    ```vb
    ' Construct an instance of the XmlSerializer with the type
    ' of object that is being deserialized.
    Dim mySerializer As New XmlSerializer(GetType(MySerializableClass))
    ' To read the file, create a FileStream.
    Dim myFileStream As New FileStream("myFileName.xml", FileMode.Open)
    ' Call the Deserialize method and cast to the object type.
    Dim myObject = CType( _
    mySerializer.Deserialize(myFileStream), MySerializableClass)
    ```

    ```csharp
    // Construct an instance of the XmlSerializer with the type
    // of object that is being deserialized.
    var mySerializer = new XmlSerializer(typeof(MySerializableClass));
    // To read the file, create a FileStream.
    var myFileStream = new FileStream("myFileName.xml", FileMode.Open);
    // Call the Deserialize method and cast to the object type.
    var myObject = (MySerializableClass) mySerializer.Deserialize(myFileStream)
    ```

## <a name="see-also"></a>Confira também

- [Apresentando a serialização XML](introducing-xml-serialization.md)
- [Como serializar um objeto](how-to-serialize-an-object.md)
