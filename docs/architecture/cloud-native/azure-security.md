---
title: Segurança do Azure para aplicativos nativos de nuvem
description: Arquitetando aplicativos .NET nativos da nuvem para o Azure | Segurança do Azure para aplicativos nativos de nuvem
ms.date: 05/13/2020
ms.openlocfilehash: 996c7075b252466a3b3374f1e75e64315fdd6fc7
ms.sourcegitcommit: 8bfeb5930ca48b2ee6053f16082dcaf24d46d221
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/18/2020
ms.locfileid: "88557640"
---
# <a name="azure-security-for-cloud-native-apps"></a>Segurança do Azure para aplicativos nativos de nuvem

Os aplicativos nativos de nuvem podem ser mais fáceis e difíceis de proteger do que os aplicativos tradicionais. Na desvantagem, você precisa proteger aplicativos mais menores e dedicar mais energia para criar a infraestrutura de segurança. A natureza heterogênea de linguagens de programação e estilos na maioria das implantações de serviço também significa que você precisa prestar mais atenção aos boletins de segurança de vários provedores diferentes.

No lado do inverso, serviços menores, cada um com seu próprio armazenamento de dados, limitam o escopo de um ataque. Se um invasor comprometer um sistema, provavelmente será mais difícil para o invasor fazer o salto para outro sistema do que ele está em um aplicativo monolítico. Limites de processo são limites fortes. Além disso, se um backup de banco de dados vazar, o dano será mais limitado, pois esse banco de dado contém apenas um subconjunto de informações e é improvável que ele contenha dados pessoais.

## <a name="threat-modeling"></a>Modelagem de ameaças

Não importa se as vantagens superam as desvantagens dos aplicativos nativos de nuvem, a mesma mentalidade de segurança holística deve ser seguida. A segurança e o pensamento seguro devem fazer parte de cada etapa da história de desenvolvimento e de operações. Ao planejar um aplicativo, faça perguntas como:

- Qual seria o impacto dessa perda de dados?
- Como é possível limitar os danos dos dados inválidos que estão sendo injetados nesse serviço?
- Quem deve ter acesso a esses dados?
- Há políticas de auditoria em vigor em relação ao processo de desenvolvimento e versão?

Todas essas perguntas fazem parte de um processo chamado [modelagem de ameaças](https://docs.microsoft.com/azure/security/azure-security-threat-modeling-tool). Esse processo tenta responder à pergunta de quais ameaças existem no sistema, quão prováveis são as ameaças e os possíveis danos deles.

Depois que a lista de ameaças tiver sido estabelecida, você precisará decidir se vale a pena mitigar. Às vezes, uma ameaça é tão improvável e dispendiosa de planejar que não vale a pena gastar energia. Por exemplo, algum ator de nível de Estado poderia injetar alterações no design de um processo que é usado por milhões de dispositivos. Agora, em vez de executar um determinado trecho de código no [anel 3](https://en.wikipedia.org/wiki/Protection_ring), esse código é executado no anel 0. Isso permite uma exploração que pode ignorar o hipervisor e executar o código de ataque nos computadores bare-metal, permitindo ataques em todas as máquinas virtuais em execução nesse hardware.

Os processadores alterados são difíceis de detectar sem uma vigiados e um conhecimento avançado do design de silício do processador. Esse cenário é improvável de ocorrer e caro de atenuar, portanto, provavelmente nenhum modelo de ameaça recomendaria criar proteção de exploração para ele.

As ameaças mais prováveis, como controles de acesso desfeitos que permitam `Id` a incrementação de ataques (substituindo `Id=2` com `Id=3` na URL) ou injeção de SQL, são mais atraentes para a criação de proteções. As mitigações para essas ameaças são bastante razoáveis para criar e evitar brechas de segurança constrangedoras que mancham a reputação da empresa.

## <a name="principle-of-least-privilege"></a>Princípio de privilégios mínimos

Uma das ideias encontradas na segurança do computador é o princípio de privilégios mínimos (POLP). Na verdade, é uma ideia fundamental na maioria das formas de segurança ser digital ou física. Em suma, o princípio é que qualquer usuário ou processo deve ter o menor número de direitos possíveis para executar sua tarefa.

Como exemplo, imagine os informativos em um banco: o acesso à segurança é uma atividade incomum. Portanto, a forma média não pode abrir a própria segurança. Para obter acesso, eles precisam escalonar sua solicitação por meio de um gerente bancário, que realiza verificações de segurança adicionais.

Em um sistema de computador, um exemplo fantástico é o direito de um usuário se conectar a um banco de dados. Em muitos casos, há uma única conta de usuário usada para compilar a estrutura do banco de dados e executar o aplicativo. Exceto em casos extremos, a conta que executa o aplicativo não precisa da capacidade de atualizar informações de esquema. Deve haver várias contas que forneçam níveis de privilégio diferentes. O aplicativo deve usar apenas o nível de permissão que concede acesso de leitura e gravação aos dados nas tabelas. Esse tipo de proteção elimina ataques que visavam descartar tabelas de bancos de dados ou introduzir gatilhos mal-intencionados.

Quase todas as partes da criação de um aplicativo nativo de nuvem podem se beneficiar da memorização do princípio de privilégios mínimos. Você pode encontrá-lo em jogo ao configurar firewalls, grupos de segurança de rede, funções e escopos no RBAC (controle de acesso baseado em função).

## <a name="penetration-testing"></a>Testes de penetração

À medida que os aplicativos se tornam mais complicados, o número de vetores de ataque aumenta em uma taxa alarmante. A modelagem de ameaças tem falhas no que tende a ser executada pelas mesmas pessoas que estão compilando o sistema. Da mesma forma que muitos desenvolvedores têm problemas para prever as interações do usuário e, em seguida, criar interfaces de usuário inutilizáveis, a maioria dos desenvolvedores tem dificuldade em ver cada vetor de ataque. Também é possível que os desenvolvedores que criam o sistema não estejam bem familiarizados com metodologias de ataque e percam algo crucial.

Teste de penetração ou "teste de caneta" envolve trazer atores externos para tentar atacar o sistema. Esses invasores podem ser uma empresa de consultoria externa ou outros desenvolvedores com um bom conhecimento de segurança de outra parte do negócio. Eles recebem Blanche carte para tentar subverter o sistema. Frequentemente, eles encontrarão grandes buracos de segurança que precisam ser corrigidos. Às vezes, o vetor de ataque será algo totalmente inesperado, como explorar um ataque de phishing contra o CEO.

O Azure em si está constantemente passando por ataques de uma [equipe de hackers dentro da Microsoft](https://azure.microsoft.com/resources/videos/red-vs-blue-internal-security-penetration-testing-of-microsoft-azure/). Ao longo dos anos, eles foram os primeiros a encontrar dezenas de vetores de ataque potencialmente catastróficos, fechando-os antes que possam ser explorados externamente. Quanto mais tentador for um destino, mais provável que os atores eternas tentarão explorá-lo e que há alguns destinos no mundo mais tentadores do que o Azure.

## <a name="monitoring"></a>Monitoramento

Se um invasor tentar penetrar em um aplicativo, deve haver algum aviso. Frequentemente, os ataques podem ser exportados examinando os logs dos serviços. Os ataques deixam os sinais de indícios que podem ser excedidos antes de terem sucesso. Por exemplo, um invasor tentando adivinhar uma senha fará muitas solicitações em um sistema de logon. O monitoramento em torno do sistema de logon pode detectar padrões estranhos que estão fora de linha com o padrão de acesso típico. Esse monitoramento pode ser transformado em um alerta que pode, por sua vez, alertar uma pessoa de operações para ativar algum tipo de contramedida. Um sistema de monitoramento altamente maduro pode até tomar medidas com base nesses desvios, adicionando regras proativamente para bloquear solicitações ou limitar respostas.

## <a name="securing-the-build"></a>Protegendo a compilação

Um local em que a segurança é muitas vezes ignorada em relação ao processo de compilação. O Build não só deve executar verificações de segurança, como a verificação de código inseguro ou credenciais com check-in, mas a compilação em si deve ser segura. Se o servidor de compilação estiver comprometido, ele fornecerá um vetor fantástico para introduzir código arbitrário no produto.

Imagine que um invasor esteja procurando roubar as senhas de pessoas que se conectam a um aplicativo Web. Eles poderiam introduzir uma etapa de compilação que modifica o código com check-out para espelhar qualquer solicitação de logon para outro servidor. Na próxima vez que o código passar pela compilação, ele será atualizado silenciosamente. A verificação da vulnerabilidade do código-fonte não detectará isso à medida que ela for executada antes da compilação. Igualmente, ninguém vai capturá-lo em uma revisão de código porque as etapas de compilação residem no servidor de compilação. O código explorado vai para a produção onde ele pode coletar senhas. Provavelmente, não há nenhum log de auditoria das alterações do processo de compilação ou pelo menos ninguém monitorando a auditoria.

Este é um exemplo perfeito de um destino aparentemente pouco baixo que pode ser usado para dividir o sistema. Quando um invasor viola o perímetro do sistema, ele pode começar a trabalhar com a localização de maneiras de elevar suas permissões até o ponto que elas podem causar danos reais em qualquer lugar.

## <a name="building-secure-code"></a>Compilando código seguro

.NET Framework já é uma estrutura bastante segura. Ele evita algumas das armadilhas de código não gerenciado, como a retirada das extremidades das matrizes. O trabalho é realizado ativamente para corrigir brechas de segurança à medida que são descobertos. Há até mesmo um [programa de Bounty de bugs](https://www.microsoft.com/msrc/bounty) que paga pesquisadores para encontrar problemas na estrutura e relatá-los em vez de explorá-los.

Há muitas maneiras de tornar o código .NET mais seguro. As diretrizes a seguir, como o artigo [diretrizes de codificação de segurança para .net](../../standard/security/secure-coding-guidelines.md) , são uma etapa razoável para garantir que o código seja protegido do zero. O [OWASP Top 10](https://owasp.org/www-project-top-ten/) é outro guia inestimável para criar código seguro.

O processo de compilação é um bom lugar para colocar as ferramentas de verificação para detectar problemas no código-fonte antes de colocá-lo em produção. A maioria de cada projeto tem dependências em alguns outros pacotes. Uma ferramenta que pode examinar pacotes desatualizados detectará problemas em uma compilação noturna. Mesmo ao criar imagens do Docker, é útil verificar e garantir que a imagem base não tenha vulnerabilidades conhecidas. Outra coisa a ser verificada é que ninguém verificou as credenciais acidentalmente.

## <a name="built-in-security"></a>Segurança interna

O Azure foi projetado para balancear a usabilidade e a segurança para a maioria dos usuários. Diferentes usuários terão requisitos de segurança diferentes e, portanto, precisarão ajustar sua abordagem à segurança da nuvem. A Microsoft publica uma grande quantidade de informações de segurança na [central de confiabilidade](https://azure.microsoft.com/support/trust-center/). Esse recurso deve ser a primeira parada para os profissionais interessados em entender como funcionam as tecnologias internas de mitigação de ataques.

No portal do Azure, o assistente [do Azure](https://azure.microsoft.com/services/advisor/) é um sistema que está verificando constantemente um ambiente e fazendo recomendações. Algumas dessas recomendações foram projetadas para economizar dinheiro dos usuários, mas outras são projetadas para identificar configurações potencialmente inseguras, como ter um contêiner de armazenamento aberto para o mundo e não protegidas por uma rede virtual.

## <a name="azure-network-infrastructure"></a>Infraestrutura de rede do Azure

Em um ambiente de implantação local, uma grande quantidade de energia é dedicada à configuração da rede. Configurar roteadores, comutadores e, como tal, é um trabalho complicado. As redes permitem que determinados recursos se comuniquem com outros recursos e impeçam o acesso em alguns casos. Uma regra de rede frequente é restringir o acesso ao ambiente de produção do ambiente de desenvolvimento por meio da possibilidade de que uma parte de código desenvolvida pela metade seja executada errado e exclua um lançamento de dados.

Na caixa, a maioria dos recursos de PaaS do Azure tem apenas a configuração de rede mais básica e permissiva. Por exemplo, qualquer pessoa na Internet pode acessar um serviço de aplicativo. Novas instâncias de SQL Server normalmente vêm restritas, para que as partes externas não possam acessá-las, mas os intervalos de endereços IP usados pelo próprio Azure são permitidos por meio do. Portanto, embora o SQL Server seja protegido contra ameaças externas, um invasor só precisa configurar um bridgehead do Azure do qual eles podem iniciar ataques em todas as instâncias do SQL no Azure.

Felizmente, a maioria dos recursos do Azure pode ser colocada em uma rede virtual do Azure que permite um controle de acesso mais preciso. De forma semelhante à maneira como as redes locais estabelecem redes privadas que são protegidas do mundo mais amplo, as redes virtuais são ilhas de endereços IP privados localizados na rede do Azure.

![Figura 9-1 uma rede virtual no Azure](./media/virtual-network.png)

**Figura 9-1**. Uma rede virtual no Azure.

Da mesma forma que as redes locais têm um firewall que rege o acesso à rede, você pode estabelecer um firewall semelhante no limite da rede virtual. Por padrão, todos os recursos em uma rede virtual ainda podem se comunicar com a Internet. São apenas conexões de entrada que exigem alguma forma de exceção de firewall explícita.

Com a rede estabelecida, recursos internos, como contas de armazenamento, podem ser configurados para permitir apenas o acesso por recursos que também estão na rede virtual. Esse firewall fornece um nível extra de segurança, caso as chaves dessa conta de armazenamento sejam vazadas, os invasores não conseguirão se conectar a ela para explorar as chaves vazadas. Este é outro exemplo do princípio de privilégios mínimos.

Os nós em um cluster kubernetes do Azure podem participar de uma rede virtual, assim como outros recursos mais nativos do Azure. Essa funcionalidade é chamada de [interface de rede de contêiner do Azure](https://github.com/Azure/azure-container-networking/blob/master/docs/cni.md). Na verdade, ele aloca uma sub-rede dentro da rede virtual na qual as máquinas virtuais e as imagens de contêiner são distribuídas.

Continuando o caminho para ilustrar o princípio de menos privilégios, nem todos os recursos em uma rede virtual precisam se comunicar com todos os outros recursos. Por exemplo, em um aplicativo que fornece uma API Web em uma conta de armazenamento e um banco de dados SQL, é improvável que o banco de dados e a conta de armazenamento precisem se comunicar entre si. Qualquer compartilhamento de dados entre eles passaria pelo aplicativo Web. Assim, um [NSG (grupo de segurança de rede)](https://docs.microsoft.com/azure/virtual-network/security-overview) poderia ser usado para negar o tráfego entre os dois serviços.

Uma política de negação de comunicação entre recursos pode ser irritante de ser implementada, especialmente proveniente de uma experiência de uso do Azure sem restrições de tráfego. Em algumas outras nuvens, o conceito de grupos de segurança de rede é muito mais predominante. Por exemplo, a política padrão em AWS é que os recursos não podem se comunicar entre si até serem habilitados por regras em um NSG. Embora seja mais lento para desenvolver isso, um ambiente mais restritivo fornece um padrão mais seguro. Fazer uso de práticas DevOps adequadas, especialmente usando [Azure Resource Manager ou Terraform](infrastructure-as-code.md) para gerenciar permissões pode facilitar o controle das regras.

As redes virtuais também podem ser úteis ao configurar a comunicação entre recursos locais e na nuvem. Uma rede virtual privada pode ser usada para conectar diretamente as duas redes. Isso permite a execução de uma rede virtual sem qualquer tipo de gateway para cenários em que todos os usuários estejam no local. Há várias tecnologias que podem ser usadas para estabelecer essa rede. A mais simples é usar uma [VPN site a site](https://docs.microsoft.com/azure/vpn-gateway/vpn-gateway-about-vpngateways?toc=%2fazure%2fvirtual-network%2ftoc.json#s2smulti) que possa ser estabelecida entre vários roteadores e o Azure. O tráfego é criptografado e encapsulado pela Internet no mesmo custo por byte que qualquer outro tráfego. Para cenários em que mais largura de banda ou mais segurança são desejáveis, o Azure oferece um serviço chamado [rota expressa](https://docs.microsoft.com/azure/vpn-gateway/vpn-gateway-about-vpngateways?toc=%2fazure%2fvirtual-network%2ftoc.json#ExpressRoute) que usa um circuito privado entre uma rede local e o Azure. É mais dispendioso e difícil de estabelecer, mas também mais seguro.

## <a name="role-based-access-control-for-restricting-access-to-azure-resources"></a>Controle de acesso baseado em função para restringir o acesso aos recursos do Azure

O RBAC é um sistema que fornece uma identidade para aplicativos em execução no Azure. Os aplicativos podem acessar recursos usando essa identidade em vez de ou além de usar chaves ou senhas.

## <a name="security-principals"></a>Entidades de segurança

O primeiro componente no RBAC é uma entidade de segurança. Uma entidade de segurança pode ser um usuário, grupo, entidade de serviço ou identidade gerenciada.

![Figura 9-2 tipos diferentes de entidades de segurança](./media/rbac-security-principal.png)

**Figura 9-2**. Tipos diferentes de entidades de segurança.

- Usuário-qualquer usuário que tenha uma conta no Azure Active Directory é um usuário.
- Grupo-uma coleção de usuários de Azure Active Directory. Como membro de um grupo, um usuário assume as funções desse grupo, além de seus próprios.
- Entidade de serviço-uma identidade de segurança na qual os serviços ou aplicativos são executados.
- Identidade gerenciada-uma identidade de Azure Active Directory gerenciada pelo Azure. Identidades gerenciadas são normalmente usadas ao desenvolver aplicativos de nuvem que gerenciam as credenciais para autenticação nos serviços do Azure.

A entidade de segurança pode ser aplicada à maioria dos recursos. Isso significa que é possível atribuir uma entidade de segurança a um contêiner em execução no Azure kubernetes, permitindo que ele acesse os segredos armazenados no Key Vault. Uma função do Azure pode assumir uma permissão que permite que ele se comunique com uma instância de Active Directory para validar um JWT para um usuário de chamada. Depois que os serviços são habilitados com uma entidade de serviço, suas permissões podem ser gerenciadas de maneira granular usando funções e escopos.

## <a name="roles"></a>Funções

Uma entidade de segurança pode assumir muitas funções ou, usando uma analogia mais sartorial, ter muitas chapéus. Cada função define uma série de permissões, como "ler mensagens do ponto de extremidade do barramento de serviço do Azure". O conjunto de permissões efetivas de uma entidade de segurança é a combinação de todas as permissões atribuídas a todas as funções que a entidade de segurança tem. O Azure tem um grande número de funções internas e os usuários podem definir suas próprias funções.

![Figura 9-3 definições de função do RBAC](./media/rbac-role-definition.png)

**Figura 9-3**. Definições de função RBAC.

O integrado ao Azure também é uma série de funções de alto nível, como proprietário, colaborador, leitor e administrador de conta de usuário. Com a função de proprietário, uma entidade de segurança pode acessar todos os recursos e atribuir permissões a outros. Um colaborador tem o mesmo nível de acesso a todos os recursos, mas não pode atribuir permissões. Um leitor só pode exibir recursos existentes do Azure e um administrador de conta de usuário pode gerenciar o acesso aos recursos do Azure.

Funções internas mais granulares, como [colaborador de zona DNS](https://docs.microsoft.com/azure/role-based-access-control/built-in-roles#dns-zone-contributor) , têm direitos limitados a um único serviço. As entidades de segurança podem assumir qualquer número de funções.

## <a name="scopes"></a>Escopos

As funções podem ser aplicadas a um conjunto restrito de recursos no Azure. Por exemplo, aplicando o escopo ao exemplo anterior de leitura de uma fila do barramento de serviço, você pode restringir a permissão a uma única fila: "ler mensagens do ponto de extremidade do barramento de serviço do Azure `blah.servicebus.windows.net/queue1` "

O escopo pode ser tão estreito quanto um único recurso ou pode ser aplicado a um grupo de recursos, assinatura ou até mesmo grupo de gerenciamento inteiro.

Ao testar se uma entidade de segurança tem uma determinada permissão, a combinação de função e escopo é levada em conta. Essa combinação fornece um poderoso mecanismo de autorização.

## <a name="deny"></a>Negar

Anteriormente, apenas as regras "Allow" eram permitidas para o RBAC. Esse comportamento fez com que alguns escopos sejam complicados de criar. Por exemplo, permitir acesso de entidade de segurança a todas as contas de armazenamento, exceto uma necessária, concedendo permissão explícita a uma lista potencialmente infinita de contas de armazenamento. Toda vez que uma nova conta de armazenamento foi criada, ela teria que ser adicionada a essa lista de contas. Essa sobrecarga de gerenciamento adicional que certamente não era desejável.

As regras de negação têm precedência sobre regras de permissão. Agora que representa o mesmo escopo "permitir todos, exceto um", pode ser representado como duas regras "permitir tudo" e "negar esta específica". As regras de negação não apenas facilitam o gerenciamento, mas permitem recursos que são mais seguros, negando acesso a todos.

## <a name="checking-access"></a>Verificando o acesso

Como você pode imaginar, ter um grande número de funções e escopos pode tornar a descoberta da permissão efetiva de uma entidade de serviço muito difícil. Empilhando regras de negação além disso, serve apenas para aumentar a complexidade. Felizmente, há uma calculadora de permissões que pode mostrar as permissões efetivas para qualquer entidade de serviço. Normalmente, ele é encontrado na guia IAM no portal, como mostra a Figura 10-3.

![Figura 9-4 calculadora de permissão para um serviço de aplicativo](./media/check-rbac.png)

**Figura 9-4**. Calculadora de permissão para um serviço de aplicativo.

## <a name="securing-secrets"></a>Protegendo segredos

Senhas e certificados são um vetor de ataque comum para invasores. O hardware de violação de senha pode fazer um ataque de força bruta e tentar adivinhar bilhões de senhas por segundo. Portanto, é importante que as senhas usadas para acessar os recursos sejam fortes, com uma grande variedade de caracteres. Essas senhas são exatamente o tipo de senha que quase impossível de se lembrar. Felizmente, as senhas no Azure não precisam realmente ser conhecidas por nenhum humano.

Muitos especialistas em segurança [sugerem](https://www.troyhunt.com/password-managers-dont-have-to-be-perfect-they-just-have-to-be-better-than-not-having-one/) que usar um Gerenciador de senhas para manter suas próprias senhas é a melhor abordagem. Embora centralize suas senhas em um único local, ela também permite o uso de senhas altamente complexas e a garantia de que são exclusivas para cada conta. O mesmo sistema existe no Azure: um repositório central para segredos.

## <a name="azure-key-vault"></a>Cofre de Chave do Azure

Azure Key Vault fornece um local centralizado para armazenar senhas para coisas como bancos de dados, chaves de API e certificados. Depois que um segredo é inserido no cofre, ele nunca é mostrado novamente e os comandos para extraí-los e exibi-los são propositadamente complicados. As informações no seguro são protegidas usando os módulos de segurança de hardware validados por criptografia de software ou FIPS 140-2 nível 2.

O acesso ao cofre de chaves é fornecido por meio de RBACs, o que significa que não apenas qualquer usuário pode acessar as informações no cofre. Digamos que um aplicativo Web queira acessar a cadeia de conexão de banco de dados armazenada em Azure Key Vault. Para obter acesso, os aplicativos precisam ser executados usando uma entidade de serviço. Sob essa função assumida, eles podem ler os segredos do cofre. Há várias configurações de segurança diferentes que podem limitar ainda mais o acesso que um aplicativo tem ao cofre, para que ele não possa atualizar segredos, mas apenas lê-los.

O acesso ao cofre de chaves pode ser monitorado para garantir que apenas os aplicativos esperados estejam acessando o cofre. Os logs podem ser integrados de volta ao Azure Monitor, desbloqueando a capacidade de configurar alertas quando condições inesperadas forem encontradas.

## <a name="kubernetes"></a>Kubernetes

No kubernetes, há um serviço semelhante para manter pequenas partes de informações secretas. Os segredos do kubernetes podem ser definidos por meio do `kubectl` executável típico.

Criar um segredo é tão simples quanto localizar a versão Base64 dos valores a serem armazenados:

```console
echo -n 'admin' | base64
YWRtaW4=
echo -n '1f2d1e2e67df' | base64
MWYyZDFlMmU2N2Rm
```

Em seguida, adicione-o a um arquivo de segredos chamado, `secret.yml` por exemplo, semelhante ao exemplo a seguir:

```yml
apiVersion: v1
kind: Secret
metadata:
  name: mysecret
type: Opaque
data:
  username: YWRtaW4=
  password: MWYyZDFlMmU2N2Rm
```

Por fim, esse arquivo pode ser carregado em kubernetes executando o seguinte comando:

```console
kubectl apply -f ./secret.yaml
```

Esses segredos podem então ser montados em volumes ou expostos a processos de contêiner por meio de variáveis de ambiente. A abordagem de [aplicativo de doze fatores](https://12factor.net/) para a criação de aplicativos sugere o uso do menor denominador comum para transmitir configurações a um aplicativo. As variáveis de ambiente são o denominador mais baixo comum, pois elas são suportadas não importa o sistema operacional ou aplicativo.

Uma alternativa para usar os segredos internos do kubernetes é acessar os segredos em Azure Key Vault de dentro do kubernetes. A maneira mais simples de fazer isso é atribuir uma função de RBAC ao contêiner, buscando carregar segredos. O aplicativo pode usar as APIs de Azure Key Vault para acessar os segredos. No entanto, essa abordagem requer modificações no código e não segue o padrão de uso de variáveis de ambiente. Em vez disso, é possível injetar valores em um contêiner. Essa abordagem é, na verdade, mais segura do que usar os segredos do kubernetes diretamente, pois eles podem ser acessados por usuários no cluster.

## <a name="encryption-in-transit-and-at-rest"></a>Criptografia em trânsito e em repouso

Manter os dados seguros é importante se ele estiver em disco ou transitando entre vários serviços diferentes. A maneira mais eficaz de manter os dados contra vazamento é criptografá-los em um formato que não pode ser lido facilmente por outras pessoas. O Azure dá suporte a uma ampla variedade de opções de criptografia.

### <a name="in-transit"></a>Em trânsito

Há várias maneiras de criptografar o tráfego na rede no Azure. O acesso aos serviços do Azure normalmente é feito em conexões que usam TLS (Transport Layer Security). Por exemplo, todas as conexões com as APIs do Azure exigem conexões TLS. Igualmente, as conexões com pontos de extremidade no armazenamento do Azure podem ser restritas para trabalhar somente em conexões criptografadas por TLS.

O TLS é um protocolo complicado e simplesmente saber que a conexão está usando o TLS não é suficiente para garantir a segurança. Por exemplo, o TLS 1,0 é cronicamente inseguro e o TLS 1,1 não é muito melhor. Mesmo dentro das versões do TLS, há várias configurações que podem facilitar a descriptografia das conexões. A melhor ação é verificar e ver se a conexão do servidor está usando protocolos atualizados e bem configurados.

Essa verificação pode ser feita por um serviço externo, como o teste do servidor SSL dos laboratórios SSL. Uma execução de teste em um ponto de extremidade do Azure típico, nesse caso, um ponto de extremidade do barramento de serviço, produz uma pontuação quase perfeita de um.

Até mesmo serviços, como bancos de dados SQL do Azure, usam criptografia TLS para manter o dado oculto. A parte interessante sobre criptografar os dados em trânsito usando o TLS é que não é possível, mesmo para a Microsoft, escutar na conexão entre os computadores que executam o TLS. Isso deve ser confortável para as empresas que se preocupam que seus dados possam estar em risco da Microsoft apropriada ou até mesmo de um ator de estado com mais recursos do que o invasor padrão.

![Figura 9-5 relatório de laboratórios SSL mostrando uma pontuação de um ponto de extremidade do barramento de serviço.](./media/ssl-report.png)

**Figura 9-5**. Relatório de laboratórios de SSL mostrando uma pontuação de um ponto de extremidade do barramento de serviço.

Embora esse nível de criptografia não seja suficiente para todo o tempo, deve inspirar a confiança de que as conexões do Azure TLS são bem seguras. O Azure continuará a desenvolver seus padrões de segurança conforme a criptografia melhora. É bom saber que há alguém assistindo aos padrões de segurança e atualizando o Azure à medida que eles melhoram.

### <a name="at-rest"></a>Em repouso

Em qualquer aplicativo, há vários locais em que os dados são colocados no disco. O código do aplicativo em si é carregado de algum mecanismo de armazenamento. A maioria dos aplicativos também usa algum tipo de banco de dados, como SQL Server, Cosmos DB ou até mesmo o enorme armazenamento de tabelas eficiente em termos de preço. Todos esses bancos de dados usam armazenamento com criptografia intensa para garantir que ninguém diferente dos aplicativos com as permissões adequadas possa ler seus dados. Até mesmo os operadores do sistema não podem ler dados que foram criptografados. Assim, os clientes podem permanecer confiantes de que suas informações secretas permanecem secretas.

### <a name="storage"></a>Armazenamento

A base de grande parte do Azure é o mecanismo de armazenamento do Azure. Os discos de máquina virtual são montados sobre o armazenamento do Azure. Os serviços Kubernetess do Azure são executados em máquinas virtuais que, por sua conta, são hospedadas no armazenamento do Azure. Mesmo as tecnologias sem servidor, como Azure Functions aplicativos e instâncias de contêiner do Azure, ficam fora do disco que faz parte do armazenamento do Azure.

Se o armazenamento do Azure estiver bem criptografado, ele fornecerá uma base para que a maioria das outras coisas também seja criptografada. O armazenamento do Azure [é criptografado](https://docs.microsoft.com/azure/storage/common/storage-service-encryption) com o [AES de 256 bits](https://en.wikipedia.org/wiki/Advanced_Encryption_Standard)compatível com [FIPS 140-2](https://en.wikipedia.org/wiki/FIPS_140) . Essa é uma tecnologia de criptografia bem considerada que tem sido o assunto de uma ampla investigação acadêmica nos últimos 20 anos. No momento, não há nenhum ataque prático conhecido que permitisse alguém sem conhecimento da chave para ler os dados criptografados pelo AES.

Por padrão, as chaves usadas para criptografar o armazenamento do Azure são gerenciadas pela Microsoft. Há proteções extensivas em vigor para garantir o acesso mal-intencionado a essas chaves. No entanto, os usuários com requisitos de criptografia específicos também podem [fornecer suas próprias chaves de armazenamento](https://docs.microsoft.com/azure/storage/common/storage-encryption-keys-powershell) que são gerenciadas em Azure Key Vault. Essas chaves podem ser revogadas a qualquer momento, o que efetivamente renderizaria o conteúdo da conta de armazenamento usando-os inacessíveis.

As máquinas virtuais usam o armazenamento criptografado, mas é possível fornecer outra camada de criptografia usando tecnologias como BitLocker no Windows ou DM-cript no Linux. Essas tecnologias significam que, mesmo que a imagem do disco tenha sido vazada fora do armazenamento, ela permaneceria quase impossível de ler.

### <a name="azure-sql"></a>SQL do Azure

Os bancos de dados hospedados no SQL do Azure usam uma tecnologia chamada [Transparent Data Encryption (TDE)](/sql/relational-databases/security/encryption/transparent-data-encryption) para garantir que os dados permaneçam criptografados. Ele é habilitado por padrão em todos os bancos de dados SQL recém-criados, mas deve ser habilitado manualmente para bancos de dados herdados. O TDE executa criptografia e descriptografia em tempo real, não apenas o banco de dados, mas também os logs de transações e backups.

Os parâmetros de criptografia são armazenados no `master` banco de dados e, na inicialização, são lidos na memória para as operações restantes. Isso significa que o `master` banco de dados deve permanecer sem criptografia. A chave real é gerenciada pela Microsoft. No entanto, os usuários com requisitos de segurança de reação podem fornecer sua própria chave em Key Vault praticamente da mesma forma como é feito para o armazenamento do Azure. O Key Vault fornece serviços como rotação de chaves e revogação.

A parte "transparente" do TDS é proveniente do fato de que não há alterações do cliente necessárias para usar um banco de dados criptografado. Embora essa abordagem forneça uma boa segurança, o vazamento da senha do banco de dados é suficiente para que os usuários possam descriptografá-los. Há outra abordagem que criptografa colunas individuais ou tabelas em um banco de dados. [Always Encrypted](https://docs.microsoft.com/azure/sql-database/sql-database-always-encrypted-azure-key-vault) garante que, sem nenhum momento, os dados criptografados apareçam em texto sem formatação dentro do banco de dados.

A configuração dessa camada de criptografia requer a execução por meio de um assistente no SQL Server Management Studio para selecionar o tipo de criptografia e onde em Key Vault armazenar as chaves associadas.

![Figura 9-6 selecionando colunas em uma tabela a ser criptografada usando Always Encrypted](./media/always-encrypted.png)

**Figura 9-6**. Selecionando colunas em uma tabela a ser criptografada usando Always Encrypted.

Os aplicativos cliente que lêem informações dessas colunas criptografadas precisam fazer concessões especiais para ler dados criptografados. As cadeias de conexão precisam ser atualizadas com `Column Encryption Setting=Enabled` e as credenciais do cliente devem ser recuperadas do Key Vault. Em seguida, o cliente de SQL Server deve ser acessado com as chaves de criptografia de coluna. Quando isso for feito, as ações restantes usarão as interfaces padrão para o cliente SQL. Ou seja, ferramentas como Dapper e Entity Framework, que são criadas com base no cliente SQL, continuarão funcionando sem alterações. Always Encrypted pode ainda não estar disponível para cada driver de SQL Server em cada idioma.

A combinação de TDE e Always Encrypted, que pode ser usada com chaves específicas do cliente, garante que até mesmo os requisitos de criptografia mais exacionados têm suporte.

### <a name="cosmos-db"></a>Cosmos DB

Cosmos DB é o banco de dados mais recente fornecido pela Microsoft no Azure. Ele foi criado desde o início com a segurança e a criptografia em mente. A criptografia AES-256bit é padrão para todos os bancos de dados do Cosmos DB e não pode ser desabilitada. Junto com o requisito de TLS 1,2 para comunicação, toda a solução de armazenamento é criptografada.

![Figura 9-7 o fluxo de criptografia de dados no Cosmos DB](./media/cosmos-encryption.png)

**Figura 9-7**. O fluxo de criptografia de dados no Cosmos DB.

Embora Cosmos DB não forneça as chaves de criptografia do cliente, houve um trabalho significativo feito pela equipe para garantir que ela permaneça compatível com PCI-DSS sem isso. Cosmos DB também não dá suporte a nenhum tipo de criptografia de coluna única semelhante ao Always Encrypted do SQL do Azure ainda.

## <a name="keeping-secure"></a>Mantendo a segurança

O Azure tem todas as ferramentas necessárias para lançar um produto altamente seguro. No entanto, uma cadeia é tão forte quanto seu link mais fraco. Se os aplicativos implantados na parte superior do Azure não forem desenvolvidos com uma mentalidade de segurança adequada e boas auditorias de segurança, eles se tornarão o link fraco na cadeia. Há muitas excelentes ferramentas de análise estática, bibliotecas de criptografia e práticas de segurança que podem ser usadas para garantir que o software instalado no Azure seja tão seguro quanto o próprio Azure. Os exemplos incluem [ferramentas de análise estática](https://www.whitesourcesoftware.com/), [bibliotecas de criptografia](https://www.libressl.org/)e práticas de [segurança](https://azure.microsoft.com/resources/videos/red-vs-blue-internal-security-penetration-testing-of-microsoft-azure/).

>[!div class="step-by-step"]
>[Anterior](security.md) 
> [Avançar](devops.md)
