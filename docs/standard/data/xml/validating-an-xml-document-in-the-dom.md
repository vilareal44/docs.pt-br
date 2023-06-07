---
title: Validando um documento XML no DOM
ms.date: 03/30/2017
ms.technology: dotnet-standard
dev_langs:
- csharp
- vb
- cpp
ms.assetid: 2c61c920-d0f8-4c72-bfcc-6524570f3060
ms.openlocfilehash: 949dc52c332b17784b0e1851d178465fe4881b6f
ms.sourcegitcommit: 33deec3e814238fb18a49b2a7e89278e27888291
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/02/2020
ms.locfileid: "84287630"
---
# <a name="validating-an-xml-document-in-the-dom"></a>Validando um documento XML no DOM

A classe <xref:System.Xml.XmlDocument> não valida o XML no DOM (Modelo de Objeto do Documento) em um esquema XSD (linguagem de definição de esquema XML) ou uma DTD (definição de tipo de documento) por padrão; o XML só é verificado quanto a se está bem formado.

Para validar o XML no DOM, você pode validar XML quando ele é carregado no DOM passando um método <xref:System.Xml.XmlReader> de validação de esquema para o método <xref:System.Xml.XmlDocument.Load%2A> da classe <xref:System.Xml.XmlDocument> ou validar um documento XML não validado anteriormente no DOM usando o método <xref:System.Xml.XmlDocument.Validate%2A> da classe <xref:System.Xml.XmlDocument>.

## <a name="validating-an-xml-document-as-it-is-loaded-into-the-dom"></a>Validando um documento XML à medida que ele é carregado no DOM

A classe <xref:System.Xml.XmlDocument> valida os dados XML à medida que eles são carregados no DOM quando um <xref:System.Xml.XmlReader> de validação é passado para o método <xref:System.Xml.XmlDocument.Load%2A> da classe <xref:System.Xml.XmlDocument>.

Após uma validação bem-sucedida, os padrões do esquema são aplicados, os valores de texto são convertidos em valores atômicos conforme o necessário e as informações de tipo estão associadas aos itens de informação validados. Como resultado, os dados XML tipados substituem os dados XML sem tipo anteriores.

### <a name="creating-an-xml-schema-validating-xmlreader"></a>Criando um XmlReader de validação de esquema XML

Para criar um <xref:System.Xml.XmlReader> de validação de esquema XML, siga estas etapas.

1. Construa uma nova instância de <xref:System.Xml.XmlReaderSettings>.

2. Adicione um esquema XML à propriedade <xref:System.Xml.XmlReaderSettings.Schemas%2A> da instância <xref:System.Xml.XmlReaderSettings>.

3. Especificar `Schema` como o <xref:System.Xml.XmlReaderSettings.ValidationType%2A>.

4. Especificar opcionalmente <xref:System.Xml.XmlReaderSettings.ValidationFlags%2A> e <xref:System.Xml.XmlReaderSettings.ValidationEventHandler> para manipular erros e avisos de validação de esquema encontrados durante a validação.

5. Finalmente, passe o objeto <xref:System.Xml.XmlReaderSettings> para o método <xref:System.Xml.XmlReader.Create%2A> da classe <xref:System.Xml.XmlReader> junto com o documento XML, criando um <xref:System.Xml.XmlReader> de validação de esquema.

### <a name="example"></a>Exemplo

No exemplo de código a seguir, o <xref:System.Xml.XmlReader> de esquema de validação valida os dados XML carregados no DOM. As modificações inválidas são feitas para o documento XML e o documento é então revalidados, causando erros de validação de esquema. Finalmente, um dos erros é corrigido e a parte do documento XML é validada parcialmente.

[!code-cpp[XmlDocumentValidation.Load#1](../../../../samples/snippets/cpp/VS_Snippets_Data/XmlDocumentValidation.Load/CPP/XmlDocumentValidationExample.cpp#1)]
[!code-csharp[XmlDocumentValidation.Load#1](../../../../samples/snippets/csharp/VS_Snippets_Data/XmlDocumentValidation.Load/CS/XmlDocumentValidationExample.cs#1)]
[!code-vb[XmlDocumentValidation.Load#1](../../../../samples/snippets/visualbasic/VS_Snippets_Data/XmlDocumentValidation.Load/VB/XmlDocumentValidationExample.vb#1)]

O exemplo usa o arquivo `contosoBooks.xml` como entrada.

[!code-xml[XPathXMLExamples#2](../../../../samples/snippets/xml/VS_Snippets_Data/XPathXMLExamples/XML/contosoBooks.xml#2)]

O exemplo também usa o arquivo `contosoBooks.xsd` como entrada.

[!code-xml[XPathXMLExamples#3](../../../../samples/snippets/xml/VS_Snippets_Data/XPathXMLExamples/XML/contosoBooks.xsd#3)]

Considere o seguinte ao validar dados XML à medida que são carregados no DOM.

- No exemplo anterior, o <xref:System.Xml.XmlReaderSettings.ValidationEventHandler> é chamado sempre que um tipo inválido for encontrado. Se um <xref:System.Xml.XmlReaderSettings.ValidationEventHandler> não estiver definido no <xref:System.Xml.XmlReader> de validação, um <xref:System.Xml.Schema.XmlSchemaValidationException> será gerado quando <xref:System.Xml.XmlDocument.Load%2A> for chamado se algum atributo ou tipo de elemento não corresponder ao tipo correspondente especificado no esquema de validação.

- Quando um documento XML for carregado em um objeto <xref:System.Xml.XmlDocument> com um esquema associado que define os valores padrão, o <xref:System.Xml.XmlDocument> trata esses padrões como se eles tivessem aparecido no documento XML. Isso significa que a propriedade <xref:System.Xml.XmlReader.IsEmptyElement%2A> sempre retorna `false` para um elemento que foi usado como padrão do esquema. Mesmo se estiver no documento XML, ele terá sido escrito como um elemento vazio.

## <a name="validating-an-xml-document-in-the-dom"></a>Validando um documento XML no DOM

O método <xref:System.Xml.XmlDocument.Validate%2A> da classe <xref:System.Xml.XmlDocument> valida os dados XML carregados no DOM com os esquemas na propriedade <xref:System.Xml.XmlDocument> do objeto <xref:System.Xml.XmlDocument.Schemas%2A>. Após uma validação bem-sucedida, os padrões do esquema são aplicados, os valores de texto são convertidos em valores atômicos conforme o necessário e as informações de tipo estão associadas aos itens de informação validados. Como resultado, os dados XML tipados substituem os dados XML sem tipo anteriores.

O exemplo a seguir é semelhante ao exemplo em “Validando um documento XML à medida que ele é carregado no DOM” acima. Nesse exemplo, o documento XML não é validado à medida que é carregado no DOM, mas é validado depois que foi carregado no DOM usando o método <xref:System.Xml.XmlDocument.Validate%2A> da classe <xref:System.Xml.XmlDocument>. O método <xref:System.Xml.XmlDocument.Validate%2A> valida o documento XML com os esquemas XML contidos na propriedade <xref:System.Xml.XmlDocument.Schemas%2A> do <xref:System.Xml.XmlDocument>. As modificações inválidas são então feitas para o documento XML e o documento é então revalidados, causando erros de validação de esquema. Finalmente, um dos erros é corrigido e a parte do documento XML é validada parcialmente.

[!code-csharp[XmlDocumentValidation.Validate#1](../../../../samples/snippets/csharp/VS_Snippets_Data/XmlDocumentValidation.Validate/CS/XmlDocumentValidationExample.cs#1)]
[!code-vb[XmlDocumentValidation.Validate#1](../../../../samples/snippets/visualbasic/VS_Snippets_Data/XmlDocumentValidation.Validate/VB/XmlDocumentValidationExample.vb#1)]

O exemplo utiliza como entrada os arquivos `contosoBooks.xml` e `contosoBooks.xsd` mencionados em "Validando um documento XML à medida que ele é carregado no DOM" acima.

## <a name="handling-validation-errors-and-warnings"></a>Tratando erros de validação e avisos

Os erros de validação de esquema XML são relatados ao validar os dados XML carregados no DOM. Você é notificado sobre todos os erros de validação de esquema localizados enquanto valida os dados XML que estão sendo carregados ou ao validar um documento XML não validado anteriormente.

Os erros de validação são tratados pelo <xref:System.Xml.Schema.ValidationEventHandler>. Se um <xref:System.Xml.Schema.ValidationEventHandler> tiver sido atribuído à instância <xref:System.Xml.XmlReaderSettings> ou passado para o método <xref:System.Xml.XmlDocument.Validate%2A> da classe <xref:System.Xml.XmlDocument>, o <xref:System.Xml.Schema.ValidationEventHandler> tratará os erros de validação de esquema; caso contrário, um <xref:System.Xml.Schema.XmlSchemaValidationException> será gerado quando um erro de validação de esquema for localizado.

> [!NOTE]
> Os dados XML são carregados no DOM apesar dos erros de validação do esquema a menos que seu <xref:System.Xml.Schema.ValidationEventHandler> gere uma exceção para interromper o processo.
>
> Os avisos do esquema de validação não são reportados a menos que o sinalizador <xref:System.Xml.Schema.XmlSchemaValidationFlags.ReportValidationWarnings> tenha sido especificado para o objeto <xref:System.Xml.XmlReaderSettings>.

 Para ver exemplos que ilustram o <xref:System.Xml.Schema.ValidationEventHandler>, consulte “Validando um documento XML à medida que ele é carregado no DOM” e “Validando um documento XML no DOM” acima.

## <a name="see-also"></a>Veja também

- <xref:System.Xml.XmlDocument>
- <xref:System.Xml.XmlReader>
- <xref:System.Xml.Schema.ValidationEventHandler>
- <xref:System.Xml.XmlReaderSettings>
- [Processar dados XML usando o modelo DOM](process-xml-data-using-the-dom-model.md)
- [Trabalhando com esquemas XML](working-with-xml-schemas.md)
