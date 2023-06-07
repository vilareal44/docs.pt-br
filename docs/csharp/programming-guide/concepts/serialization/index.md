---
title: Serialização (C#)
description: A serialização converte um objeto em um fluxo de bytes para armazenar o objeto ou transmiti-lo para a memória, um banco de dados ou um arquivo.
ms.date: 01/02/2020
ms.openlocfilehash: 29625648b19c97556c107997ef9ecd3f0f971cbf
ms.sourcegitcommit: 7499bdb428d63ed0e19e97f54d3d576c41598659
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/31/2020
ms.locfileid: "87455742"
---
# <a name="serialization-c"></a>Serialização (C#)

A serialização é o processo de converter um objeto em um fluxo de bytes para armazenar o objeto ou transmiti-lo para a memória, um banco de dados ou um arquivo. Sua finalidade principal é salvar o estado de um objeto para recriá-lo quando necessário. O processo inverso é chamado desserialização.

## <a name="how-serialization-works"></a>Como a serialização funciona

Esta ilustração mostra o processo geral de serialização:

![Gráfico de serialização](./media/index/serialization-process.gif)

O objeto é serializado para um fluxo que transporta os dados. O fluxo também pode ter informações sobre o tipo do objeto, como sua versão, cultura e nome do assembly. A partir desse fluxo, o objeto pode ser armazenado em um banco de dados, em um arquivo ou em uma memória.

### <a name="uses-for-serialization"></a>Usos para serialização

A serialização permite que o desenvolvedor salve o estado de um objeto e recrie-o conforme necessário, fornecendo armazenamento de objetos, bem como troca de dados. Por meio da serialização, um desenvolvedor pode executar ações como:

* Enviando o objeto para um aplicativo remoto usando um serviço Web
* Passando um objeto de um domínio para outro
* Passando um objeto por meio de um firewall como uma cadeia de caracteres JSON ou XML
* Manter informações específicas de segurança ou de usuário entre aplicativos

## <a name="json-serialization"></a>Serialização JSON

O <xref:System.Text.Json> namespace contém classes para serialização e desserialização de JavaScript Object Notation (JSON). JSON é um padrão aberto que normalmente é usado para compartilhar dados na Web.

A serialização JSON serializa as propriedades públicas de um objeto em uma cadeia de caracteres, matriz de bytes ou fluxo que está em conformidade com [a especificação JSON RFC 8259](https://tools.ietf.org/html/rfc8259). Para controlar a maneira de <xref:System.Text.Json.JsonSerializer> serializar ou desserializar uma instância da classe:

* Usar um <xref:System.Text.Json.JsonSerializerOptions> objeto
* Aplicar atributos do <xref:System.Text.Json.Serialization> namespace a classes ou propriedades
* [Implementar conversores personalizados](../../../../standard/serialization/system-text-json-converters-how-to.md)

## <a name="binary-and-xml-serialization"></a>Serialização XML e binária

O <xref:System.Runtime.Serialization> namespace contém classes para serialização e desserialização binária e XML.

A serialização binária usa a codificação binária para produzir uma serialização compacta para usos como armazenamento ou fluxos de rede com base em soquete. Na serialização binária, todos os membros, mesmo aqueles que são somente leitura, são serializados e o desempenho é aprimorado.

[!INCLUDE [binary-serialization-warning](~/includes/binary-serialization-warning.md)]

A serialização XML serializa as propriedades e os campos públicos de um objeto, ou os parâmetros e os valores de retorno de métodos, em um fluxo XML que esteja de acordo com um documento XSD (linguagem de definição de esquema XML) específico. A serialização XML resulta em classes fortemente tipadas com propriedades e campos públicos que são convertidos em XML. <xref:System.Xml.Serialization>contém classes para serialização e desserialização de XML. Aplique atributos a classes e a membros de classe para controlar a maneira como o <xref:System.Xml.Serialization.XmlSerializer> serializa ou desserializa uma instância da classe.

### <a name="making-an-object-serializable"></a>Tornando um objeto serializável

Para serialização binária ou XML, você precisa de:

* O objeto a ser serializado
* Um fluxo para conter o objeto serializado
* Uma <xref:System.Runtime.Serialization.Formatter?displayProperty=fullName> instância do

Aplique o <xref:System.SerializableAttribute> atributo a um tipo para indicar que as instâncias do tipo podem ser serializadas. Uma exceção será gerada se você tentar serializar, mas o tipo não terá o atributo <xref:System.SerializableAttribute>.

Para impedir que um campo seja serializado, aplique o <xref:System.NonSerializedAttribute> atributo. Se um campo de um tipo serializável contiver um ponteiro, um identificador ou outra estrutura de dados que é específica de um determinado ambiente e o campo não puder ser reconstituído em um ambiente diferente, será necessário torná-lo não serializável.

Se uma classe serializada contiver referências a objetos de outras classes que estão marcadas como <xref:System.SerializableAttribute>, esses objetos também serão serializados.

### <a name="basic-and-custom-serialization"></a>Serialização básica e personalizada

A serialização binária e XML pode ser executada de duas maneiras, básicas e personalizadas.

A serialização básica usa o .NET para serializar automaticamente o objeto. O único requisito é que a classe tenha o <xref:System.SerializableAttribute> atributo aplicado. O <xref:System.NonSerializedAttribute> pode ser usado para impedir a serialização de campos específicos.

Quando você usa a serialização básica, a criação de versão de objetos pode criar problemas. Use a serialização personalizada quando problemas de criação de versão forem importantes. A serialização básica é a maneira mais fácil de executar a serialização, mas ela não fornece muito controle sobre o processo.

Na serialização personalizada, você pode especificar exatamente quais objetos vão ser serializados e como isso será feito. A classe deve ser marcada como <xref:System.SerializableAttribute> e implementar a interface <xref:System.Runtime.Serialization.ISerializable>. Se você quiser que seu objeto seja desserializado de uma maneira personalizada também, use um construtor personalizado.

## <a name="designer-serialization"></a>Serialização de designer

A serialização de designer é um formulário especial de serialização que envolve o tipo de persistência do objeto associado a ferramentas de desenvolvimento. A serialização de designer é o processo de conversão de um grafo do objeto em um arquivo de origem que pode, posteriormente, ser usado para recuperar o grafo do objeto. Um arquivo de origem pode conter código, marcação ou até mesmo informações de tabela do SQL.

## <a name="related-topics-and-examples"></a><a name="BKMK_RelatedTopics"></a> Exemplos e tópicos relacionados  

[System.Text.Jsem visão geral](../../../../standard/serialization/system-text-json-overview.md) Mostra como obter a `System.Text.Json` biblioteca.

[Como serializar e desserializar JSON no .net](../../../../standard/serialization/system-text-json-how-to.md).
Mostra como ler e gravar dados de objeto de e para JSON usando a <xref:System.Text.Json.JsonSerializer> classe.

[Passo a passo: mantendo um objeto no Visual Studio (C#)](walkthrough-persisting-an-object-in-visual-studio.md)  
Demonstra como a serialização pode ser usada para manter dados de um objeto entre instâncias, permitindo que você armazene e recupere valores na próxima vez que o objeto for instanciado.

[Como ler dados de objeto de um arquivo XML (C#)](how-to-read-object-data-from-an-xml-file.md)  
Mostra como ler dados de objeto que foram previamente gravados em um arquivo XML usando a classe <xref:System.Xml.Serialization.XmlSerializer>.

[Como gravar dados de objeto em um arquivo XML (C#)](how-to-write-object-data-to-an-xml-file.md)  
Mostra como gravar o objeto de uma classe para um arquivo XML usando a classe <xref:System.Xml.Serialization.XmlSerializer>.
