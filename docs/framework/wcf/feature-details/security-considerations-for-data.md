---
title: Considerações de segurança para dados
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
ms.assetid: a7eb98da-4a93-4692-8b59-9d670c79ffb2
ms.openlocfilehash: 8b54aea1409f2b4c0a3d39d215922ba62c2a3563
ms.sourcegitcommit: c4a15c6c4ecbb8a46ad4e67d9b3ab9b8b031d849
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/20/2020
ms.locfileid: "88656964"
---
# <a name="security-considerations-for-data"></a>Considerações de segurança para dados

Ao lidar com dados no Windows Communication Foundation (WCF), você deve considerar várias categorias de ameaça. A tabela a seguir lista as classes de ameaça mais importantes relacionadas ao processamento de dados. O WCF fornece ferramentas para atenuar essas ameaças.

Negação de serviço ao receber dados não confiáveis, os dados podem fazer com que o lado de recebimento acesse uma quantidade desproporcional de vários recursos, como memória, threads, conexões disponíveis ou ciclos de processador, causando cálculos longos. Um ataque de negação de serviço contra um servidor pode causar uma falha e não pode processar mensagens de outros clientes legítimos.

A execução de código mal-intencionado dados não confiáveis recebidos faz com que o lado de recebimento execute o código que não pretendia.

Divulgação de informações o invasor remoto força a parte destinatária a responder às suas solicitações de forma a divulgar mais informações do que pretender.

## <a name="user-provided-code-and-code-access-security"></a>Código fornecido pelo usuário e segurança de acesso do código

Vários locais na infraestrutura de Windows Communication Foundation (WCF) executam o código que é fornecido pelo usuário. Por exemplo, o <xref:System.Runtime.Serialization.DataContractSerializer> mecanismo de serialização pode chamar acessadores `set` e acessadores de propriedade fornecidos pelo usuário `get` . A infraestrutura de canal do WCF também pode chamar classes derivadas fornecidas pelo usuário da <xref:System.ServiceModel.Channels.Message> classe.

É responsabilidade do autor do código garantir que não existam vulnerabilidades de segurança. Por exemplo, se você criar um tipo de contrato de dados com uma propriedade de membro de dados do tipo inteiro e na `set` implementação do acessador alocar uma matriz com base no valor da propriedade, você exporá a possibilidade de um ataque de negação de serviço se uma mensagem mal-intencionada contiver um valor muito grande para esse membro de dados. Em geral, evite qualquer alocação com base em dados de entrada ou processamentos longos no código fornecido pelo usuário (especialmente se o processamento longo puder ser causado por uma pequena quantidade de dados de entrada). Ao executar a análise de segurança do código fornecido pelo usuário, considere também todos os casos de falha (ou seja, todas as ramificações de código em que as exceções são lançadas).

O exemplo final de código fornecido pelo usuário é o código dentro de sua implementação de serviço para cada operação. A segurança da implementação de seu serviço é sua responsabilidade. É fácil criar inadvertidamente implementações de operações inseguras que podem resultar em vulnerabilidades de negação de serviço. Por exemplo, uma operação que usa uma cadeia de caracteres e retorna a lista de clientes de um banco de dados cujo nome começa com essa cadeia de caracteres. Se você estiver trabalhando com um banco de dados grande e a cadeia de caracteres que está sendo passada for apenas uma única letra, seu código poderá tentar criar uma mensagem maior do que toda a memória disponível, fazendo com que o serviço inteiro falhe. (Um <xref:System.OutOfMemoryException> não é recuperável na .NET Framework e sempre resulta no encerramento do seu aplicativo.)

Você deve garantir que nenhum código mal-intencionado seja conectado aos vários pontos de extensibilidade. Isso é especialmente relevante quando executado sob confiança parcial, lidando com tipos de assemblies parcialmente confiáveis ou criando componentes utilizáveis por código parcialmente confiável. Para obter mais informações, consulte "ameaças de confiança parcial" em uma seção posterior.

Observe que, ao executar em confiança parcial, a infraestrutura de serialização de contrato de dados dá suporte apenas a um subconjunto limitado do modelo de programação de contrato de dados, por exemplo, membros de dados privados ou tipos que usam o <xref:System.SerializableAttribute> atributo não têm suporte. Para obter mais informações, consulte [confiança parcial](partial-trust.md).

## <a name="avoiding-unintentional-information-disclosure"></a>Evitando a divulgação involuntária de informações

Ao criar tipos serializáveis com a segurança em mente, a divulgação de informações é uma possível preocupação.

Considere os seguintes pontos:

- O <xref:System.Runtime.Serialization.DataContractSerializer> modelo de programação permite a exposição de dados privados e internos fora do tipo ou assembly durante a serialização. Além disso, a forma de um tipo pode ser exposta durante a exportação do esquema. Certifique-se de entender a projeção de serialização do tipo. Se você não quiser que nada seja exposto, desabilite a serialização (por exemplo, não aplicando o <xref:System.Runtime.Serialization.DataMemberAttribute> atributo no caso de um contrato de dados).

- Lembre-se de que o mesmo tipo pode ter várias projeções de serialização, dependendo do serializador em uso. O mesmo tipo pode expor um conjunto de dados quando usado com o <xref:System.Runtime.Serialization.DataContractSerializer> e outro conjunto de dados quando usado com o <xref:System.Xml.Serialization.XmlSerializer> . O uso acidental do serializador incorreto pode levar à divulgação de informações.

- O uso do <xref:System.Xml.Serialization.XmlSerializer> no modo de/Encoded de chamada de procedimento remoto herdado (RPC) pode expor involuntariamente a forma do grafo do objeto no lado de envio para o lado de recebimento.

## <a name="preventing-denial-of-service-attacks"></a>Prevenção de ataques de negação de serviço

### <a name="quotas"></a>Cotas

Fazer com que o lado de recebimento aloque uma quantidade significativa de memória é um ataque potencial de negação de serviço. Embora esta seção se concentre em problemas de consumo de memória decorrentes de mensagens grandes, podem ocorrer outros ataques. Por exemplo, as mensagens podem usar uma quantidade desproporcional de tempo de processamento.

Os ataques de negação de serviço geralmente são mitigados usando cotas. Quando uma cota é excedida, uma <xref:System.ServiceModel.QuotaExceededException> exceção normalmente é lançada. Sem a cota, uma mensagem mal-intencionada pode fazer com que toda a memória disponível seja acessada, resultando em uma <xref:System.OutOfMemoryException> exceção ou todas as pilhas disponíveis a serem acessadas, resultando em um <xref:System.StackOverflowException> .

O cenário de Cota excedida é recuperável; se encontrado em um serviço em execução, a mensagem sendo processada no momento será descartada e o serviço continuará em execução e processará outras mensagens. Os cenários de memória insuficiente e de estouro de pilha, no entanto, não são recuperáveis em nenhum lugar da .NET Framework; o serviço será encerrado se encontrar tais exceções.

As cotas no WCF não envolvem nenhuma pré-alocação. Por exemplo, se a <xref:System.ServiceModel.Channels.TransportBindingElement.MaxReceivedMessageSize%2A> cota (encontrada em várias classes) for definida como 128 KB, isso não significará que 128 KB é alocado automaticamente para cada mensagem. A quantidade real alocada depende do tamanho real da mensagem de entrada.

Muitas cotas estão disponíveis na camada de transporte. Essas são cotas impostas pelo canal de transporte específico em uso (HTTP, TCP e assim por diante). Embora este tópico discuta algumas dessas cotas, essas cotas são descritas em detalhes em [cotas de transporte](transport-quotas.md).

### <a name="hashtable-vulnerability"></a>Vulnerabilidade de Hashtable

Existe uma vulnerabilidade quando os contratos de dados contêm Hashtables ou coleções. O problema ocorre se um grande número de valores for inserido em uma tabela de hash em que um grande número desses valores gerará o mesmo valor de hash. Isso pode ser usado como um ataque DOS.  Essa vulnerabilidade pode ser atenuada Configurando a cota de vinculação de MaxReceivedMessageSize. Deve-se ter cuidado ao definir essa cota para evitar esses ataques. Essa cota coloca um limite superior no tamanho da mensagem do WCF. Além disso, evite usar Hashtables ou coleções em seus contratos de dados.

## <a name="limiting-memory-consumption-without-streaming"></a>Limitando o consumo de memória sem streaming

O modelo de segurança em mensagens grandes depende se o streaming está em uso. No caso não transmitido básico, as mensagens são armazenadas em buffer na memória. Nesse caso, use a <xref:System.ServiceModel.Channels.TransportBindingElement.MaxReceivedMessageSize%2A> cota no <xref:System.ServiceModel.Channels.TransportBindingElement> ou nas associações fornecidas pelo sistema para proteger contra mensagens grandes, limitando o tamanho máximo da mensagem a ser acessado. Observe que um serviço pode estar processando várias mensagens ao mesmo tempo, caso em que elas estão todas na memória. Use o recurso de limitação para atenuar essa ameaça.

Observe também que `MaxReceivedMessageSize` o não coloca um limite superior no consumo de memória por mensagem, mas limita-o a dentro de um fator constante. Por exemplo, se o `MaxReceivedMessageSize` for 1 MB e uma mensagem de 1 MB for recebida e, em seguida, desserializada, será necessária memória adicional para conter o grafo de objeto desserializado, resultando em um consumo de memória total bem superior a 1 MB. Por esse motivo, Evite criar tipos serializáveis que possam resultar em um consumo de memória significativo sem muitos dados de entrada. Por exemplo, um contrato de dados "mycontract" com 50 campos de membro de dados opcionais e um adicional 100 campos particulares poderiam ser instanciados com a construção XML " \<MyContract/> ". Esse XML resulta na acesso à memória para 150 campos. Observe que os membros de dados são opcionais por padrão. O problema é composto quando tal tipo faz parte de uma matriz.

`MaxReceivedMessageSize` sozinho não é suficiente para impedir todos os ataques de negação de serviço. Por exemplo, o desserializador pode ser forçado a desserializar um grafo de objeto profundamente aninhado (um objeto que contém outro objeto que contém um outro, e assim por diante) por uma mensagem de entrada. Os <xref:System.Runtime.Serialization.DataContractSerializer> métodos e de <xref:System.Xml.Serialization.XmlSerializer> chamada em uma maneira aninhada para desserializar esses grafos. O aninhamento profundo de chamadas de método pode resultar em irrecuperável <xref:System.StackOverflowException> . Essa ameaça é atenuada pela definição da <xref:System.ServiceModel.Configuration.XmlDictionaryReaderQuotasElement.MaxDepth%2A> cota para limitar o nível de aninhamento de XML, conforme discutido na seção "usando o XML com segurança" posteriormente no tópico.

Definir cotas adicionais para `MaxReceivedMessageSize` é especialmente importante ao usar a codificação XML binária. O uso da codificação binária é, de certa forma, equivalente à compactação: um pequeno grupo de bytes na mensagem de entrada pode representar muitos dados. Portanto, mesmo uma mensagem sendo ajustada ao `MaxReceivedMessageSize` limite pode ocupar muito mais memória na forma totalmente expandida. Para atenuar essas ameaças específicas de XML, todas as cotas do leitor de XML devem ser definidas corretamente, conforme discutido na seção "usando o XML com segurança" posteriormente neste tópico.

## <a name="limiting-memory-consumption-with-streaming"></a>Limitando o consumo de memória com streaming

Ao transmitir, você pode usar uma pequena `MaxReceivedMessageSize` configuração para proteger contra ataques de negação de serviço. No entanto, cenários mais complicados são possíveis com o streaming. Por exemplo, um serviço de carregamento de arquivo aceita arquivos maiores do que toda a memória disponível. Nesse caso, defina `MaxReceivedMessageSize` como um valor muito grande, esperando que quase nenhum dado seja armazenado em buffer na memória e que a mensagem seja transmitida diretamente para o disco. Se uma mensagem mal-intencionada pode, de alguma forma, forçar o WCF a armazenar em buffer dados em vez de transmiti-los nesse caso, `MaxReceivedMessageSize` o não protege mais contra a mensagem que acessa toda a memória disponível.

Para atenuar essa ameaça, existem configurações de cota específicas em vários componentes de processamento de dados do WCF que limitam o buffer. O mais importante deles é a `MaxBufferSize` propriedade em vários elementos de ligação de transporte e associações padrão. Ao transmitir, essa cota deve ser definida levando em conta a quantidade máxima de memória que você está disposto a alocar por mensagem. Assim como acontece com `MaxReceivedMessageSize` , a configuração não coloca um máximo absoluto no consumo de memória, mas limita-o apenas dentro de um fator constante. Além disso, assim como com, lembre- `MaxReceivedMessageSize` se da possibilidade de que várias mensagens sejam processadas simultaneamente.

### <a name="maxbuffersize-details"></a>Detalhes do MaxBufferSize

A `MaxBufferSize` Propriedade limita qualquer WCF de buffer em massa. Por exemplo, o WCF sempre armazena em buffer os cabeçalhos SOAP e as falhas de SOAP, bem como quaisquer partes MIME encontradas para não estar na ordem de leitura natural em uma mensagem MTOM (mecanismo de otimização de transmissão de mensagens). Essa configuração limita a quantidade de armazenamento em buffer em todos esses casos.

O WCF realiza isso passando o `MaxBufferSize` valor para os vários componentes que podem armazenar em buffer. Por exemplo, algumas <xref:System.ServiceModel.Channels.Message.CreateMessage%2A> sobrecargas da <xref:System.ServiceModel.Channels.Message> classe usam um `maxSizeOfHeaders` parâmetro. O WCF passa o `MaxBufferSize` valor para esse parâmetro para limitar a quantidade de buffer de cabeçalho SOAP. É importante definir esse parâmetro ao usar a <xref:System.ServiceModel.Channels.Message> classe diretamente. Em geral, ao usar um componente no WCF que usa parâmetros de cota, é importante entender as implicações de segurança desses parâmetros e defini-los corretamente.

O codificador de mensagem MTOM também tem uma `MaxBufferSize` configuração. Ao usar associações padrão, isso é definido automaticamente para o valor de nível de transporte `MaxBufferSize` . No entanto, ao usar o elemento de associação de codificador de mensagem MTOM para construir uma associação personalizada, é importante definir a `MaxBufferSize` propriedade como um valor seguro quando o streaming é usado.

## <a name="xml-based-streaming-attacks"></a>Ataques de streaming baseados em XML

`MaxBufferSize` sozinho não é suficiente para garantir que o WCF não possa ser forçado a armazenar em buffer quando o streaming é esperado. Por exemplo, os leitores XML do WCF sempre armazenam em buffer toda a marca de início do elemento XML ao começar a ler um novo elemento. Isso é feito para que os namespaces e atributos sejam processados corretamente. Se `MaxReceivedMessageSize` o estiver configurado para ser grande (por exemplo, para habilitar um cenário de streaming de arquivo grande de direto para disco), uma mensagem mal-intencionada poderá ser construída onde todo o corpo da mensagem for uma marca de início de elemento XML grande. Uma tentativa de ler os resultados resulta em um <xref:System.OutOfMemoryException> . Esse é um dos muitos ataques possíveis de negação de serviço baseados em XML que podem ser atenuados usando cotas de leitor XML, abordadas na seção "usando o XML com segurança" posteriormente neste tópico. Ao transmitir, é especialmente importante definir todas essas cotas.

### <a name="mixing-streaming-and-buffering-programming-models"></a>Misturando modelos de programação de streaming e armazenamento em buffer

Muitos ataques possíveis surgem da combinação de modelos de programação de streaming e não streaming no mesmo serviço. Suponha que haja um contrato de serviço com duas operações: uma <xref:System.IO.Stream> e outra usa uma matriz de algum tipo personalizado. Suponha também que `MaxReceivedMessageSize` esteja definido como um valor grande para habilitar a primeira operação para processar grandes fluxos. Infelizmente, isso significa que as mensagens grandes agora podem ser enviadas para a segunda operação também, e o desserializador armazena em buffer os dados na memória como uma matriz antes que a operação seja chamada. Esse é um ataque potencial de negação de serviço: a `MaxBufferSize` cota não limita o tamanho do corpo da mensagem, que é o que o desserializador trabalha.

Por esse motivo, evite misturar operações baseadas em fluxo e não transmitidas no mesmo contrato. Se for absolutamente necessário misturar os dois modelos de programação, use as seguintes precauções:

- Desative o <xref:System.Runtime.Serialization.IExtensibleDataObject> recurso definindo a <xref:System.ServiceModel.ServiceBehaviorAttribute.IgnoreExtensionDataObject%2A> Propriedade do <xref:System.ServiceModel.ServiceBehaviorAttribute> para `true` . Isso garante que somente os membros que fazem parte do contrato sejam desserializados.

- Defina a <xref:System.Runtime.Serialization.DataContractSerializer.MaxItemsInObjectGraph%2A> propriedade de <xref:System.Runtime.Serialization.DataContractSerializer> como um valor seguro. Essa cota também está disponível no <xref:System.ServiceModel.ServiceBehaviorAttribute> atributo ou por meio da configuração. Essa cota limita o número de objetos desserializados em um episódio de desserialização. Normalmente, cada parte do parâmetro de operação ou do corpo da mensagem em um contrato de mensagem é desserializada em um episódio. Ao desserializar matrizes, cada entrada de matriz é contada como um objeto separado.

- Defina todas as cotas do leitor XML para valores seguros. Preste atenção para <xref:System.Xml.XmlDictionaryReaderQuotas.MaxDepth%2A> , e <xref:System.Xml.XmlDictionaryReaderQuotas.MaxStringContentLength%2A> <xref:System.Xml.XmlDictionaryReaderQuotas.MaxArrayLength%2A> e evite cadeias de caracteres em operações que não sejam de streaming.

- Examine a lista de tipos conhecidos, tendo em mente que qualquer um deles pode ser instanciado a qualquer momento (consulte a seção "impedindo que tipos indesejados sejam carregados" mais adiante neste tópico).

- Não use nenhum tipo que implemente a <xref:System.Xml.Serialization.IXmlSerializable> interface que armazena muitos dados em buffer. Não adicione esses tipos à lista de tipos conhecidos.

- Não use o <xref:System.Xml.XmlElement> , <xref:System.Xml.XmlNode> matrizes, <xref:System.Byte> matrizes ou tipos que implementam <xref:System.Runtime.Serialization.ISerializable> em um contrato.

- Não use o <xref:System.Xml.XmlElement> , <xref:System.Xml.XmlNode> matrizes, <xref:System.Byte> matrizes ou tipos que implementam <xref:System.Runtime.Serialization.ISerializable> na lista de tipos conhecidos.

As precauções anteriores se aplicam quando a operação não transmitida usa o <xref:System.Runtime.Serialization.DataContractSerializer> . Nunca misture modelos de programação de streaming e não streaming no mesmo serviço se você estiver usando o <xref:System.Xml.Serialization.XmlSerializer> , porque ele não tem a proteção da <xref:System.ServiceModel.Description.DataContractSerializerOperationBehavior.MaxItemsInObjectGraph%2A> cota.

### <a name="slow-stream-attacks"></a>Ataques de fluxo lento

Uma classe de ataques de negação de serviço de streaming não envolve o consumo de memória. Em vez disso, o ataque envolve um remetente ou receptor de dados lento. Enquanto aguarda os dados serem enviados ou recebidos, recursos como threads e conexões disponíveis são esgotados. Essa situação pode ocorrer como resultado de um ataque mal-intencionado ou de um remetente/receptor legítimo em uma conexão de rede lenta.

Para atenuar esses ataques, defina os tempos limite de transporte corretamente. Para obter mais informações, consulte [cotas de transporte](transport-quotas.md). Em segundo lugar, nunca use operações síncronas `Read` ou `Write` ao trabalhar com fluxos no WCF.

## <a name="using-xml-safely"></a>Usando o XML com segurança

> [!NOTE]
> Embora esta seção seja sobre XML, as informações também se aplicam a documentos JavaScript Object Notation (JSON). As cotas funcionam de forma semelhante, usando o [mapeamento entre JSON e XML](mapping-between-json-and-xml.md).

### <a name="secure-xml-readers"></a>Leitores de XML seguros

O XML infoset constitui a base de todo o processamento de mensagens no WCF. Ao aceitar dados XML de uma fonte não confiável, existem várias possibilidades de ataque de negação de serviço que devem ser atenuadas. O WCF fornece leitores XML seguros e especiais. Esses leitores são criados automaticamente ao usar uma das codificações padrão no WCF (text, binary ou MTOM).

Alguns dos recursos de segurança desses leitores estão sempre ativos. Por exemplo, os leitores nunca processam DTDs (definições de tipo de documento), que são uma fonte potencial de ataques de negação de serviço e nunca devem aparecer em mensagens SOAP legítimas. Outros recursos de segurança incluem cotas de leitor que devem ser configuradas, que são descritas na seção a seguir.

Ao trabalhar diretamente com leitores de XML (como ao escrever seu próprio codificador personalizado ou ao trabalhar diretamente com a <xref:System.ServiceModel.Channels.Message> classe), sempre use os leitores de segurança do WCF quando houver a possibilidade de trabalhar com dados não confiáveis. Crie os leitores de segurança chamando uma das sobrecargas de método de fábrica estática de <xref:System.Xml.XmlDictionaryReader.CreateTextReader%2A> , <xref:System.Xml.XmlDictionaryReader.CreateBinaryReader%2A> ou <xref:System.Xml.XmlDictionaryReader.CreateMtomReader%2A> na <xref:System.Xml.XmlDictionaryReader> classe. Ao criar um leitor, transmita valores de cota seguros. Não chame as `Create` sobrecargas do método. Eles não criam um leitor do WCF. Em vez disso, é criado um leitor que não é protegido pelos recursos de segurança descritos nesta seção.

### <a name="reader-quotas"></a>Cotas do leitor

Os leitores de XML seguros têm cinco cotas configuráveis. Eles normalmente são configurados usando a `ReaderQuotas` Propriedade nos elementos de associação de codificação ou associações padrão, ou usando um <xref:System.Xml.XmlDictionaryReaderQuotas> objeto passado ao criar um leitor.

#### <a name="maxbytesperread"></a>MaxBytesPerRead

Essa cota limita o número de bytes que são lidos em uma única `Read` operação ao ler a marca de início do elemento e seus atributos. (Em casos não transmitidos, o nome do elemento em si não é contado em relação à cota.) <xref:System.Xml.XmlDictionaryReaderQuotas.MaxBytesPerRead%2A> é importante pelos seguintes motivos:

- O nome do elemento e seus atributos sempre são armazenados em buffer na memória quando estão sendo lidos. Portanto, é importante definir essa cota corretamente no modo de streaming para evitar o buffer excessivo quando o streaming é esperado. Consulte a `MaxDepth` seção cota para obter informações sobre a quantidade real de buffers que ocorrem.

- Ter atributos XML demais pode consumir todo o tempo de processamento de maneira desproporcional porque a exclusividade dos nomes de atributo tem que ser verificada. O `MaxBytesPerRead` atenua essa ameaça.

#### <a name="maxdepth"></a>MaxDepth

Essa cota limita a profundidade máxima de aninhamento dos elementos XML. Por exemplo, o documento " \<A> \<B> \<C/> \</B> \</A> " tem uma profundidade de aninhamento de três. <xref:System.Xml.XmlDictionaryReaderQuotas.MaxDepth%2A> é importante pelos seguintes motivos:

- O `MaxDepth` interage com o `MaxBytesPerRead`: o leitor sempre mantém dados na memória para o elemento atual e todos os seus ancestrais, para que o consumo máximo de memória do leitor seja proporcional ao produto dessas duas configurações.

- Ao desserializar um grafo de objeto aninhado mais profundamente, o desserializador é forçado para acessar a pilha inteira e gerar um <xref:System.StackOverflowException> irrecuperável. Uma correlação direta existe entre aninhamento de XML e aninhamento de objeto para <xref:System.Runtime.Serialization.DataContractSerializer> e <xref:System.Xml.Serialization.XmlSerializer>. Use `MaxDepth` para atenuar essa ameaça.

#### <a name="maxnametablecharcount"></a>MaxNameTableCharCount

Essa cota limita o tamanho do *NameTable*do leitor. O nametable contém algumas cadeias de caracteres (como namespaces e prefixos) que são encontrados ao processar um documento XML. Como essas cadeias de caracteres são armazenadas em buffer na memória, defina essa cota para evitar o buffer excessivo quando o streaming for esperado.

#### <a name="maxstringcontentlength"></a>MaxStringContentLength

Essa cota limita o tamanho máximo da cadeia de caracteres que o leitor de XML retorna. Essa cota não limita o consumo de memória no próprio leitor de XML, mas no componente que esteja usando o leitor. Por exemplo, quando o <xref:System.Runtime.Serialization.DataContractSerializer> usa um leitor protegido com <xref:System.Xml.XmlDictionaryReaderQuotas.MaxStringContentLength%2A>, ele não desserializa as cadeias de caracteres maiores do que essa cota. Ao usar a <xref:System.Xml.XmlDictionaryReader> classe diretamente, nem todos os métodos respeitam essa cota, mas apenas os métodos especificamente projetados para ler cadeias de caracteres, como o <xref:System.Xml.XmlDictionaryReader.ReadContentAsString%2A> método. A <xref:System.Xml.XmlReader.Value%2A> propriedade no leitor não é afetada por essa cota e, portanto, não deve ser usada quando a proteção fornecida por essa cota for necessária.

#### <a name="maxarraylength"></a>MaxArrayLength

Essa cota limita o tamanho máximo de uma matriz de primitivas que o leitor de XML retorna, inclusive matrizes de bytes. Essa cota não limita o consumo de memória no próprio leitor de XML, mas em qualquer componente que esteja usando o leitor. Por exemplo, quando o <xref:System.Runtime.Serialization.DataContractSerializer> usa um leitor protegido com <xref:System.Xml.XmlDictionaryReaderQuotas.MaxArrayLength%2A>, ele não desserializa as matrizes de bytes maiores do que essa cota. É importante definir essa cota ao tentar misturar modelos de programação de streaming e buffer em um único contrato. Tenha em mente que, ao usar a <xref:System.Xml.XmlDictionaryReader> classe diretamente, somente os métodos projetados especificamente para ler matrizes de tamanho arbitrário de determinados tipos primitivos, como <xref:System.Xml.XmlDictionaryReader.ReadInt32Array%2A> , respeitam essa cota.

## <a name="threats-specific-to-the-binary-encoding"></a>Ameaças específicas à codificação binária

A codificação XML binária que o WCF dá suporte inclui um recurso de *cadeias de caracteres de dicionário* . Uma cadeia de caracteres grande pode ser codificada usando apenas alguns bytes. Isso permite ganhos de desempenho significativos, mas apresenta novas ameaças de negação de serviço que devem ser atenuadas.

Há dois tipos de dicionários: *estáticos* e *dinâmicos*. O dicionário estático é uma lista interna de cadeias de caracteres longas que podem ser representadas usando um código curto na codificação binária. Essa lista de cadeias de caracteres é fixada quando o leitor é criado e não pode ser modificada. Nenhuma das cadeias de caracteres no dicionário estático que o WCF usa por padrão é suficientemente grande para representar uma séria ameaça de negação de serviço, embora elas ainda possam ser usadas em um ataque de expansão de dicionário. Em cenários avançados em que você fornece seu próprio dicionário estático, tenha cuidado ao introduzir grandes cadeias de caracteres de dicionário.

O recurso de dicionários dinâmicos permite que as mensagens definam suas próprias cadeias de caracteres e as associe com códigos curtos. Esses mapeamentos de cadeia de caracteres para código são mantidos na memória durante toda a sessão de comunicação, de modo que as mensagens subsequentes não precisam reenviar as cadeias de caracteres e podem utilizar códigos já definidos. Essas cadeias de caracteres podem ser de comprimento arbitrário e, portanto, representam uma ameaça mais séria do que aquelas no dicionário estático.

A primeira ameaça que deve ser mitigada é a possibilidade do dicionário dinâmico (a tabela de mapeamento de cadeia de caracteres para código) se tornar muito grande. Esse dicionário pode ser expandido ao longo de várias mensagens e, portanto, a `MaxReceivedMessageSize` cota não oferece proteção porque se aplica apenas a cada mensagem separadamente. Portanto, existe uma <xref:System.ServiceModel.Channels.BinaryMessageEncodingBindingElement.MaxSessionSize%2A> propriedade separada no <xref:System.ServiceModel.Channels.BinaryMessageEncodingBindingElement> que limita o tamanho do dicionário.

Ao contrário da maioria das outras cotas, essa cota também se aplica ao gravar mensagens. Se ele for excedido durante a leitura de uma mensagem, o `QuotaExceededException` será lançado como de costume. Se ele for excedido durante a gravação de uma mensagem, qualquer cadeia de caracteres que faça com que a cota seja excedida será gravada como está, sem usar o recurso de dicionários dinâmicos.

### <a name="dictionary-expansion-threats"></a>Ameaças de expansão de dicionário

Uma classe significativa de ataques de binários específicos surge da expansão do dicionário. Uma mensagem pequena em formato binário pode transformar em uma mensagem muito grande na forma textual totalmente expandida se fizer uso extensivo do recurso de dicionários de cadeia de caracteres. O fator de expansão para cadeias de caracteres de dicionário dinâmico é limitado pela <xref:System.ServiceModel.Channels.BinaryMessageEncodingBindingElement.MaxSessionSize%2A> cota, porque nenhuma cadeia de caracteres de dicionário dinâmico excede o tamanho máximo de todo o dicionário.

As <xref:System.Xml.XmlDictionaryReaderQuotas.MaxNameTableCharCount%2A> `MaxStringContentLength` Propriedades, e `MaxArrayLength` limitam apenas o consumo de memória. Normalmente, eles não são necessários para atenuar as ameaças no uso não transmitido, pois o uso de memória já é limitado pelo `MaxReceivedMessageSize` . No entanto, o `MaxReceivedMessageSize` conta os bytes de pré-implementação. Quando a codificação binária está em uso, o consumo de memória pode ir além `MaxReceivedMessageSize` , limitado apenas por um fator de <xref:System.ServiceModel.Channels.BinaryMessageEncodingBindingElement.MaxSessionSize%2A> . Por esse motivo, é importante sempre definir todas as cotas de leitor (especialmente <xref:System.Xml.XmlDictionaryReaderQuotas.MaxStringContentLength%2A> ) ao usar a codificação binária.

Ao usar a codificação binária junto com o <xref:System.Runtime.Serialization.DataContractSerializer> , a `IExtensibleDataObject` interface pode ser usada para montar um ataque de expansão de dicionário. Essa interface essencialmente fornece armazenamento ilimitado para dados arbitrários que não fazem parte do contrato. Se as cotas não puderem ser definidas com baixo suficiente, isso `MaxSessionSize` multiplicado por não `MaxReceivedMessageSize` representa um problema, desabilite o `IExtensibleDataObject` recurso ao usar a codificação binária. Defina a `IgnoreExtensionDataObject` propriedade como `true` no `ServiceBehaviorAttribute` atributo. Como alternativa, não implemente a `IExtensibleDataObject` interface. Para obter mais informações, consulte [Contratos de dados compatíveis por encaminhamento](forward-compatible-data-contracts.md).

### <a name="quotas-summary"></a>Resumo de cotas

A tabela a seguir resume as diretrizes sobre cotas.

|Condição|Cotas importantes a serem definidas|
|---------------|-----------------------------|
|Sem streaming ou streaming de mensagens pequenas, texto ou codificação MTOM|`MaxReceivedMessageSize`, `MaxBytesPerRead`, e `MaxDepth`|
|Sem streaming ou streaming de mensagens pequenas, codificação binária|`MaxReceivedMessageSize`, `MaxSessionSize` e todos `ReaderQuotas`|
|Transmissão de mensagens grandes, texto ou codificação MTOM|`MaxBufferSize` e todos `ReaderQuotas`|
|Streaming de mensagens grandes, codificação binária|`MaxBufferSize`, `MaxSessionSize` e todos `ReaderQuotas`|

- Os tempos limite de nível de transporte sempre devem ser definidos e nunca usam leituras/gravações síncronas quando o streaming está em uso, independentemente de você estar transmitindo mensagens grandes ou pequenas.

- Em caso de dúvida sobre uma cota, defina-a como um valor seguro em vez de deixá-la aberta.

## <a name="preventing-malicious-code-execution"></a>Impedindo a execução de código mal-intencionado

As seguintes classes gerais de ameaças podem executar código e ter efeitos indesejados:

- O desserializador carrega um tipo mal-intencionado, inseguro ou sensível à segurança.

- Uma mensagem de entrada faz com que o desserializador Construa uma instância de um tipo normalmente seguro de forma que ela tenha consequências indesejadas.

As seções a seguir discutem ainda mais essas classes de ameaças.

## <a name="datacontractserializer"></a>DataContractSerializer

(Para obter informações de segurança sobre o <xref:System.Xml.Serialization.XmlSerializer> , consulte a documentação relevante.) O modelo de segurança do <xref:System.Xml.Serialization.XmlSerializer> é semelhante ao do <xref:System.Runtime.Serialization.DataContractSerializer> , e difere principalmente em detalhes. Por exemplo, o <xref:System.Xml.Serialization.XmlIncludeAttribute> atributo é usado para inclusão de tipo em vez do <xref:System.Runtime.Serialization.KnownTypeAttribute> atributo. No entanto, algumas ameaças exclusivas ao <xref:System.Xml.Serialization.XmlSerializer> são discutidas posteriormente neste tópico.

### <a name="preventing-unintended-types-from-being-loaded"></a>Impedindo que tipos indesejados sejam carregados

O carregamento de tipos indesejados pode ter consequências significativas, quer o tipo seja mal-intencionado ou tenha apenas efeitos colaterais sensíveis à segurança. Um tipo pode conter vulnerabilidades de segurança explorável, executar ações sensíveis à segurança em seu construtor ou construtor de classe, ter um grande volume de memória que facilita ataques de negação de serviço ou pode gerar exceções não recuperáveis. Os tipos podem ter construtores de classe que são executados assim que o tipo é carregado e antes de qualquer instância ser criada. Por esses motivos, é importante controlar o conjunto de tipos que o desserializador pode carregar.

O <xref:System.Runtime.Serialization.DataContractSerializer> desserializa de forma menos rígida. Ele nunca lê o tipo Common Language Runtime (CLR) e os nomes de assembly dos dados de entrada. Isso é semelhante ao comportamento do <xref:System.Xml.Serialization.XmlSerializer> , mas difere do comportamento do <xref:System.Runtime.Serialization.NetDataContractSerializer> , do <xref:System.Runtime.Serialization.Formatters.Binary.BinaryFormatter> e do <xref:System.Runtime.Serialization.Formatters.Soap.SoapFormatter> . O acoplamento flexível introduz um grau de segurança, pois o invasor remoto não pode indicar um tipo arbitrário para carregar simplesmente nomeando esse tipo na mensagem.

O <xref:System.Runtime.Serialization.DataContractSerializer> sempre é permitido para carregar um tipo esperado no momento de acordo com o contrato. Por exemplo, se um contrato de dados tiver um membro de dados do tipo `Customer` , o terá <xref:System.Runtime.Serialization.DataContractSerializer> permissão para carregar o `Customer` tipo ao desserializar esse membro de dados.

Além disso, o <xref:System.Runtime.Serialization.DataContractSerializer> dá suporte ao polimorfismo. Um membro de dados pode ser declarado como <xref:System.Object> , mas os dados de entrada podem conter uma `Customer` instância. Isso só será possível se o `Customer` tipo tiver sido "conhecido" para o desserializador por meio de um destes mecanismos:

- <xref:System.Runtime.Serialization.KnownTypeAttribute> atributo aplicado a um tipo.

- `KnownTypeAttribute` atributo que especifica um método que retorna uma lista de tipos.

- `ServiceKnownTypeAttribute` Attribute.

- A `KnownTypes` seção de configuração.

- Uma lista de tipos conhecidos passados explicitamente para o <xref:System.Runtime.Serialization.DataContractSerializer> durante a construção, se estiver usando o serializador diretamente.

Cada um desses mecanismos aumenta a área da superfície introduzindo mais tipos que o desserializador pode carregar. Controle cada um desses mecanismos para garantir que nenhum tipo mal-intencionado ou não intencional seja adicionado à lista de tipos conhecidos.

Quando um tipo conhecido está no escopo, ele pode ser carregado a qualquer momento e as instâncias do tipo podem ser criadas, mesmo que o contrato proíba realmente usá-lo. Por exemplo, suponha que o tipo "myperigosatype" seja adicionado à lista de tipos conhecidos usando um dos mecanismos acima. Isso significa que:

- `MyDangerousType` é carregado e seu construtor de classe é executado.

- Mesmo ao desserializar um contrato de dados com um membro de dados de cadeia de caracteres, uma mensagem mal-intencionada ainda pode fazer com que uma instância do `MyDangerousType` seja criada. O código no `MyDangerousType` , como setters de propriedade, pode ser executado. Depois que isso for feito, o desserializador tentará atribuir essa instância ao membro de dados de cadeia de caracteres e falhará com uma exceção.

Ao escrever um método que retorna uma lista de tipos conhecidos, ou ao passar uma lista diretamente para o <xref:System.Runtime.Serialization.DataContractSerializer> Construtor, certifique-se de que o código que prepara a lista seja seguro e opere somente em dados confiáveis.

Se especificar tipos conhecidos na configuração, verifique se o arquivo de configuração é seguro. Sempre use nomes fortes na configuração (especificando a chave pública do assembly assinado onde o tipo reside), mas não especifique a versão do tipo a ser carregado. O carregador de tipo escolhe automaticamente a versão mais recente, se possível. Se você especificar uma versão específica na configuração, execute o seguinte risco: um tipo pode ter uma vulnerabilidade de segurança que pode ser corrigida em uma versão futura, mas a versão vulnerável ainda é carregada porque ela é explicitamente especificada na configuração.

Ter um número excessivo de tipos conhecidos tem outra consequência: o <xref:System.Runtime.Serialization.DataContractSerializer> cria um cache de código de serialização/desserialização no domínio do aplicativo, com uma entrada para cada tipo que ele deve serializar e desserializar. Esse cache nunca é limpo, desde que o domínio do aplicativo esteja em execução. Portanto, um invasor que esteja ciente de que um aplicativo usa muitos tipos conhecidos pode causar a desserialização de todos esses tipos, fazendo com que o cache consuma uma quantidade desproporcionalmente grande de memória.

### <a name="preventing-types-from-being-in-an-unintended-state"></a>Impedindo que os tipos estejam em um estado não intencional

Um tipo pode ter restrições de consistência internas que devem ser impostas. É preciso tomar cuidado para evitar a interrupção dessas restrições durante a desserialização.

O exemplo a seguir de um tipo representa o estado de um bloqueio de horário em um espacial e impõe a restrição de que as portas interna e externa não podem ser abertas ao mesmo tempo.

[!code-csharp[DataContractAttribute#3](../../../../samples/snippets/csharp/VS_Snippets_CFX/datacontractattribute/cs/overview.cs#3)]
[!code-vb[DataContractAttribute#3](../../../../samples/snippets/visualbasic/VS_Snippets_CFX/datacontractattribute/vb/overview.vb#3)]

Um invasor pode enviar uma mensagem mal-intencionada como esta, contornar as restrições e colocar o objeto em um estado inválido, o que pode ter consequências indesejadas e imprevisíveis.

```xml
<SpaceStationAirlock>
    <innerDoorOpen>true</innerDoorOpen>
    <outerDoorOpen>true</outerDoorOpen>
</SpaceStationAirlock>
```

Essa situação pode ser evitada por estar ciente dos seguintes pontos:

- Quando o <xref:System.Runtime.Serialization.DataContractSerializer> desserializa a maioria das classes, os construtores não são executados. Portanto, não confie em nenhum gerenciamento de estado feito no construtor.

- Use retornos de chamada para garantir que o objeto esteja em um estado válido. O retorno de chamada marcado com o <xref:System.Runtime.Serialization.OnDeserializedAttribute> atributo é especialmente útil porque é executado após a desserialização ser concluída e tem a oportunidade de examinar e corrigir o estado geral. Para obter mais informações, consulte [retornos de chamada de serialização tolerantes à versão](version-tolerant-serialization-callbacks.md).

- Não crie tipos de contrato de dados para contar com qualquer ordem específica na qual os setters de propriedade devem ser chamados.

- Tome cuidado usando tipos herdados marcados com o <xref:System.SerializableAttribute> atributo. Muitos deles foram projetados para funcionar com .NET Framework comunicação remota para uso somente com dados confiáveis. Os tipos existentes marcados com esse atributo podem não ter sido projetados com a segurança de estado em mente.

- Não confie na <xref:System.Runtime.Serialization.DataMemberAttribute.IsRequired%2A> Propriedade do <xref:System.Runtime.Serialization.DataMemberAttribute> atributo para garantir a presença de dados no que diz respeito à segurança de estado. Os dados podem ser sempre `null` , `zero` ou `invalid` .

- Nunca confie em um grafo de objeto desserializado de uma fonte de dados não confiável sem validá-lo primeiro. Cada objeto individual pode estar em um estado consistente, mas o grafo do objeto como um todo pode não ser. Além disso, mesmo que o modo de preservação do grafo de objetos esteja desabilitado, o grafo desserializado pode ter várias referências ao mesmo objeto ou ter referências circulares. Para obter mais informações, consulte [serialização e desserialização](serialization-and-deserialization.md).

### <a name="using-the-netdatacontractserializer-securely"></a>Usando o NetDataContractSerializer com segurança

O <xref:System.Runtime.Serialization.NetDataContractSerializer> é um mecanismo de serialização que usa acoplamento rígido para tipos. Isso é semelhante ao <xref:System.Runtime.Serialization.Formatters.Binary.BinaryFormatter> e ao <xref:System.Runtime.Serialization.Formatters.Soap.SoapFormatter> . Ou seja, ele determina qual tipo deve ser instanciado lendo o assembly .NET Framework e o nome do tipo dos dados de entrada. Embora faça parte do WCF, não há uma maneira fornecida de conectar-se a esse mecanismo de serialização; o código personalizado deve ser gravado. O `NetDataContractSerializer` é fornecido principalmente para facilitar a migração de .NET Framework comunicação remota para o WCF. Para obter mais informações, consulte a seção relevante em [serialização e desserialização](serialization-and-deserialization.md).

Como a própria mensagem pode indicar que qualquer tipo pode ser carregado, o <xref:System.Runtime.Serialization.NetDataContractSerializer> mecanismo é inerentemente inseguro e deve ser usado somente com dados confiáveis. Para obter mais informações, consulte o [Guia de segurança do BinaryFormatter](../../../standard/serialization/binaryformatter-security-guide.md).

Mesmo quando usado com dados confiáveis, os dados de entrada podem especificar insuficientemente o tipo a ser carregado, especialmente se a <xref:System.Runtime.Serialization.NetDataContractSerializer.AssemblyFormat%2A> propriedade estiver definida como <xref:System.Runtime.Serialization.Formatters.FormatterAssemblyStyle.Simple> . Qualquer pessoa com acesso ao diretório do aplicativo ou ao cache de assembly global pode substituir um tipo mal-intencionado no lugar daquele que deve ser carregado. Sempre garanta a segurança do diretório do seu aplicativo e do cache de assembly global definindo as permissões corretamente.

Em geral, se você permitir o acesso de código parcialmente confiável à `NetDataContractSerializer` instância do ou controlar o seletor de substituto ( <xref:System.Runtime.Serialization.ISurrogateSelector> ) ou o associador de serialização ( <xref:System.Runtime.Serialization.SerializationBinder> ), o código poderá exercer um grande controle sobre o processo de serialização/desserialização. Por exemplo, ele pode injetar tipos arbitrários, levar à divulgação de informações, violação com o grafo de objeto resultante ou dados serializados ou estouro no fluxo serializado resultante.

Outra preocupação com a segurança com o `NetDataContractSerializer` é uma negação de serviço, não uma ameaça de execução de código mal-intencionado. Ao usar o `NetDataContractSerializer` , sempre defina a <xref:System.Runtime.Serialization.NetDataContractSerializer.MaxItemsInObjectGraph%2A> cota como um valor seguro. É fácil construir uma pequena mensagem mal-intencionada que aloca uma matriz de objetos cujo tamanho é limitado apenas por essa cota.

### <a name="xmlserializer-specific-threats"></a>Ameaças específicas ao XmlSerializer

O <xref:System.Xml.Serialization.XmlSerializer> modelo de segurança é semelhante ao do <xref:System.Runtime.Serialization.DataContractSerializer> . No entanto, algumas ameaças são exclusivas do <xref:System.Xml.Serialization.XmlSerializer> .

O <xref:System.Xml.Serialization.XmlSerializer> gera *assemblies de serialização* em tempo de execução que contêm código que realmente serializa e desserializa; esses assemblies são criados em um diretório de arquivos temporários. Se algum outro processo ou usuário tiver direitos de acesso a esse diretório, eles poderão substituir o código de serialização/desserialização por código arbitrário. <xref:System.Xml.Serialization.XmlSerializer>Em seguida, o executa esse código usando seu contexto de segurança, em vez do código de serialização/desserialização. Verifique se as permissões estão definidas corretamente no diretório de arquivos temporários para evitar que isso aconteça.

O <xref:System.Xml.Serialization.XmlSerializer> também tem um modo no qual ele usa assemblies de serialização gerados previamente em vez de gerá-los em tempo de execução. Esse modo é disparado sempre que o <xref:System.Xml.Serialization.XmlSerializer> pode encontrar um assembly de serialização adequado. O <xref:System.Xml.Serialization.XmlSerializer> verifica se o assembly de serialização foi assinado pela mesma chave que foi usada para assinar o assembly que contém os tipos que estão sendo serializados. Isso oferece proteção contra assemblies mal-intencionados disfarçados como assemblies de serialização. No entanto, se o assembly que contém os tipos serializáveis não for assinado, o <xref:System.Xml.Serialization.XmlSerializer> não poderá executar essa verificação e usará qualquer assembly com o nome correto. Isso torna possível a execução do código mal-intencionado. Sempre assine os assemblies que contêm seus tipos serializáveis ou controle rigidamente o acesso ao diretório do seu aplicativo e ao cache de assembly global para evitar a introdução de assemblies mal-intencionados.

O <xref:System.Xml.Serialization.XmlSerializer> pode estar sujeito a um ataque de negação de serviço. O <xref:System.Xml.Serialization.XmlSerializer> não tem uma `MaxItemsInObjectGraph` cota (como está disponível no <xref:System.Runtime.Serialization.DataContractSerializer> ). Portanto, ele desserializa uma quantidade arbitrária de objetos, limitado apenas pelo tamanho da mensagem.

### <a name="partial-trust-threats"></a>Ameaças de confiança parcial

Observe as seguintes preocupações com relação a ameaças relacionadas ao código em execução com confiança parcial. Essas ameaças incluem código parcialmente confiável mal-intencionado, bem como código parcialmente confiável mal-intencionado em combinação com outros cenários de ataque (por exemplo, código parcialmente confiável que constrói uma cadeia de caracteres específica e, em seguida, desserializando-a).

- Ao usar qualquer componente de serialização, nunca declare nenhuma permissão antes desse uso, mesmo que todo o cenário de serialização esteja dentro do escopo de sua declaração e você não esteja lidando com dados ou objetos não confiáveis. Esse uso pode levar a vulnerabilidades de segurança.

- Nos casos em que o código parcialmente confiável tem controle sobre o processo de serialização, seja por meio de pontos de extensibilidade (substitutos), tipos sendo serializados, ou por outros meios, o código parcialmente confiável pode fazer com que o serializador gere uma grande quantidade de dados para o fluxo serializado, o que pode causar a negação de serviço (DoS) ao destinatário desse fluxo. Se você estiver serializando dados destinados a um destino que seja sensível a ameaças DoS, não Serialize tipos parcialmente confiáveis ou, caso contrário, permita a serialização de controle de código parcialmente confiável.

- Se você permitir o acesso de código parcialmente confiável à <xref:System.Runtime.Serialization.DataContractSerializer> instância do ou controlar os [substitutos de contrato de dados](../extending/data-contract-surrogates.md), poderá exercer uma grande quantidade de controle sobre o processo de serialização/desserialização. Por exemplo, ele pode injetar tipos arbitrários, levar à divulgação de informações, violação com o grafo de objeto resultante ou dados serializados ou estouro no fluxo serializado resultante. Uma <xref:System.Runtime.Serialization.NetDataContractSerializer> ameaça equivalente é descrita na seção "usando o NetDataContractSerializer com segurança".

- Se o <xref:System.Runtime.Serialization.DataContractAttribute> atributo for aplicado a um tipo (ou ao tipo marcado como <xref:System.SerializableAttribute> mas não for <xref:System.Runtime.Serialization.ISerializable> ), o desserializador poderá criar uma instância desse tipo mesmo que todos os construtores sejam não públicos ou protegidos por demandas.

- Nunca confie no resultado da desserialização, a menos que os dados a serem desserializados sejam confiáveis e você tenha certeza de que todos os tipos conhecidos são tipos confiáveis. Observe que os tipos conhecidos não são carregados do arquivo de configuração do aplicativo (mas são carregados do arquivo de configuração do computador) durante a execução em confiança parcial.

- Se você passar uma <xref:System.Runtime.Serialization.DataContractSerializer> instância com um substituto adicionado ao código parcialmente confiável, o código poderá alterar qualquer configuração modificável nesse substituto.

- Para um objeto desserializado, se o leitor de XML (ou os dados contidos) vier de código parcialmente confiável, trate o objeto desserializado resultante como dados não confiáveis.

- O fato de que o <xref:System.Runtime.Serialization.ExtensionDataObject> tipo não tem nenhum membro público não significa que os dados dentro dele são seguros. Por exemplo, se você desserializar de uma fonte de dados privilegiada em um objeto no qual alguns dados residem e, em seguida, enviar esse objeto para código parcialmente confiável, o código parcialmente confiável poderá ler os dados no `ExtensionDataObject` serializando o objeto. Considere definir <xref:System.Runtime.Serialization.DataContractSerializer.IgnoreExtensionDataObject%2A> como `true` ao desserializar de uma fonte de dados privilegiada em um objeto que é passado posteriormente para código parcialmente confiável.

- <xref:System.Runtime.Serialization.DataContractSerializer> e <xref:System.Runtime.Serialization.Json.DataContractJsonSerializer> oferecem suporte à serialização de membros privados, protegidos, internos e públicos em confiança total. No entanto, em confiança parcial, somente membros públicos podem ser serializados. Um <xref:System.Security.SecurityException> será gerado se um aplicativo tentar serializar um membro não público.

    Para permitir que membros internos ou protegidos internamente sejam serializados em confiança parcial, use o <xref:System.Runtime.CompilerServices.InternalsVisibleToAttribute> atributo assembly. Esse atributo permite que um assembly declare que seus membros internos são visíveis para algum outro assembly. Nesse caso, um assembly que deseja ter seus membros internos serializados declara que seus membros internos são visíveis para System.Runtime.Serialization.dll.

    A vantagem dessa abordagem é que ela não requer um caminho de geração de código elevado.

    Ao mesmo tempo, há duas desvantagens principais.

    A primeira desvantagem é que a propriedade de aceitação do <xref:System.Runtime.CompilerServices.InternalsVisibleToAttribute> atributo é de todo o assembly. Ou seja, você não pode especificar que apenas uma determinada classe possa ter seus membros internos serializados. É claro que você ainda pode optar por não serializar um membro interno específico, simplesmente não adicionando um <xref:System.Runtime.Serialization.DataMemberAttribute> atributo a esse membro. Da mesma forma, um desenvolvedor também pode optar por tornar um membro interno, em vez de privado ou protegido, com preocupações com pequenas visibilidade.

    A segunda desvantagem é que ela ainda não dá suporte a membros privados ou protegidos.

    Para ilustrar o uso do <xref:System.Runtime.CompilerServices.InternalsVisibleToAttribute> atributo em confiança parcial, considere o seguinte programa:

    [!code-csharp[CDF_WCF_SecurityConsiderationsForData#1](../../../../samples/snippets/csharp/VS_Snippets_CFX/cdf_wcf_securityconsiderationsfordata/cs/program.cs#1)]

    No exemplo acima, `PermissionsHelper.InternetZone` corresponde ao <xref:System.Security.PermissionSet> para confiança parcial. Agora, sem o <xref:System.Runtime.CompilerServices.InternalsVisibleToAttribute> atributo, o aplicativo falhará, lançando um <xref:System.Security.SecurityException> indicando que os membros não públicos não podem ser serializados em confiança parcial.

    No entanto, se adicionarmos a seguinte linha ao arquivo de origem, o programa será executado com êxito.

    [!code-csharp[CDF_WCF_SecurityConsiderationsForData#2](../../../../samples/snippets/csharp/VS_Snippets_CFX/cdf_wcf_securityconsiderationsfordata/cs/program.cs#2)]

## <a name="other-state-management-concerns"></a>Outras preocupações com gerenciamento de estado

Vale A pena mencionar algumas outras preocupações sobre o gerenciamento de estado do objeto:

- Ao usar o modelo de programação baseado em fluxo com um transporte de streaming, o processamento da mensagem ocorre à medida que a mensagem chega. O remetente da mensagem pode anular a operação de envio no meio do fluxo, deixando seu código em um estado imprevisível se mais conteúdo era esperado. Em geral, não confie no fluxo que está sendo concluído e não execute nenhum trabalho em uma operação baseada em fluxo que não possa ser revertida caso o fluxo seja anulado. Isso também se aplica à situação em que uma mensagem pode ser mal formada após o corpo do streaming (por exemplo, pode estar faltando uma marca de fim para o envelope SOAP ou pode ter um segundo corpo de mensagem).

- O uso do `IExtensibleDataObject` recurso pode causar a emissão de dados confidenciais. Se você estiver aceitando dados de uma fonte não confiável em contratos de dados com `IExtensibleObjectData` o e reemitindo-os posteriormente em um canal seguro em que as mensagens são assinadas, você está potencialmente comprovando dados sobre os quais não conhece. Além disso, o estado geral que você está enviando pode ser inválido se você levar em conta as partes conhecidas e desconhecidas dos dados. Evite essa situação definindo seletivamente a propriedade de dados de extensão como `null` ou desabilitando seletivamente o `IExtensibleObjectData` recurso.

## <a name="schema-import"></a>Importação de esquema

Normalmente, o processo de importação de esquema para gerar tipos ocorre apenas no momento do design, por exemplo, ao usar a [ferramenta de utilitário de metadados ServiceModel (Svcutil.exe)](../servicemodel-metadata-utility-tool-svcutil-exe.md) em um serviço Web para gerar uma classe de cliente. No entanto, em cenários mais avançados, você pode processar o esquema em tempo de execução. Lembre-se de que isso pode expô-lo a riscos de negação de serviço. Alguns esquemas podem levar muito tempo para serem importados. Nunca use o <xref:System.Xml.Serialization.XmlSerializer> componente de importação de esquema em cenários como esses, se os esquemas forem possivelmente provenientes de uma fonte não confiável.

## <a name="threats-specific-to-aspnet-ajax-integration"></a>Ameaças específicas à integração do ASP.NET AJAX

Quando o usuário implementa <xref:System.ServiceModel.Description.WebScriptEnablingBehavior> ou <xref:System.ServiceModel.Description.WebHttpBehavior> , o WCF expõe um ponto de extremidade que pode aceitar mensagens XML e JSON. No entanto, há apenas um conjunto de cotas de leitor, usado pelo leitor de XML e o leitor JSON. Algumas configurações de cota podem ser apropriadas para um leitor, mas muito grandes para o outro.

Ao implementar `WebScriptEnablingBehavior` , o usuário tem a opção de expor um proxy JavaScript no ponto de extremidade. Os seguintes problemas de segurança devem ser considerados:

- As informações sobre o serviço (nomes de operação, nomes de parâmetro e assim por diante) podem ser obtidas examinando o proxy JavaScript.

- Ao usar o ponto de extremidade JavaScript, informações confidenciais e privadas podem ser mantidas no cache do navegador da Web do cliente.

## <a name="a-note-on-components"></a>Uma observação sobre os componentes do

O WCF é um sistema flexível e personalizável. A maior parte do conteúdo deste tópico se concentra nos cenários de uso mais comuns do WCF. No entanto, é possível compor componentes que o WCF fornece de várias maneiras diferentes. É importante entender as implicações de segurança do uso de cada componente. Especialmente:

- Quando você deve usar leitores XML, use os leitores que a <xref:System.Xml.XmlDictionaryReader> classe fornece, em oposição a qualquer outro leitor. Os leitores seguros são criados <xref:System.Xml.XmlDictionaryReader.CreateTextReader%2A> usando <xref:System.Xml.XmlDictionaryReader.CreateBinaryReader%2A> métodos, ou <xref:System.Xml.XmlDictionaryReader.CreateMtomReader%2A> . Não use o <xref:System.Xml.XmlReader.Create%2A> método. Sempre configure os leitores com cotas seguras. Os mecanismos de serialização no WCF são seguros somente quando usados com leitores XML seguros do WCF.

- Ao usar o <xref:System.Runtime.Serialization.DataContractSerializer> para desserializar dados potencialmente não confiáveis, sempre defina a <xref:System.Runtime.Serialization.DataContractSerializer.MaxItemsInObjectGraph%2A> propriedade.

- Ao criar uma mensagem, defina o `maxSizeOfHeaders` parâmetro se o não `MaxReceivedMessageSize` oferecer proteção suficiente.

- Ao criar um codificador, sempre configure as cotas relevantes, como `MaxSessionSize` e `MaxBufferSize` .

- Ao usar um filtro de mensagem XPath, defina o <xref:System.ServiceModel.Dispatcher.XPathMessageFilter.NodeQuota%2A> para limitar a quantidade de nós XML que o filtro visita. Não use expressões XPath que podem levar muito tempo para serem computadas sem visitar muitos nós.

- Em geral, ao usar qualquer componente que aceite uma cota, entenda suas implicações de segurança e defina-a como um valor seguro.

## <a name="see-also"></a>Consulte também

- <xref:System.Runtime.Serialization.DataContractSerializer>
- <xref:System.Xml.XmlDictionaryReader>
- <xref:System.Xml.Serialization.XmlSerializer>
- [Tipos de contratos de dados conhecidos](data-contract-known-types.md)
