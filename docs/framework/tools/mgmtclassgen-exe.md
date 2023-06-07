---
title: Mgmtclassgen.exe (Gerador de Classe Fortemente Tipada de Gerenciamento)
description: Entenda Mgmtclassgen.exe, o gerador de classes com rigidez de tipos de gerenciamento. Essa ferramenta permite gerar rapidamente uma classe gerenciada de ligação antecipada para uma classe WMI.
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
helpviewer_keywords:
- CIM types
- Management Strongly Typed Class Generator
- WMI class
- Mgmtclassgen.exe
- early-bound managed classes
ms.assetid: 02ce6699-49b5-4a0b-b0d5-1003c491232e
ms.openlocfilehash: 89facd4369dad6168e46febd3e34d7f7c235faf0
ms.sourcegitcommit: b4f8849c47c1a7145eb26ce68bc9f9976e0dbec3
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/03/2020
ms.locfileid: "87517289"
---
# <a name="mgmtclassgenexe-management-strongly-typed-class-generator"></a>Mgmtclassgen.exe (Gerador de Classe Fortemente Tipada de Gerenciamento)
A ferramenta Gerador de Classes Fortemente Tipadas de Gerenciamento permite gerar rapidamente uma classe gerenciada Early Bound para uma classe WMI (Instrumentação de Gerenciamento do Windows) especificada. A classe gerada simplifica o código que você deve gravar para acessar uma instância da classe WMI.  
  
## <a name="syntax"></a>Sintaxe  
  
```console  
mgmtclassgen
WMIClass [options]
```  
  
|Argumento|Descrição|  
|--------------|-----------------|  
|*WMIClass*|A classe WMI para a qual uma classe gerenciada Early Bound deve ser gerada.|  
  
|Opção|DESCRIÇÃO|  
|------------|-----------------|  
|**/l**  *language*|Especifica a linguagem na qual gerar a classe gerenciada Early Bound. É possível especificar **CS** (C#, padrão), **VB** (Visual Basic), **MC** (C++) ou **JS** (JScript) como o argumento da linguagem.|  
|**/m**  *machine*|Especifica o computador ao qual se conectar, em que a classe WMI reside. O padrão é o computador local.|  
|**/n**  *path*|Especifica o caminho para o namespace WMI que contém a classe WMI. Se você não especificar essa opção, a ferramenta gerará o código para *WMIClass* no namespace **Root\cimv2** padrão.|  
|**/o**  *classnamespace*|Especifica o namespace do .NET no qual gerar a classe de código gerenciada. Se você não especificar essa opção, a ferramenta gerará o namespace usando o namespace WMI e o prefixo do esquema. O prefixo do esquema é a parte do nome da classe que antecede o caractere de sublinhado. Por exemplo, para a classe **Win32_OperatingSystem** no namespace **Root\cimv2**, a ferramenta geraria a classe em **ROOT.CIMV2.Win32**.|  
|**/p**  *filepath*|Especifica o caminho para o arquivo no qual o código gerado deve ser salvo. Se você não especificar essa opção, a ferramenta criará o arquivo no diretório atual. Ela nomeia a classe e o arquivo em que gera a classe que usa o argumento *WMIClass*. O nome da classe e do arquivo são iguais ao de *WMIClass.* Se *WMIClass* contiver um caractere de sublinhado, a ferramenta usará a parte do nome de classe depois do caractere de sublinhado. Por exemplo, se o nome *WMIClass* estiver no formato **Win32_LogicalDisk**, a classe e o arquivo gerados serão chamados de "logicaldisk". Se já existir um arquivo, a ferramenta substituirá o arquivo existente.|  
|**/pw**  *password*|Especifica a senha a ser usada durante o logon em um computador especificado pela opção **/m**.|  
|**/u**  *user name*|Especifica o nome de usuário a ser usado durante o logon em um computador especificado pela opção **/m**.|  
|**/?**|Exibe sintaxe de comando e opções para a ferramenta.|  
  
## <a name="remarks"></a>Comentários  
 Mgmtclassgen.exe usa o método <xref:System.Management.ManagementClass.GetStronglyTypedClassCode%2A?displayProperty=nameWithType>. Por isso, é possível usar qualquer provedor de código personalizado para gerar código em linguagens gerenciadas que não sejam C#, Visual Basic e JScript.  
  
 As classes geradas são associadas ao esquema para o qual são geradas. Se o esquema subjacente mudar, você deverá gerar novamente a classe se quiser refletir alterações no esquema.  
  
 A seguinte tabela mostra como tipos CIM (Common Information Model) são mapeados para tipos de dados em uma classe gerada:  
  
|Tipo CIM|Tipo de dados na classe gerada|  
|--------------|--------------------------------------|  
|CIM_SINT8|**SByte**|  
|CIM_UINT8|**Minuciosa**|  
|CIM_SINT16|**Int16**|  
|CIM_UINT16|**UInt16**|  
|CIM_SINT32|**Int32**|  
|SIM_UINT32|**UInt32**|  
|CIM_SINT64|**Int64**|  
|CIM_UINT64|**UInt64**|  
|CIM_REAL32|**Single**|  
|CIM_REAL64|**Double**|  
|CIM_BOOLEAN|**Booliano**|  
|CIM_String|**Cadeia de caracteres**|  
|CIM_DATETIME|**DateTime** ou **TimeSpan**|  
|CIM_REFERENCE|**ManagementPath**|  
|CIM_CHAR16|**º**|  
|CIM_OBJECT|**ManagementBaseObject**|  
|CIM_IUNKNOWN|**Objeto**|  
|CIM_ARRAY|Matriz dos objetos mencionados acima|  
  
 Observer os seguintes comportamentos quando você gera uma classe WMI:  
  
- É possível que uma propriedade ou um método público padrão tenha o mesmo nome de uma propriedade ou um método existente. Se isso ocorrer, a ferramenta alterará o nome da propriedade ou do método na classe gerada para evitar conflitos de nomenclatura.  
  
- É possível que o nome de uma propriedade ou de um método em uma classe gerada seja uma palavra-chave na linguagem de programação de destino. Se isso ocorrer, a ferramenta alterará o nome da propriedade ou do método na classe gerada para evitar conflitos de nomenclatura.  
  
- No WMI, os qualificadores são os modificadores que contêm informações para descrever uma classe, uma instância, uma propriedade ou um método. O WMI usa qualificadores padrão como **Leitura**, **Gravação** e **Chave** para descrever uma propriedade em uma classe gerada. Por exemplo, uma propriedade modificada com um qualificador **Leitura** é definida apenas com um acessador **get** da propriedade na classe gerada. Como uma propriedade marcada com o qualificador **Leitura** deve ser somente leitura, um acessador **set** não é definido.  
  
- Uma propriedade numérica pode ser modificada pelos qualificadores **Values** e **ValueMaps** para indicar que a propriedade pode ser definida somente como valores permitidos especificados. Uma enumeração é gerada com **Values** e **ValueMaps** e a propriedade é mapeada para a enumeração.  
  
- O WMI usa o termo singleton para descrever uma classe que só pode ter uma instância. Por isso, o construtor sem parâmetros de uma classe singleton inicializará a classe com a única instância da classe.  
  
- Uma classe WMI pode ter as propriedades que são objetos. Quando você gera uma classe fortemente tipada para esse tipo de classe WMI, considere a possibilidade de gerar classes fortemente tipadas para os tipos das propriedades do objeto inserido. Isso permitirá que você acesse os objetos inseridos de maneira fortemente tipada. Observe que o código gerado talvez não seja capaz de detectar o tipo de objeto inserido. Nesse caso, um comentário será criado no código gerado para notificar você desse problema. Em seguida, é possível modificar o código gerado para tipar a propriedade para a outra classe gerada.  
  
- No WMI, o valor de dados do tipo de dados CIM_DATETIME pode representar uma data e hora específicas ou um intervalo de tempo. Se o valor de dados representar uma data e hora, o tipo de dados na classe gerada será **DateTime**. Se o valor de dados representar um intervalo de tempo, o tipo de dados na classe gerada será **TimeSpan**.  
  
 Você pode, como alternativa, gerar uma classe com rigidez de tipos usando a extensão de gerenciamento de Gerenciador de Servidores no Visual Studio .NET.  
  
 Para obter mais informações sobre WMI, consulte o tópico **Instrumentação de Gerenciamento do Windows** na documentação do SDK da Plataforma.  
  
## <a name="examples"></a>Exemplos  
 O comando a seguir gera uma classe gerenciada no código do C# para a classe WMI **Win32_LogicalDisk** no namespace **Root\cimv2**. A ferramenta grava a classe gerenciada no arquivo de origem em c:\disk.cs no namespace **ROOT.CIMV2.Win32**.  
  
```console  
mgmtclassgen Win32_LogicalDisk /n root\cimv2 /l CS /p c:\disk.cs  
```  
  
 O código a seguir mostra como usar uma classe gerada programaticamente. Primeiro, uma instância da classe é enumerada e o caminho é impresso. Em seguida, uma instância da classe gerada a ser inicializada é criada com uma instância de WMI. `Process` é a classe gerada para **Win32_Process** e `LogicalDisk` é a classe gerada para **Win32_LogicalDisk** no namespace **Root\cimv2**.  
  
```vb  
Imports System  
Imports System.Management  
Imports ROOT.CIMV2.Win32  
  
Public Class App
   Public Shared Sub Main()
      ' Enumerate instances of the Win32_process.  
      ' Print the Name property of the instance.  
      Dim ps As Process
      For Each ps In  Process.GetInstances()  
         Console.WriteLine(ps.Name)  
      Next ps  
  
      ' Initialize the instance of LogicalDisk with  
      ' the WMI instance pointing to logical drive d:.  
      Dim dskD As New LogicalDisk(New _  
         ManagementPath("win32_LogicalDisk.DeviceId=""d:"""))  
      Console.WriteLine(dskD.Caption)  
   End Sub  
End Class  
```  
  
```csharp  
using System;  
using System.Management;  
using ROOT.CIMV2.Win32;  
  
public class App  
{  
   public static void Main()  
   {  
      // Enumerate instances of the Win32_process.  
      // Print the Name property of the instance.  
      foreach(Process ps in Process.GetInstances())  
      {  
         Console.WriteLine(ps.Name);  
      }  
  
      // Initialize the instance of LogicalDisk with  
      // the WMI instance pointing to logical drive d:.  
      LogicalDisk dskD = new LogicalDisk(new ManagementPath(  
        "win32_LogicalDisk.DeviceId=\"d:\""));  
      Console.WriteLine(dskD.Caption);  
   }  
}  
```  
  
## <a name="see-also"></a>Consulte também

- <xref:System.Management>
- <xref:System.Management.ManagementClass.GetStronglyTypedClassCode%2A?displayProperty=nameWithType>
- <xref:System.CodeDom.Compiler.CodeDomProvider?displayProperty=nameWithType>
- [Ferramentas](index.md)
- [Prompts de comando](developer-command-prompt-for-vs.md)
