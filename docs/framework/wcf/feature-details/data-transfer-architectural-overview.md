---
title: Visão geral da arquitetura de transferência de dados
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
helpviewer_keywords:
- data transfer [WCF], architectural overview
ms.assetid: 343c2ca2-af53-4936-a28c-c186b3524ee9
ms.openlocfilehash: f34bf82ec44140827c5d8da59911afe10ab7a853
ms.sourcegitcommit: cdb295dd1db589ce5169ac9ff096f01fd0c2da9d
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/09/2020
ms.locfileid: "84576467"
---
# <a name="data-transfer-architectural-overview"></a>Visão geral da arquitetura de transferência de dados
O Windows Communication Foundation (WCF) pode ser considerado uma infraestrutura de mensagens. Ele pode receber mensagens, processá-las e enviá-las ao código do usuário para uma ação adicional ou pode construir mensagens de dados fornecidos pelo código do usuário e fornecê-las a um destino. Este tópico, destinado a desenvolvedores avançados, descreve a arquitetura para lidar com mensagens e os dados contidos. Para uma exibição mais simples e orientada a tarefas de como enviar e receber dados, consulte [especificando transferência de dados em contratos de serviço](specifying-data-transfer-in-service-contracts.md).  
  
> [!NOTE]
> Este tópico discute os detalhes de implementação do WCF que não são visíveis examinando o modelo de objeto do WCF. Duas palavras de cuidado estão em ordem com relação aos detalhes de implementação documentados. Primeiro, as descrições são simplificadas; a implementação real pode ser mais complexa devido a otimizações ou outros motivos. Em segundo lugar, você nunca deve depender de detalhes de implementação específicos, até mesmo documentados, pois eles podem ser alterados sem aviso de versão para versão ou até mesmo em uma versão de serviço.  
  
## <a name="basic-architecture"></a>Arquitetura básica  
 No núcleo de recursos de manipulação de mensagens do WCF está a <xref:System.ServiceModel.Channels.Message> classe, que é descrita em detalhes no [uso da classe Message](using-the-message-class.md). Os componentes de tempo de execução do WCF podem ser divididos em duas partes principais: a pilha de canais e a estrutura de serviço, com a <xref:System.ServiceModel.Channels.Message> classe sendo o ponto de conexão.  
  
 A pilha de canais é responsável pela conversão entre uma <xref:System.ServiceModel.Channels.Message> instância válida e alguma ação que corresponda ao envio ou recebimento de dados da mensagem. No lado de envio, a pilha de canais usa uma <xref:System.ServiceModel.Channels.Message> instância válida e, após algum processamento, executa alguma ação que corresponde logicamente ao envio da mensagem. A ação pode estar enviando pacotes TCP ou HTTP, enfileirando a mensagem no enfileiramento de mensagens, gravando a mensagem em um banco de dados, salvando-a em um compartilhamento de arquivos ou qualquer outra ação, dependendo da implementação. A ação mais comum é enviar a mensagem por um protocolo de rede. No lado do recebimento, o oposto acontece — uma ação é detectada (que pode ser pacotes TCP ou HTTP chegando ou qualquer outra ação) e, após o processamento, a pilha de canais Converte essa ação em uma <xref:System.ServiceModel.Channels.Message> instância válida.  
  
 Você pode usar o WCF usando a <xref:System.ServiceModel.Channels.Message> classe e a pilha de canais diretamente. No entanto, isso é difícil e demorado. Além disso, o <xref:System.ServiceModel.Channels.Message> objeto não fornece suporte a metadados, portanto, você não poderá gerar clientes WCF com rigidez de tipos se usar o WCF dessa maneira.  
  
 Portanto, o WCF inclui uma estrutura de serviço que fornece um modelo de programação fácil de usar que você pode usar para construir e receber `Message` objetos. O Service Framework mapeia os serviços para .NET Framework tipos por meio da noção de contratos de serviço e envia mensagens para as operações do usuário que são simplesmente .NET Framework métodos marcados com o <xref:System.ServiceModel.OperationContractAttribute> atributo (para obter mais detalhes, consulte [Criando contratos de serviço](../designing-service-contracts.md)). Esses métodos podem ter parâmetros e valores de retorno. No lado do serviço, a estrutura de serviço converte as <xref:System.ServiceModel.Channels.Message> instâncias de entrada em parâmetros e converte valores de retorno em instâncias de saída <xref:System.ServiceModel.Channels.Message> . No lado do cliente, ele faz o oposto. Por exemplo, considere a `FindAirfare` operação abaixo.  
  
 [!code-csharp[c_DataArchitecture#1](../../../../samples/snippets/csharp/VS_Snippets_CFX/c_dataarchitecture/cs/source.cs#1)]
 [!code-vb[c_DataArchitecture#1](../../../../samples/snippets/visualbasic/VS_Snippets_CFX/c_dataarchitecture/vb/source.vb#1)]  
  
 Suponha que `FindAirfare` é chamado no cliente. A estrutura de serviço no cliente converte os `FromCity` `ToCity` parâmetros e em uma instância de saída <xref:System.ServiceModel.Channels.Message> e passa-o para a pilha de canais a ser enviada.  
  
 No lado do serviço, quando uma <xref:System.ServiceModel.Channels.Message> instância chega da pilha de canais, a estrutura de serviço extrai os dados relevantes da mensagem para popular os `FromCity` parâmetros e `ToCity` e, em seguida, chama o método do lado do serviço `FindAirfare` . Quando o método retorna, a estrutura de serviço usa o valor inteiro retornado e o `IsDirectFlight` parâmetro de saída e cria uma <xref:System.ServiceModel.Channels.Message> instância de objeto que contém essas informações. Em seguida, ele passa a `Message` instância para a pilha de canais a ser enviada de volta ao cliente.  
  
 No lado do cliente, uma <xref:System.ServiceModel.Channels.Message> instância que contém a mensagem de resposta surge da pilha de canais. A estrutura de serviço extrai o valor de retorno e o `IsDirectFlight` valor e retorna-os para o chamador do cliente.  
  
## <a name="message-class"></a>Classe de mensagens  
 A <xref:System.ServiceModel.Channels.Message> classe deve ser uma representação abstrata de uma mensagem, mas seu design está fortemente ligado à mensagem SOAP. Um <xref:System.ServiceModel.Channels.Message> contém três informações principais: um corpo de mensagem, cabeçalhos de mensagem e propriedades de mensagem.  
  
## <a name="message-body"></a>Corpo da mensagem  
 O corpo da mensagem destina-se a representar a carga de dados real da mensagem. O corpo da mensagem é sempre representado como um XML infoset. Isso não significa que todas as mensagens criadas ou recebidas no WCF devem estar no formato XML. Cabe à pilha de canais decidir como interpretar o corpo da mensagem. Ele pode emitir como XML, convertê-lo em algum outro formato ou até omiti-lo inteiramente. É claro que, com a maioria das associações fornecidas pelo WCF, o corpo da mensagem é representado como conteúdo XML na seção corpo de um envelope SOAP.  
  
 É importante perceber que a classe não `Message` contém necessariamente um buffer com dados XML que representam o corpo. Logicamente, `Message` contém um XML infoset, mas esse infoset pode ser construído dinamicamente e talvez nunca exista fisicamente na memória.  
  
### <a name="putting-data-into-the-message-body"></a>Colocando dados no corpo da mensagem  
 Não há um mecanismo uniforme para colocar dados em um corpo de mensagem. A <xref:System.ServiceModel.Channels.Message> classe tem um método abstrato, <xref:System.ServiceModel.Channels.Message.OnWriteBodyContents%28System.Xml.XmlDictionaryWriter%29> , que usa um <xref:System.Xml.XmlDictionaryWriter> . Cada subclasse da <xref:System.ServiceModel.Channels.Message> classe é responsável por substituir esse método e escrever seu próprio conteúdo. O corpo da mensagem contém logicamente o XML infoset que `OnWriteBodyContent` produz. Por exemplo, considere a seguinte `Message` subclasse.  
  
 [!code-csharp[c_DataArchitecture#2](../../../../samples/snippets/csharp/VS_Snippets_CFX/c_dataarchitecture/cs/source.cs#2)]
 [!code-vb[c_DataArchitecture#2](../../../../samples/snippets/visualbasic/VS_Snippets_CFX/c_dataarchitecture/vb/source.vb#2)]  
  
 Fisicamente, uma `AirfareRequestMessage` instância contém apenas duas cadeias de caracteres ("fromCity" e "tocity"). No entanto, logicamente a mensagem contém o XML infoset a seguir:  
  
```xml  
<airfareRequest>  
    <from>Tokyo</from>  
    <to>London</to>  
</airfareRequest>  
```  
  
 É claro que você normalmente não criaria mensagens dessa maneira, pois você pode usar a estrutura de serviço para criar uma mensagem como a anterior dos parâmetros de contrato de operação. Além disso, a <xref:System.ServiceModel.Channels.Message> classe tem `CreateMessage` métodos estáticos que você pode usar para criar mensagens com tipos comuns de conteúdo: uma mensagem vazia, uma mensagem que contém um objeto SERIALIZADO em XML com o <xref:System.Runtime.Serialization.DataContractSerializer> , uma mensagem que contém uma falha de SOAP, uma mensagem que contém o XML representado por um <xref:System.Xml.XmlReader> e assim por diante.  
  
### <a name="getting-data-from-a-message-body"></a>Obtendo dados de um corpo de mensagem  
 Você pode extrair os dados armazenados em um corpo de mensagem de duas maneiras principais:  
  
- Você pode obter todo o corpo da mensagem de uma só vez chamando o <xref:System.ServiceModel.Channels.Message.WriteBodyContents%28System.Xml.XmlDictionaryWriter%29> método e passando um gravador de XML. O corpo completo da mensagem é escrito para este gravador. Obter todo o corpo da mensagem ao mesmo tempo também é chamado *de gravação de uma mensagem*. A gravação é feita principalmente pela pilha de canais ao enviar mensagens – alguma parte da pilha de canais geralmente terá acesso a todo o corpo da mensagem, codificá-la e enviá-la.  
  
- Outra maneira de obter informações do corpo da mensagem é chamar <xref:System.ServiceModel.Channels.Message.GetReaderAtBodyContents> e obter um leitor XML. O corpo da mensagem pode então ser acessado sequencialmente, conforme necessário, chamando métodos no leitor. Obter o corpo da mensagem por parte também é chamado *de leitura de uma mensagem*. A leitura da mensagem é usada principalmente pela estrutura de serviço ao receber mensagens. Por exemplo, quando o <xref:System.Runtime.Serialization.DataContractSerializer> estiver em uso, a estrutura de serviço obterá um leitor XML sobre o corpo e o passará para o mecanismo de desserialização, que começará a ler o elemento de mensagem por elemento e construir o grafo de objeto correspondente.  
  
 Um corpo de mensagem pode ser recuperado apenas uma vez. Isso torna possível trabalhar com fluxos de somente encaminhamento. Por exemplo, você pode escrever uma <xref:System.ServiceModel.Channels.Message.OnWriteBodyContents%28System.Xml.XmlDictionaryWriter%29> substituição que lê de a <xref:System.IO.FileStream> e retorna os resultados como um XML infoset. Você nunca precisará "retroceder" para o início do arquivo.  
  
 Os `WriteBodyContents` `GetReaderAtBodyContents` métodos e simplesmente verificam se o corpo da mensagem nunca foi recuperado antes e, em seguida, chama `OnWriteBodyContents` ou `OnGetReaderAtBodyContents` , respectivamente.  
  
## <a name="message-usage-in-wcf"></a>Uso da mensagem no WCF  
 A maioria das mensagens pode ser classificada como *saída* (aquelas que são criadas pela estrutura de serviço a ser enviada pela pilha de canais) ou *recebidas* (aquelas que chegam da pilha de canais e são interpretadas pela estrutura de serviço). Além disso, a pilha de canais pode operar em modo de streaming ou em buffer. A estrutura de serviço também pode expor um modelo de programação em fluxo ou não-fluxo. Isso leva aos casos listados na tabela a seguir, juntamente com os detalhes simplificados de sua implementação.  
  
|Tipo de mensagem|Dados do corpo na mensagem|Implementação de gravação (OnWriteBodyContents)|Ler (OnGetReaderAtBodyContents) implementação|  
|------------------|--------------------------|--------------------------------------------------|-------------------------------------------------------|  
|Saída, criada a partir do modelo de programação não transmitido|Os dados necessários para gravar a mensagem (por exemplo, um objeto e a <xref:System.Runtime.Serialization.DataContractSerializer> instância necessária para serializá-la) *|Lógica personalizada para gravar a mensagem com base nos dados armazenados (por exemplo, chamar `WriteObject` em `DataContractSerializer` se esse for o serializador em uso) *|Chamar `OnWriteBodyContents` , armazenar em buffer os resultados, retornar um leitor de XML sobre o buffer|  
|Saída, criada a partir do modelo de programação em fluxo|O `Stream` com os dados a serem gravados *|Gravar dados do fluxo armazenado usando o <xref:System.Xml.IStreamProvider> mecanismo *|Chamar `OnWriteBodyContents` , armazenar em buffer os resultados, retornar um leitor de XML sobre o buffer|  
|Entrada da pilha de canais de streaming|Um `Stream` objeto que representa os dados provenientes da rede com um <xref:System.Xml.XmlReader> sobre ele|Gravar o conteúdo do armazenado `XmlReader` usando`WriteNode`|Retorna o armazenado`XmlReader`|  
|Entrada da pilha de canais não streaming|Um buffer que contém dados de corpo com um `XmlReader` sobre ele|Grava o conteúdo do armazenado `XmlReader` usando`WriteNode`|Retorna o idioma armazenado|  
  
 \*Esses itens não são implementados diretamente em `Message` subclasses, mas em subclasses da <xref:System.ServiceModel.Channels.BodyWriter> classe. Para obter mais informações sobre o <xref:System.ServiceModel.Channels.BodyWriter> , consulte [usando a classe Message](using-the-message-class.md).  
  
## <a name="message-headers"></a>Cabeçalhos da mensagem  
 Uma mensagem pode conter cabeçalhos. Um cabeçalho consiste logicamente em um XML infoset associado a um nome, um namespace e algumas outras propriedades. Os cabeçalhos de mensagem são acessados usando a `Headers` propriedade em <xref:System.ServiceModel.Channels.Message> . Cada cabeçalho é representado por uma <xref:System.ServiceModel.Channels.MessageHeader> classe. Normalmente, os cabeçalhos de mensagens são mapeados para cabeçalhos de mensagens SOAP ao usar uma pilha de canais configurada para trabalhar com mensagens SOAP.  
  
 Colocar informações em um cabeçalho de mensagem e extrair informações dele é semelhante ao uso do corpo da mensagem. O processo é um pouco simplificado porque não há suporte para streaming. É possível acessar o conteúdo do mesmo cabeçalho mais de uma vez, e os cabeçalhos podem ser acessados em ordem arbitrária, forçando os cabeçalhos a serem sempre armazenados em buffer. Não há nenhum mecanismo de finalidade geral disponível para obter um leitor XML em um cabeçalho, mas há uma `MessageHeader` subclasse interna ao WCF que representa um cabeçalho legível com tal funcionalidade. Esse tipo de `MessageHeader` é criado pela pilha de canais quando uma mensagem com cabeçalhos de aplicativo personalizados chega. Isso permite que a estrutura de serviço use um mecanismo de desserialização, como o <xref:System.Runtime.Serialization.DataContractSerializer> , para interpretar esses cabeçalhos.  
  
 Para obter mais informações, consulte [usando a classe Message](using-the-message-class.md).  
  
## <a name="message-properties"></a>Propriedades da Mensagem  
 Uma mensagem pode conter Propriedades. Uma *Propriedade* é qualquer .NET Framework objeto associado a um nome de cadeia de caracteres. As propriedades são acessadas por meio da `Properties` propriedade em `Message` .  
  
 Ao contrário do corpo da mensagem e dos cabeçalhos de mensagem (que normalmente mapeiam para o corpo SOAP e cabeçalhos SOAP, respectivamente), as propriedades de mensagem normalmente não são enviadas nem recebidas junto com as mensagens. As propriedades de mensagem existem principalmente como um mecanismo de comunicação para passar dados sobre a mensagem entre os vários canais na pilha de canais e entre a pilha de canais e o modelo de serviço.  
  
 Por exemplo, o canal de transporte HTTP incluído como parte do WCF é capaz de produzir vários códigos de status HTTP, como "404 (não encontrado)" e "500 (erro interno do servidor)", quando ele envia respostas aos clientes. Antes de enviar uma mensagem de resposta, ele verifica se o `Properties` de `Message` contém uma propriedade chamada "HttpResponse" que contém um objeto do tipo <xref:System.ServiceModel.Channels.HttpResponseMessageProperty> . Se essa propriedade for encontrada, ela examinará a <xref:System.ServiceModel.Channels.HttpResponseMessageProperty.StatusCode%2A> propriedade e usará esse código de status. Se não for encontrado, o código padrão "200 (OK)" será usado.  
  
 Para obter mais informações, consulte [usando a classe Message](using-the-message-class.md).  
  
### <a name="the-message-as-a-whole"></a>A mensagem como um todo  
 Até agora, discutimos métodos para acessar as várias partes da mensagem isoladamente. No entanto, a <xref:System.ServiceModel.Channels.Message> classe também fornece métodos para trabalhar com toda a mensagem como um todo. Por exemplo, o `WriteMessage` método grava toda a mensagem em um gravador de XML.  
  
 Para que isso seja possível, um mapeamento deve ser definido entre a `Message` instância inteira e um XML infoset. Esse mapeamento, na verdade, existe: o WCF usa o padrão SOAP para definir esse mapeamento. Quando uma `Message` instância é gravada como um XML infoset, o infoset resultante é o envelope SOAP válido que contém a mensagem. Portanto, `WriteMessage` normalmente executaria as seguintes etapas:  
  
1. Escreva a marca de abertura do elemento envelope SOAP.  
  
2. Escreva a marca de abertura do elemento de cabeçalho SOAP, escreva todos os cabeçalhos e feche o elemento de cabeçalho.  
  
3. Escreva a marca de abertura do elemento de corpo SOAP.  
  
4. Chamada `WriteBodyContents` ou um método equivalente para gravar o corpo.  
  
5. Feche os elementos corpo e envelope.  
  
 As etapas anteriores estão fortemente ligadas ao padrão SOAP. Isso é complicado pelo fato de que várias versões do SOAP existem, por exemplo, é impossível escrever o elemento SOAP envelope corretamente sem saber a versão SOAP em uso. Além disso, em alguns casos, pode ser desejável desativar completamente esse mapeamento complexo específico de SOAP.  
  
 Para essas finalidades, uma `Version` propriedade é fornecida em `Message` . Ele pode ser definido como a versão SOAP a ser usada ao gravar a mensagem ou pode ser definido como `None` para evitar mapeamentos específicos de SOAP. Se a `Version` propriedade for definida como `None` , os métodos que funcionam com a mensagem inteira agirão como se a mensagem consistisse apenas em seu corpo, por exemplo, `WriteMessage` simplesmente chamaria `WriteBodyContents` em vez de executar as várias etapas listadas acima. Espera-se que as mensagens de entrada `Version` sejam detectadas automaticamente e definidas corretamente.  
  
## <a name="the-channel-stack"></a>A pilha de canais  
  
### <a name="channels"></a>Canais  
 Conforme mencionado anteriormente, a pilha de canais é responsável pela conversão de <xref:System.ServiceModel.Channels.Message> instâncias de saída em alguma ação (como o envio de pacotes pela rede) ou na conversão de alguma ação (como o recebimento de pacotes de rede) em instâncias de entrada `Message` .  
  
 A pilha de canais é composta por um ou mais canais ordenados em uma sequência. Uma instância de saída `Message` é passada para o primeiro canal na pilha (também chamado de *canal superior*), que a passa para o próximo canal para baixo na pilha e assim por diante. A mensagem é encerrada no último canal, que é chamado de *canal de transporte*. As mensagens de entrada originam-se no canal de transporte e são passadas do canal para o canal acima da pilha. Do canal superior, a mensagem é geralmente passada para a estrutura de serviço. Embora esse seja o padrão usual para mensagens de aplicativo, alguns canais podem funcionar de forma ligeiramente diferente, por exemplo, eles podem enviar suas próprias mensagens de infraestrutura sem passar uma mensagem de um canal acima.  
  
 Os canais podem operar na mensagem de várias maneiras à medida que passam pela pilha. A operação mais comum é adicionar um cabeçalho a uma mensagem de saída e ler cabeçalhos em uma mensagem de entrada. Por exemplo, um canal pode computar a assinatura digital de uma mensagem e adicioná-la como um cabeçalho. Um canal também pode inspecionar esse cabeçalho de assinatura digital em mensagens de entrada e bloquear mensagens que não têm uma assinatura válida desde o seu sentido até a pilha de canais. Os canais também costumam definir ou inspecionar as propriedades da mensagem. O corpo da mensagem geralmente não é modificado, embora isso seja permitido, por exemplo, o canal de segurança do WCF pode criptografar o corpo da mensagem.  
  
### <a name="transport-channels-and-message-encoders"></a>Canais de transporte e codificadores de mensagens  
 O canal na extremidade inferior na pilha é responsável por realmente transformar um saída <xref:System.ServiceModel.Channels.Message> , conforme modificado por outros canais, em alguma ação. No lado do recebimento, esse é o canal que converte alguma ação em um `Message` processo que os outros canais processam.  
  
 Conforme mencionado anteriormente, as ações podem ser variadas: enviar ou receber pacotes de rede em vários protocolos, ler ou gravar a mensagem em um banco de dados, ou enfileirar ou enfileirar a mensagem em uma fila de enfileiramento de mensagens, para fornecer, mas alguns exemplos. Todas essas ações têm uma coisa em comum: elas exigem uma transformação entre a instância do WCF `Message` e um grupo real de bytes que podem ser enviados, recebidos, lidos, gravados, colocados na fila ou removidas da fila. O processo de converter um `Message` em um grupo de bytes é chamado de *codificação*e o processo reverso de criar um `Message` de um grupo de bytes é chamado *decodificação*.  
  
 A maioria dos canais de transporte usa componentes chamados *codificadores de mensagens* para realizar o trabalho de codificação e decodificação. Um codificador de mensagem é uma subclasse da <xref:System.ServiceModel.Channels.MessageEncoder> classe. `MessageEncoder`inclui várias `ReadMessage` `WriteMessage` sobrecargas de método para converter entre `Message` e grupos de bytes.  
  
 No lado de envio, um canal de transporte de buffer passa o `Message` objeto que ele recebeu de um canal acima dele para `WriteMessage` . Ele retorna uma matriz de bytes, que ele usa para executar sua ação (como empacotar esses bytes como pacotes TCP válidos e enviá-los para o destino correto). Um canal de transporte de streaming primeiro cria um `Stream` (por exemplo, pela conexão TCP de saída) e, em seguida, passa o `Stream` e o `Message` necessário para enviar para a `WriteMessage` sobrecarga apropriada, que grava a mensagem.  
  
 No lado do recebimento, um canal de transporte de buffer extrai os bytes de entrada (por exemplo, de pacotes TCP de entrada) em uma matriz e chama `ReadMessage` para obter um `Message` objeto que ele pode passar mais para cima na pilha de canais. Um canal de transporte de streaming cria um `Stream` objeto (por exemplo, um fluxo de rede pela conexão TCP de entrada) e o passa para `ReadMessage` para obter um `Message` objeto.  
  
 A separação entre os canais de transporte e o codificador de mensagem não é obrigatória; é possível gravar um canal de transporte que não usa um codificador de mensagem. No entanto, a vantagem dessa separação é a facilidade de composição. Desde que um canal de transporte use apenas a base <xref:System.ServiceModel.Channels.MessageEncoder> , ele pode funcionar com qualquer serviço WCF ou codificador de mensagem de terceiros. Da mesma forma, o mesmo codificador normalmente pode ser usado em qualquer canal de transporte.  
  
### <a name="message-encoder-operation"></a>Operação do codificador de mensagem  
 Para descrever a operação típica de um codificador, é útil considerar os quatro casos a seguir.  
  
|Operação|Comentário|  
|---------------|-------------|  
|Codificação, em buffer|No modo de buffer, o codificador normalmente cria um buffer de tamanho variável e, em seguida, cria um gravador de XML sobre ele. Em seguida, ele chama <xref:System.ServiceModel.Channels.Message.WriteMessage%28System.Xml.XmlWriter%29> a mensagem que está sendo codificada, que grava os cabeçalhos e, em seguida, o corpo usando <xref:System.ServiceModel.Channels.Message.WriteBodyContents%28System.Xml.XmlDictionaryWriter%29> , conforme explicado na seção anterior sobre `Message` neste tópico. O conteúdo do buffer (representado como uma matriz de bytes) é retornado pelo canal de transporte a ser usado.|  
|Codificação, em fluxo|No modo de fluxo, a operação é semelhante à anterior, mas mais simples. Não há necessidade de um buffer. Um gravador de XML normalmente é criado no fluxo e <xref:System.ServiceModel.Channels.Message.WriteMessage%28System.Xml.XmlWriter%29> é chamado no `Message` para gravá-lo nesse gravador.|  
|Decodificação, em buffer|Ao decodificar no modo em buffer, uma `Message` subclasse especial que contém os dados armazenados em buffer normalmente é criada. Os cabeçalhos da mensagem são lidos e um leitor de XML posicionado no corpo da mensagem é criado. Este é o leitor que será retornado com <xref:System.ServiceModel.Channels.Message.GetReaderAtBodyContents> .|  
|Decodificação, em fluxo|Ao decodificar no modo de fluxo, uma subclasse de mensagem especial normalmente é criada. O fluxo é avançado apenas o suficiente para ler todos os cabeçalhos e posicioná-los no corpo da mensagem. Um leitor de XML é então criado no fluxo. Este é o leitor que será retornado com <xref:System.ServiceModel.Channels.Message.GetReaderAtBodyContents> .|  
  
 Os codificadores também podem executar outras funções. Por exemplo, os codificadores podem agrupar leitores e gravadores XML. É caro criar um novo leitor ou gravador de XML toda vez que um é necessário. Portanto, os codificadores normalmente mantêm um pool de leitores e um pool de gravadores de tamanho configurável. Nas descrições da operação do codificador descrita anteriormente, sempre que a frase "criar um leitor/gravador XML" for usada, normalmente significa "pegar uma do pool ou criar uma se não houver uma disponível". O codificador (e as `Message` subclasses que ele cria durante a decodificação) contêm a lógica para retornar leitores e gravadores aos pools quando eles não são mais necessários (por exemplo, quando o `Message` é fechado).  
  
 O WCF fornece três codificadores de mensagens, embora seja possível criar tipos personalizados adicionais. Os tipos fornecidos são texto, binário e MTOM (mecanismo de otimização de transmissão de mensagens). Elas são descritas em detalhes na [escolha de um codificador de mensagem](choosing-a-message-encoder.md).  
  
### <a name="the-istreamprovider-interface"></a>A interface IStreamprovider  
 Ao gravar uma mensagem de saída que contém um corpo transmitido para um gravador de XML, o <xref:System.ServiceModel.Channels.Message> usa uma sequência de chamadas semelhantes à seguinte em sua <xref:System.ServiceModel.Channels.Message.OnWriteBodyContents%28System.Xml.XmlDictionaryWriter%29> implementação:  
  
- Escreva todas as informações necessárias que antecedem o fluxo (por exemplo, a marca XML de abertura).  
  
- Grave o fluxo.  
  
- Escreva qualquer informação após o fluxo (por exemplo, a marca XML de fechamento).  
  
 Isso funciona bem com codificações semelhantes à codificação XML textual. No entanto, algumas codificações não colocam informações XML Infoset (por exemplo, marcas de elementos XML iniciais e finais) junto com os dados contidos nos elementos. Por exemplo, na codificação MTOM, a mensagem é dividida em várias partes. Uma parte contém o XML infoset, que pode conter referências a outras partes para o conteúdo real do elemento. O XML infoset é normalmente pequeno em comparação com o conteúdo transmitido, de modo que faz sentido armazenar em buffer o infoset, gravá-lo e, em seguida, gravar o conteúdo de forma transmitida. Isso significa que, na hora em que a marca do elemento de fechamento é gravada, o fluxo ainda não deve ter sido gravado.  
  
 Para essa finalidade, a <xref:System.Xml.IStreamProvider> interface é usada. A interface tem um <xref:System.Xml.IStreamProvider.GetStream> método que retorna o fluxo a ser gravado. A maneira correta de gravar um corpo de mensagem em fluxo no <xref:System.ServiceModel.Channels.Message.OnWriteBodyContents%28System.Xml.XmlDictionaryWriter%29> é a seguinte:  
  
1. Escreva todas as informações necessárias que antecedem o fluxo (por exemplo, a marca XML de abertura).  
  
2. Chame a `WriteValue` sobrecarga no <xref:System.Xml.XmlDictionaryWriter> que usa um <xref:System.Xml.IStreamProvider> , com uma `IStreamProvider` implementação que retorna o fluxo a ser gravado.  
  
3. Escreva qualquer informação após o fluxo (por exemplo, a marca XML de fechamento).  
  
 Com essa abordagem, o gravador de XML tem a opção de quando chamar <xref:System.Xml.IStreamProvider.GetStream> e gravar os dados transmitidos. Por exemplo, os gravadores XML textuais e binários o chamarão imediatamente e gravarão o conteúdo transmitido entre as marcas de início e de fim. O gravador MTOM pode decidir chamar <xref:System.Xml.IStreamProvider.GetStream> mais tarde, quando estiver pronto para gravar a parte apropriada da mensagem.  
  
## <a name="representing-data-in-the-service-framework"></a>Representando dados na estrutura de serviço  
 Conforme indicado na seção "arquitetura básica" deste tópico, a estrutura de serviço é parte do WCF que, entre outras coisas, é responsável pela conversão entre um modelo de programação amigável para dados de mensagens e `Message` instâncias reais. Normalmente, uma troca de mensagens é representada na estrutura de serviço como um método .NET Framework marcado com o <xref:System.ServiceModel.OperationContractAttribute> atributo. O método pode ter alguns parâmetros e pode retornar um valor de retorno ou parâmetros de saída (ou ambos). No lado do serviço, os parâmetros de entrada representam a mensagem de entrada e o valor de retorno e os parâmetros de saída representam a mensagem de saída. No lado do cliente, o inverso é true. O modelo de programação para descrever mensagens usando parâmetros e o valor de retorno é descrito em detalhes na [especificação de transferência de dados em contratos de serviço](specifying-data-transfer-in-service-contracts.md). No entanto, esta seção apresentará uma breve visão geral.  
  
## <a name="programming-models"></a>Modelos de programação  
 O WCF Service Framework dá suporte a cinco modelos de programação diferentes para descrever mensagens:  
  
### <a name="1-the-empty-message"></a>1. a mensagem vazia  
 Esse é o caso mais simples. Para descrever uma mensagem de entrada vazia, não use nenhum parâmetro de entrada.  
  
 [!code-csharp[C_DataArchitecture#3](../../../../samples/snippets/csharp/VS_Snippets_CFX/c_dataarchitecture/cs/source.cs#3)]
 [!code-vb[C_DataArchitecture#3](../../../../samples/snippets/visualbasic/VS_Snippets_CFX/c_dataarchitecture/vb/source.vb#3)]  
  
 Para descrever uma mensagem de saída vazia, use um valor de retorno nulo e não use nenhum parâmetro de saída:  
  
 [!code-csharp[C_DataArchitecture#4](../../../../samples/snippets/csharp/VS_Snippets_CFX/c_dataarchitecture/cs/source.cs#4)]
 [!code-vb[C_DataArchitecture#4](../../../../samples/snippets/visualbasic/VS_Snippets_CFX/c_dataarchitecture/vb/source.vb#4)]  
  
 Observe que isso é diferente de um contrato de operação unidirecional:  
  
 [!code-csharp[C_DataArchitecture#5](../../../../samples/snippets/csharp/VS_Snippets_CFX/c_dataarchitecture/cs/source.cs#5)]
 [!code-vb[C_DataArchitecture#5](../../../../samples/snippets/visualbasic/VS_Snippets_CFX/c_dataarchitecture/vb/source.vb#5)]  
  
 No `SetDesiredTemperature` exemplo, um padrão de troca de mensagens bidirecional é descrito. Uma mensagem é retornada da operação, mas está vazia. É possível retornar uma falha da operação. No exemplo "definir lâmpada", o padrão de troca de mensagens é unidirecional, portanto, não há nenhuma mensagem de saída a ser descrita. O serviço não pode comunicar nenhum status de volta para o cliente nesse caso.  
  
### <a name="2-using-the-message-class-directly"></a>2. usando a classe Message diretamente  
 É possível usar a <xref:System.ServiceModel.Channels.Message> classe (ou uma de suas subclasses) diretamente em um contrato de operação. Nesse caso, a estrutura de serviço apenas passa o `Message` da operação para a pilha de canais e vice-versa, sem nenhum processamento adicional.  
  
 Há dois casos de uso principais para uso `Message` direto. Você pode usar isso para cenários avançados, quando nenhum dos outros modelos de programação fornece flexibilidade suficiente para descrever sua mensagem. Por exemplo, talvez você queira usar arquivos em disco para descrever uma mensagem, com as propriedades do arquivo se tornando cabeçalhos de mensagens e o conteúdo do arquivo se tornando o corpo da mensagem. Em seguida, você pode criar algo semelhante ao seguinte.  
  
 [!code-csharp[C_DataArchitecture#6](../../../../samples/snippets/csharp/VS_Snippets_CFX/c_dataarchitecture/cs/source.cs#6)]
 [!code-vb[C_DataArchitecture#6](../../../../samples/snippets/visualbasic/VS_Snippets_CFX/c_dataarchitecture/vb/source.vb#6)]  
  
 O segundo uso comum para o `Message` em um contrato de operação é quando um serviço não se preocupa com o conteúdo da mensagem específica e age na mensagem como em uma caixa preta. Por exemplo, você pode ter um serviço que encaminhe mensagens para vários outros destinatários. O contrato pode ser escrito da seguinte maneira.  
  
 [!code-csharp[C_DataArchitecture#7](../../../../samples/snippets/csharp/VS_Snippets_CFX/c_dataarchitecture/cs/source.cs#7)]
 [!code-vb[C_DataArchitecture#7](../../../../samples/snippets/visualbasic/VS_Snippets_CFX/c_dataarchitecture/vb/source.vb#7)]  
  
 A linha Action = "*" desativa efetivamente a distribuição de mensagens e garante que todas as mensagens enviadas para o `IForwardingService` contrato façam seu caminho para a `ForwardMessage` operação. (Normalmente, o Dispatcher examinaria o cabeçalho "Action" da mensagem para determinar a qual operação ela se destina. Action = " \* " significa "todos os valores possíveis do cabeçalho de ação".) A combinação de Action = " \* " e o uso da mensagem como um parâmetro é conhecida como "contrato universal" porque ele é capaz de receber todas as mensagens possíveis. Para poder enviar todas as mensagens possíveis, use a mensagem como o valor de retorno e defina `ReplyAction` como " \* ". Isso impedirá que a estrutura de serviço adicione seu próprio cabeçalho de ação, permitindo que você controle esse cabeçalho usando o `Message` objeto retornado.  
  
### <a name="3-message-contracts"></a>3. contratos de mensagem  
 O WCF fornece um modelo de programação declarativa para descrever mensagens, chamadas de *contratos de mensagem*. Esse modelo é descrito em detalhes no [uso de contratos de mensagem](using-message-contracts.md). Essencialmente, toda a mensagem é representada por um único tipo de .NET Framework que usa atributos como o <xref:System.ServiceModel.MessageBodyMemberAttribute> e <xref:System.ServiceModel.MessageHeaderAttribute> para descrever quais partes da classe de contrato de mensagem devem ser mapeadas para qual parte da mensagem.  
  
 Os contratos de mensagem fornecem uma grande quantidade de controle sobre as instâncias resultantes `Message` (embora obviamente, não tanto o controle quanto o uso da `Message` classe diretamente). Por exemplo, os corpos de mensagens são, muitas vezes, compostos por várias partes de informações, cada uma representada por seu próprio elemento XML. Esses elementos podem ocorrer diretamente no corpo (modo*simples* ) ou podem ser *encapsulados* em um elemento XML de abrangeção. O uso do modelo de programação de contrato de mensagem permite que você faça a decisão em oposição ao encapsulamento e controle o nome do nome do wrapper e o namespace.  
  
 O exemplo de código a seguir de um contrato de mensagem demonstra esses recursos.  
  
 [!code-csharp[C_DataArchitecture#9](../../../../samples/snippets/csharp/VS_Snippets_CFX/c_dataarchitecture/cs/source.cs#9)]
 [!code-vb[C_DataArchitecture#9](../../../../samples/snippets/visualbasic/VS_Snippets_CFX/c_dataarchitecture/vb/source.vb#9)]  
  
 Os itens marcados para serem serializados (com o <xref:System.ServiceModel.MessageBodyMemberAttribute> , <xref:System.ServiceModel.MessageHeaderAttribute> ou outros atributos relacionados) devem ser serializáveis para participar de um contrato de mensagem. Para obter mais informações, consulte a seção "serialização" mais adiante neste tópico.  
  
### <a name="4-parameters"></a>4. parâmetros  
 Muitas vezes, um desenvolvedor que deseja descrever uma operação que atue em várias partes de dados não precisa do grau de controle que os contratos de mensagem fornecem. Por exemplo, ao criar novos serviços, geralmente não convém fazer a decisão em oposição ao contrário e decidir sobre o nome do elemento do wrapper. A tomada dessas decisões geralmente exige um profundo conhecimento dos serviços Web e do SOAP.  
  
 O WCF Service Framework pode escolher automaticamente a melhor e mais interoperável a representação de SOAP para enviar ou receber várias partes relacionadas de informações, sem forçar essas opções no usuário. Isso é feito simplesmente descrevendo essas informações como parâmetros ou valores de retorno de um contrato de operação. Por exemplo, considere o contrato de operação a seguir.  
  
 [!code-csharp[C_DataArchitecture#11](../../../../samples/snippets/csharp/VS_Snippets_CFX/c_dataarchitecture/cs/source.cs#11)]
 [!code-vb[C_DataArchitecture#11](../../../../samples/snippets/visualbasic/VS_Snippets_CFX/c_dataarchitecture/vb/source.vb#11)]  
  
 A estrutura de serviço decide automaticamente colocar todas as três informações ( `customerID` , `item` e `quantity` ) no corpo da mensagem e encapsulá-las em um elemento wrapper chamado `SubmitOrderRequest` .  
  
 Descrever as informações a serem enviadas ou recebidas como uma lista simples de parâmetros de contrato de operação é a abordagem recomendada, a menos que haja razões especiais para mover para o contrato de mensagem mais complexo ou `Message` modelos de programação baseados em.  
  
### <a name="5-stream"></a>5. fluxo  
 Usar `Stream` o ou uma de suas subclasses em um contrato de operação ou como uma parte de corpo de mensagem exclusiva em um contrato de mensagem pode ser considerado um modelo de programação separado dos descritos acima. Usar `Stream` dessa forma é a única maneira de garantir que seu contrato será utilizável de forma transmitida, sem escrever sua própria subclasse compatível com streaming `Message` . Para obter mais informações, consulte [grandes dados e streaming](large-data-and-streaming.md).  
  
 Quando `Stream` ou uma de suas subclasses é usada dessa forma, o serializador não é invocado. Para mensagens de saída, uma subclasse de streaming especial `Message` é criada e o fluxo é gravado conforme descrito na seção na <xref:System.Xml.IStreamProvider> interface. Para mensagens de entrada, a estrutura de serviço cria uma `Stream` subclasse sobre a mensagem de entrada e a fornece à operação.  
  
## <a name="programming-model-restrictions"></a>Restrições do Modelo de Programação  
 Os modelos de programação descritos acima não podem ser combinados arbitrariamente. Por exemplo, se uma operação aceitar um tipo de contrato de mensagem, o contrato de mensagem deverá ser seu único parâmetro de entrada. Além disso, a operação deve retornar uma mensagem vazia (tipo de retorno de void) ou outro contrato de mensagem. Essas restrições de modelo de programação são descritas nos tópicos para cada modelo de programação específico: [usando contratos de mensagem](using-message-contracts.md), [usando a classe de mensagem](using-the-message-class.md), e [grandes dados e streaming](large-data-and-streaming.md).  
  
## <a name="message-formatters"></a>Formatadores de mensagem  
 Os modelos de programação descritos acima têm suporte ao conectar componentes chamados de *formatadores de mensagem* na estrutura de serviço. Os formatadores de mensagem são tipos que implementam a <xref:System.ServiceModel.Dispatcher.IClientMessageFormatter> <xref:System.ServiceModel.Dispatcher.IDispatchMessageFormatter> interface ou, ou ambos, para uso em clientes e clientes WCF de serviço, respectivamente.  
  
 Os formatadores de mensagem normalmente são conectados por comportamentos. Por exemplo, o <xref:System.ServiceModel.Description.DataContractSerializerOperationBehavior> plug-in do formatador de mensagem de contrato de dados. Isso é feito no lado do serviço definindo <xref:System.ServiceModel.Dispatcher.DispatchOperation.Formatter%2A> para o formatador correto no <xref:System.ServiceModel.Description.IOperationBehavior.ApplyDispatchBehavior%28System.ServiceModel.Description.OperationDescription%2CSystem.ServiceModel.Dispatcher.DispatchOperation%29> método ou no lado do cliente definindo <xref:System.ServiceModel.Dispatcher.ClientOperation.Formatter%2A> para o formatador correto no <xref:System.ServiceModel.Description.IOperationBehavior.ApplyClientBehavior%28System.ServiceModel.Description.OperationDescription%2CSystem.ServiceModel.Dispatcher.ClientOperation%29> método.  
  
 As tabelas a seguir listam os métodos que um formatador de mensagem pode implementar.  
  
|Interface|Método|Ação|  
|---------------|------------|------------|  
|<xref:System.ServiceModel.Dispatcher.IDispatchMessageFormatter>|<xref:System.ServiceModel.Dispatcher.IDispatchMessageFormatter.DeserializeRequest%28System.ServiceModel.Channels.Message%2CSystem.Object%5B%5D%29>|Converte uma entrada `Message` para parâmetros de operação|  
|<xref:System.ServiceModel.Dispatcher.IDispatchMessageFormatter>|<xref:System.ServiceModel.Dispatcher.IDispatchMessageFormatter.SerializeReply%28System.ServiceModel.Channels.MessageVersion%2CSystem.Object%5B%5D%2CSystem.Object%29>|Cria uma saída `Message` de parâmetros de retorno/saída de operação|  
|<xref:System.ServiceModel.Dispatcher.IClientMessageFormatter>|<xref:System.ServiceModel.Dispatcher.IClientMessageFormatter.SerializeRequest%28System.ServiceModel.Channels.MessageVersion%2CSystem.Object%5B%5D%29>|Cria um saída `Message` de parâmetros de operação|  
|<xref:System.ServiceModel.Dispatcher.IClientMessageFormatter>|<xref:System.ServiceModel.Dispatcher.IClientMessageFormatter.DeserializeReply%28System.ServiceModel.Channels.Message%2CSystem.Object%5B%5D%29>|Converte uma entrada `Message` em parâmetros de valor/saída de retorno|  
  
## <a name="serialization"></a>Serialização  
 Sempre que você usa os contratos de mensagem ou parâmetros para descrever o conteúdo da mensagem, você deve usar a serialização para converter entre tipos de .NET Framework e a representação do XML infoset. A serialização é usada em outros locais no WCF, por exemplo, <xref:System.ServiceModel.Channels.Message> tem um <xref:System.ServiceModel.Channels.Message.GetBody%2A> método genérico que você pode usar para ler todo o corpo da mensagem desserializada em um objeto.  
  
 O WCF dá suporte a duas tecnologias de serialização "prontas para uso" para serialização e desserialização de parâmetros e partes de mensagem: o <xref:System.Runtime.Serialization.DataContractSerializer> e o `XmlSerializer` . Além disso, você pode escrever serializadores personalizados. No entanto, outras partes do WCF (como o `GetBody` método genérico ou a serialização de falha de SOAP) podem ser restritas para usar apenas as <xref:System.Runtime.Serialization.XmlObjectSerializer> subclasses ( <xref:System.Runtime.Serialization.DataContractSerializer> e <xref:System.Runtime.Serialization.NetDataContractSerializer> , mas não o <xref:System.Xml.Serialization.XmlSerializer> ), ou podem até mesmo ser embutidas em código para usar apenas o <xref:System.Runtime.Serialization.DataContractSerializer> .  
  
 O `XmlSerializer` é o mecanismo de serialização usado nos serviços Web do ASP.net. O `DataContractSerializer` é o novo mecanismo de serialização que compreende o novo modelo de programação de contrato de dados. `DataContractSerializer`é a opção padrão e a escolha de usar o `XmlSerializer` pode ser feita em uma base por operação usando o <xref:System.ServiceModel.Description.DataContractSerializerOperationBehavior.DataContractFormatAttribute%2A> atributo.  
  
 <xref:System.ServiceModel.Description.DataContractSerializerOperationBehavior>e <xref:System.ServiceModel.Description.XmlSerializerOperationBehavior> são os comportamentos de operação responsáveis por conectar os formatadores de mensagem para o `DataContractSerializer` e o `XmlSerializer` , respectivamente. O <xref:System.ServiceModel.Description.DataContractSerializerOperationBehavior> comportamento pode realmente operar com qualquer serializador derivado de <xref:System.Runtime.Serialization.XmlObjectSerializer> , incluindo o <xref:System.Runtime.Serialization.NetDataContractSerializer> (descrito em detalhes no uso de serialização autônoma). O comportamento chama uma das `CreateSerializer` sobrecargas do método virtual para obter o serializador. Para conectar um serializador diferente, crie uma nova <xref:System.ServiceModel.Description.DataContractSerializerOperationBehavior> subclasse e substitua as `CreateSerializer` sobrecargas.  
  
## <a name="see-also"></a>Consulte também

- [Especificando transferência de dados em contratos de serviço](specifying-data-transfer-in-service-contracts.md)
