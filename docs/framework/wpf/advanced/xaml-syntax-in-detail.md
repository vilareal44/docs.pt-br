---
title: Sintaxe XAML em detalhes
description: Saiba mais sobre os termos que são usados para descrever os elementos da sintaxe XAML para Windows Presentation Foundation e outras estruturas que usam XAML.
ms.date: 03/30/2017
helpviewer_keywords:
- XML [WPF], namespaces
- XAML [WPF], parsing of attributes
- parsing of attributes [WPF]
- XAML [WPF], markup extensions
- attached properties [WPF]
- tag syntax [XAML]
- markup extensions [WPF]
- XAML [WPF], object element syntax
- XAML [WPF], syntax terminology
- attached events [WPF]
- lookup semantics [WPF]
- XAML [WPF], attached events
- XAML [WPF], content syntax
- XAML [WPF], lookup semantics
- content syntax [WPF]
- object element syntax [WPF]
- syntax terminology [XAML]
- XAML [WPF], attached properties
- attributes [XAML], parsing
- XAML [WPF], tag syntax
- XAML [WPF], attribute syntax
- property element syntax [WPF]
- terminology [XAML]
- namespaces [WPF], XML
- attribute syntax [XAML]
- XAML [WPF], property element syntax
ms.assetid: 67cce290-ca26-4c41-a797-b68aabc45479
ms.openlocfilehash: 6ef217a646b14f02c0b812f6316ec84f26d4b660
ms.sourcegitcommit: 87cfeb69226fef01acb17c56c86f978f4f4a13db
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/24/2020
ms.locfileid: "87168341"
---
# <a name="xaml-syntax-in-detail"></a>Sintaxe XAML em detalhes
Este tópico define os termos que são usados para descrever os elementos da sintaxe XAML. Esses termos são usados com frequência durante o restante desta documentação, tanto especificamente para a documentação do WPF quanto para as outras estruturas que usam XAML ou os conceitos básicos do XAML habilitados pelo suporte à linguagem XAML no nível de System.Xaml. Este tópico trata mais a fundo da terminologia básica introduzida no tópico [Visão geral de XAML (WPF)](../../../desktop-wpf/fundamentals/xaml.md).  

<a name="the_xaml_language_specification"></a>
## <a name="the-xaml-language-specification"></a>Especificação da Linguagem XAML  
 A terminologia de sintaxe XAML definida aqui também é definida ou referenciada dentro da especificação da linguagem XAML. XAML é uma linguagem baseada em XML e segue ou expande regras estruturais do XML. Parte da terminologia é compartilhada com ou se baseia na terminologia usada com frequência ao descrever a linguagem XML ou o modelo de objeto do documento XML.  
  
 Para obter mais informações sobre a especificação da linguagem XAML, baixe [ \[ MS \] -XAML](https://download.microsoft.com/download/0/A/6/0A6F7755-9AF5-448B-907D-13985ACCF53E/[MS-XAML].pdf) no centro de download da Microsoft.  
  
<a name="xaml_and_clr"></a>
## <a name="xaml-and-clr"></a>XAML e CLR  
 XAML é uma linguagem de marcação. O Common Language Runtime (CLR), como implícito pelo nome, permite a execução em tempo de execução. XAML por si só não é uma das linguagens comuns diretamente consumidas pelo runtime de CLR. Em vez disso, você pode pensar em XAML como dando suporte a seu próprio sistema de tipos. O sistema de análise de XAML específico que é usado pelo WPF se baseia no CLR e o sistema de tipos do CLR. Tipos XAML são mapeados para tipos do CLR para instanciar uma representação de tempo de execução quando o XAML para WPF é analisado. Por esse motivo, o restante da discussão de sintaxe neste documento incluirá referências ao sistema de tipos do CLR, embora as discussões de sintaxe equivalentes na especificação da linguagem XAML não o façam. (Segundo o nível de especificação da linguagem XAML, tipos de XAML podem ser mapeados para qualquer outro sistema de tipos, o que significa que esse sistema não precisa ser o CLR; no entanto, isso exigiria a criação e uso de um analisador XAML diferente.)  
  
#### <a name="members-of-types-and-class-inheritance"></a>Membros de tipos e herança de classe  
 Propriedades e eventos, tal como aparecem como membros XAML de um tipo [!INCLUDE[TLA2#tla_winclient](../../../../includes/tla2sharptla-winclient-md.md)], muitas vezes são herdados de tipos base. Considere este exemplo: `<Button Background="Blue" .../>`. A <xref:System.Windows.Controls.Control.Background%2A> propriedade não é uma propriedade imediatamente declarada na <xref:System.Windows.Controls.Button> classe, se você examinar a definição de classe, os resultados de reflexo ou a documentação. Em vez disso, <xref:System.Windows.Controls.Control.Background%2A> é herdado da <xref:System.Windows.Controls.Control> classe base.  
  
 O comportamento de herança de classe de elementos XAML [!INCLUDE[TLA2#tla_winclient](../../../../includes/tla2sharptla-winclient-md.md)] é uma mudança significativa de uma interpretação de marcação XML imposta pelo esquema. A herança de classe pode se tornar complexa, especialmente quando classes base intermediárias são abstratas ou interfaces estão envolvidas. Essa é uma razão pela qual o conjunto de elementos XAML e seus atributos permitidos é difícil de representar de forma precisa e completa usando os tipos de esquema que normalmente são usados para a programação XML, como o formato DTD ou XSD. Outro motivo é que os recursos de extensibilidade e mapeamento de tipo da própria linguagem XAML impedem a integridade de qualquer representação fixa dos tipos e membros permitidos.  
  
<a name="object_element_syntax"></a>
## <a name="object-element-syntax"></a>Sintaxe de elemento de objeto  
 *Sintaxe de elemento de objeto* é a sintaxe de marcação XAML que instancia uma estrutura ou classe CLR pela declaração de um elemento XML. Essa sintaxe é semelhante à sintaxe de outras linguagens de marcação como HTML. A sintaxe de elemento de objeto começa com um colchete angular esquerdo (\<), seguido imediatamente do nome do tipo da classe ou estrutura sendo instanciada. Zero ou mais espaços podem seguir o nome do tipo e zero ou mais atributos podem também ser declarados no elemento de objeto, com um ou mais espaços separando o par nome="valor" de cada atributo. Por fim, uma das seguintes condições deve ser verdadeira:  
  
- O elemento e a marca devem ser fechadas por uma barra (/) seguida imediatamente de um colchete angular direito (>).  
  
- A marca de abertura deve ser concluída por um colchete angular direito (>). Outros elementos de objeto, elementos de propriedade ou texto interno podem vir após a marca de abertura. Exatamente que conteúdo pode estar contido aqui é normalmente restrito pelo modelo de objeto do elemento. A marca de fechamento equivalente para o elemento de objeto também deve existir, com aninhamento e equilíbrio adequados em relação a outros pares de marca de abertura e fechamento.  
  
 XAML, conforme implementado pelo .NET, tem um conjunto de regras que mapeiam elementos de objeto para tipos, atributos para propriedades ou eventos e, por fim namespaces XAML para namespaces CLR mais assembly. Para WPF e .NET, os elementos de objeto XAML são mapeados para tipos .NET, conforme definido em assemblies referenciados, e os atributos são mapeados para os membros desses tipos. Quando você referencia um tipo CLR em XAML, você tem acesso também aos membros herdados desse tipo.  
  
 Por exemplo, o exemplo a seguir é uma sintaxe de elemento de objeto que instancia uma nova instância da <xref:System.Windows.Controls.Button> classe e também especifica um <xref:System.Windows.FrameworkElement.Name%2A> atributo e um valor para esse atributo:  
  
 [!code-xaml[XAMLOvwSupport#SyntaxOE](~/samples/snippets/csharp/VS_Snippets_Wpf/XAMLOvwSupport/CSharp/Page1.xaml#syntaxoe)]  
  
 O exemplo a seguir é uma sintaxe de elemento de objeto que também inclui a sintaxe de propriedade de conteúdo XAML. O texto interno contido em será usado para definir a <xref:System.Windows.Controls.TextBox> propriedade de conteúdo XAML, <xref:System.Windows.Controls.TextBox.Text%2A> .  
  
 [!code-xaml[XAMLOvwSupport#ThisIsATextBox](~/samples/snippets/csharp/VS_Snippets_Wpf/XAMLOvwSupport/CSharp/Page1.xaml#thisisatextbox)]  
  
### <a name="content-models"></a>Modelos de conteúdo  
 Uma classe pode dar suporte a um uso como um elemento de objeto XAML em termos da sintaxe, mas esse elemento só funcionará corretamente em um aplicativo ou página quando ele for colocado em uma posição esperada de uma árvore de elementos ou modelo de conteúdo geral. Por exemplo, um <xref:System.Windows.Controls.MenuItem> normalmente deve ser colocado apenas como um filho de uma <xref:System.Windows.Controls.Primitives.MenuBase> classe derivada, como <xref:System.Windows.Controls.Menu> . Modelos de conteúdo para elementos específicos são documentados como parte dos comentários nas páginas de classe para controles e outras classes [!INCLUDE[TLA2#tla_winclient](../../../../includes/tla2sharptla-winclient-md.md)] que podem ser usadas como elementos XAML.  
  
<a name="properties_of_object_elements"></a>
## <a name="properties-of-object-elements"></a>Propriedades de elementos de objeto  
 Propriedades em XAML são definidas por uma variedade de sintaxes possíveis. Qual sintaxe pode ser usada para uma determinada propriedade é algo que varia com base nas características do sistema de tipos subjacente da propriedade que você está configurando.  
  
 configurando valores de propriedades, você adiciona recursos ou características a objetos que existem no grafo de objeto em tempo de execução. O estado inicial do objeto criado a partir de um elemento Object é baseado no comportamento do construtor sem parâmetros. Normalmente, seu aplicativo usará algo diferente de uma instância completamente padronizada de um determinado objeto.  
  
<a name="attribute_syntax_properties"></a>
## <a name="attribute-syntax-properties"></a>Sintaxe de atributo (Propriedades)  
 Sintaxe de atributo é a sintaxe de marcação XAML que define um valor para uma propriedade, declarando um atributo em um elemento de objeto existente. O nome do atributo deve corresponder ao nome do membro CLR da propriedade da classe dá suporte ao elemento de objeto relevante. O nome do atributo é seguido por um operador de atribuição (=). O valor do atributo deve ser uma cadeia de caracteres entre aspas.  
  
> [!NOTE]
> Você pode usar aspas alternadas para colocar aspas literais dentro de um atributo. Por exemplo, você pode usar aspas simples como meio para declarar uma cadeia de caracteres que contém um caractere de aspas duplas. Independentemente de você usar aspas simples ou duplas, você deve usar um par correspondente para abrir e fechar a cadeia de caracteres de valor de atributo. Também há sequências de escape ou outras técnicas disponíveis para contornar as restrições de caracteres impostas por qualquer sintaxe XAML específica. Consulte [Entidades de caractere XML e XAML](../../../desktop-wpf/xaml-services/xml-character-entities.md).  
  
 Para ser configurada via sintaxe de atributo, uma propriedade deve ser pública e deve ser gravável. O valor da propriedade no sistema de tipos de suporte deve ser um tipo de valor ou deve ser um tipo de referência que possa ser instanciado ou referenciado por um processador XAML ao acessar o tipo de suporte relevante.  
  
 Para eventos de XAML WPF, o evento que é referenciado como o nome do atributo deve ser público e ter um delegado público.  
  
 A propriedade ou evento deve ser um membro da classe ou estrutura que é instanciada pelo elemento de objeto recipiente.  
  
### <a name="processing-of-attribute-values"></a>Processamento de valores de atributo  
 O valor de cadeia de caracteres dentro das aspas de abertura e fechamento é processado por um processador XAML. Para propriedades, o comportamento padrão de processamento é determinado pelo tipo da propriedade CLR subjacente.  
  
 O valor do atributo é preenchido por um dos seguintes, usando esta ordem de processamento:  
  
1. Se o processador XAML encontrar uma chave ou um elemento Object derivado de <xref:System.Windows.Markup.MarkupExtension> , a extensão de marcação referenciada será avaliada primeiro, em vez de processar o valor como uma cadeia de caracteres, e o objeto retornado pela extensão de marcação será usado como o valor. Em muitos casos, o objeto retornado por uma extensão de marcação será uma referência a um objeto existente ou então uma expressão que adia a avaliação até o tempo de execução, mas não será um objeto recém-instanciado.  
  
2. Se a propriedade for declarada com um atributo <xref:System.ComponentModel.TypeConverter> , ou o tipo de valor dessa propriedade for declarado com um atribuído <xref:System.ComponentModel.TypeConverter> , o valor da cadeia de caracteres será enviado ao conversor de tipo como uma entrada de conversão e o conversor retornará uma nova instância de objeto.  
  
3. Se não houver <xref:System.ComponentModel.TypeConverter> , será feita uma tentativa de conversão direta para o tipo de propriedade. Esse nível final é uma conversão direta do valor nativo do analisador entre tipos primitivos de linguagem XAML ou então uma verificação de nomes de constantes nomeadas em uma enumeração (o analisador então acessa os valores correspondentes).  
  
#### <a name="enumeration-attribute-values"></a>Valores de atributo de enumeração  
 Enumerações em XAML são processadas intrinsecamente pelos analisadores XAML e os membros de uma enumeração devem ser definidos especificando-se o nome da cadeia de caracteres de uma das constantes nomeadas da enumeração.  
  
 Para valores de enumeração não sinalizadores, o comportamento nativo é processar a cadeia de caracteres de um valor de atributo e resolvê-lo para um dos valores de enumeração. Você não especifica a enumeração no formato *Enumeração*.*Valor*, do modo como você faz no código. Em vez disso, você especifica somente *Valor*, enquanto *Enumeração* é inferido pelo tipo da propriedade você está configurando. Se você especificar um atributo no formato *Enumeração*.*Valor*, ele não será resolvido corretamente.  
  
 Para enumerações flagwise, o comportamento é baseado no <xref:System.Enum.Parse%2A?displayProperty=nameWithType> método. Você pode especificar vários valores para uma enumeração sinalizadora, separando cada valor com uma vírgula. No entanto, não é possível combinar valores de enumeração que não reconhecem sinalizadores. Por exemplo, você não pode usar a sintaxe de vírgula para tentar criar um <xref:System.Windows.Trigger> que atue em várias condições de uma enumeração não sinalizada:  
  
```xaml  
<!--This will not compile, because Visibility is not a flagwise enumeration.-->  
...  
<Trigger Property="Visibility" Value="Collapsed,Hidden">  
  <Setter ... />  
</Trigger>  
...  
```  
  
 Enumerações sinalizadoras que dão suporte a atributos que são configuráveis no XAML são raras no WPF. No entanto, uma dessas enumerações é <xref:System.Windows.Media.StyleSimulations> . Você poderia, por exemplo, usar a sintaxe de atributo flagwise delimitada por vírgula para modificar o exemplo fornecido nos comentários para a <xref:System.Windows.Documents.Glyphs> classe; `StyleSimulations = "BoldSimulation"` poderia se tornar `StyleSimulations = "BoldSimulation,ItalicSimulation"` . <xref:System.Windows.Input.KeyBinding.Modifiers%2A?displayProperty=nameWithType>é outra propriedade em que mais de um valor de enumeração pode ser especificado. No entanto, essa propriedade é um caso especial, porque a <xref:System.Windows.Input.ModifierKeys> Enumeração dá suporte a seu próprio conversor de tipo. O conversor de tipo para modificadores usa um sinal de adição (+) como um delimitador em vez de uma vírgula (,). Essa conversão dá suporte à sintaxe mais tradicional para representar combinações de teclas na programação do Microsoft Windows, como "Ctrl + Alt".  
  
### <a name="properties-and-event-member-name-references"></a>Propriedades e referências de nome de membro de evento  
 Ao especificar um atributo, você pode referenciar qualquer propriedade ou evento existente como um membro do tipo CLR que você instanciou para o elemento de objeto que o contém.  
  
 Ou você pode fazer referência a uma propriedade anexada ou evento anexado, independente do elemento de objeto que o contém. (Propriedades anexadas são discutidas em uma seção posterior).  
  
 Você também pode nomear qualquer evento de qualquer objeto que possa ser acessado através do namespace padrão usando um nome parcialmente qualificado *typeName*.*evento*; essa sintaxe dá suporte à anexação de manipuladores para eventos roteados em que o manipulador destina-se a manipular o roteamento de eventos de elementos filho, mas o elemento pai também não tem esse evento em sua tabela de membros. Essa sintaxe é semelhante a uma sintaxe de evento anexado, mas o evento aqui não é um evento anexado verdadeiro. Em vez disso, você está referenciando um evento com um nome qualificado. Para obter mais informações, consulte [Visão geral de eventos roteados](routed-events-overview.md).  
  
 Em alguns cenários, os nomes de propriedade às vezes são fornecidos como o valor de um atributo, em vez do nome do atributo. Esse nome de propriedade também pode incluir qualificadores como a propriedade especificada no formato *ownerType*.*dependencyPropertyName*. Esse cenário é comum ao escrever estilos ou modelos em XAML. As regras de processamento para nomes de propriedade fornecidos como um valor de atributo são diferentes e são regidas pelo tipo da propriedade sendo definida ou os comportamentos de subsistemas do WPF específicos. Para obter detalhes, consulte [Estilo e modelagem](../../../desktop-wpf/fundamentals/styles-templates-overview.md).  
  
 Outro uso para nomes de propriedade é quando um valor de atributo descreve uma relação propriedade-propriedade. Esse recurso é usado para vinculação de dados e para destinos de storyboard, e é habilitado pela <xref:System.Windows.PropertyPath> classe e seu conversor de tipo. Para obter uma descrição mais completa da semântica de pesquisa, consulte [Sintaxe XAML PropertyPath](propertypath-xaml-syntax.md).  
  
<a name="property_element_syntax"></a>
## <a name="property-element-syntax"></a>Sintaxe de elemento de propriedade  
 *Sintaxe de elemento de propriedade* é uma sintaxe que diverge um pouco das regras de sintaxe XML básicas para elementos. Em XML, o valor de um atributo é uma cadeia de caracteres de fato, com a única variação possível sendo qual formato de codificação de cadeia de caracteres está sendo usado. Em XAML, você pode atribuir outros elementos de objeto para serem o valor de uma propriedade. Essa funcionalidade é habilitada pela sintaxe de elemento de propriedade. Em vez de a propriedade ser especificada como um atributo na marca de elemento, a propriedade é especificada usando uma marca de elemento de abertura na forma *elementTypeName*.*propertyName*, o valor da propriedade é especificado ali e então o elemento de propriedade é fechado.  
  
 Especificamente, a sintaxe começa com um colchete angular esquerdo ( \<), followed immediately by the type name of the class or structure that the property element syntax is contained within. This is followed immediately by a single dot (.), then by the name of a property, then by a right angle bracket (> ). Assim como acontece com a sintaxe de atributo, essa propriedade deve existir nos membros públicos declarados do tipo especificado. O valor a ser atribuído à propriedade está contido dentro do elemento de propriedade. Normalmente, o valor é fornecido como um ou mais elementos de objeto, porque a especificação de objetos como valores é o cenário que essa sintaxe de elemento de propriedade destina-se a solucionar. Por fim, uma marca de fechamento equivalente especificando a mesma combinação *elementTypeName*.*propertyName* deve ser fornecida, com aninhamento e equilíbrio adequados em relação a outras marcas de elemento.  
  
 Por exemplo, a seguir está a sintaxe do elemento Property para a <xref:System.Windows.FrameworkElement.ContextMenu%2A> propriedade de um <xref:System.Windows.Controls.Button> .  
  
 [!code-xaml[XAMLOvwSupport#ContextMenu](~/samples/snippets/csharp/VS_Snippets_Wpf/XAMLOvwSupport/CSharp/Page1.xaml#contextmenu)]  
  
 O valor dentro de um elemento Property também pode ser dado como texto interno, em casos em que o tipo de propriedade especificado é um tipo de valor primitivo, como <xref:System.String> , ou uma enumeração em que um nome é especificado. Esses dois usos são um pouco incomuns, pois cada um desses casos também pode usar uma sintaxe de atributo mais simples. Um cenário para preencher um elemento de propriedade com uma cadeia de caracteres é para propriedades que não são a propriedade de conteúdo XAML, mas que ainda são usadas para representação de texto da interface do usuário, e elementos de espaço em branco específicos, como alimentação de linha, devem aparecer nesse texto da interface do usuário. A sintaxe do atributo não pode preservar a alimentação de discagem, mas a sintaxe do elemento de propriedade pode, desde que a preservação de espaço em branco significativa esteja ativa (para obter detalhes, consulte [processamento de espaço em branco em XAML](../../../desktop-wpf/xaml-services/white-space-processing.md)). Outro cenário é aquele no qual a [Política x:Uid](../../../desktop-wpf/xaml-services/xuid-directive.md) pode ser aplicada ao elemento de propriedade e, portanto, marca o valor dentro dele como um valor que deve ser localizado BAML de saída do no WPF ou por outras técnicas.  
  
 Um elemento de propriedade não é representado na árvore lógica do WPF. Um elemento de propriedade é apenas uma sintaxe específica para definir uma propriedade e não é um elemento que disponha do suporte de uma instância ou objeto. (Para obter detalhes sobre o conceito de árvore lógica, consulte [Árvores no WPF](trees-in-wpf.md).)  
  
 Para propriedades onde há suporte para a sintaxe de elemento de propriedade e atributo, as duas sintaxes geralmente têm o mesmo resultado, embora as sutilezas, como tratamento de espaço em branco, possam variar um pouco entre as sintaxes.  
  
<a name="collection_syntax"></a>
## <a name="collection-syntax"></a>Sintaxe de coleção  
 A especificação da linguagem XAML requer implementações de processador XAML para identificar propriedades nas quais o tipo de valor é uma coleção. A implementação geral de processador XAML no .NET é baseada em código gerenciado e no CLR e identifica os tipos de coleção por meio de um dos seguintes:  
  
- O tipo implementa <xref:System.Collections.IList> .  
  
- O tipo implementa <xref:System.Collections.IDictionary> .  
  
- O tipo deriva de <xref:System.Array> (para obter mais informações sobre matrizes em XAML, consulte [X:Array Markup Extension](../../../desktop-wpf/xaml-services/xarray-markup-extension.md)).  
  
 Se o tipo de uma propriedade é uma coleção, o tipo da coleção inferido não precisa ser especificado na marcação como um elemento de objeto. Em vez disso, os elementos a se tornarem itens na coleção são especificados como um ou mais elementos filho do elemento de propriedade. Cada um desses itens é avaliado para um objeto durante o carregamento e adicionado à coleção chamando o método `Add` da coleção implícita. Por exemplo, a <xref:System.Windows.Style.Triggers%2A> propriedade de <xref:System.Windows.Style> usa o tipo de coleção especializado <xref:System.Windows.TriggerCollection> , que implementa <xref:System.Collections.IList> . Não é necessário criar uma instância de um <xref:System.Windows.TriggerCollection> elemento Object na marcação. Em vez disso, você especifica um ou mais <xref:System.Windows.Trigger> itens como elementos dentro do `Style.Triggers` elemento Property, em que <xref:System.Windows.Trigger> (ou uma classe derivada) é o tipo esperado como o tipo de item para o fortemente tipado e implícito <xref:System.Windows.TriggerCollection> .  
  
 [!code-xaml[XAMLOvwSupport#SyntaxPECollection](~/samples/snippets/csharp/VS_Snippets_Wpf/XAMLOvwSupport/CSharp/Page1.xaml#syntaxpecollection)]  
  
 Uma propriedade pode ser tanto um tipo de coleção quanto a propriedade de conteúdo XAML para esse tipo e tipos derivados, o que é abordado na próxima seção deste tópico.  
  
 Um elemento de coleção implícita cria um membro na representação de árvore lógica, mesmo que ele não apareça na marcação como um elemento. Geralmente o construtor do tipo pai faz a instanciação para a coleção que é uma de suas propriedades e a coleção inicialmente vazia torna-se parte da árvore de objetos.  
  
> [!NOTE]
> A lista genérica e as interfaces de dicionário ( <xref:System.Collections.Generic.IList%601> e <xref:System.Collections.Generic.IDictionary%602> ) não têm suporte para detecção de coleção. No entanto, você pode usar a <xref:System.Collections.Generic.List%601> classe como uma classe base, porque ela implementa <xref:System.Collections.IList> diretamente ou <xref:System.Collections.Generic.Dictionary%602> como uma classe base, porque ela implementa <xref:System.Collections.IDictionary> diretamente.  
  
 Nas páginas de referência do .NET para tipos de coleção, essa sintaxe com a omissão deliberada do elemento de objeto para uma coleção é ocasionalmente mencionada nas seções de sintaxe XAML como sintaxe de coleção implícita.  
  
 Com exceção do elemento raiz, cada elemento de objeto em um arquivo XAML que está aninhado como um elemento filho de outro elemento é na realidade um elemento que se enquadra em um ou ambos os casos a seguir: um membro de uma propriedade de coleção implícita do seu elemento pai ou então um elemento que especifica o valor da propriedade de conteúdo XAML para o elemento pai (propriedades de conteúdo XAML serão discutidas em uma seção posterior). Em outras palavras, a relação dos elementos pai e filho em uma página de marcação é na verdade um único objeto na raiz, sendo que todo elemento de objeto abaixo da raiz é uma instância única que fornece um valor da propriedade do pai ou então é um dos itens dentro de uma coleção que é também um valor da propriedade de tipo de coleção do pai. Esse conceito de raiz única é comum com XML e é frequentemente reforçado no comportamento de APIs que carregam XAML, como <xref:System.Windows.Markup.XamlReader.Load%2A> .  
  
 O exemplo a seguir é uma sintaxe com o elemento Object para uma coleção ( <xref:System.Windows.Media.GradientStopCollection> ) especificada explicitamente.  
  
```xaml  
<LinearGradientBrush>  
  <LinearGradientBrush.GradientStops>  
    <GradientStopCollection>  
      <GradientStop Offset="0.0" Color="Red" />  
      <GradientStop Offset="1.0" Color="Blue" />  
    </GradientStopCollection>  
  </LinearGradientBrush.GradientStops>  
</LinearGradientBrush>  
```  
  
 Observe que nem sempre é possível declarar explicitamente a coleção. Por exemplo, a tentativa de declarar <xref:System.Windows.TriggerCollection> explicitamente no exemplo mostrado anteriormente <xref:System.Windows.Style.Triggers%2A> falharia. Declarar explicitamente a coleção requer que a classe de coleção tenha suporte para um construtor sem parâmetros e não <xref:System.Windows.TriggerCollection> tenha um construtor sem parâmetros.  
  
<a name="xaml_content_properties"></a>
## <a name="xaml-content-properties"></a>Propriedades de conteúdo XAML  
 Sintaxe de conteúdo XAML é uma sintaxe que só é habilitada em classes que especificam o <xref:System.Windows.Markup.ContentPropertyAttribute> como parte de sua declaração de classe. O <xref:System.Windows.Markup.ContentPropertyAttribute> faz referência ao nome da propriedade que é a propriedade de conteúdo para esse tipo de elemento (incluindo classes derivadas). Quando processados por um processador XAML, quaisquer elementos filho ou texto interno que sejam encontrados entre as marcas de abertura e fechamento do elemento de objeto serão atribuídos como sendo o valor da propriedade de conteúdo XAML para esse objeto. Você tem permissão para especificar elementos de propriedade explícitos para a propriedade de conteúdo, mas esse uso geralmente não é mostrado nas seções de sintaxe XAML na referência do .NET. A técnica explícita/detalhada tem valor ocasional para fins de esclarecimento de marcação ou como uma questão de estilo de marcação, mas a intenção de uma propriedade de conteúdo costuma ser simplificar a marcação para que os elementos relacionados intuitivamente como pai/filho possam ser diretamente aninhados. Marcas de elemento de propriedade para outras propriedades em um elemento não são atribuídas como "conteúdo" segundo uma definição estrita de linguagem XAML; eles são processados anteriormente na ordem de processamento do analisador XAML e não são considerados "conteúdo".  
  
### <a name="xaml-content-property-values-must-be-contiguous"></a>Valores de propriedade de conteúdo XAML devem ser contíguos  
 O valor de uma propriedade de conteúdo XAML deve ser fornecido inteiramente antes ou inteiramente depois de todos os outros elementos de propriedade nesse elemento de objeto. Isso é verdadeiro se o valor de uma propriedade de conteúdo XAML é especificado como uma cadeia de caracteres ou como um ou mais objetos. Por exemplo, a marcação a seguir não analisa:  
  
```xaml  
<Button>I am a
  <Button.Background>Blue</Button.Background>  
  blue button</Button>  
```  
  
 Isso é ilegal essencialmente porque se essa sintaxe tivesse sido tornada explícita usando sintaxe de elemento de propriedade para a propriedade de conteúdo, a propriedade de conteúdo seria definida duas vezes:  
  
```xaml  
<Button>  
  <Button.Content>I am a </Button.Content>  
  <Button.Background>Blue</Button.Background>  
  <Button.Content> blue button</Button.Content>  
</Button>  
```  
  
 Um exemplo similarmente ilegal é no caso de a propriedade de conteúdo ser uma coleção e elementos filho serem intercalados com elementos de propriedade:  
  
```xaml  
<StackPanel>  
  <Button>This example</Button>  
  <StackPanel.Resources>  
    <SolidColorBrush x:Key="BlueBrush" Color="Blue"/>  
  </StackPanel.Resources>  
  <Button>... is illegal XAML</Button>  
</StackPanel>  
```  
  
<a name="content_properties_and_collection_syntax_combined"></a>
## <a name="content-properties-and-collection-syntax-combined"></a>Propriedades de conteúdo e sintaxe de coleção combinados  
 Para aceitar mais de um único elemento de objeto como conteúdo, o tipo da propriedade de conteúdo deve ser especificamente um tipo de coleção. Semelhante à sintaxe de elemento de propriedade para tipos de coleção, um processador XAML deve identificar tipos que são tipos de coleção. Se um elemento tem uma propriedade de conteúdo XAML e o tipo da propriedade de conteúdo XAML é uma coleção, o tipo de coleção sugerido não precisa ser especificado na marcação como um elemento de objeto e a propriedade de conteúdo XAML não precisa ser especificada como um elemento de propriedade. Portanto, o modelo de conteúdo aparente na marcação agora pode ter mais de um elemento filho atribuído como o conteúdo. Veja a seguir a sintaxe de conteúdo para uma <xref:System.Windows.Controls.Panel> classe derivada. Todas as <xref:System.Windows.Controls.Panel> classes derivadas estabelecem a propriedade de conteúdo XAML como <xref:System.Windows.Controls.Panel.Children%2A> , que requer um valor do tipo <xref:System.Windows.Controls.UIElementCollection> .  
  
 [!code-xaml[XAMLOvwSupport#SyntaxContent](~/samples/snippets/csharp/VS_Snippets_Wpf/XAMLOvwSupport/CSharp/page5.xaml#syntaxcontent)]  
  
 Observe que nem o elemento Property <xref:System.Windows.Controls.Panel.Children%2A> nem o elemento para o <xref:System.Windows.Controls.UIElementCollection> é necessário na marcação. Esse é um recurso de design do XAML para que elementos recursivamente contidos que definem um [!INCLUDE[TLA2#tla_ui](../../../../includes/tla2sharptla-ui-md.md)] sejam representados mais intuitivamente como uma árvore de elementos aninhados com relações pai/filho imediatas, sem objetos de coleção ou marcas entrepostas. Na verdade, <xref:System.Windows.Controls.UIElementCollection> não pode ser especificado explicitamente na marcação como um elemento de objeto, por design. Como seu único uso pretendido é como uma coleção implícita, não <xref:System.Windows.Controls.UIElementCollection> expõe um construtor público sem parâmetros e, portanto, não pode ser instanciado como um elemento Object.  
  
### <a name="mixing-property-elements-and-object-elements-in-an-object-with-a-content-property"></a>Combinação de elementos de propriedade e elementos de objeto em um objeto com uma propriedade de conteúdo  
 A especificação da linguagem XAML declara que um processador XAML pode impor que elementos de objeto que são usados para preencher a propriedade de conteúdo XAML dentro de um elemento de objeto precisem ser contíguos e não possam ser misturados. Essa restrição contra misturar elementos de propriedade e conteúdo é imposta pelos processadores XAML [!INCLUDE[TLA2#tla_winclient](../../../../includes/tla2sharptla-winclient-md.md)].  
  
 Você pode ter um elemento de objeto filho como a primeira marcação imediata dentro de um elemento de objeto. Em seguida, você pode introduzir elementos de propriedade. Ou ainda, você pode especificar um ou mais elementos de propriedade, então conteúdo e depois mais elementos de propriedade. Mas uma vez que um elemento de propriedade segue o conteúdo, você não pode introduzir nenhum conteúdo adicional, você só pode adicionar elementos de propriedade.  
  
 Este requisito de ordem de elementos de propriedade/conteúdo não se aplica ao texto interno usado como conteúdo. No entanto, ele ainda é um bom estilo de marcação para manter o texto interno contíguo, pois será difícil detectar um espaço em branco significativo visualmente na marcação se os elementos de propriedade estiverem intercalados com texto interno.  
  
<a name="xaml_namespaces"></a>
## <a name="xaml-namespaces"></a>Namespaces XAML  
 Nenhum dos exemplos de sintaxe anteriores especificaram um namespace XAML diferente do namespace XAML padrão. Em aplicativos [!INCLUDE[TLA2#tla_winclient](../../../../includes/tla2sharptla-winclient-md.md)] típicos, o namespace XAML padrão é especificado como sendo o namespace [!INCLUDE[TLA2#tla_winclient](../../../../includes/tla2sharptla-winclient-md.md)]. Você pode especificar namespaces XAML diferentes do namespace XAML padrão e ainda assim usar uma sintaxe semelhante. Mas nesse casso, em qualquer lugar em que for nomeada uma classe não acessível dentro do namespace XAML padrão, esse nome de classe deverá ser precedido do prefixo do namespace XAML conforme mapeado para o namespace CLR correspondente. Por exemplo, `<custom:Example/>` é a sintaxe de elemento de objeto para criar uma instância da classe `Example`, em que o namespace CLR que contém essa classe (e possivelmente as informações de assembly externo que contêm tipos de suporte) foi anteriormente mapeado para o prefixo `custom`.  
  
 Para obter mais informações sobre namespaces XAML, consulte [Namespaces XAML e mapeamento de namespace para XAML WPF](xaml-namespaces-and-namespace-mapping-for-wpf-xaml.md).  
  
<a name="markup_extensions"></a>
## <a name="markup-extensions"></a>Extensões de marcação  
 O XAML define uma entidade de programação de extensão de marcação que habilita um escape da manipulação normal de valores de atributo de cadeia de caracteres ou elementos de objeto pelo processador XAML e adia o processamento para uma classe de suporte. O caractere que identifica um extensão de marcação para um processador XAML ao usar a sintaxe de atributo é a chave de abertura ({), seguida por qualquer caractere que não seja uma chave de fechamento (}). A primeira cadeia de caracteres após a chave de abertura deve fazer referência à classe que oferece o comportamento de extensão específico, em que a referência pode omitir a subcadeia de caracteres "Extension" se essa subcadeia faz parte do verdadeiro nome de classe. Depois disso, pode aparecer um único espaço e, em seguida, cada caractere subsequente é usado como entrada pela implementação de extensão até que a chave de fechamento é encontrada.  
  
 A implementação do XAML do .NET usa a <xref:System.Windows.Markup.MarkupExtension> classe abstrata como a base para todas as extensões de marcação com suporte, [!INCLUDE[TLA2#tla_winclient](../../../../includes/tla2sharptla-winclient-md.md)] bem como outras estruturas ou tecnologias. As extensões de marcação que [!INCLUDE[TLA2#tla_winclient](../../../../includes/tla2sharptla-winclient-md.md)] implementa especificamente geralmente se destinam a fornecer um meio para fazer referência a outros objetos existentes ou fazer referências adiadas a objetos que serão avaliados em tempo de execução. Por exemplo, uma vinculação de dados simples do WPF é feita especificando a extensão de marcação `{Binding}` no lugar do valor que uma propriedade específica normalmente aceitaria. Muitas das extensões de marcação do WPF permitem uma sintaxe de atributo para propriedades em que a sintaxe de atributo não seria possível de outra maneira. Por exemplo, um <xref:System.Windows.Style> objeto é um tipo relativamente complexo que contém uma série aninhada de objetos e propriedades. Os estilos no WPF normalmente são definidos como um recurso em um <xref:System.Windows.ResourceDictionary> e, em seguida, referenciados por meio de uma das duas extensões de marcação do WPF que solicitam um recurso. A extensão de marcação adia a avaliação do valor da propriedade para uma pesquisa de recurso e habilita o fornecimento do valor da <xref:System.Windows.FrameworkElement.Style%2A> propriedade, usando o tipo <xref:System.Windows.Style> , na sintaxe do atributo, como no exemplo a seguir:  
  
 `<Button Style="{StaticResource MyStyle}">My button</Button>`  
  
 Aqui, `StaticResource` identifica a <xref:System.Windows.StaticResourceExtension> classe que fornece a implementação da extensão de marcação. A próxima cadeia de caracteres `MyStyle` é usada como entrada para o construtor não padrão <xref:System.Windows.StaticResourceExtension> , em que o parâmetro como tirado da cadeia de caracteres de extensão declara o solicitado <xref:System.Windows.ResourceKey> . `MyStyle`deve ser o valor de [x:Key](../../../desktop-wpf/xaml-services/xkey-directive.md) de um <xref:System.Windows.Style> definido como um recurso. O uso da [extensão de marcação StaticResource](staticresource-markup-extension.md) solicita que o recurso seja usado para fornecer o <xref:System.Windows.Style> valor da propriedade por meio da lógica de pesquisa de recursos estáticos no tempo de carregamento.  
  
 Para obter mais informações sobre extensões de marcação, consulte [Extensões de marcação e XAML WPF](markup-extensions-and-wpf-xaml.md). Para obter uma referência de extensões de marcação e outros recursos de programação em XAML habilitados na implementação geral do XAML no .NET, consulte [Recursos de linguagem (x:) do namespace XAML](../../../desktop-wpf/xaml-services/namespace-language-features.md). Para extensões de marcação específicas de WPF, consulte [Extensões XAML WPF](wpf-xaml-extensions.md).  
  
<a name="attached_properties"></a>
## <a name="attached-properties"></a>Propriedades Anexadas  
 Propriedades anexadas são um conceito de programação introduzido no XAML no qual propriedades podem pertencer a um determinado tipo ou ser definidas por ele, mas definidas como atributos ou elementos de propriedade em qualquer elemento. O cenário principal para o qual propriedades anexadas se destinam é habilitar elementos filho em uma estrutura de marcação para relatar informações a um elemento pai sem exigir um modelo de objeto amplamente compartilhado entre todos os elementos. Por outro lado, propriedades anexadas podem ser usadas pelos elementos pai para relatar informações para elementos filhos. Para obter mais informações sobre o objetivo de propriedades anexadas e como criar suas próprias propriedades anexadas, consulte [Visão geral das propriedades anexadas](attached-properties-overview.md).  
  
 Propriedades anexadas utilizam uma sintaxe que superficialmente se assemelha à sintaxe de elemento de propriedade, em que você também especifica uma combinação *typeName*.*propertyName*. Há duas diferenças importantes:  
  
- Você pode usar a combinação *typeName*.*propertyName* mesmo ao definir uma propriedade anexada por meio da sintaxe de atributo. Propriedades anexadas são o único caso em que a qualificação do nome da propriedade é um requisito em uma sintaxe de atributo.  
  
- Você também pode usar a sintaxe de elemento de propriedade para propriedades anexadas. No entanto, para a sintaxe de elemento de propriedade típica, o *typeName* que você especifica é o elemento de objeto que contém o elemento de propriedade. Se você estiver fazendo referência a uma propriedade anexada, o *typeName* será a classe que define a propriedade anexada, não o elemento de objeto que a contém.  
  
<a name="attached_events"></a>
## <a name="attached-events"></a>Eventos Anexados  
 Eventos anexados são outro conceito de programação introduzido em XAML em que os eventos podem ser definidos por um tipo específico, mas manipuladores podem ser anexados a qualquer elemento de objeto. Na implementação WOF, geralmente o tipo que define um evento anexado é um tipo estático que define um serviço e, às vezes, esses eventos anexados são expostos por um alias de evento roteado em tipos que expõem o serviço. Manipuladores para eventos anexados são especificados pela sintaxe de atributo. Assim como ocorre com eventos anexados, a sintaxe de atributo é expandida para eventos anexados para permitir um uso de *typeName*.*eventName*, em que *typeName* é a classe que fornece os acessadores de manipuladores de eventos `Add` e `Remove` para a infraestrutura de evento anexado e *eventName* é o nome do evento.  
  
<a name="anatomy_of_a_xaml_page_root_element"></a>
## <a name="anatomy-of-a-xaml-root-element"></a>Anatomia de um elemento raiz XAML  
 A tabela a seguir mostra um típico elemento raiz XAML, mostrando os atributos específicos de um elemento raiz:  
  
|||  
|-|-|  
|`<Page`|Elemento de objeto de abertura do elemento raiz|  
|`xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"`|O namespace XAML padrão ([!INCLUDE[TLA2#tla_winclient](../../../../includes/tla2sharptla-winclient-md.md)])|  
|`xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"`|Namespace XAML de linguagem XAML|  
|`x:Class="ExampleNamespace.ExampleCode"`|A declaração de classe parcial que conecta a marcação a qualquer code-behind definido para a classe parcial|  
|`>`|Final do elemento de objeto para a raiz. Objeto ainda não foi fechado porque o elemento contém elementos filho|  
  
<a name="optional_and_nonrecommended_xaml_usages"></a>
## <a name="optional-and-nonrecommended-xaml-usages"></a>Usos de XAML opcionais e não recomendados  
 As seções a seguir descrevem os usos de XAML que são tecnicamente suportados pelos processadores XAML, mas que produzem detalhes ou outros problemas estéticos que interferem com arquivos XAML restantes que podem ser lidos por pessoas quando você desenvolve aplicativos que contêm fontes XAML.  
  
### <a name="optional-property-element-usages"></a>Usos de elemento de propriedade opcional  
 Usos de elemento de propriedade opcional incluem escrever explicitamente propriedades de conteúdo de elemento que o processador XAML considera implícitas. Por exemplo, ao declarar o conteúdo de um <xref:System.Windows.Controls.Menu> , você pode optar por declarar explicitamente a <xref:System.Windows.Controls.ItemsControl.Items%2A> coleção de <xref:System.Windows.Controls.Menu> como uma marca de `<Menu.Items>` elemento de propriedade e colocar cada uma <xref:System.Windows.Controls.MenuItem> delas `<Menu.Items>` , em vez de usar o comportamento de processador XAML implícito que todos os elementos filho de a <xref:System.Windows.Controls.Menu> devem ser a <xref:System.Windows.Controls.MenuItem> e são colocados na <xref:System.Windows.Controls.ItemsControl.Items%2A> coleção. Às vezes os usos opcionais podem ajudar a esclarecer visualmente a estrutura do objeto conforme representada na marcação. Ou, às vezes, um uso de elemento de propriedade explícito pode evitar uma marcação que é tecnicamente funcional, mas visualmente confusa, assim como extensões de marcação aninhadas dentro de um valor de atributo.  
  
### <a name="full-typenamemembername-qualified-attributes"></a>Atributos qualificados typeName.memberName completos  
 A forma *typeName*.*memberName* para um atributo realmente funciona mais universalmente do que apenas para o caso de evento roteado. Em outras situações, no entanto, essa forma é supérflua e você deve evitá-la, mesmo que apenas por motivos de estilo de marcação e legibilidade. No exemplo a seguir, cada uma das três referências ao <xref:System.Windows.Controls.Control.Background%2A> atributo é completamente equivalente:  
  
 [!code-xaml[XAMLOvwSupport#TypeNameProp](~/samples/snippets/csharp/VS_Snippets_Wpf/XAMLOvwSupport/CSharp/page8.xaml#typenameprop)]  
  
 `Button.Background`funciona porque a pesquisa qualificada para essa propriedade em <xref:System.Windows.Controls.Button> é bem-sucedida ( <xref:System.Windows.Controls.Control.Background%2A> foi herdada do controle) e <xref:System.Windows.Controls.Button> é a classe do elemento Object ou de uma classe base. `Control.Background`funciona porque a <xref:System.Windows.Controls.Control> classe realmente define <xref:System.Windows.Controls.Control.Background%2A> e <xref:System.Windows.Controls.Control> é uma <xref:System.Windows.Controls.Button> classe base.  
  
 No entanto, exemplo de formato de *typeName*.*memberName* a seguir não funciona e, portanto, é mostrado comentado:  
  
 [!code-xaml[XAMLOvwSupport#TypeNameBadProp](~/samples/snippets/csharp/VS_Snippets_Wpf/XAMLOvwSupport/CSharp/page8.xaml#typenamebadprop)]  
  
 <xref:System.Windows.Controls.Label>é outra classe derivada de <xref:System.Windows.Controls.Control> e, se você tiver especificado `Label.Background` dentro de um <xref:System.Windows.Controls.Label> elemento Object, esse uso funcionaria. No entanto, como <xref:System.Windows.Controls.Label> o não é a classe ou a classe base de <xref:System.Windows.Controls.Button> , o comportamento do processador XAML especificado é então processado `Label.Background` como uma propriedade anexada. `Label.Background` não é uma propriedade anexada disponível e esse uso falha.  
  
### <a name="basetypenamemembername-property-elements"></a>Elementos de propriedade baseTypeName.memberName  
 De maneira semelhante ao modo como o formulário *typeName*.*memberName* funciona para a sintaxe de atributo, uma sintaxe *baseTypeName*.*memberName* funciona para a sintaxe de elemento de propriedade. Por exemplo, a sintaxe a seguir funciona:  
  
 [!code-xaml[XAMLOvwSupport#GoofyPE](~/samples/snippets/csharp/VS_Snippets_Wpf/XAMLOvwSupport/CSharp/page8.xaml#goofype)]  
  
 Aqui o elemento de propriedade foi fornecido como `Control.Background`, embora o elemento de propriedade estivesse contido em `Button`.  
  
 Mas assim como o formato *typeName*.*memberName* para atributos, *baseTypeName*.*memberName* é um estilo ruim de marcação e você deve evitá-lo.  
  
## <a name="see-also"></a>Confira também

- [Visão geral de XAML (WPF)](../../../desktop-wpf/fundamentals/xaml.md)
- [Namespace XAML (x:) Funcionalidades de linguagem](../../../desktop-wpf/xaml-services/namespace-language-features.md)
- [Extensões XAML WPF](wpf-xaml-extensions.md)
- [Visão geral das propriedades de dependência](dependency-properties-overview.md)
- [TypeConverters e XAML](typeconverters-and-xaml.md)
- [XAML e classes personalizadas para WPF](xaml-and-custom-classes-for-wpf.md)
