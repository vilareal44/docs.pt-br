---
title: Escolhendo um filtro
ms.date: 03/30/2017
ms.assetid: 67ab5af9-b9d9-4300-b3b1-41abb5a1fd10
ms.openlocfilehash: e951c472543239df0c01dcba3e46f120ced9e192
ms.sourcegitcommit: cdb295dd1db589ce5169ac9ff096f01fd0c2da9d
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/09/2020
ms.locfileid: "84587489"
---
# <a name="choosing-a-filter"></a>Escolhendo um filtro
Ao configurar o serviço de roteamento, é importante selecionar os filtros de mensagem corretos e configurá-los para permitir que você faça correspondências exatas com as mensagens recebidas. Se os filtros selecionados forem excessivamente amplos em suas correspondências ou estiverem configurados incorretamente, as mensagens serão roteadas incorretamente. Se os filtros forem muito restritivos, talvez você não tenha nenhuma rota válida disponível para algumas de suas mensagens.

## <a name="filter-types"></a>Tipos de filtro

Ao selecionar os filtros que são usados pelo serviço de roteamento, é importante entender como cada filtro funciona, bem como quais informações estão disponíveis como parte das mensagens de entrada. Por exemplo, se todas as mensagens forem recebidas no mesmo ponto de extremidade, os filtros address e EndpointName não serão úteis, pois todas as mensagens corresponderão a esses filtros.

### <a name="action"></a>Ação

O filtro de ação inspeciona a <xref:System.ServiceModel.Channels.MessageHeaders.Action%2A> propriedade. Se o conteúdo do cabeçalho de ação na mensagem corresponder ao valor de dados de filtro especificado na configuração do filtro, esse filtro retornará `true` . O exemplo a seguir define um `FilterElement` que usa o filtro de ação para corresponder mensagens com um cabeçalho de ação que contém um valor de `http://namespace/contract/operation/` .

```xml
<filter name="action1" filterType="Action" filterData="http://namespace/contract/operation/" />
```

```csharp
ActionMessageFilter action1 = new ActionMessageFilter(new string[] { "http://namespace/contract/operation" });
```

Esse filtro deve ser usado ao rotear mensagens que contêm um cabeçalho de ação exclusivo.

### <a name="endpointaddress"></a>EndpointAddress

O filtro EndpointAddress inspeciona o EndpointAddress no qual a mensagem foi recebida. Se o endereço que a mensagem chega em corresponder exatamente ao endereço de filtro especificado na configuração do filtro, esse filtro retornará `true` . O exemplo a seguir define um `FilterElement` que usa o filtro de endereço para corresponder a qualquer mensagem endereçada a "http:// \<hostname> /vdir/s.svc/b".

```xml
<filter name="address1" filterType="EndpointAddress" filterData="http://host/vdir/s.svc/b" />
```

```csharp
EndpointAddressMessageFilter address1 = new EndpointAddressMessageFilter(new EndpointAddress("http://host/vdir/s.svc/b"), false);
```

> [!NOTE]
> É importante observar que a parte do nome do host de um endereço pode ser diferente se o cliente usar o nome de domínio totalmente qualificado, o nome NetBIOS, o endereço IP ou outro nome. Como valores diferentes podem se referir ao mesmo host, o comportamento padrão dessa comparação é não usar a parte do nome de host do endereço ao executar correspondências.
>
> Esse comportamento pode ser modificado para permitir a comparação para avaliar o nome do host ao configurar o serviço de roteamento de forma programática.

Esse filtro deve ser usado quando as mensagens de entrada são endereçadas a um endereço exclusivo.

### <a name="endpointaddressprefix"></a>EndpointAddressPrefix

O filtro EndpointAddressPrefix é semelhante ao filtro EndpointAddress. O filtro EndpointAddressPrefix inspeciona o EndpointAddress no qual a mensagem foi recebida. No entanto, o filtro EndpointAddressPrefix atua como um curinga por endereços correspondentes que começam com o valor especificado na configuração do filtro. O exemplo a seguir define um `FilterElement` que usa o filtro EndpointAddressPrefix para corresponder a qualquer mensagem endereçada ao `http://<hostname>/vdir*` .

```xml
<filter name="prefix1" filterType="EndpointAddressPrefix" filterData="http://host/vdir" />
```

```csharp
PrefixEndpointAddressMessageFilter prefix1 = new PrefixEndpointAddressMessageFilter(new EndpointAddress("http://host/vdir/s.svc/b"), false);
```

> [!NOTE]
> É importante observar que a parte do nome do host de um endereço pode ser diferente se o cliente usar o nome de domínio totalmente qualificado, o nome NetBIOS, o endereço IP ou outro nome. Como valores diferentes podem se referir ao mesmo host, o comportamento padrão dessa comparação é não usar a parte do nome de host do endereço ao executar correspondências.

Esse filtro deve ser usado ao rotear mensagens de entrada que compartilham um prefixo de endereço comum.

### <a name="and"></a>AND

O filtro AND não filtra diretamente um valor dentro de uma mensagem, mas permite que você combine dois outros filtros para criar uma `AND` condição em que ambos os filtros devem corresponder à mensagem antes de o e o filtro ser avaliado como `true` . Isso permite que você crie filtros complexos que só correspondem se todos os subfiltros corresponderem. O exemplo a seguir define um filtro de endereço e um filtro de ação e, em seguida, define um filtro AND que avalia uma mensagem em relação aos filtros de endereço e de ação. Se os filtros de endereço e de ação forem correspondentes, o filtro e retornará `true` .

```xml
<filter name="address1" filterType="AddressPrefix" filterData="http://host/vdir"/>
<filter name="action1" filterType="Action" filterData="http://namespace/contract/operation/"/>
<filter name="and1" filterType="And" filter1="address1" filter2="action1" />
```

```csharp
EndpointAddressMessageFilter address1 = new EndpointAddressMessageFilter(new EndpointAddress("http://host/vdir/s.svc/b"), false);
ActionMessageFilter action1 = new ActionMessageFilter(new string[] { "http://namespace/contract/operation" });
StrictAndMessageFilter and1=new StrictAndMessageFilter(address1, action1);
```

Esse filtro deve ser usado quando você deve combinar a lógica de vários filtros para determinar quando uma correspondência deve ser feita. Por exemplo, se você tiver vários destinos que devem receber apenas determinadas combinações de ações e mensagens para endereços específicos, você poderá usar um filtro e para combinar os filtros de ação e endereço necessários.

### <a name="custom"></a>Personalizado

Ao selecionar o tipo de filtro personalizado, você deve fornecer um valor CustomType que contém o tipo do assembly que contém a implementação de **MessageFilter** a ser usada para esse filtro. Além disso, filterData deve conter quaisquer valores que o filtro personalizado possa exigir em sua avaliação de mensagens. O exemplo a seguir define um `FilterElement` que usa a `CustomAssembly.MyCustomMsgFilter` implementação MessageFilter.

```xml
<filter name="custom1" filterType="Custom" customType="CustomAssembly.MyCustomMsgFilter, CustomAssembly" filterData="Custom Data" />
```

```csharp
MyCustomMsgFilter custom1=new MyCustomMsgFilter("Custom Data");
```

Se você precisar executar uma lógica de correspondência personalizada em uma mensagem que não é coberta pelos filtros fornecidos com o [!INCLUDE[netfx_current_short](../../../../includes/netfx-current-short-md.md)] , deverá criar um filtro personalizado que seja uma implementação da classe **MessageFilter** . Por exemplo, você pode criar um filtro personalizado que compara um campo na mensagem de entrada com relação a uma lista de valores conhecidos fornecidos ao filtro como configuração ou que faz hash de um determinado elemento de mensagem e, em seguida, examina esse valor para determinar se o filtro deve retornar `true` ou `false` .

### <a name="endpointname"></a>EndpointName

O filtro EndpointName inspeciona o nome do ponto de extremidade que recebeu a mensagem. O exemplo a seguir define um `FilterElement` que usa o filtro EndpointName para rotear as mensagens recebidas no "SvcEndpoint".

```xml
<filter name="name1" filterType="Endpoint" filterData="SvcEndpoint" />
```

```csharp
EndpointNameMessageFilter name1 = new EndpointNameMessageFilter("SvcEndpoint");
```

Esse filtro é útil quando o serviço de roteamento expõe mais de um ponto de extremidade de serviço nomeado. Por exemplo, você pode expor dois pontos de extremidade que o serviço de roteamento usa para receber mensagens; uma é usada por clientes de prioridade que exigem o processamento em tempo real de suas mensagens, enquanto o outro ponto de extremidade recebe mensagens que não são sensíveis ao tempo.

Embora você geralmente possa usar a correspondência de endereço completo para determinar em qual ponto de extremidade uma mensagem foi recebida, usar o nome do ponto de extremidade definido é um atalho conveniente que geralmente é menos propenso a erros, especialmente ao configurar um serviço de roteamento usando um arquivo de configuração (em que os nomes de ponto de extremidade são um atributo obrigatório).

### <a name="matchall"></a>MatchAll

O filtro filtro matchall corresponde a qualquer mensagem recebida. Isso é útil se você sempre deve rotear todas as mensagens recebidas para um ponto de extremidade específico, como um serviço de registro em log que armazena uma cópia de todas as mensagens recebidas. O exemplo a seguir define um `FilterElement` que usa o filtro filtro matchall.

```xml
<filter name="matchAll1" filterType="MatchAll" />
```

```csharp
MatchAllMessageFilter matchAll1 = new MatchAllMessageFilter();
```

### <a name="xpath"></a>XPath

O filtro XPath permite que você especifique uma consulta XPath que é usada para inspecionar um elemento específico dentro da mensagem. A filtragem XPath é uma opção de filtragem poderosa que permite que você inspecione diretamente qualquer entrada endereçável XML dentro da mensagem; no entanto, é necessário ter conhecimento específico da estrutura das mensagens que você está recebendo. O exemplo a seguir define um `FilterElement` que usa o filtro XPath para inspecionar a mensagem para um elemento chamado "element" dentro do namespace referenciado pelo prefixo de namespace "NS".

```xml
<filter name="xpath1" filterType="XPath" filterData="//ns:element" />
```

```csharp
XPathMessageFilter xpath1=new XPathMessageFilter("//ns:element");
```

Esse filtro será útil se você souber que as mensagens que você está recebendo contêm um valor específico. Por exemplo, se você estiver hospedando duas versões do mesmo serviço e souber que as mensagens endereçadas à versão mais recente do serviço contêm um valor exclusivo em um cabeçalho personalizado, você poderá criar um filtro que use XPath para navegar até esse cabeçalho e comparar o valor presente no cabeçalho para outro fornecido na configuração do filtro para determinar se o filtro é correspondente.

Como as consultas XPath geralmente contêm namespaces exclusivos, que geralmente são valores de cadeia de caracteres longos ou complexos, o filtro XPath permite que você use a tabela de namespace para definir prefixos exclusivos para seus namespaces. Para obter mais informações sobre a tabela de namespace, consulte [filtros de mensagem](message-filters.md).

Para obter mais informações sobre como criar consultas XPath, consulte [Sintaxe XPath](https://docs.microsoft.com/previous-versions/dotnet/netframework-4.0/ms256471(v=vs.100)).

## <a name="see-also"></a>Consulte também

- [Filtros de mensagem](message-filters.md)
- [Como usar filtros](how-to-use-filters.md)
