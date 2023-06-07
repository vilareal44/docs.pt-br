---
title: Serialização
ms.date: 07/20/2015
ms.assetid: 67379a76-5465-4af8-a781-0b0b25a62d9a
ms.openlocfilehash: db14147a23940fa2403613036750be1bca566e8e
ms.sourcegitcommit: f8c270376ed905f6a8896ce0fe25b4f4b38ff498
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/04/2020
ms.locfileid: "84413136"
---
# <a name="serialization-visual-basic"></a>Serialização (Visual Basic)
A serialização é o processo de converter um objeto em um fluxo de bytes para armazenar o objeto ou transmiti-los na memória, um banco de dados ou um arquivo. Sua finalidade principal é salvar o estado de um objeto para recriá-lo quando necessário. O processo inverso é chamado desserialização.  
  
## <a name="how-serialization-works"></a>Como a serialização funciona  
 Esta ilustração mostra o processo geral de serialização.  
  
![Gráfico de serialização](./media/index/serialization-process.gif)
  
 O objeto é serializado em um fluxo, que transporta não apenas os dados, mas informações sobre o tipo de objeto, como sua versão, cultura e nome do assembly. Desse fluxo, ele pode ser armazenado em um banco de dados, um arquivo ou uma memória.  
  
### <a name="uses-for-serialization"></a>Usos para serialização  
 A serialização permite que o desenvolvedor salve o estado de um objeto e recrie conforme necessário, fornecendo o armazenamento dos objetos, bem como a troca de dados. Pela serialização, um desenvolvedor pode executar ações como enviar o objeto para um aplicativo remoto por meio de um serviço Web, passando um objeto de um domínio para outro, passando um objeto por um firewall como uma cadeia de caracteres XML ou mantendo a segurança ou as informações específicas de usuários entre aplicativos.  
  
### <a name="making-an-object-serializable"></a>Tornando um objeto serializável  
 Para serializar um objeto, é necessário que ele esteja serializado, um fluxo contenha o objeto serializado e um <xref:System.Runtime.Serialization.Formatter>. O <xref:System.Runtime.Serialization> contém as classes necessárias para serializar e desserializar objetos.  
  
 Aplique o atributo <xref:System.SerializableAttribute> a um tipo para indicar que as instâncias desse tipo podem ser serializadas. Uma exceção de <xref:System.Runtime.Serialization.SerializationException> será gerada se você tentar serializar, mas o tipo não tiver o atributo <xref:System.SerializableAttribute>.  
  
 Se não desejar que um campo em sua classe seja serializável, aplique o atributo <xref:System.NonSerializedAttribute>. Se um campo de um tipo serializável contiver um ponteiro, um identificador ou outra estrutura de dados que é específica de um determinado ambiente e o campo não puder ser reconstituído em um ambiente diferente, será necessário torná-lo não serializável.  
  
 Se uma classe serializada contiver referências a objetos de outras classes que estão marcadas como <xref:System.SerializableAttribute>, esses objetos também serão serializados.  
  
## <a name="binary-and-xml-serialization"></a>Serialização XML e binária  
 É possível usar serialização XML ou binária. Na serialização binária, todos os membros, mesmo aqueles que são somente leitura, são serializados, e o desempenho é aprimorado. A serialização XML fornece código mais legível, bem como maior flexibilidade de compartilhamento do objeto e uso para fins de interoperabilidade.  
  
### <a name="binary-serialization"></a>Serialização binária  
 A serialização binária usa a codificação binária para produzir uma serialização compacta para usos como armazenamento ou fluxos de rede com base em soquete.  
  
### <a name="xml-serialization"></a>Serialização XML  
 A serialização XML serializa as propriedades e os campos públicos de um objeto, ou os parâmetros e os valores de retorno de métodos, em um fluxo XML que esteja de acordo com um documento XSD (linguagem de definição de esquema XML) específico. A serialização XML resulta em classes fortemente tipadas com propriedades e campos públicos que são convertidos em XML. O <xref:System.Xml.Serialization> contém as classes necessárias para serializar e desserializar XML.  
  
 Você pode aplicar atributos a classes e membros de classe para controlar a maneira como o <xref:System.Xml.Serialization.XmlSerializer> serializa ou desserializa uma instância da classe.  
  
## <a name="basic-and-custom-serialization"></a>Serialização personalizada e básica  
 A serialização pode ser realizada de duas maneiras: básica e personalizada. A serialização básica usa o .NET Framework para serializar o objeto automaticamente.  
  
### <a name="basic-serialization"></a>Serialização básica  
 O único requisito na serialização básica é que o objeto tenha o atributo <xref:System.SerializableAttribute> aplicado. O <xref:System.NonSerializedAttribute> pode ser usado para impedir a serialização de campos específicos.  
  
 Quando você usa a serialização básica, a versão dos objetos pode criar problemas e, nesse caso, a serialização personalizada pode ser preferível. A serialização básica é a maneira mais fácil de executar a serialização, mas ela não fornece muito controle sobre o processo.  
  
### <a name="custom-serialization"></a>Serialização personalizada  
 Na serialização personalizada, você pode especificar exatamente quais objetos vão ser serializados e como isso será feito. A classe deve ser marcada como <xref:System.SerializableAttribute> e implementar a interface <xref:System.Runtime.Serialization.ISerializable>.  
  
 Se você quiser que o objeto também seja desserializado de uma maneira personalizada, você deverá usar um construtor personalizado.  
  
## <a name="designer-serialization"></a>Serialização de designer  
 A serialização de designer é um formulário especial de serialização que envolve o tipo de persistência do objeto geralmente associado a ferramentas de desenvolvimento. A serialização de designer é o processo de conversão de um grafo do objeto em um arquivo de origem que pode, posteriormente, ser usado para recuperar o grafo do objeto. Um arquivo de origem pode conter código, marcação ou até mesmo informações de tabela do SQL.  
  
## <a name="related-topics-and-examples"></a><a name="BKMK_RelatedTopics"></a> Exemplos e tópicos relacionados  
 [Passo a passo: mantendo um objeto no Visual Studio (Visual Basic)](walkthrough-persisting-an-object-in-visual-studio.md)  
 Demonstra como a serialização pode ser usada para manter dados de um objeto entre instâncias, permitindo que você armazene e recupere valores na próxima vez que o objeto for instanciado.  
  
 [Como ler dados de objeto de um arquivo XML (Visual Basic)](how-to-read-object-data-from-an-xml-file.md)  
 Mostra como ler dados de objeto que foram previamente gravados em um arquivo XML usando a classe <xref:System.Xml.Serialization.XmlSerializer>.  
  
 [Como gravar dados de objeto em um arquivo XML (Visual Basic)](how-to-write-object-data-to-an-xml-file.md)  
 Mostra como gravar o objeto de uma classe para um arquivo XML usando a classe <xref:System.Xml.Serialization.XmlSerializer>.
