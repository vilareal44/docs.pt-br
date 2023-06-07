---
title: Visão geral de modelo de programação HTTP Web do WCF
ms.date: 03/30/2017
ms.assetid: 381fdc3a-6e6c-4890-87fe-91cca6f4b476
ms.openlocfilehash: 34d7945b8a7898955794e2ad5813bc66f52b60c7
ms.sourcegitcommit: cdb295dd1db589ce5169ac9ff096f01fd0c2da9d
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/09/2020
ms.locfileid: "84594927"
---
# <a name="wcf-web-http-programming-model-overview"></a>Visão geral de modelo de programação HTTP Web do WCF
O modelo de programação HTTP WEB do Windows Communication Foundation (WCF) fornece os elementos básicos necessários para criar serviços HTTP WEB com o WCF. Os serviços HTTP WEB do WCF são projetados para serem acessados pela mais ampla variedade de clientes possíveis, incluindo navegadores da Web e têm os seguintes requisitos exclusivos:  
  
- **URIs e processamento de URI** Os URIs desempenham uma função central no design dos serviços HTTP da WEB. O modelo de programação WEB HTTP do WCF usa as <xref:System.UriTemplate> <xref:System.UriTemplateTable> classes e para fornecer recursos de processamento de URI.  
  
- **Suporte para operações Get e post** Os serviços HTTP da WEB usam o verbo GET para recuperação de dados, além de vários verbos de invocação para modificação de dados e invocação remota. O modelo de programação WEB HTTP do WCF usa o <xref:System.ServiceModel.Web.WebGetAttribute> e o <xref:System.ServiceModel.Web.WebInvokeAttribute> para associar operações de serviço com VERBOS http Get e outros, como put, post e Delete.  
  
- **Vários formatos de dados** Os serviços de estilo da Web processam muitos tipos de dados além das mensagens SOAP. O modelo de programação WEB HTTP do WCF usa o <xref:System.ServiceModel.WebHttpBinding> e <xref:System.ServiceModel.Description.WebHttpBehavior> para oferecer suporte a vários formatos de dados diferentes, incluindo documentos XML, objeto de dados JSON e fluxos de conteúdo binário, como imagens, arquivos de vídeo ou texto sem formatação.  
  
 O modelo de programação WEB HTTP do WCF amplia o alcance do WCF para abranger cenários de estilo da Web que incluem serviços HTTP WEB, serviços AJAX e JSON e feeds de distribuição (ATOM/RSS). Para obter mais informações sobre os serviços AJAX e JSON, consulte [integração Ajax e suporte JSON](ajax-integration-and-json-support.md). Para obter mais informações sobre a distribuição, consulte [visão geral da agregação do WCF](wcf-syndication-overview.md).  
  
 Não há nenhuma restrição extra nos tipos de dados que podem ser retornados de um serviço HTTP da WEB. Qualquer tipo serializável pode ser retornado de uma operação de serviço HTTP WEB. Como as operações de serviço HTTP da WEB podem ser invocadas por um navegador da Web, há uma limitação sobre quais tipos de dados podem ser especificados em uma URL. Para obter mais informações sobre quais tipos têm suporte por padrão, consulte a seção **URLs e parâmetros da cadeia de consulta do UriTemplate** abaixo. O comportamento padrão pode ser alterado fornecendo sua própria implementação de T:System.ServiceModel.Dispatcher.QueryStringConverter, que especifica como converter os parâmetros especificados em uma URL para o tipo de parâmetro real. Para obter mais informações, consulte <xref:System.ServiceModel.Dispatcher.QueryStringConverter>.  
  
> [!CAUTION]
> Os serviços escritos com o modelo de programação WEB HTTP do WCF não usam mensagens SOAP. Como o SOAP não é usado, os recursos de segurança fornecidos pelo WCF não podem ser usados. Você pode, no entanto, usar a segurança baseada em transporte hospedando seu serviço com HTTPS. Para obter mais informações sobre a segurança do WCF, consulte [visão geral de segurança](security-overview.md)  
  
> [!WARNING]
> A instalação da extensão WebDAV para o IIS pode fazer com que os serviços HTTP da Web retornem um erro HTTP 405, pois a extensão WebDAV tenta lidar com todas as solicitações PUT. Para contornar esse problema, você pode desinstalar a extensão WebDAV ou desabilitar a extensão WebDAV para seu site. Para obter mais informações, consulte [IIS e WebDAV](https://learn.iis.net/page.aspx/357/webdav-for-iis-70/)  
  
## <a name="uri-processing-with-uritemplate-and-uritemplatetable"></a>Processamento de URI com UriTemplate e UriTemplateTable  
 Os modelos de URI fornecem uma sintaxe eficiente para expressar grandes conjuntos de URIs estruturalmente semelhantes. Por exemplo, o modelo a seguir expressa o conjunto de todos os URIs de três segmentos que começam com "a" e terminam com "c" sem considerar o valor do segmento intermediário: a/{Segment}/c  
  
 Este modelo descreve URIs como o seguinte:  
  
- a/x/c  
  
- a/s/c  
  
- a/z/c  
  
- e assim por diante.  
  
 Neste modelo, a notação de chave ("{Segment}") indica um segmento variável em vez de um valor literal.  
  
 .NET Framework fornece uma API para trabalhar com modelos de URI chamados <xref:System.UriTemplate> . `UriTemplates`permita que você faça o seguinte:  
  
- Você pode chamar um dos `Bind` métodos com um conjunto de parâmetros para produzir um *URI totalmente fechado* que corresponda ao modelo. Isso significa que todas as variáveis dentro do modelo de URI são substituídas por valores reais.  
  
- Você pode chamar `Match` () com um URI candidato, que usa um modelo para dividir um URI candidato em suas partes constituintes e retorna um dicionário que contém as diferentes partes do URI rotuladas de acordo com as variáveis no modelo.  
  
- `Bind`() e `Match` () são inversos para que você possa chamar `Match` ( `Bind` (x)) e voltar com o mesmo ambiente com o qual começou.  
  
 Há muitas vezes (especialmente no servidor, em que a expedição de uma solicitação para uma operação de serviço baseada no URI é necessária) que você deseja manter o controle de um conjunto de <xref:System.UriTemplate> objetos em uma estrutura de dados que possa tratar de forma independente cada um dos modelos contidos. <xref:System.UriTemplateTable>representa um conjunto de modelos de URI e seleciona a melhor correspondência de acordo com um conjunto de modelos e um URI candidato. Isso não é afiliado a nenhuma pilha de rede específica (WCF incluída) para que você possa usá-la sempre que necessário.  
  
 O modelo de serviço do WCF usa <xref:System.UriTemplate> e <xref:System.UriTemplateTable> para associar operações de serviço com um conjunto de URIs descritos por um <xref:System.UriTemplate> . Uma operação de serviço é associada a um <xref:System.UriTemplate> , usando o <xref:System.ServiceModel.Web.WebGetAttribute> ou o <xref:System.ServiceModel.Web.WebInvokeAttribute> . Para obter mais informações sobre o <xref:System.UriTemplate> e o <xref:System.UriTemplateTable> , consulte [UriTemplate e UriTemplateTable](uritemplate-and-uritemplatetable.md)  
  
## <a name="webget-and-webinvoke-attributes"></a>Atributos WebGet e WebInvoke  
 Os serviços HTTP WEB do WCF fazem uso de verbos de recuperação (por exemplo, HTTP GET) Além de vários verbos de invocação (por exemplo, HTTP POST, PUT e DELETE). O modelo de programação WEB HTTP do WCF permite que os desenvolvedores de serviço controlem o modelo de URI e o verbo associados a suas operações de serviço com o <xref:System.ServiceModel.Web.WebGetAttribute> e o <xref:System.ServiceModel.Web.WebInvokeAttribute> . O <xref:System.ServiceModel.Web.WebGetAttribute> e o <xref:System.ServiceModel.Web.WebInvokeAttribute> permitem que você controle como as operações individuais são associadas a URIs e os métodos http associados a esses URIs. Por exemplo, adicionar <xref:System.ServiceModel.Web.WebGetAttribute> e <xref:System.ServiceModel.Web.WebInvokeAttribute> no código a seguir.  
  
```csharp
[ServiceContract]  
interface ICustomer  
{  
  //"View It"  
  
  [WebGet]  
  Customer GetCustomer():  
  
  //"Do It"  
    [WebInvoke]  
  Customer UpdateCustomerName( string id,
                               string newName );  
}  
```  
  
 O código anterior permite que você faça as seguintes solicitações HTTP.  
  
 `GET /GetCustomer`  
  
 `POST /UpdateCustomerName`  
  
 <xref:System.ServiceModel.Web.WebInvokeAttribute>o padrão é POST, mas você também pode usá-lo para outros verbos.  
  
```csharp
[ServiceContract]  
interface ICustomer  
{  
  //"View It" -> HTTP GET  
    [WebGet( UriTemplate="customers/{id}" )]  
  Customer GetCustomer( string id ):  
  
  //"Do It" -> HTTP PUT  
  [WebInvoke( UriTemplate="customers/{id}", Method="PUT" )]  
  Customer UpdateCustomer( string id, Customer newCustomer );  
}  
```  
  
 Para ver um exemplo completo de um serviço WCF que usa o modelo de programação WEB HTTP do WCF, consulte [como: criar um serviço http Web do WCF básico](how-to-create-a-basic-wcf-web-http-service.md)  
  
## <a name="uritemplate-query-string-parameters-and-urls"></a>URLs e parâmetros de cadeia de consulta do UriTemplate  
 Os serviços de estilo da Web podem ser chamados de um navegador da Web, digitando uma URL associada a uma operação de serviço. Essas operações de serviço podem pegar parâmetros de cadeia de caracteres de consulta que devem ser especificados em um formulário de cadeia de caracteres dentro da URL. A tabela a seguir mostra os tipos que podem ser passados dentro de uma URL e o formato usado.  
  
|Type|Formatar|  
|----------|------------|  
|<xref:System.Byte>|0 - 255|  
|<xref:System.SByte>|-128-127|  
|<xref:System.Int16>|-32768-32767|  
|<xref:System.Int32>|-2.147.483.648-2.147.483.647|  
|<xref:System.Int64>|-9.223.372.036.854.775.808-9.223.372.036.854.775.807|  
|<xref:System.UInt16>|0 - 65535|  
|<xref:System.UInt32>|0-4.294.967.295|  
|<xref:System.UInt64>|0-18446744073709551615|  
|<xref:System.Single>|-3.402823 e38-3.402823 e38 (a notação exponencial não é necessária)|  
|<xref:System.Double>|-1.79769313486232 e308-1.79769313486232 e308 (a notação exponencial não é necessária)|  
|<xref:System.Char>|Qualquer caractere único|  
|<xref:System.Decimal>|Qualquer decimal na notação padrão (sem expoente)|  
|<xref:System.Boolean>|True ou false (não diferencia maiúsculas de minúsculas)|  
|<xref:System.String>|Qualquer cadeia de caracteres (cadeia de caracteres nula não tem suporte e nenhuma saída é feita)|  
|<xref:System.DateTime>|MM/DD/AAAA<br /><br /> MM/DD/AAAA HH: MM: SS [AM&#124;PM]<br /><br /> Mês dia ano<br /><br /> Mês dia ano HH: MM: SS [AM&#124;PM]|  
|<xref:System.TimeSpan>|Yyyy. HH: MM: SS<br /><br /> Onde DD = dias, HH = horas, MM = minutos, SS = segundos|  
|<xref:System.Guid>|Um GUID, por exemplo:<br /><br /> 936DA01F-9ABD-4d9d-80C7-02AF85C822A8|  
|<xref:System.DateTimeOffset>|MM/DD/AAAA HH: MM: SS MM: SS<br /><br /> Onde DD = dias, HH = horas, MM = minutos, SS = segundos|  
|Enumerações|O valor de enumeração, por exemplo, que define a enumeração, conforme mostrado no código a seguir.<br /><br /> `public enum Days{ Sunday, Monday, Tuesday, Wednesday, Thursday, Friday, Saturday };`<br /><br /> Qualquer um dos valores de enumeração individuais (ou seus valores inteiros correspondentes) pode ser especificado na cadeia de caracteres de consulta.|  
|Tipos que têm um `TypeConverterAttribute` que podem converter o tipo de e para uma representação de cadeia de caracteres.|Depende do conversor de tipo.|  
  
## <a name="formats-and-the-wcf-web-http-programming-model"></a>Formatos e o modelo de programação WEB HTTP do WCF  
 O modelo de programação WEB HTTP do WCF tem novos recursos para trabalhar com vários formatos de dados diferentes. Na camada de associação, o <xref:System.ServiceModel.WebHttpBinding> pode ler e gravar os seguintes tipos diferentes de dados:  
  
- XML  
  
- JSON  
  
- Fluxos binários opacos  
  
 Isso significa que o modelo de programação WEB HTTP do WCF pode lidar com qualquer tipo de dados, mas você pode estar programando <xref:System.IO.Stream> .  
  
 .NET Framework 3,5 fornece suporte para dados JSON (AJAX), bem como feeds de distribuição (incluindo ATOM e RSS). Para obter mais informações sobre esses recursos, consulte [formatação do WCF Web http](wcf-web-http-formatting.md), [visão geral da agregação do WCF](wcf-syndication-overview.md)e suporte a integração do [Ajax e JSON](ajax-integration-and-json-support.md).  
  
## <a name="wcf-web-http-programming-model-and-security"></a>Segurança e modelo de programação do WCF WEB HTTP  

Como o modelo de programação WEB HTTP do WCF não dá suporte aos protocolos WS-*, a única maneira de proteger um serviço WEB HTTP WCF é expor o serviço por HTTPS usando SSL. Para obter mais informações sobre como configurar o SSL com o IIS 7,0, consulte [como implementar o SSL no IIS](https://support.microsoft.com/help/299875/how-to-implement-ssl-in-iis).
  
## <a name="troubleshooting-the-wcf-web-http-programming-model"></a>Solução de problemas do modelo de programação WEB HTTP do WCF  
 Ao chamar WCF WEB HTTP Services usando um <xref:System.ServiceModel.Channels.ChannelFactoryBase%601> para criar um canal, o <xref:System.ServiceModel.Description.WebHttpBehavior> usa o <xref:System.ServiceModel.EndpointAddress> conjunto no arquivo de configuração, mesmo se um diferente <xref:System.ServiceModel.EndpointAddress> for passado para o <xref:System.ServiceModel.Channels.ChannelFactoryBase%601> .  
  
## <a name="see-also"></a>Confira também

- [Sindicalização do WCF](wcf-syndication.md)
- [Modelo de objeto de programação HTTP Web do WCF](wcf-web-http-programming-object-model.md)
- [Modelo de programação WCF Web HTTP](wcf-web-http-programming-model.md)
