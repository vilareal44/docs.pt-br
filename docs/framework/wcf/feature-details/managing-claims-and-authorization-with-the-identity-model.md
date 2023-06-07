---
title: Gerenciamento de declarações e autorizações com o modelo de identidade
description: Saiba mais sobre os principais conceitos de programação para o modelo de identidade do WCF, um modelo baseado em declarações para executar a autorização.
ms.date: 03/30/2017
helpviewer_keywords:
- authorization [WCF]
- WCF security
- claims [WCF], managing with the Identity Model
- claims [WCF]
- authorization [WCF], managing with the Identity Model
ms.assetid: 099defbb-5d35-434e-9336-1a49b9ec7663
ms.openlocfilehash: 0d5687f8ac5021c008254f0f5cc453eda5e538c7
ms.sourcegitcommit: 358a28048f36a8dca39a9fe6e6ac1f1913acadd5
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/23/2020
ms.locfileid: "85245122"
---
# <a name="managing-claims-and-authorization-with-the-identity-model"></a>Gerenciamento de declarações e autorizações com o modelo de identidade
A autorização é o processo de determinar quais entidades têm permissão para alterar, exibir ou acessar um recurso de computador. Por exemplo, em uma empresa, somente os gerentes podem ter permissão para acessar os arquivos de seus funcionários. Windows Communication Foundation (WCF) dá suporte a dois mecanismos para executar o processamento de autorização. O primeiro mecanismo permite que você controle a autorização usando construções de Common Language Runtime (CLR) existentes. O segundo é um modelo baseado em declarações conhecido como *modelo de identidade*. O WCF usa o modelo de identidade para criar declarações de mensagens de entrada; Classes de modelo de identidade podem ser estendidas para dar suporte a novos tipos de declaração para esquemas de autorização personalizados. Este tópico apresenta uma visão geral dos principais conceitos de programação do recurso de modelo de identidade, bem como uma lista das classes mais importantes que o recurso usa.  
  
## <a name="identity-model-scenarios"></a>Cenários de modelo de identidade  
 Os cenários a seguir representam o uso do modelo de identidade.  
  
### <a name="scenario-1-supporting-identity-role-and-group-claims"></a>Cenário 1: suporte a declarações de identidade, função e grupo  
 Os usuários enviam mensagens para um serviço Web. Os requisitos de controle de acesso do serviço Web usam identidade, funções ou grupos. O remetente da mensagem é mapeado para um conjunto de funções ou grupos. As informações de função ou grupo são usadas para executar verificações de acesso.  
  
### <a name="scenario-2-supporting-rich-claims"></a>Cenário 2: suporte a declarações avançadas  
 Os usuários enviam mensagens para um serviço Web. Os requisitos de controle de acesso do serviço Web exigem um modelo mais rico do que a identidade, as funções ou os grupos. O serviço Web determina se um determinado usuário tem acesso a um recurso protegido específico usando o modelo rico baseado em declarações. Por exemplo, um usuário pode ser capaz de ler informações específicas, como informações de salário, que outros usuários não têm acesso.  
  
### <a name="scenario-3-mapping-disparate-claims"></a>Cenário 3: mapeando declarações distintas  
 Um usuário envia uma mensagem para um serviço Web. O usuário pode especificar suas credenciais de várias maneiras diferentes: certificado X. 509, token de nome de usuário ou token Kerberos. O serviço Web é necessário para executar verificações de controle de acesso da mesma forma, independentemente do tipo de credencial do usuário. Se tipos de credenciais adicionais tiverem suporte ao longo do tempo, o sistema deverá evoluir adequadamente.  
  
### <a name="scenario-4-determining-access-to-multiple-resources"></a>Cenário 4: determinando o acesso a vários recursos  
 Um serviço Web tenta acessar vários recursos. O serviço determina a quais recursos protegidos um determinado usuário tem acesso, comparando as declarações associadas ao usuário com as declarações necessárias para acessar o recurso.  
  
## <a name="identity-model-terms"></a>Termos do modelo de identidade  
 A lista a seguir define os principais termos usados para descrever os conceitos do modelo de identidade.  
  
 Política de autorização  
 Um conjunto de regras para mapear um conjunto de declarações de entrada para um conjunto de declarações de saída. Avaliar os resultados da política de autorização em conjuntos de declarações sendo adicionados a um contexto de avaliação e, subsequentemente, a um contexto de autorização.  
  
 Contexto de autorização  
 Um conjunto de conjuntos de declarações e zero ou mais propriedades. O resultado da avaliação de uma ou mais políticas de autorização.  
  
 Declaração  
 Uma combinação de um tipo de declaração, certo e um valor.  
  
 Conjunto de declarações  
 Um conjunto de declarações emitidas por um emissor específico.  
  
 Tipo de declaração  
 Um tipo de declaração. As declarações definidas pela API do modelo de identidade são propriedades da <xref:System.IdentityModel.Claims.Claim.ClaimType%2A> classe. Exemplos de tipos de declaração fornecidos pelo sistema são,,,,,,,,, <xref:System.IdentityModel.Claims.ClaimTypes.Dns%2A> <xref:System.IdentityModel.Claims.ClaimTypes.Email%2A> <xref:System.IdentityModel.Claims.ClaimTypes.Hash%2A> <xref:System.IdentityModel.Claims.ClaimTypes.Name%2A> <xref:System.IdentityModel.Claims.ClaimTypes.Rsa%2A> <xref:System.IdentityModel.Claims.ClaimTypes.Sid%2A> <xref:System.IdentityModel.Claims.ClaimTypes.Spn%2A> <xref:System.IdentityModel.Claims.ClaimTypes.System%2A> <xref:System.IdentityModel.Claims.ClaimTypes.Thumbprint%2A> <xref:System.IdentityModel.Claims.ClaimTypes.Uri%2A> e <xref:System.IdentityModel.Claims.ClaimTypes.X500DistinguishedName%2A> .  
  
 Contexto de avaliação  
 Um contexto no qual uma política de autorização é avaliada. Contém propriedades e conjuntos de declarações. Torna-se a base de um contexto de autorização quando a avaliação é concluída.  
  
 Declaração de identidade  
 Uma declaração cujo direito é identidade.  
  
 Emissor  
 Um conjunto de declarações que contém pelo menos uma declaração de identidade e é considerado que emitiu outro conjunto de declarações.  
  
 Propriedades  
 Um conjunto de informações associadas a um contexto de avaliação ou contexto de autorização.  
  
 Recurso protegido  
 Algo no sistema que só pode ser usado, acessado ou manipulado de outra forma se determinados requisitos forem atendidos pela primeira vez.  
  
 Direita  
 Uma funcionalidade sobre um recurso. Os direitos definidos pela API do modelo de identidade são propriedades da <xref:System.IdentityModel.Claims.Rights> classe. Exemplos de direitos fornecidos pelo sistema são <xref:System.IdentityModel.Claims.Rights.Identity%2A> e <xref:System.IdentityModel.Claims.Rights.PossessProperty%2A> .  
  
 Valor  
 Algo sobre o qual um direito é reivindicado.  
  
## <a name="claims"></a>Declarações  
 O modelo de identidade é um sistema baseado em declarações. As declarações descrevem os recursos associados a alguma entidade no sistema, geralmente um usuário desse sistema. O conjunto de declarações associadas a uma determinada entidade pode ser considerado uma chave. As declarações específicas definem a forma dessa chave, semelhante a uma chave física usada para abrir um bloqueio em uma porta. As declarações são usadas para obter acesso aos recursos. O acesso a um determinado recurso protegido é determinado pela comparação das declarações necessárias para acessar esse recurso com as declarações associadas à entidade que está tentando acessar.  
  
 Uma declaração é a expressão de um direito em relação a um valor específico. Um direito pode ser algo como "leitura", "gravação" ou "executar". Um valor pode ser um banco de dados, um arquivo, uma caixa de correio ou uma propriedade. As declarações também têm um tipo de declaração. A combinação de tipo de declaração e direita fornece o mecanismo para especificar recursos em relação ao valor. Por exemplo, uma declaração do tipo "File", com right "Read" sobre o valor "Biography.doc", indica que a entidade com a qual essa declaração está associada tem acesso de leitura para o arquivo Biography.doc. Uma declaração do tipo "Name", com right "temproperty" sobre o valor "Martin", indica que a entidade com a qual tal declaração está associada possui uma propriedade Name com o valor "Martin".  
  
 Embora vários tipos de declaração e direitos sejam definidos como parte do modelo de identidade, o sistema é extensível, permitindo que vários sistemas, com base na infraestrutura de modelo de identidade, definam tipos de declaração e direitos adicionais, conforme necessário.  
  
### <a name="identity-claims"></a>Declarações de identidade  
 Um direito específico é a identidade. As declarações que possuem esse direito fazem uma instrução sobre a identidade da entidade. Por exemplo, uma declaração do tipo "nome principal do usuário" (UPN) com um valor de " someone@example.com " e um direito de identidade indica uma identidade específica em um domínio específico.  
  
#### <a name="system-identity-claim"></a>Declaração de identidade do sistema  
 O modelo de identidade define uma declaração de identidade: System. A declaração de identidade do sistema indica que uma entidade é o aplicativo ou sistema atual.  
  
### <a name="sets-of-claims"></a>Conjuntos de declarações  
 O modelo de declarações que representam a identidade é importante porque as declarações são sempre emitidas por alguma entidade no sistema, mesmo que essa entidade seja, por fim, um conceito de "self". As declarações são agrupadas como um conjunto e cada conjunto tem um emissor. Um emissor é apenas um conjunto de declarações. Essa relação recursiva deve eventualmente terminar e qualquer conjunto de declarações pode ser seu próprio emissor.  
  
 A figura a seguir mostra um exemplo de três conjuntos de declarações em que um conjunto de declarações tem, como seu emissor, outro conjunto de declarações que, por sua vez, tem a declaração do sistema definida como seu emissor. Portanto, os conjuntos de declarações formam uma hierarquia que pode ser arbitrariamente profunda.  
  
 ![Conjuntos de declarações dentro da hierarquia.](./media/managing-claims-and-authorization-with-the-identity-model/claims-sets-hierarchy.gif)  
  
 Vários conjuntos de declarações podem ter o mesmo conjunto de declarações emissores, conforme ilustrado na figura a seguir:
  
 ![Vários conjuntos de declarações com o mesmo conjunto de declarações emissores.](./media/managing-claims-and-authorization-with-the-identity-model/multiple-claim-sets-same-issuing-claim-set.gif)  
  
 Com exceção de um conjunto de declarações que é seu próprio emissor, o modelo de identidade não fornece suporte para conjuntos de declarações para formar um loop. Assim, uma situação em que o conjunto de declarações A é emitido pelo conjunto de declarações B, que, por sua vez, é emitido pelo conjunto de declarações A, nunca pode surgir. Além disso, o modelo de identidade não fornece suporte para conjuntos de declarações com vários emissores. Se dois ou mais emissores tiverem que emitir um determinado conjunto de declarações, você deverá usar vários conjuntos de declarações, cada um contendo as mesmas declarações, mas ter emissores diferentes.  
  
### <a name="the-origin-of-claims"></a>A origem das declarações  
 As declarações podem vir de uma variedade de fontes. Uma fonte comum de declarações são as credenciais apresentadas por um usuário, por exemplo, como parte de uma mensagem enviada a um serviço Web. O sistema valida essas declarações e elas se tornam parte de um conjunto de declarações associadas ao usuário. Outros componentes do sistema também podem ser fontes de declarações, incluindo, mas não se limitando a, o sistema operacional, a pilha de rede, o ambiente de tempo de execução ou o aplicativo. Além disso, os serviços remotos também podem ser uma fonte de declarações.  
  
### <a name="authorization-policies"></a>Políticas de autorização  
 No modelo de identidade, as declarações são geradas como parte do processo de avaliação da política de autorização. Uma política de autorização examina o conjunto (possivelmente vazio) de declarações existentes e pode optar por adicionar declarações adicionais com base nas declarações já presentes e informações adicionais em sua disposição. Isso fornece a base do mapeamento entre as declarações. A presença ou a ausência de declarações no sistema influencia o comportamento de uma política de autorização em relação a se ela adiciona declarações adicionais.  
  
 Por exemplo, a política de autorização tem acesso a um banco de dados que inclui as birthdates das várias entidades usando o sistema. A política de autorização usa essas informações para adicionar uma declaração "Over18" ao contexto. Observe que essa declaração Over18 não divulga nenhuma informação sobre a entidade que não seja o fato de que ela tem mais de 18 anos de idade. Observe que a interpretação da declaração ' Over18 ' depende da compreensão da semântica dessa declaração. A política de autorização que adicionou a declaração compreende essas semânticas em algum nível. O código que examina subseqüentemente as declarações que resultam da avaliação da política também são informados sobre essas semânticas.  
  
 Uma determinada política de autorização pode exigir que ela seja avaliada várias vezes porque, como outras políticas de autorização adicionam declarações, essa política de autorização pode adicionar ainda mais declarações. O modelo de identidade foi projetado para continuar a avaliação até que nenhuma declaração adicional seja adicionada ao contexto por qualquer uma das políticas de autorização em vigor. Essa avaliação contínua de políticas de autorização impede a necessidade de impor qualquer ordem de avaliação específica em relação às políticas de autorização; Eles podem ser avaliados em qualquer ordem. Por exemplo, se A política X adicionar apenas a declaração Z se A política A tiver adicionado a declaração B, se X for avaliado primeiro, ele inicialmente não adicionará a declaração Z. Subsequentemente, A é avaliada e adiciona a declaração B. X é então avaliada uma segunda vez e, desta vez, ela adiciona a declaração Z.  
  
 Um determinado sistema pode ter muitas políticas de autorização em vigor.  
  
### <a name="a-key-making-machine"></a>Um computador de criação de chaves  
 Avaliar um grupo de políticas de autorização associadas é como usar um computador que faz chaves. As políticas de autorização são avaliadas e os conjuntos de declarações são gerados, criando a forma da chave. Depois que a forma da chave for concluída, ela poderá ser usada para tentar abrir alguns bloqueios. A forma da chave é armazenada em um "contexto de autorização", que é criado por um Gerenciador de autorização.  
  
### <a name="authorization-context"></a>Contexto de autorização  
 Um Gerenciador de autorização avalia as várias políticas de autorização conforme descrito, e o resultado é um contexto de autorização (um conjunto de conjuntos de declarações e algumas propriedades associadas). O contexto de autorização pode ser examinado para determinar quais declarações estão presentes nesse contexto, as relações entre essas várias declarações (por exemplo, o conjunto de declarações emissora) e, por fim, compará-las com alguns requisitos que eles devem atender para acessar um recurso.  
  
### <a name="locks"></a>Locks  
 Se um contexto de autorização (um conjunto de declarações) for uma chave, os requisitos que devem ser satisfeitos para conceder acesso a um recurso protegido específico constituem o bloqueio que a chave deve ajustar. O modelo de identidade não formaliza como esses requisitos são expressos, mas eles fazem, considerando a natureza baseada em declarações do sistema, envolvem a comparação das declarações no contexto de autorização com relação a algum conjunto de declarações necessárias.  
  
### <a name="a-recap"></a>Uma recapitulação  
 O modelo de identidade é baseado em relação ao conceito de declarações. As declarações são agrupadas em conjuntos e agregadas em um contexto de autorização. Um contexto de autorização contém um conjunto de declarações e é o resultado da avaliação de uma ou mais políticas de autorização associadas a um Gerenciador de autorização. Esses conjuntos de declarações podem ser examinados para determinar se os requisitos de acesso foram atendidos. A figura a seguir mostra as relações entre esses vários conceitos de modelo de identidade.  
  
 ![Gerenciando reivindicações e autorização](media/xsi-recap.gif "xsi_recap")  
  
## <a name="wcf-and-identity-model"></a>WCF e modelo de identidade  
 O WCF usa a infraestrutura do modelo de identidade como a base para executar a autorização. No WCF, a <xref:System.ServiceModel.Description.ServiceAuthorizationBehavior> classe permite que você especifique políticas de *autorização* como parte de um serviço. Essas políticas de autorização são conhecidas como *políticas de autorização externa*, e podem executar o processamento de declarações com base na política local ou por interação com um serviço remoto. O Gerenciador de autorização, representado pela <xref:System.ServiceModel.ServiceAuthorizationManager> classe, avalia as políticas de autorização externa junto com as políticas de autorização que reconhecem os vários tipos de credencial (tokens) e popula o que é chamado de *contexto de autorização* com as declarações apropriadas a uma mensagem de entrada. O contexto de autorização é representado pela <xref:System.IdentityModel.Policy.AuthorizationContext> classe.  
  
## <a name="identity-model-programming"></a>Programação de modelo de identidade  
 A tabela a seguir descreve o modelo de objeto usado para programar extensões de modelo de identidade. Todas essas classes existem tanto no <xref:System.IdentityModel.Policy> ou nos <xref:System.IdentityModel.Claims> namespaces.  
  
|Classe|Descrição|  
|-----------|-----------------|  
|Componente de autorização|Uma classe de modelo de identidade que implementa a <xref:System.IdentityModel.Policy.IAuthorizationComponent> interface.|  
|<xref:System.IdentityModel.Policy.IAuthorizationComponent>|Uma interface que fornece uma única propriedade de cadeia de caracteres somente leitura: ID. O valor dessa propriedade é exclusivo para cada instância no sistema que implementa essa interface.|  
|<xref:System.IdentityModel.Policy.AuthorizationContext>|Um *componente de autorização* que contém um conjunto de `ClaimSet` instâncias com zero ou mais propriedades; o resultado da avaliação de uma ou mais políticas de autorização.|  
|<xref:System.IdentityModel.Claims.Claim>|Uma combinação de um tipo de declaração, certo e valor. As partes direita e de valor são restritas pelo tipo de declaração.|  
|<xref:System.IdentityModel.Claims.ClaimSet>|Uma classe base abstrata. Uma coleção de instâncias `Claim`.|  
|<xref:System.IdentityModel.Claims.DefaultClaimSet>|Uma classe selada. Uma implementação da `ClaimSet` classe.|  
|<xref:System.IdentityModel.Policy.EvaluationContext>|Uma classe base abstrata. Passado para uma política de autorização durante a avaliação da política.|  
|<xref:System.IdentityModel.Policy.IAuthorizationPolicy>|Uma interface derivada de `IAuthorizationComponent` e implementada por classes de política de autorização.|  
|<xref:System.IdentityModel.Claims.Rights>|Uma classe estática que contém valores de Right predefinidos.|  
  
 As classes a seguir também são usadas para programação de modelo de identidade, mas não são encontradas nos <xref:System.IdentityModel.Policy> <xref:System.IdentityModel.Claims> namespaces ou.  
  
|Classe|Descrição|  
|-----------|-----------------|  
|<xref:System.ServiceModel.ServiceAuthorizationManager>|Uma classe que fornece um método – <xref:System.ServiceModel.ServiceAuthorizationManager.CheckAccessCore%2A> – para executar verificações de autorização baseadas em declarações para cada operação em um serviço. Você deve derivar da classe e substituir o método.|  
|<xref:System.ServiceModel.Description.ServiceAuthorizationBehavior>|Uma classe selada que fornece várias propriedades relacionadas ao comportamento de um serviço, pois ele pertence à autorização.|  
|<xref:System.ServiceModel.ServiceSecurityContext>|Uma classe que fornece contexto de segurança, incluindo contexto de autorização, para a operação em execução no momento (ou prestes a ser executada). Uma instância dessa classe faz parte do <xref:System.ServiceModel.OperationContext> .|  
  
### <a name="significant-members"></a>Membros significativos  
 Os membros a seguir são comumente usados para criar novos tipos de declaração.  
  
|Membro|Description|  
|------------|-----------------|  
|<xref:System.ServiceModel.ServiceAuthorizationManager.CheckAccessCore%2A>|As classes derivadas implementam esse método para executar verificações de acesso baseadas em declarações antes da execução de operações em um serviço. Qualquer e todas as informações no fornecido <xref:System.ServiceModel.OperationContext> , ou em outro lugar, podem ser examinadas ao tomar a decisão de verificação de acesso. Se <xref:System.ServiceModel.ServiceAuthorizationManager.CheckAccessCore%2A> `true` for retornado, o acesso será concedido e a operação poderá ser executada. Se `CheckAccessCore` retorna `false` , o acesso é negado e a operação não é executada. Para obter um exemplo, consulte [como: criar um Gerenciador de autorização personalizado para um serviço](../extending/how-to-create-a-custom-authorization-manager-for-a-service.md).|  
|<xref:System.ServiceModel.Description.ServiceAuthorizationBehavior.ServiceAuthorizationManager%2A>|Retorna o <xref:System.ServiceModel.ServiceAuthorizationManager> para o serviço. O <xref:System.ServiceModel.ServiceAuthorizationManager> é responsável por tomar decisões de autorização.|  
|<xref:System.ServiceModel.Description.ServiceAuthorizationBehavior.ExternalAuthorizationPolicies%2A>|A coleção de políticas de autorização personalizadas especificadas para o serviço. Essas políticas são avaliadas além das políticas associadas às credenciais em mensagens de entrada.|  
  
## <a name="see-also"></a>Veja também

- <xref:System.IdentityModel.Policy.AuthorizationContext>
- <xref:System.IdentityModel.Claims.Claim>
- <xref:System.IdentityModel.Policy.EvaluationContext>
- <xref:System.IdentityModel.Policy.IAuthorizationComponent>
- <xref:System.IdentityModel.Policy.IAuthorizationPolicy>
- <xref:System.IdentityModel.Claims.Rights>
- <xref:System.IdentityModel.Claims>
- <xref:System.IdentityModel.Policy>
- <xref:System.IdentityModel.Tokens>
- <xref:System.IdentityModel.Selectors>
- [Declarações e tokens](claims-and-tokens.md)
- [Declarações e acesso negado para recursos](claims-and-denying-access-to-resources.md)
- [Valores de recursos e criação de declarações](claim-creation-and-resource-values.md)
- [Como criar uma declaração personalizada](../extending/how-to-create-a-custom-claim.md)
- [Como comparar declarações](../extending/how-to-compare-claims.md)
- [Como criar uma diretiva de autorização personalizada](../extending/how-to-create-a-custom-authorization-policy.md)
- [Como criar gerenciador de autorização personalizado para um serviço](../extending/how-to-create-a-custom-authorization-manager-for-a-service.md)
- [Visão geral de segurança](security-overview.md)
- [Autorização](authorization-in-wcf.md)
