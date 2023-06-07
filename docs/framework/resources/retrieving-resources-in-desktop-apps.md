---
title: Recuperando recursos em aplicativos de área de trabalho
description: Recupere recursos em aplicativos da área de trabalho. Empacotar recursos para a cultura padrão (neutra) com o assembly principal e criar um assembly satélite para cada cultura.
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
helpviewer_keywords:
- deploying applications [.NET Framework], resources
- resource files, deploying
- resource files, packaging
- application resources, packaging
- localization
- versioning satellite assemblies
- retrieving localized resources
- application resources, deploying
- packaging application resources
- versioning resources
- translating resources into languages
- localizing resources
ms.assetid: eca16922-1c46-4f68-aefe-e7a12283641f
ms.openlocfilehash: db1156106690f8321b7fd5a2890c2aa44cfe17e3
ms.sourcegitcommit: 87cfeb69226fef01acb17c56c86f978f4f4a13db
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/24/2020
ms.locfileid: "87166193"
---
# <a name="retrieving-resources-in-desktop-apps"></a>Recuperando recursos em aplicativos de área de trabalho

Quando você trabalha com recursos localizados em aplicativos de área de trabalho do .NET Framework, o ideal é empacotar os recursos para a cultura padrão ou neutra com o assembly principal e criar um assembly satélite separado para cada idioma ou cultura que oferece suporte ao seu aplicativo. Você pode usar a classe <xref:System.Resources.ResourceManager> conforme descrito na próxima seção para acessar recursos nomeados. Se você optar por não incorporar os recursos do assembly principal e os assemblies satélites, você também pode acessar os arquivos .resources binários diretamente, conforme discutido na seção [Recuperando recursos de arquivos .resources](#from_file) posteriormente neste artigo.  Para recuperar recursos em aplicativos da loja do Windows 8. x, consulte [criando e recuperando recursos em aplicativos da Windows Store](https://docs.microsoft.com/previous-versions/windows/apps/hh694557(v=vs.140)).  
  
<a name="from_assembly"></a>
## <a name="retrieving-resources-from-assemblies"></a>Recuperando recursos dos assemblies  
 A classe <xref:System.Resources.ResourceManager> fornece acesso a recursos em tempo de execução. Você usa o método <xref:System.Resources.ResourceManager.GetString%2A?displayProperty=nameWithType> para recuperar os recursos de cadeia de caracteres e o método <xref:System.Resources.ResourceManager.GetObject%2A?displayProperty=nameWithType> ou <xref:System.Resources.ResourceManager.GetStream%2A?displayProperty=nameWithType> para recuperar recursos que não são cadeias de caracteres. Cada método possui duas sobrecargas:  
  
- Uma sobrecarga cujo único parâmetro é uma cadeia de caracteres que contém o nome do recurso. O método tenta recuperar esse recurso para a cultura do thread atual. Para mais informações, consulte os métodos <xref:System.Resources.ResourceManager.GetString%28System.String%29>, <xref:System.Resources.ResourceManager.GetObject%28System.String%29> e <xref:System.Resources.ResourceManager.GetStream%28System.String%29>.  
  
- Uma sobrecarga que tem dois parâmetros: uma cadeia de caracteres que contém o nome do recurso e um objeto <xref:System.Globalization.CultureInfo> que representa a cultura cujo recurso deve ser recuperado. Se um conjunto de recursos para a cultura não for encontrado, o gerenciador de recursos usa regras de fallback para recuperar um recurso apropriado. Para mais informações, consulte os métodos <xref:System.Resources.ResourceManager.GetString%28System.String%2CSystem.Globalization.CultureInfo%29>, <xref:System.Resources.ResourceManager.GetObject%28System.String%2CSystem.Globalization.CultureInfo%29> e <xref:System.Resources.ResourceManager.GetStream%28System.String%2CSystem.Globalization.CultureInfo%29>.  
  
 O gerenciador de recursos usa o processo de fallback de recurso para controlar como o aplicativo recupera os recursos específicos de cultura. Para obter mais informações, consulte a seção "Processo de Fallback de Recurso" em [Empacotamento e implantação de recursos](packaging-and-deploying-resources-in-desktop-apps.md). Para obter informações sobre como criar uma instância de um objeto <xref:System.Resources.ResourceManager>, consulte a seção “Criando uma instância de um objeto ResourceManager” no tópico de classe <xref:System.Resources.ResourceManager>.  
  
### <a name="retrieving-string-data-an-example"></a>Recuperação de dados de cadeia de caracteres: Um exemplo  
 O exemplo a seguir chama o método <xref:System.Resources.ResourceManager.GetString%28System.String%29> para recuperar os recursos de cadeia de caracteres da cultura da interface do usuário atual. Ele inclui um recurso de cadeia de caracteres neutro para a cultura do inglês (Estados Unidos) e recursos localizados para as culturas de francês (França) e russo (Rússia). O seguinte recurso de inglês (Estados Unidos) está em um arquivo chamado Strings.txt:  
  
```text
TimeHeader=The current time is  
```  
  
 O recurso de francês (França) está em um arquivo chamado Strings.fr-FR.txt:  
  
```text
TimeHeader=L'heure actuelle est  
```  
  
 O recurso de russo (Rússia) está em um arquivo chamado Strings.ru-RU.txt:  
  
```text
TimeHeader=Текущее время —  
```  
  
 O código-fonte para este exemplo, que está em um arquivo chamado GetString.cs para a versão em C# do código e GetString.vb para a versão do Visual Basic, define uma matriz de cadeia de caracteres que contém o nome de quatro culturas: três culturas para as quais os recursos estão disponíveis e a cultura de espanhol (Espanha). Um loop executado cinco vezes aleatoriamente seleciona uma dessas culturas e a atribui às propriedades <xref:System.Threading.Thread.CurrentCulture%2A?displayProperty=nameWithType> e <xref:System.Globalization.CultureInfo.CurrentUICulture%2A?displayProperty=nameWithType>. Depois, ele chama o método <xref:System.Resources.ResourceManager.GetString%28System.String%29> para recuperar a cadeia de caracteres localizada, que ele exibe com a hora do dia.  
  
 [!code-csharp[Conceptual.Resources.Retrieving#3](../../../samples/snippets/csharp/VS_Snippets_CLR/conceptual.resources.retrieving/cs/getstring.cs#3)]
 [!code-vb[Conceptual.Resources.Retrieving#3](../../../samples/snippets/visualbasic/VS_Snippets_CLR/conceptual.resources.retrieving/vb/getstring.vb#3)]  
  
 O seguinte arquivo em lotes (.bat) compila o exemplo e gera assemblies satélites nos diretórios apropriados. Os comandos são fornecidos para a linguagem C# e para o compilador. Para o Visual Basic, altere `csc` para `vbc`, e altere `GetString.cs` para `GetString.vb`.  
  
```console
resgen strings.txt  
csc GetString.cs -resource:strings.resources  
  
resgen strings.fr-FR.txt  
md fr-FR  
al -embed:strings.fr-FR.resources -culture:fr-FR -out:fr-FR\GetString.resources.dll  
  
resgen strings.ru-RU.txt  
md ru-RU  
al -embed:strings.ru-RU.resources -culture:ru-RU -out:ru-RU\GetString.resources.dll  
```  
  
 Quando a cultura da interface do usuário atual for espanhol (Espanha), observe que o exemplo exibe os recursos do idioma inglês, porque não estão disponíveis recursos de idioma espanhol e inglês é a cultura padrão de exemplo.  
  
### <a name="retrieving-object-data-two-examples"></a>Recuperação de dados de objeto: Dois exemplos  
 Você pode usar os métodos <xref:System.Resources.ResourceManager.GetObject%2A> e <xref:System.Resources.ResourceManager.GetStream%2A> para recuperar dados de objeto. Isso inclui tipos de dados primitivos, objetos serializáveis e objetos que são armazenados em formato binário (como imagens).  
  
 O exemplo a seguir usa o método <xref:System.Resources.ResourceManager.GetStream%28System.String%29> para recuperar um bitmap que é usado em uma janela inicial de abertura do aplicativo. O código-fonte a seguir em um arquivo chamado CreateResources.cs (para C#) ou CreateResources.vb (para Visual Basic) gera um arquivo .resx que contém a imagem serializada. Nesse caso, a imagem é carregada a partir de um arquivo chamado SplashScreen.jpg; você pode modificar o nome do arquivo para substituir sua própria imagem.  
  
 [!code-csharp[Conceptual.Resources.Retrieving#4](../../../samples/snippets/csharp/VS_Snippets_CLR/conceptual.resources.retrieving/cs/createresources.cs#4)]
 [!code-vb[Conceptual.Resources.Retrieving#4](../../../samples/snippets/visualbasic/VS_Snippets_CLR/conceptual.resources.retrieving/vb/createresources.vb#4)]  
  
 O código a seguir recupera o recurso e exibe a imagem em um controle <xref:System.Windows.Forms.PictureBox>.  
  
 [!code-csharp[Conceptual.Resources.Retrieving#5](../../../samples/snippets/csharp/VS_Snippets_CLR/conceptual.resources.retrieving/cs/getstream.cs#5)]
 [!code-vb[Conceptual.Resources.Retrieving#5](../../../samples/snippets/visualbasic/VS_Snippets_CLR/conceptual.resources.retrieving/vb/getstream.vb#5)]  
  
 Você pode usar o seguinte arquivo em lotes para criar o exemplo de C#. Para o Visual Basic, altere `csc` para `vbc` e altere a extensão do arquivo de código-fonte de `.cs` para `.vb`.  
  
```console
csc CreateResources.cs  
CreateResources  
  
resgen AppResources.resx  
  
csc GetStream.cs -resource:AppResources.resources  
```  
  
 O exemplo a seguir usa o método <xref:System.Resources.ResourceManager.GetObject%28System.String%29?displayProperty=nameWithType> para desserializar um objeto personalizado. O exemplo inclui um arquivo de código-fonte chamado UIElements.cs (UIElements.vb para o Visual Basic) que define a seguinte estrutura denominada `PersonTable`. Essa estrutura destina-se a ser usada por uma rotina de exibição geral da tabela que exibe os nomes localizados das colunas da tabela. Observe que a estrutura `PersonTable` é marcada com o atributo <xref:System.SerializableAttribute>.  
  
 [!code-csharp[Conceptual.Resources.Retrieving#6](../../../samples/snippets/csharp/VS_Snippets_CLR/conceptual.resources.retrieving/cs/example.cs#6)]
 [!code-vb[Conceptual.Resources.Retrieving#6](../../../samples/snippets/visualbasic/VS_Snippets_CLR/conceptual.resources.retrieving/vb/example.vb#6)]  
  
 O código a seguir de um arquivo chamado CreateResources.cs (CreateResources.vb para o Visual Basic) cria um arquivo de recurso XML chamado UIResources.resx que armazena um título de tabela e um objeto `PersonTable` que contém informações para um aplicativo localizado para o idioma inglês.  
  
 [!code-csharp[Conceptual.Resources.Retrieving#7](../../../samples/snippets/csharp/VS_Snippets_CLR/conceptual.resources.retrieving/cs/example1.cs#7)]
 [!code-vb[Conceptual.Resources.Retrieving#7](../../../samples/snippets/visualbasic/VS_Snippets_CLR/conceptual.resources.retrieving/vb/example.vb#7)]  
  
 O código a seguir em um arquivo de código-fonte chamado GetObject.cs (GetObject.vb), em seguida, recupera os recursos e os exibe no console.  
  
 [!code-csharp[Conceptual.Resources.Retrieving#8](../../../samples/snippets/csharp/VS_Snippets_CLR/conceptual.resources.retrieving/cs/example2.cs#8)]
 [!code-vb[Conceptual.Resources.Retrieving#8](../../../samples/snippets/visualbasic/VS_Snippets_CLR/conceptual.resources.retrieving/vb/example2.vb#8)]  
  
 Você pode criar o arquivo de recurso necessário e os assemblies e executar o aplicativo, executando o seguinte arquivo em lotes. Você deve usar a opção `/r` para fornecer ao arquivo Resgen.exe uma referência para UIElements.dll para que ele possa acessar informações sobre a estrutura `PersonTable`. Se você estiver usando C#, substitua o nome do compilador `vbc` por `csc` e substitua a extensão `.vb` por `.cs`.  
  
```console
vbc -t:library UIElements.vb  
vbc CreateResources.vb -r:UIElements.dll  
CreateResources  
  
resgen UIResources.resx  -r:UIElements.dll  
vbc GetObject.vb -r:UIElements.dll -resource:UIResources.resources  
  
GetObject.exe  
```  
  
## <a name="versioning-support-for-satellite-assemblies"></a>Controlando a versão de assemblies satélite  
 Por padrão, quando o objeto <xref:System.Resources.ResourceManager> recupera os recursos solicitados, ele procura por assemblies satélite que tenham números de versão que correspondam ao número de versão do assembly principal. Depois de ter implantado um aplicativo, convém atualizar o assembly principal ou assemblies de satélite de recursos específicos. O .NET Framework fornece suporte para controle de versão do assembly principal e dos assemblies satélites.  
  
 O atributo <xref:System.Resources.SatelliteContractVersionAttribute> dá suporte ao controle de versão para um assembly principal. A especificação desse atributo no assembly principal do aplicativo permite que você atualize e implante novamente um assembly principal sem atualizar seus assemblies satélites. Depois de atualizar o assembly principal, incremente o número de versão do assembly principal mas deixe o número de versão do contrato do satélite inalterado. Quando o gerenciador de recursos recupera os recursos solicitados, ele carrega a versão do assembly satélite especificada por esse atributo.  
  
 Os assemblies de política do publicador oferecem suporte para controle de versão de assemblies satélites. Você pode atualizar e reimplantar um assembly satélite sem atualizar o assembly principal. Depois de atualizar um assembly satélite, aumente o número da versão e envie-o com um assembly de política do publicador. No assembly de política do publicador, especifique que seu novo assembly satélite é compatível com a versão anterior. O gerenciador de recursos usará o atributo <xref:System.Resources.SatelliteContractVersionAttribute> para determinar a versão do assembly satélite, mas o carregador de assembly será associado à versão do assembly satélite especificada pela política do publicador. Para obter mais informações sobre assemblies de política do publicador, consulte [Criando um arquivo de política do publicador](../configure-apps/how-to-create-a-publisher-policy.md).  
  
 Para habilitar o suporte de controle de versão completo do assembly, é recomendável você implantar assemblies de nome forte no [cache de assembly global](../app-domains/gac.md) e implantar assemblies que não tenham nomes fortes no diretório do aplicativo. Se você desejar implantar assemblies de nome forte no diretório do aplicativo, você não poderá incrementar o número da versão de um assembly satélite quando você atualiza o assembly. Em vez disso, você deve executar uma atualização no local onde você substitui o código existente pelo código atualizado e mantém o mesmo número de versão. Por exemplo, se você deseja atualizar a versão 1.0.0.0 de um assembly satélite com o nome de assembly totalmente especificado "myApp.resources, Version=1.0.0.0, Culture=de, PublicKeyToken=b03f5f11d50a3a", substitua-o pelo myApp.resources.dll atualizado que foi compilado com o mesmo nome de assembly totalmente especificado "myApp.resources, Version=1.0.0.0, Culture=de, PublicKeyToken=b03f5f11d50a3a". Observe que usar atualizações locais nos arquivos de assembly satélite torna difícil para um aplicativo determinar com precisão a versão de um assembly satélite.  
  
 Para obter mais informações sobre controle de versão do assembly, consulte [Controle de versão do Assembly](../../standard/assembly/versioning.md) e [Como o runtime localiza assemblies](../deployment/how-the-runtime-locates-assemblies.md).  
  
<a name="from_file"></a>
## <a name="retrieving-resources-from-resources-files"></a>Para recuperar recursos dos arquivos .resources  
 Se você optar por não implantar recursos em assemblies satélites, ainda poderá usar um objeto <xref:System.Resources.ResourceManager> para acessar recursos de arquivos .resources diretamente. Para fazer isso, você deve implantar os arquivos .resources corretamente. Em seguida, você usa o método <xref:System.Resources.ResourceManager.CreateFileBasedResourceManager%2A?displayProperty=nameWithType> para criar instâncias de um objeto <xref:System.Resources.ResourceManager> e especificar o diretório que contém os arquivos .resources autônomos.  
  
### <a name="deploying-resources-files"></a>Implantando arquivos .resources  
 Quando você incorporar arquivos .resources em um assembly de aplicativo e assemblies satélites, cada assembly satélite possui o mesmo nome de arquivo, mas é colocado em um subdiretório que reflete a cultura do assembly satélite. Por outro lado, quando você acessar diretamente os recursos dos arquivos .resources, você pode colocar todos os arquivos .resources em um único diretório, geralmente um subdiretório do diretório do aplicativo. O nome do arquivo .resources do aplicativo padrão consiste em um nome de raiz, sem nenhuma indicação de sua cultura (por exemplo, strings.resources). Os recursos de cada cultura localizada são armazenados em um arquivo cujo nome consiste no nome de raiz seguido pela cultura (por exemplo, strings.ja.resources ou strings.de-DE.resources).

 A ilustração a seguir mostra onde os arquivos de recurso devem estar localizados na estrutura de diretórios. Ela também fornece as convenções de nomenclatura para arquivos .resource.  

 ![Ilustração que mostra o diretório principal do aplicativo.](./media/retrieving-resources-in-desktop-apps/resource-application-directory.gif)  
  
### <a name="using-the-resource-manager"></a>Usando o Gerenciador de Recursos  
 Depois de ter criado os recursos e colocando-os no diretório apropriado, você cria um objeto <xref:System.Resources.ResourceManager> para usar os recursos chamando o método <xref:System.Resources.ResourceManager.CreateFileBasedResourceManager%28System.String%2CSystem.String%2CSystem.Type%29>. O primeiro parâmetro especifica o nome da raiz do arquivo .resources padrão do aplicativo ("strings" para o exemplo na seção anterior). O segundo parâmetro especifica o local dos recursos ("Resources" no exemplo anterior). O terceiro parâmetro especifica a implementação de <xref:System.Resources.ResourceSet> a ser usada. Se o terceiro parâmetro for `null`, o runtime padrão <xref:System.Resources.ResourceSet> será usado.  
  
> [!NOTE]
> Não implante aplicativos do ASP.NET usando arquivos .resources autônomos. Isso pode causar problemas de bloqueio e interrompe a implantação de XCOPY. É recomendável que você implante recursos do ASP.NET em assemblies satélites. Para obter mais informações, consulte [Visão geral dos recursos da página da Web do ASP.NET](https://docs.microsoft.com/previous-versions/aspnet/ms227427(v=vs.100)).  
  
 Depois de criar uma instância do objeto <xref:System.Resources.ResourceManager>, você usará os métodos <xref:System.Resources.ResourceManager.GetString%2A>, <xref:System.Resources.ResourceManager.GetObject%2A> e <xref:System.Resources.ResourceManager.GetStream%2A> conforme discutido anteriormente para recuperar os recursos. No entanto, a recuperação de recursos diretamente de arquivos .resources é diferente da recuperação de recursos incorporados em assemblies. Quando você recupera os recursos de arquivos .resources, os métodos <xref:System.Resources.ResourceManager.GetString%28System.String%29>, <xref:System.Resources.ResourceManager.GetObject%28System.String%29> e <xref:System.Resources.ResourceManager.GetStream%28System.String%29> sempre recuperam os recursos da cultura padrão independentemente da cultura atual. Para recuperar os recursos da cultura atual ou de uma cultura específica do aplicativo, você deve chamar o método <xref:System.Resources.ResourceManager.GetString%28System.String%2CSystem.Globalization.CultureInfo%29>, <xref:System.Resources.ResourceManager.GetObject%28System.String%2CSystem.Globalization.CultureInfo%29> ou <xref:System.Resources.ResourceManager.GetStream%28System.String%2CSystem.Globalization.CultureInfo%29> e especificar a cultura cujos recursos devem ser recuperados. Para recuperar os recursos da cultura atual, especifique o valor da propriedade <xref:System.Globalization.CultureInfo.CurrentCulture%2A?displayProperty=nameWithType> como o argumento `culture`. Se o gerenciador de recursos não pode recuperar os recursos de `culture`, ele usa as regras de fallback de recurso padrões para recuperar os recursos apropriados.  
  
### <a name="an-example"></a>Um Exemplo  
 O exemplo a seguir ilustra como o gerenciador de recursos recupera recursos diretamente dos arquivos .resources. O exemplo consiste em três arquivos de recurso baseados em texto para as culturas de inglês (Estados Unidos), francês (França) e russo (Rússia). Inglês (Estados Unidos) é a cultura padrão do exemplo. Seus recursos são armazenados no seguinte arquivo denominado Strings.txt:  
  
```text
Greeting=Hello  
Prompt=What is your name?  
```  
  
 Recursos para a cultura francês (França) são armazenados no seguinte arquivo, que é chamado Strings.fr-FR.txt:  
  
```text
Greeting=Bon jour  
Prompt=Comment vous appelez-vous?  
```  
  
 Recursos para a cultura russo (França) são armazenados no seguinte arquivo, que é chamado Strings.ru-RU.txt:  
  
```text
Greeting=Здравствуйте  
Prompt=Как вас зовут?  
```  
  
 Este é o código-fonte para o exemplo. O exemplo cria uma instância dos objetos <xref:System.Globalization.CultureInfo> para as culturas inglês (Estados Unidos), inglês (Canadá), francês (França) e russo (Rússia) e faz de cada cultura a cultura atual. O método <xref:System.Resources.ResourceManager.GetString%28System.String%2CSystem.Globalization.CultureInfo%29?displayProperty=nameWithType>, em seguida, fornece o valor da propriedade <xref:System.Globalization.CultureInfo.CurrentCulture%2A?displayProperty=nameWithType> como o argumento `culture` para recuperar os recursos específicos à cultura apropriados.  
  
 [!code-csharp[Conceptual.Resources.Retrieving#9](../../../samples/snippets/csharp/VS_Snippets_CLR/conceptual.resources.retrieving/cs/example3.cs#9)]
 [!code-vb[Conceptual.Resources.Retrieving#9](../../../samples/snippets/visualbasic/VS_Snippets_CLR/conceptual.resources.retrieving/vb/example3.vb#9)]  
  
 Você pode compilar a versão em C# do exemplo, executando o seguinte arquivo em lotes. Se você estiver usando Visual Basic, substitua `csc` por `vbc` e substitua a extensão `.cs` por `.vb`.  
  
```console
Md Resources  
Resgen Strings.txt Resources\Strings.resources  
Resgen Strings.fr-FR.txt Resources\Strings.fr-FR.resources  
Resgen Strings.ru-RU.txt Resources\Strings.ru-RU.resources  
  
csc Example.cs  
```  
  
## <a name="see-also"></a>Confira também

- <xref:System.Resources.ResourceManager>
- [Recursos em aplicativos da área de trabalho](index.md)
- [Empacotando e implantando recursos](packaging-and-deploying-resources-in-desktop-apps.md)
- [Como o runtime localiza assemblies](../deployment/how-the-runtime-locates-assemblies.md)
- [Criando e recuperando recursos em aplicativos da Windows Store](https://docs.microsoft.com/previous-versions/windows/apps/hh694557(v=vs.140))
