---
title: Compilação e .NET nativo
ms.date: 03/30/2017
ms.assetid: e38ae4f3-3e3d-42c3-a4b8-db1aa9d84f85
ms.openlocfilehash: cf5c9f05b2f2cb4ca15e4add5b53bc9bdca757a3
ms.sourcegitcommit: b16c00371ea06398859ecd157defc81301c9070f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/06/2020
ms.locfileid: "73128253"
---
# <a name="net-native-and-compilation"></a>Compilação e .NET nativo

Aplicativos do Windows 8.1 e aplicativos de Área de Trabalho do Windows direcionados ao .NET Framework são escritos em uma linguagem de programação específica e compilados em IL (linguagem intermediária). Em runtime, um compilador JIT (Just-in-Time) é responsável pela compilação de IL em código nativo para o computador local antes de um método ser executado pela primeira vez. Por outro lado, a cadeia de ferramentas do .NET Native converte código-fonte em código nativo em tempo de compilação. Este tópico compara .NET Native com outras tecnologias de compilação disponíveis para aplicativos do .NET Framework e também fornece uma visão prática de como o .NET Native produz código nativo que pode lhe ajudar a entender por que as exceções que ocorrem no código compilado com o .NET Native não ocorrem no código com compilação JIT.

## <a name="net-native-generating-native-binaries"></a>.NET Native: gerando binários nativos

Um aplicativo que tem como destino o .NET Framework e que não é compilado usando a cadeia de ferramentas do .NET Native consiste em seu assembly de aplicativo, que inclui o seguinte:

- [Metadados](../../standard/metadata-and-self-describing-components.md) que descrevem o assembly, as dependências desses metadados, os tipos que eles contêm e os respectivos membros. Os metadados são usados para reflexão e acesso de associação tardia e, em alguns casos, pelas ferramentas de compilador e de build também.

- Código de implementação. Isso consiste de opcodes de IL (linguagem intermediária). Em runtime, o compilador JIT (Just-In-Time) o converte em código nativo para a plataforma de destino.

 Além de seu assembly principal do aplicativo, um aplicativo requer que os seguintes elementos estejam presentes:

- Quaisquer bibliotecas de classes adicionais ou assemblies de terceiros que sejam exigidos pelo seu aplicativo. Da mesma forma, esses assemblies incluem metadados que descrevem o assembly, os tipos e membros dele, bem como a IL que implementa todos os membros de tipo.

- A biblioteca de classes .NET Framework. Esta é uma coleção de assemblies que está instalada no sistema local com a instalação do .NET Framework. Os assemblies incluídos na biblioteca de classes .NET Framework incluem um conjunto completo de metadados e código de implementação.

- O Common Language Runtime. Isso é uma coleção de bibliotecas de vínculo dinâmico que executam serviços como o carregamento do assembly, coleta de lixo e gerenciamento de memória, tratamento de exceções, compilação Just-In-Time, comunicação remota e interoperabilidade. Assim como a biblioteca de classes, o runtime está instalado no sistema local como parte da instalação do .NET Framework.

Observe que todo o Common Language Runtime, bem como os metadados e IL para todos os tipos em assemblies específicos de aplicativo, os assemblies de terceiros e os assemblies do sistema devem estar presentes para o aplicativo ser executado com êxito.

## <a name="net-native-and-just-in-time-compilation"></a>Compilação Just-In-Time e por .NET Native

A entrada para a cadeia de ferramentas do .NET Native é o aplicativo da Windows Store criado pelo C# ou pelo compilador do Visual Basic. Em outras palavras, a cadeia de ferramentas do .NET Native começa a ser executada quando o compilador de linguagem conclui a compilação de um aplicativo da Windows Store.

> [!TIP]
> Já que a entrada para o .NET Native é a IL e os metadados gravados para assemblies gerenciados, você ainda pode executar a geração de código personalizado ou outras operações personalizadas usando os eventos pré-build ou pós-build ou modificando o arquivo de projeto do MSBuild.
>
> No entanto, não há suporte para categorias de ferramentas que modificam a IL e, portanto, impedem que a cadeia de ferramentas .NET analise a IL de um aplicativo. Os ofuscadores são as ferramentas mais notáveis desse tipo.

Durante a conversão de um aplicativo de IL para código nativo, a cadeia de ferramentas do .NET Native executa operações como as seguintes:

- Para determinados caminhos de código, ele substitui o código que depende de reflexão e metadados pelo código nativo estático. Por exemplo, se um tipo de valor não substitui o método <xref:System.ValueType.Equals%2A?displayProperty=nameWithType>, o teste padrão de igualdade usa reflexão para recuperar objetos <xref:System.Reflection.FieldInfo> que representam os campos de tipo de valor e, em seguida, compara os valores de campo de duas instâncias. Ao compilar para código nativo, a cadeia de ferramentas do .NET Native substitui o código de reflexão e metadados por uma comparação estática dos valores de campo.

- Sempre que possível, ele tenta eliminar todos os metadados.

- Ele inclui nos assemblies do aplicativo final apenas o código de implementação que é realmente invocado pelo aplicativo. Isso afeta especialmente o código nas bibliotecas de terceiros e na biblioteca de classes .NET Framework. Como resultado, um aplicativo já não depende de bibliotecas de terceiros ou da biblioteca de classes .NET Framework completa; em vez disso, o código em bibliotecas de classes .NET Framework e de terceiros agora é local para o aplicativo.

- Ele substitui o CLR completo com um runtime refatorado que contém principalmente o coletor de lixo. O runtime refatorado encontra-se em uma biblioteca de chamada mrt100_app.dll que é local para o aplicativo e tem apenas algumas centenas de kilobytes de tamanho. Isso é possível porque a vinculação estática elimina a necessidade de muitos dos serviços executados pelo Common Language Runtime.

  > [!NOTE]
  > O .NET Native usa o mesmo coletor de lixo que o Common Language Runtime padrão. No coletor de lixo do .NET Native, a coleta de lixo em segundo plano é habilitada por padrão. Para obter mais informações sobre a coleta de lixo, consulte [Conceitos básicos da coleta de lixo](../../standard/garbage-collection/fundamentals.md).

> [!IMPORTANT]
> O .NET Native compila um aplicativo inteiro para um aplicativo nativo. Ele não permite que você compile um único assembly que contém uma biblioteca de classes para código nativo para que ele possa ser chamado independentemente por meio do código gerenciado.

O aplicativo resultante que é produzido pela cadeia de ferramentas do .NET Native é gravado em um diretório chamado ilc.out no diretório Debug ou Release do diretório do projeto. Ele consiste dos seguintes arquivos:

- *\<appName>*. exe, um executável de stub que simplesmente transfere o controle para uma `Main` exportação especial no *\<appName>* . dll.

- *\<appName>*. dll, uma biblioteca de vínculo dinâmico do Windows que contém todo o código do aplicativo, bem como o código da biblioteca de classes de .NET Framework e quaisquer bibliotecas de terceiros nas quais você tem uma dependência.  Ele também contém código de suporte, tal como o código necessário para interoperação com o Windows e para serializar objetos em seu aplicativo.

- mrt100_app.dll, um runtime refatorado que fornece serviços de runtime como coleta de lixo.

 Todas as dependências são capturadas pelo manifesto APPX do aplicativo.  Além do exe, dll e mrt100_app.dll do aplicativo, que são empacotados diretamente no pacote appx, isso inclui mais dois arquivos:

- msvcr140_app.dll, a biblioteca de tempo de execução (CRT) de C usada pelo mrt100_app.dll. Ela é incluída por uma referência de estrutura no pacote.

- mrt100.dll. Essa biblioteca inclui funções que podem melhorar o desempenho de mrt100_app.dll, embora sua ausência não impeça que mrt100_app.dll funcione. Ele será carregado do diretório system32 no computador local, se tal diretório estiver presente.

Já que a cadeia de ferramentas do .NET Native vinculará o código de implementação em seu aplicativo somente se souber que seu aplicativo realmente invoca esse código, os metadados ou o código de implementação necessários nos cenários a seguir poderão não ser incluídos com seu aplicativo:

- Reflexão.

- Invocação dinâmica ou de associação tardia.

- Serialização e desserialização.

- Interoperabilidade COM.

Se o código de implementação ou de metadados necessário está ausente em runtime, o runtime do .NET Native gera uma exceção. Você pode evitar essas exceções e verificar se a cadeia de ferramentas do .NET Native inclui o código de implementação e os metadados necessários, usando um [arquivo de diretivas de runtime](runtime-directives-rd-xml-configuration-file-reference.md), um arquivo XML que designa os elementos do programa cujos metadados ou código de implementação devem estar disponíveis em runtime e atribui uma política de runtime a elas. Este é o arquivo de diretivas de runtime padrão que é adicionado a um projeto da Windows Store que é compilado pela cadeia de ferramentas do .NET Native:

```xml
<Directives xmlns="http://schemas.microsoft.com/netfx/2013/01/metadata">
  <Application>
    <Assembly Name="*Application*" Dynamic="Required All" />
  </Application>
</Directives>
```

Isso habilita todos os tipos, bem como todos os respectivos membros em todos os assemblies em seu pacote do aplicativo, para reflexão e invocação dinâmica. No entanto, isso não habilita reflexão ou ativação dinâmica de tipos em assemblies de biblioteca de classes .NET Framework. Em muitos casos, isso é adequado.

## <a name="net-native-and-ngen"></a>.NET Native e NGEN

O NGEN [(gerador de imagem nativa)](../tools/ngen-exe-native-image-generator.md) compila os assemblies para código nativo e os instala no cache de imagem nativa no computador local. No entanto, embora NGEN, assim como o .NET Native, produza código nativo, ele é diferente do .NET Native de algumas maneiras significativas:

- Se nenhuma imagem nativa estiver disponível para um método específico, o NGEN reverterá para compilação de código via JIT. Isso significa que imagens nativas devem continuar a incluir IL e metadados caso o NGEN precise reverter para a compilação JIT. Por outro lado, o .NET Native produz apenas imagens nativas e não será revertido para a compilação JIT. Como resultado, apenas os metadados necessários para alguns cenários de interoperabilidade, serialização e reflexão devem ser preservados.

- O NGEN continua a depender do Common Language Runtime completo para serviços como o carregamento de assembly, comunicação remota, interoperabilidade, gerenciamento de memória, coleta de lixo e, se necessário, compilação JIT. No .NET Native, muitos desses serviços são desnecessários (compilação JIT) ou são resolvidos em tempo de build e incorporados no assembly do aplicativo. Os serviços restantes, entre os quais o mais importante é a coleta de lixo, são incluídos em um runtime muito menor, refatorado, denominado mrt100_app.dll.

- As imagens NGEN tendem a ser frágeis. Por exemplo, um patch ou alteração para uma dependência normalmente requer que os assemblies que a usam também tenham imagem gerada novamente por NGEN. Isso é particularmente verdadeiro em assemblies do sistema na biblioteca de classes .NET Framework. Por outro lado, o .NET Native permite que aplicativos sejam atendidos independentemente uns dos outros.

## <a name="see-also"></a>Confira também

- [Metadados e componentes autodescritivos](../../standard/metadata-and-self-describing-components.md)
- [Por dentro do .NET Native (vídeo do Channel 9)](https://channel9.msdn.com/Shows/Going+Deep/Inside-NET-Native)
- [Reflexão e .NET Nativo](reflection-and-net-native.md)
- [Solução de problemas gerais do .NET Nativo](net-native-general-troubleshooting.md)
