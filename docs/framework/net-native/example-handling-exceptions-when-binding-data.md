---
title: 'Exemplo: lidar com exceções ao associar dados'
ms.date: 03/30/2017
ms.assetid: bd63ed96-9853-46dc-ade5-7bd1b0f39110
ms.openlocfilehash: b774d1bce4f4d1c03258ed44b27d3871e7c5275f
ms.sourcegitcommit: b16c00371ea06398859ecd157defc81301c9070f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/06/2020
ms.locfileid: "79181030"
---
# <a name="example-handling-exceptions-when-binding-data"></a>Exemplo: lidar com exceções ao associar dados
> [!NOTE]
> Este tópico refere-se ao Developer Preview do .NET Nativo, que é um software em pré-lançamento. Você pode baixar a versão prévia do [site Microsoft Connect](https://go.microsoft.com/fwlink/?LinkId=394611) (registro obrigatório).  
  
 O exemplo a seguir mostra como resolver uma exceção [MissingMetadataException](missingmetadataexception-class-net-native.md) que é gerada quando um aplicativo compilado com a cadeia de ferramentas .net Native tenta associar dados. Aqui estão as informações da exceção:  
  
```output
This operation cannot be carried out as metadata for the following type was removed for performance reasons:
App.ViewModels.MainPageVM  
```  
  
 Esta é a pilha de chamadas associadas:  
  
```output
Reflection::Execution::ReflectionDomainSetupImplementation.CreateNonInvokabilityException+0x238  
Reflection::Core::ReflectionDomain.CreateNonInvokabilityException+0x2e  
Reflection::Core::Execution::ExecutionEnvironment.+0x316  
System::Reflection::Runtime::PropertyInfos::RuntimePropertyInfo.GetValue+0x1cb  
System::Reflection::PropertyInfo.GetValue+0x22  
System::Runtime::InteropServices::WindowsRuntime::CustomPropertyImpl.GetValue+0x42  
App!$66_Interop::McgNative.Func_IInspectable_IInspectable+0x158  
App!$66_Interop::McgNative::__vtable_Windows_UI_Xaml_Data__ICustomProperty.GetValue__STUB+0x46  
Windows_UI_Xaml!DirectUI::PropertyProviderPropertyAccess::GetValue+0x3f
Windows_UI_Xaml!DirectUI::PropertyAccessPathStep::GetValue+0x31
Windows_UI_Xaml!DirectUI::PropertyPathListener::ConnectPathStep+0x113  
```  
  
## <a name="what-was-the-app-doing"></a>O que o aplicativo estava fazendo?  
 Na base da pilha, os quadros do <xref:Windows.UI.Xaml?displayProperty=nameWithType> namespace indicam que o mecanismo de RENDERIZAÇÃO XAML estava em execução.   O uso do método <xref:System.Reflection.PropertyInfo.GetValue%2A?displayProperty=nameWithType> indica uma pesquisa de reflexão do valor da propriedade no tipo cujos metadados foi removido.  
  
 A primeira etapa no fornecimento de uma diretiva de metadados seria adicionar metadados `serialize` ao tipo de forma que suas propriedades estejam acessíveis:  
  
```xml  
<Type Name="App.ViewModels.MainPageVM" Serialize="Required Public" />  
```  
  
## <a name="is-this-an-isolated-case"></a>Esta é uma ocorrência isolada?  
 Neste cenário, se a associação dos dados possuem metadados incompletos para um `ViewModel`, isso também pode ocorrer para outros.  Se o código é estruturado de forma que os modelos de exibição do aplicativo estão todos no namespace `App.ViewModels`, você pode usar uma diretiva de runtime mais geral:  
  
```xml  
<Namespace Name="App.ViewModels " Serialize="Required Public" />  
```  
  
## <a name="could-the-code-be-rewritten-to-not-use-reflection"></a>O código pode ser reconfigurado para não usar reflexão?  
 Como associação de dados possui reflexão intensa, alterar o código para evitar reflexão não é viável.  
  
 No entanto, existem maneiras para especificar o `ViewModel` para a página XAML para que a cadeia de ferramentas possa associar propriedades de vinculação com o tipo correto no tempo de compilação e manter os metadados sem usar uma diretiva de runtime.  Por exemplo, você pode aplicar o <xref:Windows.UI.Xaml.Data.BindableAttribute?displayProperty=nameWithType> atributo em Propriedades. Isso faz com que o compilador XAML gere informações de pesquisa necessárias e evita que necessitem de uma diretiva de runtime no arquivo Default.rd.xml.  
  
## <a name="see-also"></a>Confira também

- [Introdução](getting-started-with-net-native.md)
- [Exemplo: solução de problemas de programação dinâmica](example-troubleshooting-dynamic-programming.md)
