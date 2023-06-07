---
title: Terminologia de segurança do WCF
ms.date: 03/30/2017
helpviewer_keywords:
- security [WCF], terminology
- security glossary [WCF]
- security terms [WCF]
ms.assetid: 68dde024-8e51-40ba-804f-ec52d85e9ca9
ms.openlocfilehash: a07d7c6da71f4195cb1641ae8ac7585b4158ed63
ms.sourcegitcommit: cdb295dd1db589ce5169ac9ff096f01fd0c2da9d
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/09/2020
ms.locfileid: "84600965"
---
# <a name="wcf-security-terminology"></a>Terminologia de segurança do WCF
Parte da terminologia usada para discutir a segurança pode não ser familiar. Este tópico fornece breves explicações de alguns termos de segurança, mas não pretende fornecer uma documentação abrangente para cada item.  
  
 Para obter mais informações sobre os termos usados na documentação do Windows Communication Foundation (WCF), consulte [conceitos fundamentais de Windows Communication Foundation](../fundamental-concepts.md).  
  
 lista de controle de acesso (ACL)  
 Uma lista de proteções de segurança que se aplica a um objeto. (Um objeto pode ser um arquivo, processo, evento ou qualquer outra coisa que tenha um descritor de segurança.) Uma entrada em uma ACL é uma ACE (entrada de controle de acesso). Há dois tipos de ACLs: discricionárias e de sistema.  
  
 autenticação  
 O processo para verificar se um usuário, computador, serviço ou processo é quem ou o que ele alega ser.  
  
 autorização  
 O ato de controlar o acesso e os direitos a um recurso. Por exemplo, permitir que os membros de um grupo leiam um arquivo, mas permitindo que somente membros de outro grupo alterem o arquivo.  
  
 certificado de autoridade de certificação (CA)  
 Identifica a autoridade de certificação que emite certificados de autenticação de servidor e cliente para os servidores e clientes que solicitam esses certificados. Como ele contém uma chave pública usada em assinaturas digitais, ele também é conhecido como um *certificado de assinatura*. Se a autoridade de certificação for uma autoridade raiz, o certificado de autoridade de certificação poderá ser chamado de *certificado raiz*. Às vezes, também conhecido como um *certificado de site*.  
  
 Hierarquia de CA  
 Uma hierarquia de CA contém várias CAs. Ele é organizado de forma que cada CA seja certificada por outra CA em um nível mais alto da hierarquia até que a parte superior da hierarquia, também conhecida como a *autoridade raiz*, seja atingida.  
  
 certificado  
 Uma instrução assinada digitalmente que contém informações sobre uma entidade e a chave pública da entidade, associando essas duas informações juntas. Um certificado é emitido por uma organização (ou entidade) confiável, chamada de autoridade de certificação, depois que a autoridade verificou que a entidade é quem diz ser.  
  
 Os certificados podem conter tipos diferentes de dados. Por exemplo, um certificado X. 509 inclui o formato do certificado, o número de série do certificado, o algoritmo usado para assinar o certificado, o nome da autoridade de certificação que emitiu o certificado, o nome e a chave pública da entidade que solicita o certificado e a assinatura da autoridade de certificação.  
  
 repositório de certificados  
 Normalmente, um armazenamento permanente em que certificados, CRLs (listas de certificados revogados) e listas de certificados confiáveis (CTLs) são armazenados. No entanto, é possível criar e abrir um repositório de certificados somente na memória ao trabalhar com certificados que não precisam ser colocados no armazenamento permanente.  
  
 declarações  
 Informações passadas de uma entidade para outra usada para estabelecer a identidade do remetente. Por exemplo, um token de nome de usuário e senha ou um certificado X. 509.  
  
 certificado do cliente  
 Refere-se a um certificado usado para autenticação de cliente, como autenticar um navegador da Web em um servidor Web. Quando um cliente de navegador da Web tenta acessar um servidor Web seguro, o cliente envia seu certificado ao servidor para permitir que ele verifique a identidade do cliente.  
  
 credenciais  
 Os dados de logon autenticados anteriormente que uma entidade de segurança usa para estabelecer sua própria identidade, como uma senha ou um tíquete de protocolo Kerberos. As credenciais são usadas para controlar o acesso aos recursos.  
  
 dados resumidos  
 Um tipo de conteúdo de dados definido pelo padrão de criptografia de chave pública (PKCS) #7 que consiste em qualquer tipo de dados mais um hash de mensagem (Digest) do conteúdo.  
  
 assinatura digital  
 Dados que associam a identidade de um remetente às informações que estão sendo enviadas. Uma assinatura digital pode ser agrupada com qualquer mensagem, arquivo ou outra informação codificada digitalmente ou transmitida separadamente. As assinaturas digitais são usadas em ambientes de chave pública e fornecem serviços de autenticação e integridade.  
  
 codificando  
 O processo de transformar dados em um fluxo de bits. A codificação faz parte do processo de serialização que converte dados em um fluxo de uns e zeros.  
  
 par de chaves do Exchange  
 Um par de chaves pública/privada usado para criptografar chaves de sessão para que elas possam ser armazenadas e trocadas com segurança com outros usuários.  
  
 hash  
 Um valor numérico de tamanho fixo obtido pela aplicação de uma função matemática (consulte algoritmo de hash) a uma quantidade arbitrária de dados. Os dados normalmente incluem dados aleatórios, conhecidos como um *nonce*. O serviço e o cliente contribuem com os nonces do Exchange para aumentar a complexidade do resultado. O resultado também é conhecido como um *Resumo de mensagem*. O envio de um valor de hash é mais seguro do que enviar dados confidenciais, como uma senha, mesmo que a senha seja criptografada. O remetente e o receptor de hash devem concordar com o algoritmo de hash e os nonces para que, uma vez recebido, um hash possa ser verificado.  
  
 algoritmo de hash  
 Um algoritmo usado para produzir um valor de hash de algum dado, como uma mensagem ou chave de sessão. Os algoritmos de hash típicos incluem MD2, MD4, MD5 e SHA-1.  
  
 Protocolo Kerberos  
 Um protocolo que define como os clientes interagem com um serviço de autenticação de rede. Os clientes obtêm tíquetes do centro de distribuição de chaves do Kerberos (KDC) e apresentam esses tíquetes para servidores quando as conexões são estabelecidas. Os tíquetes Kerberos representam as credenciais de rede do cliente.  
  
 autoridade de segurança local (LSA)  
 Um subsistema protegido que autentica e faz logon de usuários no sistema local. O LSA também mantém informações sobre todos os aspectos da segurança local em um sistema, coletivamente conhecido como a diretiva de segurança local do sistema.  
  
 Negotiate  
 Um SSP (provedor de suporte de segurança) que atua como uma camada de aplicativo entre a interface do provedor de suporte de segurança (SSPI) e os outros SSPs. Quando um aplicativo chama o SSPI para fazer logon em uma rede, ele pode especificar um SSP para processar a solicitação. Se o aplicativo especificar `Negotiate` , o `Negotiate` analisará a solicitação e escolherá o melhor SSP para lidar com a solicitação com base na política de segurança configurada pelo cliente.  
  
 nonce  
 Um valor gerado aleatoriamente usado para anular ataques de "reprodução".  
  
 Não-repúdio  
 A capacidade de identificar os usuários que executaram determinadas ações, portanto, irrefutably o contador de quaisquer tentativas feitas por um usuário para negar a responsabilidade. Por exemplo, um sistema pode registrar em log a ID de um usuário sempre que um arquivo for excluído.  
  
 PKCS (padrão de criptografia de chave pública)  
 Especificações produzidas pela RSA Data Security, Inc. em cooperação com desenvolvedores de sistemas seguros em todo o mundo para acelerar a implantação da criptografia de chave pública.  
  
 #7 PKCS  
 O padrão de sintaxe de mensagem criptográfica. Uma sintaxe geral de dados para os quais a criptografia pode ser aplicada, como assinaturas digitais e criptografia. Ele também fornece a sintaxe para a disseminação de certificados ou listas de certificados revogados e outros atributos de mensagem, como carimbos de data/hora, para a mensagem.  
  
 texto não criptografado  
 Uma mensagem que não está criptografada. As mensagens em texto não criptografado são, às vezes, chamadas de mensagens não *criptografadas*  
  
 privilege  
 O direito de um usuário executar várias operações relacionadas ao sistema, como desligar o sistema, carregar drivers de dispositivo ou alterar a hora do sistema. O token de acesso de um usuário contém uma lista dos privilégios que o usuário ou os grupos do usuário mantêm.  
  
 chave privada  
 A metade secreta de um par de chaves usado em um algoritmo de chave pública. As chaves privadas normalmente são usadas para criptografar uma chave de sessão simétrica, assinar digitalmente uma mensagem ou descriptografar uma mensagem que foi criptografada com a chave pública correspondente. Consulte também "chave pública".  
  
 process  
 O contexto de segurança no qual um aplicativo é executado. Normalmente, o contexto de segurança é associado a um usuário, de modo que todos os aplicativos em execução em um determinado processo assumem as permissões e os privilégios do usuário proprietário.  
  
 par de chaves pública/privada  
 Um conjunto de chaves de criptografia usadas para criptografia de chave pública. Para cada usuário, um CSP (provedor de serviços de criptografia) geralmente mantém dois pares de chaves públicas/privadas: um par de chaves do Exchange e um par de chaves de assinatura digital. Os dois pares de chaves são mantidos da sessão para a sessão.  
  
 chave pública  
 Uma chave criptográfica normalmente usada ao descriptografar uma chave de sessão ou uma assinatura digital. A chave pública também pode ser usada para criptografar uma mensagem, garantindo que apenas a pessoa com a chave privada correspondente possa descriptografar a mensagem.  
  
 criptografia de chave pública  
 Criptografia que usa um par de chaves, uma chave para criptografar dados e a outra chave para descriptografar dados. Em contraste, os algoritmos de criptografia simétrica que usam a mesma chave para criptografia e descriptografia. Na prática, a criptografia de chave pública normalmente é usada para proteger a chave de sessão que um algoritmo de criptografia simétrica usa. Nesse caso, a chave pública é usada para criptografar a chave de sessão, que, por sua vez, foi usada para criptografar alguns dados, e a chave privada é usada para descriptografia. Além de proteger as chaves de sessão, a criptografia de chave pública também pode ser usada para assinar digitalmente uma mensagem (usando a chave privada) e validar a assinatura (usando a chave pública).  
  
 PKI (infraestrutura de chave pública)  
 Uma infraestrutura que fornece um conjunto integrado de serviços e ferramentas administrativas para criar, implantar e gerenciar aplicativos de chave pública.  
  
 repúdio  
 A capacidade de um usuário de negar falsamente de ter executado uma ação enquanto outras partes não podem provar o contrário. Por exemplo, um usuário que exclui um arquivo e que pode negar com êxito fazer isso.  
  
 autoridade raiz  
 A AC na parte superior de uma hierarquia de autoridade de certificação. A autoridade raiz certifica CAs no próximo nível da hierarquia.  
  
 SHA (algoritmo de hash seguro)  
 Um algoritmo de hash que gera um resumo de mensagem. O SHA é usado com o algoritmo de assinatura digital (DSA) no padrão de assinatura digital (DSS), entre outros locais. Há quatro variedades de SHA: SHA-1, SHA-256, SHA-384 e SHA-512. O SHA-1 gera um resumo de mensagens de 160 bits. SHA-256, SHA-384 e SHA-512 geram resumos de mensagens de 256 bits, de 384 bits e de 512 bits, respectivamente. O SHA foi desenvolvido pelo National Institute of Standards and Technology (NIST) e pela NSA (National Security Agency).  
  
 protocolo SSL  
 Um protocolo para comunicações de rede seguras usando uma combinação de tecnologia de chave pública e secreta.  
  
 contexto de segurança  
 Os atributos de segurança ou as regras que estão em vigor no momento. Por exemplo, o usuário atual fez logon no computador ou o número de identificação pessoal inserido pelo usuário do cartão inteligente. Para SSPI, um contexto de segurança é uma estrutura de dados opaca que contém dados de segurança relevantes para uma conexão, como uma chave de sessão ou uma indicação da duração da sessão.  
  
 entidade de segurança  
 Uma entidade reconhecida pelo sistema de segurança. As entidades podem incluir usuários humanos, bem como processos autônomos.  
  
 provedor de suporte de segurança (SSP)  
 Uma DLL (biblioteca de vínculo dinâmico) que implementa o SSPI, disponibilizando um ou mais pacotes de segurança para os aplicativos. Cada pacote de segurança fornece mapeamentos entre as chamadas de função SSPI de um aplicativo e as funções de um modelo de segurança real. Os pacotes de segurança dão suporte a protocolos de segurança como a autenticação Kerberos e o LanMan (Microsoft LAN Manager).  
  
 SSPI (interface do provedor de suporte de segurança)  
 Uma interface comum entre aplicativos de nível de transporte, como a RPC (chamada de procedimento remoto) da Microsoft e provedores de segurança, como a segurança distribuída do Windows. O SSPI permite que um aplicativo de transporte chame um de vários provedores de segurança para obter uma conexão autenticada. Essas chamadas não exigem conhecimento extensivo dos detalhes do protocolo de segurança.  
  
 serviço de token de segurança  
 Serviços projetados para emitir e gerenciar tokens de segurança personalizados (tokens emitidos) em um cenário de multiatendimento. Os tokens personalizados geralmente são tokens SAML (Security Asserties Markup Language) que incluem uma credencial personalizada.  
  
 certificado do servidor  
 Refere-se a um certificado usado para autenticação de servidor, como autenticar um servidor Web em um navegador da Web. Quando um cliente de navegador da Web tenta acessar um servidor Web seguro, o servidor envia seu certificado ao navegador para permitir que ele verifique a identidade do servidor.  
  
 sessão  
 Uma troca de mensagens sob a proteção de uma única peça do material de chaveamento. Por exemplo, as sessões SSL usam uma única chave para enviar várias mensagens por essa chave.  
  
 chave de sessão  
 Uma chave gerada aleatoriamente que é usada uma vez e, em seguida, descartada. As chaves de sessão são simétricas (usadas para criptografia e descriptografia). Eles são enviados com a mensagem, protegidos por criptografia com uma chave pública do destinatário pretendido. Uma chave de sessão consiste em um número aleatório de aproximadamente 40 a 2.000 bits.  
  
 credenciais complementares  
 Credenciais para uso na autenticação de uma entidade de segurança para domínios de segurança externa.  
  
 criptografia simétrica  
 Criptografia que usa uma chave única para criptografia e descriptografia. A criptografia simétrica é preferida ao criptografar grandes quantidades de dados. Alguns dos algoritmos de criptografia simétrica mais comuns são RC2, RC4 e DES (Data Encryption Standard).  
  
 Consulte também "criptografia de chave pública".  
  
 chave simétrica  
 Uma única chave usada para criptografia e descriptografia. As chaves de sessão geralmente são simétricas.  
  
 token (token de acesso)  
 Um token de acesso contém as informações de segurança para uma sessão de logon. O sistema cria um token de acesso quando um usuário faz logon e cada processo executado em nome do usuário tem uma cópia do token. O token identifica o usuário, os grupos do usuário e os privilégios do usuário. O sistema usa o token para controlar o acesso a objetos protegíveis e controlar a capacidade do usuário de executar várias operações relacionadas ao sistema no computador local. Há dois tipos de tokens de acesso, primário e representação.  
  
 camada de transporte  
 A camada de rede que é responsável por qualidade de serviço e entrega precisa de informações. Entre as tarefas executadas nesta camada estão a detecção de erros e a correção.  
  
 lista de confiança (lista de certificados confiáveis ou CTL)  
 Uma lista predefinida de itens que foram assinados por uma entidade confiável. Uma CTL pode ser qualquer coisa, como uma lista de hashes de certificados ou uma lista de nomes de arquivos. Todos os itens da lista são autenticados (aprovados) pela entidade de assinatura.  
  
 provedor de confiança  
 O software que decide se um determinado arquivo é confiável. Essa decisão se baseia no certificado associado ao arquivo.  
  
 nome principal do usuário (UPN)  
 Um nome de conta de usuário (às vezes chamado de *nome de logon do usuário*) e um nome de domínio que identifica o domínio no qual a conta de usuário está localizada. Esse é o uso padrão para fazer logon em um domínio do Windows. O formato é: someone@example.com (como para um endereço de email).  
  
> [!NOTE]
> Além do formulário UPN padrão, o WCF aceita UPNs no formato de nível inferior, por exemplo, cohowinery. com\someone.  
  
 X.509  
 Um padrão reconhecido internacionalmente para certificados que define suas partes necessárias.  
  
## <a name="see-also"></a>Consulte também

- [Conceitos fundamentais do Windows Communication Foundation](../fundamental-concepts.md)
- [Conceitos de segurança](security-concepts.md)
- [Modelo de segurança para o Windows Server app Fabric](https://docs.microsoft.com/previous-versions/appfabric/ee677202(v=azure.10))
