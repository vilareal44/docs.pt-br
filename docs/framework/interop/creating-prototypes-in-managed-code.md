---
title: Criando protótipos em código gerenciado
description: Crie protótipos no código gerenciado do .NET, para que você possa acessar funções não gerenciadas e usar campos de atributo que anotam a definição de método no código gerenciado.
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
- cpp
helpviewer_keywords:
- prototypes in managed code
- COM interop, DLL functions
- unmanaged functions
- platform invoke, creating prototypes
- COM interop, platform invoke
- interoperation with unmanaged code, DLL functions
- interoperation with unmanaged code, platform invoke
- platform invoke, object fields
- DLL functions
- object fields in platform invoke
ms.assetid: ecdcf25d-cae3-4f07-a2b6-8397ac6dc42d
ms.openlocfilehash: 76b1a87c4513fdee21c5c3d5eba533b11e022e3a
ms.sourcegitcommit: e02d17b2cf9c1258dadda4810a5e6072a0089aee
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/01/2020
ms.locfileid: "85622153"
---
# <a name="creating-prototypes-in-managed-code"></a>Criando protótipos em código gerenciado
Este tópico descreve como acessar funções não gerenciadas e apresenta vários campos de atributo que anotam a definição de método no código gerenciado. Para obter exemplos que demonstram como construir declarações baseadas no .NET a serem usadas com a invocação de plataforma, consulte [Realizando marshaling de dados com a invocação de plataforma](marshaling-data-with-platform-invoke.md).  
  
 Antes de poder acessar uma função de DLL não gerenciada no código gerenciado, você precisa saber o nome da função e o nome da DLL que a exporta. Com essas informações, você pode começar a escrever a definição gerenciada para uma função não gerenciada que é implementada em uma DLL. Além disso, você pode ajustar a maneira como essa invocação de plataforma cria a função e realiza marshaling dos dados bidirecionalmente na função.  
  
> [!NOTE]
> As funções de API do Windows que alocam uma cadeia de caracteres permitem que você libere a cadeia de caracteres usando um método como `LocalFree`. A invocação de plataforma manipula esses parâmetros de maneira diferente. Para chamadas de invocação de plataforma, torne o parâmetro um tipo `IntPtr`, em vez de um tipo `String`. Use métodos que são fornecidos pela classe <xref:System.Runtime.InteropServices.Marshal?displayProperty=nameWithType> para converter o tipo em uma cadeia de caracteres manualmente e libere-a manualmente.  
  
## <a name="declaration-basics"></a>Noções básicas sobre a declaração  
 As definições gerenciadas para funções não gerenciadas são dependentes de idioma, como é possível ver nos exemplos a seguir. Para obter exemplos de código mais completos, consulte [Exemplos de invocação de plataforma](platform-invoke-examples.md).  
  
```vb
Friend Class NativeMethods
    Friend Declare Auto Function MessageBox Lib "user32.dll" (
        ByVal hWnd As IntPtr,
        ByVal lpText As String,
        ByVal lpCaption As String,
        ByVal uType As UInteger) As Integer
End Class
```
  
 Para aplicar os campos <xref:System.Runtime.InteropServices.DllImportAttribute.BestFitMapping?displayProperty=nameWithType>, <xref:System.Runtime.InteropServices.DllImportAttribute.CallingConvention?displayProperty=nameWithType>, <xref:System.Runtime.InteropServices.DllImportAttribute.ExactSpelling?displayProperty=nameWithType>, <xref:System.Runtime.InteropServices.DllImportAttribute.PreserveSig?displayProperty=nameWithType>, <xref:System.Runtime.InteropServices.DllImportAttribute.SetLastError?displayProperty=nameWithType> ou <xref:System.Runtime.InteropServices.DllImportAttribute.ThrowOnUnmappableChar?displayProperty=nameWithType> a uma declaração do Visual Basic, use o atributo <xref:System.Runtime.InteropServices.DllImportAttribute>, em vez da instrução `Declare`.  
  
```vb
Imports System.Runtime.InteropServices

Friend Class NativeMethods
    <DllImport("user32.dll", CharSet:=CharSet.Auto)>
    Friend Shared Function MessageBox(
        ByVal hWnd As IntPtr,
        ByVal lpText As String,
        ByVal lpCaption As String,
        ByVal uType As UInteger) As Integer
    End Function
End Class
```
  
```csharp
using System;
using System.Runtime.InteropServices;

internal static class NativeMethods
{
    [DllImport("user32.dll")]
    internal static extern int MessageBox(
        IntPtr hWnd, string lpText, string lpCaption, uint uType);
}
```
  
```cpp
using namespace System;
using namespace System::Runtime::InteropServices;

[DllImport("user32.dll")]
extern "C" int MessageBox(
    IntPtr hWnd, String* lpText, String* lpCaption, unsigned int uType);
```
  
## <a name="adjusting-the-definition"></a>Ajustando a definição  
 Independentemente de você defini-los explicitamente ou não, os campos de atributo estão trabalhando, definindo o comportamento do código gerenciado. A invocação de plataforma funciona de acordo com os valores padrão definidos em vários campos que existem como metadados em um assembly. Altere esse comportamento padrão ajustando os valores de um ou mais campos. Em muitos casos, use o <xref:System.Runtime.InteropServices.DllImportAttribute> para definir um valor.  
  
 A tabela a seguir lista o conjunto completo de campos de atributo que pertencem à invocação de plataforma. Para cada campo, a tabela inclui o valor padrão e um link para informações sobre como usar esses campos para definir funções de DLL não gerenciadas.  
  
|Campo|Descrição|  
|-----------|-----------------|  
|<xref:System.Runtime.InteropServices.DllImportAttribute.BestFitMapping>|Habilita ou desabilita o mapeamento de melhor ajuste.|  
|<xref:System.Runtime.InteropServices.DllImportAttribute.CallingConvention>|Especifica a convenção de chamada a ser usada ao passar argumentos de método. O padrão é `WinAPI`, que corresponde a `__stdcall` nas plataformas Intel de 32 bits.|  
|<xref:System.Runtime.InteropServices.DllImportAttribute.CharSet>|Controla a desconfiguração de nome e a maneira como os argumentos de cadeia de caracteres devem ter o marshaling realizado para a função. O padrão é `CharSet.Ansi`.|  
|<xref:System.Runtime.InteropServices.DllImportAttribute.EntryPoint>|Especifica o ponto de entrada de DLL a ser chamado.|  
|<xref:System.Runtime.InteropServices.DllImportAttribute.ExactSpelling>|Controla se um ponto de entrada deve ser modificado para que ele corresponda ao conjunto de caracteres. O valor padrão varia de acordo com a linguagem de programação.|  
|<xref:System.Runtime.InteropServices.DllImportAttribute.PreserveSig>|Controla se a assinatura de método gerenciada deve ser transformada em uma assinatura não gerenciada que retorna um HRESULT e que tem um argumento adicional [out, retval] para o valor retornado.<br /><br /> O padrão é `true` (a assinatura não deve ser transformada).|  
|<xref:System.Runtime.InteropServices.DllImportAttribute.SetLastError>|Permite ao chamador usar a função de API `Marshal.GetLastWin32Error` para determinar se ocorreu um erro ao executar o método. No Visual Basic, o padrão é `true`; no C# e no C++, o padrão é `false`.|  
|<xref:System.Runtime.InteropServices.DllImportAttribute.ThrowOnUnmappableChar>|Controla a geração de uma exceção em um caractere Unicode não mapeável que é convertido em um caractere “?” ANSI.|  
  
 Para obter informações detalhadas sobre a referência, consulte <xref:System.Runtime.InteropServices.DllImportAttribute>.  
  
## <a name="platform-invoke-security-considerations"></a>Considerações sobre segurança da invocação de plataforma  
 Os membros `Assert`, `Deny` e `PermitOnly` da enumeração <xref:System.Security.Permissions.SecurityAction> são chamados de *modificadores de movimentação de pilha*. Esses membros serão ignorados se forem usados como atributos declarativos em declarações da invocação de plataforma e em instruções COM da linguagem IDL.  
  
### <a name="platform-invoke-examples"></a>Exemplos de invocação de plataforma  
 As amostras de invocação de plataforma desta seção ilustram o uso do atributo `RegistryPermission` com os modificadores de movimentação de pilha.  
  
 No exemplo a seguir, os modificadores <xref:System.Security.Permissions.SecurityAction>`Assert`, `Deny` e `PermitOnly` são ignorados.  
  
```csharp  
[DllImport("MyClass.dll", EntryPoint = "CallRegistryPermission")]  
[RegistryPermission(SecurityAction.Assert, Unrestricted = true)]  
    private static extern bool CallRegistryPermissionAssert();  
  
[DllImport("MyClass.dll", EntryPoint = "CallRegistryPermission")]  
[RegistryPermission(SecurityAction.Deny, Unrestricted = true)]  
    private static extern bool CallRegistryPermissionDeny();  
  
[DllImport("MyClass.dll", EntryPoint = "CallRegistryPermission")]  
[RegistryPermission(SecurityAction.PermitOnly, Unrestricted = true)]  
    private static extern bool CallRegistryPermissionDeny();  
```  
  
 No entanto, o modificador `Demand` no exemplo a seguir é aceito.  
  
```csharp
[DllImport("MyClass.dll", EntryPoint = "CallRegistryPermission")]  
[RegistryPermission(SecurityAction.Demand, Unrestricted = true)]  
    private static extern bool CallRegistryPermissionDeny();  
```  
  
 Os modificadores <xref:System.Security.Permissions.SecurityAction> funcionam corretamente se são colocados em uma classe que contém (encapsula) a chamada da invocação de plataforma.  
  
```cpp  
      [RegistryPermission(SecurityAction.Demand, Unrestricted = true)]  
public ref class PInvokeWrapper  
{  
public:  
[DllImport("MyClass.dll", EntryPoint = "CallRegistryPermission")]  
    private static extern bool CallRegistryPermissionDeny();  
};  
```  
  
```csharp  
[RegistryPermission(SecurityAction.Demand, Unrestricted = true)]  
class PInvokeWrapper  
{  
[DllImport("MyClass.dll", EntryPoint = "CallRegistryPermission")]  
    private static extern bool CallRegistryPermissionDeny();  
}  
```  
  
 Os modificadores <xref:System.Security.Permissions.SecurityAction> também funcionam corretamente em um cenário aninhado em que são colocados no chamador da chamada de invocação de plataforma:  
  
```cpp  
      {  
public ref class PInvokeWrapper  
public:  
    [DllImport("MyClass.dll", EntryPoint = "CallRegistryPermission")]  
    private static extern bool CallRegistryPermissionDeny();  
  
    [RegistryPermission(SecurityAction.Demand, Unrestricted = true)]  
    public static bool CallRegistryPermission()  
    {  
     return CallRegistryPermissionInternal();  
    }  
};  
```  
  
```csharp  
class PInvokeScenario  
{  
    [DllImport("MyClass.dll", EntryPoint = "CallRegistryPermission")]  
    private static extern bool CallRegistryPermissionInternal();  
  
    [RegistryPermission(SecurityAction.Assert, Unrestricted = true)]  
    public static bool CallRegistryPermission()  
    {  
     return CallRegistryPermissionInternal();  
    }  
}  
```  
  
#### <a name="com-interop-examples"></a>Exemplos de interoperabilidade COM  
 As amostras de interoperabilidade COM desta seção ilustram o uso do atributo `RegistryPermission` com os modificadores de movimentação de pilha.  
  
 As declarações da interface de interoperabilidade COM a seguir ignoram os modificadores `Assert`, `Deny` e `PermitOnly`, da mesma forma que os exemplos da invocação de plataforma da seção anterior.  
  
```csharp
[ComImport, Guid("12345678-43E6-43c9-9A13-47F40B338DE0")]  
interface IAssertStubsItf  
{  
[RegistryPermission(SecurityAction.Assert, Unrestricted = true)]  
    bool CallRegistryPermission();  
[FileIOPermission(SecurityAction.Assert, Unrestricted = true)]  
    bool CallFileIoPermission();  
}  
  
[ComImport, Guid("12345678-43E6-43c9-9A13-47F40B338DE0")]  
interface IDenyStubsItf  
{  
[RegistryPermission(SecurityAction.Deny, Unrestricted = true)]  
    bool CallRegistryPermission();  
[FileIOPermission(SecurityAction.Deny, Unrestricted = true)]  
    bool CallFileIoPermission();  
}  
  
[ComImport, Guid("12345678-43E6-43c9-9A13-47F40B338DE0")]  
interface IAssertStubsItf  
{  
[RegistryPermission(SecurityAction.PermitOnly, Unrestricted = true)]  
    bool CallRegistryPermission();  
[FileIOPermission(SecurityAction.PermitOnly, Unrestricted = true)]  
    bool CallFileIoPermission();  
}  
```  
  
 Além disso, o modificador `Demand` não é aceito em cenários de declaração da interface de interoperabilidade COM, conforme mostrado no exemplo a seguir.  
  
```csharp  
[ComImport, Guid("12345678-43E6-43c9-9A13-47F40B338DE0")]  
interface IDemandStubsItf  
{  
[RegistryPermission(SecurityAction.Demand, Unrestricted = true)]  
    bool CallRegistryPermission();  
[FileIOPermission(SecurityAction.Demand, Unrestricted = true)]  
    bool CallFileIoPermission();  
}  
```  
  
## <a name="see-also"></a>Consulte também

- [Consumindo funções de DLL não gerenciadas](consuming-unmanaged-dll-functions.md)
- [Especificando um ponto de entrada](specifying-an-entry-point.md)
- [Especificando um conjunto de caracteres](specifying-a-character-set.md)
- [Exemplos de invocação de plataforma](platform-invoke-examples.md)
- [Considerações sobre Segurança da Invocação de Plataforma](https://docs.microsoft.com/previous-versions/dotnet/netframework-4.0/bb397754(v=vs.100))
- [Identificando funções em DLLs](identifying-functions-in-dlls.md)
- [Criando uma classe para conter funções de DLL](creating-a-class-to-hold-dll-functions.md)
- [Chamando uma função de DLL](calling-a-dll-function.md)
