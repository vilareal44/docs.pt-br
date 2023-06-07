---
title: Protegendo o acesso dos métodos
description: Saiba como tornar o método acesso seguro para métodos que não são destinados ao uso público, mas que ainda precisam ser públicos.
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
helpviewer_keywords:
- code security, method access
- secure coding, method access
- security [.NET Framework], method access
- method access security
ms.assetid: f7c2d6ec-3b18-4e0e-9991-acd97189d818
ms.openlocfilehash: 88868ab29fc37854959a044b9c0fed5bd8c82d77
ms.sourcegitcommit: c37e8d4642fef647ebab0e1c618ecc29ddfe2a0f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/06/2020
ms.locfileid: "87855758"
---
# <a name="securing-method-access"></a>Protegendo o acesso dos métodos

[!INCLUDE[net_security_note](../../../includes/net-security-note-md.md)]  
  
 Alguns métodos podem não ser adequados para permitir que um código não confiável arbitrário chame. Esses métodos apresentam vários riscos: o método pode fornecer algumas informações restritas; pode acreditar que todas as informações sejam passadas para ele; Ele pode não fazer a verificação de erro nos parâmetros; ou com os parâmetros errados, ele pode não funcionar ou fazer algo prejudicial. Você deve estar atento a esses casos e tomar medidas para ajudar a proteger o método.  
  
 Em alguns casos, talvez seja necessário restringir métodos que não são destinados ao uso público, mas que ainda devem ser públicos. Por exemplo, você pode ter uma interface que precisa ser chamada em suas próprias DLLs e, portanto, deve ser pública, mas você não deseja expô-las publicamente para impedir que os clientes as usem ou para impedir que códigos mal-intencionados explorem o ponto de entrada em seu componente. Outro motivo comum para restringir um método não destinado ao uso público (mas que deve ser público) é evitar ter que documentar e dar suporte ao que pode ser uma interface interna.  
  
 O código gerenciado oferece várias maneiras de restringir o acesso ao método:  
  
- Limite o escopo de acessibilidade à classe, ao assembly ou às classes derivadas, se eles puderem ser confiáveis. Essa é a maneira mais simples de limitar o acesso do método. Em geral, as classes derivadas podem ser menos confiáveis do que a classe da qual derivam, embora, em alguns casos, eles compartilhem a identidade da classe pai. Em particular, não infere confiança da palavra-chave `protected` , que não é necessariamente usada no contexto de segurança.  
  
- Limitar o acesso do método a chamadores de uma identidade especificada – essencialmente, qualquer [evidência](https://docs.microsoft.com/previous-versions/dotnet/netframework-4.0/7y5x1hcd%28v=vs.100%29) específica (nome forte, Publicador, zona e assim por diante) que você escolher.  
  
- Limite o método de acesso a chamadores que têm quaisquer permissões que você selecionar.  
  
 Da mesma forma, a segurança declarativa permite que você controle a herança de classes. Você pode usar **InheritanceDemand** para fazer o seguinte:  
  
- Exigir que classes derivadas tenham uma identidade ou permissão especificada.  
  
- Exigir classes derivadas que substituam métodos específicos para ter uma identidade ou permissão especificada.  
  
 O exemplo a seguir mostra como ajudar a proteger uma classe pública para acesso limitado, exigindo que os chamadores sejam assinados com um nome forte específico. Este exemplo usa o <xref:System.Security.Permissions.StrongNameIdentityPermissionAttribute> com uma **demanda** para o nome forte. Para obter informações baseadas em tarefas sobre como assinar um assembly com um nome forte, consulte [criando e usando assemblies de nome forte](../../standard/assembly/create-use-strong-named.md).  
  
```vb  
<StrongNameIdentityPermissionAttribute(SecurityAction.Demand, PublicKey := "…hex…", Name := "App1", Version := "0.0.0.0")>  _  
Public Class Class1  
End Class  
```  
  
```csharp  
[StrongNameIdentityPermissionAttribute(SecurityAction.Demand, PublicKey="…hex…", Name="App1", Version="0.0.0.0")]  
public class Class1  
{  
  
}
```  
  
## <a name="excluding-classes-and-members-from-use-by-untrusted-code"></a>Excluindo classes e membros do uso por código não confiável  
 Use as declarações mostradas nesta seção para impedir que classes e métodos específicos, bem como propriedades e eventos, sejam usados pelo código parcialmente confiável. Aplicando essas declarações a uma classe, você aplica a proteção a todos os seus métodos, propriedades e eventos. No entanto, o acesso ao campo não é afetado pela segurança declarativa. Observe também que as demandas de link ajudam a proteger contra apenas os chamadores imediatos e podem ainda estar sujeitos a ataques chamariz.  
  
> [!NOTE]
> Um novo modelo de transparência foi introduzido no .NET Framework 4. O [código de segurança transparente, modelo de nível 2,](security-transparent-code-level-2.md) identifica o código seguro com o <xref:System.Security.SecurityCriticalAttribute> atributo. O código de segurança crítica exige que os chamadores e os herdeiros sejam totalmente confiáveis. Os assemblies que estão em execução nas regras de segurança de acesso ao código de versões anteriores do .NET Framework podem chamar assemblies de nível 2. Nesse caso, os atributos de segurança crítica serão tratados como demandas de link para confiança total.  
  
 Em assemblies de nome forte, um [LinkDemand](link-demands.md) é aplicado a todos os métodos, propriedades e eventos publicamente acessíveis para restringir seu uso a chamadores totalmente confiáveis. Para desabilitar esse recurso, você deve aplicar o <xref:System.Security.AllowPartiallyTrustedCallersAttribute> atributo. Portanto, marcar explicitamente as classes para excluir chamadores não confiáveis é necessário apenas para assemblies não assinados ou assemblies com esse atributo; Você pode usar essas declarações para marcar um subconjunto de tipos que não se destinam a chamadores não confiáveis.  
  
 Os exemplos a seguir mostram como impedir que classes e membros sejam usados por código não confiável.  
  
 Para classes não seladas públicas:  
  
```vb  
<System.Security.Permissions.PermissionSetAttribute(System.Security.Permissions.SecurityAction.InheritanceDemand, Name := "FullTrust"), _
System.Security.Permissions.PermissionSetAttribute(System.Security.Permissions.SecurityAction.LinkDemand, Name := "FullTrust")>  _  
Public Class CanDeriveFromMe  
End Class  
```  
  
```csharp  
[System.Security.Permissions.PermissionSetAttribute(System.Security.Permissions.SecurityAction.InheritanceDemand, Name="FullTrust")]  
[System.Security.Permissions.PermissionSetAttribute(System.Security.Permissions.SecurityAction.LinkDemand, Name="FullTrust")]  
public class CanDeriveFromMe  
{  
}  
```  
  
 Para classes lacradas públicas:  
  
```vb  
<System.Security.Permissions.PermissionSetAttribute(System.Security.Permissions.SecurityAction.LinkDemand, Name := "FullTrust")>  _  
NotInheritable Public Class CannotDeriveFromMe  
End Class  
```  
  
```csharp  
[System.Security.Permissions.PermissionSetAttribute(System.Security.Permissions.SecurityAction.LinkDemand, Name="FullTrust")]  
public sealed class CannotDeriveFromMe  
{  
}  
```  
  
 Para classes abstratas públicas:  
  
```vb  
<System.Security.Permissions.PermissionSetAttribute(System.Security.Permissions.SecurityAction.InheritanceDemand, Name := "FullTrust"), _  
System.Security.Permissions.PermissionSetAttribute(System.Security.Permissions.SecurityAction.LinkDemand, Name := "FullTrust")>  _  
MustInherit Public Class CannotCreateInstanceOfMe_CanCastToMe  
End Class  
```  
  
```csharp  
[System.Security.Permissions.PermissionSetAttribute(System.Security.Permissions.SecurityAction.InheritanceDemand, Name="FullTrust")]  
[System.Security.Permissions.PermissionSetAttribute(System.Security.Permissions.SecurityAction.LinkDemand, Name="FullTrust")]  
public abstract class CannotCreateInstanceOfMe_CanCastToMe {}  
```  
  
 Para funções virtuais públicas:  
  
```vb  
Class Base1
<System.Security.Permissions.PermissionSetAttribute(System.Security.Permissions.SecurityAction.InheritanceDemand, Name:="FullTrust"), System.Security.Permissions.PermissionSetAttribute(System.Security.Permissions.SecurityAction.LinkDemand, Name:="FullTrust")> _  
    Public Overridable Sub CanOverrideOrCallMe()  
    End Sub 'CanOverrideOrCallMe  
End Class 'Base1  
```  
  
```csharp  
class Base1
{  
[System.Security.Permissions.PermissionSetAttribute(  
System.Security.Permissions.SecurityAction.InheritanceDemand, Name="FullTrust")]  
[System.Security.Permissions.PermissionSetAttribute(  
System.Security.Permissions.SecurityAction.LinkDemand, Name="FullTrust")]  
    public virtual void CanOverrideOrCallMe() {}  
}  
```  
  
 Para funções abstratas públicas:  
  
```vb  
MustInherit Class Base2  
    <System.Security.Permissions.PermissionSetAttribute(System.Security.Permissions.SecurityAction.InheritanceDemand, Name:="FullTrust"), System.Security.Permissions.PermissionSetAttribute(System.Security.Permissions.SecurityAction.LinkDemand, Name:="FullTrust")> _  
    Public Sub MustOverrideMe()  
    End Sub  
End Class 'Base2  
```  
  
```csharp  
abstract class Base2 {  
[System.Security.Permissions.PermissionSetAttribute(  
System.Security.Permissions.SecurityAction.InheritanceDemand, Name = "FullTrust")]  
[System.Security.Permissions.PermissionSetAttribute(  
System.Security.Permissions.SecurityAction.LinkDemand, Name = "FullTrust")]  
public abstract void MustOverrideMe();  
}  
```  
  
 Para funções de substituição pública em que a classe base não exige confiança total:  
  
```vb  
Class Derived  
    Inherits Base1  
<System.Security.Permissions.PermissionSetAttribute(System.Security.Permissions.SecurityAction.Demand, Name:="FullTrust")> _  
    Public Overrides Sub CanOverrideOrCallMe()  
        MyBase.CanOverrideOrCallMe()  
    End Sub 'CanOverrideOrCallMe  
End Class 'Derived  
```  
  
```csharp  
class Derived : Base1  
{
[System.Security.Permissions.PermissionSetAttribute(System.Security.Permissions.SecurityAction.Demand, Name="FullTrust")]
    public override void CanOverrideOrCallMe()
    {  
        base.CanOverrideOrCallMe();  
    }  
}  
```  
  
 Para funções de substituição pública em que a classe base exige confiança total:  
  
```vb  
Class Derived  
    Inherits Base1  
<System.Security.Permissions.PermissionSetAttribute(System.Security.Permissions.SecurityAction.LinkDemand, Name:="FullTrust")> _  
    Public Overrides Sub CanOverrideOrCallMe()  
        MyBase.CanOverrideOrCallMe()  
    End Sub 'CanOverrideOrCallMe
End Class 'Derived  
```  
  
```csharp  
class Derived : Base1  
{
[System.Security.Permissions.PermissionSetAttribute(System.Security.Permissions.SecurityAction.LinkDemand, Name="FullTrust")]
    public override void CanOverrideOrCallMe()
    {  
        base.CanOverrideOrCallMe();  
    }  
}  
```  
  
 Para interfaces públicas:  
  
```vb  
Public Interface ICanCastToMe  
    <System.Security.Permissions.PermissionSetAttribute(System.Security.Permissions.SecurityAction.LinkDemand, Name:="FullTrust"), System.Security.Permissions.PermissionSetAttribute(System.Security.Permissions.SecurityAction.InheritanceDemand, Name:="FullTrust")> _  
    Sub CanImplementMe()  
End Interface 'ICanCastToMe  
<System.Security.Permissions.PermissionSetAttribute(System.Security.Permissions.SecurityAction.LinkDemand, Name:="FullTrust"), System.Security.Permissions.PermissionSetAttribute(System.Security.Permissions.SecurityAction.InheritanceDemand, Name:="FullTrust")> _  
Class Implemented  
    Implements ICanCastToMe  
    Public Sub CanImplementMe()  
    End Sub 'CanImplementMe  
```  
  
```csharp  
public interface ICanCastToMe
{  
[System.Security.Permissions.PermissionSetAttribute(System.Security.Permissions.SecurityAction.LinkDemand, Name = "FullTrust")]  
[System.Security.Permissions.PermissionSetAttribute(System.Security.Permissions.SecurityAction.InheritanceDemand, Name = "FullTrust")]  
void CanImplementMe();  
}  
[System.Security.Permissions.PermissionSetAttribute(System.Security.Permissions.SecurityAction.LinkDemand, Name = "FullTrust")]  
[System.Security.Permissions.PermissionSetAttribute(System.Security.Permissions.SecurityAction.InheritanceDemand, Name = "FullTrust")]  
class Implemented : ICanCastToMe  
{  
    public void CanImplementMe()  
    {  
    }  
}  
```  
  
## <a name="virtual-internal-overrides-or-overloads-overridable-friend"></a>Substituições com virtual internal ou Overloads Overridable Friend  
  
> [!NOTE]
> Esta seção avisa sobre um problema de segurança ao declarar um método como `virtual` e `internal` ( `Overloads` `Overridable` `Friend` em Visual Basic). Esse aviso se aplica somente às versões 1,0 e 1,1 do .NET Framework e não se aplica a versões posteriores.  
  
 No .NET Framework versões 1,0 e 1,1, você deve estar ciente de uma nuance do tipo acessibilidade do sistema ao confirmar que seu código não está disponível para outros assemblies. Um método declarado **virtual** e **interno** (**sobrecargas Friend presubstituíveis** em Visual Basic) pode substituir a entrada vtable da classe pai e pode ser usado somente de dentro do mesmo assembly porque ele é interno. No entanto, a acessibilidade para substituição é determinada pela palavra-chave **virtual** , e isso pode ser substituído de outro assembly, desde que o código tenha acesso à própria classe. Se a possibilidade de uma substituição apresentar um problema, use a segurança declarativa para corrigi-lo ou remova a palavra-chave **virtual** se ela não for estritamente necessária.  
  
 Mesmo que um compilador de linguagem impeça essas substituições com um erro de compilação, é possível que o código escrito com outros compiladores substitua.  
  
## <a name="see-also"></a>Veja também

- [Diretrizes de codificação segura](../../standard/security/secure-coding-guidelines.md)
