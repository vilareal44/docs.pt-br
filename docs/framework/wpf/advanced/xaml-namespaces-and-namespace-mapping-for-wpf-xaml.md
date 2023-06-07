---
title: Namespaces XAML e mapeamento de namespace
description: Saiba mais sobre a presença e a finalidade dos dois mapeamentos de namespace XAML geralmente encontrados na marca raiz de um arquivo XAML Windows Presentation Foundation.
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
helpviewer_keywords:
- custom classes [WPF], mapping namespaces to
- XAML [WPF], namespaces
- namespace mapping [WPF]
- assemblies [WPF], mapping namespaces to
- mapping namespaces [WPF]
- XAML [WPF], namespace mapping
- classes [WPF], mapping namespaces to
- namespaces [WPF]
ms.assetid: 5c0854e3-7470-435d-9fe2-93eec9d3634e
ms.openlocfilehash: 42808df8e7483f60b1420fda890fe374493538f1
ms.sourcegitcommit: 87cfeb69226fef01acb17c56c86f978f4f4a13db
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/24/2020
ms.locfileid: "87168369"
---
# <a name="xaml-namespaces-and-namespace-mapping-for-wpf-xaml"></a>Namespaces XAML e mapeamento de namespace para XAML WPF
Este tópico oferece explicação adicional sobre a presença e a finalidade dos dois mapeamentos de namespace de XAML como frequentemente encontrados na marca raiz de um arquivo XAML do WPF. Ele também descreve como gerar mapeamentos semelhantes para usar elementos que são definidos em seu próprio código e/ou em assemblies separados.  

## <a name="what-is-a-xaml-namespace"></a>O que é um Namespace de XAML?  
 Um namespace de XAML é, na verdade, uma extensão do conceito de um namespace de XML. As técnicas para especificar um namespace de XAML contam com a sintaxe do namespace de XML, a convenção do uso de URIs como identificadores de namespace, o uso de prefixos para fornecer um meio para referenciar vários namespaces com base na mesma fonte de marcação e assim por diante. O conceito principal que é adicionado à definição de XAML do namespace de XML é que um namespace de XAML implica tanto em um escopo de exclusividade para os usos de marcação como também influencia em como as entidades de marcação potencialmente contam com o suporte de namespaces específicos e assemblies referenciados do CLR. Essa última consideração também é influenciada pelo conceito de um contexto de esquema XAML. Mas, para fins de como o WPF funciona com namespaces de XAML, você geralmente pode pensar nos namespaces de XAML em termos de um namespace de XAML padrão, o namespace de linguagem XAML e quaisquer outros namespaces de XAML como mapeados pela sua marcação de XAML diretamente para namespaces e assemblies referenciados específicos com suporte do CLR.  
  
<a name="The_WPF_and_XAML_Namespace_Declarations"></a>
## <a name="the-wpf-and-xaml-namespace-declarations"></a>As declarações de namespace de WPF e XAML  
 Dentro das declarações de namespace na marca raiz de muitos arquivos XAML, você verá que geralmente há duas declarações de namespace de XML. A primeira declaração mapeia o namespace de XAML de cliente/estrutura geral do WPF como o padrão:  
  
 `xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"`  
  
 A segunda declaração mapeia um namespace de XAML separado, mapeando-o (tipicamente) para o prefixo `x:`.  
  
 `xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"`  
  
 A relação entre essas declarações é que o mapeamento de prefixo `x:` dá suporte aos intrínsecos que fazem parte da definição de linguagem XAML e o [!INCLUDE[TLA2#tla_winclient](../../../../includes/tla2sharptla-winclient-md.md)] é uma implementação que usa XAML como uma linguagem e define um vocabulário de seus objetos para XAML. Como os usos do vocabulário do WPF serão muito mais comuns que os usos de intrínsecos do XAML, o vocabulário do WPF é mapeado como o padrão.  
  
 A `x:` Convenção de prefixo para mapear o suporte de intrínsecos da linguagem XAML é seguida por modelos de projeto, código de exemplo e a documentação dos recursos de linguagem nesse SDK. O namespace de XAML define vários recursos comumente usados que são necessários até mesmo para aplicativos básicos do WPF. Por exemplo, para unir qualquer code-behind a um arquivo XAML por meio de uma classe parcial, você deve nomear essa classe como o atributo `x:Class` no elemento raiz do arquivo XAML relevante. Ou, qualquer elemento, conforme definido em uma página XAML que você deseja acessar como um recurso com chave, deve ter o atributo `x:Key` definido no elemento em questão. Para obter mais informações sobre esses e outros aspectos do XAML, consulte [Visão geral de XAML (WPF)](../../../desktop-wpf/fundamentals/xaml.md) ou [Sintaxe de XAML em detalhes](xaml-syntax-in-detail.md).  
  
<a name="Mapping_To_Custom_Classes_and_Assemblies"></a>
## <a name="mapping-to-custom-classes-and-assemblies"></a>Mapeando para assemblies e classes personalizadas  
 Você pode mapear namespaces de XML para assemblies usando uma série de tokens em um prefixo de declaração `xmlns`, de forma semelhante a como os namespaces de XAML padrão do WPF e intrínsecos de XAML são mapeados para prefixos.  
  
 A sintaxe recebe os seguintes possíveis tokens nomeados e os seguintes valores:  
  
 `clr-namespace:` O namespace do CLR declarado dentro do assembly que contém os tipos públicos a serem expostos como elementos.  
  
 `assembly=`O assembly que contém um ou todos os namespaces CLR referenciados. Esse valor é, geralmente, apenas o nome do assembly e não o caminho, e não inclui a extensão (como .dll ou .exe). O caminho para esse assembly deve ser estabelecido como uma referência de projeto no arquivo de projeto que contém o XAML que você está tentando mapear. Para incorporar o controle de versão e a assinatura de nome forte, o `assembly` valor pode ser uma cadeia de caracteres conforme definido por <xref:System.Reflection.AssemblyName> , em vez do nome da cadeia de caracteres simples.  
  
 Observe que o caractere separando o token `clr-namespace` de seu valor é um dois-pontos (:) enquanto que o caractere que separa o token `assembly` de seu valor é um sinal de igual (=). O caractere a ser usado entre esses dois tokens é um ponto e vírgula. Além disso, não inclua nenhum espaço em branco em qualquer lugar na declaração.  
  
### <a name="a-basic-custom-mapping-example"></a>Um exemplo básico de mapeamento personalizado  
 O código a seguir define uma classe personalizada de exemplo:  
  
```csharp  
namespace SDKSample {  
    public class ExampleClass : ContentControl {  
        public ExampleClass() {  
        ...  
        }  
    }  
}  
```  
  
```vb  
Namespace SDKSample  
    Public Class ExampleClass  
        Inherits ContentControl  
         ...  
        Public Sub New()  
        End Sub  
    End Class  
End Namespace  
```  
  
 Essa classe personalizada é compilada em uma biblioteca, que é chamada de `SDKSampleLibrary` por configurações do projeto (não mostradas).  
  
 Para fazer referência a essa classe personalizada, você também precisa incluí-la como uma referência para o projeto atual, o que você normalmente faria usando a interface do usuário do Gerenciador de Soluções no Visual Studio.  
  
 Agora que você tem uma biblioteca que contém uma classe e uma referência a ela nas configurações do projeto, você pode adicionar o seguinte mapeamento de prefixo como parte do seu elemento raiz no XAML:  
  
 `xmlns:custom="clr-namespace:SDKSample;assembly=SDKSampleLibrary"`  
  
 Para reunir tudo isso, abaixo está o XAML que inclui o mapeamento personalizado juntamente com o padrão típico e os mapeamentos de x: na marca raiz e que, em seguida, usa uma referência prefixada para instanciar a `ExampleClass` na interface do usuário:  
  
```xaml  
<Page x:Class="WPFApplication1.MainPage"  
    xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
    xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"  
    xmlns:custom="clr-namespace:SDKSample;assembly=SDKSampleLibrary">  
  ...  
  <custom:ExampleClass/>  
...  
</Page>  
```  
  
### <a name="mapping-to-current-assemblies"></a>Mapeando para assemblies atuais  
 O `assembly` poderá ser omitido se o `clr-namespace` referenciado estiver sendo definido dentro do mesmo assembly como o código do aplicativo que faz referência às classes personalizadas. Ou, uma sintaxe equivalente para este caso é especificar `assembly=`, sem nenhum token de cadeia de caracteres após o sinal de igual.  
  
 As classes personalizadas não podem ser usadas como o elemento raiz de uma página se definidas no mesmo assembly. As classes parciais não precisam ser mapeadas. Somente as classes que não são a classe parcial da página do seu aplicativo precisam ser mapeadas se você pretende referenciá-las como elementos no XAML.  
  
<a name="Mapping_CLR_Namespaces_to_XML_Namespaces_in_an"></a>
## <a name="mapping-clr-namespaces-to-xml-namespaces-in-an-assembly"></a>Mapeando namespaces de CLR para namespaces de XML em um assembly  
 O WPF define um atributo de CLR que é consumido pelos processadores XAML para mapear vários namespaces de CLR para um único namespace de XAML. Esse atributo, <xref:System.Windows.Markup.XmlnsDefinitionAttribute> , é colocado no nível do assembly no código-fonte que produz o assembly. O código-fonte do assembly do WPF usa esse atributo para mapear os vários namespaces comuns, como <xref:System.Windows> e <xref:System.Windows.Controls> , para o `http://schemas.microsoft.com/winfx/2006/xaml/presentation` namespace.  
  
 O <xref:System.Windows.Markup.XmlnsDefinitionAttribute> usa dois parâmetros: o nome do namespace XML/XAML e o nome do namespace CLR. Mais de um <xref:System.Windows.Markup.XmlnsDefinitionAttribute> pode existir para mapear vários namespaces CLR para o mesmo namespace de XML. Depois de mapeados, os membros desses namespaces também podem ser referenciados sem qualificação completa, se desejado, fornecendo a instrução `using` apropriada na página code-behind de classe parcial. Para obter mais detalhes, confira <xref:System.Windows.Markup.XmlnsDefinitionAttribute>.  
  
## <a name="designer-namespaces-and-other-prefixes-from-xaml-templates"></a>Namespaces de designer e outros prefixos de modelos de XAML  
 Se você estiver trabalhando com ambientes de desenvolvimento e/ou com ferramentas de design para XAML do WPF, notará que há outros namespaces/prefixos de XAML definidos na marcação XAML.  
  
 O WPF Designer para Visual Studio usa um namespace de designer que é normalmente mapeado para o prefixo `d:` . Modelos de projeto mais recentes para o WPF podem mapear previamente esse namespace XAML para dar suporte ao intercâmbio do XAML entre o WPF Designer para Visual Studio e outros ambientes de design. Esse namespace de XAML de design é usado para manter o estado de design durante a movimentação da interface do usuário baseada em XAML no designer. Ele também é usado para recursos como `d:IsDataSource`, que habilitam fontes de dados em runtime em um designer.  
  
 Outro prefixo que talvez você veja mapeado é o `mc:`. O `mc:` é para a compatibilidade de marcação e aproveita um padrão de compatibilidade de marcação que não é, necessariamente, específico de XAML. Até certo ponto, os recursos de compatibilidade de marcação podem ser usados para a troca de XAML entre estruturas ou entre outros limites de implementação de suporte, para trabalhar entre contextos de esquema de XAML, para fornecer compatibilidade para modos limitados nos designers e assim por diante. Para obter mais informações sobre os conceitos de compatibilidade de marcação e como eles se relacionam com o WPF, consulte [Recursos de linguagem de compatibilidade de marcação (mc:)](markup-compatibility-mc-language-features.md).  
  
## <a name="wpf-and-assembly-loading"></a>WPF e carregamento de assembly  
 O contexto do esquema XAML para o WPF integra-se com o modelo de aplicativo do WPF, que por sua vez usa o conceito definido pelo CLR de <xref:System.AppDomain> . A sequência a seguir descreve como o contexto do esquema XAML interpreta como carregar assemblies ou localizar tipos em tempo de execução ou em tempo de design, com base no uso do WPF de <xref:System.AppDomain> e outros fatores.  
  
1. Itere pelo <xref:System.AppDomain> , procurando um assembly já carregado que corresponda a todos os aspectos do nome, começando do assembly carregado mais recentemente.  
  
2. Se o nome for qualificado, chame <xref:System.Reflection.Assembly.Load%28System.String%29?displayProperty=nameWithType> o nome qualificado.  
  
3. Se o nome curto + token de chave pública de um nome qualificado corresponder ao assembly do qual a marcação foi carregada, retorne esse assembly.  
  
4. Use o nome curto + token de chave pública para chamar <xref:System.Reflection.Assembly.Load%28System.String%29?displayProperty=nameWithType> .  
  
5. Se o nome for não qualificado, chame <xref:System.Reflection.Assembly.LoadWithPartialName%2A?displayProperty=nameWithType> .  
  
 O XAML livre não usa a etapa 3. Não há nenhum assembly do qual ele foi carregado.  
  
 O XAML compilado para o WPF (gerado via XamlBuildTask) não usa os assemblies já carregados do <xref:System.AppDomain> (etapa 1). Além disso, o nome nunca deve ser não qualificado na saída de XamlBuildTask, portanto, a etapa 5 não se aplica.  
  
 O BAML compilado (gerado por meio de PresentationBuildTask) usa todas as etapas, embora o BAML também não deva conter nomes de assembly não qualificados.  
  
## <a name="see-also"></a>Confira também

- [Noções básicas sobre namespaces de XML](https://docs.microsoft.com/previous-versions/aa468565(v=msdn.10))
- [Visão geral de XAML (WPF)](../../../desktop-wpf/fundamentals/xaml.md)
