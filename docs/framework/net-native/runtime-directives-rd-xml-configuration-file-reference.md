---
title: Referência do arquivo de configuração de diretivas do runtime (rd.xml)
ms.date: 03/30/2017
ms.assetid: 8241523f-d8e1-4fb6-bf6a-b29bfe07b38a
ms.openlocfilehash: e74d34693446cca645003a9f93bc1777849e3182
ms.sourcegitcommit: b16c00371ea06398859ecd157defc81301c9070f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/06/2020
ms.locfileid: "76738415"
---
# <a name="runtime-directives-rdxml-configuration-file-reference"></a>Referência do arquivo de configuração de diretivas do runtime (rd.xml)

O arquivo de diretivas de runtime (. rd.xml) é um arquivo de configuração XML que especifica se os elementos do programa designado estão disponíveis para reflexão. Aqui está um exemplo de um arquivo de diretivas de runtime:

```xml
<Directives xmlns="http://schemas.microsoft.com/netfx/2013/01/metadata">
  <Application>
    <Namespace Name="Contoso.Cloud.AppServices" Serialize="Required Public" />
    <Namespace Name="ContosoClient.ViewModels" Serialize="Required Public" />
    <Namespace Name="ContosoClient.DataModel" Serialize="Required Public" />
    <Namespace Name="Contoso.Reader.UtilityLib" Serialize="Required Public" />

    <Namespace Name="System.Collections.ObjectModel" >
      <TypeInstantiation Name="ObservableCollection"
            Arguments="ContosoClient.DataModel.ProductItem" Serialize="Public" />
      <TypeInstantiation Name="ReadOnlyObservableCollection"
            Arguments="ContosoClient.DataModel.ProductGroup" Serialize="Public" />
    </Namespace>
  </Application>
</Directives>
```

## <a name="the-structure-of-a-runtime-directives-file"></a>A estrutura de um arquivo de diretivas de runtime

O arquivo de diretivas de runtime usa o namespace `http://schemas.microsoft.com/netfx/2013/01/metadata`.

O elemento raiz é o elemento [Directives](directives-element-net-native.md). Ele pode conter zero ou mais elementos [Library](library-element-net-native.md) e zero ou um elemento [Application](application-element-net-native.md), conforme mostrado na estrutura a seguir. Os atributos do elemento [Application](application-element-net-native.md) podem definir a política de reflexão do runtime de todo o aplicativo ou podem servir como um contêiner para elementos filhos. O elemento [Library](library-element-net-native.md), por outro lado, é simplesmente um contêiner. Os filhos dos elementos [Application](application-element-net-native.md) e [Library](library-element-net-native.md) definem os tipos, métodos, campos, propriedades e eventos que estão disponíveis para reflexão.

Para obter informações de referência, escolha os elementos na estrutura de seguir ou consulte [Elementos da diretiva de runtime](runtime-directive-elements.md). Na seguinte hierarquia, as reticências marcam uma estrutura recursiva. As informações entre colchetes indicam se o elemento é necessário ou opcional e, se usado, quantas instâncias (uma ou várias) são permitidas.

- [Directives](directives-element-net-native.md) [1:1]
  - [Application](application-element-net-native.md) [0:1]
    - [Assembly](assembly-element-net-native.md) [0:M]
      - [Namespace](namespace-element-net-native.md) [0: M]. . .
      - [Digite](type-element-net-native.md) [0: M]. . .
      - [TypeInstantiation](typeinstantiation-element-net-native.md) (tipo genérico construído) [0: M]. . .
    - [Namespace](namespace-element-net-native.md) [0:M]
      - [Namespace](namespace-element-net-native.md) [0: M]. . .
      - [Digite](type-element-net-native.md) [0: M]. . .
      - [TypeInstantiation](typeinstantiation-element-net-native.md) (tipo genérico construído) [0: M]. . .
    - [Type](type-element-net-native.md) [0:M]
      - [Subtypes](subtypes-element-net-native.md) (subclasses do tipo recipiente) [O:1]
      - [Digite](type-element-net-native.md) [0: M]. . .
      - [TypeInstantiation](typeinstantiation-element-net-native.md) (tipo genérico construído) [0: M]. . .
      - [AttributeImplies](attributeimplies-element-net-native.md) (o tipo recipiente é um atributo) [O:1]
      - [GenericParameter](genericparameter-element-net-native.md) [0:M]
      - [Method](method-element-net-native.md) [0:M]
        - [Parameter](parameter-element-net-native.md) [0:M]
        - [TypeParameter](typeparameter-element-net-native.md) [0:M]
        - [GenericParameter](genericparameter-element-net-native.md) [0:M]
      - [MethodInstantiation](methodinstantiation-element-net-native.md) (método genérico construído) [0:M]
      - [Property](property-element-net-native.md) [0:M]
      - [Field](field-element-net-native.md) [0:M]
      - [Event](event-element-net-native.md) [0:M]
    - [TypeInstantiation](typeinstantiation-element-net-native.md) (tipo genérico construído) [0:M]
      - [Digite](type-element-net-native.md) [0: M]. . .
      - [TypeInstantiation](typeinstantiation-element-net-native.md) (tipo genérico construído) [0: M]. . .
      - [Method](method-element-net-native.md) [0:M]
        - [Parameter](parameter-element-net-native.md) [0:M]
        - [TypeParameter](typeparameter-element-net-native.md) [0:M]
        - [GenericParameter](genericparameter-element-net-native.md) [0:M]
      - [MethodInstantiation](methodinstantiation-element-net-native.md) (método genérico construído) [0:M]
      - [Property](property-element-net-native.md) [0:M]
      - [Field](field-element-net-native.md) [0:M]
      - [Event](event-element-net-native.md) [0:M]
  - [Library](library-element-net-native.md) [0:M]
    - [Assembly](assembly-element-net-native.md) [0:M]
      - [Namespace](namespace-element-net-native.md) [0: M]. . .
      - [Digite](type-element-net-native.md) [0: M]. . .
      - [TypeInstantiation](typeinstantiation-element-net-native.md) (tipo genérico construído) [0: M]. . .
    - [Namespace](namespace-element-net-native.md) [0:M]
      - [Namespace](namespace-element-net-native.md) [0: M]. . .
      - [Digite](type-element-net-native.md) [0: M]. . .
      - [TypeInstantiation](typeinstantiation-element-net-native.md) (tipo genérico construído) [0: M]. . .
    - [Type](type-element-net-native.md) [0:M]
      - [Subtypes](subtypes-element-net-native.md) (subclasses do tipo recipiente) [O:1]
      - [Digite](type-element-net-native.md) [0: M]. . .
      - [TypeInstantiation](typeinstantiation-element-net-native.md) (tipo genérico construído) [0: M]. . .
      - [AttributeImplies](attributeimplies-element-net-native.md) (o tipo recipiente é um atributo) [O:1]
      - [GenericParameter](genericparameter-element-net-native.md) [0:M]
      - [Method](method-element-net-native.md) [0:M]
      - [MethodInstantiation](methodinstantiation-element-net-native.md) (método genérico construído) [0:M]
      - [Property](property-element-net-native.md) [0:M]
      - [Field](field-element-net-native.md) [0:M]
      - [Event](event-element-net-native.md) [0:M]
    - [TypeInstantiation](typeinstantiation-element-net-native.md) (tipo genérico construído) [0:M]
      - [Digite](type-element-net-native.md) [0: M]. . .
      - [TypeInstantiation](typeinstantiation-element-net-native.md) (tipo genérico construído) [0: M]. . .
      - [Method](method-element-net-native.md) [0:M]
      - [MethodInstantiation](methodinstantiation-element-net-native.md) (método genérico construído) [0:M]
      - [Property](property-element-net-native.md) [0:M]
      - [Field](field-element-net-native.md) [0:M]
      - [Event](event-element-net-native.md) [0:M]

O elemento [Application](application-element-net-native.md) pode não ter nenhum atributo ou pode ter os atributos de política discutidos na [seção de Política e diretiva de runtime](#Directives).

Um elemento [Library](library-element-net-native.md) tem um único atributo, `Name`, que especifica o nome de uma biblioteca ou um assembly, sem a extensão do arquivo. Por exemplo, o elemento [Library](library-element-net-native.md) a seguir aplica-se a um assembly denominado Extensions.dll.

```xml
<Directives xmlns="http://schemas.microsoft.com/netfx/2013/01/metadata">
  <Application>
     <!-- Child elements go here -->
  </Application>
  <Library Name="Extensions">
     <!-- Child elements go here -->
  </Library>
</Directives>
```

<a name="Directives"></a>

## <a name="runtime-directives-and-policy"></a>Política e diretivas de runtime

O próprio elemento [Application](application-element-net-native.md) e os elementos filhos dos elementos [Library](library-element-net-native.md) e [Application](application-element-net-native.md) expressam a política, ou seja, definem a maneira pela qual o aplicativo pode aplicar reflexão a um elemento do programa. O tipo de política é definido por um atributo do elemento (por exemplo, `Serialize`). O valor de política é definido pelo valor do atributo (por exemplo, `Serialize="Required"`).

Qualquer política especificada por um atributo de um elemento aplica-se a todos os elementos filhos que não especifique um valor para essa política. Por exemplo, se uma política é especificada por um elemento [Type](type-element-net-native.md), a política aplica-se a todos os tipos e membros contidos para os quais uma política não foi especificada explicitamente.

A política que pode ser expressa pelos elementos [Application](application-element-net-native.md), [Assembly](assembly-element-net-native.md), [AttributeImplies](attributeimplies-element-net-native.md), [Namespace](namespace-element-net-native.md), [Subtypes](subtypes-element-net-native.md) e [Type](type-element-net-native.md) diferem daquela que pode ser expressa para membros individuais (pelos elementos [Method](method-element-net-native.md), [Property](property-element-net-native.md), [Field](field-element-net-native.md) e [Event](event-element-net-native.md)).

### <a name="specifying-policy-for-assemblies-namespaces-and-types"></a>Especificando a política para assemblies, namespaces e tipos

Os elementos [Application](application-element-net-native.md), [Assembly](assembly-element-net-native.md), [AttributeImplies](attributeimplies-element-net-native.md), [Namespace](namespace-element-net-native.md), [Subtypes](subtypes-element-net-native.md) e [Type](type-element-net-native.md) dão suporte aos seguintes tipos de política:

- `Activate`. Controla o acesso de runtime a construtores para habilitar a ativação de instâncias.

- `Browse`. Controla as consultas para obter informações sobre elementos do programa, mas não permite qualquer acesso do runtime.

- `Dynamic`. Controla o acesso a todos os tipos de membro ao runtime, incluindo construtores, métodos, campos, propriedades e eventos, habilitando a programação dinâmica.

- `Serialize`. Controla o acesso ao runtime para construtores, campos e propriedades para habilitar a serialização por bibliotecas de terceiros como o serializador Newtonsoft JSON.

- `DataContractSerializer`. Controla a política de serialização que usa a classe <xref:System.Runtime.Serialization.DataContractSerializer?displayProperty=nameWithType>.

- `DataContractJsonSerializer`. Controla a política de serialização JSON que usa a classe <xref:System.Runtime.Serialization.DataContractSerializer?displayProperty=nameWithType>.

- `XmlSerializer`. Controla a política de serialização XML que usa a classe <xref:System.Xml.Serialization.XmlSerializer?displayProperty=nameWithType>.

- `MarshalObject`. Controla a política de marshaling de tipos de referência do WinRT e COM.

- `MarshalDelegate`. Controla a diretiva de marshaling de tipos delegados como ponteiros de função para código nativo.

- `MarshalStructure` . Controla a política de estruturas de marshaling para código nativo.

As configurações associadas a esses tipos de política são:

- `All`. Habilite a política para todos os tipos e membros que a cadeia de ferramentas não remover.

- `Auto`. Use o comportamento padrão. (Não especificar uma política é equivalente à configurar a política para `Auto`, a menos que essa política seja substituída, por exemplo, por um elemento pai.)

- `Excluded`. Desabilite a política para o elemento de programa.

- `Public`. Habilite a política para tipos públicos ou membros, a menos que a cadeia de ferramentas determine que o membro é desnecessário e, portanto, remove-o. (No segundo caso, você deve usar `Required Public` para garantir que o membro é mantido e tem recursos de reflexão.)

- `PublicAndInternal`. Habilite a política para tipos ou membros internos e públicos se a cadeia de ferramenta não removê-los.

- `Required Public`. Necessita da cadeia de ferramentas para manter os tipos e membros públicos, sejam usados ou não, e habilita a política para eles.

- `Required PublicAndInternal`. Necessita da cadeia de ferramentas para manter os tipos e membros públicos e internos, sejam usados ou não, e habilita a política para eles.

- `Required All`. Necessita da cadeia de ferramentas para manter todos os tipos e membros públicos, sejam usados ou não, e habilita a política para eles.

Por exemplo, o seguinte arquivo de diretivas de runtime define a política para todos os tipos e membros no assembly DataClasses.dll. Ele habilita a reflexão para serialização de todas as propriedades públicas, permite procurar todos os tipos e membros de tipo, permite a ativação de todos os tipos de (devido ao atributo `Dynamic`) e permite a reflexão para todos os tipos e membros públicos.

```xml
<Directives xmlns="http://schemas.microsoft.com/netfx/2013/01/metadata">
   <Application>
      <Assembly Name="DataClasses" Serialize="Required Public"
                Browse="All" Activate="PublicAndInternal"
                Dynamic="Public"  />
   </Application>
   <Library Name="UtilityLibrary">
     <!-- Child elements go here -->
   </Library>
</Directives>
```

### <a name="specifying-policy-for-members"></a>Especificando a política para membros

Os elementos [Property](property-element-net-native.md) e [Field](field-element-net-native.md) dão suporte aos seguintes tipos de política:

- `Browse` - Controla as consultas para obter informações este membro, mas não permite qualquer acesso do runtime.

- `Dynamic` - Controla o acesso a todos os tipos de membro ao runtime, incluindo construtores, métodos, campos, propriedades e eventos, habilitando a programação dinâmica. Também controla a consulta para obter informações sobre o tipo recipiente.

- `Serialize` - Controla o acesso de runtime ao membro para habilitar as instâncias do tipo a serem serializadas e desserializadas por bibliotecas como o serializador Newtonsoft JSON. Esta política pode ser aplicada a construtores, campos e propriedades.

Os elementos [Method](method-element-net-native.md) e [Event](event-element-net-native.md) dão suporte aos seguintes tipos de política:

- `Browse` - Controla as consultas para obter informações este membro, mas não permite qualquer acesso do runtime.

- `Dynamic` - Controla o acesso a todos os tipos de membro ao runtime, incluindo construtores, métodos, campos, propriedades e eventos, habilitando a programação dinâmica. Também controla a consulta para obter informações sobre o tipo recipiente.

 As configurações associadas a esses tipos de política são:

- `Auto` - Use o comportamento padrão. (Não especificar uma política é equivalente a configurar essa política como `Auto`, a menos que algo substitua-a.)

- `Excluded` - Nunca inclua metadados para o membro.

- `Included` - Habilite a política se o tipo pai estiver presente na saída.

- `Required` - Necessita que a cadeia de ferramentas mantenha esse membro mesmo se parece ser não usados e habilita a política para ele.

## <a name="runtime-directives-file-semantics"></a>Semântica do arquivo de diretivas de runtime

A política pode ser definida simultaneamente para elementos de nível superior e de nível inferior. Por exemplo, a política pode ser definida para um assembly e para alguns dos tipos contidos neste assembly. Se um determinado elemento de nível inferior não for representado, ele herdará a política do pai. Por exemplo, se um elemento `Assembly` estiver presente, mas elementos `Type` não estiverem, a política especificada no elemento `Assembly` se aplica a cada tipo no assembly. Vários elementos também podem aplicar a política ao mesmo elemento de programa. Por exemplo, diferentes elementos [Assembly](assembly-element-net-native.md) podem definir o mesmo elemento de política para o mesmo assembly diferentemente. As seções a seguir explicam como a política para um determinado tipo é resolvida nesses casos.

Um elemento [Type](type-element-net-native.md) ou [Method](method-element-net-native.md) de um tipo ou método genérico aplica sua política a todas as instanciações que não têm sua própria política. Por exemplo, um elemento `Type` que especifica a política para <xref:System.Collections.Generic.List%601> aplica-se a todas as instâncias construídas desse tipo genérico, a menos que seja substituído por um determinado tipo genérico construído (como um `List<Int32>`) por um elemento `TypeInstantiation`. Caso contrário, os elementos definem a política para o elemento de programa denominado.

Quando um elemento é ambíguo, o mecanismo procura correspondências e, se encontrar uma correspondência exata, ele a usará. Se ele encontrar várias correspondências, haverá um erro ou aviso.

### <a name="if-two-directives-apply-policy-to-the-same-program-element"></a>Se as duas diretivas aplicam a política ao mesmo elemento de programa

Se dois elementos em arquivos de diretivas de runtime diferentes tentarem definir o mesmo tipo de política para o mesmo elemento de programa (como um assembly ou tipo) com valores diferentes, o conflito é resolvido da seguinte maneira:

1. Se o elemento `Excluded` estiver presente, ele tem precedência.

2. `Required` tem precedência sobre não `Required`.

3. `All` tem precedência sobre `PublicAndInternal`, que tem precedência sobre `Public`.

4. Qualquer configuração explícita tem precedência sobre `Auto`.

Por exemplo, se um único projeto inclui os dois seguintes arquivos de diretivas de runtime, a política de serialização para DataClasses.dll é definida para `Required Public` e `All`. Nesse caso, a política de serialização seria resolvida como `Required All`.

```xml
<Directives xmlns="http://schemas.microsoft.com/netfx/2013/01/metadata">
   <Application>
      <Assembly Name="DataClasses" Serialize="Required Public"/>
   </Application>
   <Library Name="DataClasses">
      <!-- any other elements -->
   </Library>
</Directives>
```

```xml
<Directives xmlns="http://schemas.microsoft.com/netfx/2013/01/metadata">
   <Application>
      <Assembly Name="DataClasses" Serialize="All" />
   </Application>
   <Library Name="DataClasses">
      <!-- any other elements -->
   </Library>
</Directives>
```

No entanto, se duas diretivas em um mesmo arquivo de diretivas de runtime tentarem definir o mesmo tipo de política para o mesmo elemento de programa, a ferramenta de Definição de Esquema de XML exibe uma mensagem de erro.

### <a name="if-child-and-parent-elements-apply-the-same-policy-type"></a>Se os elementos filho e pai aplicam o mesmo tipo de política

Elementos filhos substituem seus elementos pai, incluindo a configuração `Excluded`. A substituição é o principal motivo pelo qual você desejaria especificar `Auto`.

No exemplo a seguir, a configuração da política de serialização para tudo no `DataClasses` não presente em `DataClasses.ViewModels` seria `Required Public`, e tudo que está presente em ambos `DataClasses` e `DataClasses.ViewModels` seria `All`.

```xml
<Directives xmlns="http://schemas.microsoft.com/netfx/2013/01/metadata">
   <Application>
      <Assembly Name="DataClasses" Serialize="Required Public" >
         <Namespace Name="DataClasses.ViewModels" Serialize="All" />
      </Assembly>
   </Application>
   <Library Name="DataClasses">
      <!-- any other elements -->
   </Library>
</Directives>
```

### <a name="if-open-generics-and-instantiated-elements-apply-the-same-policy-type"></a>Se elementos abertos genéricos e instanciados aplicarem o mesmo tipo de política

No exemplo a seguir, `Dictionary<int,int>` recebe a política `Browse` somente se o mecanismo tiver outro motivo para conferir a política `Browse` (que seria o comportamento padrão); todas as outras instanciações do <xref:System.Collections.Generic.Dictionary%602> terão todos os seus membros pesquisáveis.

```xml
<Directives xmlns="http://schemas.microsoft.com/netfx/2013/01/metadata">
   <Application>
      <Assembly Name="DataClasses" Serialize="Required Public" >
         <Namespace Name="DataClasses.ViewModels" Serialize="All" />
      </Assembly>
      <Namespace Name="DataClasses.Generics" />
      <Type Name="Dictionary" Browse="All" />
      <TypeInstantiation Name="Dictionary"
                         Arguments="System.Int32,System.Int32" Browse="Auto" />
   </Application>
   <Library Name="DataClasses">
      <!-- any other elements -->
   </Library>
</Directives>
```

### <a name="how-policy-is-inferred"></a>Como a política é deduzida

Cada tipo de política possui um conjunto de regras diferente que determina como a presença desse tipo de política afeta outros constructos.

#### <a name="the-effect-of-browse-policy"></a>O efeito da política Browse

Aplicar a política `Browse` a um tipo envolve as seguintes alterações de política:

- O tipo base do tipo é marcado com a política `Browse`.

- Se o tipo for um tipo genérico instanciado, a versão não instanciada do tipo é marcada com a política `Browse`.

- Se o tipo for um delegado, o método `Invoke` no tipo é marcado com a política `Dynamic`.

- Cada interface do tipo é marcada com a política `Browse`.

- O tipo de cada atributo aplicado ao tipo é marcado com a política `Browse`.

- Se o tipo for genérico, cada tipo de restrição é marcado com a política `Browse`.

- Se o tipo for genérico, os tipos pelos quais o tipo é instanciado são marcados com a política `Browse`.

Aplicar a política `Browse` a um método envolve as seguintes alterações de política:

- Cada tipo de parâmetro do método é marcado com a política `Browse`.

- O tipo de retorno do método é marcado com a política `Browse`.

- O tipo recipiente do método é marcado com a política `Browse`.

- Se o método for um método genérico instanciado, o método genérico não instanciado é marcado com a política `Browse`.

- O tipo de cada atributo aplicado ao método é marcado com a política `Browse`.

- Se o método for genérico, cada tipo de restrição é marcado com a política `Browse`.

- Se o método for genérico, os tipos pelos quais o método é instanciado são marcados com a política `Browse`.

Aplicar a política `Browse` a um campo envolve as seguintes alterações de política:

- O tipo de cada atributo aplicado ao campo é marcado com a política `Browse`.

- O tipo do campo é marcado com a política `Browse`.

- O tipo ao qual o campo pertence é marcado com a política `Browse`.

#### <a name="the-effect-of-dynamic-policy"></a>O efeito da política Dynamic

Aplicar a política `Dynamic` a um tipo envolve as seguintes alterações de política:

- O tipo base do tipo é marcado com a política `Dynamic`.

- Se o tipo for um tipo genérico instanciado, a versão não instanciada do tipo é marcada com a política `Dynamic`.

- Se o tipo for um tipo delegado, o método `Invoke` no tipo é marcado com a política `Dynamic`.

- Cada interface do tipo é marcada com a política `Browse`.

- O tipo de cada atributo aplicado ao tipo é marcado com a política `Browse`.

- Se o tipo for genérico, cada tipo de restrição é marcado com a política `Browse`.

- Se o tipo for genérico, os tipos pelos quais o tipo é instanciado são marcados com a política `Browse`.

Aplicar a política `Dynamic` a um método envolve as seguintes alterações de política:

- Cada tipo de parâmetro do método é marcado com a política `Browse`.

- O tipo de retorno do método é marcado com a política `Dynamic`.

- O tipo recipiente do método é marcado com a política `Dynamic`.

- Se o método for um método genérico instanciado, o método genérico não instanciado é marcado com a política `Browse`.

- O tipo de cada atributo aplicado ao método é marcado com a política `Browse`.

- Se o método for genérico, cada tipo de restrição é marcado com a política `Browse`.

- Se o método for genérico, os tipos pelos quais o método é instanciado são marcados com a política `Browse`.

- O método pode ser invocado por `MethodInfo.Invoke`, e a criação de delegado é possibilitada por <xref:System.Reflection.MethodInfo.CreateDelegate%2A?displayProperty=nameWithType>.

Aplicar a política `Dynamic` a um campo envolve as seguintes alterações de política:

- O tipo de cada atributo aplicado ao campo é marcado com a política `Browse`.

- O tipo do campo é marcado com a política `Dynamic`.

- O tipo ao qual o campo pertence é marcado com a política `Dynamic`.

#### <a name="the-effect-of-activation-policy"></a>O efeito da política Activation

Aplicar a política de Ativação a um tipo envolve as seguintes alterações de política:

- Se o tipo for um tipo genérico instanciado, a versão não instanciada do tipo é marcada com a política `Browse`.

- Se o tipo for um tipo delegado, o método `Invoke` no tipo é marcado com a política `Dynamic`.

- Construtores de tipo são marcados com a política `Activation`.

Aplicar a política `Activation` a um método envolve as seguintes alterações de política:

- O construtor pode ser invocado pelos métodos <xref:System.Reflection.ConstructorInfo.Invoke%2A?displayProperty=nameWithType> e <xref:System.Activator.CreateInstance%2A?displayProperty=nameWithType>. Para métodos, a política `Activation` afeta apenas construtores.

Aplicar a política `Activation` a um campo não tem efeito.

#### <a name="the-effect-of-serialize-policy"></a>O efeito da política Serialize

A política `Serialize` permite o uso de serializadores comuns baseados em reflexão. No entanto, como os padrões de acesso de reflexão exata de serializadores não Microsoft não são conhecidos pela Microsoft, essa política pode não ser totalmente eficiente.

Aplicar a política `Serialize` a um tipo envolve as seguintes alterações de política:

- O tipo base do tipo é marcado com a política `Serialize`.

- Se o tipo for um tipo genérico instanciado, a versão não instanciada do tipo é marcada com a política `Browse`.

- Se o tipo for um tipo delegado, o método `Invoke` no tipo é marcado com a política `Dynamic`.

- Se o tipo é uma enumeração, uma matriz do tipo é marcada com a política `Serialize`.

- Se o tipo implementa <xref:System.Collections.Generic.IEnumerable%601>, `T` é marcada com a política `Serialize`.

- Se o tipo for <xref:System.Collections.Generic.IEnumerable%601>, <xref:System.Collections.Generic.IList%601>, <xref:System.Collections.Generic.ICollection%601>, <xref:System.Collections.Generic.IReadOnlyCollection%601> ou <xref:System.Collections.Generic.IReadOnlyList%601>, `T[]` e <xref:System.Collections.Generic.List%601> são marcados com a política `Serialize`, mas nenhum membro do tipo de interface é marcado com a política `Serialize`.

- Se o tipo for <xref:System.Collections.Generic.List%601>, nenhum membro do tipo é marcado com a política `Serialize`.

- Se o tipo for <xref:System.Collections.Generic.IDictionary%602>, <xref:System.Collections.Generic.Dictionary%602> é marcado com a política `Serialize`. mas nenhum membro do tipo é marcado com a política `Serialize`.

- Se o tipo for <xref:System.Collections.Generic.Dictionary%602>, nenhum membro do tipo é marcado com a política `Serialize`.

- Se o tipo implementa <xref:System.Collections.Generic.IDictionary%602>, `TKey` e `TValue` são marcados com a política `Serialize`.

- Cada construtor, cada acesso de propriedade e cada campo é marcado com a política `Serialize`.

Aplicar a política `Serialize` a um método envolve as seguintes alterações de política:

- O tipo recipiente estiver marcado com a política `Serialize`.

- O tipo de retorno do método é marcado com a política `Serialize`.

Aplicar a política `Serialize` a um campo envolve as seguintes alterações de política:

- O tipo recipiente estiver marcado com a política `Serialize`.

- O tipo do campo é marcado com a política `Serialize`.

#### <a name="the-effect-of-xmlserializer-datacontractserializer-and-datacontractjsonserializer-policies"></a>O efeito das políticas XmlSerializer, DataContractSerializer e DataContractJsonSerializer

Ao contrário da `Serialize` política, que é destinada a serializadores baseados em reflexão, as <xref:System.Xml.Serialization.XmlSerializer> <xref:System.Runtime.Serialization.DataContractSerializer> políticas, e <xref:System.Runtime.Serialization.Json.DataContractJsonSerializer> são usadas para habilitar um conjunto de serializadores que são conhecidos para a cadeia de ferramentas de .net Native. Esses serializadores não são implementados usando reflexão, mas o conjunto de tipos que pode ser serializado no tempo de execução é determinado de maneira semelhante, como tipos reflexíveis.

Aplicar de uma dessas políticas a um tipo permite que o tipo seja serializado com o serializador correspondente. Além disso, quaisquer tipos que o mecanismo de serialização determinar estaticamente como necessitando serialização também serão serializáveis.

Essas políticas não têm efeito nos campos ou métodos.

Para obter mais informações, consulte a seção "Diferenças em serializadores" em [Migrar seu aplicativo da Windows Store para .NET Native](migrating-your-windows-store-app-to-net-native.md).

## <a name="see-also"></a>Confira também

- [Elementos da diretiva de runtime](runtime-directive-elements.md)
- [Reflexão e .NET Nativo](reflection-and-net-native.md)
