---
title: XmlSchemaSet para compilação de esquema
ms.date: 03/30/2017
ms.technology: dotnet-standard
dev_langs:
- csharp
- vb
ms.assetid: 55c4b175-3170-4071-9d60-dd5a42f79b54
ms.openlocfilehash: 44850755c41b212e88e0b90dd3b016f96a0af96d
ms.sourcegitcommit: 33deec3e814238fb18a49b2a7e89278e27888291
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/02/2020
ms.locfileid: "84290221"
---
# <a name="xmlschemaset-for-schema-compilation"></a>XmlSchemaSet para compilação de esquema
Descreve <xref:System.Xml.Schema.XmlSchemaSet>, um cache onde os esquemas de linguagem de definição de esquema XML (XSD) podem ser armazenados e validado.  
  
## <a name="the-xmlschemaset-class"></a>A classe de XmlSchemaSet  
 <xref:System.Xml.Schema.XmlSchemaSet> é um cache onde os esquemas de linguagem de definição de esquema XML (XSD) podem ser armazenados e validado.  
  
 Na versão 1,0 de <xref:System.Xml?displayProperty=nameWithType> , os esquemas XML foram carregados em uma classe de <xref:System.Xml.Schema.XmlSchemaCollection> como uma biblioteca de esquemas. Na versão 2,0 de <xref:System.Xml?displayProperty=nameWithType> , <xref:System.Xml.XmlValidatingReader> e as classes de <xref:System.Xml.Schema.XmlSchemaCollection> são obsoletos, e foram substituídas por métodos de <xref:System.Xml.XmlReader.Create%2A> , e a classe de <xref:System.Xml.Schema.XmlSchemaSet> respectivamente.  
  
 <xref:System.Xml.Schema.XmlSchemaSet> foi introduzido para corrigir um número de problemas que incluem a compatibilidade de padrão, o desempenho, e o formato reduzido do esquema do Microsoft com dados (XDR) obsoletos.  
  
 A seguir está uma comparação entre a classe de <xref:System.Xml.Schema.XmlSchemaCollection> e a classe de <xref:System.Xml.Schema.XmlSchemaSet> .  
  
|XmlSchemaCollection|XmlSchemaSet|  
|-------------------------|------------------|  
|Esquemas XML de XDR e o W3C da Microsoft de suporte.|Somente esquemas XML W3C de suporte.|  
|Os esquemas são compilados quando o método de <xref:System.Xml.Schema.XmlSchemaCollection.Add%2A> é chamado.|Os esquemas não são compilados quando o método de <xref:System.Xml.Schema.XmlSchemaSet.Add%2A> é chamado. Isso fornece uma melhoria de desempenho durante a criação de biblioteca de esquema.|  
|Cada esquema gerencia uma versão compilada individual que pode resultar de “para ilhas esquema.” Como resultado, tudo inclui e importações são do escopo apenas dentro desse esquema.|Os esquemas compilados gerenciar um único esquema lógico, “set” de esquemas. Todos os esquemas importados em um esquema que são adicionados ao dataset são adicionados diretamente ao conjunto próprios. Isso significa que todos os tipos estão disponíveis para todos os esquemas.|  
|Somente um esquema para um namespace de destino específico pode existir na coleção.|Vários esquemas para a mesma namespace de destino podem ser adicionados como não há nenhum conflito de tipo.|  
  
## <a name="migrating-to-the-xmlschemaset"></a>Migrar para o XmlSchemaSet  
 O exemplo de código fornece um guia para migrar para a nova classe de <xref:System.Xml.Schema.XmlSchemaSet> da classe obsoleta de <xref:System.Xml.Schema.XmlSchemaCollection> . O exemplo de código a seguir ilustra as seguintes diferenças entre as duas classes.  
  
- Ao contrário do método de <xref:System.Xml.Schema.XmlSchemaCollection.Add%2A> da classe de <xref:System.Xml.Schema.XmlSchemaCollection> , os esquemas não são compilados ao chamar o método de <xref:System.Xml.Schema.XmlSchemaSet.Add%2A> de <xref:System.Xml.Schema.XmlSchemaSet>. O método de <xref:System.Xml.Schema.XmlSchemaSet.Compile%2A> de <xref:System.Xml.Schema.XmlSchemaSet> é chamado explicitamente no código de exemplo.  
  
- Para iterar sobre <xref:System.Xml.Schema.XmlSchemaSet>, você deve usar a propriedade de <xref:System.Xml.Schema.XmlSchemaSet.Schemas%2A> de <xref:System.Xml.Schema.XmlSchemaSet>.  
  
 A seguir está o exemplo de código obsoleto de <xref:System.Xml.Schema.XmlSchemaCollection> .  
  
```vb  
Dim schemaCollection As XmlSchemaCollection = New XmlSchemaCollection()  
schemaCollection.Add("http://www.contoso.com/retail", "http://www.contoso.com/retail.xsd")  
schemaCollection.Add("http://www.contoso.com/books", "http://www.contoso.com/books.xsd")  
  
Dim schema As XmlSchema  
  
For Each schema in schemaCollection  
  
   Console.WriteLine(schema.TargetNamespace)  
  
Next  
```  
  
```csharp  
XmlSchemaCollection schemaCollection = new XmlSchemaCollection();  
schemaCollection.Add("http://www.contoso.com/retail", "http://www.contoso.com/retail.xsd");  
schemaCollection.Add("http://www.contoso.com/books", "http://www.contoso.com/books.xsd");  
  
foreach(XmlSchema schema in schemaCollection)  
{  
   Console.WriteLine(schema.TargetNamespace);  
}  
```  
  
 O exemplo a seguir é o equivalente de código de <xref:System.Xml.Schema.XmlSchemaSet> .  
  
```vb  
Dim schemaSet As XmlSchemaSet = New XmlSchemaSet()  
schemaSet.Add("http://www.contoso.com/retail", "http://www.contoso.com/retail.xsd")  
schemaSet.Add("http://www.contoso.com/books", "http://www.contoso.com/books.xsd")  
schemaSet.Compile()  
  
Dim schema As XmlSchema  
  
For Each schema in schemaSet.Schemas()  
  
   Console.WriteLine(schema.TargetNamespace)  
  
Next  
```  
  
```csharp  
XmlSchemaSet schemaSet = new XmlSchemaSet();  
schemaSet.Add("http://www.contoso.com/retail", "http://www.contoso.com/retail.xsd");  
schemaSet.Add("http://www.contoso.com/books", "http://www.contoso.com/books.xsd");  
schemaSet.Compile();  
  
foreach(XmlSchema schema in schemaSet.Schemas())  
{  
   Console.WriteLine(schema.TargetNamespace);  
}  
```  
  
## <a name="adding-and-retrieving-schemas"></a>Adicionando e recuperando esquemas  
 Os esquemas são adicionados a <xref:System.Xml.Schema.XmlSchemaSet> usando o método <xref:System.Xml.Schema.XmlSchemaSet.Add%2A> de <xref:System.Xml.Schema.XmlSchemaSet>. Quando um esquema é adicionado a <xref:System.Xml.Schema.XmlSchemaSet>, está associado com o URI de um namespace de destino. O URI de namespace de destino ou pode ser especificado como um parâmetro para o método de <xref:System.Xml.Schema.XmlSchemaSet.Add%2A> ou se nenhuma namespace de destino for especificada, <xref:System.Xml.Schema.XmlSchemaSet> usa o namespace de destino definida no esquema.  
  
 Os esquemas são recuperados de <xref:System.Xml.Schema.XmlSchemaSet> usando a propriedade de <xref:System.Xml.Schema.XmlSchemaSet.Schemas%2A> de <xref:System.Xml.Schema.XmlSchemaSet>. A propriedade de <xref:System.Xml.Schema.XmlSchemaSet.Schemas%2A> de <xref:System.Xml.Schema.XmlSchemaSet> permite que você faz iterações sobre os objetos de <xref:System.Xml.Schema.XmlSchema> contidos em <xref:System.Xml.Schema.XmlSchemaSet>. A propriedade de <xref:System.Xml.Schema.XmlSchemaSet.Schemas%2A> retorna todos os objetos de <xref:System.Xml.Schema.XmlSchema> contidos em <xref:System.Xml.Schema.XmlSchemaSet>, ou, dado um parâmetro de namespace de destino, retorna todos os objetos de <xref:System.Xml.Schema.XmlSchema> que pertencem ao namespace de destino. Se `null` é especificado como o parâmetro de namespace de destino, a propriedade de <xref:System.Xml.Schema.XmlSchemaSet.Schemas%2A> retorna todos os esquemas sem um namespace.  
  
 O exemplo a seguir adiciona o esquema de `books.xsd` no espaço de `http://www.contoso.com/books` a <xref:System.Xml.Schema.XmlSchemaSet>, recupera todos os esquemas que pertencem ao namespace de `http://www.contoso.com/books` de <xref:System.Xml.Schema.XmlSchemaSet>, e grava os esquemas a <xref:System.Console>.  
  
```vb  
Dim schemaSet As XmlSchemaSet = New XmlSchemaSet  
schemaSet.Add("http://www.contoso.com/books", "books.xsd")  
  
Dim schema As XmlSchema  
  
For Each schema In schemaSet.Schemas("http://www.contoso.com/books")  
  
   schema.Write(Console.Out)  
  
Next  
```  
  
```csharp  
XmlSchemaSet schemaSet = new XmlSchemaSet();  
schemaSet.Add("http://www.contoso.com/books", "books.xsd");  
  
foreach (XmlSchema schema in schemaSet.Schemas("http://www.contoso.com/books"))  
{  
   schema.Write(Console.Out);  
}  
```  
  
 Para obter mais informações sobre como adicionar e recuperar esquemas de <xref:System.Xml.Schema.XmlSchemaSet> objeto, consulte o método de <xref:System.Xml.Schema.XmlSchemaSet.Add%2A> e documentação de referência da propriedade de <xref:System.Xml.Schema.XmlSchemaSet.Schemas%2A> .  
  
## <a name="compiling-schemas"></a>Esquemas de compilação  
 Os esquemas em <xref:System.Xml.Schema.XmlSchemaSet> são compilados em um esquema lógico pelo método de <xref:System.Xml.Schema.XmlSchemaSet.Compile%2A> de <xref:System.Xml.Schema.XmlSchemaSet>.  
  
> [!NOTE]
> Ao contrário da classe obsoleta de <xref:System.Xml.Schema.XmlSchemaCollection> , os esquemas não são compilados quando o método de <xref:System.Xml.Schema.XmlSchemaSet.Add%2A> é chamado.  
  
 Se o método de <xref:System.Xml.Schema.XmlSchemaSet.Compile%2A> executa com êxito, a propriedade de <xref:System.Xml.Schema.XmlSchemaSet.IsCompiled%2A> de <xref:System.Xml.Schema.XmlSchemaSet> é definida como `true`.  
  
> [!NOTE]
> A propriedade de <xref:System.Xml.Schema.XmlSchemaSet.IsCompiled%2A> não é afetado se os esquemas são editados quando em <xref:System.Xml.Schema.XmlSchemaSet>. As atualizações de esquemas individuais em <xref:System.Xml.Schema.XmlSchemaSet> não são controladas. Como resultado, a propriedade de <xref:System.Xml.Schema.XmlSchemaSet.IsCompiled%2A> pode ser `true` mesmo que um dos esquemas contidos em <xref:System.Xml.Schema.XmlSchemaSet> é modificado, como nenhum esquema foi adicionado ou removido de <xref:System.Xml.Schema.XmlSchemaSet>.  
  
 O exemplo a seguir adiciona o arquivo de `books.xsd` a <xref:System.Xml.Schema.XmlSchemaSet> e chama o método de <xref:System.Xml.Schema.XmlSchemaSet.Compile%2A> .  
  
```vb  
Dim schemaSet As XmlSchemaSet = New XmlSchemaSet()  
schemaSet.Add("http://www.contoso.com/books", "books.xsd")  
schemaSet.Compile()  
```  
  
```csharp  
XmlSchemaSet schemaSet = new XmlSchemaSet();  
schemaSet.Add("http://www.contoso.com/books", "books.xsd");  
schemaSet.Compile();  
```  
  
 Para obter mais informações sobre esquemas de compilação em <xref:System.Xml.Schema.XmlSchemaSet>, consulte a documentação de referência de método <xref:System.Xml.Schema.XmlSchemaSet.Compile%2A> .  
  
## <a name="reprocessing-schemas"></a>Reprocessamento esquemas  
 Reprocessamento um esquema em <xref:System.Xml.Schema.XmlSchemaSet> executa todas as etapas de pré-processamento executadas em um esquema quando o método de <xref:System.Xml.Schema.XmlSchemaSet.Add%2A> de <xref:System.Xml.Schema.XmlSchemaSet> é chamado. Se a chamada ao método de <xref:System.Xml.Schema.XmlSchemaSet.Reprocess%2A> for bem-sucedida, a propriedade de <xref:System.Xml.Schema.XmlSchemaSet.IsCompiled%2A> de <xref:System.Xml.Schema.XmlSchemaSet> é definida como `false`.  
  
 O método de <xref:System.Xml.Schema.XmlSchemaSet.Reprocess%2A> deve ser usado quando um esquema em <xref:System.Xml.Schema.XmlSchemaSet> foi alterado depois que <xref:System.Xml.Schema.XmlSchemaSet> executar a compilação.  
  
 O exemplo a seguir ilustra o reprocessamento um esquema adicionado a <xref:System.Xml.Schema.XmlSchemaSet> usando o método <xref:System.Xml.Schema.XmlSchemaSet.Reprocess%2A> . Após <xref:System.Xml.Schema.XmlSchemaSet> é compilado usando o método de <xref:System.Xml.Schema.XmlSchemaSet.Compile%2A> , e o esquema adicionado a <xref:System.Xml.Schema.XmlSchemaSet> é alterado, a propriedade de <xref:System.Xml.Schema.XmlSchemaSet.IsCompiled%2A> é definida como `true` mesmo que um esquema em <xref:System.Xml.Schema.XmlSchemaSet> é alterado. Chamando o método <xref:System.Xml.Schema.XmlSchemaSet.Reprocess%2A> executa todo o pré-processamento executado pelo método de <xref:System.Xml.Schema.XmlSchemaSet.Add%2A> , e defina a propriedade de <xref:System.Xml.Schema.XmlSchemaSet.IsCompiled%2A> a `false`.  
  
```vb  
Dim schemaSet As XmlSchemaSet = New XmlSchemaSet()  
Dim schema As XmlSchema = schemaSet.Add("http://www.contoso.com/books", "http://www.contoso.com/books.xsd")  
schemaSet.Compile()  
  
Dim element As XmlSchemaElement = New XmlSchemaElement()  
schema.Items.Add(element)  
element.Name = "book"  
element.SchemaTypeName = New XmlQualifiedName("string", "http://www.w3.org/2001/XMLSchema")  
  
schemaSet.Reprocess(schema)  
```  
  
```csharp  
XmlSchemaSet schemaSet = new XmlSchemaSet();  
XmlSchema schema = schemaSet.Add("http://www.contoso.com/books", "http://www.contoso.com/books.xsd");  
schemaSet.Compile();  
  
XmlSchemaElement element = new XmlSchemaElement();  
schema.Items.Add(element);  
element.Name = "book";  
element.SchemaTypeName = new XmlQualifiedName("string", "http://www.w3.org/2001/XMLSchema");  
  
schemaSet.Reprocess(schema);  
```  
  
 Para obter mais informações sobre reprocessamento de um esquema em <xref:System.Xml.Schema.XmlSchemaSet>, consulte a documentação de referência de método <xref:System.Xml.Schema.XmlSchemaSet.Reprocess%2A> .  
  
## <a name="checking-for-a-schema"></a>Verificar se um esquema  
 Você pode usar o método de <xref:System.Xml.Schema.XmlSchemaSet.Contains%2A> de <xref:System.Xml.Schema.XmlSchemaSet> para verificar se um esquema está contido dentro de <xref:System.Xml.Schema.XmlSchemaSet>. O método de <xref:System.Xml.Schema.XmlSchemaSet.Contains%2A> leva um namespace de destino ou um objeto de <xref:System.Xml.Schema.XmlSchema> para verificar. Em ambos os casos, o método de <xref:System.Xml.Schema.XmlSchemaSet.Contains%2A> retorna `true` se o esquema está contido dentro de <xref:System.Xml.Schema.XmlSchemaSet>; caso contrário, retorna `false`.  
  
 Para obter mais informações sobre como verificar para um esquema, consulte a documentação de referência de método <xref:System.Xml.Schema.XmlSchemaSet.Contains%2A> .  
  
## <a name="removing-schemas"></a>Removendo os esquemas  
 Os esquemas são removidos de <xref:System.Xml.Schema.XmlSchemaSet> usando os métodos de <xref:System.Xml.Schema.XmlSchemaSet.Remove%2A> e de <xref:System.Xml.Schema.XmlSchemaSet.RemoveRecursive%2A> de <xref:System.Xml.Schema.XmlSchemaSet>. O método de <xref:System.Xml.Schema.XmlSchemaSet.Remove%2A> remove o esquema especificado de <xref:System.Xml.Schema.XmlSchemaSet>, quando o método de <xref:System.Xml.Schema.XmlSchemaSet.RemoveRecursive%2A> remover esquema especificado e todos os esquemas que importa de <xref:System.Xml.Schema.XmlSchemaSet>.  
  
 O exemplo a seguir ilustra adicionar vários esquemas a <xref:System.Xml.Schema.XmlSchemaSet>, então o uso do método de <xref:System.Xml.Schema.XmlSchemaSet.RemoveRecursive%2A> para remover um de esquemas e todos os esquemas que importa.  
  
```vb  
Dim schemaSet As XmlSchemaSet = New XmlSchemaSet()  
schemaSet.Add("http://www.contoso.com/retail", "http://www.contoso.com/retail.xsd")  
schemaSet.Add("http://www.contoso.com/books", "http://www.contoso.com/books.xsd")  
schemaSet.Add("http://www.contoso.com/music", "http://www.contoso.com/music.xsd")  
  
Dim schema As XmlSchema  
  
For Each schema In schemaSet.Schemas()  
  
   If schema.TargetNamespace = "http://www.contoso.com/music" Then  
      schemaSet.RemoveRecursive(schema)  
   End If  
  
Next  
```  
  
```csharp  
XmlSchemaSet schemaSet = new XmlSchemaSet();  
schemaSet.Add("http://www.contoso.com/retail", "http://www.contoso.com/retail.xsd");  
schemaSet.Add("http://www.contoso.com/books", "http://www.contoso.com/books.xsd");  
schemaSet.Add("http://www.contoso.com/music", "http://www.contoso.com/music.xsd");  
  
foreach (XmlSchema schema in schemaSet.Schemas())  
{  
   if (schema.TargetNamespace == "http://www.contoso.com/music")  
   {  
      schemaSet.RemoveRecursive(schema);  
   }  
}  
```  
  
 Para obter mais informações sobre a remoção de esquemas de <xref:System.Xml.Schema.XmlSchemaSet>, consulte a documentação de referência dos métodos de <xref:System.Xml.Schema.XmlSchemaSet.Remove%2A> e de <xref:System.Xml.Schema.XmlSchemaSet.RemoveRecursive%2A> .  
  
## <a name="schema-resolution-and-xsimport"></a>Resolução e xs:importde esquema  
 Os exemplos a seguir descrevem comportamento de <xref:System.Xml.Schema.XmlSchemaSet> para importar esquemas quando vários esquemas para uma determinada namespace existem em <xref:System.Xml.Schema.XmlSchemaSet>.  
  
 Por exemplo, considere <xref:System.Xml.Schema.XmlSchemaSet> que contém vários esquemas para o espaço de `http://www.contoso.com` . Um esquema com a seguinte diretiva de `xs:import` é adicionado a <xref:System.Xml.Schema.XmlSchemaSet>.  
  
```xml  
<xs:import namespace="http://www.contoso.com" schemaLocation="http://www.contoso.com/schema.xsd" />  
```  
  
 <xref:System.Xml.Schema.XmlSchemaSet> tenta importar um esquema para o espaço de `http://www.contoso.com` carregar a URL de `http://www.contoso.com/schema.xsd` . Somente a declaração e os tipos esquema declarados no documento de esquema estão disponíveis no esquema importando, mesmo que haja outros documentos de esquema para o espaço de `http://www.contoso.com` em <xref:System.Xml.Schema.XmlSchemaSet>. Se o arquivo de `schema.xsd` não pode ser localizado no URL de `http://www.contoso.com/schema.xsd` , nenhum esquema para o espaço de `http://www.contoso.com` é importado no esquema importando.  
  
## <a name="validating-xml-documents"></a>Validando documentos XML  
 Documentos XML podem ser validadas contra esquemas em <xref:System.Xml.Schema.XmlSchemaSet>. Você valide um documento XML adicionando um esquema para a propriedade de <xref:System.Xml.Schema.XmlSchemaSet><xref:System.Xml.XmlReaderSettings.Schemas%2A> de um objeto de <xref:System.Xml.XmlReaderSettings> , ou adicionando <xref:System.Xml.Schema.XmlSchemaSet> à propriedade de <xref:System.Xml.XmlReaderSettings.Schemas%2A> de um objeto de <xref:System.Xml.XmlReaderSettings> . O objeto de <xref:System.Xml.XmlReaderSettings> é então usado pelo método de <xref:System.Xml.XmlReader.Create%2A> da classe de <xref:System.Xml.XmlReader> para criar um objeto de <xref:System.Xml.XmlReader> e para validar o documento XML.  
  
 Para saber mais sobre como validar documentos XML usando um <xref:System.Xml.Schema.XmlSchemaSet>, confira [Validação de XSD (esquema XML) com XmlSchemaSet](xml-schema-xsd-validation-with-xmlschemaset.md).  
  
## <a name="see-also"></a>Veja também

- <xref:System.Xml.Schema.XmlSchemaSet.Add%2A>
- <xref:System.Xml.Schema.XmlSchemaSet.Schemas%2A>
- <xref:System.Xml.Schema.XmlSchemaSet.Contains%2A>
- <xref:System.Xml.Schema.XmlSchemaSet.Compile%2A>
- <xref:System.Xml.Schema.XmlSchemaSet.Reprocess%2A>
- <xref:System.Xml.Schema.XmlSchemaSet.Remove%2A>
- <xref:System.Xml.Schema.XmlSchemaSet.RemoveRecursive%2A>
- [XmlSchemaSet como um cache de esquema](xmlschemaset-for-schema-compilation.md)
- [Validação de XSD (esquema XML) com XmlSchemaSet](xml-schema-xsd-validation-with-xmlschemaset.md)
