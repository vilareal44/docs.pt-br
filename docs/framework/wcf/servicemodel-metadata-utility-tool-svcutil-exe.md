---
title: Ferramenta Utilitário de Metadados ServiceModel (Svcutil.exe)
description: Saiba mais sobre o utilitário de metadados ServiceModel, que gera o código de modelo de Serviço WFC de documentos de metadados e documentos de metadados do código de modelo de serviço.
ms.date: 03/30/2017
helpviewer_keywords:
- clients [WCF], building
- endpoints [WCF]
- Svcutil.exe
- clients [WCF], consuming services
ms.assetid: 1abf3d9f-b420-46f1-b628-df238751f308
ms.openlocfilehash: 9f8e8e0239f8f8cd149bc6e8b1d7921124731087
ms.sourcegitcommit: 358a28048f36a8dca39a9fe6e6ac1f1913acadd5
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/23/2020
ms.locfileid: "85245941"
---
# <a name="servicemodel-metadata-utility-tool-svcutilexe"></a>Ferramenta Utilitário de Metadados ServiceModel (Svcutil.exe)

A ferramenta de utilitário de metadados ServiceModel é usada para gerar código de modelo de serviço a partir de documentos de metadados e documentos de metadados do código de modelo de serviço.

## <a name="svcutilexe"></a>SvcUtil.exe

A ferramenta de utilitário de metadados ServiceModel pode ser encontrada no local de instalação do SDK do Windows, especificamente *%ProgramFiles%\Microsoft SDKs\Windows\v6.0\Bin*.

### <a name="functionalities"></a>Funcionalidades

A tabela a seguir resume as várias funcionalidades fornecidas por essa ferramenta e o tópico correspondente que discute como ele é usado:

|Tarefa|Tópico|
|----------|-----------|
|Gera código de serviços em execução ou documentos estáticos de metadados.|[Gerando um cliente do WCF de metadados de serviço](./feature-details/generating-a-wcf-client-from-service-metadata.md)|
|Exporta documentos de metadados de código compilado.|[Como utilizar o Svcutil.exe para exportar metadados de código de serviço compilado](./feature-details/how-to-use-svcutil-exe-to-export-metadata-from-compiled-service-code.md)|
|Validates código compilado de serviço.|[Como usar o Svcutil.exe para validar o código de serviço compilado](./feature-details/how-to-use-svcutil-exe-to-validate-compiled-service-code.md)|
|Baixa documentos de metadados de serviços em execução.|[Como usar o Svcutil.exe para baixar documentos de metadados](./feature-details/how-to-use-svcutil-exe-to-download-metadata-documents.md)|
|Gera código de serialização.|[Como melhorar o tempo de inicialização dos aplicativos do cliente WCF usando o XmlSerializer](./feature-details/startup-time-of-wcf-client-applications-using-the-xmlserializer.md)|

> [!CAUTION]
> SvcUtil substitui os arquivos existentes em um disco se os nomes fornecidos como parâmetros forem idênticos. Isso pode incluir arquivos de código, configuração ou arquivos de metadados. Para evitar isso ao gerar arquivos de código e de configuração, use a `/mergeConfig` opção.
>
> Além disso, as `/r` `/ct` Opções e para os tipos de referência são para a geração de contratos de dados. Essas opções não funcionam ao usar XmlSerializer.

### <a name="timeout"></a>Tempo limite

A ferramenta tem um tempo limite de cinco minutos ao recuperar metadados. Esse tempo limite somente se aplica para recuperar metadados pela rede. Ele não se aplica a nenhum processamento desses metadados.

### <a name="multi-targeting"></a>Multiplataforma

A ferramenta não dá suporte à multiplataforma. Se você quiser gerar um artefato do .NET 4 de *svcutil.exe*, use o *svcutil.exe* do SDK do .NET 4. Para gerar um artefato do .NET 3.5, use o executável do SDK do .NET 3.5.

### <a name="accessing-wsdl-documents"></a>Acessando documentos WSDL

Quando você usa o Svcutil para acessar um documento WSDL que tem uma referência para um serviço de token de segurança (STS), o Svcutil faz uma chamada WS-MetadataExchange para o STS. No entanto, o serviço pode expor seus documentos WSDL usando WS-MetadataExchange ou HTTP GET. Portanto, se o STS tiver exposto apenas o documento WSDL usando HTTP GET, um cliente escrito em WinFX falhará. Para clientes escritos em .NET Framework 3,5, svcutil tenta usar WS-MetadataExchange e HTTP GET para obter o WSDL do STS.

## <a name="using-svcutilexe"></a>Usando SvcUtil.exe

### <a name="common-usages"></a>Usos comuns

A tabela a seguir mostra algumas opções comumente usadas para essa ferramenta:

|Opção|Description|
|------------|-----------------|
|/diretório\<directory>|Diretório no qual criar arquivos.<br /><br /> Padrão: o diretório atual.<br /><br /> Forma abreviada: `/d`|
|/help|Exibe a sintaxe de comando e as opções para a ferramenta.<br /><br /> Forma abreviada: `/?`|
|/noLogo|Suprime a mensagem de copyright e de banner.|
|/svcutilConfig:\<configFile>|Especifica um arquivo de configuração personalizado para usar em vez do arquivo App.config. Isso pode ser usado para registrar extensões system.serviceModel sem alterar o arquivo de configuração da ferramenta.|
|/target:\<output type>|Especifica a saída a ser gerada pela ferramenta.<br /><br /> Os valores válidos são código, metadados ou xmlSerializer.<br /><br /> Forma abreviada: `/t`|

### <a name="code-generation"></a>Geração de código

Svcutil.exe pode gerar código para contratos de serviço, clientes e tipos de dados a partir de documentos de metadados. Esses documentos de metadados podem estar em um armazenamento durável, ou serem recuperados online. A recuperação online segue o protocolo WS-Metadata Exchange ou o protocolo DISCO (para obter detalhes, consulte a seção Download de metadados).

Você pode usar a ferramenta *SvcUtil.exe* para gerar contratos de serviço e de dados com base em um documento WSDL predefinido. Use a opção /serviceContract e especifique uma URL ou local do arquivo de onde o documento WSDL pode ser baixado ou localizado. Isso gera os contratos de serviço e de dados definidos no documento WSDL que podem ser usados para implementar um serviço de reclamação. Para obter mais informações, consulte [como recuperar metadados e implementar um serviço em conformidade](./feature-details/how-to-retrieve-metadata-and-implement-a-compliant-service.md).

Para um serviço com um ponto de extremidade BasicHttpContextBinding, *Svcutil.exe* gera um basicHttpBinding com o `allowCookies` atributo definido como `true` em vez disso. Os cookies são usados para o contexto no servidor. Se você quiser gerenciar o contexto no cliente quando o serviço usa cookies, poderá modificar manualmente a configuração para usar uma associação de contexto.

> [!CAUTION]
> O Svcutil.exe gera o cliente com base no WSDL ou no arquivo de políticas recebido do serviço. O nome UPN é gerado pela concatenação do nome de usuário, " \@ " e um FQDN (nome de domínio totalmente qualificado). Entretanto, para os usuários registrados no Active Directory, esse formato é inválido e o UPN gerado pela ferramenta causa uma falha na autenticação Kerberos com a mensagem de erro “Falha ao tentar fazer logon”. Para resolver esse problema, você deve corrigir manualmente o arquivo cliente gerado por essa ferramenta.

`svcutil.exe [/t:code]  <metadataDocumentPath>* | <url>* | <epr>`

|Argumento|Descrição|
|--------------|-----------------|
|`epr`|O caminho para um arquivo XML que contém um WS-Addressing EndpointReference para um ponto de extremidade de serviço que dá suporte a WS-Metadata Exchange. Para obter mais informações, consulte a seção Download de metadados.|
|`metadataDocumentPath`|O caminho para um documento de metadados (*WSDL* ou *XSD*) que contém o contrato a ser importado para o código (. WSDL,. xsd,. WSPolicy ou. wsmex).<br /><br /> O Svcutil segue as importações e inclui quando você especifica uma URL remota para metadados. Entretanto, se você quiser processar os arquivos de metadados no sistema de arquivos local, deverá especificar todos os arquivos nesse argumento. Dessa maneira, você poderá usar o Svcutil em um ambiente de compilação onde não poderá ter dependências de rede. Você pode usar caracteres curinga (*. xsd, \* . WSDL) para esse argumento.|
|`url`|A URL para um ponto de extremidade de serviço que fornece metadados ou para um documento de metadados hospedado online. Para obter mais informações sobre como esses documentos são recuperados, consulte a seção Download de metadados.|

|Opção|Description|
|------------|-----------------|
|/async|Gera assinaturas de método síncronas e assíncronas.<br /><br /> Padrão: gerar apenas assinaturas de método síncronas.<br /><br /> Forma abreviada: `/a`|
|collectionType\<type>|Especifica o tipo de coleção de lista para um cliente WCF.<br/><br /> Padrão: o tipo de coleção é System. Array. <br /><br /> Forma abreviada: `/ct`|
|/config: \<configFile>|Especifica o nome do arquivo para o arquivo de configuração gerado.<br /><br /> Padrão: output.config|
|/dataContractOnly|Gera código somente para tipos de contrato de dados. Os tipos de contrato de serviço não são gerados.<br /><br /> Você deve especificar somente arquivos dos metadados locais para essa opção.<br /><br /> Forma abreviada: `/dconly`|
|/enableDataBinding|Implementa a interface <xref:System.ComponentModel.INotifyPropertyChanged> em todos os tipos de Contrato de Dados para habilitar a vinculação de dados.<br /><br /> Forma abreviada: `/edb`|
|/excludeType:\<type>|Especifica um nome de tipo totalmente qualificado ou qualificado do assembly a ser excluído dos tipos de contrato referenciados.<br /><br /> Ao usar essa opção junto com `/r` de DLL separadas, o nome completo da classe XSD é referenciado.<br /><br /> Forma abreviada: `/et`|
|/importXmlTypes|Configurar o serializador do Contrato de Dados para importar tipos de Contrato de não Dados como tipos IXmlSerializable.|
|/internal|Gera as classes que são marcadas como internas. Padrão: gerar somente classes públicas.<br /><br /> Forma abreviada: `/i`|
|idioma\<language>|Especifica a linguagem de programação a ser usada para gerar o código. Você deve fornecer um nome de idioma registrado no arquivo de Machine.config ou o nome totalmente qualificado de uma classe herdada de <xref:System.CodeDom.Compiler.CodeDomProvider> .<br /><br /> Valores: c#, cs, csharp, vb, visualbasic, c++, cpp<br /><br /> Padrão: csharp<br /><br /> Forma abreviada: `/l`|
|/mergeConfig|Mescla a configuração gerada em um arquivo existente, em vez de substituir o arquivo existente.|
|/messageContract|Gera tipos de contrato de mensagem.<br /><br /> Forma abreviada: `/mc`|
|/namespace\<string,string>|Especifica um mapeamento de um targetNamespace do WSDL ou do esquema XML para um namespace de CLR. Usar ' \* ' para o targetNamespace mapeia todos os targetnamespaces sem um mapeamento explícito para esse namespace CLR.<br /><br /> Para garantir que o nome do contrato da mensagem não colida com o nome da operação, você deverá qualificar a referência de tipo com `::` ou verificar se os nomes são exclusivos.<br /><br /> Padrão: derivado do namespace de destino do documento de esquema para Contratos de Dados. O namespace padrão é usado para todos os outros tipos gerados.<br /><br /> Forma abreviada: `/n` **Observação:** ao gerar tipos para usar com o XmlSerializer, apenas um único mapeamento de namespace tem suporte. Todos os tipos gerados estarão no namespace padrão ou no namespace especificado por ' * '.|
|/noConfig|Não gera arquivos de configuração.|
|/noStdLib|Não faz referência às bibliotecas padrão.<br /><br /> Padrão: Mscorlib.dll e System.servicemodel.dll são referenciados.|
|/out\<file>|Especifica o nome do arquivo para o código gerado.<br /><br /> Padrão: derivado do nome de definição WSDL, serviço WSDL ou namespace de destino de um dos esquemas.<br /><br /> Forma abreviada: `/o`|
|/Reference\<file path>|Os tipos de referência no assembly especificado. Ao gerar clientes, use esta opção para especificar os assemblies que podem conter tipos que representam os metadados que estão sendo importados.<br /><br /> Você não pode especificar os contratos de mensagem e os tipos <xref:System.Xml.Serialization.XmlSerializer> usando essa opção.<br /><br /> Se <xref:System.DateTimeOffset> for referenciado, esse tipo é usado em vez de gerar um novo tipo. Se o aplicativo for gravado usando .NET Framework 3,5, SvcUtil.exe referências <xref:System.DateTimeOffset> automaticamente.<br /><br /> Forma abreviada: `/r`|
|/serializable|Gera classes marcadas com o atributo Serializable.<br /><br /> Forma abreviada: `/s`|
|/serviceContract|Gerar código somente para contratos de serviço. A classe e a configuração do cliente não serão geradas<br /><br /> Forma abreviada: `/sc`|
|/serializer:Auto|Selecionar automaticamente o serializador. Isso tentará usar o serializador de contrato de dados e usará o XmlSerializer se isso falhar.<br /><br /> Forma abreviada: `/ser`|
|/serializer:DataContractSerializer|Gera os tipos de dados que usam o serializador Contrato de Dados para serialização e desserialização.<br /><br /> Forma abreviada: `/ser:DataContractSerializer`|
|/serializer:XmlSerializer|Gera os tipos de dados que usam o <xref:System.Xml.Serialization.XmlSerializer> para serialização e desserialização.<br /><br /> Forma abreviada: `/ser:XmlSerializer`|
|/targetClientVersion|Especifique qual versão do .NET Framework o aplicativo está direcionando. Os valores válidos são `Version30` e `Version35`. O valor padrão é `Version30`.<br /><br /> Forma abreviada: `/tcv`<br /><br /> `Version30`: Use `/tcv:Version30` se você estiver gerando código para clientes que usam o WinFX.<br /><br /> `Version35`: Use `/tcv:Version35` se você estiver gerando código para clientes que usam o .NET Framework 3,5. Ao usar `/tcv:Version35` com a opção `/async`, os métodos assíncronos baseados em evento e baseados em retorno de chamada/representante são gerados. Além disso, o suporte para DataSets habilitados para LINQ e <xref:System.DateTimeOffset> está habilitado.|
|/wrapped|Controla se a diferença entre maiúsculas e minúsculas é usada para documentos de estilo literal com parâmetros encapsulados. Use a opção **/wrapped** com a ferramenta de [Utilitário de metadados do modelo de serviço (Svcutil.exe)](servicemodel-metadata-utility-tool-svcutil-exe.md) para especificar o uso normal de maiúsculas e minúsculas.|

> [!NOTE]
> Quando a associação de serviço é uma das associações fornecidas pelo sistema (consulte [associações fornecidas pelo sistema](system-provided-bindings.md)) e a <xref:System.ServiceModel.ServiceContractAttribute.ProtectionLevel%2A> propriedade é definida como `None` ou `Sign` , SvcUtil gera um arquivo de configuração usando o [\<customBinding>](../configure-apps/file-schema/wcf/custombinding.md) elemento em vez do elemento esperado fornecido pelo sistema. Por exemplo, se o serviço usa o elemento `<wsHttpBinding>` com `ProtectionLevel` definido como `Sign`, a configuração gerada tem `<customBinding>` na seção de associação em vez de `<wsHttpBinding>`. Para obter mais informações sobre o nível de proteção, consulte [noções básicas](understanding-protection-level.md)sobre o nível de proteção.

### <a name="metadata-export"></a>Exportação de metadados

O Svcutil.exe pode exportar os metadados para serviços, contratos e tipos de dados em assemblies compilados. Para exportar metadados para um serviço, você deve usar a opção `/serviceName` para especificar o serviço que você gostaria de exportar. Para exportar todos os tipos de contrato de dados dentro de um assembly, você deverá usar a opção `/dataContractOnly`. Por padrão, os metadados são exportados para todos os contratos de serviço nos assemblies de entrada.

`svcutil.exe [/t:metadata] [/serviceName:<serviceConfigName>] [/dataContractOnly] <assemblyPath>*`

|Argumento|Descrição|
|--------------|-----------------|
|`assemblyPath`|Especifica o caminho para um assembly que contém serviços, contratos ou tipos de contrato de dados para ser exportados. Curingas padrão de linha de comando podem ser usados para fornecer vários arquivos como entrada.|

|Opção|Description|
|------------|-----------------|
|ServiceName\<serviceConfigName>|Especifica o nome da configuração de um serviço a ser exportado. Se esta opção for usada, um assembly executável com um arquivo de configuração associado deverá ser passado como entrada. O Svcutil.exe procura todos os arquivos de configuração associados para a configuração do serviço. Se os arquivos de configuração contiverem tipos de extensão, os assemblies que contêm esses tipos deverão estar no GAC ou ser fornecidos explicitamente usando a opção `/reference`.|
|/Reference\<file path>|Adiciona o assembly especificado ao conjunto de assemblies usados para resolver referências de tipo. Se você estiver exportando ou validando um serviço que usa extensões de terceiros (comportamentos, associações e BindingElements) registradas em configuração, use esta opção para localizar assemblies de extensão que não estão no GAC.<br /><br /> Forma abreviada: `/r`|
|/dataContractOnly|Opera somente em tipos de contrato de dados. Contratos de serviço não são processados.<br /><br /> Você deve especificar somente arquivos dos metadados locais para essa opção.<br /><br /> Forma abreviada: `/dconly`|
|/excludeType:\<type>|Especifica o nome totalmente qualificado ou qualificado do assembly de um tipo a ser excluído da exportação. Essa opção pode ser usada ao exportar metadados para um serviço, ou um conjunto de contratos de serviço para excluir tipos de serem exportados. Esta opção não pode ser usada junto com a opção `/dconly`.<br /><br /> Quando você tiver um único assembly contendo vários serviços, e cada um usar classes separadas com o mesmo nome XSD, deverá especificar o nome do serviço em vez do nome da classe XSD para essa opção.<br /><br /> XSD ou tipos de contrato de dados não têm suporte.<br /><br /> Forma abreviada: `/et`|

### <a name="service-validation"></a>Validação de serviço

A validação pode ser usada para detectar erros em implementações de serviço sem hospedar o serviço. Você deve usar a opção `/serviceName` para indicar o serviço que deseja validar.

`svcutil.exe /validate /serviceName:<serviceConfigName>  <assemblyPath>*`

|Argumento|Descrição|
|--------------|-----------------|
|`assemblyPath`|Especifica o caminho para um assembly que contém os tipos de serviço a serem validados. O assembly deve ter um arquivo de configuração associado para fornecer a configuração do serviço. Curingas padrão de linha de comando podem ser usados para fornecer vários assemblies.|

|Opção|Description|
|------------|-----------------|
|/validate|Valida uma implementação de serviço especificada pela opção `/serviceName`. Se esta opção for usada, um assembly executável com um arquivo de configuração associado deverá ser passado como entrada.<br /><br /> Forma abreviada: `/v`|
|ServiceName\<serviceConfigName>|Especifica o nome da configuração de um serviço a ser validado. O Svcutil.exe procura todos os arquivos de configuração associados de todos os assemblies de entrada para a configuração do serviço. Se os arquivos de configuração contiverem tipos de extensão, os assemblies que contêm esses tipos deverão estar no GAC ou ser fornecidos explicitamente usando a opção `/reference`.|
|/Reference\<file path>|Adiciona o assembly especificado ao conjunto de assemblies usados para resolver referências de tipo. Se você estiver exportando ou validando um serviço que usa extensões de terceiros (comportamentos, associações e BindingElements) registradas em configuração, use esta opção para localizar assemblies de extensão que não estão no GAC.<br /><br /> Forma abreviada: `/r`|
|/dataContractOnly|Opera somente em tipos de contrato de dados. Contratos de serviço não são processados.<br /><br /> Você deve especificar somente arquivos dos metadados locais para essa opção.<br /><br /> Forma abreviada: `/dconly`|
|/excludeType:\<type>|Especifica o nome totalmente qualificado ou qualificado do assembly de um tipo a ser excluído da validação.<br /><br /> Forma abreviada: `/et`|

### <a name="metadata-download"></a>Download de metadados

O Svcutil.exe pode ser usado para baixar metadados de serviços em execução e salvar os metadados em arquivos locais. Para baixar metadados, você deverá especificar a opção `/t:metadata`. Caso contrário, o código do cliente será gerado. Para os esquemas de URL de HTTP e HTTPS, o Svcutil.exe tenta recuperar metadados usando WS-Metadata Exchange e DISCO. Para todos os outros esquemas de URL, o Svcutil.exe usa apenas WS-Metadata Exchange.

O Svcutil emite as seguintes solicitações de metadados para recuperar metadados simultaneamente.

- Solicitação de MEX (WS-Transfer) para o endereço fornecido

- A solicitação de MEX para o endereço fornecido com /mex adicionado

- Solicitação de DISCO (usando o DiscoveryClientProtocol de ASMX) para o endereço fornecido.

Por padrão, o Svcutil.exe usa as associações definidas na classe <xref:System.ServiceModel.Description.MetadataExchangeBindings> para fazer solicitações de MEX. Para configurar a associação usada para WS-Metadata Exchange, você deverá definir um ponto de extremidade de cliente na configuração usando o contrato de IMetadataExchange. Isso pode ser definido no arquivo de configuração de Svcutil.exe ou outro arquivo de configuração especificado usando a opção `/svcutilConfig`.

`svcutil.exe /t:metadata  <url>* | <epr>`

|Argumento|Descrição|
|--------------|-----------------|
|`url`|A URL para um ponto de extremidade de serviço que fornece metadados ou para um documento de metadados hospedado online.|
|`epr`|O caminho para um arquivo XML que contém um WS-Addressing EndpointReference para um ponto de extremidade de serviço que dá suporte a WS-Metadata Exchange.|

### <a name="xmlserializer-type-generation"></a>Geração de tipo de XmlSerializer

Os serviços e os aplicativos cliente que usam tipos de dados que são serializados usando <xref:System.Xml.Serialization.XmlSerializer> geram e compilam o código de serialização para esses tipos de dados em runtime, o que pode levar a um desempenho de inicialização lento.

> [!NOTE]
> O código de serialização pré-gerado somente pode ser usado em aplicativos cliente e não em serviços.

O Svcutil.exe pode gerar o código de serialização de C# necessário a partir de assemblies compilados para o aplicativo, melhorando então o desempenho de inicialização para esses aplicativos. Para obter mais informações, consulte [como: melhorar o tempo de inicialização de aplicativos cliente WCF usando o XmlSerializer](./feature-details/startup-time-of-wcf-client-applications-using-the-xmlserializer.md).

> [!NOTE]
> O Svcutil.exe somente gera código para os tipos usados por contratos de serviço localizados em assemblies de entrada.

`svcutil.exe /t:xmlSerializer  <assemblyPath>*`

|Argumento|Descrição|
|--------------|-----------------|
|`assemblyPath`|Especifica o caminho para um assembly que contém tipos de contrato de serviço. Os tipos de serialização são gerados para todos os tipos XML serializáveis em cada contrato.|

|Opção|Description|
|------------|-----------------|
|/Reference\<file path>|Adiciona o assembly especificado ao conjunto de assemblies usados para resolver referências de tipo.<br /><br /> Forma abreviada: `/r`|
|/excludeType:\<type>|Especifica o nome totalmente qualificado ou qualificado do assembly de um tipo a ser excluído da exportação ou validação.<br /><br /> Forma abreviada: `/et`|
|/out\<file>|Especifica o nome do arquivo para o código gerado. Essa opção será ignorada quando vários assemblies são passados como entrada para a ferramenta.<br /><br /> Padrão: derivado do nome do assembly.<br /><br /> Forma abreviada: `/o`|
|/UseSerializerForFaults|Especifica que o <xref:System.Xml.Serialization.XmlSerializer> deve ser usado para ler e gravar falhas, em vez do <xref:System.Runtime.Serialization.DataContractSerializer> padrão.|

## <a name="examples"></a>Exemplos

O comando a seguir gera o código do cliente de um serviço em execução ou documentos de metadados online.

`svcutil http://service/metadataEndpoint`

O comando a seguir gera o código do cliente de documentos de metadados locais.

`svcutil *.wsdl *.xsd /language:C#`

O comando a seguir gera os tipos de contrato de dados no Visual Basic de documentos de esquema locais.

`svcutil /dconly *.xsd /language:VB`

O comando a seguir baixa os documentos de metadados de serviços em execução.

`svcutil /t:metadata http://service/metadataEndpoint`

O comando a seguir gera documentos de metadados para contratos de serviço e tipos associados em um assembly.

`svcutil myAssembly.dll`

O comando a seguir gera documentos de metadados para um serviço e todos os contratos de serviço associados e tipos de dados em um assembly.

`svcutil myServiceHost.exe /serviceName:myServiceName`

O comando a seguir gera documentos de metadados para tipos de dados em um assembly.

`svcutil myServiceHost.exe /dconly`

O comando a seguir verifica a hospedagem do serviço.

`svcutil /validate /serviceName:myServiceName myServiceHost.exe`

O comando a seguir gera tipos de serialização para os tipos <xref:System.Xml.Serialization.XmlSerializer> usados por todos os contratos de serviço no assembly.

`svcutil /t:xmlserializer myContractLibrary.exe`

## <a name="maximum-nametable-character-count-quota"></a>Cota máxima da conta de caracteres Nametable

Ao usar o svcutil para gerar metadados para um serviço, você poderá receber a seguinte mensagem:

Erro: não é possível obter metadados da `http://localhost:8000/somesservice/mex` cota máxima de contagem de caracteres NameTable (16384) foi excedido durante a leitura de dados XML. Nametable é uma estrutura de dados usada para armazenar cadeias de caracteres encontradas durante o processamento do XML - documentos XML longos com nomes de elementos, nomes de atributos e valores de atributos não repetidos, podem disparar essa cota. Essa cota pode ser aumentada com a alteração da propriedade MaxNameTableCharCount no objeto XmlDictionaryReaderQuotas usado na criação da leitora XML.

Este erro pode ser causado por um serviço que retorna um arquivo WSDL grande quando você solicita seus metadados. O problema é que a cota de caracteres para a ferramenta svcutil.exe foi excedida. Esse valor é definido para ajudar a evitar ataques de negação de serviço (dos). Você pode aumentar essa cota especificando o seguinte arquivo de configuração para o svcutil.

O arquivo de configuração a seguir mostra como configurar as cotas do leitor para o svcutil

```xml
<?xml version="1.0" encoding="utf-8"?>
<configuration>
    <system.serviceModel>
        <bindings>
            <customBinding>
                <binding name="MyBinding">
                    <textMessageEncoding>
                        <readerQuotas maxDepth="2147483647" maxStringContentLength="2147483647"
                            maxArrayLength="2147483647" maxBytesPerRead="2147483647" maxNameTableCharCount="2147483647" />
                    </textMessageEncoding>
                    <httpTransport maxReceivedMessageSize="2147483647" maxBufferSize="2147483647" />
                </binding>
            </customBinding>
        </bindings>
        <client>
            <endpoint binding="customBinding" bindingConfiguration="MyBinding"
                contract="IMetadataExchange"
                name="http" />
        </client>
    </system.serviceModel>
</configuration>
```

Crie um novo arquivo chamado svcutil.exe.config e copie o código de exemplo XML nele. Em seguida, coloque o arquivo no mesmo diretório que svcutil.exe. Na próxima vez que svcutil.exe é executado, ele utilizará as novas configurações.

## <a name="security-concerns"></a>Problemas de segurança

Você deverá usar a lista de controle de acesso (ACL) apropriado para proteger a pasta de instalação do Svcutil.exe, Svcutil.config, e os arquivos que estão sendo apontados por `/svcutilConfig`. Isso pode impedir as extensões mal-intencionadas de serem registradas e executadas.

Além disso, para minimizar a chance de comprometimento da segurança, você não deve adicionar extensões não confiáveis para fazer parte do sistema ou usar provedores de código não confiáveis com Svcutil.exe.

Finalmente, você não deverá usar a ferramenta na camada intermediária do seu aplicativo, porque pode causar a negação de serviço para o processo atual.

## <a name="see-also"></a>Veja também

- <xref:System.Runtime.Serialization.DataContractAttribute>
- <xref:System.Runtime.Serialization.DataMemberAttribute>
- [Como criar um cliente](how-to-create-a-wcf-client.md)
