---
title: Noções básicas da segurança de acesso do código
description: 'Aprenda noções básicas de segurança de acesso ao código para aplicativos destinados ao CLR: código de tipo seguro, sintaxe imperativa e declarativa, bibliotecas de classes seguras e código Transparent.'
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
helpviewer_keywords:
- security [.NET Framework], code access security
ms.assetid: 4eaa6535-d9fe-41a1-91d8-b437cfc16921
ms.openlocfilehash: 9d1f2e35c79ca32595711316885991717c4c1696
ms.sourcegitcommit: 97ce5363efa88179dd76e09de0103a500ca9b659
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/13/2020
ms.locfileid: "86281740"
---
# <a name="code-access-security-basics"></a>Noções básicas da segurança de acesso do código

[!INCLUDE[net_security_note](../../../includes/net-security-note-md.md)]

Todos os aplicativos que se destinam à Common Language Runtime (ou seja, todos os aplicativos gerenciados) devem interagir com o sistema de segurança do tempo de execução. Quando um aplicativo gerenciado é carregado, seu host o concede automaticamente a ele um conjunto de permissões. Essas permissões são determinadas pelas configurações de segurança local do host ou pela área restrita em que o aplicativo está. Dependendo dessas permissões, o aplicativo é executado corretamente ou gera uma exceção de segurança.

O host padrão para aplicativos de área de trabalho permite que o código seja executado em confiança total. Portanto, se o seu aplicativo for direcionado para a área de trabalho, ele terá um conjunto de permissões irrestrito. Outros hosts ou áreas restritas fornecem um conjunto de permissões limitado para aplicativos. Como o conjunto de permissões pode ser alterado de host para host, você deve projetar seu aplicativo para usar apenas as permissões que o host de destino permite.

Você deve estar familiarizado com os seguintes conceitos de segurança de acesso ao código para escrever aplicativos efetivos direcionados ao Common Language Runtime:

- **Código de tipo seguro**: o código de tipo seguro é o código que acessa tipos somente de maneiras bem definidas e permitidas. Por exemplo, considerando uma referência de objeto válida, o código de tipo seguro pode acessar a memória em deslocamentos fixos que correspondem aos membros reais do campo. Se o código acessar a memória em deslocamentos arbitrários fora do intervalo de memória que pertence aos campos expostos publicamente do objeto, ele não será de tipo seguro. Para permitir que o código se beneficie da segurança de acesso ao código, você deve usar um compilador que gera um código de tipo seguro com verificação. Para obter mais informações, consulte a seção [escrevendo código com segurança de tipo verificável](#typesafe_code) mais adiante neste tópico.

- **Sintaxe imperativa e declarativa**: o código que tem como alvo o Common Language Runtime pode interagir com o sistema de segurança solicitando permissões, exigindo que os chamadores tenham permissões especificadas e substituindo determinadas configurações de segurança (dadas privilégios suficientes). Você usa duas formas de sintaxe para interagir de forma programática com o .NET Framework sistema de segurança: sintaxe declarativa e sintaxe imperativa. Chamadas declarativas são executadas usando atributos; as chamadas imperativas são executadas usando novas instâncias de classes dentro de seu código. Algumas chamadas podem ser executadas apenas imperativamente, outras podem ser executadas apenas de forma declarativa e algumas chamadas podem ser executadas de qualquer maneira.

- **Bibliotecas de classes seguras**: uma biblioteca de classes segura usa as demandas de segurança para garantir que os chamadores da biblioteca tenham permissão para acessar os recursos que a biblioteca expõe. Por exemplo, uma biblioteca de classes segura pode ter um método para criar arquivos que exigem que seus chamadores tenham permissões para criar arquivos. O .NET Framework consiste em bibliotecas de classes seguras. Você deve estar ciente das permissões necessárias para acessar qualquer biblioteca que seu código usa. Para obter mais informações, consulte a seção [usando bibliotecas de classes seguras](#secure_library) mais adiante neste tópico.

- **Código Transparent**: começando com o .NET Framework 4, além de identificar permissões específicas, você também deve determinar se o seu código deve ser executado como de segurança transparente. O código de segurança transparente não pode chamar tipos ou membros que são identificados como segurança crítica. Essa regra se aplica a aplicativos de confiança total, bem como a aplicativos parcialmente confiáveis. Para obter mais informações, consulte [código de segurança transparente](security-transparent-code.md).

<a name="typesafe_code"></a>

## <a name="writing-verifiably-type-safe-code"></a>Escrevendo um código fortemente tipado verificável

A compilação JIT (just-in-time) executa um processo de verificação que examina o código e tenta determinar se o código é de tipo seguro. O código que é comprovado durante a verificação para ser digitado com segurança é chamado de *código de tipo seguro verificável*. O código pode ser de tipo seguro, mas pode não ser verificável com segurança devido às limitações do processo de verificação ou do compilador. Nem todas as linguagens são de tipo seguro, e alguns compiladores de linguagem, como Microsoft Visual C++, não podem gerar código gerenciado com segurança de tipo verificável. Para determinar se o compilador de linguagem que você usa gera um código com segurança de tipo verificável, consulte a documentação do compilador. Se você usar um compilador de linguagem que gera código de tipo seguro, apenas quando você evita determinadas construções de linguagem, convém usar a [ferramenta PEVerify](../tools/peverify-exe-peverify-tool.md) para determinar se o seu código é verificamente seguro.

O código que não é verificável de tipo seguro pode tentar executar se a política de segurança permitir que o código ignore a verificação. No entanto, como a segurança de tipo é uma parte essencial do mecanismo do tempo de execução para isolar assemblies, a segurança não poderá ser imposta de forma confiável se o código violar as regras de segurança de tipo. Por padrão, o código que não é de tipo seguro pode ser executado somente se for originado do computador local. Portanto, o código móvel deve ser de tipo seguro.

<a name="secure_library"></a>

## <a name="using-secure-class-libraries"></a>Usando bibliotecas de classes seguras

Se seu código solicitar e receber as permissões exigidas pela biblioteca de classes, ele terá permissão para acessar a biblioteca e os recursos expostos pela biblioteca serão protegidos contra o acesso não autorizado. Se o seu código não tiver as permissões apropriadas, ele não terá permissão para acessar a biblioteca de classes e o código mal-intencionado não poderá usar seu código para acessar indiretamente os recursos protegidos. Outro código que chama seu código também deve ter permissão para acessar a biblioteca. Caso contrário, seu código também será impedido de ser executado.

A segurança de acesso ao código não elimina a possibilidade de erro humano em escrever código. No entanto, se seu aplicativo usar bibliotecas de classes seguras para acessar recursos protegidos, o risco de segurança para o código do aplicativo será reduzido, pois as bibliotecas de classes são rigorosamente fiscalizadas para possíveis problemas de segurança.

## <a name="declarative-security"></a>Segurança declarativa

A sintaxe de segurança declarativa usa [atributos](../../standard/attributes/index.md) para posicionar informações de segurança nos [metadados](../../standard/metadata-and-self-describing-components.md) do seu código. Os atributos podem ser colocados no nível de assembly, classe ou membro, para indicar o tipo de solicitação, demanda ou substituição que você deseja usar. As solicitações são usadas em aplicativos direcionados à Common Language Runtime para informar o sistema de segurança de tempo de execução sobre as permissões que seu aplicativo precisa ou não deseja. As demandas e substituições são usadas em bibliotecas para ajudar a proteger recursos de chamadores ou para substituir o comportamento de segurança padrão.

> [!NOTE]
> No .NET Framework 4, houve alterações importantes na .NET Framework modelo e terminologia de segurança. Para obter mais informações sobre essas alterações, consulte [Security Changes](https://docs.microsoft.com/previous-versions/dotnet/framework/security/security-changes).

Para usar chamadas de segurança declarativas, você deve inicializar os dados de estado do objeto de permissão para que ele represente a forma específica de permissão necessária. Cada permissão interna tem um atributo que é passado <xref:System.Security.Permissions.SecurityAction> como uma enumeração para descrever o tipo de operação de segurança que você deseja executar. No entanto, as permissões também aceitam seus próprios parâmetros que são exclusivos para eles.

O fragmento de código a seguir mostra a sintaxe declarativa para solicitar que os chamadores de seu código tenham uma permissão personalizada chamada `MyPermission` . Essa permissão é uma permissão personalizada hipotética e não existe no .NET Framework. Neste exemplo, a chamada declarativa é colocada diretamente antes da definição de classe, especificando que essa permissão seja aplicada ao nível de classe. O atributo é passado uma estrutura **SecurityAction. redemand** para especificar que os chamadores devem ter essa permissão para serem executados.

```vb
<MyPermission(SecurityAction.Demand, Unrestricted = True)> Public Class MyClass1

   Public Sub New()
      'The constructor is protected by the security call.
   End Sub

   Public Sub MyMethod()
      'This method is protected by the security call.
   End Sub

   Public Sub YourMethod()
      'This method is protected by the security call.
   End Sub
End Class
```

```csharp
[MyPermission(SecurityAction.Demand, Unrestricted = true)]
public class MyClass
{
   public MyClass()
   {
      //The constructor is protected by the security call.
   }

   public void MyMethod()
   {
      //This method is protected by the security call.
   }

   public void YourMethod()
   {
      //This method is protected by the security call.
   }
}
```

## <a name="imperative-security"></a>Segurança obrigatória

A sintaxe de segurança imperativa emite uma chamada de segurança criando uma nova instância do objeto de permissão que você deseja invocar. Você pode usar a sintaxe imperativa para executar demandas e substituições, mas não solicitações.

Antes de fazer a chamada de segurança, você deve inicializar os dados de estado do objeto de permissão para que ele represente a forma específica da permissão necessária. Por exemplo, ao criar um <xref:System.Security.Permissions.FileIOPermission> objeto, você pode usar o construtor para inicializar o objeto **FileIOPermission** para que ele represente o acesso irrestrito a todos os arquivos ou nenhum acesso aos arquivos. Ou, você pode usar um objeto **FileIOPermission** diferente, passando parâmetros que indicam o tipo de acesso que você deseja que o objeto represente (ou seja, leitura, acréscimo ou gravação) e quais arquivos você deseja que o objeto proteja.

Além de usar a sintaxe de segurança imperativa para invocar um único objeto de segurança, você pode usá-lo para inicializar um grupo de permissões em um conjunto de permissões. Por exemplo, essa técnica é a única maneira de executar chamadas [Assert](using-the-assert-method.md) de forma confiável em várias permissões em um método. Use as <xref:System.Security.PermissionSet> <xref:System.Security.NamedPermissionSet> classes e para criar um grupo de permissões e, em seguida, chame o método apropriado para invocar a chamada de segurança desejada.

Você pode usar a sintaxe imperativa para executar demandas e substituições, mas não solicitações. Você pode usar a sintaxe imperativa para demandas e substituições em vez de sintaxe declarativa quando as informações necessárias para inicializar o estado de permissão se tornarem conhecidas somente no tempo de execução. Por exemplo, se você quiser garantir que os chamadores tenham permissão para ler um determinado arquivo, mas não souber o nome desse arquivo até o tempo de execução, use uma demanda imperativa. Você também pode optar por usar verificações imperativas em vez de verificações declarativas quando precisar determinar em tempo de execução se uma condição mantiver e, com base no resultado do teste, fazer uma demanda de segurança (ou não).

O fragmento de código a seguir mostra a sintaxe imperativa para solicitar que os chamadores de seu código tenham uma permissão personalizada chamada `MyPermission` . Essa permissão é uma permissão personalizada hipotética e não existe no .NET Framework. Uma nova instância do `MyPermission` é criada no `MyMethod` , protegendo apenas esse método com a chamada de segurança.

```vb
Public Class MyClass1

   Public Sub New()

   End Sub

   Public Sub MyMethod()
      'MyPermission is demanded using imperative syntax.
      Dim Perm As New MyPermission()
      Perm.Demand()
      'This method is protected by the security call.
   End Sub

   Public Sub YourMethod()
      'YourMethod 'This method is not protected by the security call.
   End Sub
End Class
```

```csharp
public class MyClass {
   public MyClass(){

   }

   public void MyMethod() {
       //MyPermission is demanded using imperative syntax.
       MyPermission Perm = new MyPermission();
       Perm.Demand();
       //This method is protected by the security call.
   }

   public void YourMethod() {
       //This method is not protected by the security call.
   }
}
```

## <a name="using-managed-wrapper-classes"></a>Usando classes de wrapper gerenciadas

A maioria dos aplicativos e componentes (exceto bibliotecas seguras) não deve chamar diretamente o código não gerenciado. Há várias razões para isso. Se o código chamar o código não gerenciado diretamente, ele não poderá ser executado em muitas circunstâncias porque o código deve receber um alto nível de confiança para chamar o código nativo. Se a política for modificada para permitir que tal aplicativo seja executado, ele poderá reduzir significativamente a segurança do sistema, deixando o aplicativo livre para executar praticamente qualquer operação.

Além disso, o código que tem permissão para acessar o código não gerenciado pode, provavelmente, executar quase qualquer operação chamando uma API não gerenciada. Por exemplo, o código que tem permissão para chamar código não gerenciado não precisa <xref:System.Security.Permissions.FileIOPermission> acessar um arquivo; ele pode simplesmente chamar uma API de arquivo não gerenciado (Win32) diretamente, ignorando a API de arquivo gerenciado que requer **FileIOPermission**. Se o código gerenciado tiver permissão para chamar o código não gerenciado e chamar diretamente o código não gerenciado, o sistema de segurança não poderá impor restrições de segurança de forma confiável, já que o tempo de execução não pode impor essas restrições em código não gerenciado.

Se você quiser que seu aplicativo execute uma operação que requer o acesso a código não gerenciado, ele deve fazer isso por meio de uma classe gerenciada confiável que encapsula a funcionalidade necessária (se tal classe existir). Não crie uma classe wrapper por conta própria se já existir uma em uma biblioteca de classes segura. A classe wrapper, que deve receber um alto grau de confiança para ter permissão para fazer a chamada em código não gerenciado, é responsável por exigir que seus chamadores tenham as permissões apropriadas. Se você usar a classe wrapper, seu código só precisará solicitar e receber as permissões exigidas pela classe wrapper.

## <a name="see-also"></a>Confira também

- <xref:System.Security.PermissionSet>
- <xref:System.Security.Permissions.FileIOPermission>
- <xref:System.Security.NamedPermissionSet>
- <xref:System.Security.Permissions.SecurityAction>
- [Declarar](using-the-assert-method.md)
- [Segurança de acesso do código](code-access-security.md)
- [Noções básicas da segurança de acesso do código](code-access-security-basics.md)
- [Atributos](../../standard/attributes/index.md)
- [Metadados e componentes autodescritivos](../../standard/metadata-and-self-describing-components.md)
