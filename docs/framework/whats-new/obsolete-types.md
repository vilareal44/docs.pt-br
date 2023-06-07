---
title: Tipos obsoletos no .NET Framework
description: Consulte a lista de tipos que estão obsoletos no .NET Framework 4,5 e no .NET 4,6, organizados por assembly. As alternativas recomendadas também são listadas.
ms.date: 03/30/2017
helpviewer_keywords:
- .NET Framework 4.5, obsolete types
- types, obsolete in .NET Framework 4.5
- obsolete types [.NET Framework]
ms.assetid: e636d024-0fac-45eb-b721-25a8c0ceca8f
ms.openlocfilehash: 29df80fcefc2565850b026bebd30802dc77e1896
ms.sourcegitcommit: 40de8df14289e1e05b40d6e5c1daabd3c286d70c
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/22/2020
ms.locfileid: "86925807"
---
# <a name="obsolete-types-in-the-net-framework"></a>Tipos obsoletos no .NET Framework

<a name="introduction"></a> As tabelas deste artigo listam os membros que estão obsoletos no .NET Framework 4.5 e 4.6, organizados por assembly. Use os links a seguir para ver uma lista dos tipos obsoletos e das alternativas recomendadas em cada assembly. Como esses tipos são obsoletos, todos seus membros também estão obsoletos. Para obter uma lista de membros obsoletos adicionais na biblioteca de classes .NET Framework, confira [Membros obsoletos](obsolete-members.md).

- [Tipos obsoletos em assemblies do sistema](#obsolete_types_in_system_assemblies)

  - [mscorlib.dll](#mscorlib)

  - [System.Core.dll](#Core)

  - [System.Data.dll](#data)

  - [System.Data.OracleClient.dll](#oracleclient)

  - [System.Design.dll](#design)

  - [System.dll](#system)

  - [System.EnterpriseServices.dll](#enterpriseservices)

  - [System.Net.dll](#net)

  - [System.ServiceModel.dll](#servicemodel)

  - [System.Web.dll](#web)

  - [System.Web.Mobile.dll](#mobile)

  - [System.Workflow.Activities.dll](#workflow_activities)

  - [System.Workflow.ComponentModel.dll](#workflow_componentmodel)

  - [System.Workflow.Runtime.dll](#workflow_runtime)

  - [System.WorkflowServices.dll](#workflowservices)

  - [System.Xaml.dll](#xaml)

  - [System.Xml.dll](#xml)

  - [WindowsBase.dll](#WindowsBase)

- [Tipos obsoletos em assemblies da Microsoft](#obsolete_types_in_microsoft_assemblies)

  - [IEHost.dll e IEExec.exe](#IEHost)

  - [Microsoft.Build.Engine.dll](#Engine)

  - [Microsoft.JScript.dll](#jscript)

  - [Microsoft.VisualBasic.Compatibility.dll](#VBCompat)

  - [Microsoft.VisualBasic.Compatibility.Data.dll](#VBCompatData)

  - [Microsoft.VisualC.dll](#visualc)

<a name="obsolete_types_in_system_assemblies"></a>

## <a name="obsolete-types-in-system-assemblies"></a>Tipos obsoletos em assemblies de sistema

As tabelas a seguir listam os tipos que foram declarados obsoletos em assemblies de sistema. Esses assemblies são usados no desenvolvimento de aplicativos de uso\-geral direcionados ao .NET Framework.

<a name="mscorlib"></a>

### <a name="assembly-mscorlibdll"></a>Assembly: mscorlib.dll

|Tipo|Mensagem|
|----------|-------------|
|<xref:System.ExecutionEngineException?displayProperty=nameWithType>|Esse tipo indicava um erro fatal não especificado no runtime. Como o runtime não aciona mais essa exceção, esse tipo está obsoleto.|
|<xref:System.Collections.CaseInsensitiveHashCodeProvider?displayProperty=nameWithType>|Use <xref:System.StringComparer?displayProperty=nameWithType> em seu lugar.|
|<xref:System.Collections.IHashCodeProvider?displayProperty=nameWithType>|Use <xref:System.Collections.IEqualityComparer?displayProperty=nameWithType> em seu lugar.|
|<xref:System.Configuration.Assemblies.AssemblyHash?displayProperty=nameWithType>|A classe <xref:System.Configuration.Assemblies.AssemblyHash> foi substituída.|
|<xref:System.Diagnostics.Contracts.Internal.ContractHelper?displayProperty=nameWithType>|Substituído primeiramente no .NET Framework 4.5. Em vez disso, use a classe <xref:System.Runtime.CompilerServices.ContractHelper?displayProperty=nameWithType> no namespace System.Runtime.CompilerServices.|
|<xref:System.Reflection.Emit.UnmanagedMarshal?displayProperty=nameWithType>|Uma API alternativa está disponível: emita o atributo personalizado <xref:System.Runtime.InteropServices.MarshalAsAttribute?displayProperty=nameWithType> em seu lugar.|
|<xref:System.Runtime.InteropServices.BIND_OPTS?displayProperty=nameWithType>|Use <xref:System.Runtime.InteropServices.ComTypes.BIND_OPTS?displayProperty=nameWithType> em vez disso.|
|<xref:System.Runtime.InteropServices.BINDPTR?displayProperty=nameWithType>|Use <xref:System.Runtime.InteropServices.ComTypes.BINDPTR?displayProperty=nameWithType> em vez disso.|
|<xref:System.Runtime.InteropServices.CALLCONV?displayProperty=nameWithType>|Use <xref:System.Runtime.InteropServices.ComTypes.CALLCONV?displayProperty=nameWithType> em vez disso.|
|<xref:System.Runtime.InteropServices.CONNECTDATA?displayProperty=nameWithType>|Use <xref:System.Runtime.InteropServices.ComTypes.CONNECTDATA?displayProperty=nameWithType> em vez disso.|
|<xref:System.Runtime.InteropServices.DESCKIND?displayProperty=nameWithType>|Use <xref:System.Runtime.InteropServices.ComTypes.DESCKIND?displayProperty=nameWithType> em vez disso.|
|<xref:System.Runtime.InteropServices.DISPPARAMS?displayProperty=nameWithType>|Use <xref:System.Runtime.InteropServices.ComTypes.DISPPARAMS?displayProperty=nameWithType> em vez disso.|
|<xref:System.Runtime.InteropServices.ELEMDESC?displayProperty=nameWithType>|Use <xref:System.Runtime.InteropServices.ComTypes.ELEMDESC?displayProperty=nameWithType> em vez disso.|
|<xref:System.Runtime.InteropServices.EXCEPINFO?displayProperty=nameWithType>|Use <xref:System.Runtime.InteropServices.ComTypes.EXCEPINFO?displayProperty=nameWithType> em vez disso.|
|<xref:System.Runtime.InteropServices.FILETIME?displayProperty=nameWithType>|Use <xref:System.Runtime.InteropServices.ComTypes.FILETIME?displayProperty=nameWithType> em vez disso.|
|<xref:System.Runtime.InteropServices.FUNCDESC?displayProperty=nameWithType>|Use <xref:System.Runtime.InteropServices.ComTypes.FUNCDESC?displayProperty=nameWithType> em vez disso.|
|<xref:System.Runtime.InteropServices.FUNCFLAGS?displayProperty=nameWithType>|Use <xref:System.Runtime.InteropServices.ComTypes.FUNCFLAGS?displayProperty=nameWithType> em vez disso.|
|<xref:System.Runtime.InteropServices.FUNCKIND?displayProperty=nameWithType>|Use <xref:System.Runtime.InteropServices.ComTypes.FUNCKIND?displayProperty=nameWithType> em vez disso.|
|<xref:System.Runtime.InteropServices.IDispatchImplAttribute?displayProperty=nameWithType>|Esse atributo foi substituído e será removido em uma versão futura.|
|<xref:System.Runtime.InteropServices.IDispatchImplType?displayProperty=nameWithType>|O <xref:System.Runtime.InteropServices.IDispatchImplAttribute?displayProperty=nameWithType> foi substituído.|
|<xref:System.Runtime.InteropServices.IDLDESC?displayProperty=nameWithType>|Use <xref:System.Runtime.InteropServices.ComTypes.IDLDESC?displayProperty=nameWithType> em vez disso.|
|<xref:System.Runtime.InteropServices.IDLFLAG?displayProperty=nameWithType>|Use <xref:System.Runtime.InteropServices.ComTypes.IDLFLAG?displayProperty=nameWithType> em vez disso.|
|<xref:System.Runtime.InteropServices.IMPLTYPEFLAGS?displayProperty=nameWithType>|Use <xref:System.Runtime.InteropServices.ComTypes.IMPLTYPEFLAGS?displayProperty=nameWithType> em vez disso.|
|<xref:System.Runtime.InteropServices.INVOKEKIND?displayProperty=nameWithType>|Use <xref:System.Runtime.InteropServices.ComTypes.INVOKEKIND?displayProperty=nameWithType> em vez disso.|
|<xref:System.Runtime.InteropServices.LIBFLAGS?displayProperty=nameWithType>|Use <xref:System.Runtime.InteropServices.ComTypes.LIBFLAGS?displayProperty=nameWithType> em vez disso.|
|<xref:System.Runtime.InteropServices.PARAMDESC?displayProperty=nameWithType>|Use <xref:System.Runtime.InteropServices.ComTypes.PARAMDESC?displayProperty=nameWithType> em vez disso.|
|<xref:System.Runtime.InteropServices.PARAMFLAG?displayProperty=nameWithType>|Use <xref:System.Runtime.InteropServices.ComTypes.PARAMFLAG?displayProperty=nameWithType> em vez disso.|
|<xref:System.Runtime.InteropServices.SetWin32ContextInIDispatchAttribute?displayProperty=nameWithType>|Esse atributo foi substituído. Domínios de aplicativo não respeitam mais limites do contexto de ativação em chamadas IDispatch.|
|<xref:System.Runtime.InteropServices.STATSTG?displayProperty=nameWithType>|Use <xref:System.Runtime.InteropServices.ComTypes.STATSTG?displayProperty=nameWithType> em vez disso.|
|<xref:System.Runtime.InteropServices.SYSKIND?displayProperty=nameWithType>|Use <xref:System.Runtime.InteropServices.ComTypes.SYSKIND?displayProperty=nameWithType> em vez disso.|
|<xref:System.Runtime.InteropServices.TYPEATTR?displayProperty=nameWithType>|Use <xref:System.Runtime.InteropServices.ComTypes.TYPEATTR?displayProperty=nameWithType> em vez disso.|
|<xref:System.Runtime.InteropServices.TYPEDESC?displayProperty=nameWithType>|Use <xref:System.Runtime.InteropServices.ComTypes.TYPEDESC?displayProperty=nameWithType> em vez disso.|
|<xref:System.Runtime.InteropServices.TYPEFLAGS?displayProperty=nameWithType>|Use <xref:System.Runtime.InteropServices.ComTypes.TYPEFLAGS?displayProperty=nameWithType> em vez disso.|
|<xref:System.Runtime.InteropServices.TYPEKIND?displayProperty=nameWithType>|Use <xref:System.Runtime.InteropServices.ComTypes.TYPEKIND?displayProperty=nameWithType> em vez disso.|
|<xref:System.Runtime.InteropServices.TYPELIBATTR?displayProperty=nameWithType>|Use <xref:System.Runtime.InteropServices.ComTypes.TYPELIBATTR?displayProperty=nameWithType> em vez disso.|
|<xref:System.Runtime.InteropServices.UCOMIBindCtx?displayProperty=nameWithType>|Use <xref:System.Runtime.InteropServices.ComTypes.IBindCtx?displayProperty=nameWithType> em vez disso.|
|<xref:System.Runtime.InteropServices.UCOMIConnectionPoint?displayProperty=nameWithType>|Use <xref:System.Runtime.InteropServices.ComTypes.IConnectionPoint?displayProperty=nameWithType> em vez disso.|
|<xref:System.Runtime.InteropServices.UCOMIConnectionPointContainer?displayProperty=nameWithType>|Use <xref:System.Runtime.InteropServices.ComTypes.IConnectionPointContainer?displayProperty=nameWithType> em vez disso.|
|<xref:System.Runtime.InteropServices.UCOMIEnumConnectionPoints?displayProperty=nameWithType>|Use <xref:System.Runtime.InteropServices.ComTypes.IEnumConnectionPoints?displayProperty=nameWithType> em vez disso.|
|<xref:System.Runtime.InteropServices.UCOMIEnumConnections?displayProperty=nameWithType>|Use <xref:System.Runtime.InteropServices.ComTypes.IEnumConnections?displayProperty=nameWithType> em vez disso.|
|<xref:System.Runtime.InteropServices.UCOMIEnumMoniker?displayProperty=nameWithType>|Use <xref:System.Runtime.InteropServices.ComTypes.IEnumMoniker?displayProperty=nameWithType> em vez disso.|
|<xref:System.Runtime.InteropServices.UCOMIEnumString?displayProperty=nameWithType>|Use <xref:System.Runtime.InteropServices.ComTypes.IEnumString?displayProperty=nameWithType> em vez disso.|
|<xref:System.Runtime.InteropServices.UCOMIEnumVARIANT?displayProperty=nameWithType>|Use <xref:System.Runtime.InteropServices.ComTypes.IEnumVARIANT?displayProperty=nameWithType> em vez disso.|
|<xref:System.Runtime.InteropServices.UCOMIMoniker?displayProperty=nameWithType>|Use <xref:System.Runtime.InteropServices.ComTypes.IMoniker?displayProperty=nameWithType> em vez disso.|
|<xref:System.Runtime.InteropServices.UCOMIPersistFile?displayProperty=nameWithType>|Use <xref:System.Runtime.InteropServices.ComTypes.IPersistFile?displayProperty=nameWithType> em vez disso.|
|<xref:System.Runtime.InteropServices.UCOMIRunningObjectTable?displayProperty=nameWithType>|Use <xref:System.Runtime.InteropServices.ComTypes.IRunningObjectTable?displayProperty=nameWithType> em vez disso.|
|<xref:System.Runtime.InteropServices.UCOMIStream?displayProperty=nameWithType>|Use <xref:System.Runtime.InteropServices.ComTypes.IStream?displayProperty=nameWithType> em vez disso.|
|<xref:System.Runtime.InteropServices.UCOMITypeComp?displayProperty=nameWithType>|Use <xref:System.Runtime.InteropServices.ComTypes.ITypeComp?displayProperty=nameWithType> em vez disso.|
|<xref:System.Runtime.InteropServices.UCOMITypeInfo?displayProperty=nameWithType>|Use <xref:System.Runtime.InteropServices.ComTypes.ITypeInfo?displayProperty=nameWithType> em vez disso.|
|<xref:System.Runtime.InteropServices.UCOMITypeLib?displayProperty=nameWithType>|Use <xref:System.Runtime.InteropServices.ComTypes.ITypeLib?displayProperty=nameWithType> em vez disso.|
|<xref:System.Runtime.InteropServices.VARDESC?displayProperty=nameWithType>|Use <xref:System.Runtime.InteropServices.ComTypes.VARDESC?displayProperty=nameWithType> em vez disso.|
|<xref:System.Runtime.InteropServices.VARFLAGS?displayProperty=nameWithType>|Use <xref:System.Runtime.InteropServices.ComTypes.VARFLAGS?displayProperty=nameWithType> em vez disso.|
|<xref:System.Security.SecurityCriticalScope?displayProperty=nameWithType>|<xref:System.Security.SecurityCriticalScope> só é usado na compatibilidade de transparência do .NET 2.0.|
|<xref:System.Security.SecurityTreatAsSafeAttribute?displayProperty=nameWithType>|<xref:System.Security.SecurityTreatAsSafeAttribute> só é usado na compatibilidade de transparência do .NET 2.0. Use <xref:System.Security.SecuritySafeCriticalAttribute?displayProperty=nameWithType> em seu lugar.|
|<xref:System.Security.Policy.FirstMatchCodeGroup?displayProperty=nameWithType>|Esse tipo é obsoleto e será removido em uma versão futura do .NET Framework.|
|<xref:System.Security.Policy.PermissionRequestEvidence?displayProperty=nameWithType>|A segurança declarativa no nível do assembly é obsoleta e não é mais imposta pelo CLR por padrão.|
|<xref:System.Security.Policy.UnionCodeGroup?displayProperty=nameWithType>|Esse tipo é obsoleto e será removido em uma versão futura do .NET Framework.|

[Voltar ao início](#introduction)

<a name="Core"></a>

### <a name="assembly-systemcoredll"></a>Assembly: System.Core.dll

|Tipo|Mensagem|
|----------|-------------|
|<xref:System.Runtime.CompilerServices.ExecutionScope?displayProperty=nameWithType>|O uso desse tipo gera um erro do compilador.<br /><br /> Não use esse tipo.|

[Voltar ao início](#introduction)

<a name="data"></a>

### <a name="assembly-systemdatadll"></a>Assembly: System.Data.dll

|Tipo|Mensagem|
|----------|-------------|
|<xref:System.Data.DataSysDescriptionAttribute?displayProperty=nameWithType>|<xref:System.Data.DataSysDescriptionAttribute> foi preterido.|
|<xref:System.Data.PropertyAttributes?displayProperty=nameWithType>|<xref:System.Data.PropertyAttributes> foi preterido.|
|<xref:System.Data.TypedDataSetGenerator?displayProperty=nameWithType>|A classe <xref:System.Data.TypedDataSetGenerator> será removida em uma versão futura. Use <xref:System.Data.Design.TypedDataSetGenerator?displayProperty=nameWithType> em System.Design.dll.|
|<xref:System.Xml.XmlDataDocument?displayProperty=nameWithType>|A classe <xref:System.Xml.XmlDataDocument> será removida em uma versão futura.|

[Voltar ao início](#introduction)

<a name="oracleclient"></a>

### <a name="assembly-systemdataoracleclientdll"></a>Assembly: System.Data.OracleClient.dll

|Tipo|Mensagem|
|----------|-------------|
|<xref:System.Data.OracleClient.OracleClientFactory?displayProperty=nameWithType>|<xref:System.Data.OracleClient.OracleClientFactory> foi preterido.|
|<xref:System.Data.OracleClient.OracleCommand?displayProperty=nameWithType>|<xref:System.Data.OracleClient.OracleCommand> foi preterido.|
|<xref:System.Data.OracleClient.OracleCommandBuilder?displayProperty=nameWithType>|<xref:System.Data.OracleClient.OracleCommandBuilder> foi preterido.|
|<xref:System.Data.OracleClient.OracleConnection?displayProperty=nameWithType>|<xref:System.Data.OracleClient.OracleConnection> foi preterido.|
|<xref:System.Data.OracleClient.OracleConnectionStringBuilder?displayProperty=nameWithType>|<xref:System.Data.OracleClient.OracleConnectionStringBuilder> foi preterido.|
|<xref:System.Data.OracleClient.OracleDataAdapter?displayProperty=nameWithType>|<xref:System.Data.OracleClient.OracleDataAdapter> foi preterido.|
|<xref:System.Data.OracleClient.OraclePermission?displayProperty=nameWithType>|<xref:System.Data.OracleClient.OraclePermission> foi preterido.|
|<xref:System.Data.OracleClient.OraclePermissionAttribute?displayProperty=nameWithType>|<xref:System.Data.OracleClient.OraclePermissionAttribute?displayProperty=nameWithType> foi preterido.|

[Voltar ao início](#introduction)

<a name="design"></a>

### <a name="assembly-systemdesigndll"></a>Assembly: System.Design.dll

|Tipo|Mensagem|
|----------|-------------|
|<xref:System.ComponentModel.Design.LocalizationExtenderProvider?displayProperty=nameWithType>|Essa classe foi substituída. Use <xref:System.ComponentModel.Design.Serialization.CodeDomLocalizationProvider?displayProperty=nameWithType> em vez disso.|
|<xref:System.Web.UI.Design.DataBindingCollectionConverter?displayProperty=nameWithType>|O uso desse tipo não é recomendado porque a edição de DataBindings é iniciada por meio de <xref:System.ComponentModel.Design.DesignerActionList?displayProperty=nameWithType> em vez da grade de propriedade.|
|<xref:System.Web.UI.Design.DataBindingCollectionEditor?displayProperty=nameWithType>|O uso desse tipo não é recomendado porque a edição de DataBindings é iniciada por meio de <xref:System.ComponentModel.Design.DesignerActionList?displayProperty=nameWithType> em vez da grade de propriedade.|
|<xref:System.Web.UI.Design.IControlDesignerBehavior?displayProperty=nameWithType>|A alternativa recomendada é <xref:System.Web.UI.Design.IControlDesignerTag?displayProperty=nameWithType> e <xref:System.Web.UI.Design.IControlDesignerView?displayProperty=nameWithType>.|
|<xref:System.Web.UI.Design.IHtmlControlDesignerBehavior?displayProperty=nameWithType>|A alternativa recomendada é <xref:System.Web.UI.Design.IControlDesignerTag?displayProperty=nameWithType> e <xref:System.Web.UI.Design.IControlDesignerView?displayProperty=nameWithType>.|
|<xref:System.Web.UI.Design.ITemplateEditingFrame?displayProperty=nameWithType>|O uso de esse tipo não é recomendado porque a edição do modelo é identificada em <xref:System.Web.UI.Design.ControlDesigner?displayProperty=nameWithType>. Para dar suporte à edição do modelo, exponha os dados do modelo na propriedade <xref:System.Web.UI.Design.ControlDesigner.TemplateGroups%2A?displayProperty=nameWithType> e chame <xref:System.Web.UI.Design.ControlDesigner.SetViewFlags%2A?displayProperty=nameWithType>.|
|<xref:System.Web.UI.Design.IWebFormReferenceManager?displayProperty=nameWithType>|A alternativa recomendada é <xref:System.Web.UI.Design.WebFormsReferenceManager?displayProperty=nameWithType>. O <xref:System.Web.UI.Design.WebFormsReferenceManager> contém funcionalidade adicional e possibilita mais extensibilidade. Para obter o <xref:System.Web.UI.Design.WebFormsReferenceManager>, use a propriedade `RootDesigner.ReferenceManager` do seu <xref:System.Web.UI.Design.ControlDesigner?displayProperty=nameWithType>.|
|<xref:System.Web.UI.Design.IWebFormsDocumentService?displayProperty=nameWithType>|A alternativa recomendada é <xref:System.Web.UI.Design.WebFormsRootDesigner?displayProperty=nameWithType>. O <xref:System.Web.UI.Design.WebFormsRootDesigner> contém funcionalidade adicional e possibilita mais extensibilidade. Para obter o <xref:System.Web.UI.Design.WebFormsRootDesigner>, use a propriedade <xref:System.Web.UI.Design.ControlDesigner.RootDesigner%2A> do seu <xref:System.Web.UI.Design.ControlDesigner?displayProperty=nameWithType>.|
|<xref:System.Web.UI.Design.ITemplateEditingService?displayProperty=nameWithType>|O uso de esse tipo não é recomendado porque a edição do modelo é identificada em <xref:System.Web.UI.Design.ControlDesigner?displayProperty=nameWithType>. Para dar suporte à edição do modelo, exponha os dados do modelo na propriedade <xref:System.Web.UI.Design.ControlDesigner.TemplateGroups%2A?displayProperty=nameWithType> e chame <xref:System.Web.UI.Design.ControlDesigner.SetViewFlags%2A?displayProperty=nameWithType>.|
|<xref:System.Web.UI.Design.ReadWriteControlDesigner?displayProperty=nameWithType>|A alternativa recomendada é <xref:System.Web.UI.Design.ContainerControlDesigner?displayProperty=nameWithType> porque ela usa um <xref:System.Web.UI.Design.EditableDesignerRegion?displayProperty=nameWithType> para editar o conteúdo. Regiões de designer possibilitam um melhor controle do conteúdo que está sendo editado.|
|<xref:System.Web.UI.Design.TemplateEditingService?displayProperty=nameWithType>|O uso de esse tipo não é recomendado porque a edição do modelo é identificada em <xref:System.Web.UI.Design.ControlDesigner?displayProperty=nameWithType>. Para dar suporte à edição do modelo, exponha os dados do modelo na propriedade <xref:System.Web.UI.Design.ControlDesigner.TemplateGroups%2A?displayProperty=nameWithType> e chame <xref:System.Web.UI.Design.ControlDesigner.SetViewFlags%2A?displayProperty=nameWithType>.|
|<xref:System.Web.UI.Design.TemplateEditingVerb?displayProperty=nameWithType>|O uso de esse tipo não é recomendado porque a edição do modelo é identificada em <xref:System.Web.UI.Design.ControlDesigner?displayProperty=nameWithType>. Para dar suporte à edição do modelo, exponha os dados do modelo na propriedade <xref:System.Web.UI.Design.ControlDesigner.TemplateGroups%2A?displayProperty=nameWithType> e chame <xref:System.Web.UI.Design.ControlDesigner.SetViewFlags%2A?displayProperty=nameWithType>.|
|<xref:System.Web.UI.Design.WebControls.CalendarAutoFormatDialog?displayProperty=nameWithType>|O uso desse tipo não é recomendado porque a caixa de diálogo AutoFormat é iniciada pelo host do designer. A lista de AutoFormats disponíveis está exposta na <xref:System.Web.UI.Design.ControlDesigner?displayProperty=nameWithType> propriedade <xref:System.Web.UI.Design.ControlDesigner.AutoFormats%2A?displayProperty=nameWithType>.|
|<xref:System.Web.UI.Design.WebControls.PanelDesigner?displayProperty=nameWithType>|A alternativa recomendada é <xref:System.Web.UI.Design.WebControls.PanelContainerDesigner?displayProperty=nameWithType> porque ela usa um <xref:System.Web.UI.Design.EditableDesignerRegion?displayProperty=nameWithType> para editar o conteúdo. Regiões de designer possibilitam um melhor controle do conteúdo que está sendo editado.|

[Voltar ao início](#introduction)

<a name="system"></a>

### <a name="assembly-systemdll"></a>Assembly: System.dll

|Tipo|Mensagem|
|----------|-------------|
|<xref:System.ComponentModel.IComNativeDescriptorHandler?displayProperty=nameWithType>|Essa interface foi substituída. Adicione um <xref:System.ComponentModel.TypeDescriptionProvider?displayProperty=nameWithType> ao tipo de identificador <xref:System.ComponentModel.TypeDescriptor.ComObjectType%2A?displayProperty=nameWithType> em vez disso.|
|<xref:System.ComponentModel.RecommendedAsConfigurableAttribute?displayProperty=nameWithType>|Use <xref:System.ComponentModel.SettingsBindableAttribute?displayProperty=nameWithType> em seu lugar para trabalhar com o novo modelo de configurações.|
|<xref:System.ComponentModel.Design.Serialization.RootDesignerSerializerAttribute?displayProperty=nameWithType>|Esse atributo foi substituído. Use <xref:System.ComponentModel.Design.Serialization.DesignerSerializerAttribute?displayProperty=nameWithType> em vez disso.|
|<xref:System.Diagnostics.DiagnosticsConfigurationHandler?displayProperty=nameWithType>|Essa classe foi substituída.|
|<xref:System.Diagnostics.PerformanceCounterManager?displayProperty=nameWithType>|Essa classe foi substituída. Use os contadores de desempenho por meio da classe <xref:System.Diagnostics.PerformanceCounter?displayProperty=nameWithType> em seu lugar.|
|<xref:System.Net.GlobalProxySelection?displayProperty=nameWithType>|Essa classe foi substituída. Use <xref:System.Net.WebRequest.DefaultWebProxy%2A?displayProperty=nameWithType> em seu lugar para acessar e definir o proxy padrão global. Use 'null' em lugar de <xref:System.Net.GlobalProxySelection.GetEmptyWebProxy%2A?displayProperty=nameWithType>.|
|<xref:System.Net.Sockets.SocketClientAccessPolicyProtocol?displayProperty=nameWithType>|Substituído primeiramente no .NET Framework 4.5.<br /><br /> O uso desse tipo gera um erro do compilador.<br /><br /> Essa API dá suporte à infraestrutura do .NET Framework e não deve ser usada diretamente no código.|

[Voltar ao início](#introduction)

<a name="enterpriseservices"></a>

### <a name="assembly-systementerpriseservicesdll"></a>Assembly: System.EnterpriseServices.dll

|Tipo|Mensagem|
|----------|-------------|
|<xref:System.EnterpriseServices.RegistrationHelperTx?displayProperty=nameWithType>|A classe <xref:System.EnterpriseServices.RegistrationHelperTx> foi substituída.|

[Voltar ao início](#introduction)

<a name="net"></a>

### <a name="assembly-systemnetdll"></a>Assembly: System.Net.dll

|Tipo|Mensagem|
|----------|-------------|
|<xref:System.Net.INetworkProgress?displayProperty=nameWithType>|Substituído primeiramente no .NET Framework 4.5.<br /><br /> O uso desse tipo gera um erro do compilador.<br /><br /> Essa API dá suporte à infraestrutura do .NET Framework e não deve ser usada diretamente no código.|
|<xref:System.Net.IUnsafeWebRequestCreate?displayProperty=nameWithType>|Substituído primeiramente no .NET Framework 4.5.<br /><br /> O uso desse tipo gera um erro do compilador.<br /><br /> Essa API dá suporte à infraestrutura do .NET Framework e não deve ser usada diretamente no código.|
|<xref:System.Net.NetworkProgressChangedEventArgs?displayProperty=nameWithType>|Substituído primeiramente no .NET Framework 4.5.<br /><br /> O uso desse tipo gera um erro do compilador.<br /><br /> Essa API dá suporte à infraestrutura do .NET Framework e não deve ser usada diretamente no código.|
|<xref:System.Net.UiSynchronizationContext?displayProperty=nameWithType>|Substituído primeiramente no .NET Framework 4.5.<br /><br /> O uso desse tipo gera um erro do compilador.<br /><br /> Essa API dá suporte à infraestrutura do .NET Framework e não deve ser usada diretamente no código.|
|<xref:System.Net.Sockets.HttpPolicyDownloaderProtocol?displayProperty=nameWithType>|Substituído primeiramente no .NET Framework 4.5.<br /><br /> O uso desse tipo gera um erro do compilador.<br /><br /> Essa API dá suporte à infraestrutura do .NET Framework e não deve ser usada diretamente no código.|
|<xref:System.Net.Sockets.SecurityCriticalAction?displayProperty=nameWithType>|Substituído primeiramente no .NET Framework 4.5.<br /><br /> O uso desse tipo gera um erro do compilador.<br /><br /> Essa API dá suporte à infraestrutura do .NET Framework e não deve ser usada diretamente no código.|
|<xref:System.Net.Sockets.SocketPolicy?displayProperty=nameWithType>|Substituído primeiramente no .NET Framework 4.5.<br /><br /> O uso desse tipo gera um erro do compilador.<br /><br /> Essa API dá suporte à infraestrutura do .NET Framework e não deve ser usada diretamente no código.|
|<xref:System.Net.Sockets.UdpAnySourceMulticastClient?displayProperty=nameWithType>|Substituído primeiramente no .NET Framework 4.5.<br /><br /> O uso desse tipo gera um erro do compilador.<br /><br /> Essa API dá suporte à infraestrutura do .NET Framework e não deve ser usada diretamente no código.|
|<xref:System.Net.Sockets.UdpSingleSourceMulticastClient?displayProperty=nameWithType>|Substituído primeiramente no .NET Framework 4.5.<br /><br /> O uso desse tipo gera um erro do compilador.<br /><br /> Essa API dá suporte à infraestrutura do .NET Framework e não deve ser usada diretamente no código.|

[Voltar ao início](#introduction)

<a name="servicemodel"></a>

### <a name="assembly-systemservicemodeldll"></a>Assembly: System.ServiceModel.dll

|Tipo|Mensagem|
|----------|-------------|
|<xref:System.ServiceModel.NetPeerTcpBinding?displayProperty=nameWithType>|Substituído primeiramente no .NET Framework 4.5.<br /><br /> O recurso de canal par está obsoleto e será removido no futuro.|
|<xref:System.ServiceModel.Channels.HttpCookieContainerBindingElement?displayProperty=nameWithType>|Substituído primeiramente no .NET Framework 4.5.<br /><br /> Esse tipo está obsoleto. Para habilitar HTTP <xref:System.Net.CookieContainer>, use a propriedade `AllowCookies` na associação HTTP ou no <xref:System.ServiceModel.Channels.HttpTransportBindingElement>.|
|<xref:System.ServiceModel.Channels.PeerCustomResolverBindingElement?displayProperty=nameWithType>|Substituído primeiramente no .NET Framework 4.5.<br /><br /> O recurso de canal par está obsoleto e será removido no futuro.|
|<xref:System.ServiceModel.Channels.PeerTransportBindingElement?displayProperty=nameWithType>|Substituído primeiramente no .NET Framework 4.5.<br /><br /> O recurso de canal par está obsoleto e será removido no futuro.|
|<xref:System.ServiceModel.Configuration.NetPeerTcpBindingCollectionElement?displayProperty=nameWithType>|Substituído primeiramente no .NET Framework 4.5.<br /><br /> O recurso de canal par está obsoleto e será removido no futuro.|
|<xref:System.ServiceModel.Configuration.NetPeerTcpBindingElement?displayProperty=nameWithType>|Substituído primeiramente no .NET Framework 4.5.<br /><br /> O recurso de canal par está obsoleto e será removido no futuro.|
|<xref:System.ServiceModel.Configuration.PeerTransportElement?displayProperty=nameWithType>|Substituído primeiramente no .NET Framework 4.5.<br /><br /> O recurso de canal par está obsoleto e será removido no futuro.|
|<xref:System.ServiceModel.PeerResolvers.CustomPeerResolverService?displayProperty=nameWithType>|Substituído primeiramente no .NET Framework 4.5.<br /><br /> O recurso de canal par está obsoleto e será removido no futuro.|

[Voltar ao início](#introduction)

<a name="web"></a>

### <a name="assembly-systemwebdll"></a>Assembly: System.Web.dll

|Tipo|Mensagem|
|----------|-------------|
|<xref:System.Web.Configuration.PassportAuthentication?displayProperty=nameWithType>|Esse tipo está obsoleto. O produto de autenticação do Passport não tem mais suporte e foi substituído pela [conta da Microsoft](https://account.microsoft.com/account/Account?destrt=home-index)|
|<xref:System.Web.Mail.MailAttachment?displayProperty=nameWithType>|A alternativa recomendada é <xref:System.Net.Mail.Attachment?displayProperty=nameWithType>.|
|<xref:System.Web.Mail.MailEncoding?displayProperty=nameWithType>|A alternativa recomendada é <xref:System.Net.Mime.TransferEncoding?displayProperty=nameWithType>.|
|<xref:System.Web.Mail.MailFormat?displayProperty=nameWithType>|A alternativa recomendada é <xref:System.Net.Mail.MailMessage.IsBodyHtml%2A?displayProperty=nameWithType>.|
|<xref:System.Web.Mail.MailMessage?displayProperty=nameWithType>|A alternativa recomendada é <xref:System.Net.Mail.MailMessage?displayProperty=nameWithType>.|
|<xref:System.Web.Mail.MailPriority?displayProperty=nameWithType>|A alternativa recomendada é <xref:System.Net.Mail.MailPriority?displayProperty=nameWithType>.|
|<xref:System.Web.Mail.SmtpMail?displayProperty=nameWithType>|A alternativa recomendada é <xref:System.Net.Mail.SmtpClient?displayProperty=nameWithType>.|
|<xref:System.Web.Security.PassportAuthenticationEventArgs?displayProperty=nameWithType>|Esse tipo está obsoleto. O produto de autenticação do Passport não tem mais suporte e foi substituído pela [conta da Microsoft](https://account.microsoft.com/account/Account?destrt=home-index)|
|<xref:System.Web.Security.PassportAuthenticationEventHandler?displayProperty=nameWithType>|Esse tipo está obsoleto. O produto de autenticação do Passport não tem mais suporte e foi substituído pela [conta da Microsoft](https://account.microsoft.com/account/Account?destrt=home-index)|
|<xref:System.Web.Security.PassportAuthenticationModule?displayProperty=nameWithType>|Esse tipo está obsoleto. O produto de autenticação do Passport não tem mais suporte e foi substituído pela [conta da Microsoft](https://account.microsoft.com/account/Account?destrt=home-index)|
|<xref:System.Web.Security.PassportIdentity?displayProperty=nameWithType>|Esse tipo está obsoleto. O produto de autenticação do Passport não tem mais suporte e foi substituído pela [conta da Microsoft](https://account.microsoft.com/account/Account?destrt=home-index)|
|<xref:System.Web.Security.PassportPrincipal?displayProperty=nameWithType>|Esse tipo está obsoleto. O produto de autenticação do Passport não tem mais suporte e foi substituído pela [conta da Microsoft](https://account.microsoft.com/account/Account?destrt=home-index)|
|<xref:System.Web.UI.ObjectConverter?displayProperty=nameWithType>|A alternativa recomendada é <xref:System.Convert?displayProperty=nameWithType> e <xref:System.String.Format%2A?displayProperty=nameWithType>.|

[Voltar ao início](#introduction)

<a name="mobile"></a>

### <a name="assembly-systemwebmobiledll"></a>Assembly: System.Web.Mobile.dll

|Tipo|Mensagem|
|----------|-------------|
|<xref:System.Web.Mobile.CookielessData?displayProperty=nameWithType>|O assembly System.Web.Mobile.dll foi substituído e não deve ser mais usado. Para saber mais sobre como desenvolver aplicativos móveis ASP.NET, confira [ASP.NET for Mobiles](/aspnet/mobile/overview) (ASP.NET para mobilidade).|
|<xref:System.Web.Mobile.DeviceFilterElement?displayProperty=nameWithType>|O assembly System.Web.Mobile.dll foi substituído e não deve ser mais usado. Para saber mais sobre como desenvolver aplicativos móveis ASP.NET, confira [ASP.NET for Mobiles](/aspnet/mobile/overview) (ASP.NET para mobilidade).|
|<xref:System.Web.Mobile.DeviceFilterElementCollection?displayProperty=nameWithType>|O assembly System.Web.Mobile.dll foi substituído e não deve ser mais usado. Para saber mais sobre como desenvolver aplicativos móveis ASP.NET, confira [ASP.NET for Mobiles](/aspnet/mobile/overview) (ASP.NET para mobilidade).|
|<xref:System.Web.Mobile.DeviceFiltersSection?displayProperty=nameWithType>|O assembly System.Web.Mobile.dll foi substituído e não deve ser mais usado. Para saber mais sobre como desenvolver aplicativos móveis ASP.NET, confira [ASP.NET for Mobiles](/aspnet/mobile/overview) (ASP.NET para mobilidade).|
|<xref:System.Web.Mobile.ErrorHandlerModule?displayProperty=nameWithType>|O assembly System.Web.Mobile.dll foi substituído e não deve ser mais usado. Para saber mais sobre como desenvolver aplicativos móveis ASP.NET, confira [ASP.NET for Mobiles](/aspnet/mobile/overview) (ASP.NET para mobilidade).|
|<xref:System.Web.Mobile.MobileCapabilities?displayProperty=nameWithType>|O assembly System.Web.Mobile.dll foi substituído e não deve ser mais usado. Para saber mais sobre como desenvolver aplicativos móveis ASP.NET, confira [ASP.NET for Mobiles](/aspnet/mobile/overview) (ASP.NET para mobilidade).|
|<xref:System.Web.Mobile.MobileDeviceCapabilitiesSectionHandler?displayProperty=nameWithType>|O assembly System.Web.Mobile.dll foi substituído e não deve ser mais usado. Para saber mais sobre como desenvolver aplicativos móveis ASP.NET, confira [ASP.NET for Mobiles](/aspnet/mobile/overview) (ASP.NET para mobilidade).|
|<xref:System.Web.Mobile.MobileErrorInfo?displayProperty=nameWithType>|O assembly System.Web.Mobile.dll foi substituído e não deve ser mais usado. Para saber mais sobre como desenvolver aplicativos móveis ASP.NET, confira [ASP.NET for Mobiles](/aspnet/mobile/overview) (ASP.NET para mobilidade).|
|<xref:System.Web.Mobile.MobileFormsAuthentication?displayProperty=nameWithType>|O assembly System.Web.Mobile.dll foi substituído e não deve ser mais usado. Para saber mais sobre como desenvolver aplicativos móveis ASP.NET, confira [ASP.NET for Mobiles](/aspnet/mobile/overview) (ASP.NET para mobilidade).|
|<xref:System.Web.UI.Design.MobileControls.IMobileDesigner?displayProperty=nameWithType>|O assembly System.Web.Mobile.dll foi substituído e não deve ser mais usado. Para saber mais sobre como desenvolver aplicativos móveis ASP.NET, confira [ASP.NET for Mobiles](/aspnet/mobile/overview) (ASP.NET para mobilidade).|
|<xref:System.Web.UI.Design.MobileControls.IMobileWebFormServices?displayProperty=nameWithType>|O assembly System.Web.Mobile.dll foi substituído e não deve ser mais usado. Para saber mais sobre como desenvolver aplicativos móveis ASP.NET, confira [ASP.NET for Mobiles](/aspnet/mobile/overview) (ASP.NET para mobilidade).|
|<xref:System.Web.UI.Design.MobileControls.MobileResource?displayProperty=nameWithType>|O assembly System.Web.Mobile.dll foi substituído e não deve ser mais usado. Para saber mais sobre como desenvolver aplicativos móveis ASP.NET, confira [ASP.NET for Mobiles](/aspnet/mobile/overview) (ASP.NET para mobilidade).|
|<xref:System.Web.UI.Design.MobileControls.Converters.DataFieldConverter?displayProperty=nameWithType>|O assembly System.Web.Mobile.dll foi substituído e não deve ser mais usado. Para saber mais sobre como desenvolver aplicativos móveis ASP.NET, confira [ASP.NET for Mobiles](/aspnet/mobile/overview) (ASP.NET para mobilidade).|
|<xref:System.Web.UI.Design.MobileControls.Converters.DataMemberConverter?displayProperty=nameWithType>|O assembly System.Web.Mobile.dll foi substituído e não deve ser mais usado. Para saber mais sobre como desenvolver aplicativos móveis ASP.NET, confira [ASP.NET for Mobiles](/aspnet/mobile/overview) (ASP.NET para mobilidade).|
|<xref:System.Web.UI.MobileControls.AdRotator?displayProperty=nameWithType>|O assembly System.Web.Mobile.dll foi substituído e não deve ser mais usado. Para saber mais sobre como desenvolver aplicativos móveis ASP.NET, confira [ASP.NET for Mobiles](/aspnet/mobile/overview) (ASP.NET para mobilidade).|
|<xref:System.Web.UI.MobileControls.Alignment?displayProperty=nameWithType>|O assembly System.Web.Mobile.dll foi substituído e não deve ser mais usado. Para saber mais sobre como desenvolver aplicativos móveis ASP.NET, confira [ASP.NET for Mobiles](/aspnet/mobile/overview) (ASP.NET para mobilidade).|
|<xref:System.Web.UI.MobileControls.ArrayListCollectionBase?displayProperty=nameWithType>|O assembly System.Web.Mobile.dll foi substituído e não deve ser mais usado. Para saber mais sobre como desenvolver aplicativos móveis ASP.NET, confira [ASP.NET for Mobiles](/aspnet/mobile/overview) (ASP.NET para mobilidade).|
|<xref:System.Web.UI.MobileControls.BaseValidator?displayProperty=nameWithType>|O assembly System.Web.Mobile.dll foi substituído e não deve ser mais usado. Para saber mais sobre como desenvolver aplicativos móveis ASP.NET, confira [ASP.NET for Mobiles](/aspnet/mobile/overview) (ASP.NET para mobilidade).|
|<xref:System.Web.UI.MobileControls.BooleanOption?displayProperty=nameWithType>|O assembly System.Web.Mobile.dll foi substituído e não deve ser mais usado. Para saber mais sobre como desenvolver aplicativos móveis ASP.NET, confira [ASP.NET for Mobiles](/aspnet/mobile/overview) (ASP.NET para mobilidade).|
|<xref:System.Web.UI.MobileControls.Calendar?displayProperty=nameWithType>|O assembly System.Web.Mobile.dll foi substituído e não deve ser mais usado. Para saber mais sobre como desenvolver aplicativos móveis ASP.NET, confira [ASP.NET for Mobiles](/aspnet/mobile/overview) (ASP.NET para mobilidade).|
|<xref:System.Web.UI.MobileControls.Command?displayProperty=nameWithType>|O assembly System.Web.Mobile.dll foi substituído e não deve ser mais usado. Para saber mais sobre como desenvolver aplicativos móveis ASP.NET, confira [ASP.NET for Mobiles](/aspnet/mobile/overview) (ASP.NET para mobilidade).|
|<xref:System.Web.UI.MobileControls.CommandFormat?displayProperty=nameWithType>|O assembly System.Web.Mobile.dll foi substituído e não deve ser mais usado. Para saber mais sobre como desenvolver aplicativos móveis ASP.NET, confira [ASP.NET for Mobiles](/aspnet/mobile/overview) (ASP.NET para mobilidade).|
|<xref:System.Web.UI.MobileControls.CompareValidator?displayProperty=nameWithType>|O assembly System.Web.Mobile.dll foi substituído e não deve ser mais usado. Para saber mais sobre como desenvolver aplicativos móveis ASP.NET, confira [ASP.NET for Mobiles](/aspnet/mobile/overview) (ASP.NET para mobilidade).|
|<xref:System.Web.UI.MobileControls.Constants?displayProperty=nameWithType>|O assembly System.Web.Mobile.dll foi substituído e não deve ser mais usado. Para saber mais sobre como desenvolver aplicativos móveis ASP.NET, confira [ASP.NET for Mobiles](/aspnet/mobile/overview) (ASP.NET para mobilidade).|
|<xref:System.Web.UI.MobileControls.ControlElement?displayProperty=nameWithType>|O assembly System.Web.Mobile.dll foi substituído e não deve ser mais usado. Para saber mais sobre como desenvolver aplicativos móveis ASP.NET, confira [ASP.NET for Mobiles](/aspnet/mobile/overview) (ASP.NET para mobilidade).|
|<xref:System.Web.UI.MobileControls.ControlElementCollection?displayProperty=nameWithType>|O assembly System.Web.Mobile.dll foi substituído e não deve ser mais usado. Para saber mais sobre como desenvolver aplicativos móveis ASP.NET, confira [ASP.NET for Mobiles](/aspnet/mobile/overview) (ASP.NET para mobilidade).|
|<xref:System.Web.UI.MobileControls.ControlPager?displayProperty=nameWithType>|O assembly System.Web.Mobile.dll foi substituído e não deve ser mais usado. Para saber mais sobre como desenvolver aplicativos móveis ASP.NET, confira [ASP.NET for Mobiles](/aspnet/mobile/overview) (ASP.NET para mobilidade).|
|<xref:System.Web.UI.MobileControls.CustomValidator?displayProperty=nameWithType>|O assembly System.Web.Mobile.dll foi substituído e não deve ser mais usado. Para saber mais sobre como desenvolver aplicativos móveis ASP.NET, confira [ASP.NET for Mobiles](/aspnet/mobile/overview) (ASP.NET para mobilidade).|
|<xref:System.Web.UI.MobileControls.DesignerAdapterAttribute?displayProperty=nameWithType>|O assembly System.Web.Mobile.dll foi substituído e não deve ser mais usado. Para saber mais sobre como desenvolver aplicativos móveis ASP.NET, confira [ASP.NET for Mobiles](/aspnet/mobile/overview) (ASP.NET para mobilidade).|
|<xref:System.Web.UI.MobileControls.DeviceElement?displayProperty=nameWithType>|O assembly System.Web.Mobile.dll foi substituído e não deve ser mais usado. Para saber mais sobre como desenvolver aplicativos móveis ASP.NET, confira [ASP.NET for Mobiles](/aspnet/mobile/overview) (ASP.NET para mobilidade).|
|<xref:System.Web.UI.MobileControls.DeviceElementCollection?displayProperty=nameWithType>|O assembly System.Web.Mobile.dll foi substituído e não deve ser mais usado. Para saber mais sobre como desenvolver aplicativos móveis ASP.NET, confira [ASP.NET for Mobiles](/aspnet/mobile/overview) (ASP.NET para mobilidade).|
|<xref:System.Web.UI.MobileControls.DeviceOverridableAttribute?displayProperty=nameWithType>|O assembly System.Web.Mobile.dll foi substituído e não deve ser mais usado. Para saber mais sobre como desenvolver aplicativos móveis ASP.NET, confira [ASP.NET for Mobiles](/aspnet/mobile/overview) (ASP.NET para mobilidade).|
|<xref:System.Web.UI.MobileControls.DeviceSpecific?displayProperty=nameWithType>|O assembly System.Web.Mobile.dll foi substituído e não deve ser mais usado. Para saber mais sobre como desenvolver aplicativos móveis ASP.NET, confira [ASP.NET for Mobiles](/aspnet/mobile/overview) (ASP.NET para mobilidade).|
|<xref:System.Web.UI.MobileControls.DeviceSpecificChoice?displayProperty=nameWithType>|O assembly System.Web.Mobile.dll foi substituído e não deve ser mais usado. Para saber mais sobre como desenvolver aplicativos móveis ASP.NET, confira [ASP.NET for Mobiles](/aspnet/mobile/overview) (ASP.NET para mobilidade).|
|<xref:System.Web.UI.MobileControls.DeviceSpecificChoiceCollection?displayProperty=nameWithType>|O assembly System.Web.Mobile.dll foi substituído e não deve ser mais usado. Para saber mais sobre como desenvolver aplicativos móveis ASP.NET, confira [ASP.NET for Mobiles](/aspnet/mobile/overview) (ASP.NET para mobilidade).|
|<xref:System.Web.UI.MobileControls.DeviceSpecificChoiceControlBuilder?displayProperty=nameWithType>|O assembly System.Web.Mobile.dll foi substituído e não deve ser mais usado. Para saber mais sobre como desenvolver aplicativos móveis ASP.NET, confira [ASP.NET for Mobiles](/aspnet/mobile/overview) (ASP.NET para mobilidade).|
|<xref:System.Web.UI.MobileControls.DeviceSpecificChoiceTemplateBuilder?displayProperty=nameWithType>|O assembly System.Web.Mobile.dll foi substituído e não deve ser mais usado. Para saber mais sobre como desenvolver aplicativos móveis ASP.NET, confira [ASP.NET for Mobiles](/aspnet/mobile/overview) (ASP.NET para mobilidade).|
|<xref:System.Web.UI.MobileControls.DeviceSpecificChoiceTemplateContainer?displayProperty=nameWithType>|O assembly System.Web.Mobile.dll foi substituído e não deve ser mais usado. Para saber mais sobre como desenvolver aplicativos móveis ASP.NET, confira [ASP.NET for Mobiles](/aspnet/mobile/overview) (ASP.NET para mobilidade).|
|<xref:System.Web.UI.MobileControls.DeviceSpecificControlBuilder?displayProperty=nameWithType>|O assembly System.Web.Mobile.dll foi substituído e não deve ser mais usado. Para saber mais sobre como desenvolver aplicativos móveis ASP.NET, confira [ASP.NET for Mobiles](/aspnet/mobile/overview) (ASP.NET para mobilidade).|
|<xref:System.Web.UI.MobileControls.ErrorFormatterPage?displayProperty=nameWithType>|O assembly System.Web.Mobile.dll foi substituído e não deve ser mais usado. Para saber mais sobre como desenvolver aplicativos móveis ASP.NET, confira [ASP.NET for Mobiles](/aspnet/mobile/overview) (ASP.NET para mobilidade).|
|<xref:System.Web.UI.MobileControls.FontInfo?displayProperty=nameWithType>|O assembly System.Web.Mobile.dll foi substituído e não deve ser mais usado. Para saber mais sobre como desenvolver aplicativos móveis ASP.NET, confira [ASP.NET for Mobiles](/aspnet/mobile/overview) (ASP.NET para mobilidade).|
|<xref:System.Web.UI.MobileControls.FontSize?displayProperty=nameWithType>|O assembly System.Web.Mobile.dll foi substituído e não deve ser mais usado. Para saber mais sobre como desenvolver aplicativos móveis ASP.NET, confira [ASP.NET for Mobiles](/aspnet/mobile/overview) (ASP.NET para mobilidade).|
|<xref:System.Web.UI.MobileControls.Form?displayProperty=nameWithType>|O assembly System.Web.Mobile.dll foi substituído e não deve ser mais usado. Para saber mais sobre como desenvolver aplicativos móveis ASP.NET, confira [ASP.NET for Mobiles](/aspnet/mobile/overview) (ASP.NET para mobilidade).|
|<xref:System.Web.UI.MobileControls.FormControlBuilder?displayProperty=nameWithType>|O assembly System.Web.Mobile.dll foi substituído e não deve ser mais usado. Para saber mais sobre como desenvolver aplicativos móveis ASP.NET, confira [ASP.NET for Mobiles](/aspnet/mobile/overview) (ASP.NET para mobilidade).|
|<xref:System.Web.UI.MobileControls.FormMethod?displayProperty=nameWithType>|O assembly System.Web.Mobile.dll foi substituído e não deve ser mais usado. Para saber mais sobre como desenvolver aplicativos móveis ASP.NET, confira [ASP.NET for Mobiles](/aspnet/mobile/overview) (ASP.NET para mobilidade).|
|<xref:System.Web.UI.MobileControls.IControlAdapter?displayProperty=nameWithType>|O assembly System.Web.Mobile.dll foi substituído e não deve ser mais usado. Para saber mais sobre como desenvolver aplicativos móveis ASP.NET, confira [ASP.NET for Mobiles](/aspnet/mobile/overview) (ASP.NET para mobilidade).|
|<xref:System.Web.UI.MobileControls.Image?displayProperty=nameWithType>|O assembly System.Web.Mobile.dll foi substituído e não deve ser mais usado. Para saber mais sobre como desenvolver aplicativos móveis ASP.NET, confira [ASP.NET for Mobiles](/aspnet/mobile/overview) (ASP.NET para mobilidade).|
|<xref:System.Web.UI.MobileControls.IObjectListFieldCollection?displayProperty=nameWithType>|O assembly System.Web.Mobile.dll foi substituído e não deve ser mais usado. Para saber mais sobre como desenvolver aplicativos móveis ASP.NET, confira [ASP.NET for Mobiles](/aspnet/mobile/overview) (ASP.NET para mobilidade).|
|<xref:System.Web.UI.MobileControls.IPageAdapter?displayProperty=nameWithType>|O assembly System.Web.Mobile.dll foi substituído e não deve ser mais usado. Para saber mais sobre como desenvolver aplicativos móveis ASP.NET, confira [ASP.NET for Mobiles](/aspnet/mobile/overview) (ASP.NET para mobilidade).|
|<xref:System.Web.UI.MobileControls.ItemPager?displayProperty=nameWithType>|O assembly System.Web.Mobile.dll foi substituído e não deve ser mais usado. Para saber mais sobre como desenvolver aplicativos móveis ASP.NET, confira [ASP.NET for Mobiles](/aspnet/mobile/overview) (ASP.NET para mobilidade).|
|<xref:System.Web.UI.MobileControls.ITemplateable?displayProperty=nameWithType>|O assembly System.Web.Mobile.dll foi substituído e não deve ser mais usado. Para saber mais sobre como desenvolver aplicativos móveis ASP.NET, confira [ASP.NET for Mobiles](/aspnet/mobile/overview) (ASP.NET para mobilidade).|
|<xref:System.Web.UI.MobileControls.Label?displayProperty=nameWithType>|O assembly System.Web.Mobile.dll foi substituído e não deve ser mais usado. Para saber mais sobre como desenvolver aplicativos móveis ASP.NET, confira [ASP.NET for Mobiles](/aspnet/mobile/overview) (ASP.NET para mobilidade).|
|<xref:System.Web.UI.MobileControls.Link?displayProperty=nameWithType>|O assembly System.Web.Mobile.dll foi substituído e não deve ser mais usado. Para saber mais sobre como desenvolver aplicativos móveis ASP.NET, confira [ASP.NET for Mobiles](/aspnet/mobile/overview) (ASP.NET para mobilidade).|
|<xref:System.Web.UI.MobileControls.List?displayProperty=nameWithType>|O assembly System.Web.Mobile.dll foi substituído e não deve ser mais usado. Para saber mais sobre como desenvolver aplicativos móveis ASP.NET, confira [ASP.NET for Mobiles](/aspnet/mobile/overview) (ASP.NET para mobilidade).|
|<xref:System.Web.UI.MobileControls.ListCommandEventArgs?displayProperty=nameWithType>|O assembly System.Web.Mobile.dll foi substituído e não deve ser mais usado. Para saber mais sobre como desenvolver aplicativos móveis ASP.NET, confira [ASP.NET for Mobiles](/aspnet/mobile/overview) (ASP.NET para mobilidade).|
|<xref:System.Web.UI.MobileControls.ListCommandEventHandler?displayProperty=nameWithType>|O assembly System.Web.Mobile.dll foi substituído e não deve ser mais usado. Para saber mais sobre como desenvolver aplicativos móveis ASP.NET, confira [ASP.NET for Mobiles](/aspnet/mobile/overview) (ASP.NET para mobilidade).|
|<xref:System.Web.UI.MobileControls.ListControlBuilder?displayProperty=nameWithType>|O assembly System.Web.Mobile.dll foi substituído e não deve ser mais usado. Para saber mais sobre como desenvolver aplicativos móveis ASP.NET, confira [ASP.NET for Mobiles](/aspnet/mobile/overview) (ASP.NET para mobilidade).|
|<xref:System.Web.UI.MobileControls.ListDataBindEventArgs?displayProperty=nameWithType>|O assembly System.Web.Mobile.dll foi substituído e não deve ser mais usado. Para saber mais sobre como desenvolver aplicativos móveis ASP.NET, confira [ASP.NET for Mobiles](/aspnet/mobile/overview) (ASP.NET para mobilidade).|
|<xref:System.Web.UI.MobileControls.ListDataBindEventHandler?displayProperty=nameWithType>|O assembly System.Web.Mobile.dll foi substituído e não deve ser mais usado. Para saber mais sobre como desenvolver aplicativos móveis ASP.NET, confira [ASP.NET for Mobiles](/aspnet/mobile/overview) (ASP.NET para mobilidade).|
|<xref:System.Web.UI.MobileControls.ListDecoration?displayProperty=nameWithType>|O assembly System.Web.Mobile.dll foi substituído e não deve ser mais usado. Para saber mais sobre como desenvolver aplicativos móveis ASP.NET, confira [ASP.NET for Mobiles](/aspnet/mobile/overview) (ASP.NET para mobilidade).|
|<xref:System.Web.UI.MobileControls.ListSelectType?displayProperty=nameWithType>|O assembly System.Web.Mobile.dll foi substituído e não deve ser mais usado. Para saber mais sobre como desenvolver aplicativos móveis ASP.NET, confira [ASP.NET for Mobiles](/aspnet/mobile/overview) (ASP.NET para mobilidade).|
|<xref:System.Web.UI.MobileControls.LiteralLink?displayProperty=nameWithType>|O assembly System.Web.Mobile.dll foi substituído e não deve ser mais usado. Para saber mais sobre como desenvolver aplicativos móveis ASP.NET, confira [ASP.NET for Mobiles](/aspnet/mobile/overview) (ASP.NET para mobilidade).|
|<xref:System.Web.UI.MobileControls.LiteralText?displayProperty=nameWithType>|O assembly System.Web.Mobile.dll foi substituído e não deve ser mais usado. Para saber mais sobre como desenvolver aplicativos móveis ASP.NET, confira [ASP.NET for Mobiles](/aspnet/mobile/overview) (ASP.NET para mobilidade).|
|<xref:System.Web.UI.MobileControls.LiteralTextContainerControlBuilder?displayProperty=nameWithType>|O assembly System.Web.Mobile.dll foi substituído e não deve ser mais usado. Para saber mais sobre como desenvolver aplicativos móveis ASP.NET, confira [ASP.NET for Mobiles](/aspnet/mobile/overview) (ASP.NET para mobilidade).|
|<xref:System.Web.UI.MobileControls.LiteralTextControlBuilder?displayProperty=nameWithType>|O assembly System.Web.Mobile.dll foi substituído e não deve ser mais usado. Para saber mais sobre como desenvolver aplicativos móveis ASP.NET, confira [ASP.NET for Mobiles](/aspnet/mobile/overview) (ASP.NET para mobilidade).|
|<xref:System.Web.UI.MobileControls.LoadItemsEventArgs?displayProperty=nameWithType>|O assembly System.Web.Mobile.dll foi substituído e não deve ser mais usado. Para saber mais sobre como desenvolver aplicativos móveis ASP.NET, confira [ASP.NET for Mobiles](/aspnet/mobile/overview) (ASP.NET para mobilidade).|
|<xref:System.Web.UI.MobileControls.LoadItemsEventHandler?displayProperty=nameWithType>|O assembly System.Web.Mobile.dll foi substituído e não deve ser mais usado. Para saber mais sobre como desenvolver aplicativos móveis ASP.NET, confira [ASP.NET for Mobiles](/aspnet/mobile/overview) (ASP.NET para mobilidade).|
|<xref:System.Web.UI.MobileControls.MobileControl?displayProperty=nameWithType>|O assembly System.Web.Mobile.dll foi substituído e não deve ser mais usado. Para saber mais sobre como desenvolver aplicativos móveis ASP.NET, confira [ASP.NET for Mobiles](/aspnet/mobile/overview) (ASP.NET para mobilidade).|
|<xref:System.Web.UI.MobileControls.MobileControlBuilder?displayProperty=nameWithType>|O assembly System.Web.Mobile.dll foi substituído e não deve ser mais usado. Para saber mais sobre como desenvolver aplicativos móveis ASP.NET, confira [ASP.NET for Mobiles](/aspnet/mobile/overview) (ASP.NET para mobilidade).|
|<xref:System.Web.UI.MobileControls.MobileControlsSection?displayProperty=nameWithType>|O assembly System.Web.Mobile.dll foi substituído e não deve ser mais usado. Para saber mais sobre como desenvolver aplicativos móveis ASP.NET, confira [ASP.NET for Mobiles](/aspnet/mobile/overview) (ASP.NET para mobilidade).|
|<xref:System.Web.UI.MobileControls.MobileControlsSectionHandler?displayProperty=nameWithType>|O assembly System.Web.Mobile.dll foi substituído e não deve ser mais usado. Para saber mais sobre como desenvolver aplicativos móveis ASP.NET, confira [ASP.NET for Mobiles](/aspnet/mobile/overview) (ASP.NET para mobilidade).|
|<xref:System.Web.UI.MobileControls.MobileListItem?displayProperty=nameWithType>|O assembly System.Web.Mobile.dll foi substituído e não deve ser mais usado. Para saber mais sobre como desenvolver aplicativos móveis ASP.NET, confira [ASP.NET for Mobiles](/aspnet/mobile/overview) (ASP.NET para mobilidade).|
|<xref:System.Web.UI.MobileControls.MobileListItemCollection?displayProperty=nameWithType>|O assembly System.Web.Mobile.dll foi substituído e não deve ser mais usado. Para saber mais sobre como desenvolver aplicativos móveis ASP.NET, confira [ASP.NET for Mobiles](/aspnet/mobile/overview) (ASP.NET para mobilidade).|
|<xref:System.Web.UI.MobileControls.MobileListItemType?displayProperty=nameWithType>|O assembly System.Web.Mobile.dll foi substituído e não deve ser mais usado. Para saber mais sobre como desenvolver aplicativos móveis ASP.NET, confira [ASP.NET for Mobiles](/aspnet/mobile/overview) (ASP.NET para mobilidade).|
|<xref:System.Web.UI.MobileControls.MobilePage?displayProperty=nameWithType>|O assembly System.Web.Mobile.dll foi substituído e não deve ser mais usado. Para saber mais sobre como desenvolver aplicativos móveis ASP.NET, confira [ASP.NET for Mobiles](/aspnet/mobile/overview) (ASP.NET para mobilidade).|
|<xref:System.Web.UI.MobileControls.MobileTypeNameConverter?displayProperty=nameWithType>|O assembly System.Web.Mobile.dll foi substituído e não deve ser mais usado. Para saber mais sobre como desenvolver aplicativos móveis ASP.NET, confira [ASP.NET for Mobiles](/aspnet/mobile/overview) (ASP.NET para mobilidade).|
|<xref:System.Web.UI.MobileControls.MobileUserControl?displayProperty=nameWithType>|O assembly System.Web.Mobile.dll foi substituído e não deve ser mais usado. Para saber mais sobre como desenvolver aplicativos móveis ASP.NET, confira [ASP.NET for Mobiles](/aspnet/mobile/overview) (ASP.NET para mobilidade).|
|<xref:System.Web.UI.MobileControls.ObjectList?displayProperty=nameWithType>|O assembly System.Web.Mobile.dll foi substituído e não deve ser mais usado. Para saber mais sobre como desenvolver aplicativos móveis ASP.NET, confira [ASP.NET for Mobiles](/aspnet/mobile/overview) (ASP.NET para mobilidade).|
|<xref:System.Web.UI.MobileControls.ObjectListCommand?displayProperty=nameWithType>|O assembly System.Web.Mobile.dll foi substituído e não deve ser mais usado. Para saber mais sobre como desenvolver aplicativos móveis ASP.NET, confira [ASP.NET for Mobiles](/aspnet/mobile/overview) (ASP.NET para mobilidade).|
|<xref:System.Web.UI.MobileControls.ObjectListCommandCollection?displayProperty=nameWithType>|O assembly System.Web.Mobile.dll foi substituído e não deve ser mais usado. Para saber mais sobre como desenvolver aplicativos móveis ASP.NET, confira [ASP.NET for Mobiles](/aspnet/mobile/overview) (ASP.NET para mobilidade).|
|<xref:System.Web.UI.MobileControls.ObjectListCommandEventArgs?displayProperty=nameWithType>|O assembly System.Web.Mobile.dll foi substituído e não deve ser mais usado. Para saber mais sobre como desenvolver aplicativos móveis ASP.NET, confira [ASP.NET for Mobiles](/aspnet/mobile/overview) (ASP.NET para mobilidade).|
|<xref:System.Web.UI.MobileControls.ObjectListCommandEventHandler?displayProperty=nameWithType>|O assembly System.Web.Mobile.dll foi substituído e não deve ser mais usado. Para saber mais sobre como desenvolver aplicativos móveis ASP.NET, confira [ASP.NET for Mobiles](/aspnet/mobile/overview) (ASP.NET para mobilidade).|
|<xref:System.Web.UI.MobileControls.ObjectListControlBuilder?displayProperty=nameWithType>|O assembly System.Web.Mobile.dll foi substituído e não deve ser mais usado. Para saber mais sobre como desenvolver aplicativos móveis ASP.NET, confira [ASP.NET for Mobiles](/aspnet/mobile/overview) (ASP.NET para mobilidade).|
|<xref:System.Web.UI.MobileControls.ObjectListDataBindEventArgs?displayProperty=nameWithType>|O assembly System.Web.Mobile.dll foi substituído e não deve ser mais usado. Para saber mais sobre como desenvolver aplicativos móveis ASP.NET, confira [ASP.NET for Mobiles](/aspnet/mobile/overview) (ASP.NET para mobilidade).|
|<xref:System.Web.UI.MobileControls.ObjectListDataBindEventHandler?displayProperty=nameWithType>|O assembly System.Web.Mobile.dll foi substituído e não deve ser mais usado. Para saber mais sobre como desenvolver aplicativos móveis ASP.NET, confira [ASP.NET for Mobiles](/aspnet/mobile/overview) (ASP.NET para mobilidade).|
|<xref:System.Web.UI.MobileControls.ObjectListField?displayProperty=nameWithType>|O assembly System.Web.Mobile.dll foi substituído e não deve ser mais usado. Para saber mais sobre como desenvolver aplicativos móveis ASP.NET, confira [ASP.NET for Mobiles](/aspnet/mobile/overview) (ASP.NET para mobilidade).|
|<xref:System.Web.UI.MobileControls.ObjectListFieldCollection?displayProperty=nameWithType>|O assembly System.Web.Mobile.dll foi substituído e não deve ser mais usado. Para saber mais sobre como desenvolver aplicativos móveis ASP.NET, confira [ASP.NET for Mobiles](/aspnet/mobile/overview) (ASP.NET para mobilidade).|
|<xref:System.Web.UI.MobileControls.ObjectListItem?displayProperty=nameWithType>|O assembly System.Web.Mobile.dll foi substituído e não deve ser mais usado. Para saber mais sobre como desenvolver aplicativos móveis ASP.NET, confira [ASP.NET for Mobiles](/aspnet/mobile/overview) (ASP.NET para mobilidade).|
|<xref:System.Web.UI.MobileControls.ObjectListItemCollection?displayProperty=nameWithType>|O assembly System.Web.Mobile.dll foi substituído e não deve ser mais usado. Para saber mais sobre como desenvolver aplicativos móveis ASP.NET, confira [ASP.NET for Mobiles](/aspnet/mobile/overview) (ASP.NET para mobilidade).|
|<xref:System.Web.UI.MobileControls.ObjectListSelectEventArgs?displayProperty=nameWithType>|O assembly System.Web.Mobile.dll foi substituído e não deve ser mais usado. Para saber mais sobre como desenvolver aplicativos móveis ASP.NET, confira [ASP.NET for Mobiles](/aspnet/mobile/overview) (ASP.NET para mobilidade).|
|<xref:System.Web.UI.MobileControls.ObjectListSelectEventHandler?displayProperty=nameWithType>|O assembly System.Web.Mobile.dll foi substituído e não deve ser mais usado. Para saber mais sobre como desenvolver aplicativos móveis ASP.NET, confira [ASP.NET for Mobiles](/aspnet/mobile/overview) (ASP.NET para mobilidade).|
|<xref:System.Web.UI.MobileControls.ObjectListShowCommandsEventArgs?displayProperty=nameWithType>|O assembly System.Web.Mobile.dll foi substituído e não deve ser mais usado. Para saber mais sobre como desenvolver aplicativos móveis ASP.NET, confira [ASP.NET for Mobiles](/aspnet/mobile/overview) (ASP.NET para mobilidade).|
|<xref:System.Web.UI.MobileControls.ObjectListShowCommandsEventHandler?displayProperty=nameWithType>|O assembly System.Web.Mobile.dll foi substituído e não deve ser mais usado. Para saber mais sobre como desenvolver aplicativos móveis ASP.NET, confira [ASP.NET for Mobiles](/aspnet/mobile/overview) (ASP.NET para mobilidade).|
|<xref:System.Web.UI.MobileControls.ObjectListTitleAttribute?displayProperty=nameWithType>|O assembly System.Web.Mobile.dll foi substituído e não deve ser mais usado. Para saber mais sobre como desenvolver aplicativos móveis ASP.NET, confira [ASP.NET for Mobiles](/aspnet/mobile/overview) (ASP.NET para mobilidade).|
|<xref:System.Web.UI.MobileControls.ObjectListViewMode?displayProperty=nameWithType>|O assembly System.Web.Mobile.dll foi substituído e não deve ser mais usado. Para saber mais sobre como desenvolver aplicativos móveis ASP.NET, confira [ASP.NET for Mobiles](/aspnet/mobile/overview) (ASP.NET para mobilidade).|
|<xref:System.Web.UI.MobileControls.PagedControl?displayProperty=nameWithType>|O assembly System.Web.Mobile.dll foi substituído e não deve ser mais usado. Para saber mais sobre como desenvolver aplicativos móveis ASP.NET, confira [ASP.NET for Mobiles](/aspnet/mobile/overview) (ASP.NET para mobilidade).|
|<xref:System.Web.UI.MobileControls.PagerStyle?displayProperty=nameWithType>|O assembly System.Web.Mobile.dll foi substituído e não deve ser mais usado. Para saber mais sobre como desenvolver aplicativos móveis ASP.NET, confira [ASP.NET for Mobiles](/aspnet/mobile/overview) (ASP.NET para mobilidade).|
|<xref:System.Web.UI.MobileControls.Panel?displayProperty=nameWithType>|O assembly System.Web.Mobile.dll foi substituído e não deve ser mais usado. Para saber mais sobre como desenvolver aplicativos móveis ASP.NET, confira [ASP.NET for Mobiles](/aspnet/mobile/overview) (ASP.NET para mobilidade).|
|<xref:System.Web.UI.MobileControls.PanelControlBuilder?displayProperty=nameWithType>|O assembly System.Web.Mobile.dll foi substituído e não deve ser mais usado. Para saber mais sobre como desenvolver aplicativos móveis ASP.NET, confira [ASP.NET for Mobiles](/aspnet/mobile/overview) (ASP.NET para mobilidade).|
|<xref:System.Web.UI.MobileControls.PersistNameAttribute?displayProperty=nameWithType>|O assembly System.Web.Mobile.dll foi substituído e não deve ser mais usado. Para saber mais sobre como desenvolver aplicativos móveis ASP.NET, confira [ASP.NET for Mobiles](/aspnet/mobile/overview) (ASP.NET para mobilidade).|
|<xref:System.Web.UI.MobileControls.PhoneCall?displayProperty=nameWithType>|O assembly System.Web.Mobile.dll foi substituído e não deve ser mais usado. Para saber mais sobre como desenvolver aplicativos móveis ASP.NET, confira [ASP.NET for Mobiles](/aspnet/mobile/overview) (ASP.NET para mobilidade).|
|<xref:System.Web.UI.MobileControls.RangeValidator?displayProperty=nameWithType>|O assembly System.Web.Mobile.dll foi substituído e não deve ser mais usado. Para saber mais sobre como desenvolver aplicativos móveis ASP.NET, confira [ASP.NET for Mobiles](/aspnet/mobile/overview) (ASP.NET para mobilidade).|
|<xref:System.Web.UI.MobileControls.RegularExpressionValidator?displayProperty=nameWithType>|O assembly System.Web.Mobile.dll foi substituído e não deve ser mais usado. Para saber mais sobre como desenvolver aplicativos móveis ASP.NET, confira [ASP.NET for Mobiles](/aspnet/mobile/overview) (ASP.NET para mobilidade).|
|<xref:System.Web.UI.MobileControls.RequiredFieldValidator?displayProperty=nameWithType>|O assembly System.Web.Mobile.dll foi substituído e não deve ser mais usado. Para saber mais sobre como desenvolver aplicativos móveis ASP.NET, confira [ASP.NET for Mobiles](/aspnet/mobile/overview) (ASP.NET para mobilidade).|
|<xref:System.Web.UI.MobileControls.SelectionList?displayProperty=nameWithType>|O assembly System.Web.Mobile.dll foi substituído e não deve ser mais usado. Para saber mais sobre como desenvolver aplicativos móveis ASP.NET, confira [ASP.NET for Mobiles](/aspnet/mobile/overview) (ASP.NET para mobilidade).|
|<xref:System.Web.UI.MobileControls.Style?displayProperty=nameWithType>|O assembly System.Web.Mobile.dll foi substituído e não deve ser mais usado. Para saber mais sobre como desenvolver aplicativos móveis ASP.NET, confira [ASP.NET for Mobiles](/aspnet/mobile/overview) (ASP.NET para mobilidade).|
|<xref:System.Web.UI.MobileControls.StyleSheet?displayProperty=nameWithType>|O assembly System.Web.Mobile.dll foi substituído e não deve ser mais usado. Para saber mais sobre como desenvolver aplicativos móveis ASP.NET, confira [ASP.NET for Mobiles](/aspnet/mobile/overview) (ASP.NET para mobilidade).|
|<xref:System.Web.UI.MobileControls.StyleSheetControlBuilder?displayProperty=nameWithType>|O assembly System.Web.Mobile.dll foi substituído e não deve ser mais usado. Para saber mais sobre como desenvolver aplicativos móveis ASP.NET, confira [ASP.NET for Mobiles](/aspnet/mobile/overview) (ASP.NET para mobilidade).|
|<xref:System.Web.UI.MobileControls.TemplateContainer?displayProperty=nameWithType>|O assembly System.Web.Mobile.dll foi substituído e não deve ser mais usado. Para saber mais sobre como desenvolver aplicativos móveis ASP.NET, confira [ASP.NET for Mobiles](/aspnet/mobile/overview) (ASP.NET para mobilidade).|
|<xref:System.Web.UI.MobileControls.TextBox?displayProperty=nameWithType>|O assembly System.Web.Mobile.dll foi substituído e não deve ser mais usado. Para saber mais sobre como desenvolver aplicativos móveis ASP.NET, confira [ASP.NET for Mobiles](/aspnet/mobile/overview) (ASP.NET para mobilidade).|
|<xref:System.Web.UI.MobileControls.TextBoxControlBuilder?displayProperty=nameWithType>|O assembly System.Web.Mobile.dll foi substituído e não deve ser mais usado. Para saber mais sobre como desenvolver aplicativos móveis ASP.NET, confira [ASP.NET for Mobiles](/aspnet/mobile/overview) (ASP.NET para mobilidade).|
|<xref:System.Web.UI.MobileControls.TextControl?displayProperty=nameWithType>|O assembly System.Web.Mobile.dll foi substituído e não deve ser mais usado. Para saber mais sobre como desenvolver aplicativos móveis ASP.NET, confira [ASP.NET for Mobiles](/aspnet/mobile/overview) (ASP.NET para mobilidade).|
|<xref:System.Web.UI.MobileControls.TextView?displayProperty=nameWithType>|O assembly System.Web.Mobile.dll foi substituído e não deve ser mais usado. Para saber mais sobre como desenvolver aplicativos móveis ASP.NET, confira [ASP.NET for Mobiles](/aspnet/mobile/overview) (ASP.NET para mobilidade).|
|<xref:System.Web.UI.MobileControls.TextViewElement?displayProperty=nameWithType>|O assembly System.Web.Mobile.dll foi substituído e não deve ser mais usado. Para saber mais sobre como desenvolver aplicativos móveis ASP.NET, confira [ASP.NET for Mobiles](/aspnet/mobile/overview) (ASP.NET para mobilidade).|
|<xref:System.Web.UI.MobileControls.ValidationSummary?displayProperty=nameWithType>|O assembly System.Web.Mobile.dll foi substituído e não deve ser mais usado. Para saber mais sobre como desenvolver aplicativos móveis ASP.NET, confira [ASP.NET for Mobiles](/aspnet/mobile/overview) (ASP.NET para mobilidade).|
|<xref:System.Web.UI.MobileControls.Wrapping?displayProperty=nameWithType>|O assembly System.Web.Mobile.dll foi substituído e não deve ser mais usado. Para saber mais sobre como desenvolver aplicativos móveis ASP.NET, confira [ASP.NET for Mobiles](/aspnet/mobile/overview) (ASP.NET para mobilidade).|
|<xref:System.Web.UI.MobileControls.Adapters.ChtmlCalendarAdapter?displayProperty=nameWithType>|O assembly System.Web.Mobile.dll foi substituído e não deve ser mais usado. Para saber mais sobre como desenvolver aplicativos móveis ASP.NET, confira [ASP.NET for Mobiles](/aspnet/mobile/overview) (ASP.NET para mobilidade).|
|<xref:System.Web.UI.MobileControls.Adapters.ChtmlCommandAdapter?displayProperty=nameWithType>|O assembly System.Web.Mobile.dll foi substituído e não deve ser mais usado. Para saber mais sobre como desenvolver aplicativos móveis ASP.NET, confira [ASP.NET for Mobiles](/aspnet/mobile/overview) (ASP.NET para mobilidade).|
|<xref:System.Web.UI.MobileControls.Adapters.ChtmlFormAdapter?displayProperty=nameWithType>|O assembly System.Web.Mobile.dll foi substituído e não deve ser mais usado. Para saber mais sobre como desenvolver aplicativos móveis ASP.NET, confira [ASP.NET for Mobiles](/aspnet/mobile/overview) (ASP.NET para mobilidade).|
|<xref:System.Web.UI.MobileControls.Adapters.ChtmlImageAdapter?displayProperty=nameWithType>|O assembly System.Web.Mobile.dll foi substituído e não deve ser mais usado. Para saber mais sobre como desenvolver aplicativos móveis ASP.NET, confira [ASP.NET for Mobiles](/aspnet/mobile/overview) (ASP.NET para mobilidade).|
|<xref:System.Web.UI.MobileControls.Adapters.ChtmlLinkAdapter?displayProperty=nameWithType>|O assembly System.Web.Mobile.dll foi substituído e não deve ser mais usado. Para saber mais sobre como desenvolver aplicativos móveis ASP.NET, confira [ASP.NET for Mobiles](/aspnet/mobile/overview) (ASP.NET para mobilidade).|
|<xref:System.Web.UI.MobileControls.Adapters.ChtmlMobileTextWriter?displayProperty=nameWithType>|O assembly System.Web.Mobile.dll foi substituído e não deve ser mais usado. Para saber mais sobre como desenvolver aplicativos móveis ASP.NET, confira [ASP.NET for Mobiles](/aspnet/mobile/overview) (ASP.NET para mobilidade).|
|<xref:System.Web.UI.MobileControls.Adapters.ChtmlPageAdapter?displayProperty=nameWithType>|O assembly System.Web.Mobile.dll foi substituído e não deve ser mais usado. Para saber mais sobre como desenvolver aplicativos móveis ASP.NET, confira [ASP.NET for Mobiles](/aspnet/mobile/overview) (ASP.NET para mobilidade).|
|<xref:System.Web.UI.MobileControls.Adapters.ChtmlPhoneCallAdapter?displayProperty=nameWithType>|O assembly System.Web.Mobile.dll foi substituído e não deve ser mais usado. Para saber mais sobre como desenvolver aplicativos móveis ASP.NET, confira [ASP.NET for Mobiles](/aspnet/mobile/overview) (ASP.NET para mobilidade).|
|<xref:System.Web.UI.MobileControls.Adapters.ChtmlSelectionListAdapter?displayProperty=nameWithType>|O assembly System.Web.Mobile.dll foi substituído e não deve ser mais usado. Para saber mais sobre como desenvolver aplicativos móveis ASP.NET, confira [ASP.NET for Mobiles](/aspnet/mobile/overview) (ASP.NET para mobilidade).|
|<xref:System.Web.UI.MobileControls.Adapters.ChtmlTextBoxAdapter?displayProperty=nameWithType>|O assembly System.Web.Mobile.dll foi substituído e não deve ser mais usado. Para saber mais sobre como desenvolver aplicativos móveis ASP.NET, confira [ASP.NET for Mobiles](/aspnet/mobile/overview) (ASP.NET para mobilidade).|
|<xref:System.Web.UI.MobileControls.Adapters.ControlAdapter?displayProperty=nameWithType>|O assembly System.Web.Mobile.dll foi substituído e não deve ser mais usado. Para saber mais sobre como desenvolver aplicativos móveis ASP.NET, confira [ASP.NET for Mobiles](/aspnet/mobile/overview) (ASP.NET para mobilidade).|
|<xref:System.Web.UI.MobileControls.Adapters.HtmlCalendarAdapter?displayProperty=nameWithType>|O assembly System.Web.Mobile.dll foi substituído e não deve ser mais usado. Para saber mais sobre como desenvolver aplicativos móveis ASP.NET, confira [ASP.NET for Mobiles](/aspnet/mobile/overview) (ASP.NET para mobilidade).|
|<xref:System.Web.UI.MobileControls.Adapters.HtmlCommandAdapter?displayProperty=nameWithType>|O assembly System.Web.Mobile.dll foi substituído e não deve ser mais usado. Para saber mais sobre como desenvolver aplicativos móveis ASP.NET, confira [ASP.NET for Mobiles](/aspnet/mobile/overview) (ASP.NET para mobilidade).|
|<xref:System.Web.UI.MobileControls.Adapters.HtmlControlAdapter?displayProperty=nameWithType>|O assembly System.Web.Mobile.dll foi substituído e não deve ser mais usado. Para saber mais sobre como desenvolver aplicativos móveis ASP.NET, confira [ASP.NET for Mobiles](/aspnet/mobile/overview) (ASP.NET para mobilidade).|
|<xref:System.Web.UI.MobileControls.Adapters.HtmlFormAdapter?displayProperty=nameWithType>|O assembly System.Web.Mobile.dll foi substituído e não deve ser mais usado. Para saber mais sobre como desenvolver aplicativos móveis ASP.NET, confira [ASP.NET for Mobiles](/aspnet/mobile/overview) (ASP.NET para mobilidade).|
|<xref:System.Web.UI.MobileControls.Adapters.HtmlImageAdapter?displayProperty=nameWithType>|O assembly System.Web.Mobile.dll foi substituído e não deve ser mais usado. Para saber mais sobre como desenvolver aplicativos móveis ASP.NET, confira [ASP.NET for Mobiles](/aspnet/mobile/overview) (ASP.NET para mobilidade).|
|<xref:System.Web.UI.MobileControls.Adapters.HtmlLabelAdapter?displayProperty=nameWithType>|O assembly System.Web.Mobile.dll foi substituído e não deve ser mais usado. Para saber mais sobre como desenvolver aplicativos móveis ASP.NET, confira [ASP.NET for Mobiles](/aspnet/mobile/overview) (ASP.NET para mobilidade).|
|<xref:System.Web.UI.MobileControls.Adapters.HtmlLinkAdapter?displayProperty=nameWithType>|O assembly System.Web.Mobile.dll foi substituído e não deve ser mais usado. Para saber mais sobre como desenvolver aplicativos móveis ASP.NET, confira [ASP.NET for Mobiles](/aspnet/mobile/overview) (ASP.NET para mobilidade).|
|<xref:System.Web.UI.MobileControls.Adapters.HtmlListAdapter?displayProperty=nameWithType>|O assembly System.Web.Mobile.dll foi substituído e não deve ser mais usado. Para saber mais sobre como desenvolver aplicativos móveis ASP.NET, confira [ASP.NET for Mobiles](/aspnet/mobile/overview) (ASP.NET para mobilidade).|
|<xref:System.Web.UI.MobileControls.Adapters.HtmlLiteralTextAdapter?displayProperty=nameWithType>|O assembly System.Web.Mobile.dll foi substituído e não deve ser mais usado. Para saber mais sobre como desenvolver aplicativos móveis ASP.NET, confira [ASP.NET for Mobiles](/aspnet/mobile/overview) (ASP.NET para mobilidade).|
|<xref:System.Web.UI.MobileControls.Adapters.HtmlMobileTextWriter?displayProperty=nameWithType>|O assembly System.Web.Mobile.dll foi substituído e não deve ser mais usado. Para saber mais sobre como desenvolver aplicativos móveis ASP.NET, confira [ASP.NET for Mobiles](/aspnet/mobile/overview) (ASP.NET para mobilidade).|
|<xref:System.Web.UI.MobileControls.Adapters.HtmlObjectListAdapter?displayProperty=nameWithType>|O assembly System.Web.Mobile.dll foi substituído e não deve ser mais usado. Para saber mais sobre como desenvolver aplicativos móveis ASP.NET, confira [ASP.NET for Mobiles](/aspnet/mobile/overview) (ASP.NET para mobilidade).|
|<xref:System.Web.UI.MobileControls.Adapters.HtmlPageAdapter?displayProperty=nameWithType>|O assembly System.Web.Mobile.dll foi substituído e não deve ser mais usado. Para saber mais sobre como desenvolver aplicativos móveis ASP.NET, confira [ASP.NET for Mobiles](/aspnet/mobile/overview) (ASP.NET para mobilidade).|
|<xref:System.Web.UI.MobileControls.Adapters.HtmlPanelAdapter?displayProperty=nameWithType>|O assembly System.Web.Mobile.dll foi substituído e não deve ser mais usado. Para saber mais sobre como desenvolver aplicativos móveis ASP.NET, confira [ASP.NET for Mobiles](/aspnet/mobile/overview) (ASP.NET para mobilidade).|
|<xref:System.Web.UI.MobileControls.Adapters.HtmlPhoneCallAdapter?displayProperty=nameWithType>|O assembly System.Web.Mobile.dll foi substituído e não deve ser mais usado. Para saber mais sobre como desenvolver aplicativos móveis ASP.NET, confira [ASP.NET for Mobiles](/aspnet/mobile/overview) (ASP.NET para mobilidade).|
|<xref:System.Web.UI.MobileControls.Adapters.HtmlSelectionListAdapter?displayProperty=nameWithType>|O assembly System.Web.Mobile.dll foi substituído e não deve ser mais usado. Para saber mais sobre como desenvolver aplicativos móveis ASP.NET, confira [ASP.NET for Mobiles](/aspnet/mobile/overview) (ASP.NET para mobilidade).|
|<xref:System.Web.UI.MobileControls.Adapters.HtmlTextBoxAdapter?displayProperty=nameWithType>|O assembly System.Web.Mobile.dll foi substituído e não deve ser mais usado. Para saber mais sobre como desenvolver aplicativos móveis ASP.NET, confira [ASP.NET for Mobiles](/aspnet/mobile/overview) (ASP.NET para mobilidade).|
|<xref:System.Web.UI.MobileControls.Adapters.HtmlTextViewAdapter?displayProperty=nameWithType>|O assembly System.Web.Mobile.dll foi substituído e não deve ser mais usado. Para saber mais sobre como desenvolver aplicativos móveis ASP.NET, confira [ASP.NET for Mobiles](/aspnet/mobile/overview) (ASP.NET para mobilidade).|
|<xref:System.Web.UI.MobileControls.Adapters.HtmlValidationSummaryAdapter?displayProperty=nameWithType>|O assembly System.Web.Mobile.dll foi substituído e não deve ser mais usado. Para saber mais sobre como desenvolver aplicativos móveis ASP.NET, confira [ASP.NET for Mobiles](/aspnet/mobile/overview) (ASP.NET para mobilidade).|
|<xref:System.Web.UI.MobileControls.Adapters.HtmlValidatorAdapter?displayProperty=nameWithType>|O assembly System.Web.Mobile.dll foi substituído e não deve ser mais usado. Para saber mais sobre como desenvolver aplicativos móveis ASP.NET, confira [ASP.NET for Mobiles](/aspnet/mobile/overview) (ASP.NET para mobilidade).|
|<xref:System.Web.UI.MobileControls.Adapters.MobileTextWriter?displayProperty=nameWithType>|O assembly System.Web.Mobile.dll foi substituído e não deve ser mais usado. Para saber mais sobre como desenvolver aplicativos móveis ASP.NET, confira [ASP.NET for Mobiles](/aspnet/mobile/overview) (ASP.NET para mobilidade).|
|<xref:System.Web.UI.MobileControls.Adapters.MultiPartWriter?displayProperty=nameWithType>|O assembly System.Web.Mobile.dll foi substituído e não deve ser mais usado. Para saber mais sobre como desenvolver aplicativos móveis ASP.NET, confira [ASP.NET for Mobiles](/aspnet/mobile/overview) (ASP.NET para mobilidade).|
|<xref:System.Web.UI.MobileControls.Adapters.UpWmlMobileTextWriter?displayProperty=nameWithType>|O assembly System.Web.Mobile.dll foi substituído e não deve ser mais usado. Para saber mais sobre como desenvolver aplicativos móveis ASP.NET, confira [ASP.NET for Mobiles](/aspnet/mobile/overview) (ASP.NET para mobilidade).|
|<xref:System.Web.UI.MobileControls.Adapters.UpWmlPageAdapter?displayProperty=nameWithType>|O assembly System.Web.Mobile.dll foi substituído e não deve ser mais usado. Para saber mais sobre como desenvolver aplicativos móveis ASP.NET, confira [ASP.NET for Mobiles](/aspnet/mobile/overview) (ASP.NET para mobilidade).|
|<xref:System.Web.UI.MobileControls.Adapters.WmlCalendarAdapter?displayProperty=nameWithType>|O assembly System.Web.Mobile.dll foi substituído e não deve ser mais usado. Para saber mais sobre como desenvolver aplicativos móveis ASP.NET, confira [ASP.NET for Mobiles](/aspnet/mobile/overview) (ASP.NET para mobilidade).|
|<xref:System.Web.UI.MobileControls.Adapters.WmlCommandAdapter?displayProperty=nameWithType>|O assembly System.Web.Mobile.dll foi substituído e não deve ser mais usado. Para saber mais sobre como desenvolver aplicativos móveis ASP.NET, confira [ASP.NET for Mobiles](/aspnet/mobile/overview) (ASP.NET para mobilidade).|
|<xref:System.Web.UI.MobileControls.Adapters.WmlControlAdapter?displayProperty=nameWithType>|O assembly System.Web.Mobile.dll foi substituído e não deve ser mais usado. Para saber mais sobre como desenvolver aplicativos móveis ASP.NET, confira [ASP.NET for Mobiles](/aspnet/mobile/overview) (ASP.NET para mobilidade).|
|<xref:System.Web.UI.MobileControls.Adapters.WmlFormAdapter?displayProperty=nameWithType>|O assembly System.Web.Mobile.dll foi substituído e não deve ser mais usado. Para saber mais sobre como desenvolver aplicativos móveis ASP.NET, confira [ASP.NET for Mobiles](/aspnet/mobile/overview) (ASP.NET para mobilidade).|
|<xref:System.Web.UI.MobileControls.Adapters.WmlImageAdapter?displayProperty=nameWithType>|O assembly System.Web.Mobile.dll foi substituído e não deve ser mais usado. Para saber mais sobre como desenvolver aplicativos móveis ASP.NET, confira [ASP.NET for Mobiles](/aspnet/mobile/overview) (ASP.NET para mobilidade).|
|<xref:System.Web.UI.MobileControls.Adapters.WmlLabelAdapter?displayProperty=nameWithType>|O assembly System.Web.Mobile.dll foi substituído e não deve ser mais usado. Para saber mais sobre como desenvolver aplicativos móveis ASP.NET, confira [ASP.NET for Mobiles](/aspnet/mobile/overview) (ASP.NET para mobilidade).|
|<xref:System.Web.UI.MobileControls.Adapters.WmlLinkAdapter?displayProperty=nameWithType>|O assembly System.Web.Mobile.dll foi substituído e não deve ser mais usado. Para saber mais sobre como desenvolver aplicativos móveis ASP.NET, confira [ASP.NET for Mobiles](/aspnet/mobile/overview) (ASP.NET para mobilidade).|
|<xref:System.Web.UI.MobileControls.Adapters.WmlListAdapter?displayProperty=nameWithType>|O assembly System.Web.Mobile.dll foi substituído e não deve ser mais usado. Para saber mais sobre como desenvolver aplicativos móveis ASP.NET, confira [ASP.NET for Mobiles](/aspnet/mobile/overview) (ASP.NET para mobilidade).|
|<xref:System.Web.UI.MobileControls.Adapters.WmlLiteralTextAdapter?displayProperty=nameWithType>|O assembly System.Web.Mobile.dll foi substituído e não deve ser mais usado. Para saber mais sobre como desenvolver aplicativos móveis ASP.NET, confira [ASP.NET for Mobiles](/aspnet/mobile/overview) (ASP.NET para mobilidade).|
|<xref:System.Web.UI.MobileControls.Adapters.WmlMobileTextWriter?displayProperty=nameWithType>|O assembly System.Web.Mobile.dll foi substituído e não deve ser mais usado. Para saber mais sobre como desenvolver aplicativos móveis ASP.NET, confira [ASP.NET for Mobiles](/aspnet/mobile/overview) (ASP.NET para mobilidade).|
|<xref:System.Web.UI.MobileControls.Adapters.WmlObjectListAdapter?displayProperty=nameWithType>|O assembly System.Web.Mobile.dll foi substituído e não deve ser mais usado. Para saber mais sobre como desenvolver aplicativos móveis ASP.NET, confira [ASP.NET for Mobiles](/aspnet/mobile/overview) (ASP.NET para mobilidade).|
|<xref:System.Web.UI.MobileControls.Adapters.WmlPageAdapter?displayProperty=nameWithType>|O assembly System.Web.Mobile.dll foi substituído e não deve ser mais usado. Para saber mais sobre como desenvolver aplicativos móveis ASP.NET, confira [ASP.NET for Mobiles](/aspnet/mobile/overview) (ASP.NET para mobilidade).|
|<xref:System.Web.UI.MobileControls.Adapters.WmlPanelAdapter?displayProperty=nameWithType>|O assembly System.Web.Mobile.dll foi substituído e não deve ser mais usado. Para saber mais sobre como desenvolver aplicativos móveis ASP.NET, confira [ASP.NET for Mobiles](/aspnet/mobile/overview) (ASP.NET para mobilidade).|
|<xref:System.Web.UI.MobileControls.Adapters.WmlPhoneCallAdapter?displayProperty=nameWithType>|O assembly System.Web.Mobile.dll foi substituído e não deve ser mais usado. Para saber mais sobre como desenvolver aplicativos móveis ASP.NET, confira [ASP.NET for Mobiles](/aspnet/mobile/overview) (ASP.NET para mobilidade).|
|<xref:System.Web.UI.MobileControls.Adapters.WmlPostFieldType?displayProperty=nameWithType>|O assembly System.Web.Mobile.dll foi substituído e não deve ser mais usado. Para saber mais sobre como desenvolver aplicativos móveis ASP.NET, confira [ASP.NET for Mobiles](/aspnet/mobile/overview) (ASP.NET para mobilidade).|
|<xref:System.Web.UI.MobileControls.Adapters.WmlSelectionListAdapter?displayProperty=nameWithType>|O assembly System.Web.Mobile.dll foi substituído e não deve ser mais usado. Para saber mais sobre como desenvolver aplicativos móveis ASP.NET, confira [ASP.NET for Mobiles](/aspnet/mobile/overview) (ASP.NET para mobilidade).|
|<xref:System.Web.UI.MobileControls.Adapters.WmlTextBoxAdapter?displayProperty=nameWithType>|O assembly System.Web.Mobile.dll foi substituído e não deve ser mais usado. Para saber mais sobre como desenvolver aplicativos móveis ASP.NET, confira [ASP.NET for Mobiles](/aspnet/mobile/overview) (ASP.NET para mobilidade).|
|<xref:System.Web.UI.MobileControls.Adapters.WmlTextViewAdapter?displayProperty=nameWithType>|O assembly System.Web.Mobile.dll foi substituído e não deve ser mais usado. Para saber mais sobre como desenvolver aplicativos móveis ASP.NET, confira [ASP.NET for Mobiles](/aspnet/mobile/overview) (ASP.NET para mobilidade).|
|<xref:System.Web.UI.MobileControls.Adapters.WmlValidationSummaryAdapter?displayProperty=nameWithType>|O assembly System.Web.Mobile.dll foi substituído e não deve ser mais usado. Para saber mais sobre como desenvolver aplicativos móveis ASP.NET, confira [ASP.NET for Mobiles](/aspnet/mobile/overview) (ASP.NET para mobilidade).|
|<xref:System.Web.UI.MobileControls.Adapters.WmlValidatorAdapter?displayProperty=nameWithType>|O assembly System.Web.Mobile.dll foi substituído e não deve ser mais usado. Para saber mais sobre como desenvolver aplicativos móveis ASP.NET, confira [ASP.NET for Mobiles](/aspnet/mobile/overview) (ASP.NET para mobilidade).|
|<xref:System.Web.UI.MobileControls.Adapters.XhtmlAdapters.Doctype?displayProperty=nameWithType>|O assembly System.Web.Mobile.dll foi substituído e não deve ser mais usado. Para saber mais sobre como desenvolver aplicativos móveis ASP.NET, confira [ASP.NET for Mobiles](/aspnet/mobile/overview) (ASP.NET para mobilidade).|
|<xref:System.Web.UI.MobileControls.Adapters.XhtmlAdapters.StyleSheetLocation?displayProperty=nameWithType>|O assembly System.Web.Mobile.dll foi substituído e não deve ser mais usado. Para saber mais sobre como desenvolver aplicativos móveis ASP.NET, confira [ASP.NET for Mobiles](/aspnet/mobile/overview) (ASP.NET para mobilidade).|
|<xref:System.Web.UI.MobileControls.Adapters.XhtmlAdapters.XhtmlCalendarAdapter?displayProperty=nameWithType>|O assembly System.Web.Mobile.dll foi substituído e não deve ser mais usado. Para saber mais sobre como desenvolver aplicativos móveis ASP.NET, confira [ASP.NET for Mobiles](/aspnet/mobile/overview) (ASP.NET para mobilidade).|
|<xref:System.Web.UI.MobileControls.Adapters.XhtmlAdapters.XhtmlCommandAdapter?displayProperty=nameWithType>|O assembly System.Web.Mobile.dll foi substituído e não deve ser mais usado. Para saber mais sobre como desenvolver aplicativos móveis ASP.NET, confira [ASP.NET for Mobiles](/aspnet/mobile/overview) (ASP.NET para mobilidade).|
|<xref:System.Web.UI.MobileControls.Adapters.XhtmlAdapters.XhtmlControlAdapter?displayProperty=nameWithType>|O assembly System.Web.Mobile.dll foi substituído e não deve ser mais usado. Para saber mais sobre como desenvolver aplicativos móveis ASP.NET, confira [ASP.NET for Mobiles](/aspnet/mobile/overview) (ASP.NET para mobilidade).|
|<xref:System.Web.UI.MobileControls.Adapters.XhtmlAdapters.XhtmlCssHandler?displayProperty=nameWithType>|O assembly System.Web.Mobile.dll foi substituído e não deve ser mais usado. Para saber mais sobre como desenvolver aplicativos móveis ASP.NET, confira [ASP.NET for Mobiles](/aspnet/mobile/overview) (ASP.NET para mobilidade).|
|<xref:System.Web.UI.MobileControls.Adapters.XhtmlAdapters.XhtmlFormAdapter?displayProperty=nameWithType>|O assembly System.Web.Mobile.dll foi substituído e não deve ser mais usado. Para saber mais sobre como desenvolver aplicativos móveis ASP.NET, confira [ASP.NET for Mobiles](/aspnet/mobile/overview) (ASP.NET para mobilidade).|
|<xref:System.Web.UI.MobileControls.Adapters.XhtmlAdapters.XhtmlImageAdapter?displayProperty=nameWithType>|O assembly System.Web.Mobile.dll foi substituído e não deve ser mais usado. Para saber mais sobre como desenvolver aplicativos móveis ASP.NET, confira [ASP.NET for Mobiles](/aspnet/mobile/overview) (ASP.NET para mobilidade).|
|<xref:System.Web.UI.MobileControls.Adapters.XhtmlAdapters.XhtmlLabelAdapter?displayProperty=nameWithType>|O assembly System.Web.Mobile.dll foi substituído e não deve ser mais usado. Para saber mais sobre como desenvolver aplicativos móveis ASP.NET, confira [ASP.NET for Mobiles](/aspnet/mobile/overview) (ASP.NET para mobilidade).|
|<xref:System.Web.UI.MobileControls.Adapters.XhtmlAdapters.XhtmlLinkAdapter?displayProperty=nameWithType>|O assembly System.Web.Mobile.dll foi substituído e não deve ser mais usado. Para saber mais sobre como desenvolver aplicativos móveis ASP.NET, confira [ASP.NET for Mobiles](/aspnet/mobile/overview) (ASP.NET para mobilidade).|
|<xref:System.Web.UI.MobileControls.Adapters.XhtmlAdapters.XhtmlListAdapter?displayProperty=nameWithType>|O assembly System.Web.Mobile.dll foi substituído e não deve ser mais usado. Para saber mais sobre como desenvolver aplicativos móveis ASP.NET, confira [ASP.NET for Mobiles](/aspnet/mobile/overview) (ASP.NET para mobilidade).|
|<xref:System.Web.UI.MobileControls.Adapters.XhtmlAdapters.XhtmlLiteralTextAdapter?displayProperty=nameWithType>|O assembly System.Web.Mobile.dll foi substituído e não deve ser mais usado. Para saber mais sobre como desenvolver aplicativos móveis ASP.NET, confira [ASP.NET for Mobiles](/aspnet/mobile/overview) (ASP.NET para mobilidade).|
|<xref:System.Web.UI.MobileControls.Adapters.XhtmlAdapters.XhtmlMobileTextWriter?displayProperty=nameWithType>|O assembly System.Web.Mobile.dll foi substituído e não deve ser mais usado. Para saber mais sobre como desenvolver aplicativos móveis ASP.NET, confira [ASP.NET for Mobiles](/aspnet/mobile/overview) (ASP.NET para mobilidade).|
|<xref:System.Web.UI.MobileControls.Adapters.XhtmlAdapters.XhtmlObjectListAdapter?displayProperty=nameWithType>|O assembly System.Web.Mobile.dll foi substituído e não deve ser mais usado. Para saber mais sobre como desenvolver aplicativos móveis ASP.NET, confira [ASP.NET for Mobiles](/aspnet/mobile/overview) (ASP.NET para mobilidade).|
|<xref:System.Web.UI.MobileControls.Adapters.XhtmlAdapters.XhtmlPageAdapter?displayProperty=nameWithType>|O assembly System.Web.Mobile.dll foi substituído e não deve ser mais usado. Para saber mais sobre como desenvolver aplicativos móveis ASP.NET, confira [ASP.NET for Mobiles](/aspnet/mobile/overview) (ASP.NET para mobilidade).|
|<xref:System.Web.UI.MobileControls.Adapters.XhtmlAdapters.XhtmlPanelAdapter?displayProperty=nameWithType>|O assembly System.Web.Mobile.dll foi substituído e não deve ser mais usado. Para saber mais sobre como desenvolver aplicativos móveis ASP.NET, confira [ASP.NET for Mobiles](/aspnet/mobile/overview) (ASP.NET para mobilidade).|
|<xref:System.Web.UI.MobileControls.Adapters.XhtmlAdapters.XhtmlPhoneCallAdapter?displayProperty=nameWithType>|O assembly System.Web.Mobile.dll foi substituído e não deve ser mais usado. Para saber mais sobre como desenvolver aplicativos móveis ASP.NET, confira [ASP.NET for Mobiles](/aspnet/mobile/overview) (ASP.NET para mobilidade).|
|<xref:System.Web.UI.MobileControls.Adapters.XhtmlAdapters.XhtmlSelectionListAdapter?displayProperty=nameWithType>|O assembly System.Web.Mobile.dll foi substituído e não deve ser mais usado. Para saber mais sobre como desenvolver aplicativos móveis ASP.NET, confira [ASP.NET for Mobiles](/aspnet/mobile/overview) (ASP.NET para mobilidade).|
|<xref:System.Web.UI.MobileControls.Adapters.XhtmlAdapters.XhtmlTextBoxAdapter?displayProperty=nameWithType>|O assembly System.Web.Mobile.dll foi substituído e não deve ser mais usado. Para saber mais sobre como desenvolver aplicativos móveis ASP.NET, confira [ASP.NET for Mobiles](/aspnet/mobile/overview) (ASP.NET para mobilidade).|
|<xref:System.Web.UI.MobileControls.Adapters.XhtmlAdapters.XhtmlTextViewAdapter?displayProperty=nameWithType>|O assembly System.Web.Mobile.dll foi substituído e não deve ser mais usado. Para saber mais sobre como desenvolver aplicativos móveis ASP.NET, confira [ASP.NET for Mobiles](/aspnet/mobile/overview) (ASP.NET para mobilidade).|
|<xref:System.Web.UI.MobileControls.Adapters.XhtmlAdapters.XhtmlValidationSummaryAdapter?displayProperty=nameWithType>|O assembly System.Web.Mobile.dll foi substituído e não deve ser mais usado. Para saber mais sobre como desenvolver aplicativos móveis ASP.NET, confira [ASP.NET for Mobiles](/aspnet/mobile/overview) (ASP.NET para mobilidade).|
|<xref:System.Web.UI.MobileControls.Adapters.XhtmlAdapters.XhtmlValidatorAdapter?displayProperty=nameWithType>|O assembly System.Web.Mobile.dll foi substituído e não deve ser mais usado. Para saber mais sobre como desenvolver aplicativos móveis ASP.NET, confira [ASP.NET for Mobiles](/aspnet/mobile/overview) (ASP.NET para mobilidade).|

[Voltar ao início](#introduction)

<a name="workflow_activities"></a>

### <a name="assembly-systemworkflowactivitiesdll"></a>Assembly: System.Workflow.Activities.dll

|Tipo|Mensagem|
|----------|-------------|
|Todos os tipos no namespace <xref:System.Workflow.Activities?displayProperty=nameWithType>|Substituído primeiramente no .NET Framework 4.5.<br /><br /> Os tipos System.Workflow.\* são preteridos. Em vez de isso, use os novos tipos de <xref:System.Activities>\*.|
|<xref:System.Workflow.Activities.Configuration.ActiveDirectoryRoleFactoryConfiguration?displayProperty=nameWithType>|Substituído primeiramente no .NET Framework 4.5.<br /><br /> Os tipos System.Workflow.\* são preteridos. Em vez de isso, use os novos tipos de <xref:System.Activities>\*.|
|<xref:System.Workflow.Activities.Rules.RuleActionTrackingEvent?displayProperty=nameWithType>|Substituído primeiramente no .NET Framework 4.5.<br /><br /> Os tipos System.Workflow.\* são preteridos. Em vez de isso, use os novos tipos de <xref:System.Activities>\*.|
|<xref:System.Workflow.Activities.Rules.RuleConditionReference?displayProperty=nameWithType>|Substituído primeiramente no .NET Framework 4.5.<br /><br /> Os tipos System.Workflow.\* são preteridos. Em vez de isso, use os novos tipos de <xref:System.Activities>\*.|
|<xref:System.Workflow.Activities.Rules.RuleSetReference?displayProperty=nameWithType>|Substituído primeiramente no .NET Framework 4.5.<br /><br /> Os tipos System.Workflow.\* são preteridos. Em vez de isso, use os novos tipos de <xref:System.Activities>\*.|

[Voltar ao início](#introduction)

<a name="workflow_componentmodel"></a>

### <a name="assembly-systemworkflowcomponentmodeldll"></a>Assembly: System.Workflow.ComponentModel.dll

|Tipo|Mensagem|
|----------|-------------|
|Todos os tipos no namespace <xref:System.Workflow.ComponentModel>, exceto <xref:System.Workflow.ComponentModel.GetValueOverride?displayProperty=nameWithType> e <xref:System.Workflow.ComponentModel.SetValueOverride?displayProperty=nameWithType>|Substituído primeiramente no .NET Framework 4.5.<br /><br /> Os tipos System.Workflow.\* são preteridos. Em vez de isso, use os novos tipos de <xref:System.Activities>\*.|
|Todos os tipos no namespace <xref:System.Workflow.ComponentModel.Compiler>, exceto <xref:System.Workflow.ComponentModel.Compiler.ValidationError?displayProperty=nameWithType> e <xref:System.Workflow.ComponentModel.Compiler.ValidationErrorCollection?displayProperty=nameWithType>|Substituído primeiramente no .NET Framework 4.5.<br /><br /> Os tipos System.Workflow.\* são preteridos. Em vez de isso, use os novos tipos de <xref:System.Activities>\*.|
|Todos os tipos no namespace <xref:System.Workflow.ComponentModel.Design>, exceto <xref:System.Workflow.ComponentModel.Design.ConnectorEventHandler>|Substituído primeiramente no .NET Framework 4.5.<br /><br /> Os tipos System.Workflow.\* são preteridos. Em vez de isso, use os novos tipos de <xref:System.Activities>\*.|
|<xref:System.Workflow.ComponentModel.Serialization.ActivityCodeDomSerializationManager?displayProperty=nameWithType>|Substituído primeiramente no .NET Framework 4.5.<br /><br /> Os tipos System.Workflow.\* são preteridos. Em vez de isso, use os novos tipos de <xref:System.Activities>\*.|
|<xref:System.Workflow.ComponentModel.Serialization.ActivityCodeDomSerializer?displayProperty=nameWithType>|Substituído primeiramente no .NET Framework 4.5.<br /><br /> Os tipos System.Workflow.\* são preteridos. Em vez de isso, use os novos tipos de <xref:System.Activities>\*.|
|<xref:System.Workflow.ComponentModel.Serialization.ActivityMarkupSerializer?displayProperty=nameWithType>|Substituído primeiramente no .NET Framework 4.5.<br /><br /> Os tipos System.Workflow.\* são preteridos. Em vez de isso, use os novos tipos de <xref:System.Activities>\*.|
|<xref:System.Workflow.ComponentModel.Serialization.ActivitySurrogateSelector?displayProperty=nameWithType>|Substituído primeiramente no .NET Framework 4.5.<br /><br /> Os tipos System.Workflow.\* são preteridos. Em vez de isso, use os novos tipos de <xref:System.Activities>\*.|
|<xref:System.Workflow.ComponentModel.Serialization.ActivityTypeCodeDomSerializer?displayProperty=nameWithType>|Substituído primeiramente no .NET Framework 4.5.<br /><br /> Os tipos System.Workflow.\* são preteridos. Em vez de isso, use os novos tipos de <xref:System.Activities>\*.|
|<xref:System.Workflow.ComponentModel.Serialization.CompositeActivityMarkupSerializer?displayProperty=nameWithType>|Substituído primeiramente no .NET Framework 4.5.<br /><br /> Os tipos System.Workflow.\* são preteridos. Em vez de isso, use os novos tipos de <xref:System.Activities>\*.|
|<xref:System.Workflow.ComponentModel.Serialization.DependencyObjectCodeDomSerializer?displayProperty=nameWithType>|Substituído primeiramente no .NET Framework 4.5.<br /><br /> Os tipos System.Workflow.\* são preteridos. Em vez de isso, use os novos tipos de <xref:System.Activities>\*.|

[Voltar ao início](#introduction)

<a name="workflow_runtime"></a>

### <a name="assembly-systemworkflowruntimedll"></a>Assembly: System.Workflow.Runtime.dll

|Tipo|Mensagem|
|----------|-------------|
|<xref:System.Activities.Statements.Interop?displayProperty=nameWithType>|Substituído primeiramente no .NET Framework 4.5.<br /><br />Os tipos Workflow Foundation 3.0 foram preteridos. Em vez disso, use os tipos do Workflow 4.0 de <xref:System.Activities>\*.|
|<xref:System.Activities.Tracking.InteropTrackingRecord?displayProperty=nameWithType>|Substituído primeiramente no .NET Framework 4.5.<br /><br />Os tipos Workflow Foundation 3.0 foram preteridos. Em vez disso, use os tipos do Workflow 4.0 de <xref:System.Activities>\*.|
|Todos os tipos no namespace <xref:System.Workflow.Runtime>|Substituído primeiramente no .NET Framework 4.5.<br /><br /> Os tipos System.Workflow.\* são preteridos. Em vez de isso, use os novos tipos de <xref:System.Activities>\*.|
|Todos os tipos no namespace <xref:System.Workflow.Runtime.Configuration>|Substituído primeiramente no .NET Framework 4.5.<br /><br /> Os tipos System.Workflow.\* são preteridos. Em vez de isso, use os novos tipos de <xref:System.Activities>\*.|
|Todos os tipos no namespace <xref:System.Workflow.Runtime.DebugEngine>, exceto <xref:System.Workflow.Runtime.DebugEngine.DebugEngineCallback>|Substituído primeiramente no .NET Framework 4.5.<br /><br /> Os tipos System.Workflow.\* são preteridos. Em vez de isso, use os novos tipos de <xref:System.Activities>\*.|
|Todos os tipos no namespace <xref:System.Workflow.Runtime.Hosting>, exceto <xref:System.Workflow.Runtime.Hosting.WorkflowCommitWorkBatchService.CommitWorkBatchCallback>|Substituído primeiramente no .NET Framework 4.5.<br /><br /> Os tipos System.Workflow.\* são preteridos. Em vez de isso, use os novos tipos de <xref:System.Activities>\*.|
|Todos os tipos no namespace <xref:System.Workflow.Runtime.Tracking>|Substituído primeiramente no .NET Framework 4.5.<br /><br /> Os tipos System.Workflow.\* são preteridos. Em vez de isso, use os novos tipos de <xref:System.Activities>\*.|

[Voltar ao início](#introduction)

<a name="workflowservices"></a>

### <a name="assembly-systemworkflowservicesdll"></a>Assembly: System.WorkflowServices.dll

|Tipo|Mensagem|
|----------|-------------|
|<xref:System.ServiceModel.WorkflowServiceHost?displayProperty=nameWithType>|Substituído primeiramente no .NET Framework 4.5.<br /><br /> Os tipos WF 3 foram substituídos. Em vez de isso, use os novos tipos do WF 4 de <xref:System.Activities>\*.|
|<xref:System.ServiceModel.Activation.WorkflowServiceHostFactory?displayProperty=nameWithType>|Substituído primeiramente no .NET Framework 4.5.<br /><br /> Os tipos WF 3 foram substituídos. Em vez de isso, use os novos tipos do WF 4 de <xref:System.Activities>\*.|
|<xref:System.ServiceModel.Activities.Description.WorkflowRuntimeEndpoint?displayProperty=nameWithType>|Substituído primeiramente no .NET Framework 4.5.<br /><br /> Os tipos WF 3 foram substituídos. Em vez de isso, use os novos tipos do WF 4 de <xref:System.Activities>\*.|
|<xref:System.ServiceModel.Configuration.ExtendedWorkflowRuntimeServiceElementCollection?displayProperty=nameWithType>|Substituído primeiramente no .NET Framework 4.5.<br /><br /> Os tipos WF 3 foram substituídos. Em vez de isso, use os novos tipos do WF 4 de <xref:System.Activities>\*.|
|<xref:System.ServiceModel.Configuration.PersistenceProviderElement?displayProperty=nameWithType>|Substituído primeiramente no .NET Framework 4.5.<br /><br /> Os tipos WF 3 foram substituídos. Em vez de isso, use os novos tipos do WF 4 de <xref:System.Activities>\*.|
|<xref:System.ServiceModel.Configuration.WorkflowRuntimeElement?displayProperty=nameWithType>|Substituído primeiramente no .NET Framework 4.5.<br /><br /> Os tipos WF 3 foram substituídos. Em vez de isso, use os novos tipos do WF 4 de <xref:System.Activities>\*.|
|<xref:System.ServiceModel.Description.DurableOperationAttribute?displayProperty=nameWithType>|Substituído primeiramente no .NET Framework 4.5.<br /><br /> Os tipos WF 3 foram substituídos. Em vez de isso, use os novos tipos do WF 4 de <xref:System.Activities>\*.|
|<xref:System.ServiceModel.Description.DurableServiceAttribute?displayProperty=nameWithType>|Substituído primeiramente no .NET Framework 4.5.<br /><br /> Os tipos WF 3 foram substituídos. Em vez de isso, use os novos tipos do WF 4 de <xref:System.Activities>\*.|
|<xref:System.ServiceModel.Description.PersistenceProviderBehavior?displayProperty=nameWithType>|Substituído primeiramente no .NET Framework 4.5.<br /><br /> Os tipos WF 3 foram substituídos. Em vez de isso, use os novos tipos do WF 4 de <xref:System.Activities>\*.|
|<xref:System.ServiceModel.Description.UnknownExceptionAction?displayProperty=nameWithType>|Substituído primeiramente no .NET Framework 4.5.<br /><br /> Os tipos WF 3 foram substituídos. Em vez de isso, use os novos tipos do WF 4 de <xref:System.Activities>\*.|
|<xref:System.ServiceModel.Description.WorkflowRuntimeBehavior?displayProperty=nameWithType>|Substituído primeiramente no .NET Framework 4.5.<br /><br /> Os tipos WF 3 foram substituídos. Em vez de isso, use os novos tipos do WF 4 de <xref:System.Activities>\*.|
|<xref:System.ServiceModel.Dispatcher.DurableOperationContext?displayProperty=nameWithType>|Substituído primeiramente no .NET Framework 4.5.<br /><br /> Os tipos WF 3 foram substituídos. Em vez de isso, use os novos tipos do WF 4 de <xref:System.Activities>\*.|
|<xref:System.ServiceModel.Persistence.InstanceLockException?displayProperty=nameWithType>|Substituído primeiramente no .NET Framework 4.5.<br /><br /> Os tipos WF 3 foram substituídos. Em vez de isso, use os novos tipos do WF 4 de <xref:System.Activities>\*.|
|<xref:System.ServiceModel.Persistence.InstanceNotFoundException?displayProperty=nameWithType>|Substituído primeiramente no .NET Framework 4.5.<br /><br /> Os tipos WF 3 foram substituídos. Em vez de isso, use os novos tipos do WF 4 de <xref:System.Activities>\*.|
|<xref:System.ServiceModel.Persistence.LockingPersistenceProvider?displayProperty=nameWithType>|Substituído primeiramente no .NET Framework 4.5.<br /><br /> Os tipos WF 3 foram substituídos. Em vez de isso, use os novos tipos do WF 4 de <xref:System.Activities>\*.|
|<xref:System.ServiceModel.Persistence.PersistenceException?displayProperty=nameWithType>|Substituído primeiramente no .NET Framework 4.5.<br /><br /> Os tipos WF 3 foram substituídos. Em vez de isso, use os novos tipos do WF 4 de <xref:System.Activities>\*.|
|<xref:System.ServiceModel.Persistence.PersistenceProvider?displayProperty=nameWithType>|Substituído primeiramente no .NET Framework 4.5.<br /><br /> Os tipos WF 3 foram substituídos. Em vez de isso, use os novos tipos do WF 4 de <xref:System.Activities>\*.|
|<xref:System.ServiceModel.Persistence.PersistenceProviderFactory?displayProperty=nameWithType>|Substituído primeiramente no .NET Framework 4.5.<br /><br /> Os tipos WF 3 foram substituídos. Em vez de isso, use os novos tipos do WF 4 de <xref:System.Activities>\*.|
|<xref:System.ServiceModel.Persistence.SqlPersistenceProviderFactory?displayProperty=nameWithType>|Substituído primeiramente no .NET Framework 4.5.<br /><br /> Os tipos WF 3 foram substituídos. Em vez de isso, use os novos tipos do WF 4 de <xref:System.Activities>\*.|
|Todos os tipos no namespace <xref:System.Workflow.Activities?displayProperty=nameWithType>|Substituído primeiramente no .NET Framework 4.5.<br /><br /> Os tipos WF 3 foram substituídos. Em vez de isso, use os novos tipos do WF 4 de <xref:System.Activities>\*.|
|<xref:System.Workflow.Runtime.Hosting.ChannelManagerService?displayProperty=nameWithType>|Substituído primeiramente no .NET Framework 4.5.<br /><br /> Os tipos WF 3 foram substituídos. Em vez de isso, use os novos tipos do WF 4 de <xref:System.Activities>\*.|

[Voltar ao início](#introduction)

<a name="xaml"></a>

### <a name="assembly-systemxamldll"></a>Assembly: System.Xaml.dll

|Tipo|Mensagem|
|----------|-------------|
|<xref:System.Windows.Markup.AcceptedMarkupExtensionExpressionTypeAttribute?displayProperty=nameWithType>|Ele não é usado pelo analisador XAML. Observe <xref:System.Windows.Markup.XamlSetMarkupExtensionAttribute?displayProperty=nameWithType>.|

[Voltar ao início](#introduction)

<a name="xml"></a>

### <a name="assembly-systemxmldll"></a>Assembly: System.Xml.dll

|Tipo|Mensagem|
|----------|-------------|
|<xref:System.Xml.IApplicationResourceStreamResolver?displayProperty=nameWithType>|Substituído primeiramente no .NET Framework 4.5.<br /><br /> O uso desse tipo gera um erro do compilador.<br /><br /> Essa API dá suporte à infraestrutura do .NET Framework e não deve ser usada diretamente no código.|
|<xref:System.Xml.Schema.XmlSchemaCollection?displayProperty=nameWithType>|Use <xref:System.Xml.Schema.XmlSchemaSet?displayProperty=nameWithType> na compilação e na validação do esquema.|
|<xref:System.Xml.XmlValidatingReader?displayProperty=nameWithType>|Use um <xref:System.Xml.XmlReader?displayProperty=nameWithType> criado pelo método <xref:System.Xml.XmlReader.Create%2A?displayProperty=nameWithType> usando o <xref:System.Xml.XmlReaderSettings?displayProperty=nameWithType> apropriado em seu lugar.|
|<xref:System.Xml.XmlXapResolver?displayProperty=nameWithType>|O uso desse tipo gera um erro do compilador. Essa API dá suporte à infraestrutura do .NET Framework e não deve ser usada diretamente no código.|
|<xref:System.Xml.Xsl.XslTransform?displayProperty=nameWithType>|Essa classe foi substituída. Use <xref:System.Xml.Xsl.XslCompiledTransform?displayProperty=nameWithType> em seu lugar.|

[Voltar ao início](#introduction)

<a name="WindowsBase"></a>

### <a name="assembly-windowsbasedll"></a>Assembly: WindowsBase.dll

|Tipo|Mensagem|
|----------|-------------|
|<xref:System.Windows.Markup.IReceiveMarkupExtension?displayProperty=nameWithType>|<xref:System.Windows.Markup.IReceiveMarkupExtension?displayProperty=nameWithType> foi preterido. Essa interface não é mais usada.|

[Voltar ao início](#introduction)

<a name="obsolete_types_in_microsoft_assemblies"></a>

## <a name="obsolete-types-in-microsoft-assemblies"></a>Tipos obsoletos em assemblies Microsoft

As seções a seguir listam os tipos obsoletos em assemblies Microsoft. Esses assemblies são assemblies de finalidade especial, como assemblies que segmentam uma linguagem individual (por exemplo, Microsoft.JScript.dll ou Microsoft.VisualC.dll).

<a name="IEHost"></a>

### <a name="assembly-iehostdll-and-ieexecexe"></a>Assembly: IEHost.dll e IEExec.exe

Os assemblies IEHost.dll e IEExec.exe foram removidos do .NET Framework. Todos os seus tipos e membros estão obsoletos e não são compatíveis desde o .NET Framework 4. Esses assemblies foram usados para hospedar controles de Windows Forms e executar executáveis no Internet Explorer. Entre as alternativas recomendadas estão ClickOnce, aplicativos de navegador XAML (XBAP) e o Microsoft Silverlight.

[Voltar ao início](#introduction)

<a name="Engine"></a>

### <a name="assembly-microsoftbuildenginedll"></a>Assembly: Microsoft.Build.Engine.dll

|Tipo|Mensagem|
|----------|-------------|
|<xref:Microsoft.Build.BuildEngine.Engine?displayProperty=nameWithType>|Essa classe foi substituída. Use <xref:Microsoft.Build.Evaluation.ProjectCollection?displayProperty=nameWithType> do assembly *Microsoft.Build* em seu lugar.|
|<xref:Microsoft.Build.BuildEngine.Project?displayProperty=nameWithType>|Essa classe foi substituída. Use <xref:Microsoft.Build.Evaluation.ProjectCollection?displayProperty=nameWithType> do assembly *Microsoft.Build* em seu lugar.|

[Voltar ao início](#introduction)

<a name="jscript"></a>

### <a name="assembly-microsoftjscriptdll"></a>Assembly: Microsoft.JScript.dll

|Tipo|Mensagem|
|----------|-------------|
|<xref:Microsoft.JScript.Vsa.BaseVsaEngine?displayProperty=nameWithType>|Esse tipo foi preterido no Visual Studio 2005; não há nenhuma substituição para esse recurso. Consulte a documentação de <xref:System.CodeDom.Compiler.ICodeCompiler?displayProperty=nameWithType> para obter ajuda adicional.|
|<xref:Microsoft.JScript.Vsa.BaseVsaSite?displayProperty=nameWithType>|Esse tipo foi preterido no Visual Studio 2005; não há nenhuma substituição para esse recurso. Consulte a documentação de <xref:System.CodeDom.Compiler.ICodeCompiler?displayProperty=nameWithType> para obter ajuda adicional.|
|<xref:Microsoft.JScript.Vsa.BaseVsaStartup?displayProperty=nameWithType>|Esse tipo foi preterido no Visual Studio 2005; não há nenhuma substituição para esse recurso. Consulte a documentação de <xref:System.CodeDom.Compiler.ICodeCompiler?displayProperty=nameWithType> para obter ajuda adicional.|
|<xref:Microsoft.JScript.Vsa.IJSVsaCodeItem?displayProperty=nameWithType>|Esse tipo foi preterido no Visual Studio 2005; não há nenhuma substituição para esse recurso. Consulte a documentação de <xref:System.CodeDom.Compiler.ICodeCompiler?displayProperty=nameWithType> para obter ajuda adicional.|
|<xref:Microsoft.JScript.Vsa.IJSVsaEngine?displayProperty=nameWithType>|Esse tipo foi preterido no Visual Studio 2005; não há nenhuma substituição para esse recurso. Consulte a documentação de <xref:System.CodeDom.Compiler.ICodeCompiler?displayProperty=nameWithType> para obter ajuda adicional.|
|<xref:Microsoft.JScript.Vsa.IJSVsaError?displayProperty=nameWithType>|Esse tipo foi preterido no Visual Studio 2005; não há nenhuma substituição para esse recurso. Consulte a documentação de <xref:System.CodeDom.Compiler.ICodeCompiler?displayProperty=nameWithType> para obter ajuda adicional.|
|<xref:Microsoft.JScript.Vsa.IJSVsaGlobalItem?displayProperty=nameWithType>|Esse tipo foi preterido no Visual Studio 2005; não há nenhuma substituição para esse recurso. Consulte a documentação de <xref:System.CodeDom.Compiler.ICodeCompiler?displayProperty=nameWithType> para obter ajuda adicional.|
|<xref:Microsoft.JScript.Vsa.IJSVsaItem?displayProperty=nameWithType>|Esse tipo foi preterido no Visual Studio 2005; não há nenhuma substituição para esse recurso. Consulte a documentação de <xref:System.CodeDom.Compiler.ICodeCompiler?displayProperty=nameWithType> para obter ajuda adicional.|
|<xref:Microsoft.JScript.Vsa.IJSVsaItems?displayProperty=nameWithType>|Esse tipo foi preterido no Visual Studio 2005; não há nenhuma substituição para esse recurso. Consulte a documentação de <xref:System.CodeDom.Compiler.ICodeCompiler?displayProperty=nameWithType> para obter ajuda adicional.|
|<xref:Microsoft.JScript.Vsa.IJSVsaPersistSite?displayProperty=nameWithType>|Esse tipo foi preterido no Visual Studio 2005; não há nenhuma substituição para esse recurso. Consulte a documentação de <xref:System.CodeDom.Compiler.ICodeCompiler?displayProperty=nameWithType> para obter ajuda adicional.|
|<xref:Microsoft.JScript.Vsa.IJSVsaReferenceItem?displayProperty=nameWithType>|Esse tipo foi preterido no Visual Studio 2005; não há nenhuma substituição para esse recurso. Consulte a documentação de <xref:System.CodeDom.Compiler.ICodeCompiler?displayProperty=nameWithType> para obter ajuda adicional.|
|<xref:Microsoft.JScript.Vsa.IJSVsaSite?displayProperty=nameWithType>|Esse tipo foi preterido no Visual Studio 2005; não há nenhuma substituição para esse recurso. Consulte a documentação de <xref:System.CodeDom.Compiler.ICodeCompiler?displayProperty=nameWithType> para obter ajuda adicional.|
|<xref:Microsoft.JScript.Vsa.JSVsaError?displayProperty=nameWithType>|Esse tipo foi preterido no Visual Studio 2005; não há nenhuma substituição para esse recurso. Consulte a documentação de <xref:System.CodeDom.Compiler.ICodeCompiler?displayProperty=nameWithType> para obter ajuda adicional.|
|<xref:Microsoft.JScript.Vsa.JSVsaException?displayProperty=nameWithType>|Esse tipo foi preterido no Visual Studio 2005; não há nenhuma substituição para esse recurso. Consulte a documentação de <xref:System.CodeDom.Compiler.ICodeCompiler?displayProperty=nameWithType> para obter ajuda adicional.|
|<xref:Microsoft.JScript.Vsa.JSVsaItemFlag?displayProperty=nameWithType>|Esse tipo foi preterido no Visual Studio 2005; não há nenhuma substituição para esse recurso. Consulte a documentação de <xref:System.CodeDom.Compiler.ICodeCompiler?displayProperty=nameWithType> para obter ajuda adicional.|
|<xref:Microsoft.JScript.Vsa.JSVsaItemType?displayProperty=nameWithType>|Esse tipo foi preterido no Visual Studio 2005; não há nenhuma substituição para esse recurso. Consulte a documentação de <xref:System.CodeDom.Compiler.ICodeCompiler?displayProperty=nameWithType> para obter ajuda adicional.|
|<xref:Microsoft.JScript.Vsa.ResInfo?displayProperty=nameWithType>|Esse tipo foi preterido no Visual Studio 2005; não há nenhuma substituição para esse recurso. Consulte a documentação de <xref:System.CodeDom.Compiler.ICodeCompiler?displayProperty=nameWithType> para obter ajuda adicional.|
|<xref:Microsoft.JScript.Vsa.VsaEngine?displayProperty=nameWithType>|Esse tipo foi preterido no Visual Studio 2005; não há nenhuma substituição para esse recurso. Consulte a documentação de <xref:System.CodeDom.Compiler.ICodeCompiler?displayProperty=nameWithType> para obter ajuda adicional.|

[Voltar ao início](#introduction)

<a name="VBCompat"></a>

### <a name="assembly-microsoftvisualbasiccompatibilitydll"></a>Assembly: Microsoft.VisualBasic.Compatibility.dll

Para obter informações sobre a migração do Visual Basic 6, consulte [Centro de Recursos do Visual Basic 6.0](https://docs.microsoft.com/previous-versions/visualstudio/visual-basic-6/visual-basic-6.0-documentation).

|Tipo|Mensagem|
|----------|-------------|
|<xref:Microsoft.VisualBasic.Compatibility.VB6.BaseControlArray?displayProperty=nameWithType>|Este membro está obsoleto.|
|<xref:Microsoft.VisualBasic.Compatibility.VB6.BaseOcxArray?displayProperty=nameWithType>|Este membro está obsoleto.|
|<xref:Microsoft.VisualBasic.Compatibility.VB6.ButtonArray?displayProperty=nameWithType>|Este membro está obsoleto.|
|<xref:Microsoft.VisualBasic.Compatibility.VB6.CheckBoxArray?displayProperty=nameWithType>|Este membro está obsoleto.|
|<xref:Microsoft.VisualBasic.Compatibility.VB6.CheckedListBoxArray?displayProperty=nameWithType>|Este membro está obsoleto.|
|<xref:Microsoft.VisualBasic.Compatibility.VB6.ColorDialogArray?displayProperty=nameWithType>|Este membro está obsoleto.|
|<xref:Microsoft.VisualBasic.Compatibility.VB6.ComboBoxArray?displayProperty=nameWithType>|Este membro está obsoleto.|
|<xref:Microsoft.VisualBasic.Compatibility.VB6.DirListBox?displayProperty=nameWithType>|Este membro está obsoleto.|
|<xref:Microsoft.VisualBasic.Compatibility.VB6.DirListBoxArray?displayProperty=nameWithType>|Este membro está obsoleto.|
|<xref:Microsoft.VisualBasic.Compatibility.VB6.DriveListBox?displayProperty=nameWithType>|Este membro está obsoleto.|
|<xref:Microsoft.VisualBasic.Compatibility.VB6.DriveListBoxArray?displayProperty=nameWithType>|Este membro está obsoleto.|
|<xref:Microsoft.VisualBasic.Compatibility.VB6.FileListBox?displayProperty=nameWithType>|Este membro está obsoleto.|
|<xref:Microsoft.VisualBasic.Compatibility.VB6.FileListBoxArray?displayProperty=nameWithType>|Este membro está obsoleto.|
|<xref:Microsoft.VisualBasic.Compatibility.VB6.FixedLengthString?displayProperty=nameWithType>|Este membro está obsoleto.|
|<xref:Microsoft.VisualBasic.Compatibility.VB6.FontDialogArray?displayProperty=nameWithType>|Este membro está obsoleto.|
|<xref:Microsoft.VisualBasic.Compatibility.VB6.FormShowConstants?displayProperty=nameWithType>|Este membro está obsoleto.|
|<xref:Microsoft.VisualBasic.Compatibility.VB6.GroupBoxArray?displayProperty=nameWithType>|Este membro está obsoleto.|
|<xref:Microsoft.VisualBasic.Compatibility.VB6.HScrollBarArray?displayProperty=nameWithType>|Este membro está obsoleto.|
|<xref:Microsoft.VisualBasic.Compatibility.VB6.ImageListArray?displayProperty=nameWithType>|Este membro está obsoleto.|
|<xref:Microsoft.VisualBasic.Compatibility.VB6.LabelArray?displayProperty=nameWithType>|Este membro está obsoleto.|
|<xref:Microsoft.VisualBasic.Compatibility.VB6.ListBoxArray?displayProperty=nameWithType>|Este membro está obsoleto.|
|<xref:Microsoft.VisualBasic.Compatibility.VB6.ListBoxItem?displayProperty=nameWithType>|Este membro está obsoleto.|
|<xref:Microsoft.VisualBasic.Compatibility.VB6.ListViewArray?displayProperty=nameWithType>|Este membro está obsoleto.|
|<xref:Microsoft.VisualBasic.Compatibility.VB6.LoadResConstants?displayProperty=nameWithType>|Este membro está obsoleto.|
|<xref:Microsoft.VisualBasic.Compatibility.VB6.MaskedTextBoxArray?displayProperty=nameWithType>|Este membro está obsoleto.|
|<xref:Microsoft.VisualBasic.Compatibility.VB6.MenuItemArray?displayProperty=nameWithType>|Este membro está obsoleto.|
|<xref:Microsoft.VisualBasic.Compatibility.VB6.MouseButtonConstants?displayProperty=nameWithType>|Este membro está obsoleto.|
|<xref:Microsoft.VisualBasic.Compatibility.VB6.OpenFileDialogArray?displayProperty=nameWithType>|Este membro está obsoleto.|
|<xref:Microsoft.VisualBasic.Compatibility.VB6.PanelArray?displayProperty=nameWithType>|Este membro está obsoleto.|
|<xref:Microsoft.VisualBasic.Compatibility.VB6.PictureBoxArray?displayProperty=nameWithType>|Este membro está obsoleto.|
|<xref:Microsoft.VisualBasic.Compatibility.VB6.PrintDialogArray?displayProperty=nameWithType>|Este membro está obsoleto.|
|<xref:Microsoft.VisualBasic.Compatibility.VB6.ProgressBarArray?displayProperty=nameWithType>|Este membro está obsoleto.|
|<xref:Microsoft.VisualBasic.Compatibility.VB6.RadioButtonArray?displayProperty=nameWithType>|Este membro está obsoleto.|
|<xref:Microsoft.VisualBasic.Compatibility.VB6.RichTextBoxArray?displayProperty=nameWithType>|Este membro está obsoleto.|
|<xref:Microsoft.VisualBasic.Compatibility.VB6.SaveFileDialogArray?displayProperty=nameWithType>|Este membro está obsoleto.|
|<xref:Microsoft.VisualBasic.Compatibility.VB6.ScaleMode?displayProperty=nameWithType>|Este membro está obsoleto.|
|<xref:Microsoft.VisualBasic.Compatibility.VB6.ShiftConstants?displayProperty=nameWithType>|Este membro está obsoleto.|
|<xref:Microsoft.VisualBasic.Compatibility.VB6.StatusBarArray?displayProperty=nameWithType>|Este membro está obsoleto.|
|<xref:Microsoft.VisualBasic.Compatibility.VB6.StatusStripArray?displayProperty=nameWithType>|Este membro está obsoleto.|
|<xref:Microsoft.VisualBasic.Compatibility.VB6.Support?displayProperty=nameWithType>|Este membro está obsoleto.|
|<xref:Microsoft.VisualBasic.Compatibility.VB6.TabControlArray?displayProperty=nameWithType>|Este membro está obsoleto.|
|<xref:Microsoft.VisualBasic.Compatibility.VB6.TextBoxArray?displayProperty=nameWithType>|Este membro está obsoleto.|
|<xref:Microsoft.VisualBasic.Compatibility.VB6.TimerArray?displayProperty=nameWithType>|Este membro está obsoleto.|
|<xref:Microsoft.VisualBasic.Compatibility.VB6.ToolBarArray?displayProperty=nameWithType>|Este membro está obsoleto.|
|<xref:Microsoft.VisualBasic.Compatibility.VB6.ToolStripArray?displayProperty=nameWithType>|Este membro está obsoleto.|
|<xref:Microsoft.VisualBasic.Compatibility.VB6.ToolStripMenuItemArray?displayProperty=nameWithType>|Este membro está obsoleto.|
|<xref:Microsoft.VisualBasic.Compatibility.VB6.TreeViewArray?displayProperty=nameWithType>|Este membro está obsoleto.|
|<xref:Microsoft.VisualBasic.Compatibility.VB6.VScrollBarArray?displayProperty=nameWithType>|Este membro está obsoleto.|
|<xref:Microsoft.VisualBasic.Compatibility.VB6.WebBrowserArray?displayProperty=nameWithType>|Este membro está obsoleto.|
|<xref:Microsoft.VisualBasic.Compatibility.VB6.WebClass?displayProperty=nameWithType>|Este membro está obsoleto.|
|<xref:Microsoft.VisualBasic.Compatibility.VB6.WebClassContainingClassNotOptional?displayProperty=nameWithType>|Este membro está obsoleto.|
|<xref:Microsoft.VisualBasic.Compatibility.VB6.WebClassCouldNotFindEvent?displayProperty=nameWithType>|Este membro está obsoleto.|
|<xref:Microsoft.VisualBasic.Compatibility.VB6.WebClassNextItemCannotBeCurrentWebItem?displayProperty=nameWithType>|Este membro está obsoleto.|
|<xref:Microsoft.VisualBasic.Compatibility.VB6.WebClassNextItemRespondNotFound?displayProperty=nameWithType>|Este membro está obsoleto.|
|<xref:Microsoft.VisualBasic.Compatibility.VB6.WebClassUserWebClassNameNotOptional?displayProperty=nameWithType>|Este membro está obsoleto.|
|<xref:Microsoft.VisualBasic.Compatibility.VB6.WebClassWebClassFileNameNotOptional?displayProperty=nameWithType>|Este membro está obsoleto.|
|<xref:Microsoft.VisualBasic.Compatibility.VB6.WebClassWebItemNotValid?displayProperty=nameWithType>|Este membro está obsoleto.|
|<xref:Microsoft.VisualBasic.Compatibility.VB6.WebItem?displayProperty=nameWithType>|Este membro está obsoleto.|
|<xref:Microsoft.VisualBasic.Compatibility.VB6.WebItemAssociatedWebClassNotOptional?displayProperty=nameWithType>|Este membro está obsoleto.|
|<xref:Microsoft.VisualBasic.Compatibility.VB6.WebItemClosingTagNotFound?displayProperty=nameWithType>|Este membro está obsoleto.|
|<xref:Microsoft.VisualBasic.Compatibility.VB6.WebItemCouldNotLoadEmbeddedResource?displayProperty=nameWithType>|Este membro está obsoleto.|
|<xref:Microsoft.VisualBasic.Compatibility.VB6.WebItemCouldNotLoadTemplateFile?displayProperty=nameWithType>|Este membro está obsoleto.|
|<xref:Microsoft.VisualBasic.Compatibility.VB6.WebItemNameNotOptional?displayProperty=nameWithType>|Este membro está obsoleto.|
|<xref:Microsoft.VisualBasic.Compatibility.VB6.WebItemNoTemplateSpecified?displayProperty=nameWithType>|Este membro está obsoleto.|
|<xref:Microsoft.VisualBasic.Compatibility.VB6.WebItemTooManyNestedTags?displayProperty=nameWithType>|Este membro está obsoleto.|
|<xref:Microsoft.VisualBasic.Compatibility.VB6.WebItemUnexpectedErrorReadingTemplateFile?displayProperty=nameWithType>|Este membro está obsoleto.|
|<xref:Microsoft.VisualBasic.Compatibility.VB6.ZOrderConstants?displayProperty=nameWithType>|Este membro está obsoleto.|

[Voltar ao início](#introduction)

<a name="VBCompatData"></a>

### <a name="assembly-microsoftvisualbasiccompatibilitydatadll"></a>Assembly: Microsoft.VisualBasic.Compatibility.Data.dll

|Tipo|Mensagem|
|----------|-------------|
|<xref:Microsoft.VisualBasic.Compatibility.VB6.ADODC?displayProperty=nameWithType>|Este membro está obsoleto.|
|<xref:Microsoft.VisualBasic.Compatibility.VB6.ADODC.BOFActionEnum?displayProperty=nameWithType>|Este membro está obsoleto.|
|<xref:Microsoft.VisualBasic.Compatibility.VB6.ADODC.EndOfRecordsetDelegate?displayProperty=nameWithType>|Este membro está obsoleto.|
|<xref:Microsoft.VisualBasic.Compatibility.VB6.ADODC.EOFActionEnum?displayProperty=nameWithType>|Este membro está obsoleto.|
|<xref:Microsoft.VisualBasic.Compatibility.VB6.ADODC.ErrorDelegate?displayProperty=nameWithType>|Este membro está obsoleto.|
|<xref:Microsoft.VisualBasic.Compatibility.VB6.ADODC.FetchCompleteDelegate?displayProperty=nameWithType>|Este membro está obsoleto.|
|<xref:Microsoft.VisualBasic.Compatibility.VB6.ADODC.FetchProgressDelegate?displayProperty=nameWithType>|Este membro está obsoleto.|
|<xref:Microsoft.VisualBasic.Compatibility.VB6.ADODC.FieldChangeCompleteDelegate?displayProperty=nameWithType>|Este membro está obsoleto.|
|<xref:Microsoft.VisualBasic.Compatibility.VB6.ADODC.MoveCompleteDelegate?displayProperty=nameWithType>|Este membro está obsoleto.|
|<xref:Microsoft.VisualBasic.Compatibility.VB6.ADODC.OrientationEnum?displayProperty=nameWithType>|Este membro está obsoleto.|
|<xref:Microsoft.VisualBasic.Compatibility.VB6.ADODC.RecordChangeCompleteDelegate?displayProperty=nameWithType>|Este membro está obsoleto.|
|<xref:Microsoft.VisualBasic.Compatibility.VB6.ADODC.RecordsetChangeCompleteDelegate?displayProperty=nameWithType>|Este membro está obsoleto.|
|<xref:Microsoft.VisualBasic.Compatibility.VB6.ADODC.WillChangeFieldDelegate?displayProperty=nameWithType>|Este membro está obsoleto.|
|<xref:Microsoft.VisualBasic.Compatibility.VB6.ADODC.WillChangeRecordDelegate?displayProperty=nameWithType>|Este membro está obsoleto.|
|<xref:Microsoft.VisualBasic.Compatibility.VB6.ADODC.WillChangeRecordsetDelegate?displayProperty=nameWithType>|Este membro está obsoleto.|
|<xref:Microsoft.VisualBasic.Compatibility.VB6.ADODC.WillMoveDelegate?displayProperty=nameWithType>|Este membro está obsoleto.|
|<xref:Microsoft.VisualBasic.Compatibility.VB6.ADODCArray?displayProperty=nameWithType>|Este membro está obsoleto.|
|<xref:Microsoft.VisualBasic.Compatibility.VB6.BaseDataEnvironment?displayProperty=nameWithType>|Este membro está obsoleto.|
|<xref:Microsoft.VisualBasic.Compatibility.VB6.BindingCollectionEnumerator?displayProperty=nameWithType>|Este membro está obsoleto.|
|<xref:Microsoft.VisualBasic.Compatibility.VB6.CONNECTDATA?displayProperty=nameWithType>|Este membro está obsoleto.|
|<xref:Microsoft.VisualBasic.Compatibility.VB6.DBBINDING?displayProperty=nameWithType>|Este membro está obsoleto.|
|<xref:Microsoft.VisualBasic.Compatibility.VB6.DBCOLUMNINFO?displayProperty=nameWithType>|Este membro está obsoleto.|
|<xref:Microsoft.VisualBasic.Compatibility.VB6.DBID?displayProperty=nameWithType>|Este membro está obsoleto.|
|<xref:Microsoft.VisualBasic.Compatibility.VB6.DBinding?displayProperty=nameWithType>|Este membro está obsoleto.|
|<xref:Microsoft.VisualBasic.Compatibility.VB6.DBindingCollection?displayProperty=nameWithType>|Este membro está obsoleto.|
|<xref:Microsoft.VisualBasic.Compatibility.VB6.DBKINDENUM?displayProperty=nameWithType>|Este membro está obsoleto.|
|<xref:Microsoft.VisualBasic.Compatibility.VB6.DBPROPIDSET?displayProperty=nameWithType>|Este membro está obsoleto.|
|<xref:Microsoft.VisualBasic.Compatibility.VB6.IAccessor?displayProperty=nameWithType>|Este membro está obsoleto.|
|<xref:Microsoft.VisualBasic.Compatibility.VB6.IChapteredRowset?displayProperty=nameWithType>|Este membro está obsoleto.|
|<xref:Microsoft.VisualBasic.Compatibility.VB6.IColumnsInfo?displayProperty=nameWithType>|Este membro está obsoleto.|
|<xref:Microsoft.VisualBasic.Compatibility.VB6.IConnectionPoint?displayProperty=nameWithType>|Este membro está obsoleto.|
|<xref:Microsoft.VisualBasic.Compatibility.VB6.IConnectionPointContainer?displayProperty=nameWithType>|Este membro está obsoleto.|
|<xref:Microsoft.VisualBasic.Compatibility.VB6.IDataFormat?displayProperty=nameWithType>|Este membro está obsoleto.|
|<xref:Microsoft.VisualBasic.Compatibility.VB6.IDataFormatDisp?displayProperty=nameWithType>|Este membro está obsoleto.|
|<xref:Microsoft.VisualBasic.Compatibility.VB6.IEnumConnectionPoints?displayProperty=nameWithType>|Este membro está obsoleto.|
|<xref:Microsoft.VisualBasic.Compatibility.VB6.IEnumConnections?displayProperty=nameWithType>|Este membro está obsoleto.|
|<xref:Microsoft.VisualBasic.Compatibility.VB6.IRowPosition?displayProperty=nameWithType>|Este membro está obsoleto.|
|<xref:Microsoft.VisualBasic.Compatibility.VB6.IRowPositionChange?displayProperty=nameWithType>|Este membro está obsoleto.|
|<xref:Microsoft.VisualBasic.Compatibility.VB6.IRowset?displayProperty=nameWithType>|Este membro está obsoleto.|
|<xref:Microsoft.VisualBasic.Compatibility.VB6.IRowsetChange?displayProperty=nameWithType>|Este membro está obsoleto.|
|<xref:Microsoft.VisualBasic.Compatibility.VB6.IRowsetIdentity?displayProperty=nameWithType>|Este membro está obsoleto.|
|<xref:Microsoft.VisualBasic.Compatibility.VB6.IRowsetInfo?displayProperty=nameWithType>|Este membro está obsoleto.|
|<xref:Microsoft.VisualBasic.Compatibility.VB6.IRowsetNotify?displayProperty=nameWithType>|Este membro está obsoleto.|
|<xref:Microsoft.VisualBasic.Compatibility.VB6.MBinding?displayProperty=nameWithType>|Este membro está obsoleto.|
|<xref:Microsoft.VisualBasic.Compatibility.VB6.MBindingCollection?displayProperty=nameWithType>|Este membro está obsoleto.|
|<xref:Microsoft.VisualBasic.Compatibility.VB6.SRDescriptionAttribute?displayProperty=nameWithType>|Este membro está obsoleto.|
|<xref:Microsoft.VisualBasic.Compatibility.VB6.UGUID?displayProperty=nameWithType>|Este membro está obsoleto.|
|<xref:Microsoft.VisualBasic.Compatibility.VB6.UNAME?displayProperty=nameWithType>|Este membro está obsoleto.|
|<xref:Microsoft.VisualBasic.Compatibility.VB6.UpdateMode?displayProperty=nameWithType>|Este membro está obsoleto.|

[Voltar ao início](#introduction)

<a name="visualc"></a>

### <a name="assembly-microsoftvisualcdll"></a>Assembly: Microsoft.VisualC.dll

|Tipo|Mensagem|
|----------|-------------|
|<xref:Microsoft.VisualC.DebugInfoInPDBAttribute?displayProperty=nameWithType>|Microsoft.VisualC.dll é um assembly obsoleto e existe somente para compatibilidade com versões anteriores.|
|<xref:Microsoft.VisualC.DecoratedNameAttribute?displayProperty=nameWithType>|Microsoft.VisualC.dll é um assembly obsoleto e existe somente para compatibilidade com versões anteriores.|
|<xref:Microsoft.VisualC.IsConstModifier?displayProperty=nameWithType>|Microsoft.VisualC.dll é um assembly obsoleto e existe somente para compatibilidade com versões anteriores.|
|<xref:Microsoft.VisualC.IsCXXReferenceModifier?displayProperty=nameWithType>|Microsoft.VisualC.dll é um assembly obsoleto e existe somente para compatibilidade com versões anteriores.|
|<xref:Microsoft.VisualC.IsLongModifier?displayProperty=nameWithType>|Microsoft.VisualC.dll é um assembly obsoleto e existe somente para compatibilidade com versões anteriores.|
|<xref:Microsoft.VisualC.IsSignedModifier?displayProperty=nameWithType>|Microsoft.VisualC.dll é um assembly obsoleto e existe somente para compatibilidade com versões anteriores.|
|<xref:Microsoft.VisualC.IsVolatileModifier?displayProperty=nameWithType>|Microsoft.VisualC.dll é um assembly obsoleto e existe somente para compatibilidade com versões anteriores.|
|<xref:Microsoft.VisualC.MiscellaneousBitsAttribute?displayProperty=nameWithType>|Microsoft.VisualC.dll é um assembly obsoleto e existe somente para compatibilidade com versões anteriores.|
|<xref:Microsoft.VisualC.NeedsCopyConstructorModifier?displayProperty=nameWithType>|Microsoft.VisualC.dll é um assembly obsoleto e existe somente para compatibilidade com versões anteriores.|
|<xref:Microsoft.VisualC.NoSignSpecifiedModifier?displayProperty=nameWithType>|Microsoft.VisualC.dll é um assembly obsoleto e existe somente para compatibilidade com versões anteriores.|

## <a name="see-also"></a>Veja também

- [O que está obsoleto na biblioteca de classes](whats-obsolete.md)
- [Membros obsoletos](obsolete-members.md)
