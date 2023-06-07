---
title: Usando o método Assert
description: Veja como você pode usar o método Assert para habilitar seu código (e o código de chamadores downstream) para fazer coisas para as quais seu código tem permissão, mas não seus chamadores.
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
helpviewer_keywords:
- granting permissions, overriding security checks
- code access security, overriding security checks
- overriding security checks
- security [.NET Framework], overriding security checks
- security [.NET Framework], assertions
- asserted permissions
- Assert method
- caller security checks
- permissions [.NET Framework], overriding security checks
- permissions [.NET Framework], assertions
ms.assetid: 1e40f4d3-fb7d-4f19-b334-b6076d469ea9
ms.openlocfilehash: 573b84f991e795c2513f213ddb52999fef51c454
ms.sourcegitcommit: c37e8d4642fef647ebab0e1c618ecc29ddfe2a0f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/06/2020
ms.locfileid: "87855655"
---
# <a name="using-the-assert-method"></a>Usando o método Assert

[!INCLUDE[net_security_note](../../../includes/net-security-note-md.md)]  
  
 <xref:System.Security.CodeAccessPermission.Assert%2A>é um método que pode ser chamado em classes de permissão de acesso ao código e na <xref:System.Security.PermissionSet> classe. Você pode usar **Assert** para habilitar seu código (e chamadores downstream) para executar ações que seu código tem permissão para fazer, mas seus chamadores podem não ter permissão para fazer. Uma asserção de segurança altera o processo normal que o tempo de execução executa durante uma verificação de segurança. Quando você declara uma permissão, ele diz ao sistema de segurança para não verificar os chamadores do seu código para a permissão declarada.  
  
> [!CAUTION]
> Use as asserções com cuidado porque elas podem abrir brechas de segurança e prejudicar o mecanismo do tempo de execução para impor restrições de segurança.  
  
 As asserções são úteis em situações em que uma biblioteca chama código não gerenciado ou faz uma chamada que requer uma permissão que não é obviamente relacionada ao uso pretendido da biblioteca. Por exemplo, todo código gerenciado que chama o código não gerenciado deve ter **SecurityPermission** com o sinalizador **UnmanagedCode** especificado. O código que não é originário do computador local, como o código baixado da intranet local, não receberá essa permissão por padrão. Portanto, para que o código que é baixado da intranet local seja capaz de chamar uma biblioteca que usa código não gerenciado, ele deve ter a permissão declarada pela biblioteca. Além disso, algumas bibliotecas podem fazer chamadas que não são vistas aos chamadores e exigem permissões especiais.  
  
 Você também pode usar asserções em situações em que seu código acessa um recurso de uma forma que está completamente oculta dos chamadores. Por exemplo, suponha que sua biblioteca adquire informações de um banco de dados, mas no processo também lê informações do registro do computador. Como os desenvolvedores que usam sua biblioteca não têm acesso à sua fonte, eles não têm como saber que seu código requer **RegistryPermission** para usar seu código. Nesse caso, se você decidir que não é razoável ou necessário exigir que os chamadores do seu código tenham permissão para acessar o registro, você pode declarar a permissão para ler o registro. Nessa situação, é apropriado que a biblioteca declare a permissão para que os chamadores sem **RegistryPermission** possam usar a biblioteca.  
  
 A asserção afetará a movimentação da pilha somente se a permissão declarada e uma permissão exigida por um chamador downstream forem do mesmo tipo e se a permissão solicitada for um subconjunto da permissão declarada. Por exemplo, se você declarar **FileIOPermission** para ler todos os arquivos na unidade C e uma demanda de downstream for feita para **FileIOPermission** ler arquivos em C:\temp, a asserção poderá afetar a movimentação da pilha; no entanto, se a demanda fosse para **FileIOPermission** gravar na unidade C, a asserção não teria nenhum efeito.  
  
 Para executar asserções, seu código deve receber a permissão que você está declarando e o <xref:System.Security.Permissions.SecurityPermission> que representa o direito de fazer asserções. Embora você possa declarar uma permissão de que seu código não foi concedido, a asserção seria inútil, pois a verificação de segurança falharia antes que a asserção pudesse ser bem-sucedida.  
  
 A ilustração a seguir mostra o que acontece quando você usa **Assert**. Suponha que as instruções a seguir sejam verdadeiras sobre assemblies A, B, C, e e F e duas permissões, P1 e P1A:  
  
- P1A representa o direito de ler arquivos. txt na unidade C.  
  
- P1 representa o direito de ler todos os arquivos na unidade C.  
  
- P1A e P1 são tipos **FileIOPermission** e P1A é um subconjunto de P1.  
  
- Os assemblies E e F receberam permissão P1A.  
  
- O assembly C recebeu a permissão P1.  
  
- Os assemblies A e B receberam as permissões P1 e P1A.  
  
- O método A está contido no assembly A, o método B está contido no assembly B e assim por diante.  
  
 ![Diagrama que mostra os assemblies do método Assert.](./media/using-the-assert-method/assert-method-assemblies.gif)
  
 Nesse cenário, o método A chama B, B chama C, C calls e e e chamadas F. o método C declara a permissão para ler arquivos na unidade C (permissão P1) e o método E exige permissão para ler arquivos. txt na unidade C (P1A de permissão). Quando a demanda em F é encontrada em tempo de execução, uma movimentação de pilha é executada para verificar as permissões de todos os chamadores de F, começando com E. E recebeu a permissão P1A, de modo que a movimentação da pilha continua a examinar as permissões de C, onde a declaração de C é descoberta. Como a permissão exigida (P1A) é um subconjunto da permissão declarada (P1), a movimentação da pilha é interrompida e a verificação de segurança é automaticamente realizada com sucesso. Não importa que os assemblies A e B não tenham recebido A permissão P1A. Ao declarar P1, o método C garante que seus chamadores possam acessar o recurso protegido pelo P1, mesmo que os chamadores não tenham recebido permissão para acessar esse recurso.  
  
 Se você projetar uma biblioteca de classes e uma classe acessar um recurso protegido, você deverá, na maioria dos casos, fazer uma demanda de segurança exigindo que os chamadores da classe tenham a permissão apropriada. Se a classe executar uma operação para a qual você sabe que a maioria de seus chamadores não terá permissão e, se você estiver disposto a assumir a responsabilidade de permitir que esses chamadores chamem seu código, poderá declarar a permissão chamando o método **Assert** em um objeto de permissão que representa a operação que o código está executando. Usar **Assert** dessa maneira permite que os chamadores que normalmente não podiam fazer isso chamem seu código. Portanto, se você declarar uma permissão, deverá ter certeza de executar as verificações de segurança apropriadas antecipadamente para impedir que o componente seja usado incorretamente.  
  
 Por exemplo, suponha que sua classe de biblioteca altamente confiável tenha um método que exclui arquivos. Ele acessa o arquivo chamando uma função Win32 não gerenciada. Um chamador invoca o método **delete** de seu código, passando o nome do arquivo a ser excluído, C:\Test.txt. Dentro do método **delete** , seu código cria um <xref:System.Security.Permissions.FileIOPermission> objeto que representa acesso de gravação para C:\Test.txt. (É necessário acesso de gravação para excluir um arquivo.) Em seguida, seu código invoca uma verificação de segurança imperativa chamando o método de **demanda** do objeto **FileIOPermission** . Se um dos chamadores na pilha de chamadas não tiver essa permissão, um <xref:System.Security.SecurityException> será gerado. Se nenhuma exceção for gerada, você saberá que todos os chamadores têm o direito de acessar C:\Test.txt. Como você acredita que a maioria dos seus chamadores não terá permissão para acessar o código não gerenciado, seu código cria um <xref:System.Security.Permissions.SecurityPermission> objeto que representa o direito de chamar o código não gerenciado e chama o método **Assert** do objeto. Por fim, ele chama a função Win32 não gerenciada para excluir C:\Text.txt e retorna o controle para o chamador.  
  
> [!CAUTION]
> Você deve ter certeza de que seu código não usa declarações em situações em que seu código pode ser usado por outro código para acessar um recurso protegido pela permissão que você está declarando. Por exemplo, no código que grava em um arquivo cujo nome é especificado pelo chamador como um parâmetro, você não deve declarar o **FileIOPermission** para gravar em arquivos porque seu código estaria aberto para uso indevido de terceiros.  
  
 Quando você usa a sintaxe de segurança imperativa, chamar o método **Assert** em várias permissões no mesmo método faz com que uma exceção de segurança seja gerada. Em vez disso, você deve criar um objeto **PermissionSet** , passá-lo para as permissões individuais que deseja invocar e, em seguida, chamar o método **Assert** no objeto **PermissionSet** . Você pode chamar o método **Assert** mais de uma vez ao usar a sintaxe de segurança declarativa.  
  
 O exemplo a seguir mostra a sintaxe declarativa para substituir verificações de segurança usando o método **Assert** . Observe que a sintaxe **FileIOPermissionAttribute** usa dois valores: uma <xref:System.Security.Permissions.SecurityAction> enumeração e o local do arquivo ou diretório ao qual a permissão deve ser concedida. A chamada para **Assert** faz com que as demandas de acesso `C:\Log.txt` tenham sucesso, mesmo que os chamadores não sejam verificados quanto à permissão para acessar o arquivo.  
  
```vb  
Option Explicit  
Option Strict  
  
Imports System  
Imports System.IO  
Imports System.Security.Permissions  
  
Namespace LogUtil  
   Public Class Log  
      Public Sub New()  
  
      End Sub  
  
     <FileIOPermission(SecurityAction.Assert, All := "C:\Log.txt")> Public Sub
      MakeLog()  
         Dim TextStream As New StreamWriter("C:\Log.txt")  
         TextStream.WriteLine("This  Log was created on {0}", DateTime.Now) '  
         TextStream.Close()  
      End Sub  
   End Class  
End Namespace  
```  
  
```csharp  
namespace LogUtil  
{  
   using System;  
   using System.IO;  
   using System.Security.Permissions;  
  
   public class Log  
   {  
      public Log()  
      {
      }
      [FileIOPermission(SecurityAction.Assert, All = @"C:\Log.txt")]  
      public void MakeLog()  
      {
         StreamWriter TextStream = new StreamWriter(@"C:\Log.txt");  
         TextStream.WriteLine("This  Log was created on {0}", DateTime.Now);  
         TextStream.Close();  
      }  
   }  
}
```  
  
 Os fragmentos de código a seguir mostram a sintaxe imperativa para substituir verificações de segurança usando o método **Assert** . Neste exemplo, uma instância do objeto **FileIOPermission** é declarada. Seu construtor é passado **FileIOPermissionAccess. myaccess** para definir o tipo de acesso permitido, seguido por uma cadeia de caracteres que descreve o local do arquivo. Depois que o objeto **FileIOPermission** é definido, você só precisa chamar seu método **Assert** para substituir a verificação de segurança.  
  
```vb  
Option Explicit  
Option Strict  
Imports System  
Imports System.IO  
Imports System.Security.Permissions  
Namespace LogUtil  
   Public Class Log  
      Public Sub New()  
      End Sub 'New  
  
      Public Sub MakeLog()  
         Dim FilePermission As New FileIOPermission(FileIOPermissionAccess.AllAccess, "C:\Log.txt")  
         FilePermission.Assert()  
         Dim TextStream As New StreamWriter("C:\Log.txt")  
         TextStream.WriteLine("This  Log was created on {0}", DateTime.Now)  
         TextStream.Close()  
      End Sub  
   End Class  
End Namespace  
```  
  
```csharp  
namespace LogUtil  
{  
   using System;  
   using System.IO;  
   using System.Security.Permissions;  
  
   public class Log  
   {  
      public Log()  
      {
      }
      public void MakeLog()  
      {  
         FileIOPermission FilePermission = new FileIOPermission(FileIOPermissionAccess.AllAccess,@"C:\Log.txt");
         FilePermission.Assert();  
         StreamWriter TextStream = new StreamWriter(@"C:\Log.txt");  
         TextStream.WriteLine("This  Log was created on {0}", DateTime.Now);  
         TextStream.Close();  
      }  
   }  
}  
```  
  
## <a name="see-also"></a>Veja também

- <xref:System.Security.PermissionSet>
- <xref:System.Security.Permissions.SecurityPermission>
- <xref:System.Security.Permissions.FileIOPermission>
- <xref:System.Security.Permissions.SecurityAction>
- [Atributos](../../standard/attributes/index.md)
- [Segurança de acesso do código](code-access-security.md)
