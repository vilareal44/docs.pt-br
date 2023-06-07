---
title: Visão geral de atributos
ms.date: 07/20/2015
ms.assetid: 1449f69b-c063-41de-8d89-f0bbdcf96ac6
ms.openlocfilehash: a0a080d44796289cc3562803c84ec915dcedd314
ms.sourcegitcommit: f8c270376ed905f6a8896ce0fe25b4f4b38ff498
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/04/2020
ms.locfileid: "84400701"
---
# <a name="attributes-overview-visual-basic"></a>Visão geral de atributos (Visual Basic)

Os atributos fornecem um método eficiente de associação de metadados, ou informações declarativas, ao código (assemblies, tipos, métodos, propriedades e etc.). Após um atributo ser associado a uma entidade de programa, o atributo poderá ser consultado no tempo de execução usando uma técnica chamada *reflexão*. Para obter mais informações, consulte [Reflexão (Visual Basic)](../reflection.md).

Os atributos têm as seguintes propriedades:

- Os atributos adicionam metadados ao seu programa. Os *metadados* são informações sobre os tipos definidos em um programa. Todos os assemblies .NET contêm um conjunto de metadados especificado que descreve os tipos e os membros de tipo definidos no assembly. Você pode adicionar atributos personalizados para especificar qualquer informação adicional necessária. Para obter mais informações, consulte [Criando atributos personalizados (Visual Basic)](creating-custom-attributes.md).

- Você pode aplicar um ou mais atributos a assemblies completos, módulos ou elementos de programas menores, como classes e propriedades.

- Os atributos podem aceitar argumentos da mesma forma que métodos e propriedades.

- Seu programa pode examinar seus próprios metadados ou os metadados em outros programas usando reflexão. Para obter mais informações, consulte [Acessando atributos usando reflexão (Visual Basic)](accessing-attributes-by-using-reflection.md).

## <a name="using-attributes"></a>Usando atributos

Os atributos podem ser colocados em quase qualquer declaração, embora um atributo específico possa restringir os tipos de declarações nas quais ele é válido. No Visual Basic, um atributo é colocado entre colchetes angulares ( \< > ). Ele deverá aparecer imediatamente antes do elemento ao qual ele é aplicado, na mesma linha.

Neste exemplo, o atributo <xref:System.SerializableAttribute> é usado para aplicar uma característica específica a uma classe:

```vb
<System.Serializable()> Public Class SampleClass
    ' Objects of this type can be serialized.
End Class
```

 Um método com o atributo <xref:System.Runtime.InteropServices.DllImportAttribute> é declarado como este:

```vb
Imports System.Runtime.InteropServices
```

```vb
<System.Runtime.InteropServices.DllImport("user32.dll")>
Sub SampleMethod()
End Sub
```

Mais de um atributo pode ser colocado em uma declaração:

```vb
Imports System.Runtime.InteropServices
```

```vb
Sub MethodA(<[In](), Out()> ByVal x As Double)
End Sub
Sub MethodB(<Out(), [In]()> ByVal x As Double)
End Sub
```

Alguns atributos podem ser especificados mais de uma vez para uma determinada entidade. Um exemplo de um atributo multiuso é <xref:System.Diagnostics.ConditionalAttribute>:

```vb
<Conditional("DEBUG"), Conditional("TEST1")>
Sub TraceMethod()
End Sub
```

> [!NOTE]
> Por convenção, todos os nomes de atributo terminam com a palavra "Atributo" para distingui-los de outros itens no .NET Framework. No entanto, você não precisa especificar o sufixo de atributo ao usar atributos no código. Por exemplo, `[DllImport]` é equivalente a `[DllImportAttribute]`, mas `DllImportAttribute` é o nome do atributo real no .NET Framework.

### <a name="attribute-parameters"></a>Parâmetros de atributo

Muitos atributos têm parâmetros, que podem ser nomeados, sem nome ou posicionais. Quaisquer parâmetros posicionais devem ser especificados em uma determinada ordem e não podem ser omitidos. Os parâmetros nomeados são opcionais e podem ser especificados em qualquer ordem. Os parâmetros posicionais são especificados primeiro. Por exemplo, esses três atributos são equivalentes:

```vb
<DllImport("user32.dll")>
<DllImport("user32.dll", SetLastError:=False, ExactSpelling:=False)>
<DllImport("user32.dll", ExactSpelling:=False, SetLastError:=False)>
```

O primeiro parâmetro, o nome da DLL, é posicional e sempre vir em primeiro lugar; os outros são nomeados. Nesse caso, ambos os parâmetros nomeados são padronizados como false e, portanto, podem ser omitidos. Consulte a documentação do atributo individual para obter informações sobre valores de parâmetro padrão.

### <a name="attribute-targets"></a>Destinos de Atributos

O *destino* de um atributo é a entidade à qual o atributo se aplica. Por exemplo, um atributo pode ser aplicado a uma classe, um método específico ou um assembly inteiro. Por padrão, um atributo aplica-se ao elemento que ele precede. Mas você pode identificar explicitamente, por exemplo, se um atributo é aplicado a um método, ou a seu parâmetro ou a seu valor retornado.

Para identificar explicitamente um atributo de destino, use a seguinte sintaxe:

```vb
<target : attribute-list>
```

A lista de possíveis valores `target` é mostrada na tabela a seguir.

|Valor de destino|Aplica-se a|
|------------------|----------------|
|`assembly`|Assembly inteiro|
|`module`|Módulo de assembly atual (que é diferente de um módulo do Visual Basic)|

 O exemplo a seguir mostra como aplicar atributos a módulos e assemblies. Para obter mais informações, consulte [Atributos comuns (Visual Basic)](common-attributes.md).

```vb
Imports System.Reflection
<Assembly: AssemblyTitleAttribute("Production assembly 4"),
Module: CLSCompliant(True)>
```

## <a name="common-uses-for-attributes"></a>Usos comuns para os atributos

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

- [Criando atributos personalizados (Visual Basic)](creating-custom-attributes.md)

- [Acessando atributos usando reflexão (Visual Basic)](accessing-attributes-by-using-reflection.md)

- [Como criar uma união do C/C++ usando atributos (Visual Basic)](how-to-create-a-c-cpp-union-by-using-attributes.md)

- [Atributos comuns (Visual Basic)](common-attributes.md)

- [Informações do chamador (Visual Basic)](../caller-information.md)

## <a name="see-also"></a>Confira também

- [Guia de programação do Visual Basic](../../index.md)
- [Reflexão (Visual Basic)](../reflection.md)
- [Atributos](../../../../standard/attributes/index.md)
