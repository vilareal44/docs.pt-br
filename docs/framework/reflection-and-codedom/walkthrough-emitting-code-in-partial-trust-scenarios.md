---
title: 'Passo a passo: Emitindo o código em cenários de confiança parcial'
description: Consulte como emitir código em cenários de confiança parcial. A emissão de reflexão usa as mesmas APIs, mas alguns recursos precisam de permissões especiais em código parcialmente confiável.
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
helpviewer_keywords:
- reflection emit, anonymously hosted dynamic methods
- partial trust, reflection
- partial trust, emitting dynamic methods
- reflection emit, partial trust scenarios
- anonymously hosted dynamic methods [.NET Framework]
- emitting dynamic assemblies,partial trust scenarios
- reflection emit, dynamic methods
- dynamic methods
ms.assetid: c45be261-2a9d-4c4e-9bd6-27f0931b7d25
ms.openlocfilehash: 70adb3ce67b45459b18741948092a912f6173731
ms.sourcegitcommit: 3d84eac0818099c9949035feb96bbe0346358504
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/21/2020
ms.locfileid: "86865184"
---
# <a name="walkthrough-emitting-code-in-partial-trust-scenarios"></a>Passo a passo: Emitindo o código em cenários de confiança parcial

A emissão de reflexão usa a mesma API definida na confiança total ou parcial, porém alguns recursos exigem permissões especiais em código parcialmente confiável. Além disso, a emissão de reflexão tem um recurso, os métodos dinâmicos hospedados anonimamente, que é projetado para ser usado com confiança parcial e por assemblies transparentes de segurança.

> [!NOTE]
> Antes do .NET Framework 3.5, a emissão de código necessitava de <xref:System.Security.Permissions.ReflectionPermission> com o sinalizador <xref:System.Security.Permissions.ReflectionPermissionFlag.ReflectionEmit?displayProperty=nameWithType>. Essa permissão é incluída por padrão nos conjuntos de permissões denominados `FullTrust` e `Intranet`, mas não no conjunto de permissões `Internet`. Portanto, uma biblioteca poderia ser usada da confiança parcial somente se tivesse o atributo <xref:System.Security.SecurityCriticalAttribute> e também executasse um método <xref:System.Security.PermissionSet.Assert%2A> para <xref:System.Security.Permissions.ReflectionPermissionFlag.ReflectionEmit>. Tais bibliotecas exigem uma análise atenta da segurança, pois erros de código poderiam resultar em falhas de segurança. O .NET Framework 3.5 permite que o código seja emitido em cenários de confiança parcial sem a emissão de nenhuma demanda de segurança, pois a geração de código não é uma operação inerentemente privilegiada. Ou seja, o código gerado não tem mais permissões que o assembly que o emite. Isso permite que bibliotecas que emitem código sejam transparentes de segurança e elimina a necessidade de declarar <xref:System.Security.Permissions.ReflectionPermissionFlag.ReflectionEmit>, de modo que escrever uma biblioteca de segurança não exige uma análise minuciosa da segurança.

Este passo a passo ilustra as seguintes tarefas:

- [Configurando uma área restrita simples para testar código parcialmente confiável](#Setting_up).

  > [!IMPORTANT]
  > Essa é uma maneira simples de experimentar o código em confiança parcial. Para executar código que realmente vem de locais não confiáveis, consulte [Como executar código parcialmente confiável em uma área restrita](../misc/how-to-run-partially-trusted-code-in-a-sandbox.md).

- [Executando código em domínios de aplicativo parcialmente confiável](#Running_code).

- [Usando métodos dinâmicos hospedados anonimamente para emitir e executar código em confiança parcial](#Using_methods).

Para obter mais informações sobre como emitir código em cenários de confiança parcial, consulte [Problemas de segurança na emissão de reflexão](security-issues-in-reflection-emit.md).

Para obter uma lista completa do código mostrado nesses procedimentos, consulte a [Seção de exemplos](#Example) no final deste passo a passo.

<a name="Setting_up"></a>

## <a name="setting-up-partially-trusted-locations"></a>Configurar locais parcialmente confiáveis

Os dois procedimentos a seguir mostram como configurar locais de onde você pode testar o código com confiança parcial.

- O primeiro procedimento mostra como criar um domínio do aplicativo em área restrita no qual o código recebe permissões da Internet.

- O segundo procedimento mostra como adicionar <xref:System.Security.Permissions.ReflectionPermission> com o sinalizador <xref:System.Security.Permissions.ReflectionPermissionFlag.RestrictedMemberAccess?displayProperty=nameWithType> para um domínio do aplicativo parcialmente confiável, a fim de habilitar o acesso aos dados particulares em assemblies de confiança iguais ou inferior.

### <a name="creating-sandboxed-application-domains"></a>Criar domínios do aplicativo em área restrita

Para criar um domínio do aplicativo no qual seus assemblies são executados com confiança parcial, você deve especificar o conjunto de permissões a serem concedidas aos assemblies usando a sobrecarga de método <xref:System.AppDomain.CreateDomain%28System.String%2CSystem.Security.Policy.Evidence%2CSystem.AppDomainSetup%2CSystem.Security.PermissionSet%2CSystem.Security.Policy.StrongName%5B%5D%29?displayProperty=nameWithType> para criar o domínio do aplicativo. A maneira mais fácil de especificar o conjunto de concessões é recuperar um conjunto de permissões nomeadas da política de segurança.

O procedimento a seguir cria um domínio do aplicativo em área restrita que executa seu código com confiança parcial para testar cenários em que o código emitido pode acessar somente membros públicos de tipos públicos. Um procedimento subsequente mostra como adicionar <xref:System.Security.Permissions.ReflectionPermissionFlag.RestrictedMemberAccess> para testar cenários em que o código emitido pode acessar tipos não públicos e membros em assemblies que recebem permissões iguais ou inferiores.

#### <a name="to-create-an-application-domain-with-partial-trust"></a>Criar um domínio do aplicativo com confiança parcial

1. Crie um conjunto de permissões que serão concedidas aos assemblies no domínio do aplicativo em área restrita. Nesse caso, o conjunto de permissões da zona da Internet é usado.

    [!code-csharp[HowToEmitCodeInPartialTrust#2](../../../samples/snippets/csharp/VS_Snippets_CLR/HowToEmitCodeInPartialTrust/cs/source.cs#2)]
    [!code-vb[HowToEmitCodeInPartialTrust#2](../../../samples/snippets/visualbasic/VS_Snippets_CLR/HowToEmitCodeInPartialTrust/vb/source.vb#2)]

2. Crie um objeto <xref:System.AppDomainSetup> para inicializar o domínio do aplicativo com um caminho de aplicativo.

    > [!IMPORTANT]
    > Para simplificar, este exemplo de código usa a pasta atual. Para executar um código que realmente vem da Internet, use uma pasta separada para o código não confiável, conforme descrito em [Como executar código parcialmente confiável em uma área restrita](../misc/how-to-run-partially-trusted-code-in-a-sandbox.md).

    [!code-csharp[HowToEmitCodeInPartialTrust#3](../../../samples/snippets/csharp/VS_Snippets_CLR/HowToEmitCodeInPartialTrust/cs/source.cs#3)]
    [!code-vb[HowToEmitCodeInPartialTrust#3](../../../samples/snippets/visualbasic/VS_Snippets_CLR/HowToEmitCodeInPartialTrust/vb/source.vb#3)]

3. Crie o domínio do aplicativo especificando as informações de configuração de domínio do aplicativo e o conjunto de concessões para todos os assemblies que são executadas no domínio do aplicativo.

    [!code-csharp[HowToEmitCodeInPartialTrust#5](../../../samples/snippets/csharp/VS_Snippets_CLR/HowToEmitCodeInPartialTrust/cs/source.cs#5)]
    [!code-vb[HowToEmitCodeInPartialTrust#5](../../../samples/snippets/visualbasic/VS_Snippets_CLR/HowToEmitCodeInPartialTrust/vb/source.vb#5)]

    O último parâmetro da sobrecarga do método <xref:System.AppDomain.CreateDomain%28System.String%2CSystem.Security.Policy.Evidence%2CSystem.AppDomainSetup%2CSystem.Security.PermissionSet%2CSystem.Security.Policy.StrongName%5B%5D%29?displayProperty=nameWithType> permite que você especifique um conjunto de assemblies que devem receber confiança total, em vez do conjunto de concessões do domínio do aplicativo. Você não precisa especificar os assemblies do .NET Framework que o aplicativo usa, pois tais assemblies estão no cache de assembly global. Assemblies no cache de assembly global sempre são totalmente confiáveis. Você pode usar esse parâmetro para especificar os assemblies de nome forte que não estão no cache de assembly global.

### <a name="adding-restrictedmemberaccess-to-sandboxed-domains"></a>Adicionando RestrictedMemberAccess a domínios em área restrita

Aplicativos host podem permitir que os métodos dinâmicos hospedados anonimamente tenham acesso aos dados particulares em assemblies com níveis de confiança iguais ou inferior ao nível de confiança do assembly que emite o código. Para habilitar essa capacidade restrita de ignorar as verificações de visibilidade JIT (Just-In-Time), o aplicativo host adiciona um objeto <xref:System.Security.Permissions.ReflectionPermission> com o sinalizador <xref:System.Security.Permissions.ReflectionPermissionFlag.RestrictedMemberAccess?displayProperty=nameWithType> (ADM) para o conjunto de concessões.

Por exemplo, um host pode conceder permissões da Internet a aplicativos da Internet mais ADM para que um aplicativo da Internet possa emitir código que acessa dados particulares em seus próprios assemblies. Como o acesso é limitado aos assemblies de confiança igual ou inferior, um aplicativo da Internet não pode acessar membros de assemblies totalmente confiáveis, como assemblies do .NET Framework.

> [!NOTE]
> Para evitar a elevação de privilégio, as informações de pilha para o assembly de emissão são incluídas quando ps métodos dinâmicos hospedados anonimamente são construídos. Quando o método é invocado, as informações de pilha são verificadas. Dessa forma, um método dinâmico hospedado anonimamente que é invocado do código totalmente confiável ainda é limitado ao nível de confiança do assembly de emissão.

#### <a name="to-create-an-application-domain-with-partial-trust-plus-rma"></a>Criar um domínio do aplicativo com confiança parcial mais ADM

1. Crie um novo objeto <xref:System.Security.Permissions.ReflectionPermission> com o sinalizador <xref:System.Security.Permissions.ReflectionPermissionFlag.RestrictedMemberAccess> (ADM) e use o método <xref:System.Security.PermissionSet.SetPermission%2A?displayProperty=nameWithType> para adicionar a permissão ao conjunto de concessões.

    [!code-csharp[HowToEmitCodeInPartialTrust#7](../../../samples/snippets/csharp/VS_Snippets_CLR/HowToEmitCodeInPartialTrust/cs/source.cs#7)]
    [!code-vb[HowToEmitCodeInPartialTrust#7](../../../samples/snippets/visualbasic/VS_Snippets_CLR/HowToEmitCodeInPartialTrust/vb/source.vb#7)]

    O método <xref:System.Security.PermissionSet.AddPermission%2A> adiciona a permissão ao conjunto de concessões se ele não já estiver incluído. Se a permissão já estiver incluída no conjunto de concessões, os sinalizadores especificados serão adicionados à permissão existente.

    > [!NOTE]
    > O ADM é um recurso de métodos dinâmicos hospedados anonimamente. Quando métodos dinâmicos comuns ignoram verificações de visibilidade JIT, o código emitido requer confiança total.

2. Crie o domínio do aplicativo especificando as informações de configuração do domínio do aplicativo e o conjunto de concessões.

    [!code-csharp[HowToEmitCodeInPartialTrust#8](../../../samples/snippets/csharp/VS_Snippets_CLR/HowToEmitCodeInPartialTrust/cs/source.cs#8)]
    [!code-vb[HowToEmitCodeInPartialTrust#8](../../../samples/snippets/visualbasic/VS_Snippets_CLR/HowToEmitCodeInPartialTrust/vb/source.vb#8)]

<a name="Running_code"></a>

## <a name="running-code-in-sandboxed-application-domains"></a>Executando código em domínios do aplicativo em área restrita

O procedimento a seguir explica como definir uma classe com métodos que podem ser executados em um domínio do aplicativo, como criar uma instância da classe do domínio e como executar seus métodos.

#### <a name="to-define-and-execute-a-method-in-an-application-domain"></a>Definir e executar um método em um domínio do aplicativo

1. Defina uma classe derivada de <xref:System.MarshalByRefObject>. Isso permite criar instâncias da classe em outros domínios do aplicativo e fazer chamadas de método entre limites de domínio do aplicativo. Nesse exemplo, a classe é denominada `Worker`.

    [!code-csharp[HowToEmitCodeInPartialTrust#10](../../../samples/snippets/csharp/VS_Snippets_CLR/HowToEmitCodeInPartialTrust/cs/source.cs#10)]
    [!code-vb[HowToEmitCodeInPartialTrust#10](../../../samples/snippets/visualbasic/VS_Snippets_CLR/HowToEmitCodeInPartialTrust/vb/source.vb#10)]

2. Defina um método público que contém o código que você deseja executar. Neste exemplo, o código emite um método dinâmico simples, cria um delegado para executar o método e invoca o delegado.

    [!code-csharp[HowToEmitCodeInPartialTrust#11](../../../samples/snippets/csharp/VS_Snippets_CLR/HowToEmitCodeInPartialTrust/cs/source.cs#11)]
    [!code-vb[HowToEmitCodeInPartialTrust#11](../../../samples/snippets/visualbasic/VS_Snippets_CLR/HowToEmitCodeInPartialTrust/vb/source.vb#11)]

3. Em seu programa principal, obtenha o nome de exibição do seu assembly. Esse nome é usado quando você cria instâncias da classe `Worker` no domínio do aplicativo em área restrita.

    [!code-csharp[HowToEmitCodeInPartialTrust#14](../../../samples/snippets/csharp/VS_Snippets_CLR/HowToEmitCodeInPartialTrust/cs/source.cs#14)]
    [!code-vb[HowToEmitCodeInPartialTrust#14](../../../samples/snippets/visualbasic/VS_Snippets_CLR/HowToEmitCodeInPartialTrust/vb/source.vb#14)]

4. Em seu programa principal, crie um domínio do aplicativo em área restrita, conforme descrito [no primeiro procedimento](#Setting_up) neste passo a passo. Não é necessário adicionar permissões ao conjunto de permissões `Internet`, pois o método `SimpleEmitDemo` usa apenas métodos públicos.

5. No seu programa principal, crie uma instância da classe `Worker` no domínio do aplicativo em área restrita.

    [!code-csharp[HowToEmitCodeInPartialTrust#12](../../../samples/snippets/csharp/VS_Snippets_CLR/HowToEmitCodeInPartialTrust/cs/source.cs#12)]
    [!code-vb[HowToEmitCodeInPartialTrust#12](../../../samples/snippets/visualbasic/VS_Snippets_CLR/HowToEmitCodeInPartialTrust/vb/source.vb#12)]

    O método <xref:System.AppDomain.CreateInstanceAndUnwrap%2A> cria o objeto no domínio do aplicativo de destino e retorna um proxy pode ser usado para chamar as propriedades e métodos do objeto.

    > [!NOTE]
    > Se você usar esse código em Visual Studio, deverá alterar o nome da classe para incluir o namespace. Por padrão, o namespace é o nome do projeto. Por exemplo, se o projeto for “PartialTrust”, o nome de classe deverá ser “PartialTrust.Worker”.

6. Adicionar código para chamar o método `SimpleEmitDemo`. É realizado marshaling na chamada além do limite de domínio do aplicativo e o código é executado no domínio do aplicativo em área restrita.

    [!code-csharp[HowToEmitCodeInPartialTrust#13](../../../samples/snippets/csharp/VS_Snippets_CLR/HowToEmitCodeInPartialTrust/cs/source.cs#13)]
    [!code-vb[HowToEmitCodeInPartialTrust#13](../../../samples/snippets/visualbasic/VS_Snippets_CLR/HowToEmitCodeInPartialTrust/vb/source.vb#13)]

<a name="Using_methods"></a>

## <a name="using-anonymously-hosted-dynamic-methods"></a>Usando métodos dinâmicos hospedados anonimamente

Métodos dinâmicos hospedados anonimamente são associados a um assembly transparente que é fornecido pelo sistema. Portanto, o código que eles contêm é transparente. Métodos dinâmicos comuns, por outro lado, devem ser associados a um módulo existente (especificado diretamente ou inferido de um tipo associado) e usar o nível de segurança desse módulo.

> [!NOTE]
> A única maneira de associar um método dinâmico ao assembly que fornece hospedagem anônima é usar os construtores descritos no procedimento a seguir. Não é possível especificar explicitamente um módulo no assembly hospedagem anônima.

Métodos dinâmicos comuns têm acesso os membros internos do módulo aos quais estão associados ou aos membros privados do tipo aos quais estão associados. Como os métodos dinâmicos hospedados anonimamente são isolados do código, eles não tem acesso a dados particulares. No entanto, eles têm uma capacidade restrita para ignorar verificações de visibilidade JIT para obter acesso aos dados particulares. Essa capacidade é limitada a assemblies com níveis de confiança igual ou inferior ao nível de confiança do assembly que emite o código.

Para evitar a elevação de privilégio, as informações de pilha para o assembly de emissão são incluídas quando ps métodos dinâmicos hospedados anonimamente são construídos. Quando o método é invocado, as informações de pilha são verificadas. Um método dinâmico hospedado anonimamente que é invocado do código totalmente confiável ainda é limitado ao nível de confiança do assembly que o emitiu.

#### <a name="to-use-anonymously-hosted-dynamic-methods"></a>Usar métodos dinâmicos hospedados anonimamente

- Crie um método dinâmico hospedado anonimamente usando um construtor que não especifica um tipo ou um módulo associado.

  [!code-csharp[HowToEmitCodeInPartialTrust#15](../../../samples/snippets/csharp/VS_Snippets_CLR/HowToEmitCodeInPartialTrust/cs/source.cs#15)]
  [!code-vb[HowToEmitCodeInPartialTrust#15](../../../samples/snippets/visualbasic/VS_Snippets_CLR/HowToEmitCodeInPartialTrust/vb/source.vb#15)]

  Se um método dinâmico hospedado anonimamente usar apenas métodos e tipos públicos, ele não requer acesso de membro restrito e não precisa ignorar verificações de visibilidade JIT.

  Nenhuma permissão especial é necessária para emitir um método dinâmico, mas o código emitido requer as permissões exigidas por esses tipos e métodos que ele usa. Por exemplo, se o código emitido chama um método que acessa um arquivo, ele requer <xref:System.Security.Permissions.FileIOPermission>. Se o nível de confiança não inclui essa permissão, uma exceção de segurança é gerada quando o código emitido é executado. O código mostrado aqui emite um método dinâmico que usa apenas o método <xref:System.Console.WriteLine%2A?displayProperty=nameWithType>. Portanto, o código pode ser executado em locais parcialmente confiáveis.

- Como alternativa, crie um método dinâmico hospedado anonimamente com capacidade restrita de ignorar as verificações de visibilidade JIT usando o construtor <xref:System.Reflection.Emit.DynamicMethod.%23ctor%28System.String%2CSystem.Type%2CSystem.Type%5B%5D%2CSystem.Boolean%29> e especificando `true` para o parâmetro `restrictedSkipVisibility`.

  [!code-csharp[HowToEmitCodeInPartialTrust#16](../../../samples/snippets/csharp/VS_Snippets_CLR/HowToEmitCodeInPartialTrust/cs/source.cs#16)]
  [!code-vb[HowToEmitCodeInPartialTrust#16](../../../samples/snippets/visualbasic/VS_Snippets_CLR/HowToEmitCodeInPartialTrust/vb/source.vb#16)]

  A restrição é que o método dinâmico hospedado anonimamente poderá acessar dados particulares somente em assemblies com níveis de confiança igual ou inferior ao nível de confiança do assembly de emissão. Por exemplo, se o método dinâmico estiver em execução com confiança de Internet, ele poderá acessar os dados particulares em outros assemblies que também são executados com confiança de Internet, mas não poderá acessar dados particulares de assemblies do .NET Framework. Assemblies do .NET Framework são instalados no cache de assembly global e sempre são totalmente confiáveis.

  Métodos dinâmicos hospedados anonimamente podem usar essa capacidade restrita para ignorar as verificações de visibilidade JIT somente se o aplicativo host conceder <xref:System.Security.Permissions.ReflectionPermission> com o sinalizador <xref:System.Security.Permissions.ReflectionPermissionFlag.RestrictedMemberAccess?displayProperty=nameWithType>. A demanda por essa permissão é realizada quando o método é invocado.

  > [!NOTE]
  > Informações de pilha de chamadas para o assembly de emissão são incluídas quando o método dinâmico é construído. Portanto, a demanda é feita em relação às permissões do assembly de emissão, em vez do assembly que invoca o método. Isso impede que o código emitido seja executado com permissões elevadas.

  O [exemplo de código completo](#Example) no final dessa instrução passo a passo demonstra o uso e as limitações de acesso de membro restrito. Sua classe `Worker` inclui um método que pode criar métodos dinâmicos hospedados anonimamente com ou sem o recurso restrito para ignorar as verificações de visibilidade e o exemplo mostra o resultado da execução desse método em domínios do aplicativo com níveis diferentes de confiança.

  > [!NOTE]
  > A capacidade restrita de ignorar as verificações de visibilidade é um recurso de métodos dinâmicos hospedados anonimamente. Quando métodos dinâmicos comuns ignoram verificações de visibilidade JIT, eles devem receber confiança total.

<a name="Example"></a>

## <a name="example"></a>Exemplo

### <a name="description"></a>Descrição

O exemplo de código a seguir demonstra o uso do sinalizador <xref:System.Security.Permissions.ReflectionPermissionFlag.RestrictedMemberAccess> para permitir que métodos dinâmicos hospedados anonimamente ignorem as verificações de visibilidade JIT, mas somente quando o membro de destino tiver um nível de confiança igual ou inferior ao assembly que emite o código.

O exemplo define uma classe `Worker` que pode passar por marshaling entre limites de domínio do aplicativo. A classe tem duas sobrecargas de método `AccessPrivateMethod` que emitem e executam métodos dinâmicos. A primeira sobrecarga emite um método dinâmico que chama o método `PrivateMethod` particular da classe `Worker` e pode emitir o método dinâmico com ou sem verificações de visibilidade JIT. A segunda sobrecarga emite um método dinâmico que acessa uma propriedade `internal` (propriedade `Friend` no Visual Basic) da classe <xref:System.String>.

O exemplo usa um método auxiliar para criar um conjunto de concessões limitado a permissões `Internet` e, em seguida, cria um domínio do aplicativo, usando a sobrecarga do método <xref:System.AppDomain.CreateDomain%28System.String%2CSystem.Security.Policy.Evidence%2CSystem.AppDomainSetup%2CSystem.Security.PermissionSet%2CSystem.Security.Policy.StrongName%5B%5D%29?displayProperty=nameWithType> para especificar que todo o código que é executado no domínio usa esse conjunto de concessões. O exemplo cria uma instância da classe `Worker` no domínio do aplicativo e executa o método `AccessPrivateMethod` duas vezes.

- Na primeira vez que o método `AccessPrivateMethod` é executado, as verificações de visibilidade JIT são impostas. O método dinâmico falhará quando for invocado, pois as verificações de visibilidade JIT o impedirão de acessar o método particular.

- Na segunda vez que o método `AccessPrivateMethod` é executado, as verificações de visibilidade JIT são ignoradas. O método dinâmico falha quando ele é compilado, porque o conjunto de concessões `Internet` não concede permissões suficientes para ignorar as verificações de visibilidade.

O exemplo adiciona <xref:System.Security.Permissions.ReflectionPermission> com <xref:System.Security.Permissions.ReflectionPermissionFlag.RestrictedMemberAccess?displayProperty=nameWithType> ao conjunto de concessões. O exemplo a seguir cria um segundo domínio, especificando que todo o código que é executado no domínio recebe as permissões no novo conjunto de concessões. O exemplo cria uma instância da classe `Worker` no domínio do aplicativo e executa ambas as sobrecargas do método `AccessPrivateMethod`.

- A primeira sobrecarga do método `AccessPrivateMethod` é executada e as verificações de visibilidade JIT são ignoradas. O método dinâmico é compilado e executado com êxito, pois o assembly que emite o código é o mesmo que o assembly que contém o método particular. Portanto, os níveis de confiança são iguais. Se o aplicativo que contém a classe `Worker` tiver vários assemblies, o mesmo processo teria êxito para qualquer um desses assemblies, pois todos estão no mesmo nível de confiança.

- A segunda sobrecarga do método `AccessPrivateMethod` é executada e as verificações de visibilidade JIT são ignoradas novamente. Desta vez, o método dinâmico falha quando é compilado, pois ele tenta acessar a `internal` `FirstChar` propriedade da <xref:System.String> classe. O assembly que contém a classe <xref:System.String> é totalmente confiável. Portanto, ele está em um nível mais alto que o assembly que emite o código.

Essa comparação mostra como <xref:System.Security.Permissions.ReflectionPermissionFlag.RestrictedMemberAccess?displayProperty=nameWithType> habilita código parcialmente confiável para ignorar as verificações de visibilidade para outros códigos parcialmente confiáveis sem comprometer a segurança do código confiável.

### <a name="code"></a>Código

[!code-csharp[HowToEmitCodeInPartialTrust#1](../../../samples/snippets/csharp/VS_Snippets_CLR/HowToEmitCodeInPartialTrust/cs/source.cs#1)]
[!code-vb[HowToEmitCodeInPartialTrust#1](../../../samples/snippets/visualbasic/VS_Snippets_CLR/HowToEmitCodeInPartialTrust/vb/source.vb#1)]

## <a name="compiling-the-code"></a>Compilando o código

- Se você compilar esse exemplo de código no Visual Studio, deverá alterar o nome da classe para incluir o namespace ao passá-lo para o método <xref:System.AppDomain.CreateInstanceAndUnwrap%2A>. Por padrão, o namespace é o nome do projeto. Por exemplo, se o projeto for “PartialTrust”, o nome de classe deverá ser “PartialTrust.Worker”.

## <a name="see-also"></a>Veja também

- [Problemas de segurança na emissão de reflexão](security-issues-in-reflection-emit.md)
- [Como executar código parcialmente confiável em uma área restrita](../misc/how-to-run-partially-trusted-code-in-a-sandbox.md)
