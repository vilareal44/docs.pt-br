---
title: Práticas recomendadas para carregamento de assemblies
description: Explore as práticas recomendadas para o carregamento de assembly no .NET. Evite problemas de identidade de tipo que podem levar a conversões inválidas, métodos ausentes e outras exceções.
ms.date: 03/30/2017
helpviewer_keywords:
- assemblies,binding
- LoadFrom method
- default load context
- TypeLoadException class,causes
- assembly loading errors
- load contexts
- assemblies,loading
- LoadWithPartialName method
- load-from context
ms.assetid: 68d1c539-6a47-4614-ab59-4b071c9d4b4c
ms.openlocfilehash: 8ee5243258ea1b853b4690b79ec032c46d1b3777
ms.sourcegitcommit: c23d9666ec75b91741da43ee3d91c317d68c7327
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/01/2020
ms.locfileid: "85803488"
---
# <a name="best-practices-for-assembly-loading"></a>Práticas recomendadas para carregamento de assemblies
Este artigo descreve maneiras de evitar problemas de identidade de tipo que podem levar a <xref:System.InvalidCastException>, <xref:System.MissingMethodException> e outros erros. O artigo aborda as seguintes recomendações:  
  
- [Entender as vantagens e desvantagens dos contextos de carga](#load_contexts)  
  
- [Evite associações em nomes de assembly parciais](#avoid_partial_names)  
  
- [Evite o carregamento de um assembly em vários contextos](#avoid_loading_into_multiple_contexts)  
  
- [Evite carregar várias versões de um assembly no mesmo contexto](#avoid_loading_multiple_versions)  
  
- [Considere a possibilidade de alternar para o contexto de carregamento padrão](#switch_to_default)  
  
 A primeira recomendação, [entenda as vantagens e desvantagens de contextos de carga](#load_contexts), fornece informações de plano de fundo para as outras recomendações, porque todas elas dependem de um conhecimento de contextos de carregamento.  
  
<a name="load_contexts"></a>
## <a name="understand-the-advantages-and-disadvantages-of-load-contexts"></a>Entenda as vantagens e as desvantagens dos contextos de carregamento  
 Em um domínio do aplicativo, os assemblies podem ser carregados em um dos três contextos ou podem ser carregados sem contexto:  
  
- O contexto de carregamento padrão contém assemblies localizados investigando o cache de assembly global, o armazenamento do assembly do host se o runtime é hospedado (por exemplo, no SQL Server) e o <xref:System.AppDomainSetup.ApplicationBase%2A> e o <xref:System.AppDomainSetup.PrivateBinPath%2A> do domínio do aplicativo. A maioria das sobrecargas do método <xref:System.Reflection.Assembly.Load%2A> carrega os assemblies nesse contexto.  
  
- O contexto de origem de carregamento contém assemblies carregados a partir de locais que não são pesquisados pelo carregador. Por exemplo, suplementos podem ser instalados em um diretório que não está no caminho do aplicativo. <xref:System.Reflection.Assembly.LoadFrom%2A?displayProperty=nameWithType>, <xref:System.AppDomain.CreateInstanceFrom%2A?displayProperty=nameWithType> e <xref:System.AppDomain.ExecuteAssembly%2A?displayProperty=nameWithType> são exemplos de métodos que carregam pelo caminho.  
  
- O contexto somente para reflexão contém assemblies carregados com os métodos <xref:System.Reflection.Assembly.ReflectionOnlyLoad%2A> e <xref:System.Reflection.Assembly.ReflectionOnlyLoadFrom%2A>. O código neste contexto não pode ser executado, portanto, não é discutido aqui. Para obter mais informações, consulte [Como carregar assemblies no contexto de somente para reflexão](../reflection-and-codedom/how-to-load-assemblies-into-the-reflection-only-context.md).  
  
- Se você gerou um assembly dinâmico transitório usando a emissão de reflexo, o assembly não está em nenhum contexto. Além disso, a maioria dos assemblies carregados usando o método <xref:System.Reflection.Assembly.LoadFile%2A> são carregados sem contexto e os assemblies carregados de matrizes de bytes são carregados sem contexto, a menos que sua identidade (depois que a política é aplicada) estabeleça que estão no cache de assembly global.  
  
 Os contextos de execução têm vantagens e desvantagens, conforme discutido nas seções a seguir.  
  
### <a name="default-load-context"></a>Contexto de carregamento padrão  
 Quando os assemblies são carregados no contexto de carregamento padrão, suas dependências são carregadas automaticamente. As dependências carregadas no contexto de carregamento padrão são encontradas automaticamente para assemblies no contexto de carregamento padrão ou no contexto de origem de carregamento. O carregamento por identidade do assembly aumenta a estabilidade de aplicativos ao garantir que versões desconhecidas do aplicativo não sejam usadas (consulte a seção [Evite associações em nomes de assembly parciais](#avoid_partial_names)).  
  
 Usar o contexto de carregamento padrão tem as seguintes desvantagens:  
  
- As dependências carregadas em outros contextos não estão disponíveis.  
  
- Você não pode carregar assemblies de locais fora do caminho de investigação para o contexto de carregamento padrão.  
  
### <a name="load-from-context"></a>Contexto de origem de carregamento  
 O contexto de origem de carregamento permite que você carregue um assembly de um caminho que não está no caminho do aplicativo e, portanto, não está incluído na investigação. Ele permite que as dependências sejam localizadas e carregadas a partir desse caminho porque as informações de caminho são mantidas pelo contexto. Além disso, os assemblies nesse contexto podem usar dependências carregadas no contexto de carregamento padrão.  
  
 Carregar assemblies usando o método <xref:System.Reflection.Assembly.LoadFrom%2A?displayProperty=nameWithType>, ou um dos outros métodos que carregam pelo caminho, tem as seguintes desvantagens:  
  
- Se um assembly com a mesma identidade já estiver carregado, <xref:System.Reflection.Assembly.LoadFrom%2A> retornará o assembly carregado, mesmo se um caminho diferente foi especificado.  
  
- Se um assembly é carregado com <xref:System.Reflection.Assembly.LoadFrom%2A> e posteriormente um assembly no contexto de carregamento padrão tenta carregar o mesmo assembly por nome de exibição, a tentativa de carregamento falhará. Isso pode ocorrer quando um assembly é desserializado.  
  
- Se um assembly é carregado com <xref:System.Reflection.Assembly.LoadFrom%2A> e o caminho de investigação inclui um assembly com a mesma identidade, mas em um local diferente, um <xref:System.InvalidCastException>, <xref:System.MissingMethodException> ou outro comportamento inesperado pode ocorrer.  
  
- <xref:System.Reflection.Assembly.LoadFrom%2A> exige <xref:System.Security.Permissions.FileIOPermissionAccess.Read?displayProperty=nameWithType> e <xref:System.Security.Permissions.FileIOPermissionAccess.PathDiscovery?displayProperty=nameWithType>, ou <xref:System.Net.WebPermission>, no caminho especificado.  
  
- Se existir uma imagem nativa para o assembly, ela não será usada.  
  
- O assembly não pode ser carregado como de domínio neutro.  
  
- Nas versões do .NET Framework 1.0 e 1.1, a política não é aplicada.  
  
### <a name="no-context"></a>Sem contexto  
 O carregamento sem contexto é a única opção para assemblies transitórios que são gerados com a emissão de reflexão. O carregamento sem contexto é a única maneira de carregar vários assemblies que têm a mesma identidade em um domínio do aplicativo. O custo de investigação é evitado.  
  
 Os assemblies que são carregados de matrizes de bytes são carregados sem contexto, a menos que a identidade do assembly, que é estabelecida quando a política é aplicada, corresponda à identidade de um assembly no cache de assembly global. Nesse caso, o assembly é carregado do cache de assembly global.  
  
 Carregar assemblies sem contexto tem as seguintes desvantagens:  
  
- Outros assemblies não podem ser associados a assemblies que são carregados sem contexto, a menos que você manipule o evento <xref:System.AppDomain.AssemblyResolve?displayProperty=nameWithType>.  
  
- As dependências não são carregadas automaticamente. Você pode pré-carregá-las sem contexto, pré-carregá-las no contexto de carregamento padrão ou carregá-las manipulando o evento <xref:System.AppDomain.AssemblyResolve?displayProperty=nameWithType>.  
  
- Carregar vários assemblies com a mesma identidade sem contexto pode causar problemas de identidade de tipo semelhantes aos causados por carregar assemblies com a mesma identidade em vários contextos. Consulte [Evite o carregamento de um assembly em vários contextos](#avoid_loading_into_multiple_contexts).  
  
- Se existir uma imagem nativa para o assembly, ela não será usada.  
  
- O assembly não pode ser carregado como de domínio neutro.  
  
- Nas versões do .NET Framework 1.0 e 1.1, a política não é aplicada.  
  
<a name="avoid_partial_names"></a>
## <a name="avoid-binding-on-partial-assembly-names"></a>Evite associações em nomes de assembly parciais  
 A associação de nome parcial ocorre quando você especifica apenas parte do nome de exibição do assembly (<xref:System.Reflection.Assembly.FullName%2A>) ao carregar um assembly. Por exemplo, você pode chamar o método <xref:System.Reflection.Assembly.Load%2A?displayProperty=nameWithType> com somente o nome simples do assembly, omitindo a versão, a cultura e o token de chave pública. Ou você pode chamar o método <xref:System.Reflection.Assembly.LoadWithPartialName%2A?displayProperty=nameWithType>, que primeiro chama o método <xref:System.Reflection.Assembly.Load%2A?displayProperty=nameWithType> e, se ele falhar em localizar o assembly, pesquisa o cache de assembly global e carrega a versão mais recente disponível do assembly.  
  
 A associação de nome parcial pode causar vários problemas, incluindo o seguinte:  
  
- O método <xref:System.Reflection.Assembly.LoadWithPartialName%2A?displayProperty=nameWithType> pode carregar um assembly diferente com o mesmo nome simples. Por exemplo, dois aplicativos podem instalar dois assemblies completamente diferentes que têm o nome simples `GraphicsLibrary` no cache de assembly global.  
  
- O assembly realmente carregado pode não ser compatível com versões anteriores. Por exemplo, não especificar a versão pode resultar no carregamento de uma versão muito posterior à versão que seu programa foi originalmente escrito para usar. Alterações na versão mais recente podem causar erros no seu aplicativo.  
  
- O assembly realmente carregado pode não ser compatível com versões futuras. Por exemplo, você pode ter compilado e testado seu aplicativo com a versão mais recente de um assembly, mas a associação parcial pode carregar uma versão muito anterior que não possui recursos que seu aplicativo usa.  
  
- Instalar novos aplicativos pode interromper aplicativos existentes. Um aplicativo que usa o método <xref:System.Reflection.Assembly.LoadWithPartialName%2A> pode ser interrompido ao instalar uma versão incompatível mais recente de um assembly compartilhado.  
  
- O carregamento de dependência inesperado pode ocorrer. Se você carregar dois assemblies que compartilham uma dependência, carregá-los com a associação parcial pode resultar em um assembly usando um componente com o qual ele não foi compilado ou testado.  
  
 Devido aos problemas que isso pode causar, o método <xref:System.Reflection.Assembly.LoadWithPartialName%2A> foi marcado como obsoleto. É recomendável que você use o método <xref:System.Reflection.Assembly.Load%2A?displayProperty=nameWithType> em vez disso e especifique os nomes de exibição completos do assembly. Consulte [Entenda as vantagens e as desvantagens dos contextos de carregamento](#load_contexts) e [Considere a possibilidade de alternar para o contexto de carregamento padrão](#switch_to_default).  
  
 Se você quiser usar o método <xref:System.Reflection.Assembly.LoadWithPartialName%2A> porque ele facilita o carregamento de assembly, considere que seu aplicativo falhar com uma mensagem de erro que identifica o assembly ausente provavelmente fornecerá uma experiência de usuário melhor do que usar automaticamente uma versão desconhecida do assembly, o que pode causar um comportamento imprevisível e falhas de segurança.  
  
<a name="avoid_loading_into_multiple_contexts"></a>
## <a name="avoid-loading-an-assembly-into-multiple-contexts"></a>Evite o carregamento de um assembly em vários contextos  
 Carregar um assembly em vários contextos pode causar problemas de identidade de tipo. Se o mesmo tipo for carregado a partir do mesmo assembly em dois contextos diferentes, é como se dois tipos diferentes com o mesmo nome tivessem sido carregados. Uma <xref:System.InvalidCastException> é lançada se você tenta converter um tipo para outro, com a mensagem confusa de que o tipo `MyType` não pode ser convertido para o tipo `MyType`.  
  
 Por exemplo, suponha que a interface `ICommunicate` é declarada em um assembly chamado `Utility`, que é referenciado por seu programa e também por outros assemblies que seu programa carrega. Esses outros assemblies contêm tipos que implementam a interface `ICommunicate`, permitindo que seu programa os use.  
  
 Agora, considere o que acontece quando o programa é executado. Os assemblies referenciados pelo seu programa são carregados no contexto de carregamento padrão. Se você carregar um assembly de destino por sua identidade, usando o método <xref:System.Reflection.Assembly.Load%2A>, ele estará no contexto de carregamento padrão, assim como suas dependências. O programa e o assembly de destino usarão o mesmo assembly `Utility`.  
  
 No entanto, suponha que você carregue o assembly de destino por seu caminho de arquivo, usando o método <xref:System.Reflection.Assembly.LoadFile%2A>. O assembly é carregado sem qualquer contexto, portanto, suas dependências não são carregadas automaticamente. Você pode ter um manipulador para o evento <xref:System.AppDomain.AssemblyResolve?displayProperty=nameWithType> para fornecer a dependência e pode carregar o assembly `Utility` sem nenhum contexto usando o método <xref:System.Reflection.Assembly.LoadFile%2A>. Agora quando você cria uma instância de um tipo que está contido no assembly de destino e tente atribuí-la a uma variável do tipo `ICommunicate`, uma <xref:System.InvalidCastException> é gerada porque o runtime considera as interfaces `ICommunicate` em duas cópias do assembly `Utility` para tipos diferentes.  
  
 Há muitos outros cenários em que um assembly pode ser carregado em vários contextos. A melhor abordagem é evitar conflitos realocando o assembly de destino no caminho do aplicativo e usando o método <xref:System.Reflection.Assembly.Load%2A> com o nome de exibição completo. O assembly é então carregado no contexto de carregamento padrão e ambos os assemblies usam o mesmo assembly `Utility`.  
  
 Se o assembly de destino dever permanecer fora do caminho do aplicativo, você pode usar o método <xref:System.Reflection.Assembly.LoadFrom%2A> para carregá-lo no contexto de origem de carregamento. Se o assembly de destino foi compilado com uma referência ao assembly `Utility` do seu aplicativo, ele usará o assembly `Utility` em que seu aplicativo carregou no contexto de carregamento padrão. Observe que podem ocorrer problemas se o assembly de destino tiver uma dependência em uma cópia do assembly `Utility` localizado fora do caminho do aplicativo. Se o assembly for carregado no contexto de origem de carregamento antes de seu aplicativo carregar o assembly `Utility`, o carregamento do aplicativo falhará.  
  
 A seção [Considere a possibilidade de alternar para o contexto de carregamento padrão](#switch_to_default) discute alternativas ao uso de cargas de caminho de arquivo como <xref:System.Reflection.Assembly.LoadFile%2A> e <xref:System.Reflection.Assembly.LoadFrom%2A>.  
  
<a name="avoid_loading_multiple_versions"></a>
## <a name="avoid-loading-multiple-versions-of-an-assembly-into-the-same-context"></a>Evite o carregamento de várias versões de um assembly no mesmo contexto  
 Carregar várias versões de um assembly em um contexto de carregamento pode causar problemas de identidade de tipo. Se o mesmo tipo for carregado a partir de duas versões do mesmo assembly, é como se dois tipos diferentes com o mesmo nome tivessem sido carregados. Uma <xref:System.InvalidCastException> é lançada se você tenta converter um tipo para outro, com a mensagem confusa de que o tipo `MyType` não pode ser convertido para o tipo `MyType`.  
  
 Por exemplo, seu programa pode carregar uma versão do assembly `Utility` diretamente e posteriormente ele pode carregar outro assembly que carrega uma versão diferente do assembly `Utility`. Ou um erro de código pode fazer com que dois caminhos de código diferentes no aplicativo carreguem versões diferentes de um assembly.  
  
 No contexto de carregamento padrão, esse problema pode ocorrer quando você usa o método <xref:System.Reflection.Assembly.Load%2A?displayProperty=nameWithType> e especifica os nomes de exibição completos do assembly que incluem números de versão diferentes. Para assemblies que são carregados sem contexto, o problema pode ser causado pelo uso do método <xref:System.Reflection.Assembly.LoadFile%2A?displayProperty=nameWithType> para carregar o mesmo assembly de diferentes caminhos. O runtime considera dois assemblies que são carregados de caminhos diferentes para serem assemblies diferentes, mesmo que suas identidades sejam as mesmas.  
  
 Além dos problemas de identidade de tipo, várias versões de um assembly podem causar um <xref:System.MissingMethodException> se um tipo carregado de uma versão do assembly é passado para o código que espera esse tipo de uma versão diferente. Por exemplo, o código pode esperar um método que foi adicionado à versão mais recente.  
  
 Podem ocorrer erros mais sutis se o comportamento do tipo foi alterado entre as versões. Por exemplo, um método pode lançar uma exceção inesperada ou retornar um valor inesperado.  
  
 Examine cuidadosamente o código para garantir que apenas uma versão de um assembly esteja carregada. Você pode usar o método <xref:System.AppDomain.GetAssemblies%2A?displayProperty=nameWithType> para determinar quais assemblies são carregados em um determinado momento.  
  
<a name="switch_to_default"></a>
## <a name="consider-switching-to-the-default-load-context"></a>Considere a possibilidade de alternar para o contexto de carregamento padrão  
 Examine os padrões de implantação e carregamento de assembly do aplicativo. Você pode eliminar os assemblies carregados de matrizes de bytes? Você pode mover assemblies para o caminho de investigação? Se os assemblies estão localizados no cache de assembly global ou caminho de investigação do domínio do aplicativo (isto é, seu <xref:System.AppDomainSetup.ApplicationBase%2A> e <xref:System.AppDomainSetup.PrivateBinPath%2A>), você pode carregar o assembly por sua identidade.  
  
 Se não for possível colocar todos os assemblies no caminho de investigação, considere alternativas como usar o modelo de suplemento do .NET Framework, colocar os assemblies no cache de assembly global ou criar domínios de aplicativo.  
  
### <a name="consider-using-the-net-framework-add-in-model"></a>Considere usar o modelo de suplemento do Framework .NET  
 Se você estiver usando o contexto de carregamento para implementar suplementos, que normalmente não são instalados na base de aplicativo, use o modelo de suplemento do .NET Framework. Esse modelo fornece isolamento no nível de domínio ou processo do aplicativo, sem a necessidade de gerenciar domínios do aplicativo por conta própria. Para obter informações sobre o modelo de suplemento, consulte [Suplementos e extensibilidade](https://docs.microsoft.com/previous-versions/dotnet/netframework-4.0/bb384200(v%3dvs.100)).  
  
### <a name="consider-using-the-global-assembly-cache"></a>Considere usar o cache de assembly global  
 Coloque os assemblies no cache de assembly global para obter o benefício de um caminho de assembly compartilhado que está fora da base de aplicativo, sem perder as vantagens do contexto de carregamento padrão ou assumir as desvantagens dos outros contextos.  
  
### <a name="consider-using-application-domains"></a>Considere usar domínios do aplicativo  
 Se você determinar que alguns de seus assemblies não podem ser implantados no caminho de investigação do aplicativo, considere criar um novo domínio do aplicativo para esses assemblies. Use um <xref:System.AppDomainSetup> para criar o novo domínio do aplicativo e use a propriedade <xref:System.AppDomainSetup.ApplicationBase%2A?displayProperty=nameWithType> para especificar o caminho que contém os assemblies que você deseja carregar. Se você tiver vários diretórios para investigar, é possível definir a <xref:System.AppDomainSetup.ApplicationBase%2A> para um diretório raiz e usar a propriedade <xref:System.AppDomainSetup.PrivateBinPath%2A?displayProperty=nameWithType> para identificar os subdiretórios a serem investigados. Como alternativa, você pode criar vários domínios de aplicativo e definir o <xref:System.AppDomainSetup.ApplicationBase%2A> de cada domínio do aplicativo para o caminho adequado para seus assemblies.  
  
 Observe que você pode usar o método <xref:System.Reflection.Assembly.LoadFrom%2A?displayProperty=nameWithType> para carregar esses assemblies. Como agora eles estão no caminho de investigação, eles serão carregados no contexto de carregamento padrão em vez de no contexto de origem do carregamento. No entanto, recomendamos que você alterne para o método <xref:System.Reflection.Assembly.Load%2A?displayProperty=nameWithType> e forneça nomes de exibição completos do assembly para garantir que as versões corretas sejam sempre usadas.  
  
## <a name="see-also"></a>Confira também

- <xref:System.Reflection.Assembly.Load%2A?displayProperty=nameWithType>
- <xref:System.Reflection.Assembly.LoadFrom%2A?displayProperty=nameWithType>
- <xref:System.Reflection.Assembly.LoadFile%2A?displayProperty=nameWithType>
- <xref:System.AppDomain.AssemblyResolve?displayProperty=nameWithType>
