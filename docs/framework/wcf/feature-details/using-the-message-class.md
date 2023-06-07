---
title: Usando a classe de mensagens
description: Saiba mais sobre a classe de mensagem, que é fundamental para o WCF. Você precisa programar usando a classe de mensagem diretamente apenas em alguns cenários avançados.
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
ms.assetid: d1d62bfb-2aa3-4170-b6f8-c93d3afdbbed
ms.openlocfilehash: f806e257cfd3ccc5118a5783e2eda48eef4ba0bf
ms.sourcegitcommit: 358a28048f36a8dca39a9fe6e6ac1f1913acadd5
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/23/2020
ms.locfileid: "85246487"
---
# <a name="using-the-message-class"></a>Usando a classe de mensagens
A <xref:System.ServiceModel.Channels.Message> classe é fundamental para Windows Communication Foundation (WCF). Toda a comunicação entre clientes e serviços finalmente resulta em <xref:System.ServiceModel.Channels.Message> instâncias sendo enviadas e recebidas.  
  
 Normalmente, você não interage com a <xref:System.ServiceModel.Channels.Message> classe diretamente. Em vez disso, as construções de modelo de serviço do WCF, como contratos de dados, contratos de mensagem e contratos de operação, são usadas para descrever mensagens de entrada e saída. No entanto, em alguns cenários avançados, você pode programar usando a <xref:System.ServiceModel.Channels.Message> classe diretamente. Por exemplo, talvez você queira usar a <xref:System.ServiceModel.Channels.Message> classe:  
  
- Quando você precisa de uma maneira alternativa de criar o conteúdo da mensagem de saída (por exemplo, criar uma mensagem diretamente de um arquivo em disco) em vez de serializar os objetos .NET Framework.  
  
- Quando você precisa de uma maneira alternativa de usar o conteúdo da mensagem de entrada (por exemplo, quando você deseja aplicar uma transformação XSLT ao conteúdo XML bruto) em vez de desserializar em objetos .NET Framework.  
  
- Quando você precisa lidar com mensagens de uma maneira geral, independentemente do conteúdo da mensagem (por exemplo, ao rotear ou encaminhar mensagens ao criar um roteador, balanceador de carga ou um sistema de publicação/assinatura).  
  
 Antes de usar a <xref:System.ServiceModel.Channels.Message> classe, familiarize-se com a arquitetura de transferência de dados do WCF em [transferência de dados visão geral da arquitetura](data-transfer-architectural-overview.md).  
  
 Um <xref:System.ServiceModel.Channels.Message> é um contêiner de finalidade geral para dados, mas seu design segue o design de uma mensagem no protocolo SOAP. Assim como no SOAP, uma mensagem tem um corpo de mensagem e cabeçalhos. O corpo da mensagem contém os dados de carga reais, enquanto os cabeçalhos contêm contêineres de dados nomeados adicionais. As regras para ler e gravar o corpo e os cabeçalhos são diferentes, por exemplo, os cabeçalhos sempre são armazenados em buffer na memória e podem ser acessados em qualquer ordem várias vezes, enquanto o corpo pode ser lido apenas uma vez e pode ser transmitido. Normalmente, ao usar SOAP, o corpo da mensagem é mapeado para o corpo SOAP e os cabeçalhos da mensagem são mapeados para os cabeçalhos SOAP.  
  
## <a name="using-the-message-class-in-operations"></a>Usando a classe Message em operações  
 Você pode usar a <xref:System.ServiceModel.Channels.Message> classe como um parâmetro de entrada de uma operação, o valor de retorno de uma operação ou ambos. Se <xref:System.ServiceModel.Channels.Message> for usado em qualquer lugar em uma operação, as seguintes restrições serão aplicáveis:  
  
- A operação não pode ter `out` nenhum `ref` parâmetro ou.  
  
- Não pode haver mais de um `input` parâmetro. Se o parâmetro estiver presente, ele deverá ser uma mensagem ou um tipo de contrato de mensagem.  
  
- O tipo de retorno deve ser `void` , `Message` ou um tipo de contrato de mensagem.  
  
 O exemplo de código a seguir contém um contrato de operação válido.  
  
 [!code-csharp[C_UsingTheMessageClass#1](../../../../samples/snippets/csharp/VS_Snippets_CFX/c_usingthemessageclass/cs/source.cs#1)]
 [!code-vb[C_UsingTheMessageClass#1](../../../../samples/snippets/visualbasic/VS_Snippets_CFX/c_usingthemessageclass/vb/source.vb#1)]  
  
## <a name="creating-basic-messages"></a>Criando mensagens básicas  
 A <xref:System.ServiceModel.Channels.Message> classe fornece métodos de fábrica estáticos `CreateMessage` que você pode usar para criar mensagens básicas.  
  
 Todas as `CreateMessage` sobrecargas usam um parâmetro de versão do tipo <xref:System.ServiceModel.Channels.MessageVersion> que indica as versões SOAP e WS-Addressing a serem usadas para a mensagem. Se você quiser usar as mesmas versões de protocolo que a mensagem de entrada, poderá usar a <xref:System.ServiceModel.OperationContext.IncomingMessageVersion%2A> Propriedade na <xref:System.ServiceModel.OperationContext> instância obtida da <xref:System.ServiceModel.OperationContext.Current%2A> propriedade. A maioria das `CreateMessage` sobrecargas também tem um parâmetro de cadeia de caracteres que indica a ação SOAP a ser usada para a mensagem. A versão pode ser definida como `None` para desabilitar a geração de envelope SOAP; a mensagem consiste apenas no corpo.  
  
## <a name="creating-messages-from-objects"></a>Criando mensagens de objetos  
 A sobrecarga mais básica `CreateMessage` que usa apenas uma versão e uma ação cria uma mensagem com um corpo vazio. Outra sobrecarga usa um <xref:System.Object> parâmetro adicional; isso cria uma mensagem cujo corpo é a representação serializada do objeto fornecido. Use as <xref:System.Runtime.Serialization.DataContractSerializer> configurações padrão para serialização. Se você quiser usar um serializador diferente ou desejar configurá-lo de `DataContractSerializer` forma diferente, use a `CreateMessage` sobrecarga que também usa um `XmlObjectSerializer` parâmetro.  
  
 Por exemplo, para retornar um objeto em uma mensagem, você pode usar o código a seguir.  
  
 [!code-csharp[C_UsingTheMessageClass#2](../../../../samples/snippets/csharp/VS_Snippets_CFX/c_usingthemessageclass/cs/source.cs#2)]
 [!code-vb[C_UsingTheMessageClass#2](../../../../samples/snippets/visualbasic/VS_Snippets_CFX/c_usingthemessageclass/vb/source.vb#2)]  
  
## <a name="creating-messages-from-xml-readers"></a>Criando mensagens de leitores XML  
 Há `CreateMessage` sobrecargas que usam um <xref:System.Xml.XmlReader> ou um <xref:System.Xml.XmlDictionaryReader> para o corpo em vez de um objeto. Nesse caso, o corpo da mensagem contém o XML que resulta da leitura do leitor de XML passado. Por exemplo, o código a seguir retorna uma mensagem com conteúdo do corpo lido a partir de um arquivo XML.  
  
 [!code-csharp[C_UsingTheMessageClass#3](../../../../samples/snippets/csharp/VS_Snippets_CFX/c_usingthemessageclass/cs/source.cs#3)]
 [!code-vb[C_UsingTheMessageClass#3](../../../../samples/snippets/visualbasic/VS_Snippets_CFX/c_usingthemessageclass/vb/source.vb#3)]  
  
 Além disso, há `CreateMessage` sobrecargas que usam um <xref:System.Xml.XmlReader> ou um <xref:System.Xml.XmlDictionaryReader> que representa a mensagem inteira e não apenas o corpo. Essas sobrecargas também usam um `maxSizeOfHeaders` parâmetro inteiro. Os cabeçalhos sempre são armazenados em buffer na memória assim que a mensagem é criada, e esse parâmetro limita a quantidade de buffers que ocorre. É importante definir esse parâmetro como um valor seguro se o XML estiver vindo de uma fonte não confiável para atenuar a possibilidade de um ataque de negação de serviço. As versões SOAP e WS-Addressing da mensagem que o leitor de XML representa devem corresponder às versões indicadas usando o parâmetro version.  
  
## <a name="creating-messages-with-bodywriter"></a>Criando mensagens com BodyWriter  
 Uma `CreateMessage` sobrecarga usa uma `BodyWriter` instância para descrever o corpo da mensagem. Uma `BodyWriter` é uma classe abstrata que pode ser derivada para personalizar como os corpos de mensagens são criados. Você pode criar sua própria `BodyWriter` classe derivada para descrever os corpos de mensagens de uma maneira personalizada. Você deve substituir o `BodyWriter.OnWriteBodyContents` método que recebe um <xref:System.Xml.XmlDictionaryWriter> ; esse método é responsável por gravar o corpo.  
  
 Gravadores de corpo podem ser armazenados em buffer ou não armazenados em buffer (transmitidos). Gravadores de corpo em buffer podem gravar seu conteúdo várias vezes, enquanto os transmitidos podem gravar seu conteúdo apenas uma vez. A `IsBuffered` propriedade indica se um gravador de corpo está armazenado em buffer ou não. Você pode definir isso para o gravador de corpo chamando o `BodyWriter` construtor protegido que usa um `isBuffered` parâmetro booliano. Escritores de corpo dão suporte à criação de um gravador de corpo em buffer de um gravador de corpo sem buffer. Você pode substituir o `OnCreateBufferedCopy` método para personalizar esse processo. Por padrão, um buffer na memória que contém o XML retornado por `OnWriteBodyContents` é usado. `OnCreateBufferedCopy`usa um `maxBufferSize` parâmetro inteiro; se você substituir esse método, não deverá criar buffers maiores que esse tamanho máximo.  
  
 A `BodyWriter` classe fornece os `WriteBodyContents` `CreateBufferedCopy` métodos e, que são essencialmente invólucros finos `OnWriteBodyContents` e `OnCreateBufferedCopy` métodos, respectivamente. Esses métodos executam a verificação de estado para garantir que um gravador de corpo não armazenado em buffer não seja acessado mais de uma vez. Esses métodos são chamados diretamente somente ao criar `Message` classes derivadas personalizadas com base em `BodyWriters` .  
  
## <a name="creating-fault-messages"></a>Criando mensagens de falha  
 Você pode usar determinadas `CreateMessage` sobrecargas para criar mensagens de falha de SOAP. O mais básico deles usa um <xref:System.ServiceModel.Channels.MessageFault> objeto que descreve a falha. Outras sobrecargas são fornecidas por conveniência. A primeira sobrecarga usa uma `FaultCode` e uma cadeia de caracteres de motivo e cria um `MessageFault` usando `MessageFault.CreateFault` essas informações. A outra sobrecarga usa um objeto de detalhe e também a passa para `CreateFault` junto com o código de falha e o motivo. Por exemplo, a operação a seguir retorna uma falha.  
  
 [!code-csharp[C_UsingTheMessageClass#4](../../../../samples/snippets/csharp/VS_Snippets_CFX/c_usingthemessageclass/cs/source.cs#4)]
 [!code-vb[C_UsingTheMessageClass#4](../../../../samples/snippets/visualbasic/VS_Snippets_CFX/c_usingthemessageclass/vb/source.vb#4)]  
  
## <a name="extracting-message-body-data"></a>Extraindo dados do corpo da mensagem  
 A `Message` classe dá suporte a várias maneiras de extrair informações de seu corpo. Elas podem ser classificadas nas seguintes categorias:  
  
- Obtendo todo o corpo da mensagem escrito de uma vez a um gravador de XML. Isso é chamado de *gravação de uma mensagem*.  
  
- Obtendo um leitor de XML no corpo da mensagem. Isso permite que você acesse mais tarde o corpo da mensagem por parte, conforme necessário. Isso é conhecido como *leitura de uma mensagem*.  
  
- A mensagem inteira, incluindo seu corpo, pode ser copiada para um buffer na memória do <xref:System.ServiceModel.Channels.MessageBuffer> tipo. Isso é conhecido como *copiar uma mensagem*.  
  
 Você pode acessar o corpo de uma `Message` só vez, independentemente de como ele é acessado. Um objeto Message tem uma `State` propriedade, que é inicialmente definida como Created. Os três métodos de acesso descritos na lista anterior definem o estado como gravado, lido e copiado, respectivamente. Além disso, um `Close` método pode definir o estado como fechado quando o conteúdo do corpo da mensagem não é mais necessário. O corpo da mensagem pode ser acessado somente no estado criado e não há como voltar para o estado criado depois que o estado for alterado.  
  
## <a name="writing-messages"></a>Gravando mensagens  
 O <xref:System.ServiceModel.Channels.Message.WriteBodyContents%28System.Xml.XmlDictionaryWriter%29> método grava o conteúdo do corpo de uma determinada `Message` instância em um determinado gravador de XML. O <xref:System.ServiceModel.Channels.Message.WriteBody%2A> método faz o mesmo, exceto pelo fato de que ele inclui o conteúdo do corpo no elemento wrapper apropriado (por exemplo, <`soap:body`>). Por fim, <xref:System.ServiceModel.Channels.Message.WriteMessage%2A> o grava toda a mensagem, incluindo o envelope SOAP de quebra e os cabeçalhos. Se SOAP está desativado ( <xref:System.ServiceModel.Channels.Message.Version> is <xref:System.ServiceModel.Channels.MessageVersion.None?displayProperty=nameWithType> ), todos os três métodos fazem a mesma coisa: eles gravam o conteúdo do corpo da mensagem.  
  
 Por exemplo, o código a seguir grava o corpo de uma mensagem de entrada em um arquivo.  
  
 [!code-csharp[C_UsingTheMessageClass#5](../../../../samples/snippets/csharp/VS_Snippets_CFX/c_usingthemessageclass/cs/source.cs#5)]
 [!code-vb[C_UsingTheMessageClass#5](../../../../samples/snippets/visualbasic/VS_Snippets_CFX/c_usingthemessageclass/vb/source.vb#5)]  
  
 Dois métodos auxiliares adicionais gravam determinadas marcas de elemento de início SOAP. Esses métodos não acessam o corpo da mensagem e, portanto, não alteram o estado da mensagem. Eles incluem:  
  
- <xref:System.ServiceModel.Channels.Message.WriteStartBody%2A>grava o elemento do corpo inicial, por exemplo, `<soap:Body>` .  
  
- <xref:System.ServiceModel.Channels.Message.WriteStartEnvelope%2A>grava o elemento iniciar envelope, por exemplo, `<soap:Envelope>` .  
  
 Para gravar as marcas de elemento final correspondentes, chame `WriteEndElement` o gravador de XML correspondente. Esses métodos raramente são chamados diretamente.  
  
## <a name="reading-messages"></a>Lendo mensagens  
 A principal maneira de ler um corpo de mensagem é chamar <xref:System.ServiceModel.Channels.Message.GetReaderAtBodyContents%2A> . Você Obtém um <xref:System.Xml.XmlDictionaryReader> que pode ser usado para ler o corpo da mensagem. Observe que as <xref:System.ServiceModel.Channels.Message> transições para o estado de leitura assim que <xref:System.ServiceModel.Channels.Message.GetReaderAtBodyContents%2A> são chamadas, e não quando você usa o leitor XML retornado.  
  
 O <xref:System.ServiceModel.Channels.Message.GetBody%2A> método também permite que você acesse o corpo da mensagem como um objeto tipado. Internamente, esse método usa `GetReaderAtBodyContents` e, portanto, também faz a transição do estado da mensagem para o <xref:System.ServiceModel.Channels.MessageState.Read> estado (consulte a <xref:System.ServiceModel.Channels.Message.State%2A> Propriedade).  
  
 É uma boa prática verificar a <xref:System.ServiceModel.Channels.Message.IsEmpty%2A> Propriedade; nesse caso, o corpo da mensagem está vazio e <xref:System.ServiceModel.Channels.Message.GetReaderAtBodyContents%2A> gera um <xref:System.InvalidOperationException> . Além disso, se for uma mensagem recebida (por exemplo, a resposta), talvez você também queira verificar <xref:System.ServiceModel.Channels.Message.IsFault%2A> , que indica se a mensagem contém uma falha.  
  
 A sobrecarga mais básica de <xref:System.ServiceModel.Channels.Message.GetBody%2A> desserializa o corpo da mensagem em uma instância de um tipo (indicado pelo parâmetro genérico) usando um <xref:System.Runtime.Serialization.DataContractSerializer> configurado com as configurações padrão e com a <xref:System.Runtime.Serialization.DataContractSerializer.MaxItemsInObjectGraph%2A> cota desabilitada. Se você quiser usar um mecanismo de serialização diferente ou configurar o `DataContractSerializer` de forma não padrão, use a <xref:System.ServiceModel.Channels.Message.GetBody%2A> sobrecarga que usa um <xref:System.Runtime.Serialization.XmlObjectSerializer> .  
  
 Por exemplo, o código a seguir extrai dados de um corpo de mensagem que contém um `Person` objeto serializado e imprime o nome da pessoa.  
  
 [!code-csharp[C_UsingTheMessageClass#6](../../../../samples/snippets/csharp/VS_Snippets_CFX/c_usingthemessageclass/cs/source.cs#6)]
 [!code-vb[C_UsingTheMessageClass#6](../../../../samples/snippets/visualbasic/VS_Snippets_CFX/c_usingthemessageclass/vb/source.vb#6)]  
  
## <a name="copying-a-message-into-a-buffer"></a>Copiando uma mensagem em um buffer  
 Às vezes, é necessário acessar o corpo da mensagem mais de uma vez, por exemplo, para encaminhar a mesma mensagem a vários destinos como parte de um sistema de assinante Publicador. Nesse caso, é necessário armazenar em buffer a mensagem inteira (incluindo o corpo) na memória. Você pode fazer isso chamando <xref:System.ServiceModel.Channels.Message.CreateBufferedCopy%28System.Int32%29> . Esse método usa um parâmetro inteiro que representa o tamanho máximo do buffer e cria um buffer que não é maior que esse tamanho. É importante definir isso como um valor seguro se a mensagem for proveniente de uma fonte não confiável.  
  
 O buffer é retornado como uma <xref:System.ServiceModel.Channels.MessageBuffer> instância. Você pode acessar dados no buffer de várias maneiras. A principal maneira é chamar <xref:System.ServiceModel.Channels.Message.CreateMessage%2A> para criar `Message` instâncias a partir do buffer.  
  
 Outra maneira de acessar os dados no buffer é implementar a <xref:System.Xml.XPath.IXPathNavigable> interface que a <xref:System.ServiceModel.Channels.MessageBuffer> classe implementa para acessar o XML subjacente diretamente. Algumas <xref:System.ServiceModel.Channels.MessageBuffer.CreateNavigator%2A> sobrecargas permitem que você crie <xref:System.Xml.XPath> navegadores protegidos por uma cota de nó, limitando o número de nós XML que podem ser visitados. Isso ajuda a evitar ataques de negação de serviço com base no tempo de processamento demorado. Essa cotação está desabilitada por padrão. Algumas `CreateNavigator` sobrecargas permitem que você especifique como o espaço em branco deve ser manipulado no XML usando a <xref:System.Xml.XmlSpace> enumeração, com o padrão sendo `XmlSpace.None` .  
  
 Uma maneira final de acessar o conteúdo de um buffer de mensagens é gravar seu conteúdo em um fluxo usando <xref:System.ServiceModel.Channels.Message.WriteMessage%2A> .  
  
 O exemplo a seguir demonstra o processo de trabalhar com um `MessageBuffer` : uma mensagem de entrada é encaminhada para vários destinatários e, em seguida, registrada em um arquivo. Sem o armazenamento em buffer, isso não é possível, porque o corpo da mensagem pode ser acessado apenas uma vez.  
  
 [!code-csharp[C_UsingTheMessageClass#7](../../../../samples/snippets/csharp/VS_Snippets_CFX/c_usingthemessageclass/cs/source.cs#7)]
 [!code-vb[C_UsingTheMessageClass#7](../../../../samples/snippets/visualbasic/VS_Snippets_CFX/c_usingthemessageclass/vb/source.vb#7)]  
  
 A `MessageBuffer` classe tem outros membros que vale a pena observar. O <xref:System.ServiceModel.Channels.MessageBuffer.Close%2A> método pode ser chamado para liberar recursos quando o conteúdo do buffer não for mais necessário. A <xref:System.ServiceModel.Channels.MessageBuffer.BufferSize%2A> propriedade retorna o tamanho do buffer alocado. A <xref:System.ServiceModel.Channels.MessageBuffer.MessageContentType%2A> propriedade retorna o tipo de conteúdo MIME da mensagem.  
  
## <a name="accessing-the-message-body-for-debugging"></a>Acessando o corpo da mensagem para depuração  
 Para fins de depuração, você pode chamar o <xref:System.ServiceModel.Channels.Message.ToString%2A> método para obter uma representação da mensagem como uma cadeia de caracteres. Essa representação geralmente corresponde à maneira como uma mensagem examinaria a transmissão se fosse codificada com o codificador de texto, exceto que o XML seria melhor formatado para facilitar a leitura humana. A única exceção é o corpo da mensagem. O corpo pode ser lido apenas uma vez e `ToString` não altera o estado da mensagem. Portanto, o `ToString` método pode não ser capaz de acessar o corpo e pode substituir um espaço reservado (por exemplo, "..." ou três pontos) em vez do corpo da mensagem. Portanto, não use `ToString` o para registrar mensagens se o conteúdo do corpo das mensagens for importante.  
  
## <a name="accessing-other-message-parts"></a>Acessando outras partes da mensagem  
 Várias propriedades são fornecidas para acessar informações sobre a mensagem, além do conteúdo do corpo. No entanto, eles não podem ser chamados depois que a mensagem for fechada:  
  
- A <xref:System.ServiceModel.Channels.Message.Headers%2A> propriedade representa os cabeçalhos da mensagem. Consulte a seção "trabalhando com cabeçalhos" mais adiante neste tópico.  
  
- A <xref:System.ServiceModel.Channels.Message.Properties%2A> propriedade representa as propriedades de mensagem, que são partes de dados nomeados anexados à mensagem que geralmente não são emitidas quando a mensagem é enviada. Consulte a seção "trabalhando com propriedades" mais adiante neste tópico.  
  
- A <xref:System.ServiceModel.Channels.Message.Version%2A> propriedade indica a versão SOAP e WS-Addressing associada à mensagem, ou `None` se SOAP estiver desabilitado.  
  
- A <xref:System.ServiceModel.Channels.Message.IsFault%2A> propriedade retornará `true` se a mensagem for uma mensagem de falha SOAP.  
  
- A <xref:System.ServiceModel.Channels.Message.IsEmpty%2A> propriedade retornará `true` se a mensagem estiver vazia.  
  
 Você pode usar o <xref:System.ServiceModel.Channels.Message.GetBodyAttribute%28System.String%2CSystem.String%29> método para acessar um atributo específico no elemento wrapper do corpo (por exemplo, `<soap:Body>` ) identificado por um nome e namespace específicos. Se esse atributo não for encontrado, `null` será retornado. Esse método pode ser chamado somente quando o `Message` estiver no estado criado (quando o corpo da mensagem ainda não tiver sido acessado).  
  
## <a name="working-with-headers"></a>Trabalhando com cabeçalhos  
 Um `Message` pode conter qualquer número de fragmentos de XML nomeados, chamados *cabeçalhos*. Cada fragmento normalmente é mapeado para um cabeçalho SOAP. Os cabeçalhos são acessados por meio da `Headers` Propriedade do tipo <xref:System.ServiceModel.Channels.MessageHeaders> . <xref:System.ServiceModel.Channels.MessageHeaders>é uma coleção de <xref:System.ServiceModel.Channels.MessageHeaderInfo> objetos, e cabeçalhos individuais podem ser acessados por meio de sua <xref:System.Collections.IEnumerable> interface ou por meio de seu indexador. Por exemplo, o código a seguir lista os nomes de todos os cabeçalhos em um `Message` .  
  
 [!code-csharp[C_UsingTheMessageClass#8](../../../../samples/snippets/csharp/VS_Snippets_CFX/c_usingthemessageclass/cs/source.cs#8)]
 [!code-vb[C_UsingTheMessageClass#8](../../../../samples/snippets/visualbasic/VS_Snippets_CFX/c_usingthemessageclass/vb/source.vb#8)]  
  
#### <a name="adding-removing-finding-headers"></a>Adicionando, removendo, localizando cabeçalhos  
 Você pode adicionar um novo cabeçalho ao final de todos os cabeçalhos existentes usando o <xref:System.ServiceModel.Channels.MessageHeaders.Add%2A> método. Você pode usar o <xref:System.ServiceModel.Channels.MessageHeaders.Insert%2A> método para inserir um cabeçalho em um índice específico. Os cabeçalhos existentes são deslocados para o item inserido. Os cabeçalhos são ordenados de acordo com seu índice e o primeiro índice disponível é 0. Você pode usar as várias <xref:System.ServiceModel.Channels.MessageHeaders.CopyHeadersFrom%2A> sobrecargas de método para adicionar cabeçalhos de uma `Message` instância diferente ou `MessageHeaders` . Algumas sobrecargas copiam um cabeçalho individual, enquanto outros copiam todos eles. O <xref:System.ServiceModel.Channels.MessageHeaders.Clear%2A> método remove todos os cabeçalhos. O <xref:System.ServiceModel.Channels.MessageHeaders.RemoveAt%2A> método Remove um cabeçalho em um índice específico (deslocando todos os cabeçalhos depois dele). O <xref:System.ServiceModel.Channels.MessageHeaders.RemoveAll%2A> método remove todos os cabeçalhos com um nome e namespace específicos.  
  
 Recupere um cabeçalho específico usando o <xref:System.ServiceModel.Channels.MessageHeaders.FindHeader%2A> método. Esse método usa o nome e o namespace do cabeçalho para localizar e retorna seu índice. Se o cabeçalho ocorrer mais de uma vez, uma exceção será lançada. Se o cabeçalho não for encontrado, ele retornará-1.  
  
 No modelo de cabeçalho SOAP, os cabeçalhos podem ter um `Actor` valor que especifica o destinatário pretendido do cabeçalho. A sobrecarga mais básica `FindHeader` procura somente os cabeçalhos destinados ao receptor final da mensagem. No entanto, outra sobrecarga permite que você especifique quais `Actor` valores serão incluídos na pesquisa. Para obter mais informações, consulte a especificação SOAP.  
  
 Um <xref:System.ServiceModel.Channels.MessageHeaders.CopyTo%28System.ServiceModel.Channels.MessageHeaderInfo%5B%5D%2CSystem.Int32%29> método é fornecido para copiar cabeçalhos de uma <xref:System.ServiceModel.Channels.MessageHeaders> coleção para uma matriz de <xref:System.ServiceModel.Channels.MessageHeaderInfo> objetos.  
  
 Para acessar os dados XML em um cabeçalho, você pode chamar <xref:System.ServiceModel.Channels.MessageHeaders.GetReaderAtHeader%2A> e retornar um leitor de XML para o índice de cabeçalho específico. Se você quiser desserializar o conteúdo do cabeçalho em um objeto, use <xref:System.ServiceModel.Channels.MessageHeaders.GetHeader%60%601%28System.Int32%29> ou uma das outras sobrecargas. As sobrecargas mais básicas desserializam cabeçalhos usando o <xref:System.Runtime.Serialization.DataContractSerializer> configurado da maneira padrão. Se você quiser usar um serializador diferente ou uma configuração diferente do `DataContractSerializer` , use uma das sobrecargas que usam um `XmlObjectSerializer` . Também há sobrecargas que usam o nome do cabeçalho, o namespace e, opcionalmente, uma lista de `Actor` valores em vez de um índice; essa é uma combinação de `FindHeader` e `GetHeader` .  
  
## <a name="working-with-properties"></a>Trabalhando com propriedades  
 Uma `Message` instância pode conter um número arbitrário de objetos nomeados de tipos arbitrários. Essa coleção é acessada por meio da `Properties` Propriedade do tipo `MessageProperties` . A coleção implementa a <xref:System.Collections.Generic.IDictionary%602> interface e atua como um mapeamento de <xref:System.String> para <xref:System.Object> . Normalmente, os valores de propriedade não são mapeados diretamente para qualquer parte da mensagem na conexão, mas fornecem várias dicas de processamento de mensagens para os vários canais na pilha de canais do WCF ou para a <xref:System.ServiceModel.Channels.MessageHeaders.CopyTo%28System.ServiceModel.Channels.MessageHeaderInfo%5B%5D%2CSystem.Int32%29> estrutura de serviço. Para obter um exemplo, consulte [visão geral da arquitetura de transferência de dados](data-transfer-architectural-overview.md).  
  
## <a name="inheriting-from-the-message-class"></a>Herdando da classe Message  
 Se os tipos de mensagem internos criados usando `CreateMessage` o não atenderem aos seus requisitos, crie uma classe derivada da `Message` classe.  
  
### <a name="defining-the-message-body-contents"></a>Definindo o conteúdo do corpo da mensagem  
 Existem três técnicas principais para acessar dados dentro de um corpo de mensagem: gravando, lendo e copiando-a em um buffer. Em última análise, essas operações resultam nos <xref:System.ServiceModel.Channels.Message.OnWriteBodyContents%2A> <xref:System.ServiceModel.Channels.Message.OnGetReaderAtBodyContents%2A> métodos, e <xref:System.ServiceModel.Channels.Message.OnCreateBufferedCopy%2A> que estão sendo chamados, respectivamente, na classe derivada de `Message` . A `Message` classe base garante que apenas um desses métodos seja chamado para cada `Message` instância e que não seja chamado mais de uma vez. A classe base também garante que os métodos não sejam chamados em uma mensagem fechada. Não é necessário acompanhar o estado da mensagem em sua implementação.  
  
 <xref:System.ServiceModel.Channels.Message.OnWriteBodyContents%2A>é um método abstrato e deve ser implementado. A maneira mais básica de definir o conteúdo do corpo de sua mensagem é escrever usando esse método. Por exemplo, a seguinte mensagem contém 100.000 números aleatórios de 1 a 20.  
  
 [!code-csharp[C_UsingTheMessageClass#9](../../../../samples/snippets/csharp/VS_Snippets_CFX/c_usingthemessageclass/cs/source.cs#9)]
 [!code-vb[C_UsingTheMessageClass#9](../../../../samples/snippets/visualbasic/VS_Snippets_CFX/c_usingthemessageclass/vb/source.vb#9)]  
  
 Os <xref:System.ServiceModel.Channels.Message.OnGetReaderAtBodyContents> <xref:System.ServiceModel.Channels.Message.OnCreateBufferedCopy%2A> métodos e têm implementações padrão que funcionam na maioria dos casos. As implementações padrão chamam <xref:System.ServiceModel.Channels.Message.OnWriteBodyContents%2A> , armazenam em buffer os resultados e funcionam com o buffer resultante. No entanto, em alguns casos, isso pode não ser suficiente. No exemplo anterior, a leitura dos resultados da mensagem em 100.000 elementos XML estão sendo armazenados em buffer, o que pode não ser desejável. Talvez você queira substituir <xref:System.ServiceModel.Channels.Message.OnGetReaderAtBodyContents> para retornar uma <xref:System.Xml.XmlDictionaryReader> classe derivada personalizada que atenda a números aleatórios. Em seguida, você pode substituir <xref:System.ServiceModel.Channels.Message.OnWriteBodyContents%2A> para usar o leitor que o <xref:System.ServiceModel.Channels.Message.OnGetReaderAtBodyContents> método retorna, como mostrado no exemplo a seguir.  
  
 [!code-csharp[C_UsingTheMessageClass#10](../../../../samples/snippets/csharp/VS_Snippets_CFX/c_usingthemessageclass/cs/source.cs#10)]
 [!code-vb[C_UsingTheMessageClass#10](../../../../samples/snippets/visualbasic/VS_Snippets_CFX/c_usingthemessageclass/vb/source.vb#10)]  
  
 Da mesma forma, talvez você queira substituir `OnCreateBufferedCopy` para retornar sua própria `MessageBuffer` classe derivada.  
  
 Além de fornecer o conteúdo do corpo da mensagem, sua classe derivada de mensagem também deve substituir as `Version` `Headers` Propriedades, e `Properties` .  
  
 Observe que, se você criar uma cópia de uma mensagem, a cópia usará os cabeçalhos de mensagem do original.  
  
### <a name="other-members-that-can-be-overridden"></a>Outros membros que podem ser substituídos  
 Você pode substituir os <xref:System.ServiceModel.Channels.Message.OnWriteStartEnvelope%2A> <xref:System.ServiceModel.Channels.Message.OnWriteStartHeaders%2A> métodos, e <xref:System.ServiceModel.Channels.Message.OnWriteStartBody%2A> para especificar como as marcas de início de envelope SOAP, cabeçalhos SOAP e elemento de corpo SOAP são gravadas. Normalmente, eles correspondem a `<soap:Envelope>` , `<soap:Header>` , e `<soap:Body>` . Esses métodos normalmente não devem gravar nada se a <xref:System.ServiceModel.Channels.Message.Version> Propriedade retornar <xref:System.ServiceModel.Channels.MessageVersion.None> .  
  
> [!NOTE]
> A implementação padrão de `OnGetReaderAtBodyContents` chamadas `OnWriteStartEnvelope` e `OnWriteStartBody` antes `OnWriteBodyContents` de chamar e armazenar em buffer os resultados. Os cabeçalhos não são gravados.  
  
 Substitua o <xref:System.ServiceModel.Channels.Message.OnWriteMessage%2A> método para alterar a forma como a mensagem inteira é construída a partir de suas várias partes. O `OnWriteMessage` método é chamado de <xref:System.ServiceModel.Channels.Message.WriteMessage%2A> e da implementação padrão <xref:System.ServiceModel.Channels.Message.OnCreateBufferedCopy%2A> . Observe que a substituição <xref:System.ServiceModel.Channels.Message.WriteMessage%2A> não é uma prática recomendada. É melhor substituir os `On` métodos apropriados (por exemplo,, <xref:System.ServiceModel.Channels.Message.OnWriteStartEnvelope%2A> <xref:System.ServiceModel.Channels.Message.OnWriteStartHeaders%2A> e <xref:System.ServiceModel.Channels.BodyWriter.OnWriteBodyContents%2A> .  
  
 Substitua <xref:System.ServiceModel.Channels.Message.OnBodyToString%2A> para substituir como o corpo da mensagem é representado durante a depuração. O padrão é representá-lo como três pontos ("..."). Observe que esse método pode ser chamado várias vezes quando o estado da mensagem é algo diferente de Closed. Uma implementação desse método nunca deve causar nenhuma ação que deve ser executada apenas uma vez (como a leitura de um fluxo de somente encaminhamento).  
  
 Substitua o <xref:System.ServiceModel.Channels.Message.OnGetBodyAttribute%2A> método para permitir o acesso a atributos no elemento de corpo SOAP. Esse método pode ser chamado quantas vezes, mas o `Message` tipo base garante que ele seja chamado apenas quando a mensagem estiver no estado criado. Não é necessário verificar o estado em uma implementação. A implementação padrão sempre retorna `null` , o que indica que não há atributos no elemento body.  
  
 Se o `Message` objeto precisar fazer qualquer limpeza especial quando o corpo da mensagem não for mais necessário, você poderá substituir <xref:System.ServiceModel.Channels.Message.OnClose%2A> . A implementação padrão não faz nada.  
  
 As `IsEmpty` `IsFault` Propriedades e podem ser substituídas. Por padrão, ambos retornam `false` .
