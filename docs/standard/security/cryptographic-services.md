---
title: Serviços criptográficos
description: Uma visão geral dos métodos e das práticas de criptografia com suporte no .NET.
ms.date: 07/14/2020
ms.technology: dotnet-standard
helpviewer_keywords:
- cryptography [.NET]
- pattern of derived class inheritance
- digital signatures
- asymmetric cryptographic algorithms
- digital signatures, public-key systems
- public keys
- decryption [.NET]
- private keys
- MAC algorithms
- cryptographic algorithms
- private keys, overview
- encryption [.NET]
- security [.NET], encryption
- cryptographic services
- symmetric cryptographic algorithms
- hash
- message authentication codes
- derived class inheritance
- cryptography [.NET], about
- random number generation
ms.assetid: f96284bc-7b73-44b5-ac59-fac613ad09f8
ms.openlocfilehash: 4cd4e493e0e7d159b2749dac78b9a560e20fd75c
ms.sourcegitcommit: b7a8b09828bab4e90f66af8d495ecd7024c45042
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/04/2020
ms.locfileid: "87557015"
---
# <a name="cryptographic-services"></a>Serviços criptográficos

Redes públicas, como a Internet, não fornecem meios de comunicação segura entre entidades. A comunicação por tais redes é suscetível a ser lida ou até mesmo modificada por terceiros não autorizados. A criptografia ajuda a proteger os dados de serem exibidos, fornece maneiras de detectar se os dados foram modificados e ajuda a fornecer meios seguros de comunicação em canais não seguros de outra forma. Por exemplo, os dados podem ser criptografados usando um algoritmo criptográfico, transmitidos em um estado criptografado e posteriormente descriptografados pela parte pretendida. Se um terceiro interceptar os dados criptografados, será difícil decifrar.

No .NET, as classes no <xref:System.Security.Cryptography> namespace gerenciam muitos detalhes de criptografia para você. Alguns são wrappers para implementações de sistema operacional, enquanto outros são implementações puramente gerenciadas. Você não precisa ser um especialista em criptografia para usar essas classes. Quando você cria uma nova instância de uma das classes de algoritmo de criptografia, as chaves são geradas automaticamente para facilitar o uso e as propriedades padrão são mais seguras e seguras possíveis.

Esta visão geral fornece uma sinopse dos métodos e das práticas de criptografia com suporte do .NET, incluindo os manifestos do ClickOnce.

## <a name="cryptographic-primitives"></a>Primitivos criptográficos

Em uma situação típica em que a criptografia é usada, duas partes (Alice e Bob) se comunicam por um canal não seguro. Alice e Bob querem garantir que a comunicação permaneça incompreensível por qualquer pessoa que possa estar ouvindo. Além disso, como Alice e Bob estão em locais remotos, Alice deve garantir que as informações recebidas de Bob não tenham sido modificadas por ninguém durante a transmissão. Além disso, ela deve certificar-se de que as informações realmente se originam de Bob e não de alguém que esteja representando Bob.

A criptografia é usada para atingir as seguintes metas:

- Confidencialidade: para ajudar a proteger a identidade ou os dados de um usuário de serem lidos.

- Integridade dos dados: para ajudar a proteger os dados de serem alterados.

- Autenticação: para garantir que os dados sejam originados de uma entidade específica.

- Não repúdio: para impedir que uma parte específica negue que enviou uma mensagem.

Para atingir essas metas, você pode usar uma combinação de algoritmos e práticas conhecidas como primitivos de criptografia para criar um esquema criptográfico. A tabela a seguir lista os primitivos criptográficos e seus usos.

|Primitivo de criptografia|Uso|
|-----------------------------|---------|
|Criptografia de chave secreta (criptografia simétrica)|Executa uma transformação nos dados para impedir que sejam lidos por terceiros. Esse tipo de criptografia usa uma única chave secreta compartilhada para criptografar e descriptografar dados.|
|Criptografia de chave pública (criptografia assimétrica)|Executa uma transformação nos dados para impedir que sejam lidos por terceiros. Esse tipo de criptografia usa um par de chaves pública/privada para criptografar e descriptografar dados.|
|Assinatura criptográfica|Ajuda a verificar se os dados são provenientes de uma parte específica por meio da criação de uma assinatura digital exclusiva para essa parte. Esse processo também usa funções de hash.|
|Hashes criptográficos|Mapeia dados de qualquer comprimento para uma sequência de bytes de comprimento fixo. Hashes são estatisticamente exclusivos; uma sequência diferente de dois bytes não fará hash para o mesmo valor.|

## <a name="secret-key-encryption"></a>Criptografia de chave secreta

Os algoritmos de criptografia de chave secreta usam uma única chave secreta para criptografar e descriptografar dados. Você deve proteger a chave do acesso por agentes não autorizados, pois qualquer parte que tenha a chave pode usá-la para descriptografar seus dados ou criptografar seus próprios dados, reivindicando-os de sua origem.

A criptografia de chave secreta também é conhecida como criptografia simétrica porque a mesma chave é usada para criptografia e descriptografia. Os algoritmos de criptografia de chave secreta são muito rápidos (em comparação com os algoritmos de chave pública) e são adequados para executar transformações de criptografia em grandes fluxos de dados. Os algoritmos de criptografia assimétrica, como RSA, são limitados matematicamente na quantidade de dados que eles podem criptografar. Os algoritmos de criptografia simétrica geralmente não têm esses problemas.

Um tipo de algoritmo de chave secreta chamado de codificação de bloco é usado para criptografar um bloco de dados de cada vez. As codificações de bloco, como o padrão de criptografia de dados (DES), TripleDES e criptografia AES (AES) transformam criptograficamente um bloco de entrada de *n* bytes em um bloco de saída de bytes criptografados. Se você quiser criptografar ou descriptografar uma sequência de bytes, será necessário bloquear por bloco. Como *n* é pequeno (8 bytes para des e TripleDES; 16 bytes [o padrão], 24 bytes ou 32 bytes para AES), os valores de dados maiores que *n* precisam ser criptografados um bloco por vez. Valores de dados menores que *n* precisam ser expandidos para *n* para serem processados.

Uma forma simples de codificação de bloco é chamada de modo de Codebook eletrônico (ECB). O modo ECB não é considerado seguro, pois não usa um vetor de inicialização para inicializar o primeiro bloco de texto sem formatação. Para uma determinada chave secreta *k*, uma codificação simples de bloco que não usa um vetor de inicialização criptografará o mesmo bloco de entrada de texto sem formatação no mesmo bloco de saída de texto cifrado. Portanto, se você tiver blocos duplicados em seu fluxo de texto não criptografado de entrada, você terá blocos duplicados no fluxo de texto cifrado de saída. Esses blocos de saída duplicados alertam usuários não autorizados à criptografia fraca, usavam os algoritmos que podem ter sido empregados e os possíveis modos de ataque. O modo de codificação ECB é, portanto, muito vulnerável à análise e, por fim, à descoberta de chaves.

As classes de codificação de bloco que são fornecidas na biblioteca de classes base usam um modo de encadeamento padrão chamado CBC (encadeamento de bloco de codificação), embora você possa alterar esse padrão, se desejar.

As codificações do CBC superam os problemas associados a cifras de ECB usando um vetor de inicialização (IV) para criptografar o primeiro bloco de texto sem formatação. Cada bloco subseqüente de texto sem formatação passa por uma operação bit-a-OR ( `XOR` ) exclusiva com o bloco cifrado anterior antes de ser criptografado. Portanto, cada bloco de texto cifrado depende de todos os blocos anteriores. Quando esse sistema é usado, os cabeçalhos de mensagens comuns que podem ser conhecidos por um usuário não autorizado não podem ser usados para fazer engenharia reversa de uma chave.

Uma maneira de comprometer os dados criptografados com um CBC Cipher é executar uma pesquisa detalhada de cada chave possível. Dependendo do tamanho da chave usada para executar a criptografia, esse tipo de pesquisa é muito demorado usando até os computadores mais rápidos e, portanto, é inviável. Tamanhos de chave maiores são mais difíceis de decifrar. Embora a criptografia não o torne teoricamente impossível para um adversário recuperar os dados criptografados, ele aumenta o custo de fazer isso. Se levar três meses para executar uma pesquisa exaustiva para recuperar dados que são significativos apenas por alguns dias, o método de pesquisa exaustiva é impraticável.

A desvantagem da criptografia de chave secreta é que ela presume que duas partes concordaram em uma chave e IV, e que comunicaram seus valores. O IV não é considerado um segredo e pode ser transmitido em texto não criptografado com a mensagem. No entanto, a chave deve ser mantida em segredo de usuários não autorizados. Devido a esses problemas, a criptografia de chave secreta geralmente é usada junto com a criptografia de chave pública para comunicar de forma privada os valores da chave e do IV.

Supondo que Alice e Bob sejam duas partes que desejam se comunicar por meio de um canal não seguro, eles podem usar a criptografia de chave secreta da seguinte maneira: Alice e Bob concordam em usar um algoritmo específico (AES, por exemplo) com uma chave e um IV específicos. Alice compõe uma mensagem e cria um fluxo de rede (talvez um pipe nomeado ou um email de rede) no qual enviar a mensagem. Em seguida, ela criptografa o texto usando a chave e o IV e envia a mensagem criptografada e a IV para Bob pela intranet. Bob recebe o texto criptografado e descriptografa-o usando o IV e a chave concordada anteriormente. Se a transmissão for interceptada, o interceptador não poderá recuperar a mensagem original, pois não saberá a chave. Nesse cenário, somente a chave deve permanecer secreta. Em um cenário do mundo real, Alice ou Bob gera uma chave secreta e usa a criptografia assimétrica (chave pública) para transferir a chave secreta (simétrica) para a outra parte. Para obter mais informações sobre criptografia de chave pública, consulte a próxima seção.

O .NET fornece as seguintes classes que implementam algoritmos de criptografia de chave secreta:

- <xref:System.Security.Cryptography.Aes>

- <xref:System.Security.Cryptography.HMACSHA256>, <xref:System.Security.Cryptography.HMACSHA384> e <xref:System.Security.Cryptography.HMACSHA512> (Esses são tecnicamente algoritmos de chave secreta porque representam códigos de autenticação de mensagens que são calculados usando uma função de hash criptográfico combinada com uma chave secreta. Consulte [valores de hash](#hash-values), mais adiante neste artigo.)

## <a name="public-key-encryption"></a>Criptografia de chave pública

A criptografia de chave pública usa uma chave privada que deve ser mantida em segredo de usuários não autorizados e uma chave pública que pode ser tornada pública para qualquer pessoa. A chave pública e a chave privada são matematicamente vinculadas; os dados criptografados com a chave pública só podem ser descriptografados com a chave privada, e os dados assinados com a chave privada podem ser verificados somente com a chave pública. A chave pública pode ser disponibilizada para qualquer pessoa; Ele é usado para criptografar dados a serem enviados para o protetor da chave privada. Os algoritmos de criptografia de chave pública também são conhecidos como algoritmos assimétricos porque uma chave é necessária para criptografar dados e outra chave é necessária para descriptografar dados. Uma regra básica de criptografia proíbe a reutilização de chave e ambas as chaves devem ser exclusivas para cada sessão de comunicação. No entanto, na prática, as chaves assimétricas geralmente são de longa duração.

Duas partes (Alice e Bob) podem usar a criptografia de chave pública da seguinte maneira: primeiro, Alice gera um par de chaves pública/privada. Se Bob quiser enviar uma mensagem criptografada para Alice, ele perguntará sua chave pública. Alice envia ao Bob sua chave pública em uma rede não segura e Bob usa essa chave para criptografar uma mensagem. Bob envia a mensagem criptografada para Alice e a descriptografa usando sua chave privada. Se Bob recebeu a chave de Alice por um canal não seguro, como uma rede pública, Bob está aberto a um ataque man-in-the-Middle. Portanto, Bob deve verificar com Alice que ele tem uma cópia correta de sua chave pública.

Durante a transmissão da chave pública de Alice, um agente não autorizado pode interceptar a chave. Além disso, o mesmo agente pode interceptar a mensagem criptografada de Bob. No entanto, o agente não pode descriptografar a mensagem com a chave pública. A mensagem pode ser descriptografada somente com a chave privada de Alice, que não foi transmitida. Alice não usa sua chave privada para criptografar uma mensagem de resposta para Bob, porque qualquer pessoa com a chave pública poderia descriptografar a mensagem. Se Alice quiser enviar uma mensagem de volta para Bob, ela perguntará Bob por sua chave pública e criptografará sua mensagem usando essa chave pública. Em seguida, Bob descriptografa a mensagem usando sua chave privada associada.

Nesse cenário, Alice e Bob usam criptografia assimétrica (de chave pública) para transferir uma chave secreta (simétrica) e usar criptografia de chave secreta para o restante da sessão.

A lista a seguir oferece comparações entre algoritmos criptográficos de chave pública e chave secreta:

- Os algoritmos de criptografia de chave pública usam um tamanho de buffer fixo, enquanto os algoritmos de criptografia de chave secreta usam um buffer de comprimento variável.

- Os algoritmos de chave pública não podem ser usados para encadear dados em fluxos do modo como os algoritmos de chave secreta podem, porque apenas pequenas quantidades de dados podem ser criptografadas. Portanto, as operações assimétricas não usam o mesmo modelo de streaming que as operações simétricas.

- A criptografia de chave pública tem um espaço de chaves muito maior (intervalo de valores possíveis para a chave) que a criptografia de chave secreta. Portanto, a criptografia de chave pública é menos suscetível a ataques exaustivos que tentam cada chave possível.

- As chaves públicas são fáceis de distribuir porque não precisam ser protegidas, desde que alguma maneira exista para verificar a identidade do remetente.

- Alguns algoritmos de chave pública (como RSA e DSA, mas não Diffie-Hellman) podem ser usados para criar assinaturas digitais para verificar a identidade do remetente dos dados.

- Os algoritmos de chave pública são muito lentos em comparação com os algoritmos de chave secreta e não são projetados para criptografar grandes quantidades de dados. Os algoritmos de chave pública são úteis apenas para transferir quantidades muito pequenas de dados. Normalmente, a criptografia de chave pública é usada para criptografar uma chave e um IV a serem usados por um algoritmo de chave secreta. Depois que a chave e a IV são transferidas, a criptografia de chave secreta é usada para o restante da sessão.

O .NET fornece as seguintes classes que implementam algoritmos de chave pública:

- <xref:System.Security.Cryptography.RSA>

- <xref:System.Security.Cryptography.ECDsa>

- <xref:System.Security.Cryptography.ECDiffieHellman>

- <xref:System.Security.Cryptography.DSA>

A RSA permite criptografia e assinatura, mas o DSA pode ser usado apenas para assinatura. O DSA não é tão seguro quanto o RSA, e recomendamos a RSA. O Diffie-Hellman pode ser usado somente para geração de chave. Em geral, os algoritmos de chave pública são mais limitados em seus usos que os algoritmos de chave privada.

## <a name="digital-signatures"></a>Assinaturas digitais

Os algoritmos de chave pública também podem ser usados para formar assinaturas digitais. As assinaturas digitais autenticam a identidade de um remetente (se você confia na chave pública do remetente) e ajudam a proteger a integridade dos dados. Usando uma chave pública gerada por Alice, o destinatário dos dados de Alice pode verificar se Alice o enviou comparando a assinatura digital para os dados de Alice e a chave pública de Alice.

Para usar a criptografia de chave pública para assinar digitalmente uma mensagem, Alice primeiro aplica um algoritmo de hash à mensagem para criar um resumo de mensagem. O resumo da mensagem é uma representação compacta e exclusiva dos dados. Em seguida, Alice criptografa o resumo da mensagem com sua chave privada para criar sua assinatura pessoal. Após receber a mensagem e a assinatura, Bob descriptografa a assinatura usando a chave pública de Alice para recuperar o resumo da mensagem e faz hash da mensagem usando o mesmo algoritmo de hash usado por Alice. Se o resumo da mensagem que Bob calcula exatamente corresponder ao resumo da mensagem recebido de Alice, Bob terá a garantia de que a mensagem veio do possessor da chave privada e que os dados não foram modificados. Se Bob confiar que Alice é a possessor da chave privada, ele sabe que a mensagem veio de Alice.

> [!NOTE]
> Uma assinatura pode ser verificada por qualquer pessoa, pois a chave pública do remetente é um conhecimento comum e normalmente é incluída no formato de assinatura digital. Esse método não retém o sigilo da mensagem; para que a mensagem seja secreta, ela também deve ser criptografada.

O .NET fornece as seguintes classes que implementam algoritmos de assinatura digital:

- <xref:System.Security.Cryptography.RSA>

- <xref:System.Security.Cryptography.ECDsa>

- <xref:System.Security.Cryptography.DSA>

## <a name="hash-values"></a>Valores de hash

Os algoritmos de hash mapeiam valores binários de um comprimento arbitrário para valores binários menores de um comprimento fixo, conhecidos como valores de hash. Um valor de hash é uma representação numérica de uma parte dos dados. Se você aplicar o hash em um parágrafo de texto não criptografado e alterar até mesmo uma letra do parágrafo, um hash subsequente produzirá um valor diferente. Se o hash for criptograficamente forte, seu valor será alterado significativamente. Por exemplo, se um único bit de uma mensagem for alterado, uma função de hash forte poderá produzir uma saída diferente de 50 por cento. Muitos valores de entrada podem fazer hash para o mesmo valor de saída. No entanto, é computacionalmente impossível encontrar duas entradas distintas que contenham hash para o mesmo valor.

Duas partes (Alice e Bob) podem usar uma função de hash para garantir a integridade da mensagem. Eles selecionariam um algoritmo de hash para assinar suas mensagens. Alice escreveria uma mensagem e, em seguida, criaria um hash dessa mensagem usando o algoritmo selecionado. Em seguida, eles seguirão um dos seguintes métodos:

- Alice envia a mensagem de texto sem formatação e a mensagem com hash (assinatura digital) para Bob. Bob recebe e aplica hash à mensagem e compara seu valor de hash ao valor de hash recebido de Alice. Se os valores de hash forem idênticos, a mensagem não foi alterada. Se os valores não forem idênticos, a mensagem foi alterada depois que Alice o escreveu.

  Infelizmente, esse método não estabelece a autenticidade do remetente. Qualquer pessoa pode representar Alice e enviar uma mensagem para Bob. Eles podem usar o mesmo algoritmo de hash para assinar suas mensagens, e todos os Bob podem determinar que a mensagem corresponde à sua assinatura. Essa é uma forma de ataque man-in-the-Middle. Para obter mais informações, consulte [exemplo de comunicação segura da CNG (Cryptography Next Generation)](https://docs.microsoft.com/previous-versions/cc488018(v=vs.100)).

- Alice envia a mensagem de texto não criptografado para Bob em um canal público não seguro. Ela envia a mensagem com hash para Bob por um canal privado seguro. Bob recebe a mensagem de texto sem formatação, faz hash e compara o hash ao hash trocado de modo privado. Se os hashes corresponderem, Bob saberá duas coisas:

  - A mensagem não foi alterada.

  - O remetente da mensagem (Alice) é autêntico.

  Para que esse sistema funcione, Alice deve ocultar seu valor de hash original de todas as partes, exceto Bob.

- Alice envia a mensagem de texto não criptografado para Bob em um canal público não seguro e coloca a mensagem com hash em seu site da Web publicamente visível.

  Esse método impede a violação de mensagens, impedindo que alguém modifique o valor de hash. Embora a mensagem e seu hash possam ser lidos por qualquer pessoa, o valor de hash só pode ser alterado por Alice. Um invasor que deseja representar Alice exigiria acesso ao site de Alice.

Nenhum dos métodos anteriores impedirá que alguém leia as mensagens de Alice, pois elas são transmitidas em texto sem formatação. A segurança completa normalmente requer assinaturas digitais (assinatura de mensagem) e criptografia.

O .NET fornece as seguintes classes que implementam algoritmos de hash:

- <xref:System.Security.Cryptography.SHA256>.

- <xref:System.Security.Cryptography.SHA384>.

- <xref:System.Security.Cryptography.SHA512>.

O .NET também fornece <xref:System.Security.Cryptography.MD5> e <xref:System.Security.Cryptography.SHA1> . Mas os algoritmos MD5 e SHA-1 foram considerados inseguros e, em vez disso, o SHA-2 é recomendado. O SHA-2 inclui SHA256, SHA384 e SHA512.

## <a name="random-number-generation"></a>Geração de números aleatórios

A geração de números aleatórios é integral para muitas operações criptográficas. Por exemplo, as chaves de criptografia precisam ser o mais aleatório possível para que seja impraticável reproduzi-las. Os geradores de número aleatório criptográfico devem gerar uma saída que seja computacionalmente inviável para prever com uma probabilidade melhor do que uma metade. Portanto, qualquer método de previsão do próximo bit de saída não deve funcionar melhor do que a adivinhação aleatória. As classes no .NET Framework usam geradores de números aleatórios para gerar chaves criptográficas.

A <xref:System.Security.Cryptography.RandomNumberGenerator> classe é uma implementação de um algoritmo gerador de números aleatórios.

## <a name="clickonce-manifests"></a>Manifestos ClickOnce

No .NET Framework 3,5, as seguintes classes de criptografia permitem obter e verificar informações sobre assinaturas de manifesto para aplicativos que são implantados usando a [tecnologia ClickOnce](/visualstudio/deployment/clickonce-security-and-deployment):

- A <xref:System.Security.Cryptography.ManifestSignatureInformation> classe obtém informações sobre uma assinatura de manifesto quando você usa suas <xref:System.Security.Cryptography.ManifestSignatureInformation.VerifySignature%2A> sobrecargas de método.

- Você pode usar a <xref:System.Security.ManifestKinds> enumeração para especificar quais manifestos verificar. O resultado da verificação é um dos valores de <xref:System.Security.Cryptography.SignatureVerificationResult> enumeração.

- A <xref:System.Security.Cryptography.ManifestSignatureInformationCollection> classe fornece uma coleção somente leitura de <xref:System.Security.Cryptography.ManifestSignatureInformation> objetos das assinaturas verificadas.

 Além disso, as seguintes classes fornecem informações de assinatura específicas:

- <xref:System.Security.Cryptography.StrongNameSignatureInformation>contém as informações de assinatura de nome forte para um manifesto.

- <xref:System.Security.Cryptography.X509Certificates.AuthenticodeSignatureInformation>representa as informações de assinatura Authenticode de um manifesto.

- <xref:System.Security.Cryptography.X509Certificates.TimestampInformation>contém informações sobre o carimbo de data/hora em uma assinatura Authenticode.

- <xref:System.Security.Cryptography.X509Certificates.TrustStatus>fornece uma maneira simples de verificar se uma assinatura Authenticode é confiável.

## <a name="cryptography-next-generation-cng-classes"></a>Classes CNG (Cryptography Next Generation)

No .NET Framework 3,5 e versões posteriores, as classes CNG (Cryptography Next Generation) fornecem um wrapper gerenciado em relação às funções CNG nativas. (CNG é a substituição para CryptoAPI.) Essas classes têm "CNG" como parte de seus nomes. Central para as classes de wrapper CNG é a <xref:System.Security.Cryptography.CngKey> classe de contêiner de chave, que abstrai o armazenamento e o uso de chaves CNG. Essa classe permite que você armazene um par de chaves ou uma chave pública com segurança e faça referência a ela usando um nome de cadeia de caracteres simples. A classe de assinatura baseada em curva elíptica <xref:System.Security.Cryptography.ECDsaCng> e a <xref:System.Security.Cryptography.ECDiffieHellmanCng> classe de criptografia podem usar <xref:System.Security.Cryptography.CngKey> objetos.

A <xref:System.Security.Cryptography.CngKey> classe é usada para uma variedade de operações adicionais, incluindo abrir, criar, excluir e exportar chaves. Ele também fornece acesso ao identificador de chave subjacente a ser usado ao chamar funções nativas diretamente.

O .NET Framework 3,5 também inclui uma variedade de classes CNG com suporte, como as seguintes:

- <xref:System.Security.Cryptography.CngProvider>mantém um provedor de armazenamento de chaves.

- <xref:System.Security.Cryptography.CngAlgorithm>mantém um algoritmo CNG.

- <xref:System.Security.Cryptography.CngProperty>mantém as propriedades de chave usadas com frequência.

## <a name="see-also"></a>Confira também

- [Modelo de criptografia](cryptography-model.md) – descreve como a criptografia é implementada na biblioteca de classes base.
- [Criptografia de plataforma cruzada](cross-platform-cryptography.md)
- [Vulnerabilidades de temporização com descriptografia simétrica no modo CBC usando preenchimento](vulnerabilities-cbc-mode.md)
- [Proteção de dados do ASP.NET Core](/aspnet/core/security/data-protection/introduction)
