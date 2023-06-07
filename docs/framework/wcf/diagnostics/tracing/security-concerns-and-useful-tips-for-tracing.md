---
title: Preocupações de segurança e dicas úteis para rastreamento
ms.date: 03/30/2017
ms.assetid: 88bc2880-ecb9-47cd-9816-39016a07076f
ms.openlocfilehash: 0a09e387a4f964441f11d07a84bd492345d5b691
ms.sourcegitcommit: cdb295dd1db589ce5169ac9ff096f01fd0c2da9d
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/09/2020
ms.locfileid: "84578871"
---
# <a name="security-concerns-and-useful-tips-for-tracing"></a>Preocupações de segurança e dicas úteis para rastreamento
Este tópico descreve como você pode proteger informações confidenciais de serem expostas, bem como dicas úteis ao usar o webhost.  
  
## <a name="using-a-custom-trace-listener-with-webhost"></a>Usando um ouvinte de rastreamento personalizado com Webhost  
 Se estiver escrevendo seu próprio ouvinte de rastreamento, você deve estar ciente da possibilidade de que os rastreamentos possam ser perdidos no caso de um serviço hospedado na Web. Quando o Webhost é reciclado, ele desliga o processo em tempo real enquanto uma duplicata assume. No entanto, os dois processos ainda devem ter acesso ao mesmo recurso por algum tempo, o que depende do tipo de ouvinte. Nesse caso, o `XmlWriterTraceListener` cria um novo arquivo de rastreamento para o segundo processo; enquanto o rastreamento de eventos do Windows gerencia vários processos dentro da mesma sessão e fornece acesso ao mesmo arquivo. Se seu próprio ouvinte não fornecer funcionalidades semelhantes, os rastreamentos poderão ser perdidos quando o arquivo for bloqueado pelos dois processos.  
  
 Você também deve estar ciente de que um ouvinte de rastreamento personalizado pode enviar rastreamentos e mensagens na conexão, por exemplo, para um banco de dados remoto. Como um implantador de aplicativos, você deve configurar ouvintes personalizados com o controle de acesso apropriado. Você também deve aplicar o controle de segurança em qualquer informação pessoal que possa ser exposta no local remoto.  
  
## <a name="logging-sensitive-information"></a>Registrando informações confidenciais  
 Os rastreamentos contêm cabeçalhos de mensagem quando uma mensagem está no escopo. Quando o rastreamento está habilitado, as informações pessoais em cabeçalhos específicos do aplicativo, como uma cadeia de caracteres de consulta; e as informações de corpo, como um número de cartão de crédito, podem se tornar visíveis nos logs. O implantador de aplicativos é responsável por impor o controle de acesso nos arquivos de configuração e de rastreamento. Se você não quiser que esse tipo de informação fique visível, desabilite o rastreamento ou filtre parte dos dados se desejar compartilhar os logs de rastreamento.  
  
 As dicas a seguir podem ajudá-lo a impedir que o conteúdo de um arquivo de rastreamento seja exposto de forma não intencional:  
  
- Verifique se os arquivos de log estão protegidos por ACLs (listas de controle de acesso) em cenários de hospedagem interna e de host.  
  
- Escolha uma extensão de arquivo que não possa ser facilmente servida usando uma solicitação da Web. Por exemplo, a extensão de arquivo. xml não é uma opção segura. Você pode verificar o guia de administração do IIS para ver uma lista de extensões que podem ser servidas.  
  
- Especifique um caminho absoluto para o local do arquivo de log, que deve estar fora do diretório público do Webhost vroot para impedir que ele seja acessado por uma parte externa usando um navegador da Web.  
  
 Por padrão, as chaves e as informações de identificação pessoal (PII), como nome de usuário e senha, não são registradas em rastreamentos e mensagens registradas. Um administrador de máquina, no entanto, pode usar o `enableLoggingKnownPII` atributo no `machineSettings` elemento do arquivo Machine. config para permitir que os aplicativos em execução no computador registrem PII (informações de identificação pessoal) conhecidas da seguinte maneira:  
  
```xml  
<configuration>  
   <system.ServiceModel>  
      <machineSettings enableLoggingKnownPii="Boolean"/>  
   </system.ServiceModel>  
</configuration>
```  
  
 Um implantador de aplicativo pode usar o `logKnownPii` atributo no arquivo app. config ou Web. config para habilitar o log de PII da seguinte maneira:  
  
```xml  
<system.diagnostics>  
  <sources>  
      <source name="System.ServiceModel.MessageLogging"  
        logKnownPii="true">  
        <listeners>  
                 <add name="messages"  
                 type="System.Diagnostics.XmlWriterTraceListener"  
                 initializeData="c:\logs\messages.svclog" />  
          </listeners>  
      </source>  
  </sources>  
</system.diagnostics>  
```  
  
 Somente quando as duas configurações são o `true` log de PII habilitado. A combinação de duas opções permite a flexibilidade de registrar PII conhecida para cada aplicativo.  
  
 Você deve estar ciente de que, se você especificar duas ou mais fontes personalizadas em um arquivo de configuração, somente os atributos da primeira fonte serão lidos. Os outros são ignorados. Isso significa que, para o seguinte app. config, o arquivo, PII não é registrado para ambas as fontes, embora o log de PII esteja explicitamente habilitado para a segunda fonte.  
  
```xml  
<system.diagnostics>  
  <sources>  
      <source name="System.ServiceModel.MessageLogging"  
        logKnownPii="false">  
        <listeners>  
           <add name="messages"  
                type="System.Diagnostics.XmlWriterTraceListener"  
                initializeData="c:\logs\messages.svclog" />  
          </listeners>  
      </source>  
      <source name="System.ServiceModel"
         logKnownPii="true">  
         <listeners>  
            <add name="xml" />  
         </listeners>  
      </source>  
  </sources>  
</system.diagnostics>  
```  
  
 Se o `<machineSettings enableLoggingKnownPii="Boolean"/>` elemento existir fora do arquivo Machine. config, o sistema lançará um <xref:System.Configuration.ConfigurationErrorsException> .  
  
 As alterações entram em vigor somente quando o aplicativo é iniciado ou reiniciado. Um evento é registrado na inicialização quando ambos os atributos são definidos como `true` . Um evento também será registrado se `logKnownPii` for definido como `true` , mas `enableLoggingKnownPii` for `false` .  
  
 Para obter mais informações sobre o log de PII, consulte exemplo de [bloqueio de segurança de PII](../../samples/pii-security-lockdown.md) .  
  
 O administrador do computador e o implantador de aplicativos devem ter muito cuidado ao usar essas duas opções. Se o log de PII estiver habilitado, as chaves de segurança e PII serão registradas. Se estiver desabilitado, os dados confidenciais e específicos do aplicativo ainda serão registrados em cabeçalhos e corpos de mensagens. Para obter uma discussão mais completa sobre privacidade e proteger a PII de ser exposta, consulte [privacidade do usuário](https://docs.microsoft.com/previous-versions/dotnet/articles/aa480490(v=msdn.10)).  
  
 Além disso, o endereço IP do remetente da mensagem é registrado uma vez por conexão para transportes orientados por conexão e uma vez por mensagem enviada de outra forma. Isso é feito sem o consentimento do remetente. No entanto, esse log só ocorre nas informações ou níveis de rastreamento detalhados, que não são os níveis de rastreamento padrão ou recomendados na produção, exceto para a depuração dinâmica.  
  
## <a name="see-also"></a>Consulte também

- [Rastreamento](index.md)
