---
title: Aplicando atributos de interoperabilidade
description: Este artigo resume os atributos de interoperabilidade COM do namespace System. Runtime. InteropServices, incluindo atributos de tempo de design e de ferramenta de conversão.
ms.date: 03/30/2017
helpviewer_keywords:
- design-time attributes
- .NET Framework, exposing components to COM
- attributes [.NET Framework], design-time functionality
- conversion-tool attributes
- attributes [.NET Framework], interop-specific
- attributes [.NET Framework], conversion-tool
- interoperation with unmanaged code, applying attributes
- interoperation with unmanaged code, exposing .NET Framework components
- COM interop, exposing COM components
- COM interop, applying attributes
ms.assetid: b6014613-641c-4912-9e2f-83a99210a037
ms.openlocfilehash: f9ccf59e52c1ef27649cd70a57f7b24bb5a8e9bf
ms.sourcegitcommit: 33deec3e814238fb18a49b2a7e89278e27888291
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/02/2020
ms.locfileid: "84291325"
---
# <a name="applying-interop-attributes"></a>Aplicando atributos de interoperabilidade
O namespace <xref:System.Runtime.InteropServices> fornece três categorias de atributos específicos à interoperabilidade: aquelas aplicadas por você em tempo de design, aquelas aplicadas pelas ferramentas de interoperabilidade COM e as APIs durante o processo de conversão e aquelas aplicadas por você ou pela interoperabilidade COM.  
  
 Se você não estiver familiarizado com a tarefa de aplicação de atributos ao código gerenciado, consulte [Estendendo metadados usando atributos](../attributes/index.md). Assim como outros atributos personalizados, você pode aplicar atributos específicos à interoperabilidade a tipos, métodos, propriedades, parâmetros, campos e outros membros.  
  
## <a name="design-time-attributes"></a>Atributos em tempo de design  
 Ajuste o resultado do processo de conversão executado pelas APIs e pelas ferramentas de interoperabilidade COM usando atributos em tempo de design. A tabela a seguir descreve os atributos que podem ser aplicados ao código-fonte gerenciado. Às vezes, as ferramentas de interoperabilidade COM também podem aplicar os atributos descritos nesta tabela.  
  
|Atributo|Descrição|  
|---------------|-----------------|  
|<xref:System.Runtime.InteropServices.AutomationProxyAttribute>|Especifica se o tipo deve ter o marshaling realizado usando o marshaler de Automação ou um proxy e stub personalizados.|  
|<xref:System.Runtime.InteropServices.ClassInterfaceAttribute>|Controla o tipo de interface gerado para uma classe.|  
|<xref:System.Runtime.InteropServices.CoClassAttribute>|Identifica o CLSID da coclass original importado de uma biblioteca de tipos.<br /><br /> As ferramentas de interoperabilidade COM geralmente aplicam esse atributo.|  
|<xref:System.Runtime.InteropServices.ComImportAttribute>|Indica que uma definição de coclass ou interface foi importada de uma biblioteca de tipos COM. O runtime usa esse sinalizador para saber como ativar e realizar marshaling do tipo. Esse atributo proíbe o tipo que está sendo exportado novamente para uma biblioteca de tipos.<br /><br /> As ferramentas de interoperabilidade COM geralmente aplicam esse atributo.|  
|<xref:System.Runtime.InteropServices.ComRegisterFunctionAttribute>|Indica que um método deve ser chamado quando o assembly é registrado para uso por meio do COM, de modo que o código escrito pelo usuário possa ser executado durante o processo de registro.|  
|<xref:System.Runtime.InteropServices.ComSourceInterfacesAttribute>|Identifica as interfaces que são fontes de eventos para a classe.<br /><br /> Ferramentas de interoperabilidade COM podem aplicar esse atributo.|  
|<xref:System.Runtime.InteropServices.ComUnregisterFunctionAttribute>|Indica que um método deve ser chamado quando o assembly tem o registro cancelado por meio do COM, de modo que o código escrito pelo usuário possa ser executado durante o processo.|  
|<xref:System.Runtime.InteropServices.ComVisibleAttribute>|Renderiza tipos invisíveis para o COM quando o valor do atributo é igual a **false**. Esse atributo pode ser aplicado a um tipo individual ou a um assembly inteiro para controlar a visibilidade COM. Por padrão, todos os tipos gerenciados e públicos são visíveis; o atributo não é necessário para torná-los visíveis.|  
|<xref:System.Runtime.InteropServices.DispIdAttribute>|Especifica o DISPID (identificador de expedição) COM de um método ou campo. Este atributo contém o DISPID do método, do campo ou da propriedade que ele descreve.<br /><br /> Ferramentas de interoperabilidade COM podem aplicar esse atributo.|
|<xref:System.Runtime.InteropServices.ComDefaultInterfaceAttribute>|Indica a interface padrão para uma classe COM implementada no .NET.<br /><br /> Ferramentas de interoperabilidade COM podem aplicar esse atributo.|
|<xref:System.Runtime.InteropServices.FieldOffsetAttribute>|Indica a posição física de cada campo dentro de uma classe quando usado com o **StructLayoutAttribute** e o **LayoutKind** é definido como Explicit.|  
|<xref:System.Runtime.InteropServices.GuidAttribute>|Especifica o GUID (identificador global exclusivo) de uma classe, uma interface ou toda uma biblioteca de tipos. A cadeia de caracteres passada para o atributo deve ter um formato que seja um argumento de construtor aceitável para o tipo **System.Guid**.<br /><br /> Ferramentas de interoperabilidade COM podem aplicar esse atributo.|  
|<xref:System.Runtime.InteropServices.IDispatchImplAttribute>|Indica qual implementação da interface **IDispatch** o Common Language Runtime usa ao expor interfaces duplas e dispinterfaces ao COM.|  
|<xref:System.Runtime.InteropServices.InAttribute>|Indica se os dados devem ter o marshaling realizado para o chamador. Pode ser usado para parâmetros de atributo.|  
|<xref:System.Runtime.InteropServices.InterfaceTypeAttribute>|Controla como uma interface gerenciada é exposta aos clientes COM (Dupla, derivada de IUnknown ou somente IDispatch).<br /><br /> Ferramentas de interoperabilidade COM podem aplicar esse atributo.|  
|<xref:System.Runtime.InteropServices.LCIDConversionAttribute>|Indica que uma assinatura de método não gerenciado espera um parâmetro LCID.<br /><br /> Ferramentas de interoperabilidade COM podem aplicar esse atributo.|  
|<xref:System.Runtime.InteropServices.MarshalAsAttribute>|Indica como os dados em campos ou parâmetros devem ter o marshaling realizado entre o código gerenciado e não gerenciado. O atributo sempre é opcional, porque cada tipo de dados tem um comportamento de marshaling padrão.<br /><br /> Ferramentas de interoperabilidade COM podem aplicar esse atributo.|  
|<xref:System.Runtime.InteropServices.OptionalAttribute>|Indica que um parâmetro é opcional.<br /><br /> Ferramentas de interoperabilidade COM podem aplicar esse atributo.|  
|<xref:System.Runtime.InteropServices.OutAttribute>|Indica se os dados em um campo ou parâmetro devem ter o marshaling realizado de um objeto chamado novamente para seu chamador.|  
|<xref:System.Runtime.InteropServices.PreserveSigAttribute>|Suprime a transformação de assinatura HRESULT ou retval que normalmente ocorre durante chamadas de interoperação. O atributo afeta o marshaling, bem como a exportação da biblioteca de tipos.<br /><br /> Ferramentas de interoperabilidade COM podem aplicar esse atributo.|  
|<xref:System.Runtime.InteropServices.ProgIdAttribute>|Especifica o ProgID de uma classe do .NET Framework. Pode ser usado para classes de atributos.|  
|<xref:System.Runtime.InteropServices.StructLayoutAttribute>|Controla o layout físico dos campos de uma classe.<br /><br /> Ferramentas de interoperabilidade COM podem aplicar esse atributo.|  
  
## <a name="conversion-tool-attributes"></a>Atributos da ferramenta de conversão  
 A tabela a seguir descreve os atributos que as ferramentas de interoperabilidade COM aplicam durante o processo de conversão. Esses atributos não são aplicados em tempo de design.  
  
|Atributo|Descrição|  
|---------------|-----------------|  
|<xref:System.Runtime.InteropServices.ComAliasNameAttribute>|Indica o alias COM de um parâmetro ou tipo de campo. Pode ser usado para parâmetros de atributo, campos ou valores retornados.|  
|<xref:System.Runtime.InteropServices.ComConversionLossAttribute>|Indica que as informações sobre uma classe ou interface foram perdidas quando foram importadas de uma biblioteca de tipos para um assembly.|  
|<xref:System.Runtime.InteropServices.ComEventInterfaceAttribute>|Identifica a interface de origem e a classe que implementa os métodos da interface do evento.|  
|<xref:System.Runtime.InteropServices.ImportedFromTypeLibAttribute>|Indica que o assembly foi originalmente importado de uma biblioteca de tipos COM. Este atributo contém a definição de biblioteca de tipos da biblioteca de tipos original.|  
|<xref:System.Runtime.InteropServices.TypeLibFuncAttribute>|Contém o **FUNCFLAGS** que foi originalmente importado para essa função da biblioteca de tipos COM.|  
|<xref:System.Runtime.InteropServices.TypeLibTypeAttribute>|Contém o **TYPEFLAGS** que foi originalmente importado para esse tipo da biblioteca de tipos COM.|  
|<xref:System.Runtime.InteropServices.TypeLibVarAttribute>|Contém o **VARFLAGS** que foi originalmente importado para essa variável da biblioteca de tipos COM.|  
  
## <a name="see-also"></a>Veja também

- <xref:System.Runtime.InteropServices>
- [Expondo componentes do .NET Framework para COM](../../framework/interop/exposing-dotnet-components-to-com.md)
- [Atributos](../attributes/index.md)
- [Qualificando tipos do .NET para interoperação](qualify-net-types-for-interoperation.md)
- [Como empacotar um assembly .NET Framework para o COM](../../framework/interop/packaging-an-assembly-for-com.md)
