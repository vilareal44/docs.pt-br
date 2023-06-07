---
title: Como executar código parcialmente confiável em uma área restrita
description: Descubra como executar código parcialmente confiável em uma área restrita no .NET. A classe AppDomain é uma maneira eficaz de colocar aplicativos gerenciados em área restrita.
ms.date: 03/30/2017
helpviewer_keywords:
- partially trusted code
- sandboxing
- partial trust
- restricted security environment
- code security, sandboxing
ms.assetid: d1ad722b-5b49-4040-bff3-431b94bb8095
ms.openlocfilehash: 415a42f7c4f4866bb72f19bdd6f02bfdb5158bf8
ms.sourcegitcommit: c37e8d4642fef647ebab0e1c618ecc29ddfe2a0f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/06/2020
ms.locfileid: "87855797"
---
# <a name="how-to-run-partially-trusted-code-in-a-sandbox"></a>Como executar código parcialmente confiável em uma área restrita

[!INCLUDE[net_security_note](../../../includes/net-security-note-md.md)]  
  
 A área restrita é a prática de executar código em um ambiente de segurança restrito, que limita as permissões de acesso concedidas ao código. Por exemplo, se você tiver uma biblioteca gerenciada de uma fonte que não confia completamente, não deverá executá-la como totalmente confiável. Em vez disso, você deve colocar o código em uma área restrita que limita suas permissões para aqueles que você espera que sejam necessários (por exemplo, <xref:System.Security.Permissions.SecurityPermissionFlag.Execution> permissão).  
  
 Você também pode usar a área restrita para testar o código que será distribuído e que será executado em ambientes parcialmente confiáveis.  
  
 Um <xref:System.AppDomain> é uma maneira eficaz de fornecer uma área restrita para aplicativos gerenciados. Os domínios de aplicativo que são usados para executar código parcialmente confiável têm permissões que definem os recursos protegidos que estão disponíveis durante a execução dentro dele <xref:System.AppDomain> . O código que é executado dentro do <xref:System.AppDomain> é associado pelas permissões associadas ao <xref:System.AppDomain> e tem permissão para acessar apenas os recursos especificados. O <xref:System.AppDomain> também inclui uma <xref:System.Security.Policy.StrongName> matriz que é usada para identificar assemblies que devem ser carregados como totalmente confiáveis. Isso permite que o criador de um <xref:System.AppDomain> inicie um novo domínio em área restrita que permite que assemblies auxiliares específicos sejam totalmente confiáveis. Outra opção para carregar assemblies como totalmente confiável é colocá-los no cache de assembly global; no entanto, isso carregará os assemblies como totalmente confiáveis em todos os domínios de aplicativo criados nesse computador. A lista de nomes fortes dá suporte a uma por <xref:System.AppDomain> decisão que fornece uma determinação mais restritiva.  
  
 Você pode usar a <xref:System.AppDomain.CreateDomain%28System.String%2CSystem.Security.Policy.Evidence%2CSystem.AppDomainSetup%2CSystem.Security.PermissionSet%2CSystem.Security.Policy.StrongName%5B%5D%29?displayProperty=nameWithType> sobrecarga do método para especificar o conjunto de permissões para aplicativos que são executados em uma área restrita. Essa sobrecarga permite que você especifique o nível exato de segurança de acesso do código desejado. Os assemblies que são carregados em um usando <xref:System.AppDomain> essa sobrecarga podem ter apenas o Grant Set especificado ou podem ser totalmente confiáveis. O assembly terá confiança total se estiver no cache de assembly global ou listado no `fullTrustAssemblies` parâmetro de matriz (o <xref:System.Security.Policy.StrongName> ). Somente os assemblies conhecidos como totalmente confiáveis devem ser adicionados à `fullTrustAssemblies` lista.  
  
 A sobrecarga tem a seguinte assinatura:  
  
```csharp
AppDomain.CreateDomain( string friendlyName,  
                        Evidence securityInfo,  
                        AppDomainSetup info,  
                        PermissionSet grantSet,  
                        params StrongName[] fullTrustAssemblies);  
```  
  
 Os parâmetros para a <xref:System.AppDomain.CreateDomain%28System.String%2CSystem.Security.Policy.Evidence%2CSystem.AppDomainSetup%2CSystem.Security.PermissionSet%2CSystem.Security.Policy.StrongName%5B%5D%29> sobrecarga do método especificam o nome do <xref:System.AppDomain> , a evidência para o <xref:System.AppDomain> , o <xref:System.AppDomainSetup> objeto que identifica a base do aplicativo para a área restrita, o conjunto de permissões a ser usado e os nomes fortes para assemblies totalmente confiáveis.  
  
 Por motivos de segurança, a base do aplicativo especificada no `info` parâmetro não deve ser a base do aplicativo para o aplicativo de hospedagem.  
  
 Para o `grantSet` parâmetro, você pode especificar um conjunto de permissões que você criou explicitamente ou um conjunto de permissões padrão criado pelo <xref:System.Security.SecurityManager.GetStandardSandbox%2A> método.  
  
 Ao contrário <xref:System.AppDomain> da maioria das cargas, a evidência para o <xref:System.AppDomain> (que é fornecida pelo `securityInfo` parâmetro) não é usada para determinar o conjunto de concessão para os assemblies parcialmente confiáveis. Em vez disso, ele é especificado independentemente pelo `grantSet` parâmetro. No entanto, a evidência pode ser usada para outras finalidades, como determinar o escopo de armazenamento isolado.  
  
### <a name="to-run-an-application-in-a-sandbox"></a>Para executar um aplicativo em uma área restrita  
  
1. Crie o conjunto de permissões a ser concedido ao aplicativo não confiável. A permissão mínima que você pode conceder é a <xref:System.Security.Permissions.SecurityPermissionFlag.Execution> permissão. Você também pode conceder permissões adicionais que você ache que podem ser seguras para código não confiável; por exemplo, <xref:System.Security.Permissions.IsolatedStorageFilePermission> . O código a seguir cria um novo conjunto de permissões com apenas a <xref:System.Security.Permissions.SecurityPermissionFlag.Execution> permissão.  
  
    ```csharp
    PermissionSet permSet = new PermissionSet(PermissionState.None);  
    permSet.AddPermission(new SecurityPermission(SecurityPermissionFlag.Execution));  
    ```  
  
     Como alternativa, você pode usar um conjunto de permissões nomeadas existente, como a Internet.  
  
    ```csharp
    Evidence ev = new Evidence();  
    ev.AddHostEvidence(new Zone(SecurityZone.Internet));  
    PermissionSet internetPS = SecurityManager.GetStandardSandbox(ev);  
    ```  
  
     O <xref:System.Security.SecurityManager.GetStandardSandbox%2A> método retorna um `Internet` conjunto de permissões ou um `LocalIntranet` conjunto de permissões dependendo da zona na evidência. <xref:System.Security.SecurityManager.GetStandardSandbox%2A>também constrói permissões de identidade para alguns dos objetos de evidência passados como referências.  
  
2. Assine o assembly que contém a classe de hospedagem (denominada `Sandboxer` neste exemplo) que chama o código não confiável. Adicione o <xref:System.Security.Policy.StrongName> usado para assinar o assembly à <xref:System.Security.Policy.StrongName> matriz do `fullTrustAssemblies` parâmetro da <xref:System.AppDomain.CreateDomain%2A> chamada. A classe de hospedagem deve ser executada como totalmente confiável para habilitar a execução do código de confiança parcial ou para oferecer serviços ao aplicativo de confiança parcial. É assim que você lê o <xref:System.Security.Policy.StrongName> de um assembly:  
  
    ```csharp
    StrongName fullTrustAssembly = typeof(Sandboxer).Assembly.Evidence.GetHostEvidence<StrongName>();  
    ```  
  
     .NET Framework assemblies como mscorlib e System.dll não precisam ser adicionados à lista de confiança total porque eles são carregados como totalmente confiáveis do cache de assembly global.  
  
3. Inicialize o <xref:System.AppDomainSetup> parâmetro do <xref:System.AppDomain.CreateDomain%2A> método. Com esse parâmetro, você pode controlar muitas das configurações do novo <xref:System.AppDomain> . A <xref:System.AppDomainSetup.ApplicationBase%2A> propriedade é uma configuração importante e deve ser diferente da <xref:System.AppDomainSetup.ApplicationBase%2A> propriedade para o <xref:System.AppDomain> do aplicativo de hospedagem. Se as <xref:System.AppDomainSetup.ApplicationBase%2A> configurações forem as mesmas, o aplicativo de confiança parcial poderá fazer com que o aplicativo de hospedagem seja carregado (como totalmente confiável) uma exceção que ele define, explorando-o. Essa é outra razão pela qual uma captura (exceção) não é recomendada. Definir a base de aplicativos do host de forma diferente da base de aplicativos do aplicativo de área restrita reduz o risco de explorações.  
  
    ```csharp
    AppDomainSetup adSetup = new AppDomainSetup();  
    adSetup.ApplicationBase = Path.GetFullPath(pathToUntrusted);  
    ```  
  
4. Chame a <xref:System.AppDomain.CreateDomain%28System.String%2CSystem.Security.Policy.Evidence%2CSystem.AppDomainSetup%2CSystem.Security.PermissionSet%2CSystem.Security.Policy.StrongName%5B%5D%29> sobrecarga do método para criar o domínio do aplicativo usando os parâmetros que especificamos.  
  
     A assinatura para esse método é:  
  
    ```csharp
    public static AppDomain CreateDomain(string friendlyName,
        Evidence securityInfo, AppDomainSetup info, PermissionSet grantSet,
        params StrongName[] fullTrustAssemblies)  
    ```  
  
     Informações adicionais:  
  
    - Essa é a única sobrecarga do <xref:System.AppDomain.CreateDomain%2A> método que usa um <xref:System.Security.PermissionSet> como parâmetro e, portanto, a única sobrecarga que permite carregar um aplicativo em uma configuração de confiança parcial.  
  
    - O `evidence` parâmetro não é usado para calcular um conjunto de permissões; ele é usado para identificação por outros recursos do .NET Framework.  
  
    - A definição da <xref:System.AppDomainSetup.ApplicationBase%2A> Propriedade do `info` parâmetro é obrigatória para essa sobrecarga.  
  
    - O `fullTrustAssemblies` parâmetro tem a `params` palavra-chave, o que significa que não é necessário criar uma <xref:System.Security.Policy.StrongName> matriz. Passar 0, 1 ou nomes mais fortes como parâmetros é permitido.  
  
    - O código para criar o domínio do aplicativo é:  
  
    ```csharp
    AppDomain newDomain = AppDomain.CreateDomain("Sandbox", null, adSetup, permSet, fullTrustAssembly);  
    ```  
  
5. Carregue o código na área restrita <xref:System.AppDomain> que você criou. Isso pode ser feito de duas maneiras:  
  
    - Chame o <xref:System.AppDomain.ExecuteAssembly%2A> método para o assembly.  
  
    - Use o <xref:System.Activator.CreateInstanceFrom%2A> método para criar uma instância de uma classe derivada de <xref:System.MarshalByRefObject> no novo <xref:System.AppDomain> .  
  
     O segundo método é preferível, pois torna mais fácil passar parâmetros para a nova <xref:System.AppDomain> instância. O <xref:System.Activator.CreateInstanceFrom%2A> método fornece dois recursos importantes:  
  
    - Você pode usar uma base de código que aponta para um local que não contém seu assembly.  
  
    - Você pode fazer a criação em um <xref:System.Security.CodeAccessPermission.Assert%2A> para confiança total ( <xref:System.Security.Permissions.PermissionState.Unrestricted?displayProperty=nameWithType> ), que permite criar uma instância de uma classe crítica. (Isso ocorre sempre que seu assembly não tem marcações de transparência e é carregado como totalmente confiável.) Portanto, você precisa ter cuidado para criar apenas o código em que confia com essa função e é recomendável criar apenas instâncias de classes totalmente confiáveis no novo domínio do aplicativo.  
  
    ```csharp
    ObjectHandle handle = Activator.CreateInstanceFrom(  
    newDomain, typeof(Sandboxer).Assembly.ManifestModule.FullyQualifiedName,  
           typeof(Sandboxer).FullName );  
    ```  
  
     Para criar uma instância de uma classe em um novo domínio, a classe deve estender a <xref:System.MarshalByRefObject> classe.
  
    ```csharp
    class Sandboxer:MarshalByRefObject  
    ```  
  
6. Desenvolva a nova instância de domínio em uma referência neste domínio. Essa referência é usada para executar o código não confiável.  
  
    ```csharp
    Sandboxer newDomainInstance = (Sandboxer) handle.Unwrap();  
    ```  
  
7. Chame o `ExecuteUntrustedCode` método na instância da `Sandboxer` classe que você acabou de criar.  
  
    ```csharp
    newDomainInstance.ExecuteUntrustedCode(untrustedAssembly, untrustedClass, entryPoint, parameters);  
    ```  
  
     Essa chamada é executada no domínio de aplicativo em área restrita, que tem permissões restritas.  
  
    ```csharp
    public void ExecuteUntrustedCode(string assemblyName, string typeName, string entryPoint, Object[] parameters)  
    {  
        //Load the MethodInfo for a method in the new assembly. This might be a method you know, or
        //you can use Assembly.EntryPoint to get to the entry point in an executable.  
        MethodInfo target = Assembly.Load(assemblyName).GetType(typeName).GetMethod(entryPoint);  
        try  
        {  
            // Invoke the method.  
            target.Invoke(null, parameters);  
        }  
        catch (Exception ex)  
        {  
        //When information is obtained from a SecurityException extra information is provided if it is
        //accessed in full-trust.  
            new PermissionSet(PermissionState.Unrestricted).Assert();  
            Console.WriteLine("SecurityException caught:\n{0}", ex.ToString());  
            CodeAccessPermission.RevertAssert();  
            Console.ReadLine();  
        }  
    }  
    ```  
  
     <xref:System.Reflection>é usado para obter um identificador de um método no assembly parcialmente confiável. O identificador pode ser usado para executar código de forma segura com permissões mínimas.  
  
     No código anterior, observe o <xref:System.Security.PermissionSet.Assert%2A> para a permissão de confiança total antes de imprimir o <xref:System.Security.SecurityException> .  
  
    ```csharp
    new PermissionSet(PermissionState.Unrestricted).Assert()  
    ```  
  
     A declaração de confiança total é usada para obter informações estendidas do <xref:System.Security.SecurityException> . Sem o <xref:System.Security.PermissionSet.Assert%2A> , o <xref:System.Security.SecurityException.ToString%2A> método do <xref:System.Security.SecurityException> irá descobrir que há código parcialmente confiável na pilha e restringirá as informações retornadas. Isso pode causar problemas de segurança se o código de confiança parcial pudesse ler essas informações, mas o risco é mitigado não concedendo <xref:System.Security.Permissions.UIPermission> . A declaração de confiança total deve ser usada com moderação e somente quando você tiver certeza de que não está permitindo que o código de confiança parcial eleve a confiança total. Como regra, não chame o código que você não confia na mesma função e depois de chamar uma declaração para confiança total. É recomendável sempre reverter a declaração quando terminar de usá-la.  
  
## <a name="example"></a>Exemplo  
 O exemplo a seguir implementa o procedimento na seção anterior. No exemplo, um projeto chamado `Sandboxer` em uma solução do Visual Studio também contém um projeto chamado `UntrustedCode` , que implementa a classe `UntrustedClass` . Esse cenário pressupõe que você tenha baixado um assembly de biblioteca que contém um método que deve retornar `true` ou `false` para indicar se o número fornecido é um número Fibonacci. Em vez disso, o método tenta ler um arquivo do seu computador. O exemplo a seguir mostra o código não confiável.  
  
```csharp
using System;  
using System.IO;  
namespace UntrustedCode  
{  
    public class UntrustedClass  
    {  
        // Pretend to be a method checking if a number is a Fibonacci  
        // but which actually attempts to read a file.  
        public static bool IsFibonacci(int number)  
        {  
           File.ReadAllText("C:\\Temp\\file.txt");  
           return false;  
        }  
    }  
}  
```  
  
 O exemplo a seguir mostra o `Sandboxer` código do aplicativo que executa o código não confiável.  
  
```csharp
using System;  
using System.Collections.Generic;  
using System.Linq;  
using System.Text;  
using System.IO;  
using System.Security;  
using System.Security.Policy;  
using System.Security.Permissions;  
using System.Reflection;  
using System.Runtime.Remoting;  
  
//The Sandboxer class needs to derive from MarshalByRefObject so that we can create it in another
// AppDomain and refer to it from the default AppDomain.  
class Sandboxer : MarshalByRefObject  
{  
    const string pathToUntrusted = @"..\..\..\UntrustedCode\bin\Debug";  
    const string untrustedAssembly = "UntrustedCode";  
    const string untrustedClass = "UntrustedCode.UntrustedClass";  
    const string entryPoint = "IsFibonacci";  
    private static Object[] parameters = { 45 };  
    static void Main()  
    {  
        //Setting the AppDomainSetup. It is very important to set the ApplicationBase to a folder
        //other than the one in which the sandboxer resides.  
        AppDomainSetup adSetup = new AppDomainSetup();  
        adSetup.ApplicationBase = Path.GetFullPath(pathToUntrusted);  
  
        //Setting the permissions for the AppDomain. We give the permission to execute and to
        //read/discover the location where the untrusted code is loaded.  
        PermissionSet permSet = new PermissionSet(PermissionState.None);  
        permSet.AddPermission(new SecurityPermission(SecurityPermissionFlag.Execution));  
  
        //We want the sandboxer assembly's strong name, so that we can add it to the full trust list.  
        StrongName fullTrustAssembly = typeof(Sandboxer).Assembly.Evidence.GetHostEvidence<StrongName>();  
  
        //Now we have everything we need to create the AppDomain, so let's create it.  
        AppDomain newDomain = AppDomain.CreateDomain("Sandbox", null, adSetup, permSet, fullTrustAssembly);  
  
        //Use CreateInstanceFrom to load an instance of the Sandboxer class into the  
        //new AppDomain.
        ObjectHandle handle = Activator.CreateInstanceFrom(  
            newDomain, typeof(Sandboxer).Assembly.ManifestModule.FullyQualifiedName,  
            typeof(Sandboxer).FullName  
            );  
        //Unwrap the new domain instance into a reference in this domain and use it to execute the
        //untrusted code.  
        Sandboxer newDomainInstance = (Sandboxer) handle.Unwrap();  
        newDomainInstance.ExecuteUntrustedCode(untrustedAssembly, untrustedClass, entryPoint, parameters);  
    }  
    public void ExecuteUntrustedCode(string assemblyName, string typeName, string entryPoint, Object[] parameters)  
    {  
        //Load the MethodInfo for a method in the new Assembly. This might be a method you know, or
        //you can use Assembly.EntryPoint to get to the main function in an executable.  
        MethodInfo target = Assembly.Load(assemblyName).GetType(typeName).GetMethod(entryPoint);  
        try  
        {  
            //Now invoke the method.  
            bool retVal = (bool)target.Invoke(null, parameters);  
        }  
        catch (Exception ex)  
        {  
            // When we print informations from a SecurityException extra information can be printed if we are
            //calling it with a full-trust stack.  
            new PermissionSet(PermissionState.Unrestricted).Assert();  
            Console.WriteLine("SecurityException caught:\n{0}", ex.ToString());  
            CodeAccessPermission.RevertAssert();  
            Console.ReadLine();  
        }  
    }  
}  
```  
  
## <a name="see-also"></a>Veja também

- [Diretrizes de codificação segura](../../standard/security/secure-coding-guidelines.md)
