---
title: Marshaling padrão para matrizes
description: Entender o marshaling padrão para matrizes. Examine matrizes gerenciadas, matrizes não gerenciadas, passando parâmetros de matriz para código .NET e passando matrizes para COM.
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
helpviewer_keywords:
- interop marshaling, arrays
- arrays, interop marshaling
ms.assetid: 8a3cca8b-dd94-4e3d-ad9a-9ee7590654bc
ms.openlocfilehash: eafed0e0a0150923aae0fa68a1b96e6d9d66b07a
ms.sourcegitcommit: e02d17b2cf9c1258dadda4810a5e6072a0089aee
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/01/2020
ms.locfileid: "85622556"
---
# <a name="default-marshaling-for-arrays"></a>Marshaling padrão para matrizes
Em um aplicativo que consiste inteiramente em um código gerenciado, o Common Language Runtime passa tipos de matriz como parâmetros de Entrada/Saída. Por outro lado, o marshaler de interoperabilidade passa uma matriz como parâmetros de Entrada, por padrão.  
  
 Com a [otimização de anexação](copying-and-pinning.md), uma matriz blittable pode aparecer operar como um parâmetro de Entrada/Saída ao interagir com objetos no mesmo apartment. No entanto, se você exportar o código posteriormente para uma biblioteca de tipos usada para gerar o proxy entre computadores e essa biblioteca for usada para realizar marshaling das chamadas entre apartments, as chamadas poderão reverter como verdadeiro o comportamento do parâmetro de Entrada.  
  
 Matrizes são complexas por natureza e as distinções entre matrizes gerenciadas e não gerenciadas garantem mais informações do que outros tipos não blittable.  
  
## <a name="managed-arrays"></a>Matrizes gerenciadas  
 Os tipos de matriz gerenciada podem variar; no entanto, a classe <xref:System.Array?displayProperty=nameWithType> é a classe base de todos os tipos de matriz. A classe **System.Array** tem propriedades para determinar a classificação, o tamanho e os limites inferior e superior de uma matriz, bem como métodos para acessar, classificar, pesquisar, copiar e criar matrizes.  
  
 Esses tipos de matriz são dinâmicos e não tem um tipo estático correspondente definido na biblioteca de classes base. É conveniente pensar em cada combinação de tipo de elemento e classificação como um tipo distinto de matriz. Portanto, uma matriz unidimensional de inteiros é de um tipo diferente do que uma matriz unidimensional de tipos double. Da mesma forma, uma matriz bidimensional de inteiros é diferente de uma matriz unidimensional de inteiros. Os limites da matriz não são considerados durante a comparação de tipos.  
  
 Como mostra a tabela a seguir, qualquer instância de uma matriz gerenciada deve ser de um tipo de elemento, classificação e limite inferior específicos.  
  
|Tipo de matriz gerenciada|Tipo de elemento|Classificação|Limite inferior|Notação de assinatura|  
|------------------------|------------------|----------|-----------------|------------------------|  
|**ELEMENT_TYPE_ARRAY**|Especificado pelo tipo.|Especificado pela classificação.|Opcionalmente, especificado pelos limites.|*tipo* **[** *n*,*m* **]**|  
|**ELEMENT_TYPE_CLASS**|Unknown (desconhecido)|Unknown (desconhecido)|Unknown (desconhecido)|**System.Array**|  
|**ELEMENT_TYPE_SZARRAY**|Especificado pelo tipo.|1|0|*tipo* **[** *n* **]**|  
  
## <a name="unmanaged-arrays"></a>Matrizes não gerenciadas  
 Matrizes não gerenciadas são matrizes seguras de estilo COM ou matrizes C-style com comprimento fixo ou variável. Matrizes seguras são matrizes autodescritivas que levam o tipo, a classificação e os limites dos dados da matriz associada. Matrizes C-style são matrizes tipadas unidimensionais com um limite inferior fixo igual a 0. O serviço de marshaling tem suporte limitado para ambos os tipos de matrizes.  
  
## <a name="passing-array-parameters-to-net-code"></a>Passando parâmetros de matriz para um código do .NET  
 As matrizes C-style e as matrizes seguras podem ser passadas para um código do .NET de um código não gerenciado, como uma matriz segura ou uma matriz C-style. A tabela a seguir mostra o valor de tipo não gerenciado e o tipo importado.  
  
|Tipo não gerenciado|Tipo importado|  
|--------------------|-------------------|  
|**SafeArray (** *tipo* **)**|**ELEMENT_TYPE_SZARRAY****\<** *ConvertedType* **>**<br /><br /> Classificação = 1, limite inferior = 0. O tamanho é conhecido apenas se é fornecido na assinatura gerenciada. Matrizes seguras que não têm a classificação = 1 ou o limite inferior = 0 não podem ter o marshaling realizado como **SZARRAY**.|  
|*Tipo*  **[]**|**ELEMENT_TYPE_SZARRAY****\<** *ConvertedType* **>**<br /><br /> Classificação = 1, limite inferior = 0. O tamanho é conhecido apenas se é fornecido na assinatura gerenciada.|  
  
### <a name="safe-arrays"></a>Matrizes seguras  
 Quando uma matriz segura é importada de uma biblioteca de tipos para um assembly .NET, a matriz é convertida em uma matriz unidimensional de um tipo conhecido (como **int**). As mesmas regras de conversão de tipo que se aplicam a parâmetros também se aplicam a elementos de matriz. Por exemplo, uma matriz segura de tipos **BSTR** torna-se uma matriz gerenciada de cadeias de caracteres e uma matriz segura de variantes torna-se uma matriz gerenciada de objetos. O tipo de elemento **SAFEARRAY** é capturado na biblioteca de tipos e salvo no valor **SAFEARRAY** da enumeração <xref:System.Runtime.InteropServices.UnmanagedType>.  
  
 Como a classificação e os limites da matriz segura não podem ser determinados na biblioteca de tipos, a classificação é considerada igual a 1 e o limite inferior é considerado igual a 0. A classificação e os limites devem ser definidos na assinatura gerenciada produzida pelo [Importador da Biblioteca de Tipos (Tlbimp.exe)](../tools/tlbimp-exe-type-library-importer.md). Se a classificação passada para o método em tempo de execução for diferente, uma <xref:System.Runtime.InteropServices.SafeArrayRankMismatchException> será gerada. Se o tipo da matriz passado em tempo de execução for diferente, uma <xref:System.Runtime.InteropServices.SafeArrayTypeMismatchException> será gerada. O exemplo a seguir mostra matrizes seguras em código gerenciado e não gerenciado.  
  
 **Assinatura não gerenciada**  
  
```cpp
HRESULT New1([in] SAFEARRAY( int ) ar);  
HRESULT New2([in] SAFEARRAY( DATE ) ar);  
HRESULT New3([in, out] SAFEARRAY( BSTR ) *ar);  
```  
  
 **Assinatura gerenciada**  
  
```vb  
Sub New1(<MarshalAs(UnmanagedType.SafeArray, SafeArraySubType:=VT_I4)> _  
   ar() As Integer)  
Sub New2(<MarshalAs(UnmanagedType.SafeArray, SafeArraySubType:=VT_DATE)> _
   ar() As DateTime)  
Sub New3(ByRef <MarshalAs(UnmanagedType.SafeArray, SafeArraySubType:=VT_BSTR)> _
   ar() As String)  
```  
  
```csharp  
void New1([MarshalAs(UnmanagedType.SafeArray, SafeArraySubType=VT_I4)] int[] ar) ;  
void New2([MarshalAs(UnmanagedType.SafeArray, SafeArraySubType=VT_DATE)]
   DateTime[] ar);  
void New3([MarshalAs(UnmanagedType.SafeArray, SafeArraySubType=VT_BSTR)]
   ref String[] ar);  
```  
  
 As matrizes seguras multidimensionais ou com limite diferente de zero, poderão ter o marshaling realizado em código gerenciado se a assinatura do método produzida por Tlbimp.exe for modificada para indicar um tipo de elemento **ELEMENT_TYPE_ARRAY** em vez de **ELEMENT_TYPE_SZARRAY**. Como alternativa, você pode usar a opção **/sysarray** com Tlbimp.exe para importar todas as matrizes como objetos <xref:System.Array?displayProperty=nameWithType>. Nos casos em que a matriz que está sendo passada é conhecida como multidimensional, é possível editar o código MSIL (Microsoft Intermediate Language) produzido por Tlbimp.exe e, em seguida, recompilá-lo. Para obter detalhes sobre como modificar o código MSIL, consulte [Personalizando RCWs (Runtime Callable Wrappers)](https://docs.microsoft.com/previous-versions/dotnet/netframework-4.0/e753eftz(v=vs.100)).  
  
### <a name="c-style-arrays"></a>Matrizes C-style  
 Quando uma matriz C-style é importada de uma biblioteca de tipos para um assembly .NET, a matriz é convertida em **ELEMENT_TYPE_SZARRAY**.  
  
 O tipo de elemento da matriz é determinado na biblioteca de tipos e preservado durante a importação. As mesmas regras de conversão que se aplicam a parâmetros também se aplicam a elementos de matriz. Por exemplo, uma matriz de tipos **LPStr** torna-se uma matriz de tipos **String**. O Tlbimp.exe captura o tipo de elemento da matriz e aplica o atributo <xref:System.Runtime.InteropServices.MarshalAsAttribute> ao parâmetro.  
  
 A classificação da matriz é considerada igual a 1. Se a classificação for maior que 1, a matriz terá o marshaling realizado como uma matriz unidimensional na ordem de coluna principal. O limite inferior é sempre igual a 0.  
  
 As bibliotecas de tipos podem conter matrizes de comprimento fixo ou variável. O Tlbimp.exe pode importar somente matrizes de comprimento fixo das bibliotecas de tipos porque as bibliotecas de tipos não têm as informações necessárias para realizar marshaling de matrizes de comprimento variável. Com matrizes de comprimento fixo, o tamanho é importado da biblioteca de tipos e capturado no **MarshalAsAttribute** aplicado ao parâmetro.  
  
 É necessário definir manualmente as bibliotecas de tipos que contêm matrizes de comprimento variável, conforme mostrado no exemplo a seguir.  
  
 **Assinatura não gerenciada**  
  
```cpp
HRESULT New1(int ar[10]);  
HRESULT New2(double ar[10][20]);  
HRESULT New3(LPWStr ar[10]);  
```  
  
 **Assinatura gerenciada**  
  
```vb  
Sub New1(<MarshalAs(UnmanagedType.LPArray, SizeConst=10)> _  
   ar() As Integer)  
Sub New2(<MarshalAs(UnmanagedType.LPArray, SizeConst=200)> _  
   ar() As Double)  
Sub New2(<MarshalAs(UnmanagedType.LPArray, _  
   ArraySubType=UnmanagedType.LPWStr, SizeConst=10)> _  
   ar() As String)  
```  
  
```csharp  
void New1([MarshalAs(UnmanagedType.LPArray, SizeConst=10)] int[] ar);  
void New2([MarshalAs(UnmanagedType.LPArray, SizeConst=200)] double[] ar);  
void New2([MarshalAs(UnmanagedType.LPArray,
   ArraySubType=UnmanagedType.LPWStr, SizeConst=10)] String[] ar);  
```  
  
 Embora seja possível aplicar os atributos **size_is** ou **length_is** a uma matriz na fonte da linguagem IDL para transmitir o tamanho para um cliente, o compilador da linguagem IDL da Microsoft (MIDL) não propaga essas informações para a biblioteca de tipos. Sem saber o tamanho, o serviço de marshaling de interoperabilidade não pode realizar marshaling dos elementos da matriz. Consequentemente, as matrizes de comprimento variável são importadas como argumentos de referência. Por exemplo:  
  
 **Assinatura não gerenciada**  
  
```cpp
HRESULT New1(int ar[]);  
HRESULT New2(int ArSize, [size_is(ArSize)] double ar[]);  
HRESULT New3(int ElemCnt, [length_is(ElemCnt)] LPStr ar[]);  
```  
  
 **Assinatura gerenciada**  
  
```vb  
Sub New1(ByRef ar As Integer)  
Sub New2(ByRef ar As Double)  
Sub New3(ByRef ar As String)  
```  
  
```csharp  
void New1(ref int ar);
void New2(ref double ar);
void New3(ref String ar);
```  
  
 É possível fornecer ao marshaler o tamanho da matriz editando o código MSIL (Microsoft Intermediate Language) produzido pelo Tlbimp.exe e, em seguida, recompilá-lo. Para obter detalhes sobre como modificar o código MSIL, consulte [Personalizando RCWs (Runtime Callable Wrappers)](https://docs.microsoft.com/previous-versions/dotnet/netframework-4.0/e753eftz(v=vs.100)). Para indicar o número de elementos na matriz, aplique o tipo <xref:System.Runtime.InteropServices.MarshalAsAttribute> ao parâmetro de matriz da definição de método gerenciado de uma das seguintes maneiras:  
  
- Identifique outro parâmetro que contém o número de elementos na matriz. Os parâmetros são identificados por posição, começando com o primeiro parâmetro como o número 0.
  
    ```vb  
    Sub [New](ElemCnt As Integer, _  
       \<MarshalAs(UnmanagedType.LPArray, SizeParamIndex:=1)> _  
       ar() As Integer)  
    ```  
  
    ```csharp  
    void New(  
       int ElemCnt,
       [MarshalAs(UnmanagedType.LPArray, SizeParamIndex=0)] int[] ar );  
    ```  
  
- Defina o tamanho da matriz como uma constante. Por exemplo:  
  
    ```vb  
    Sub [New](\<MarshalAs(UnmanagedType.LPArray, SizeConst:=128)> _  
       ar() As Integer)  
    ```  
  
    ```csharp  
    void New(  
       [MarshalAs(UnmanagedType.LPArray, SizeConst=128)] int[] ar );  
    ```  
  
 Ao realizar marshaling de matrizes de código não gerenciado para código gerenciado, o marshaler verifica o **MarshalAsAttribute** associado ao parâmetro para determinar o tamanho da matriz. Se o tamanho da matriz não for especificado, somente um elemento terá o marshaling realizado.  
  
> [!NOTE]
> O **MarshalAsAttribute** não tem nenhum efeito no marshaling de matrizes gerenciadas para código não gerenciado. Nessa direção, o tamanho da matriz é determinado pelo exame. Não há nenhuma maneira de realizar marshaling de um subconjunto de uma matriz gerenciada.  
  
 O marshaler de interoperabilidade usa os métodos **CoTaskMemAlloc** e **CoTaskMemFree** para alocar e recuperar a memória. A alocação de memória executada pelo código não gerenciado também deve usar esses métodos.  
  
## <a name="passing-arrays-to-com"></a>Passando matrizes para o COM  
 Todos os tipos de matriz gerenciada podem ser passados para um código não gerenciado de um código gerenciado. Dependendo do tipo gerenciado e dos atributos aplicados a ele, a matriz pode ser acessada como uma matriz segura ou uma matriz C-style, conforme mostrado na tabela a seguir.  
  
|Tipo de matriz gerenciada|Exportado como|  
|------------------------|-----------------|  
|**ELEMENT_TYPE_SZARRAY****\<** *type* **>**|<xref:System.Runtime.InteropServices.UnmanagedType> **.SafeArray(** *type* **)**<br /><br /> **UnmanagedType.LPArray**<br /><br /> O tipo é fornecido na assinatura. A classificação é sempre 1 e o limite inferior é sempre 0. O tamanho é sempre conhecido em tempo de execução.|  
|**ELEMENT_TYPE_ARRAY** **\<** *type* **>** **\<** *rank* **>**[**\<** *bounds* **>**]|**UnmanagedType.SafeArray(** *type* **)**<br /><br /> **UnmanagedType.LPArray**<br /><br /> O tipo, a classificação e os limites são fornecidos na assinatura. O tamanho é sempre conhecido em tempo de execução.|  
|**ELEMENT_TYPE_CLASS****\<**<xref:System.Array?displayProperty=nameWithType>**>**|**UT_Interface**<br /><br /> **UnmanagedType.SafeArray(** *type* **)**<br /><br /> O tipo, a classificação, os limites e o tamanho são sempre conhecidos em tempo de execução.|  
  
 Há uma limitação na Automação OLE relativa a matrizes de estruturas que contêm LPSTR ou LPWSTR.  Portanto, os campos **String** precisam ter o marshaling realizado como **UnmanagedType.BSTR**. Caso contrário, será gerada uma exceção.  
  
### <a name="element_type_szarray"></a>ELEMENT_TYPE_SZARRAY  
 Quando um método que contém um parâmetro **ELEMENT_TYPE_SZARRAY** (matriz unidimensional) é exportado de um assembly .NET para uma biblioteca de tipos, o parâmetro de matriz é convertido em uma **SAFEARRAY** de determinado tipo. As mesmas regras de conversão se aplicam a tipos de elemento da matriz. O conteúdo da matriz gerenciada é copiado automaticamente da memória gerenciada para a **SAFEARRAY**. Por exemplo:  
  
#### <a name="managed-signature"></a>Assinatura gerenciada  
  
```vb  
Sub [New](ar() As Long)  
Sub [New](ar() As String)  
```  
  
```csharp  
void New(long[] ar );  
void New(String[] ar );  
```  
  
#### <a name="unmanaged-signature"></a>Assinatura não gerenciada  
  
```cpp
HRESULT New([in] SAFEARRAY( long ) ar);
HRESULT New([in] SAFEARRAY( BSTR ) ar);  
```  
  
 A classificação de matrizes seguras é sempre 1 e o limite inferior é sempre 0. O tamanho é determinado em tempo de execução pelo tamanho da matriz gerenciada que está sendo passada.  
  
 A matriz também pode ter o marshaling realizado como uma matriz C-style com o uso do atributo <xref:System.Runtime.InteropServices.MarshalAsAttribute>. Por exemplo:  
  
#### <a name="managed-signature"></a>Assinatura gerenciada  
  
```vb  
Sub [New](<MarshalAs(UnmanagedType.LPArray, SizeParamIndex:=1)> _  
   ar() As Long, size as Integer)  
Sub [New](<MarshalAs(UnmanagedType.LPArray, SizeParamIndex:=1)> _  
   ar() As String, size as Integer)  
Sub [New](<MarshalAs(UnmanagedType.LPArray, _  
   ArraySubType= UnmanagedType.LPStr, SizeParamIndex:=1)> _  
   ar() As String, size as Integer)  
```  
  
```csharp  
void New([MarshalAs(UnmanagedType.LPArray, SizeParamIndex=1)]
   long [] ar, int size );  
void New([MarshalAs(UnmanagedType.LPArray, SizeParamIndex=1)]
   String [] ar, int size );  
void New([MarshalAs(UnmanagedType.LPArray, ArraySubType=
   UnmanagedType.LPStr, SizeParamIndex=1)]
   String [] ar, int size );  
```  
  
#### <a name="unmanaged-signature"></a>Assinatura não gerenciada  
  
```cpp
HRESULT New(long ar[]);
HRESULT New(BSTR ar[]);
HRESULT New(LPStr ar[]);  
```  
  
 Embora o marshaler tenha as informações de tamanho necessárias para realizar marshaling da matriz, o tamanho da matriz geralmente é passado como um argumento separado para transmitir o tamanho para o receptor.  
  
### <a name="element_type_array"></a>ELEMENT_TYPE_ARRAY  
 Quando um método que contém um parâmetro **ELEMENT_TYPE_ARRAY** é exportado de um assembly .NET para uma biblioteca de tipos, o parâmetro de matriz é convertido em uma **SAFEARRAY** de determinado tipo. O conteúdo da matriz gerenciada é copiado automaticamente da memória gerenciada para a **SAFEARRAY**. Por exemplo:  
  
#### <a name="managed-signature"></a>Assinatura gerenciada  
  
```vb  
Sub [New](ar(,) As Long)  
Sub [New](ar(,) As String)  
```  
  
```csharp  
void New( long [,] ar );  
void New( String [,] ar );  
```  
  
#### <a name="unmanaged-signature"></a>Assinatura não gerenciada  
  
```cpp
HRESULT New([in] SAFEARRAY( long ) ar);
HRESULT New([in] SAFEARRAY( BSTR ) ar);  
```  
  
 A classificação, o tamanho e os limites das matrizes seguras são determinados em tempo de execução pelas características da matriz gerenciada.  
  
 A matriz também pode ter o marshaling realizado como uma matriz C-style com a aplicação do atributo <xref:System.Runtime.InteropServices.MarshalAsAttribute>. Por exemplo:  
  
#### <a name="managed-signature"></a>Assinatura gerenciada  
  
```vb  
Sub [New](<MarshalAs(UnmanagedType.LPARRAY, SizeParamIndex:=1)> _  
   ar(,) As Long, size As Integer)  
Sub [New](<MarshalAs(UnmanagedType.LPARRAY, _  
   ArraySubType:=UnmanagedType.LPStr, SizeParamIndex:=1)> _  
   ar(,) As String, size As Integer)  
```  
  
```csharp  
void New([MarshalAs(UnmanagedType.LPARRAY, SizeParamIndex=1)]
   long [,] ar, int size );  
void New([MarshalAs(UnmanagedType.LPARRAY,
   ArraySubType= UnmanagedType.LPStr, SizeParamIndex=1)]
   String [,] ar, int size );  
```  
  
#### <a name="unmanaged-signature"></a>Assinatura não gerenciada  
  
```cpp
HRESULT New(long ar[]);
HRESULT New(LPStr ar[]);  
```  
  
 Não é possível realizar marshaling de matrizes aninhadas. Por exemplo, a assinatura a seguir gera um erro quando exportada com o [Exportador da Biblioteca de Tipos (Tlbexp.exe)](../tools/tlbexp-exe-type-library-exporter.md).  
  
#### <a name="managed-signature"></a>Assinatura gerenciada  
  
```vb  
Sub [New](ar()()() As Long)  
```  
  
```csharp  
void New(long [][][] ar );  
```  
  
### <a name="element_type_class-systemarray"></a>ELEMENT_TYPE_CLASS\<System.Array>  
 Quando um método que contém um parâmetro <xref:System.Array?displayProperty=nameWithType> é exportado de um assembly .NET para uma biblioteca de tipos, o parâmetro da matriz é convertido em uma interface **_Array**. O conteúdo da matriz gerenciada é acessível somente por meio dos métodos e das propriedades da interface **_Array**. **System.Array** também pode ter o marshaling realizado como uma **SAFEARRAY** usando o atributo <xref:System.Runtime.InteropServices.MarshalAsAttribute>. Ao ter o marshaling realizado como uma matriz segura, os elementos da matriz têm o marshaling realizado como variantes. Por exemplo:  
  
#### <a name="managed-signature"></a>Assinatura gerenciada  
  
```vb  
Sub New1( ar As System.Array )  
Sub New2( <MarshalAs(UnmanagedType.Safe array)> ar As System.Array )  
```  
  
```csharp  
void New1( System.Array ar );  
void New2( [MarshalAs(UnmanagedType.Safe array)] System.Array ar );  
```  
  
#### <a name="unmanaged-signature"></a>Assinatura não gerenciada  
  
```cpp
HRESULT New([in] _Array *ar);
HRESULT New([in] SAFEARRAY(VARIANT) ar);  
```  
  
### <a name="arrays-within-structures"></a>Matrizes em estruturas  
 Estruturas não gerenciadas podem conter matrizes inseridas. Por padrão, esses campos de matriz inseridos têm o marshaling realizado como uma SAFEARRAY. No exemplo a seguir, `s1` é uma matriz inserida que é alocada diretamente na própria estrutura.  
  
#### <a name="unmanaged-representation"></a>Representação não gerenciada  
  
```cpp
struct MyStruct {  
    short s1[128];  
}  
```  
  
 Matrizes podem ter o marshaling realizado como <xref:System.Runtime.InteropServices.UnmanagedType>, o que exige a definição do campo <xref:System.Runtime.InteropServices.MarshalAsAttribute>. O tamanho pode ser definido somente como uma constante. O código a seguir mostra a definição gerenciada correspondente de `MyStruct`.  
  
```vb  
Public Structure <StructLayout(LayoutKind.Sequential)> MyStruct  
   Public <MarshalAs(UnmanagedType.ByValArray, SizeConst := 128)> _  
     s1() As Short  
End Structure  
```  
  
```csharp  
[StructLayout(LayoutKind.Sequential)]  
public struct MyStruct {  
   [MarshalAs(UnmanagedType.ByValArray, SizeConst=128)] public short[] s1;  
}  
```  
  
## <a name="see-also"></a>Consulte também

- [Comportamento de marshaling padrão](default-marshaling-behavior.md)
- [Tipos blittable e não blittable](blittable-and-non-blittable-types.md)
- [Atributos direcionais](https://docs.microsoft.com/previous-versions/dotnet/netframework-4.0/77e6taeh(v=vs.100))
- [Copiando e fixando](copying-and-pinning.md)
