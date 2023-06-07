---
title: Comportamento de marshaling padrão
description: Aprenda o comportamento de marshaling padrão no .NET. Examine o gerenciamento de memória com o marshaling de interoperabilidade e veja marshaling padrão para classes, delegados e tipos de valor.
ms.date: 06/26/2018
dev_langs:
- csharp
- vb
helpviewer_keywords:
- interop marshaling, default
- interoperation with unmanaged code, marshaling
- marshaling behavior
ms.assetid: c0a9bcdf-3df8-4db3-b1b6-abbdb2af809a
ms.openlocfilehash: 0469874d016725eb6423bb8453e9657b2be923d4
ms.sourcegitcommit: e02d17b2cf9c1258dadda4810a5e6072a0089aee
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/01/2020
ms.locfileid: "85618565"
---
# <a name="default-marshaling-behavior"></a>Comportamento de marshaling padrão
O marshaling de interoperabilidade opera em regras que determinam como os dados associados aos parâmetros de método se comportam, conforme eles passam entre a memória gerenciada e não gerenciada. Essas regras internas controlam atividades de marshaling como transformações de tipo de dados, se um receptor pode alterar os dados passados para ele e retornar essas alterações ao chamador e em quais circunstâncias o marshaler fornece otimizações de desempenho.  
  
 Esta seção identifica as características comportamentais padrão do serviço de marshaling de interoperabilidade. Ela apresenta informações detalhadas sobre o marshaling de matrizes, tipos boolianos, tipos char, representantes, classes, objetos, cadeias de caracteres e estruturas.  
  
> [!NOTE]
> Não há suporte para o marshaling de tipos genéricos. Para obter mais informações, consulte [Interoperando com tipos genéricos](https://docs.microsoft.com/previous-versions/dotnet/netframework-4.0/ms229590(v=vs.100)).  
  
## <a name="memory-management-with-the-interop-marshaler"></a>Gerenciamento de memória com o marshaler de interoperabilidade  
 O marshaler de interoperabilidade sempre tenta liberar a memória alocada pelo código não gerenciado. Esse comportamento está em conformidade com as regras de gerenciamento da memória COM, mas é diferente das regras que regem o C++ nativo.  
  
 Poderá haver confusão se você antecipar o comportamento do C++ nativo (sem liberação de memória) ao usar a invocação de plataforma, que libera a memória para ponteiros automaticamente. Por exemplo, a chamada do método não gerenciado a seguir em uma DLL do C++ não libera qualquer memória automaticamente.  
  
### <a name="unmanaged-signature"></a>Assinatura não gerenciada  
  
```cpp  
BSTR MethodOne (BSTR b) {  
     return b;  
}  
```  
  
 No entanto, se você definir o método como um protótipo de invocação de plataforma, substituir cada tipo **BSTR** por um tipo <xref:System.String> e chamar `MethodOne`, o Common Language Runtime tentará liberar `b` duas vezes. Altere o comportamento de marshaling usando tipos <xref:System.IntPtr>, em vez de tipos **String**.  
  
 O runtime sempre usa o método **CoTaskMemFree** para liberar memória. Se a memória com a qual você está trabalhando não foi alocada com o método **CoTaskMemAlloc**, use um **IntPtr** e libere a memória manualmente usando o método apropriado. Da mesma forma, evite a liberação automática de memória em situações em que a memória nunca deve ser liberada, como ao usar a função **GetCommandLine** no Kernel32.dll, que retorna um ponteiro para a memória do kernel. Para obter detalhes sobre como liberar a memória manualmente, consulte a [Amostra de buffers](https://docs.microsoft.com/previous-versions/dotnet/netframework-4.0/x3txb6xc(v=vs.100)).  
  
## <a name="default-marshaling-for-classes"></a>Marshaling padrão para classes  
 As classes podem ter o marshaling realizado somente pela interoperabilidade COM e sempre têm o marshaling realizado como interfaces. Em alguns casos, a interface usada para realizar marshaling da classe é conhecida como a interface de classe. Para obter informações sobre como substituir a interface de classe por uma interface de sua preferência, confira [Apresentando a interface de classe](../../standard/native-interop/com-callable-wrapper.md#introducing-the-class-interface).  
  
### <a name="passing-classes-to-com"></a>Passando classes para o COM  
 Quando uma classe gerenciada é passada para o COM, o marshaler de interoperabilidade encapsula a classe automaticamente com um proxy COM e passa a interface de classe produzida pelo proxy para a chamada de método COM. Em seguida, o proxy delega todas as chamadas na interface de classe novamente para o objeto gerenciado. O proxy também expõe interfaces que não são implementadas pela classe explicitamente. O proxy implementa automaticamente interfaces como **IUnknown** e **IDispatch** em nome da classe.  
  
### <a name="passing-classes-to-net-code"></a>Passando classes para o código do .NET  
 As coclasses não são normalmente usadas como argumentos de método no COM. Em vez disso, uma interface padrão geralmente é passada no lugar da coclass.  
  
 Quando uma interface é passada para um código gerenciado, o marshaler de interoperabilidade é responsável por encapsular a interface com o wrapper apropriado e passar o wrapper para o método gerenciado. Pode ser difícil determinar quais wrapper usar. Cada instância de um objeto COM tem um único wrapper exclusivo, não importa quantas interfaces são implementadas pelo objeto. Por exemplo, um único objeto COM que implementa cinco interfaces distintas tem apenas um wrapper. O mesmo wrapper expõe todas as cinco interfaces. Se duas instâncias do objeto COM são criadas, duas instâncias do wrapper são criadas.  
  
 Para que o wrapper mantenha o mesmo tipo em todo seu tempo de vida, o marshaler de interoperabilidade deve identificar o wrapper correto na primeira vez que uma interface exposta pelo objeto é passada por meio do marshaler. O marshaler identifica o objeto examinando uma das interfaces implementadas pelo objeto.  
  
 Por exemplo, o marshaler determina que o wrapper de classe deve ser usado para encapsular a interface que foi passada para o código gerenciado. Quando a interface é passada pelo marshaler pela primeira vez, o marshaler verifica se a interface está sendo recebida de um objeto conhecido. Essa verificação ocorre em duas situações:  
  
- Uma interface está sendo implementada por outro objeto gerenciado que foi passado para o COM em outro lugar. O marshaler pode identificar prontamente as interfaces expostas pelos objetos gerenciados e pode fazer a correspondência da interface com o objeto gerenciado que fornece a implementação. Em seguida, o objeto gerenciado é passado para o método e nenhum wrapper é necessário.  
  
- Um objeto que já foi encapsulado está implementando a interface. Para determinar se esse é o caso, o marshaler consulta o objeto em busca de sua interface **IUnknown** e compara a interface retornada com as interfaces de outros objetos que já estão encapsulados. Se a interface for a mesma de outro wrapper, os objetos terão a mesma identidade e o wrapper existente será passado para o método.  
  
 Se uma interface não for de um objeto conhecido, o marshaler fará o seguinte:  
  
1. O marshaler consulta o objeto em busca da interface **IProvideClassInfo2**. Se for fornecido, o marshaler usará o CLSID retornado de **IProvideClassInfo2.GetGUID** para identificar a coclass que fornece a interface. Com o CLSID, o marshaler pode localizar o wrapper no Registro, caso o assembly já tenha sido registrado anteriormente.  
  
2. O marshaler consulta a interface em busca da interface **IProvideClassInfo**. Se for fornecido, o marshaler usará o **ITypeInfo** retornado de **IProvideClassInfo.GetClassinfo** para determinar o CLSID da classe que expõe a interface. O marshaler pode usar o CLSID para localizar os metadados do wrapper.  
  
3. Se o marshaler ainda não puder identificar a classe, ele encapsulará a interface com uma classe wrapper genérica chamada **System.__ComObject**.  
  
## <a name="default-marshaling-for-delegates"></a>Marshaling padrão para representantes  
 Um representante gerenciado tem o marshaling realizado como uma interface COM ou como um ponteiro de função, com base no mecanismo de chamada:  
  
- Para a invocação de plataforma, um representante tem o marshaling realizado como um ponteiro de função não gerenciada, por padrão.  
  
- Para a interoperabilidade COM, um representante tem o marshaling realizado como uma interface COM do tipo **_Delegate**, por padrão. A interface **_Delegate** é definida na biblioteca de tipos Mscorlib.tlb e contém o método <xref:System.Delegate.DynamicInvoke%2A?displayProperty=nameWithType>, que permite chamar o método referenciado pelo representante.  
  
 A tabela a seguir mostra as opções de marshaling para o tipo de dados do representante gerenciado. O atributo <xref:System.Runtime.InteropServices.MarshalAsAttribute> fornece vários valores de enumeração <xref:System.Runtime.InteropServices.UnmanagedType> para realizar marshaling de representantes.  
  
|Tipo de enumeração|Descrição do formato não gerenciado|  
|----------------------|-------------------------------------|  
|**UnmanagedType.FunctionPtr**|Um ponteiro de função não gerenciada.|  
|**UnmanagedType.Interface**|Uma interface do tipo **_Delegate**, conforme definido em Mscorlib.tlb.|  
  
 Considere o código de exemplo a seguir, no qual os métodos da `DelegateTestInterface` são exportados para uma biblioteca de tipos COM. Observe que apenas os representantes marcados com a palavra-chave **ref** (ou **ByRef**) são passados como parâmetros de Entrada/Saída.  
  
```csharp  
using System;  
using System.Runtime.InteropServices;  
  
public interface DelegateTest {  
void m1(Delegate d);  
void m2([MarshalAs(UnmanagedType.Interface)] Delegate d);
void m3([MarshalAs(UnmanagedType.Interface)] ref Delegate d);
void m4([MarshalAs(UnmanagedType.FunctionPtr)] Delegate d);
void m5([MarshalAs(UnmanagedType.FunctionPtr)] ref Delegate d);
}  
```  
  
### <a name="type-library-representation"></a>Representação da biblioteca de tipos  
  
```cpp  
importlib("mscorlib.tlb");  
interface DelegateTest : IDispatch {  
[id(…)] HRESULT m1([in] _Delegate* d);  
[id(…)] HRESULT m2([in] _Delegate* d);  
[id(…)] HRESULT m3([in, out] _Delegate** d);  
[id()] HRESULT m4([in] int d);  
[id()] HRESULT m5([in, out] int *d);  
   };  
```  
  
 Um ponteiro de função pode ser desreferenciado, assim como qualquer outro ponteiro de função não gerenciada pode ser desreferenciado.  

Neste exemplo, quando os dois representantes realizam marshal como <xref:System.Runtime.InteropServices.UnmanagedType.FunctionPtr?displayProperty=nameWithType>, o resultado é um `int` e um ponteiro para um `int`. Como os tipos de delegado estão realizando marshal, `int` representa um ponteiro para um void (`void*`), que é o endereço do delegado na memória. Em outras palavras, esse resultado só ocorre em sistemas Windows de 32 bits, pois `int` representa o tamanho do ponteiro de função.

> [!NOTE]
> Uma referência ao ponteiro de função para um representante gerenciado mantido por um código não gerenciado não impede o Common Language Runtime de executar a coleta de lixo no objeto gerenciado.  
  
 Por exemplo, o código a seguir está incorreto porque a referência ao objeto `cb`, passado para o método `SetChangeHandler`, não mantém `cb` ativo além da vida útil do método `Test`. Depois que o objeto `cb` é coletado como lixo, o ponteiro de função passado para `SetChangeHandler` não é mais válido.  
  
```csharp  
public class ExternalAPI {  
   [DllImport("External.dll")]  
   public static extern void SetChangeHandler(  
      [MarshalAs(UnmanagedType.FunctionPtr)]ChangeDelegate d);  
}  
public delegate bool ChangeDelegate([MarshalAs(UnmanagedType.LPWStr) string S);  
public class CallBackClass {  
   public bool OnChange(string S){ return true;}  
}  
internal class DelegateTest {  
   public static void Test() {  
      CallBackClass cb = new CallBackClass();  
      // Caution: The following reference on the cb object does not keep the
      // object from being garbage collected after the Main method
      // executes.  
      ExternalAPI.SetChangeHandler(new ChangeDelegate(cb.OnChange));
   }  
}  
```  
  
 Para compensar a coleta de lixo inesperada, o chamador deve garantir que o objeto `cb` é mantido ativo enquanto o ponteiro de função não gerenciada está em uso. Opcionalmente, você pode fazer com que o código não gerenciado notifique o código gerenciado quando o ponteiro de função não é mais necessário, como mostra o exemplo a seguir.  
  
```csharp  
internal class DelegateTest {  
   CallBackClass cb;  
   // Called before ever using the callback function.  
   public static void SetChangeHandler() {  
      cb = new CallBackClass();  
      ExternalAPI.SetChangeHandler(new ChangeDelegate(cb.OnChange));  
   }  
   // Called after using the callback function for the last time.  
   public static void RemoveChangeHandler() {  
      // The cb object can be collected now. The unmanaged code is
      // finished with the callback function.  
      cb = null;  
   }  
}  
```  
  
## <a name="default-marshaling-for-value-types"></a>Marshaling padrão para tipos de valor  
 A maioria dos tipos de valor, como inteiros e números de ponto flutuante, é [blittable](blittable-and-non-blittable-types.md) e não exige marshaling. Outros tipos [não blittable](blittable-and-non-blittable-types.md) têm diferentes representações na memória gerenciada e não gerenciada e exigem marshaling. Além disso, outros tipos de exigem a formatação explícita no limite de interoperabilidade.  
  
 Esta seção fornece informações sobre os seguintes tipos de valor formatados:  
  
- [Tipos de valor usados na invocação de plataforma](#value-types-used-in-platform-invoke)  
  
- [Tipos de valor usados na interoperabilidade COM](#value-types-used-in-com-interop)  
  
 Além de descrever os tipos formatados, este tópico identifica os [tipos de valor do sistema](#system-value-types) que têm um comportamento de marshaling incomum.  
  
 Um tipo formatado é um tipo complexo que contém informações que controlam explicitamente o layout de seus membros na memória. As informações de layout de membro são fornecidas usando o atributo <xref:System.Runtime.InteropServices.StructLayoutAttribute>. O layout pode ser um dos seguintes valores de enumeração <xref:System.Runtime.InteropServices.LayoutKind>:  
  
- **LayoutKind. auto**  
  
     Indica que o Common Language Runtime está livre para reordenar os membros do tipo para eficiência. No entanto, quando um tipo de valor é passado para um código não gerenciado, o layout dos membros é previsível. Uma tentativa de realizar marshaling de uma estrutura como essa causa uma exceção automaticamente.  
  
- **LayoutKind.Sequential**  
  
     Indica que os membros do tipo devem ser dispostos na memória não gerenciada na mesma ordem em que aparecem na definição de tipo gerenciado.  
  
- **LayoutKind.Explicit**  
  
     Indica que os membros são dispostos de acordo com o <xref:System.Runtime.InteropServices.FieldOffsetAttribute> fornecido com cada campo.  
  
### <a name="value-types-used-in-platform-invoke"></a>Tipos de valor usados na invocação de plataforma  
 No exemplo a seguir, os tipos `Point` e `Rect` fornecem informações de layout de membro usando o **StructLayoutAttribute**.  
  
```vb  
Imports System.Runtime.InteropServices  
<StructLayout(LayoutKind.Sequential)> Public Structure Point  
   Public x As Integer  
   Public y As Integer  
End Structure  
<StructLayout(LayoutKind.Explicit)> Public Structure Rect  
   <FieldOffset(0)> Public left As Integer  
   <FieldOffset(4)> Public top As Integer  
   <FieldOffset(8)> Public right As Integer  
   <FieldOffset(12)> Public bottom As Integer  
End Structure  
```  
  
```csharp  
using System.Runtime.InteropServices;  
[StructLayout(LayoutKind.Sequential)]  
public struct Point {  
   public int x;  
   public int y;  
}
  
[StructLayout(LayoutKind.Explicit)]  
public struct Rect {  
   [FieldOffset(0)] public int left;  
   [FieldOffset(4)] public int top;  
   [FieldOffset(8)] public int right;  
   [FieldOffset(12)] public int bottom;  
}  
```  
  
 Ao ter o marshaling realizado para um código não gerenciado, esses tipos formatados têm o marshaling realizado como estruturas C-style. Isso fornece uma maneira fácil de chamar uma API não gerenciada que tem argumentos de estrutura. Por exemplo, as estruturas `POINT` e `RECT` podem ser passadas para a função **PtInRect** da API do Microsoft Windows da seguinte maneira:  
  
```cpp  
BOOL PtInRect(const RECT *lprc, POINT pt);  
```  
  
 Passe estruturas usando a seguinte definição de invocação de plataforma:  
  
```vb
Friend Class NativeMethods
    Friend Declare Auto Function PtInRect Lib "User32.dll" (
        ByRef r As Rect, p As Point) As Boolean
End Class
```
  
```csharp
internal static class NativeMethods
{
   [DllImport("User32.dll")]
   internal static extern bool PtInRect(ref Rect r, Point p);
}
```
  
 O tipo de valor `Rect` deve ser passado por referência porque a API não gerenciada está esperando que um ponteiro para um `RECT` seja passado para a função. O tipo de valor `Point` é passado por valor porque a API não gerenciada espera que `POINT` seja passado na pilha. Essa diferença sutil é muito importante. As referências são passadas para um código não gerenciado como ponteiros. Os valores são passados para um código não gerenciado na pilha.  
  
> [!NOTE]
> Quando um tipo formatado tem o marshaling realizado como uma estrutura, apenas os campos no tipo são acessíveis. Se o tipo tiver métodos, propriedades ou eventos, eles poderão ser acessados por meio do código não gerenciado.  
  
 As classes também podem ter o marshaling realizado para um código não gerenciado como estruturas C-style, desde que tenham um layout de membro fixo. As informações de layout de membro de uma classe também são fornecidas com o atributo <xref:System.Runtime.InteropServices.StructLayoutAttribute>. A principal diferença entre os tipos de valor com layout fixo e classes com layout fixo é a maneira na qual eles tem o marshaling realizado para um código não gerenciado. Os tipos de valor são passados por valor (na pilha) e, consequentemente, as alterações feitas nos membros do tipo pelo receptor não são vistas pelo chamador. Os tipos de referência são passados por referência (uma referência ao tipo é passada na pilha); consequentemente, todas as alterações feitas nos membros do tipo blittable de um tipo pelo receptor são vistas pelo chamador.  
  
> [!NOTE]
> Se um tipo de referência tiver membros de tipos não blittable, a conversão será necessária duas vezes: na primeira vez, em que um argumento é passado para o lado não gerenciado e, na segunda, após o retorno da chamada. Devido a essa sobrecarga agregada, os parâmetros de Entrada/Saída devem ser aplicados explicitamente a um argumento se o chamador deseja ver as alterações feitas pelo receptor.  
  
 No exemplo a seguir, a classe `SystemTime` tem um layout de membro sequencial e pode ser passada para a função **GetSystemTime** da API do Windows.  
  
```vb  
<StructLayout(LayoutKind.Sequential)> Public Class SystemTime  
   Public wYear As System.UInt16  
   Public wMonth As System.UInt16  
   Public wDayOfWeek As System.UInt16  
   Public wDay As System.UInt16  
   Public wHour As System.UInt16  
   Public wMinute As System.UInt16  
   Public wSecond As System.UInt16  
   Public wMilliseconds As System.UInt16  
End Class  
```  
  
```csharp  
[StructLayout(LayoutKind.Sequential)]  
   public class SystemTime {  
   public ushort wYear;
   public ushort wMonth;  
   public ushort wDayOfWeek;
   public ushort wDay;
   public ushort wHour;
   public ushort wMinute;
   public ushort wSecond;
   public ushort wMilliseconds;
}  
```  
  
 A função **GetSystemTime** é definida da seguinte maneira:  
  
```cpp  
void GetSystemTime(SYSTEMTIME* SystemTime);  
```  
  
 A definição equivalente de invocação de plataforma para **GetSystemTime** é a seguinte:  
  
```vb
Friend Class NativeMethods
    Friend Declare Auto Sub GetSystemTime Lib "Kernel32.dll" (
        ByVal sysTime As SystemTime)
End Class
```
  
```csharp
internal static class NativeMethods
{
   [DllImport("Kernel32.dll", CharSet = CharSet.Auto)]
   internal static extern void GetSystemTime(SystemTime st);
}
```
  
 Observe que o argumento `SystemTime` não é digitado como um argumento de referência porque `SystemTime` é uma classe, não um tipo de valor. Ao contrário dos tipos de valor, as classes são sempre passadas por referência.  
  
 O exemplo de código a seguir mostra outra classe `Point` que tem um método chamado `SetXY`. Como o tipo tem um layout sequencial, ele pode ser passado para um código não gerenciado e ter o marshaling realizado como uma estrutura. No entanto, o membro `SetXY` não pode ser chamado em um código não gerenciado, mesmo que o objeto seja passado por referência.  
  
```vb  
<StructLayout(LayoutKind.Sequential)> Public Class Point  
   Private x, y As Integer  
   Public Sub SetXY(x As Integer, y As Integer)  
      Me.x = x  
      Me.y = y  
   End Sub  
End Class  
```  
  
```csharp  
[StructLayout(LayoutKind.Sequential)]  
public class Point {  
   int x, y;  
   public void SetXY(int x, int y){
      this.x = x;  
      this.y = y;  
   }  
}  
```  
  
### <a name="value-types-used-in-com-interop"></a>Tipos de valor usados na interoperabilidade COM  
 Os tipos formatados também podem ser passados para chamadas de método da interoperabilidade COM. Na verdade, quando exportados para uma biblioteca de tipos, os tipos de valor são convertidos em estruturas automaticamente. Como mostra o exemplo a seguir, o tipo de valor `Point` se torna uma definição de tipo (typedef) com o nome `Point`. Todas as referências ao tipo de valor `Point` em outros lugares da biblioteca de tipos são substituídas pela typedef `Point`.  
  
 **Representação da biblioteca de tipos**  
  
```cpp  
typedef struct tagPoint {  
   int x;  
   int y;  
} Point;  
interface _Graphics {  
   …  
   HRESULT SetPoint ([in] Point p)  
   HRESULT SetPointRef ([in,out] Point *p)  
   HRESULT GetPoint ([out,retval] Point *p)  
}  
```  
  
 As mesmas regras usadas para realizar marshaling de valores e de referências para chamadas de invocação de plataforma são usadas durante o marshaling por meio de interfaces COM. Por exemplo, quando uma instância do tipo de valor `Point` é passada do .NET Framework para o COM, o `Point` é passado por valor. Se o tipo de valor `Point` é passado por referência, um ponteiro para um `Point` é passado na pilha. O marshaler de interoperabilidade não dá suporte a níveis mais altos de indireção (**Ponto** \*\*) em qualquer direção.  
  
> [!NOTE]
> Estruturas que têm o valor de enumeração <xref:System.Runtime.InteropServices.LayoutKind> definido como **Explicit** não podem ser usadas na interoperabilidade COM porque a biblioteca de tipos exportada não pode expressar um layout explícito.  
  
### <a name="system-value-types"></a>Tipos de valor do sistema  
 O namespace <xref:System> tem vários tipos de valor que representam o formato demarcado dos tipos primitivos de runtime. Por exemplo, a estrutura <xref:System.Int32?displayProperty=nameWithType> do tipo de valor representa o formato demarcado de **ELEMENT_TYPE_I4**. Em vez de realizar marshaling desses tipos como estruturas, assim como ocorre com outros tipos formatados, realize marshaling deles da mesma maneira como os tipos primitivos demarcados por eles. Portanto, **System.Int32** tem o marshaling realizado como **ELEMENT_TYPE_I4**, em vez de como uma estrutura que contém um único membro do tipo **long**. A tabela a seguir contém uma lista dos tipos de valor no namespace **System** que são representações demarcadas de tipos primitivos.  
  
|Tipo de valor do sistema|Tipo de elemento|  
|-----------------------|------------------|  
|<xref:System.Boolean?displayProperty=nameWithType>|**ELEMENT_TYPE_BOOLEAN**|  
|<xref:System.SByte?displayProperty=nameWithType>|**ELEMENT_TYPE_I1**|  
|<xref:System.Byte?displayProperty=nameWithType>|**ELEMENT_TYPE_UI1**|  
|<xref:System.Char?displayProperty=nameWithType>|**ELEMENT_TYPE_CHAR**|  
|<xref:System.Int16?displayProperty=nameWithType>|**ELEMENT_TYPE_I2**|  
|<xref:System.UInt16?displayProperty=nameWithType>|**ELEMENT_TYPE_U2**|  
|<xref:System.Int32?displayProperty=nameWithType>|**ELEMENT_TYPE_I4**|  
|<xref:System.UInt32?displayProperty=nameWithType>|**ELEMENT_TYPE_U4**|  
|<xref:System.Int64?displayProperty=nameWithType>|**ELEMENT_TYPE_I8**|  
|<xref:System.UInt64?displayProperty=nameWithType>|**ELEMENT_TYPE_U8**|  
|<xref:System.Single?displayProperty=nameWithType>|**ELEMENT_TYPE_R4**|  
|<xref:System.Double?displayProperty=nameWithType>|**ELEMENT_TYPE_R8**|  
|<xref:System.String?displayProperty=nameWithType>|**ELEMENT_TYPE_STRING**|  
|<xref:System.IntPtr?displayProperty=nameWithType>|**ELEMENT_TYPE_I**|  
|<xref:System.UIntPtr?displayProperty=nameWithType>|**ELEMENT_TYPE_U**|  
  
 Alguns outros tipos de valor no namespace **System** são manipulados de maneira diferente. Como o código não gerenciado já tem formatos bem estabelecidos para esses tipos, o marshaler tem regras especiais para realizar marshaling deles. A tabela a seguir lista os tipos de valor especiais no namespace **System**, bem como o tipo não gerenciado para o qual eles tem o marshaling realizado.  
  
|Tipo de valor do sistema|Tipo de IDL|  
|-----------------------|--------------|  
|<xref:System.DateTime?displayProperty=nameWithType>|**DATE**|  
|<xref:System.Decimal?displayProperty=nameWithType>|**VÍRGULA**|  
|<xref:System.Guid?displayProperty=nameWithType>|**VOLUME**|  
|<xref:System.Drawing.Color?displayProperty=nameWithType>|**OLE_COLOR**|  
  
 O código a seguir mostra a definição dos tipos não gerenciados **DATE**, **GUID**, **DECIMAL** e **OLE_COLOR** na biblioteca de tipos Stdole2.  
  
#### <a name="type-library-representation"></a>Representação da biblioteca de tipos  
  
```cpp  
typedef double DATE;  
typedef DWORD OLE_COLOR;  
  
typedef struct tagDEC {  
    USHORT    wReserved;  
    BYTE      scale;  
    BYTE      sign;  
    ULONG     Hi32;  
    ULONGLONG Lo64;  
} DECIMAL;  
  
typedef struct tagGUID {  
    DWORD Data1;  
    WORD  Data2;  
    WORD  Data3;  
    BYTE  Data4[ 8 ];  
} GUID;  
```  
  
 O código a seguir mostra as definições correspondentes na interface `IValueTypes` gerenciada.  
  
```vb  
Public Interface IValueTypes  
   Sub M1(d As System.DateTime)  
   Sub M2(d As System.Guid)  
   Sub M3(d As System.Decimal)  
   Sub M4(d As System.Drawing.Color)  
End Interface  
```  
  
```csharp  
public interface IValueTypes {  
   void M1(System.DateTime d);  
   void M2(System.Guid d);  
   void M3(System.Decimal d);  
   void M4(System.Drawing.Color d);  
}  
```  
  
#### <a name="type-library-representation"></a>Representação da biblioteca de tipos  
  
```cpp  
[…]  
interface IValueTypes : IDispatch {  
   HRESULT M1([in] DATE d);  
   HRESULT M2([in] GUID d);  
   HRESULT M3([in] DECIMAL d);  
   HRESULT M4([in] OLE_COLOR d);  
};  
```  
  
## <a name="see-also"></a>Consulte também

- [Tipos blittable e não blittable](blittable-and-non-blittable-types.md)
- [Copiando e fixando](copying-and-pinning.md)
- [Marshaling padrão para matrizes](default-marshaling-for-arrays.md)
- [Marshaling padrão para objetos](default-marshaling-for-objects.md)
- [Marshaling padrão para cadeias de caracteres](default-marshaling-for-strings.md)
