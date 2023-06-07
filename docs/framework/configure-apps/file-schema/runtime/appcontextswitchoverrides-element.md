---
title: Elemento AppContextSwitchOverrides
ms.date: 04/18/2019
helpviewer_keywords:
- AppContextSwitchOverrides
- compatibility switches
- configuration switches
- configuration
ms.assetid: 4ce07f47-7ddb-4d91-b067-501bd8b88752
ms.openlocfilehash: 394523e81da96d596a00010a393b9a034c0d552f
ms.sourcegitcommit: 8bfeb5930ca48b2ee6053f16082dcaf24d46d221
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/18/2020
ms.locfileid: "88558745"
---
# <a name="appcontextswitchoverrides-element"></a>Elemento \<AppContextSwitchOverrides>

Define uma ou mais opções usadas pela classe <xref:System.AppContext> para fornecer um mecanismo de recusa de uma nova funcionalidade.

[**\<configuration>**](../configuration-element.md)\
&nbsp;&nbsp;[**\<runtime>**](runtime-element.md)\
&nbsp;&nbsp;&nbsp;&nbsp;**\<AppContextSwitchOverrides>**

## <a name="syntax"></a>Sintaxe

```xml
<AppContextSwitchOverrides value="name1=value1[[;name2=value2];...]" />
```

## <a name="attributes-and-elements"></a>Atributos e elementos
 As seções a seguir descrevem atributos, elementos filho e elementos pai.

### <a name="attributes"></a>Atributos

|Atributo|Descrição|
|---------------|-----------------|
|`value`|Atributo obrigatório.<br /><br /> Define um ou mais nomes de comutador e seus valores Boolianos associados.|

### <a name="value-attribute"></a>Atributo de valor

|Valor|Descrição|
|-----------|-----------------|
|"nome = valor"|Um nome de opção predefinido junto com seu valor ( `true` ou `false` ). Vários pares de nome/valor de comutador são separados por ponto e vírgula (";"). Para obter uma lista de nomes de comutador predefinidos com suporte pelo .NET Framework, consulte a seção comentários.|

### <a name="child-elements"></a>Elementos filho
 Nenhum.

### <a name="parent-elements"></a>Elementos pai

|Elemento|Descrição|
|-------------|-----------------|
|`configuration`|O elemento raiz em cada arquivo de configuração usado pelos aplicativos do Common Language Runtime e .NET Framework.|
|`runtime`|Contém informações sobre opções de inicialização do runtime.|

## <a name="remarks"></a>Comentários
 A partir do .NET Framework 4,6, o `<AppContextSwitchOverrides>` elemento em um arquivo de configuração permite que os chamadores de uma API determinem se seu aplicativo pode aproveitar a nova funcionalidade ou preservar a compatibilidade com versões anteriores de uma biblioteca. Por exemplo, se o comportamento de uma API tiver sido alterado entre duas versões de uma biblioteca, o `<AppContextSwitchOverrides>` elemento permitirá que os chamadores dessa API recusem o novo comportamento nas versões da biblioteca que dão suporte à nova funcionalidade. Para aplicativos que chamam APIs no .NET Framework, o `<AppContextSwitchOverrides>` elemento também pode permitir chamadores cujos aplicativos se destinam a uma versão anterior do .NET Framework para aceitar novas funcionalidades se seu aplicativo estiver em execução em uma versão do .NET Framework que inclui essa funcionalidade.

 O `value` atributo do `<AppContextSwitchOverrides>` elemento consiste em uma única cadeia de caracteres que consiste em um ou mais pares de nome/valor delimitados por ponto e vírgula.  Cada nome identifica uma opção de compatibilidade e seu valor correspondente é um booliano ( `true` ou `false` ) que indica se a opção está definida. Por padrão, o comutador é `false` , e as bibliotecas fornecem a nova funcionalidade. Eles só fornecem a funcionalidade anterior se a opção for definida (ou seja, seu valor for `true` ). Isso permite que as bibliotecas forneçam um novo comportamento para uma API existente, permitindo aos chamadores que dependem do comportamento anterior para recusar a nova funcionalidade.

O .NET Framework dá suporte às seguintes opções:

|Nome do comutador|Descrição|Incluída|
|-----------------|-----------------|----------------|
|`Switch.MS.Internal.`<br/>`DoNotApplyLayoutRoundingToMarginsAndBorderThickness`|Controla se Windows Presentation Foundation usa um algoritmo herdado para o layout de controle. Para saber mais, confira [Mitigação: layout de WPF](../../../migration-guide/mitigation-wpf-layout.md).|.NET Framework 4.6|
|`Switch.MS.Internal.`<br/>`UseSha1AsDefaultHashAlgorithmForDigitalSignatures`|Controla se o algoritmo padrão usado para assinar partes de um pacote por PackageDigitalSignatureManager é SHA1 ou SHA256.<br>Em razão de problemas de colisão com SHA1, a Microsoft recomenda SHA256.|.NET Framework 4.7.1|
|`Switch.System.Activities.`<br/>`UseMD5CryptoServiceProviderForWFDebugger`|Quando definido como `false` , permite a depuração de projetos de fluxo de trabalho baseados em XAML com o Visual Studio quando o FIPS está habilitado. Sem ele, um <xref:System.NullReferenceException> é lançado em chamadas para métodos no assembly System. Activities.|.NET Framework 4.7|
|`Switch.System.Activities.`<br/>`UseMD5ForWFDebugger`|Controla se a soma de verificação de uma instância de fluxo de trabalho no depurador usa MD5 ou SHA1. | .NET Framework 4.7|
|`Switch.System.Activities.`<br/>`UseSHA1HashForDebuggerSymbols`|Controla se o hash de soma de verificação do fluxo de trabalho usa o algoritmo SHA1 introduzido como o padrão no .NET Framework 4,7 ( `true` ), ou se ele usa o algoritmo SHA256 padrão introduzido como o padrão em .NET Framework 4,8 ( `false` ).<br>Em razão de problemas de colisão com SHA1, a Microsoft recomenda SHA256.|.NET Framework 4.8|
|`Switch.System.Diagnostics.`<br/>`IgnorePortablePDBsInStackTraces`|Controla se os rastreamentos de pilha obtêm ao usar PDBs portáteis podem incluir informações de arquivo e linha de origem. `false` para incluir informações de arquivo e linha de origem; caso contrário, `true` .|.NET Framework 4.7.2|
|`Switch.System.Drawing.`<br/>`DontSupportPngFramesInIcons`|Controla se o <xref:System.Drawing.Icon.ToBitmap%2A?displayProperty=nameWithType> método gera uma exceção quando um <xref:System.Drawing.Icon> objeto tem quadros png. Para saber mais, confira [Mitigation: PNG Frames in Icon Objects](../../../migration-guide/mitigation-png-frames-in-icon-objects.md) (Mitigação: quadros PNG em objetos de ícone).|.NET Framework 4.6|
|`Switch.System.Drawing.Text.`<br/>`DoNotRemoveGdiFontsResourcesFromFontCollection`|Determina se <xref:System.Drawing.Text.PrivateFontCollection?displayProperty=nameWithType> os objetos são descartados corretamente quando adicionados à coleção pelo <xref:System.Drawing.Text.PrivateFontCollection.AddFontFile(System.String)?displayProperty=nameWithType> método. `true` para manter o comportamento herdado; `false` para descartar todos os objetos de fonte privada. |.NET Framework 4.7.2|
|`Switch.System.Drawing.Printing.`<br>`OptimizePrintPreview`|Controla se o desempenho do <xref:System.Windows.Forms.PrintPreviewDialog> é otimizado para impressoras de rede. Para obter mais informações, consulte [visão geral do controle PrintPreviewDialog](../../../winforms/controls/printpreviewdialog-control-overview-windows-forms.md).|.NET Framework 4.6|
|`Switch.System.Globalization.EnforceJapaneseEraYearRanges`|Controla se o intervalo de ano verifica se o calendário japonês apagar está imposto. `true` para impor verificações de intervalo de ano e `false` para desabilitá-las (o comportamento padrão). Para obter mais informações, consulte [trabalhando com calendários](../../../../standard/datetime/working-with-calendars.md).|.NET Framework 4.6|
|`Switch.System.Globalization.EnforceLegacyJapaneseDateParsing`|Controla se apenas "1" é reconhecido como o primeiro ano de uma era do calendário japonês em operações de análise. `true` para reconhecer apenas "1"; `false` para reconhecer "1" ou Gannen (o comportamento padrão). Para obter mais informações, consulte [trabalhando com calendários](../../../../standard/datetime/working-with-calendars.md).|.NET Framework 4.6|
|`Switch.System.Globalization.FormatJapaneseFirstYearAsANumber`|Controla se o primeiro ano de uma era do calendário japonês é representado como "1" ou Gannen nas operações de formatação. `true` para formatar o primeiro ano da era como "1"; `false` para formatá-lo como Gannen (o comportamento padrão). Para obter mais informações, consulte [trabalhando com calendários](../../../../standard/datetime/working-with-calendars.md).|.NET Framework 4.6|
|`Switch.System.Globalization.NoAsyncCurrentCulture`|Controla se as operações assíncronas não fluem do contexto do thread de chamada. Para obter mais informações, consulte o [fluxo CurrentCulture e CurrentUICulture entre as tarefas](../../../migration-guide/retargeting/4.5.2-4.6.md#currentculture-and-currentuiculture-flow-across-tasks).|.NET Framework 4.6|
|`Switch.System.IdentityModel.`<br/>`DisableMultipleDNSEntriesInSANCertificate`|Controla se o <xref:System.IdentityModel.Claims.X509CertificateClaimSet.FindClaims%2A?displayProperty=nameWithType> método tenta corresponder ao tipo de declaração somente com a última entrada DNS. Para saber mais, confira [Mitigation: X509CertificateClaimSet.FindClaims Method](../../../migration-guide/mitigation-x509certificateclaimset-findclaims-method.md) (Mitigação: método X509CertificateClaimSet.FindClaims).|.NET Framework 4.6.1|
|`Switch.System.IdentityModel.`<br/>`EnableCachedEmptyDefaultAuthorizationContext`|Controla se o AuthorizationContext. Empty deve ser permitido para retornar um objeto mutável.|.NET Framework 4.6|
|`Switch.System.IO.BlockLongPaths`|Controla se os caminhos com mais de `MAX_PATH` (260 caracteres) lançam um <xref:System.IO.PathTooLongException> . Para obter mais informações, consulte [suporte de caminho longo](../../../migration-guide/retargeting/4.6.1-4.6.2.md#long-path-support).|.NET Framework 4.6.2|
|`Switch.System.IO.Compression.`<br/>`DoNotUseNativeZipLibraryForDecompression`|Controla se as rotinas de sistema operacional nativas são usadas para descompactação pela <xref:System.IO.Compression.DeflateStream> classe. `false` para usar APIs nativas; `true` para usar a <xref:System.IO.Compression.DeflateStream> implementação.|.NET Framework 4.7.2|
|`Switch.System.IO.Compression.ZipFile.`<br/>`UseBackslash`|Usa a barra invertida (" \\ ") em vez da barra "/") como o separador de caminho na <xref:System.IO.Compression.ZipArchiveEntry.FullName%2A?displayProperty=nameWithType> propriedade. Para obter mais informações, consulte  [atenuação: separador de caminho ZipArchiveEntry. FullName](../../../migration-guide/mitigation-ziparchiveentry-fullname-path-separator.md).|.NET Framework 4.6.1|
|`Switch.System.IO.Ports.`<br/>`DoNotCatchSerialStreamThreadExceptions`|Controla se as exceções do sistema operacional que são geradas em threads em segundo plano criados com <xref:System.IO.Ports.SerialPort> fluxos encerram o processo.|.NET Framework 4.7.1|
|`Switch.System.IO.`<br/>`UseLegacyPathHandling`|Controla se a normalização de caminho herdado é usada e caminhos de URI são suportados pelos <xref:System.IO.Path.GetDirectoryName%2A?displayProperty=nameWithType> <xref:System.IO.Path.GetPathRoot%2A?displayProperty=nameWithType> métodos e. Para obter mais informações, consulte [mitigação: normalização](../../../migration-guide/mitigation-path-normalization.md) e [mitigação de caminho: verificações de ponto-e-vírgula](../../../migration-guide/mitigation-path-colon-checks.md).|.NET Framework 4.6.2|
|`Switch.System.`<br/>`MemberDescriptorEqualsReturnsFalseIfEquivalent`|Controla se um teste de igualdade compara a <xref:System.ComponentModel.MemberDescriptor.Category%2A?displayProperty=nameWithType> propriedade de um objeto com a <xref:System.ComponentModel.MemberDescriptor.Description%2A?displayProperty=nameWithType> Propriedade do segundo objeto. Para obter mais informações, consulte [implementação incorreta de MemberDescriptor. Equals](../../../migration-guide/retargeting/4.6.1-4.6.2.md#incorrect-implementation-of-memberdescriptorequals).|.NET Framework 4.6.2|
 `Switch.System.Net.`<br/>`DontCheckCertificateEKUs`|Desabilita a validação de OID (identificador de objeto) de EKU (uso avançado de chave). Uma extensão de EKU (uso avançado de chave) é uma coleção de OIDs (identificadores de objeto) que indicam os aplicativos que usam a chave.|.NET Framework 4.6|
|`Switch.System.Net.`<br/>`DontEnableSchSendAuxRecord`|Desabilita a atenuação do navegador TLS 1.0 em relação à mitigação de SSL/TLS (fera) ao desabilitar o uso de SCH_SEND_AUX_RECORD.|.NET Framework 4.6|
|`Switch.System.Net.`<br/>`DontEnableSchUseStrongCrypto`|Controla se as <xref:System.Net.ServicePointManager?displayProperty=nameWithType> <xref:System.Net.Security.SslStream?displayProperty=nameWithType> classes e podem usar o protocolo SSL 3,0. Para saber mais, confira [Mitigation: TLS Protocols](../../../migration-guide/mitigation-tls-protocols.md) (Mitigação: protocolos TLS).|.NET Framework 4.6|
|`Switch.System.Net.`<br/>`DontEnableSystemDefaultTlsVersions`|Desabilita as versões do SystemDefault TLS retornando de volta para um padrão de Tls12, Tls11, TLS.|.NET Framework 4.7|
|`Switch.System.Net.`<br/>`DontEnableTlsAlerts`|Desabilita os alertas do lado do servidor SslStream TLS.|.NET Framework 4.7|
|`Switch.System.Runtime.InteropServices.`<br/>`DoNotMarshalOutByrefSafeArrayOnInvoke`|Controla se os parâmetros de ByRef SafeArray em eventos de interoperabilidade COM fazem marshaling de volta para o código nativo ( `false` ) ou se o marshaling de volta para o código nativo está desabilitado ( `true` ).|.NET Framework 4.8|
|`Switch.System.Runtime.Serialization.`<br/>`DoNotUseECMAScriptV6EscapeControlCharacter` |Controla se o [DataContractJsonSerializer](xref:System.Runtime.Serialization.Json.DataContractJsonSerializer) serializa alguns caracteres de controle com base nos padrões do ECMAScript V6 e do V8. Para obter mais informações, consulte [Mitigação: serialização de caracteres de controle com o DataContractJsonSerializer](../../../migration-guide/mitigation-serialization-control-characters.md)| .NET Framework 4.7 |
|`Switch.System.Runtime.Serialization.`<br/>`DoNotUseTimeZoneInfo`|Controla se o <xref:System.Runtime.Serialization.Json.DataContractJsonSerializer> dá suporte a vários ajustes ou apenas a um único ajuste para um fuso horário. Se `true` , ele usa o <xref:System.TimeZoneInfo> tipo para serializar e desserializar dados de data e hora; caso contrário, ele usa o <xref:System.TimeZone> tipo, que não dá suporte a várias regras de ajuste.|.NET Framework 4.6.2|
|`Switch.System.Runtime.Serialization.UseNewMaxArraySize`|Controla se <xref:System.Runtime.Serialization.ObjectManager?displayProperty=nameWithType> o usa um tamanho de matriz maior durante a serialização e a desserialização do objeto. Defina essa opção como `true` para melhorar o desempenho de serialização e desserialização de grafos de objeto grande por tipos como <xref:System.Runtime.Serialization.Formatters.Binary.BinaryFormatter> . |.NET Framework 4.7.2|
|`Switch.System.Security.ClaimsIdentity.`<br/>`SetActorAsReferenceWhenCopyingClaimsIdentity`|Controla se o <xref:System.Security.Claims.ClaimsIdentity.%23ctor%28System.Security.Principal.IIdentity%29> construtor define a propriedade do novo objeto <xref:System.Security.Claims.ClaimsIdentity.Actor%2A?displayProperty=nameWithType> com uma referência de objeto existente. Para saber mais, confira [Mitigation: ClaimsIdentity Constructor](../../../migration-guide/retargeting/4.6.1-4.6.2.md) (Mitigação: construtor ClaimsIdentity).|.NET Framework 4.6.2|
|`Switch.System.Security.Cryptography.`<br/>`AesCryptoServiceProvider.DontCorrectlyResetDecryptor`|Controla se a tentativa de reutilizar um <xref:System.Security.Cryptography.AesCryptoServiceProvider> descriptografador gera um <xref:System.Security.Cryptography.CryptographicException> . Para obter mais informações, consulte [o descriptografador AesCryptoServiceProvider fornece uma transformação reutilizável](../../../migration-guide/retargeting/4.6.1-4.6.2.md#aescryptoserviceprovider-decryptor-provides-a-reusable-transform).|.NET Framework 4.6.2|
|`Switch.System.Security.Cryptography.`<br/>`DoNotAddrOfCspParentWindowHandle`|Controla se o valor da propriedade [CspParameters. ParentWindowHandle](xref:System.Security.Cryptography.CspParameters.ParentWindowHandle) é um [IntPtr](xref:System.IntPtr) que representa o local da memória de um identificador de janela ou se é um identificador de janela (um HWND). Para obter mais informações, consulte [Mitigação: CspParameters.ParentWindowHandle espera um HWND](../../../migration-guide/retargeting/4.6.2-4.7.md#cspparametersparentwindowhandle-now-expects-hwnd-value). |.NET Framework 4.7|
|`Switch.System.Security.Cryptography.`<br/>`UseLegacyFipsThrow`|Controla se o uso de classes de criptografia gerenciadas no modo FIPS gera um <xref:System.Security.Cryptography.CryptographicException> ( `true` ) ou se baseia na implementação das bibliotecas do sistema ( `false` ).|.NET Framework 4.8|
|`Switch.System.Security.Cryptography.Pkcs.`<br/>`UseInsecureHashAlgorithms`|Determina se o padrão para algumas operações SignedCMS é SHA1 ou SHA256.<br>Em razão de problemas de colisão com SHA1, a Microsoft recomenda SHA256.|.NET Framework 4.7.1|
|`Switch.System.Security.Cryptography.X509Certificates.`<br/>`ECDsaCertificateExtensions.UseLegacyPublicKeyReader`|Controla se o <xref:System.Security.Cryptography.X509Certificates.ECDsaCertificateExtensions.GetECDsaPublicKey%2A?displayProperty=nameWithType> método manipula corretamente todas as curvas nomeadas com suporte pelo sistema operacional ( `false` ) ou reverte para o comportamento herdado.|.NET Framework 4.8|
|`Switch.System.Security.Cryptography.Xml.`<br/>`UseInsecureHashAlgorithms`|Determina se o padrão para algumas operações SignedXML é SHA1 ou SHA256.<br>Em razão de problemas de colisão com SHA1, a Microsoft recomenda SHA256.|.NET Framework 4.7.1|
|`Switch.System.ServiceModel.`<br/>`AllowUnsignedToHeader`|Determina se o `TransportWithMessageCredential` modo de segurança permite mensagens com um cabeçalho "para" não assinado. Essa é uma opção opcional. Para obter mais informações, consulte [alterações de tempo de execução no .NET Framework 4.6.1](../../../migration-guide/runtime/4.5.2-4.6.1.md#windows-communication-foundation-wcf).|.NET Framework 4.6.1|
|`Switch.System.ServiceModel.`<br/>`DisableAddressHeaderCollectionValidation`>|Controla se o <xref:System.ServiceModel.Channels.AddressHeaderCollection.%23ctor(System.Collections.Generic.IEnumerable{System.ServiceModel.Channels.AddressHeader})> Construtor gera um <xref:System.ArgumentException> se um dos elementos é `null` .|.NET Framework 4.7.1|
|`Switch.System.ServiceModel.`<br />`DisableCngCertificates`|Determina se a tentativa de usar certificados X509 com um provedor de armazenamento de chaves CSG gera uma exceção. Para obter mais informações, consulte a [segurança de transporte do WCF dá suporte a certificados armazenados usando a CNG](../../../migration-guide/retargeting/4.6.1-4.6.2.md#wcf-transport-security-supports-certificates-stored-using-cng).|.NET Framework 4.6.1|
|`Switch.System.ServiceModel.`<br/>`DisableExplicitConnectionCloseHeader`|Ao usar o transporte HTTP com um serviço hospedado internamente, definir esse valor como `true` faz com que o WCF ignore um aplicativo adicionando o `Connection: close` cabeçalho aos cabeçalhos de resposta de uma solicitação. Definir esse valor como `false` permite adicionar o `Connection: close` cabeçalho aos cabeçalhos de resposta, o que resulta no fechamento do soquete de solicitação depois que uma resposta é enviada.|.NET Framework 4.6|
|`Switch.System.ServiceModel.`<br/>`DisableOperationContextAsyncFlow`|Manipula deadlocks que resultam da restrição de instâncias de um serviço reentrante para um único thread de execução de cada vez.|.NET Framework 4.6.2|
|`Switch.System.ServiceModel.`<br/>`DisableUsingServicePointManagerSecurityProtocols`|Juntamente com o `Switch.System.Net.DontEnableSchUseStrongCrypto` , o determina se a segurança de mensagens do WCF usa tls 1,1 e tls 1,2.|.NET Framework 4.7 |
|`Switch.System.ServiceModel.`<br/>`DontEnableSystemDefaultTlsVersions`|Um valor `false` define a configuração padrão para permitir que o sistema operacional escolha o protocolo. Um valor de `true` define o padrão para o protocolo mais alto disponível. (Também disponível no Branch de manutenção das versões anteriores do Framework)|.NET Framework 4.7.1|
|`Switch.System.ServiceModel.`<br/>`UseSha1InMsmqEncryptionAlgorithm`|Determina se o algoritmo de assinatura de mensagem padrão para mensagens MSMQ no WCF é SHA1 ou SHA256.<br>Em razão de problemas de colisão com SHA1, a Microsoft recomenda SHA256.|.NET Framework 4.7.1|
|`Switch.System.ServiceModel.`<br/>`UseSha1InPipeConnectionGetHashAlgorithm`|Controla se o WCF usa um hash SHA1 ou SHA256 para gerar nomes aleatórios para pipes nomeados.<br>Em razão de problemas de colisão com SHA1, a Microsoft recomenda SHA256.|.NET Framework 4.7.1|
|`Switch.System.ServiceModel.Internals`<br/>`IncludeNullExceptionMessageInETWTrace`|Controla se uma [NullReferenceException](xref:System.NullReferenceException) é lançada quando a mensagem de exceção é nula.|.NET Framework 4.7|
|`Switch.System.ServiceProcess.`<br/>`DontThrowExceptionsOnStart`|Controla se as exceções geradas na inicialização do serviço são propagadas para o chamador do <xref:System.ServiceProcess.ServiceBase.Run%2A?displayProperty=nameWithType> método.|.NET Framework 4.7.1|
|`Switch.System.Threading.UseNetCoreTimer`|Controla se as instâncias aproveitam as <xref:System.Threading.Timer> melhorias de desempenho para ambientes de grande escala. Se `true` as melhorias de desempenho estiverem habilitadas; se `false` (o valor padrão), elas serão desabilitadas.|.NET Framework 4.8|
|`Switch.System.Uri.`<br/>`DontEnableStrictRFC3986ReservedCharacterSets`|Determina se determinados caracteres codificados por porcentagem, às vezes decodificados, agora são codificados sempre à esquerda. Se `true` eles forem decodificados; caso contrário, `false` .|.NET Framework 4.7.2|
|`Switch.System.Uri.`<br/>`DontKeepUnicodeBidiFormattingCharacters`|Determina a manipulação de caracteres bidirecionais Unicode em URIs. `true` para stripá-los de URIs; `false` para preservar e codificar por porcentagem.|.NET Framework 4.7.2|
|`Switch.System.Windows.Controls.Grid.`<br/>`StarDefinitionsCanExceedAvailableSpace` |Determina se Windows Presentation Foundation aplica um algoritmo antigo ( `true` ) ou um novo algoritmo ( `false` ) na alocação de espaço para \* colunas. Para obter mais informações, consulte [Mitigação: alocação de espaço do controle de grade para colunas de estrela](../../../migration-guide/retargeting/4.6.2-4.7.md#wpf-grid-allocation-of-space-to-star-columns). |.NET Framework 4.7 |
|`Switch.System.Windows.Controls.TabControl.`<br/>`SelectionPropertiesCanLagBehindSelectionChangedEvent`|Controla se um seletor ou um controle guia sempre atualiza o valor de sua propriedade de valor selecionado antes de gerar o evento de alteração de seleção.|.NET Framework 4.7.1|
|`Switch.System.Windows.Controls.Text.`<br/>`UseAdornerForTextboxSelectionRendering`|Determina se a renderização de seleção não baseada em adorno está disponível para <xref:System.Windows.Controls.TextBox> os <xref:System.Windows.Controls.PasswordBox> controles e para evitar texto obstruído ( `false` ) ou se o texto é renderizado apenas na camada de adorno ( `true` ).|.NET Framework 4.7.2|
|`Switch.System.Windows.Data.Binding.`<br/>`IListIndexerHidesCustomIndexer`|Controla se os indexadores de IList personalizados são usados incorretamente ( `true` ) ou corretamente ( `false` ) pela <xref:System.Windows.Data.Binding?displayProperty=nameWithType> classe.|.NET Framework 4.8|
|`Switch.System.Windows.DoNotScaleForDpiChanges`|Determina se as alterações de DPI ocorrem em um por sistema (um valor `false` ) ou base por monitor (um valor de `true` ).|.NET Framework 4.6.2|
|`Switch.System.Windows.`<br/>`DoNotUsePresentationDpiCapabilityTier2OrGreater`|Controla se os aprimoramentos no dimensionamento de controles em um <xref:System.Windows.Interop.HwndHost?displayProperty=nameWithType> quando o WPF é executado no modo de reconhecimento por monitor são desabilitados ( `true` ) ou habilitados ( `false` ).|.NET Framework 4.8|
|`Switch.System.Windows.Forms.`<br/>`DomainUpDown.UseLegacyScrolling`|Determina se o desenvolvedor precisa lidar especialmente com a <xref:System.Windows.Forms.DomainUpDown.UpButton?displayProperty=nameWithType> ação quando o texto de controle está presente. `true` para manipular a <xref:System.Windows.Forms.DomainUpDown.UpButton> ação; `false` para que <xref:System.Windows.Forms.DomainUpDown.UpButton?displayProperty=nameWithType> as <xref:System.Windows.Forms.DomainUpDown.DownButton?displayProperty=nameWithType> ações e sejam corretamente sincronizadas.|.NET Framework 4.7.2|
|`Switch.System.Windows.Forms.`<br />`DontSupportReentrantFilterMessage`|Opta pelo código que permite uma <xref:System.Windows.Forms.IMessageFilter.PreFilterMessage%2A?displayProperty=nameWithType> implementação personalizada para filtrar mensagens com segurança sem lançar uma exceção quando o <xref:System.Windows.Forms.Application.FilterMessage%2A?displayProperty=nameWithType> método é chamado. Para saber mais, confira [Mitigation: Custom IMessageFilter.PreFilterMessage Implementations](../../../migration-guide/mitigation-custom-imessagefilter-prefiltermessage-implementations.md) (Mitigação: implementações personalizadas de IMessageFilter.PreFilterMessage).|.NET Framework 4.6.1|
|`Switch.System.Windows.Forms.`<br/>`UseLegacyContextMenuStripSourceControlValue`|Determina se a <xref:System.Windows.Forms.ContextMenuStrip.SourceControl?displayProperty=nameWithType> propriedade retorna o controle do código-fonte quando o usuário abre o menu a partir de um controle aninhado <xref:System.Windows.Forms.ToolStripMenuItem> . `true` para retornar `null` , o comportamento herdado; `false` para retornar o controle do código-fonte.|.NET Framework 4.7.2|
|`Switch.System.Windows.Forms.UseLegacyToolTipDisplay`|Controla se o suporte à chamada de dica de ferramenta está desabilitado ( `true` ) ou habilitado ( `false` ). Habilitar o suporte à invocação de ToolTip também requer recursos de acessibilidade herdados definidos por `Switch.UseLegacyAccessibilityFeatures` , `Switch.UseLegacyAccessibilityFeatures.2` e `Switch.UseLegacyAccessibilityFeatures.3` todos eles serão desabilitados (definido como `false` ).|.NET Framework 4.8|
|`Switch.System.Windows.Input.Stylus.`<br/>`EnablePointerSupport`|Determina se uma `WM_POINTER` pilha de toque/caneta baseada em opcional está habilitada em aplicativos do WPF. Para obter mais informações, consulte [mitigação: toque baseado em ponteiro e suporte à caneta](../../../migration-guide/mitigation-pointer-based-touch-and-stylus-support.md)|.NET Framework 4.7|
|`Switch.System.Windows.Markup.`<br/>`DoNotUseSha256ForMarkupCompilerChecksumAlgorithm`|Determina se o algoritmo de hash padrão usado para somas de verificação é SHA256 ( `false` ) ou SHA1 ( `true` ).<br>Em razão de problemas de colisão com SHA1, a Microsoft recomenda SHA256.|.NET Framework 4.7.2|
|`Switch.System.Windows.Media.ImageSourceConverter.`<br/>`OverrideExceptionWithNullReferenceException`|Controla se uma [NullReferenceException](xref:System.NullReferenceException) herdada é lançada em vez da exceção que indica mais especificamente a causa da exceção (como um [DirectoryNotFoundException](xref:System.IO.DirectoryNotFoundException) ou um [FileNotFoundException](xref:System.IO.FileNotFoundException). Ele é destinado ao uso pelo código que depende da manipulação da [NullReferenceException](xref:System.NullReferenceException). | .NET Framework 4.7 |
|`Switch.System.Workflow.ComponentModel.`<br/>`UseLegacyHashForXomlFileChecksum`|Controla se o hash de soma de verificação de arquivos XOML em compilações do projeto de fluxo de trabalho usa o algoritmo MD5 ( `true` ) ou se eles usam o algoritmo SHA256 introduzido como padrão no .NET Framework 4,8.<br>Devido a problemas de colisão com o MD5, a Microsoft recomenda o SHA256.|.NET Framework 4.8|
|`Switch.System.Workflow.Runtime.`<br/>`UseLegacyHashForSqlTrackingCacheKey`|Controla se o hash de soma de verificação pelo SqlTrackingService usa o algoritmo MD5 ( `true` ) para cadeias de caracteres em cache ou se ele usa o algoritmo SHA256 introduzido como padrão no .NET Framework 4,8.<br>Devido a problemas de colisão com o MD5, a Microsoft recomenda o SHA256.|.NET Framework 4.8|
|`Switch.System.Workflow.Runtime.`<br/>`UseLegacyHashForWorkflowDefinitionDispenserCacheKey`|Controla se o hash de soma de verificação pelo tempo de execução do fluxo de trabalho usa o algoritmo MD5 ( `true` ) para definições de fluxo de trabalho em cache ou se ele usa o algoritmo SHA256 introduzido como padrão no .NET Framework 4,8.<br>Devido a problemas de colisão com o MD5, a Microsoft recomenda o SHA256.|.NET Framework 4.8|
|`Switch.UseLegacyAccessibilityFeatures`|Controla se os recursos de acessibilidade disponíveis a partir do .NET Framework 4.7.1 estão habilitados ou desabilitados. | .NET Framework 4.7.1 |
|`Switch.UseLegacyAccessibilityFeatures.2`|Controla se os recursos de acessibilidade disponíveis no .NET Framework 4.7.2 estão habilitados ( `false` ) ou desabilitados ( `true` ). Se `true` , `Switch.UseLegacyAccessibilityFeatures` também deve ser `true` para habilitar os recursos de acessibilidade do .NET Framework 4.7.1.|.NET Framework 4.7.2|
|`Switch.UseLegacyAccessibilityFeatures.3`|Controla se os recursos de acessibilidade introduzidos no .NET Framework 4,8 estão habilitados ( `false` ) ou desabilitados ( `true` ). Se `true` `Switch.UseLegacyAccessibilityFeatures` e `Switch.UseLegacyAccessibilityFeatures.2` também devem ser `true` .|.NET Framework 4.8|
|`Switch.UseLegacyToolTipDisplay`|Controla se tooltips são exibidas quando um usuário passa o cursor do mouse sobre um controle WPF ( `true` ), ou se eles são exibidos no foco do teclado e por meio da tecla de atalho do teclado ( `false` , o comportamento padrão). Para aplicativos em execução no .NET Framework 4,8, mas que visam versões anteriores do .NET Framework, habilitar o foco do teclado e o suporte à tecla de atalho requer que `Switch.UseLegacyAccessibilityFeatures` , `Switch.UseLegacyAccessibilityFeatures.2` e `Switch.UseLegacyAccessibilityFeatures.3` todos estejam definidos como `false` .|.NET Framework 4.8|
|`Switch.System.Xml.`<br />`IgnoreEmptyKeySequences`|Controla se as sequências de chaves vazias em chaves compostas são ignoradas pela validação de esquema XSD. Para obter mais informações, consulte [Mitigation: validação de esquema XML](../../../migration-guide/mitigation-xml-schema-validation.md).|.NET Framework 4.6|

> [!NOTE]
> Em vez de adicionar um `AppContextSwitchOverrides` elemento a um arquivo de configuração de aplicativo, você também pode definir as opções programaticamente chamando o `static` método (em C#) ou `Shared` (no Visual Basic) <xref:System.AppContext.SetSwitch%2A?displayProperty=nameWithType> .

 Os desenvolvedores de biblioteca também podem definir opções personalizadas para permitir que os chamadores recusem a funcionalidade alterada introduzida em versões posteriores de suas bibliotecas. Para obter mais informações, consulte a classe <xref:System.AppContext>.

## <a name="switches-in-aspnet-apps"></a>Opções em aplicativos ASP.NET

Você pode configurar um aplicativo ASP.NET para usar as configurações de compatibilidade adicionando um [\<Add>](../appsettings/add-element-for-appsettings.md) elemento à [\<appSettings>](../appsettings/index.md) seção do arquivo de web.config.

O exemplo a seguir usa o `<add>` elemento para adicionar duas configurações à `<appSettings>` seção de um arquivo de web.config:

```xml
<appSettings>
  <add key="AppContext.SetSwitch:Switch.System.Globalization.NoAsyncCurrentCulture" value="true" />
  <add key="AppContext.SetSwitch:Switch.System.Uri.DontEnableStrictRFC3986ReservedCharacterSets" value="true" />
</appSettings>
```

## <a name="example"></a>Exemplo

 O exemplo a seguir usa o `AppContextSwitchOverrides` elemento para definir uma única opção de compatibilidade de aplicativo, `Switch.System.Globalization.NoAsyncCurrentCulture` , que impede que a cultura flua entre threads em chamadas de método assíncronas.

```xml
<configuration>
   <runtime>
      <AppContextSwitchOverrides value="Switch.System.Globalization.NoAsyncCurrentCulture=true" />
   </runtime>
</configuration>
```

 O exemplo a seguir usa o `AppContextSwitchOverrides` elemento para definir duas opções de compatibilidade de aplicativos `Switch.System.Globalization.NoAsyncCurrentCulture` e `Switch.System.IO.BlockLongPaths` . Um ponto e vírgula separa os dois pares de nome/valor.

```xml
<configuration>
    <runtime>
       <AppContextSwitchOverrides
          value="Switch.System.Globalization.NoAsyncCurrentCulture=true;Switch.System.IO.BlockLongPaths=true" />
    </runtime>
</configuration>
```

## <a name="see-also"></a>Confira também

- [Atenuar novos comportamentos no .NET Framework 4,6 e posterior](../../../migration-guide/mitigations.md)
- <xref:System.AppContext?displayProperty=nameWithType>
- [\<runtime> Elementos](runtime-element.md)
- [\<configuration> Elementos](../configuration-element.md)
