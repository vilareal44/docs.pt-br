---
title: Guia de segurança do BinaryFormatter
description: Este artigo descreve os riscos de segurança inerentes ao tipo de BinaryFormatter e recomendações para diferentes serializadores usarem.
ms.date: 07/11/2020
ms.author: levib
author: GrabYourPitchforks
ms.openlocfilehash: 2c76a81650e5b83677f6c4df64770bd1ef5f775e
ms.sourcegitcommit: cbb19e56d48cf88375d35d0c27554d4722761e0d
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/19/2020
ms.locfileid: "88607941"
---
# <a name="binaryformatter-security-guide"></a>Guia de segurança do BinaryFormatter

Este artigo se aplica às seguintes implementações do .NET:

* .NET Framework todas as versões
* .NET Core 2,1-3,1
* .NET 5,0 e posterior

## <a name="background"></a>Segundo plano

> [!WARNING]
> O <xref:System.Runtime.Serialization.Formatters.Binary.BinaryFormatter> tipo é perigoso e ***não*** é recomendado para o processamento de dados. Os aplicativos devem parar de usar `BinaryFormatter` o mais rápido possível, mesmo que eles acreditem que os dados que estão processando são confiáveis. `BinaryFormatter` Não é seguro e não pode se tornar seguro.

Este artigo também se aplica aos seguintes tipos:

* <xref:System.Runtime.Serialization.Formatters.Soap.SoapFormatter>
* <xref:System.Runtime.Serialization.NetDataContractSerializer>
* <xref:System.Web.UI.LosFormatter>
* <xref:System.Web.UI.ObjectStateFormatter>

As vulnerabilidades de desserialização são uma categoria de ameaça em que as cargas de solicitação são processadas de forma insegura. Um invasor que aproveita com êxito essas vulnerabilidades em relação a um aplicativo pode causar negação de serviço (DoS), divulgação de informações ou execução remota de código dentro do aplicativo de destino. Essa categoria de risco faz com que os [10 principais OWASP](https://owasp.org/www-project-top-ten/)de forma consistente. Os destinos incluem aplicativos escritos em [uma variedade de linguagens](https://owasp.org/www-community/vulnerabilities/Deserialization_of_untrusted_data), incluindo C/C++, Java e C#.

No .NET, o maior alvo de risco são os aplicativos que usam o <xref:System.Runtime.Serialization.Formatters.Binary.BinaryFormatter> tipo para desserializar dados. `BinaryFormatter` é amplamente usado em todo o ecossistema do .NET devido à sua potência e à facilidade de uso. No entanto, essa mesma energia dá aos invasores a capacidade de influenciar o fluxo de controle no aplicativo de destino. Os ataques bem-sucedidos podem fazer com que o invasor possa executar código dentro do contexto do processo de destino.

Como uma analogia mais simples, assuma que chamar `BinaryFormatter.Deserialize` sobre uma carga é o equivalente a interpretar essa carga como um executável autônomo e iniciá-la.

## <a name="binaryformatter-security-vulnerabilities"></a>Vulnerabilidades de segurança do BinaryFormatter

> [!WARNING]
> O `BinaryFormatter.Deserialize` método __nunca__ é seguro quando usado com entrada não confiável. É altamente recomendável que os consumidores considerem o uso de uma das alternativas descritas posteriormente neste artigo.

`BinaryFormatter` foi implementada antes que as vulnerabilidades de desserialização fossem uma categoria de ameaça bem compreendida. Como resultado, o código não segue as práticas recomendadas modernas. O `Deserialize` método pode ser usado como um vetor para que os invasores executem ataques de dos em aplicativos de consumo. Esses ataques podem tornar o aplicativo sem resposta ou resultar em um encerramento de processo inesperado. Essa categoria de ataque não pode ser atenuada com um `SerializationBinder` ou qualquer outra `BinaryFormatter` opção de configuração. O .NET considera esse comportamento como sendo ***projetado*** e não emitirá uma atualização de código para modificar o comportamento.

`BinaryFormatter.Deserialize` pode ser vulnerável a outras categorias de ataque, como divulgação de informações ou execução remota de código. A utilização de recursos como um personalizado <xref:System.Runtime.Serialization.SerializationBinder> pode ser insuficiente para atenuar corretamente esses riscos. Há a possibilidade de que uma vulnerabilidade de romance seja descoberta para a qual o .NET não possa publicar uma atualização de segurança. Os consumidores devem avaliar seus cenários individuais e considerar sua exposição potencial a esses riscos.

Recomendamos que `BinaryFormatter` os consumidores realizem avaliações de risco individuais em seus aplicativos. É a única responsabilidade do consumidor determinar se ele deve ser utilizado `BinaryFormatter` . Os consumidores devem arriscar a avaliar os requisitos de segurança, técnico, reputação, legal e regulatório do uso do `BinaryFormatter` .

## <a name="preferred-alternatives"></a>Alternativas preferenciais

O .NET oferece vários serializadores na caixa que podem manipular dados não confiáveis com segurança:

* <xref:System.Xml.Serialization.XmlSerializer> e <xref:System.Runtime.Serialization.DataContractSerializer> para serializar grafos de objeto em e de XML. Não confunda `DataContractSerializer` com  <xref:System.Runtime.Serialization.NetDataContractSerializer> .
* <xref:System.IO.BinaryReader> e <xref:System.IO.BinaryWriter> para XML e JSON.
* As <xref:System.Text.Json> APIs para serializar grafos de objeto em JSON.

## <a name="dangerous-alternatives"></a>Alternativas perigosas

Evite os seguintes serializadores:

* <xref:System.Runtime.Serialization.Formatters.Soap.SoapFormatter>
* <xref:System.Web.UI.LosFormatter>
* <xref:System.Runtime.Serialization.NetDataContractSerializer>
* <xref:System.Web.UI.ObjectStateFormatter>

Todos os serializadores anteriores executam desserialização polimórfica irrestrita e são perigosos, assim como `BinaryFormatter` .

## <a name="the-risks-of-assuming-data-to-be-trustworthy"></a>Os riscos de pressupor que os dados sejam confiáveis

Frequentemente, um desenvolvedor de aplicativos pode acreditar que está processando apenas entradas confiáveis. O caso de entrada seguro é verdadeiro em raras circunstâncias. Mas é muito mais comum que uma carga cruze um limite de confiança sem que o desenvolvedor a percebesse.

__Considere um servidor__ local em que os funcionários usam um cliente de desktop de suas estações de trabalho para interagir com o serviço. Esse cenário pode ser visto de forma simples como uma configuração "segura", em que a utilização `BinaryFormatter` é aceitável. No entanto, esse cenário apresenta um vetor para malware que obtém acesso a uma máquina de um único funcionário para poder se espalhar por toda a empresa. Esse malware pode aproveitar o uso da empresa de `BinaryFormatter` para mover-se posteriormente da estação de trabalho do funcionário para o servidor de back-end. Em seguida, ele pode exfiltrar os dados confidenciais da empresa. Esses dados podem incluir segredos comerciais ou dados do cliente.

__Considere também um aplicativo que usa `BinaryFormatter` para manter o estado de salvamento.__ A primeira parece ser um cenário seguro, já que ler e gravar dados em seu próprio disco rígido representa uma ameaça secundária. No entanto, compartilhar documentos por email ou pela Internet é comum, e a maioria dos usuários finais não perceberia abrir esses arquivos baixados como um comportamento arriscado.

Esse cenário pode ser utilizado para perigoso efeito. Se o aplicativo for um jogo, os usuários que compartilharem arquivos de salvamento não saberão em risco. Os próprios desenvolvedores também podem ser direcionados. O invasor pode enviar por email o suporte técnico dos desenvolvedores, anexando um arquivo de dados malicioso e solicitando que a equipe de suporte o abra. Esse tipo de ataque pode dar ao invasor uma destaque na empresa.

Outro cenário é onde o arquivo de dados é armazenado no armazenamento em nuvem e sincronizado automaticamente entre os computadores do usuário. Um invasor que possa obter acesso à conta de armazenamento em nuvem pode envenenar o arquivo de dados. Esse arquivo de dados será sincronizado automaticamente com os computadores do usuário. Na próxima vez que o usuário abrir o arquivo de dados, a carga do invasor será executada. Assim, o invasor pode aproveitar um comprometimento de conta de armazenamento em nuvem para obter permissões completas de execução de código.

__Considere um aplicativo que se move de um modelo de instalação de desktop para um modelo de nuvem primeiro.__ Esse cenário inclui aplicativos que se movem de um aplicativo de desktop ou modelo de cliente avançado para um modelo baseado na Web. Qualquer modelo de ameaça desenhado para o aplicativo de desktop não é necessariamente aplicável ao serviço baseado em nuvem. O modelo de ameaça para o aplicativo de desktop pode ignorar uma determinada ameaça como "não interessante para o cliente atacar sozinho". Mas essa mesma ameaça pode se tornar interessante quando considera que um usuário remoto (o cliente) está atacando o serviço de nuvem em si.

> [!NOTE]
> Em termos gerais, a intenção de serialização é transmitir um objeto para dentro ou para fora de um aplicativo. Um exercício de modelagem de ameaças quase sempre marca esse tipo de transferência de dados ao cruzar um limite de confiança.

## <a name="further-resources"></a>Outros recursos

* [YSoSerial.net](https://github.com/pwntester/ysoserial.net) para pesquisa sobre como os adversários atacam os aplicativos que utilizam `BinaryFormatter` .
* Plano de fundo geral em vulnerabilidades de desserialização:
  * [OWASP Top 10-A8:2017 – desserialização insegura](https://owasp.org/www-project-top-ten/OWASP_Top_Ten_2017/Top_10-2017_A8-Insecure_Deserialization)
  * [CWE-502: desserialização de dados não confiáveis](https://cwe.mitre.org/data/definitions/502.html)
