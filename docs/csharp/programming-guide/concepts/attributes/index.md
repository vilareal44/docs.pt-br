---
title: Atributos (C#)
description: Saiba como usar atributos para associar metadados ou informações declarativas com código em C#. Um atributo pode ser consultado em tempo de execução usando reflexão.
ms.date: 04/26/2018
ms.openlocfilehash: 5c57838b649531d8e8fe89919771adf8830e7f54
ms.sourcegitcommit: 40de8df14289e1e05b40d6e5c1daabd3c286d70c
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/22/2020
ms.locfileid: "86924979"
---
# <a name="attributes-c"></a>Atributos (C#)

Os atributos fornecem um método eficiente de associação de metadados, ou informações declarativas, ao código (assemblies, tipos, métodos, propriedades e etc.). Após um atributo ser associado a uma entidade de programa, o atributo poderá ser consultado no tempo de execução usando uma técnica chamada *reflexão*. Para obter mais informações, consulte [Reflexão (C#)](../reflection.md).

Os atributos têm as seguintes propriedades:

- Os atributos adicionam metadados ao seu programa. Os *metadados* são informações sobre os tipos definidos em um programa. Todos os assemblies .NET contêm um conjunto de metadados especificado que descreve os tipos e os membros de tipo definidos no assembly. Você pode adicionar atributos personalizados para especificar qualquer informação adicional necessária. Para obter mais informações, consulte [Criando atributos personalizados (C#)](creating-custom-attributes.md).
- Você pode aplicar um ou mais atributos a assemblies completos, módulos ou elementos de programas menores, como classes e propriedades.
- Os atributos podem aceitar argumentos da mesma forma que métodos e propriedades.
- Seu programa pode examinar seus próprios metadados ou os metadados em outros programas usando reflexão. Para obter mais informações, consulte [Acessando atributos usando reflexão (C#)](accessing-attributes-by-using-reflection.md).

## <a name="using-attributes"></a>Usando atributos

Os atributos podem ser colocados em quase qualquer declaração, embora um atributo específico possa restringir os tipos de declarações nas quais ele é válido. No C#, você especifica um atributo colocando o nome do atributo entre colchetes ([]) acima da declaração da entidade à qual ele se aplica.

Neste exemplo, o atributo <xref:System.SerializableAttribute> é usado para aplicar uma característica específica a uma classe:

[!code-csharp[Using the serializable attribute](~/samples/snippets/csharp/attributes/AttributesOverview.cs#1)]

Um método com o atributo <xref:System.Runtime.InteropServices.DllImportAttribute> é declarado como este exemplo:

[!code-csharp[Declaring a method to import from an external library](../../../../../samples/snippets/csharp/attributes/AttributesOverview.cs#2)]

Mais de um atributo pode ser colocado em uma declaração como o seguinte exemplo mostra:

[!code-csharp[Including the interop namespace](~/samples/snippets/csharp/attributes/AttributesOverview.cs#3)]
[!code-csharp[Declaring two way marshaling for arguments](~/samples/snippets/csharp/attributes/AttributesOverview.cs#4)]

Alguns atributos podem ser especificados mais de uma vez para uma determinada entidade. Um exemplo de um atributo multiuso é <xref:System.Diagnostics.ConditionalAttribute>:

[!code-csharp[Using the conditional attribute](~/samples/snippets/csharp/attributes/AttributesOverview.cs#5)]

> [!NOTE]
> Por convenção, todos os nomes de atributo terminam com a palavra "Atributo" para distingui-los de outros itens nas bibliotecas do .NET. No entanto, você não precisa especificar o sufixo de atributo ao usar atributos no código. Por exemplo, `[DllImport]` é equivalente a `[DllImportAttribute]` , mas `DllImportAttribute` é o nome real do atributo na biblioteca de classes do .net.

### <a name="attribute-parameters"></a>Parâmetros de atributo

Muitos atributos têm parâmetros, que podem ser nomeados, sem nome ou posicionais. Quaisquer parâmetros de posição devem ser especificados em uma determinada ordem e não podem ser omitidos. Parâmetros nomeados são opcionais e podem ser especificados em qualquer ordem. Os parâmetros posicionais são especificados primeiro. Por exemplo, esses três atributos são equivalentes:

```csharp
[DllImport("user32.dll")]
[DllImport("user32.dll", SetLastError=false, ExactSpelling=false)]
[DllImport("user32.dll", ExactSpelling=false, SetLastError=false)]
```

O primeiro parâmetro, o nome da DLL, é posicional e sempre vir em primeiro lugar; os outros são nomeados. Nesse caso, ambos os parâmetros nomeados são padronizados como false e, portanto, podem ser omitidos. Parâmetros de posição correspondem aos parâmetros do construtor de atributo. Parâmetros nomeados ou opcionais correspondem a propriedades ou a campos do atributo. Consulte a documentação do atributo individual para obter informações sobre valores de parâmetro padrão.

### <a name="attribute-targets"></a>Destinos do atributo

O *destino* de um atributo é a entidade à qual o atributo se aplica. Por exemplo, um atributo pode ser aplicado a uma classe, um método específico ou um assembly inteiro. Por padrão, um atributo se aplica ao elemento que o segue. Mas você pode identificar explicitamente, por exemplo, se um atributo é aplicado a um método, ou a seu parâmetro ou a seu valor retornado.

Para identificar explicitamente um atributo de destino, use a seguinte sintaxe:

```csharp
[target : attribute-list]
```

A lista de possíveis valores `target` é mostrada na tabela a seguir.

|Valor de destino|Aplica-se a|
|------------------|----------------|
|`assembly`|Assembly inteiro|
|`module`|Módulo do assembly atual|
|`field`|Campo em uma classe ou um struct|
|`event`|Evento|
|`method`|Método ou acessadores de propriedade `get` e `set`|
|`param`|Parâmetros de método ou parâmetros de acessador de propriedade `set`|
|`property`|Propriedade|
|`return`|Valor retornado de um método, indexador de propriedade ou acessador de propriedade `get`|
|`type`|Struct, classe, interface, enum ou delegado|

Especifique o valor de destino `field` para aplicar um atributo ao campo de suporte criado para uma [propriedade autoimplementada](../../../properties.md).

O exemplo a seguir mostra como aplicar atributos a módulos e assemblies. Para obter mais informações, consulte [Atributos comuns (C#)](../../../language-reference/attributes/global.md).

```csharp
using System;
using System.Reflection;
[assembly: AssemblyTitleAttribute("Production assembly 4")]
[module: CLSCompliant(true)]
```

O exemplo a seguir mostra como aplicar atributos a métodos, parâmetros de método e valores de retorno de método em C#.

[!code-csharp[Applying attributes to different code elements](../../../../../samples/snippets/csharp/attributes/AttributesOverview.cs#6)]

> [!NOTE]
> Independentemente dos destinos nos quais `ValidatedContract` é definido como válido, o destino `return` deve ser especificado, mesmo se `ValidatedContract` forem definidos para serem aplicados somente a valores de retorno. Em outras palavras, o compilador não usará as informações de `AttributeUsage` para resolver os destinos de atributos ambíguos. Para obter mais informações, consulte [AttributeUsage (C#)](../../../language-reference/attributes/general.md).

## <a name="common-uses-for-attributes"></a>Usos comuns para atributos

A lista a seguir inclui alguns dos usos comuns de atributos no código:

- Marcar métodos usando o atributo `WebMethod` nos serviços Web para indicar que o método deve ser chamado por meio do protocolo SOAP. Para obter mais informações, consulte <xref:System.Web.Services.WebMethodAttribute>.
- Descrever como realizar marshaling de parâmetros de método ao interoperar com código nativo. Para obter mais informações, consulte <xref:System.Runtime.InteropServices.MarshalAsAttribute>.
- Descrever as propriedades COM para classes, métodos e interfaces.
- Chamar o código não gerenciado usando a classe <xref:System.Runtime.InteropServices.DllImportAttribute>.
- Descrever o assembly em termos de versão, título, descrição ou marca.
- Descrever quais membros de uma classe serializar para persistência.
- Descrever como fazer mapeamento entre nós XML e membros de classe para serialização de XML.
- Descrever os requisitos de segurança para métodos.
- Especificar as características usadas para impor a segurança.
- Controlar otimizações pelo compilador JIT (Just-In-Time) para que o código permaneça fácil de depurar.
- Obter informações sobre o chamador de um método.

## <a name="related-sections"></a>Seções relacionadas

Para obter mais informações, consulte:

- [Criando atributos personalizados (C#)](creating-custom-attributes.md)  
- [Acessando atributos usando reflexão (C#)](accessing-attributes-by-using-reflection.md)  
- [Como criar uma União C/C++ usando atributos (C#)](how-to-create-a-c-cpp-union-by-using-attributes.md)  
- [Atributos comuns (C#)](../../../language-reference/attributes/global.md)  
- [Informações do chamador (C#)](../../../language-reference/attributes/caller-information.md)  

## <a name="see-also"></a>Confira também

- [Guia de programação C#](../../index.md)
- [Reflexão (C#)](../reflection.md)
- [Atributos](../../../../standard/attributes/index.md)
- [Usando atributos em C #](../../../tutorials/attributes.md)
