---
title: Marshaling padrão para objetos
description: Entender o marshaling padrão de objetos. Examine as opções de marshaling. Realize marshaling de objetos para interfaces ou variantes, variantes para objetos e variantes de ByRef.
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
helpviewer_keywords:
- objects, interop marshaling
- interop marshaling, objects
ms.assetid: c2ef0284-b061-4e12-b6d3-6a502b9cc558
ms.openlocfilehash: 7b8f94f4dfd8e8b9e8e04df8de5f8266a8581a92
ms.sourcegitcommit: e02d17b2cf9c1258dadda4810a5e6072a0089aee
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/01/2020
ms.locfileid: "85618441"
---
# <a name="default-marshaling-for-objects"></a>Marshaling padrão para objetos

Os parâmetros e os campos tipados como <xref:System.Object?displayProperty=nameWithType> podem ser expostos para um código não gerenciado como um dos seguintes tipos:

- Uma variante quando o objeto é um parâmetro.

- Uma interface quando o objeto é um campo de estrutura.

Somente a interoperabilidade COM dá suporte ao marshaling de tipos de objeto. O comportamento padrão é realizar marshaling de objetos para variantes COM. Essas regras se aplicam somente ao tipo **Object** e não se aplicam a objetos fortemente tipados derivados da classe **Object**.

## <a name="marshaling-options"></a>Opções de marshaling

A tabela a seguir mostra as opções de marshaling para o tipo de dados **Object**. O atributo <xref:System.Runtime.InteropServices.MarshalAsAttribute> fornece vários valores de enumeração <xref:System.Runtime.InteropServices.UnmanagedType> para realizar marshaling de objetos.

|Tipo de enumeração|Descrição do formato não gerenciado|
|----------------------|-------------------------------------|
|**UnmanagedType.Struct**<br /><br /> (padrão para parâmetros)|Uma variante de estilo COM.|
|**UnmanagedType.Interface**|Uma interface **IDispatch**, se possível; caso contrário, uma interface **IUnknown**.|
|**UnmanagedType.IUnknown**<br /><br /> (padrão para campos)|Uma interface **IUnknown**.|
|**UnmanagedType.IDispatch**|Uma interface **IDispatch**.|

O exemplo a seguir mostra a definição de interface gerenciada para `MarshalObject`.

```vb
Interface MarshalObject
   Sub SetVariant(o As Object)
   Sub SetVariantRef(ByRef o As Object)
   Function GetVariant() As Object

   Sub SetIDispatch( <MarshalAs(UnmanagedType.IDispatch)> o As Object)
   Sub SetIDispatchRef(ByRef <MarshalAs(UnmanagedType.IDispatch)> o _
      As Object)
   Function GetIDispatch() As <MarshalAs(UnmanagedType.IDispatch)> Object
   Sub SetIUnknown( <MarshalAs(UnmanagedType.IUnknown)> o As Object)
   Sub SetIUnknownRef(ByRef <MarshalAs(UnmanagedType.IUnknown)> o _
      As Object)
   Function GetIUnknown() As <MarshalAs(UnmanagedType.IUnknown)> Object
End Interface
```

```csharp
interface MarshalObject {
   void SetVariant(Object o);
   void SetVariantRef(ref Object o);
   Object GetVariant();

   void SetIDispatch ([MarshalAs(UnmanagedType.IDispatch)]Object o);
   void SetIDispatchRef([MarshalAs(UnmanagedType.IDispatch)]ref Object o);
   [MarshalAs(UnmanagedType.IDispatch)] Object GetIDispatch();
   void SetIUnknown ([MarshalAs(UnmanagedType.IUnknown)]Object o);
   void SetIUnknownRef([MarshalAs(UnmanagedType.IUnknown)]ref Object o);
   [MarshalAs(UnmanagedType.IUnknown)] Object GetIUnknown();
}
```

O código a seguir exporta a interface `MarshalObject` para uma biblioteca de tipos.

```cpp
interface MarshalObject {
   HRESULT SetVariant([in] VARIANT o);
   HRESULT SetVariantRef([in,out] VARIANT *o);
   HRESULT GetVariant([out,retval] VARIANT *o)
   HRESULT SetIDispatch([in] IDispatch *o);
   HRESULT SetIDispatchRef([in,out] IDispatch **o);
   HRESULT GetIDispatch([out,retval] IDispatch **o)
   HRESULT SetIUnknown([in] IUnknown *o);
   HRESULT SetIUnknownRef([in,out] IUnknown **o);
   HRESULT GetIUnknown([out,retval] IUnknown **o)
}
```

> [!NOTE]
> O marshaler de interoperabilidade libera automaticamente qualquer objeto alocado dentro na variante após a chamada.

O exemplo a seguir mostra um tipo de valor formatado.

```vb
Public Structure ObjectHolder
   Dim o1 As Object
   <MarshalAs(UnmanagedType.IDispatch)> Public o2 As Object
End Structure
```

```csharp
public struct ObjectHolder {
   Object o1;
   [MarshalAs(UnmanagedType.IDispatch)]public Object o2;
}
```

O código a seguir exporta o tipo formatado para uma biblioteca de tipos.

```cpp
struct ObjectHolder {
   VARIANT o1;
   IDispatch *o2;
}
```

## <a name="marshaling-object-to-interface"></a>Realizando marshaling de objeto para interface

Quando um objeto é exposto para o COM como uma interface, essa interface é a interface de classe do tipo gerenciado <xref:System.Object> (a interface **_Object**). Essa interface é digitada como um **IDispatch** ( <xref:System.Runtime.InteropServices.UnmanagedType> ) ou um **IUnknown** (**UnmanagedType. IUnknown**) na biblioteca de tipos resultante. Os clientes COM podem invocar dinamicamente os membros da classe gerenciada ou todos os membros implementados por suas classes derivadas por meio da interface **_Object**. O cliente também pode chamar **QueryInterface** para obter qualquer outra interface implementada explicitamente pelo tipo gerenciado.

## <a name="marshaling-object-to-variant"></a>Realizando marshaling de objeto para variante

Quando um objeto tem o marshaling realizado como uma variante, o tipo de variante interno é determinado em tempo de execução, de acordo com as seguintes regras:

- Se a referência de objeto for nula (**Nothing** no Visual Basic), o objeto terá o marshaling realizado como uma variante do tipo **VT_EMPTY**.

- Se o objeto for uma instância de qualquer tipo listado na tabela a seguir, o tipo de variante resultante será determinado pelas regras internas do marshaler e mostrado na tabela.

- Outros objetos que precisam controlar explicitamente o comportamento de marshaling podem implementar a interface <xref:System.IConvertible>. Nesse caso, o tipo de variante é determinado pelo código do tipo retornado do método <xref:System.IConvertible.GetTypeCode%2A?displayProperty=nameWithType>. Caso contrário, o objeto terá o marshaling realizado como uma variante do tipo **VT_UNKNOWN**.

### <a name="marshaling-system-types-to-variant"></a>Realizando marshaling de tipos do sistema para variante

A tabela a seguir mostra os tipos de objeto gerenciados e seus tipos de variante COM correspondentes. Esses tipos são convertidos somente quando a assinatura do método que está sendo chamado é do tipo <xref:System.Object?displayProperty=nameWithType>.

|Tipo de objeto|Tipo de variante COM|
|-----------------|----------------------|
|Referência de objeto nulo (**Nothing** no Visual Basic).|**VT_EMPTY**|
|<xref:System.DBNull?displayProperty=nameWithType>|**VT_NULL**|
|<xref:System.Runtime.InteropServices.ErrorWrapper?displayProperty=nameWithType>|**VT_ERROR**|
|<xref:System.Reflection.Missing?displayProperty=nameWithType>|**VT_ERROR** com **E_PARAMNOTFOUND**|
|<xref:System.Runtime.InteropServices.DispatchWrapper?displayProperty=nameWithType>|**VT_DISPATCH**|
|<xref:System.Runtime.InteropServices.UnknownWrapper?displayProperty=nameWithType>|**VT_UNKNOWN**|
|<xref:System.Runtime.InteropServices.CurrencyWrapper?displayProperty=nameWithType>|**VT_CY**|
|<xref:System.Boolean?displayProperty=nameWithType>|**VT_BOOL**|
|<xref:System.SByte?displayProperty=nameWithType>|**VT_I1**|
|<xref:System.Byte?displayProperty=nameWithType>|**VT_UI1**|
|<xref:System.Int16?displayProperty=nameWithType>|**VT_I2**|
|<xref:System.UInt16?displayProperty=nameWithType>|**VT_UI2**|
|<xref:System.Int32?displayProperty=nameWithType>|**VT_I4**|
|<xref:System.UInt32?displayProperty=nameWithType>|**VT_UI4**|
|<xref:System.Int64?displayProperty=nameWithType>|**VT_I8**|
|<xref:System.UInt64?displayProperty=nameWithType>|**VT_UI8**|
|<xref:System.Single?displayProperty=nameWithType>|**VT_R4**|
|<xref:System.Double?displayProperty=nameWithType>|**VT_R8**|
|<xref:System.Decimal?displayProperty=nameWithType>|**VT_DECIMAL**|
|<xref:System.DateTime?displayProperty=nameWithType>|**VT_DATE**|
|<xref:System.String?displayProperty=nameWithType>|**VT_BSTR**|
|<xref:System.IntPtr?displayProperty=nameWithType>|**VT_INT**|
|<xref:System.UIntPtr?displayProperty=nameWithType>|**VT_UINT**|
|<xref:System.Array?displayProperty=nameWithType>|**VT_ARRAY**|

Usando a interface `MarshalObject` definida no exemplo anterior, o exemplo de código a seguir demonstra como passar vários tipos de variantes para um servidor COM.

```vb
Dim mo As New MarshalObject()
mo.SetVariant(Nothing)         ' Marshal as variant of type VT_EMPTY.
mo.SetVariant(System.DBNull.Value) ' Marshal as variant of type VT_NULL.
mo.SetVariant(CInt(27))        ' Marshal as variant of type VT_I2.
mo.SetVariant(CLng(27))        ' Marshal as variant of type VT_I4.
mo.SetVariant(CSng(27.0))      ' Marshal as variant of type VT_R4.
mo.SetVariant(CDbl(27.0))      ' Marshal as variant of type VT_R8.
```

```csharp
MarshalObject mo = new MarshalObject();
mo.SetVariant(null);            // Marshal as variant of type VT_EMPTY.
mo.SetVariant(System.DBNull.Value); // Marshal as variant of type VT_NULL.
mo.SetVariant((int)27);          // Marshal as variant of type VT_I2.
mo.SetVariant((long)27);          // Marshal as variant of type VT_I4.
mo.SetVariant((single)27.0);   // Marshal as variant of type VT_R4.
mo.SetVariant((double)27.0);   // Marshal as variant of type VT_R8.
```

Tipos COM que não têm tipos gerenciados correspondentes podem ter o marshaling realizado usando classes wrapper como <xref:System.Runtime.InteropServices.ErrorWrapper>, <xref:System.Runtime.InteropServices.DispatchWrapper>, <xref:System.Runtime.InteropServices.UnknownWrapper> e <xref:System.Runtime.InteropServices.CurrencyWrapper>. O exemplo de código a seguir demonstra como usar esses wrappers para passar vários tipos de variantes para um servidor COM.

```vb
Imports System.Runtime.InteropServices
' Pass inew as a variant of type VT_UNKNOWN interface.
mo.SetVariant(New UnknownWrapper(inew))
' Pass inew as a variant of type VT_DISPATCH interface.
mo.SetVariant(New DispatchWrapper(inew))
' Pass a value as a variant of type VT_ERROR interface.
mo.SetVariant(New ErrorWrapper(&H80054002))
' Pass a value as a variant of type VT_CURRENCY interface.
mo.SetVariant(New CurrencyWrapper(New Decimal(5.25)))
```

```csharp
using System.Runtime.InteropServices;
// Pass inew as a variant of type VT_UNKNOWN interface.
mo.SetVariant(new UnknownWrapper(inew));
// Pass inew as a variant of type VT_DISPATCH interface.
mo.SetVariant(new DispatchWrapper(inew));
// Pass a value as a variant of type VT_ERROR interface.
mo.SetVariant(new ErrorWrapper(0x80054002));
// Pass a value as a variant of type VT_CURRENCY interface.
mo.SetVariant(new CurrencyWrapper(new Decimal(5.25)));
```

As classes wrapper são definidas no namespace <xref:System.Runtime.InteropServices>.

### <a name="marshaling-the-iconvertible-interface-to-variant"></a>Realizando marshaling da interface IConvertible para variante

Outros tipos além daqueles listados na seção anterior podem controlar seu marshaling com a implementação da interface <xref:System.IConvertible>. Se o objeto implementar a interface **IConvertible**, o tipo de variante COM será determinado em tempo de execução pelo valor da enumeração <xref:System.TypeCode> retornado do método <xref:System.IConvertible.GetTypeCode%2A?displayProperty=nameWithType>.

A tabela a seguir mostra os valores possíveis para a enumeração **TypeCode** e o tipo de variante COM correspondente para cada valor.

|TypeCode|Tipo de variante COM|
|--------------|----------------------|
|**TypeCode.Empty**|**VT_EMPTY**|
|**TypeCode.Object**|**VT_UNKNOWN**|
|**TypeCode.DBNull**|**VT_NULL**|
|**TypeCode.Boolean**|**VT_BOOL**|
|**TypeCode.Char**|**VT_UI2**|
|**TypeCode.Sbyte**|**VT_I1**|
|**TypeCode.Byte**|**VT_UI1**|
|**TypeCode.Int16**|**VT_I2**|
|**TypeCode.UInt16**|**VT_UI2**|
|**TypeCode.Int32**|**VT_I4**|
|**TypeCode.UInt32**|**VT_UI4**|
|**TypeCode.Int64**|**VT_I8**|
|**TypeCode.UInt64**|**VT_UI8**|
|**TypeCode.Single**|**VT_R4**|
|**TypeCode.Double**|**VT_R8**|
|**TypeCode.Decimal**|**VT_DECIMAL**|
|**TypeCode.DateTime**|**VT_DATE**|
|**TypeCode.String**|**VT_BSTR**|
|Não há suporte.|**VT_INT**|
|Não há suporte.|**VT_UINT**|
|Não há suporte.|**VT_ARRAY**|
|Não há suporte.|**VT_RECORD**|
|Não há suporte.|**VT_CY**|
|Não há suporte.|**VT_VARIANT**|

O valor da variante COM é determinado com uma chamada à interface **IConvertible.To** *Type*, em que **To** *Type* é a rotina de conversão que corresponde ao tipo retornado do **IConvertible.GetTypeCode**. Por exemplo, um objeto que retorna **TypeCode.Double** de **IConvertible.GetTypeCode** tem o marshaling realizado como uma variante COM do tipo **VT_R8**. É possível obter o valor da variante (armazenado no campo **dblVal** da variante COM) com a conversão para a interface **IConvertible** e uma chamada ao método <xref:System.IConvertible.ToDouble%2A>.

## <a name="marshaling-variant-to-object"></a>Realizando marshaling de variante para objeto

Ao realizar marshaling de uma variante para um objeto, o tipo e, às vezes, o valor, da variante com marshaling determina o tipo de objeto produzido. A tabela a seguir identifica cada tipo de variante e o tipo de objeto correspondente criado pelo marshaler quando uma variante é passada do COM para o .NET Framework.

|Tipo de variante COM|Tipo de objeto|
|----------------------|-----------------|
|**VT_EMPTY**|Referência de objeto nulo (**Nothing** no Visual Basic).|
|**VT_NULL**|<xref:System.DBNull?displayProperty=nameWithType>|
|**VT_DISPATCH**|**System.__ComObject** ou nulo se (pdispVal == null)|
|**VT_UNKNOWN**|**System.__ComObject** ou nulo se (punkVal == null)|
|**VT_ERROR**|<xref:System.UInt32?displayProperty=nameWithType>|
|**VT_BOOL**|<xref:System.Boolean?displayProperty=nameWithType>|
|**VT_I1**|<xref:System.SByte?displayProperty=nameWithType>|
|**VT_UI1**|<xref:System.Byte?displayProperty=nameWithType>|
|**VT_I2**|<xref:System.Int16?displayProperty=nameWithType>|
|**VT_UI2**|<xref:System.UInt16?displayProperty=nameWithType>|
|**VT_I4**|<xref:System.Int32?displayProperty=nameWithType>|
|**VT_UI4**|<xref:System.UInt32?displayProperty=nameWithType>|
|**VT_I8**|<xref:System.Int64?displayProperty=nameWithType>|
|**VT_UI8**|<xref:System.UInt64?displayProperty=nameWithType>|
|**VT_R4**|<xref:System.Single?displayProperty=nameWithType>|
|**VT_R8**|<xref:System.Double?displayProperty=nameWithType>|
|**VT_DECIMAL**|<xref:System.Decimal?displayProperty=nameWithType>|
|**VT_DATE**|<xref:System.DateTime?displayProperty=nameWithType>|
|**VT_BSTR**|<xref:System.String?displayProperty=nameWithType>|
|**VT_INT**|<xref:System.Int32?displayProperty=nameWithType>|
|**VT_UINT**|<xref:System.UInt32?displayProperty=nameWithType>|
|**VT_ARRAY** &#124; **VT_**\*|<xref:System.Array?displayProperty=nameWithType>|
|**VT_CY**|<xref:System.Decimal?displayProperty=nameWithType>|
|**VT_RECORD**|Tipo de valor demarcado correspondente.|
|**VT_VARIANT**|Não há suporte.|

Os tipos de variante passados do COM para o código gerenciado e, em seguida, novamente para o COM podem não manter o mesmo tipo de variante durante a chamada. Considere o que acontece quando uma variante do tipo **VT_DISPATCH** é passada do COM para o .NET Framework. Durante o marshaling, a variante é convertida em um <xref:System.Object?displayProperty=nameWithType>. Se o **Object** for então passado novamente para o COM, ele terá o marshaling realizado novamente como uma variante do tipo **VT_UNKNOWN**. Não há nenhuma garantia de que a variante produzida quando um objeto tem o marshaling realizado de um código gerenciado para o COM terá o mesmo tipo de variante usado inicialmente para produzir o objeto.

## <a name="marshaling-byref-variants"></a>Realizando marshaling de variantes de ByRef

Embora as próprias variantes possam ser passadas por valor ou por referência, o sinalizador **VT_BYREF** também pode ser usado com qualquer tipo de variante para indicar que o conteúdo da variante está sendo passado por referência, em vez de por valor. A diferença entre realizar marshaling de variantes por referência e realizar marshaling de uma variante com o sinalizador **VT_BYREF** definido pode ser confusa. A seguinte ilustração esclarece as diferenças:

![Diagrama que mostra a variante passada na pilha.](./media/default-marshaling-for-objects/interop-variant-passed-value-reference.gif)
Variantes passadas por valor e por referência

**Comportamento padrão de marshaling de objetos e variantes por valor**

- Ao passar objetos do código gerenciado para o COM, o conteúdo do objeto é copiado para uma nova variante criada pelo marshaler, usando as regras definidas em [Realizando marshaling de objeto para variante](#marshaling-object-to-variant). As alterações feitas na variante no lado não gerenciado não são propagadas novamente para o objeto original após o retorno da chamada.

- Ao passar variantes do COM para o código gerenciado, o conteúdo da variante é copiado para um objeto recém-criado, usando as regras definidas em [Realizando marshaling de variante para objeto](#marshaling-variant-to-object). As alterações feitas no objeto no lado gerenciado não são propagadas novamente para a variante original após o retorno da chamada.

**Comportamento padrão de marshaling de objetos e variantes por referência**

Para propagar as alterações novamente para o chamador, os parâmetros devem ser passados por referência. Por exemplo, use a palavra-chave **ref** no C# (ou **ByRef** no código gerenciado do Visual Basic) para passar parâmetros por referência. No COM, os parâmetros de referência são passados usando um ponteiro, como uma **variante \***.

- Ao passar um objeto para o COM por referência, o marshaler cria uma nova variante e copia o conteúdo da referência de objeto para a variante antes que a chamada seja feita. A variante é passada para a função não gerenciada, na qual o usuário é livre para alterar o conteúdo da variante. Após o retorno da chamada, todas as alterações feitas na variante no lado não gerenciado são propagadas novamente para o objeto original. Se o tipo da variante for diferente do tipo da variante passada para a chamada, as alterações serão propagadas novamente para um objeto de um tipo diferente. Ou seja, o tipo do objeto passado para a chamada pode ser diferente do tipo do objeto retornado da chamada.

- Ao passar uma variante para um código gerenciado por referência, o marshaler cria um novo objeto e copia o conteúdo da variante para o objeto antes de fazer a chamada. Uma referência ao objeto é passada para a função gerenciada, em que o usuário é livre para alterar o objeto. Após o retorno da chamada, todas as alterações feitas no objeto referenciado são propagadas novamente para a variante original. Se o tipo do objeto for diferente do tipo do objeto passado para a chamada, o tipo da variante original será alterado e o valor será propagado novamente para a variante. Novamente, o tipo da variante passado para a chamada pode ser diferente do tipo da variante retornado da chamada.

 **Comportamento padrão de marshaling de uma variante com o sinalizador VT_BYREF definido**

- Uma variante que está sendo passada para o código gerenciado por valor pode ter o sinalizador **VT_BYREF** definido para indicar que a variante contém uma referência, em vez de um valor. Nesse caso, a variante ainda tem o marshaling realizado para um objeto porque a variante está sendo passada por valor. O marshaler desreferencia automaticamente o conteúdo da variante e o copia para um objeto recém-criado antes de fazer a chamada. Em seguida, o objeto é passado para a função gerenciada; no entanto, após o retorno da chamada, o objeto não é propagado novamente para a variante original. As alterações feitas no objeto gerenciado são perdidas.

  > [!CAUTION]
  > Não há nenhuma maneira de alterar o valor de uma variante passada por valor, mesmo se a variante tiver um sinalizador **VT_BYREF** definido.

- Uma variante que está sendo passada para o código gerenciado por referência também pode ter o sinalizador **VT_BYREF** definido para indicar que a variante contém outra referência. Nesse caso, a variante tem o marshaling realizado para um objeto **ref** porque a variante está sendo passada por referência. O marshaler desreferencia automaticamente o conteúdo da variante e o copia para um objeto recém-criado antes de fazer a chamada. Após o retorno da chamada, o valor do objeto é propagado novamente para a referência dentro da variante original somente se o objeto é do mesmo tipo do objeto passado. Ou seja, a propagação não altera o tipo de uma variante com o sinalizador **VT_BYREF** definido. Se o tipo do objeto for alterado durante a chamada, ocorrerá uma <xref:System.InvalidCastException> após o retorno da chamada.

A tabela a seguir resume as regras de propagação para variantes e objetos.

|De|Para|Alterações propagadas novamente|
|----------|--------|-----------------------------|
|**Variante**  *v*|**Objeto**  *o*|Nunca|
|**Objeto**  *o*|**Variante**  *v*|Nunca|
|**Variante** ***\**** *VP*     |**Objeto de Referência**  *o*|Sempre|
|**Objeto de referência**  *o*|**Variante** ***\**** *VP*     |Sempre|
|**Variante**  *v* **(VT_BYREF** *&#124;* **VT_ \* )**|**Objeto**  *o*|Nunca|
|**Variante**  *v* **(VT_BYREF** *&#124;* **VT_)**|**Objeto de Referência**  *o*|Somente se o tipo não foi alterado.|

## <a name="see-also"></a>Consulte também

- [Comportamento de marshaling padrão](default-marshaling-behavior.md)
- [Tipos blittable e não blittable](blittable-and-non-blittable-types.md)
- [Atributos direcionais](https://docs.microsoft.com/previous-versions/dotnet/netframework-4.0/77e6taeh(v=vs.100))
- [Copiando e fixando](copying-and-pinning.md)
