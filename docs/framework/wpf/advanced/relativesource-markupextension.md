---
title: RelativeSource MarkupExtension
description: Especifica as propriedades de uma fonte de associação RelativeSource, dentro de uma extensão de marcação de associação, ou ao definir a Propriedade RelativeSource de uma associação em XAML.
ms.date: 03/30/2017
f1_keywords:
- RelativeSource
helpviewer_keywords:
- RelativeSource markup extensions [WPF]
- XAML [WPF], RelativeSource markup extension
ms.assetid: 26be4721-49b5-4717-a92e-7d54ad0d3a81
ms.openlocfilehash: 2da702d23413651a85b45404e088f6708546cc25
ms.sourcegitcommit: 87cfeb69226fef01acb17c56c86f978f4f4a13db
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/24/2020
ms.locfileid: "87165937"
---
# <a name="relativesource-markupextension"></a>RelativeSource MarkupExtension

Especifica as propriedades de uma <xref:System.Windows.Data.RelativeSource> fonte de associação, a ser usada em uma [extensão de marcação de associação](binding-markup-extension.md)ou ao definir a <xref:System.Windows.Data.Binding.RelativeSource%2A> propriedade de um <xref:System.Windows.Data.Binding> elemento estabelecido em XAML.

## <a name="xaml-attribute-usage"></a>Uso do Atributo XAML

```xml
<Binding RelativeSource="{RelativeSource modeEnumValue}" ... />
```

## <a name="xaml-attribute-usage-nested-within-binding-extension"></a>Uso de atributo XAML (aninhado em uma extensão de associação)

```xml
<object property="{Binding RelativeSource={RelativeSource modeEnumValue} ...}" ... />
```

## <a name="xaml-object-element-usage"></a>Uso de elemento Object do XAML

```xml
<Binding>
  <Binding.RelativeSource>
    <RelativeSource Mode="modeEnumValue"/>
  </Binding.RelativeSource>
</Binding>
```

-ou-

```xml
<Binding>
  <Binding.RelativeSource>
    <RelativeSource
      Mode="FindAncestor"
      AncestorType="{x:Type typeName}"
      AncestorLevel="intLevel"
    />
  </Binding.RelativeSource>
</Binding>
```

## <a name="xaml-values"></a>Valores XAML

|||
|-|-|
|`modeEnumValue`|Um dos seguintes:<br /><br /> -O token de cadeia de caracteres `Self` ; corresponde a um <xref:System.Windows.Data.RelativeSource> conforme criado com sua <xref:System.Windows.Data.RelativeSource.Mode%2A> propriedade definida como <xref:System.Windows.Data.RelativeSourceMode.Self> .<br />-O token de cadeia de caracteres `TemplatedParent` ; corresponde a um <xref:System.Windows.Data.RelativeSource> conforme criado com sua <xref:System.Windows.Data.RelativeSource.Mode%2A> propriedade definida como <xref:System.Windows.Data.RelativeSourceMode.TemplatedParent> .<br />-O token de cadeia de caracteres `PreviousData` ; corresponde a um <xref:System.Windows.Data.RelativeSource> conforme criado com sua <xref:System.Windows.Data.RelativeSource.Mode%2A> propriedade definida como <xref:System.Windows.Data.RelativeSourceMode.PreviousData> .<br />– Veja abaixo para obter informações sobre o modo `FindAncestor`.|
|`FindAncestor`|O token da cadeia de caracteres `FindAncestor`. Ao usar este token entra-se em um modo por meio do qual `RelativeSource` especifica um tipo de ancestral e opcionalmente um nível de ancestral. Isso corresponde a <xref:System.Windows.Data.RelativeSource>, conforme criado com a propriedade <xref:System.Windows.Data.RelativeSource.Mode%2A>, definida como <xref:System.Windows.Data.RelativeSourceMode.FindAncestor>.|
|`typeName`|Necessário para o modo `FindAncestor`. O nome de um tipo que preenche a propriedade <xref:System.Windows.Data.RelativeSource.AncestorType%2A>.|
|`intLevel`|Opcional para o modo `FindAncestor`. Um nível de ancestral (avaliado de acordo com a direção do pai na árvore lógica).|

## <a name="remarks"></a>Comentários

Usos de associação `{RelativeSource TemplatedParent}` são técnicas-chave que atendem a um conceito maior de separação de uma interface do usuário do controle e uma lógica do controle. Isso habilita a associação a partir da definição de modelo a um pai modelo (instância do objeto de tempo de execução em que o modelo é aplicado). Nesse caso, a [Extensão de Marcação TemplateBinding](templatebinding-markup-extension.md) é, na verdade, uma abreviação da seguinte expressão de associação: `{Binding RelativeSource={RelativeSource TemplatedParent}}`. Os usos `TemplateBinding` ou `{RelativeSource TemplatedParent}` são ambos relevantes apenas no XAML que define um modelo. Para obter mais informações, consulte [extensão de marcação TemplateBinding](templatebinding-markup-extension.md).

O `{RelativeSource FindAncestor}` é usado principalmente em modelos de controle ou em composições de interface do usuário independentes previsíveis para casos em que um controle deve estar sempre em uma árvore visual de um determinado tipo de ancestral. Por exemplo, os itens de um controle de itens podem utilizar os usos `FindAncestor` para se associar às propriedades do ancestral pai do controle de itens. Ou, os elementos que fazem parte da composição de controle em um modelo podem usar associações `FindAncestor` para elementos pai na mesma estrutura da composição.

Na sintaxe de elemento de objeto do modo `FindAncestor` exibida nas seções de Sintaxe XAML, a segunda sintaxe de elemento de objeto é usada especificamente para o modo `FindAncestor`. O modo `FindAncestor` requer um valor <xref:System.Windows.Data.RelativeSource.AncestorType%2A>. Você deve definir <xref:System.Windows.Data.RelativeSource.AncestorType%2A> como um atributo usando uma referência de [extensão de marcação x:Type](../../../desktop-wpf/xaml-services/xtype-markup-extension.md) para o tipo de ancestral a ser procurado. O valor <xref:System.Windows.Data.RelativeSource.AncestorType%2A> é usado quando a requisição de associação é processada em tempo de execução.

Para o modo `FindAncestor`, a propriedade opcional <xref:System.Windows.Data.RelativeSource.AncestorLevel%2A> pode ajudar a desambiguar a consulta de ancestral em casos em que exista possivelmente mais de um ancestral daquele tipo na árvore de elemento.

Para obter mais informações sobre como usar o modo `FindAncestor`, consulte <xref:System.Windows.Data.RelativeSource>.

`{RelativeSource Self}` é útil para cenários em que uma propriedade de uma instância deve depender do valor de outra propriedade da mesma instância e não existe nenhuma relação geral da propriedade de dependência (como a coerção) entre as duas propriedades. Embora seja raro duas propriedades existirem em um objeto de modo que os valores sejam literalmente idênticos (e tipados de modo idêntica), você também pode aplicar um parâmetro de `Converter` a uma associação que tenha `{RelativeSource Self}` e usar o conversor para converter entre os tipos de origem e de destino. Outro cenário do `{RelativeSource Self}` é como parte de um <xref:System.Windows.MultiDataTrigger> .

Por exemplo, o seguinte XAML define um elemento <xref:System.Windows.Shapes.Rectangle> de modo que não importa qual valor é inserido para <xref:System.Windows.FrameworkElement.Width%2A>, <xref:System.Windows.Shapes.Rectangle> sempre será um quadrado: `<Rectangle Width="200" Height="{Binding RelativeSource={RelativeSource Self}, Path=Width}" .../>`

O `{RelativeSource PreviousData}` é útil em modelos de dados ou em casos em que as associações estão usando uma coleção como a fonte de dados. Você pode usar `{RelativeSource PreviousData}` para realçar relacionamentos entre itens de dados adjacentes na coleção. Uma técnica relacionada é estabelecer um <xref:System.Windows.Data.MultiBinding> entre os itens atuais e anteriores na fonte de dados, e usar um conversor nessa associação para determinar a diferença entre os dois itens e suas propriedades.

No exemplo a seguir, o primeiro <xref:System.Windows.Controls.TextBlock> no modelo de itens exibe o número atual. A segunda <xref:System.Windows.Controls.TextBlock> associação é um que, de maneira <xref:System.Windows.Data.MultiBinding> nominal <xref:System.Windows.Data.Binding> , tem dois constituintes: o registro atual e uma associação que usa deliberadamente o registro de dados anterior usando `{RelativeSource PreviousData}` . Em seguida, um conversor em <xref:System.Windows.Data.MultiBinding> calcula a diferença e retorne à associação.

```xml
<ListBox Name="fibolist">
    <ListBox.ItemTemplate>
        <DataTemplate>
            <StackPanel Orientation="Horizontal">
            <TextBlock Text="{Binding}"/>
            <TextBlock>, difference = </TextBlock>
                <TextBlock>
                    <TextBlock.Text>
                        <MultiBinding Converter="{StaticResource DiffConverter}">
                            <Binding/>
                            <Binding RelativeSource="{RelativeSource PreviousData}"/>
                        </MultiBinding>
                    </TextBlock.Text>
                </TextBlock>
            </StackPanel>
        </DataTemplate>
    </ListBox.ItemTemplate>
</ListBox>
```

A descrição de vinculação de dados como um conceito não é abordado aqui; consulte [Visão geral de vinculação de dados](../../../desktop-wpf/data/data-binding-overview.md).

Na [!INCLUDE[TLA2#tla_winclient](../../../../includes/tla2sharptla-winclient-md.md)] implementação do processador XAML, o tratamento para essa extensão de marcação é definido pela <xref:System.Windows.Data.RelativeSource> classe.

`RelativeSource` é uma extensão da marcação. Extensões de marcação são tipicamente implementadas quando existe um requisito que permite que valores de atributo sejam diferentes de valores literais ou nomes de manipuladores, e o requisito é mais global do que simplesmente colocar conversores de tipo em certos tipos ou propriedades. Todas as extensões de marcação em XAML usam os caracteres `{` e `}` na sintaxe de atributo, que é a convenção pela qual o processador XAML reconhece que uma extensão de marcação precisa processar o atributo. Para obter mais informações, consulte [Extensões de marcação e XAML do WPF](markup-extensions-and-wpf-xaml.md).

## <a name="see-also"></a>Confira também

- <xref:System.Windows.Data.Binding>
- [Estilo e modelagem](../../../desktop-wpf/fundamentals/styles-templates-overview.md)
- [Visão geral de XAML (WPF)](../../../desktop-wpf/fundamentals/xaml.md)
- [Extensões de marcação e XAML WPF](markup-extensions-and-wpf-xaml.md)
- [Visão geral da ligação de dados](../../../desktop-wpf/data/data-binding-overview.md)
- [Visão geral de declarações de associação](../data/binding-declarations-overview.md)
- [Extensão de marcação x:Type](../../../desktop-wpf/xaml-services/xtype-markup-extension.md)
