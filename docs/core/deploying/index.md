---
title: Publicação de aplicativos
description: Saiba mais sobre as maneiras de publicar um aplicativo .NET Core. O .NET Core pode publicar aplicativos específicos da plataforma ou de plataforma cruzada. Você pode publicar um aplicativo como independente ou dependente de estrutura. Cada modo afeta como um usuário executa seu aplicativo.
ms.date: 04/01/2020
ms.openlocfilehash: f343e184a7ccca66aaf94533b2d0262478f873f4
ms.sourcegitcommit: c4a15c6c4ecbb8a46ad4e67d9b3ab9b8b031d849
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/20/2020
ms.locfileid: "88656580"
---
# <a name="net-core-application-publishing-overview"></a>Visão geral da publicação de aplicativos do .NET Core

Os aplicativos criados com o .NET Core podem ser publicados em dois modos diferentes, e o modo afeta a forma como um usuário executa seu aplicativo.

Publicar seu aplicativo como independente produz um aplicativo que inclui o *tempo de execução* e as bibliotecas do .NET Core e seu aplicativo e suas dependências. Os usuários do aplicativo podem executá-lo em um computador que não tem o tempo de execução do .NET Core instalado.

Publicar seu aplicativo como *dependente de estrutura* produz um aplicativo que inclui somente seu aplicativo e suas dependências. Os usuários do aplicativo precisam instalar separadamente o tempo de execução do .NET Core.

Os dois modos de publicação produzem um executável específico da plataforma por padrão. Aplicativos dependentes de estrutura podem ser criados sem um executável, e esses aplicativos são de plataforma cruzada.

Quando um executável é produzido, você pode especificar a plataforma de destino com um RID (identificador de tempo de execução). Para obter mais informações sobre RIDs, consulte [Catálogo de RID do .NET Core](../rid-catalog.md).

A tabela a seguir descreve os comandos usados para publicar um aplicativo como dependente da estrutura ou independente, por versão do SDK:

| Type                                                                                     | SDK 2.1 | SDK 3. x | Comando |
| ---------------------------------------------------------------------------------------  | ------- | ------- | ------- |
| [executável dependente de estrutura](#publish-framework-dependent) para a plataforma atual. |         | ✔️      | [`dotnet publish`](../tools/dotnet-publish.md) |
| [executável dependente de estrutura](#publish-framework-dependent) para uma plataforma específica.  |         | ✔️      | [`dotnet publish -r <RID> --self-contained false`](../tools/dotnet-publish.md) |
| [binário de plataforma cruzada dependente de estrutura](#publish-framework-dependent).               | ✔️      | ✔️      | [`dotnet publish`](../tools/dotnet-publish.md) |
| [executável independente](#publish-self-contained).                                    | ✔️      | ✔️      | [`dotnet publish -r <RID>`](../tools/dotnet-publish.md) |

Para obter mais informações, consulte o [comando dotnet Publish do .NET Core](../tools/dotnet-publish.md).

## <a name="produce-an-executable"></a>Produzir um executável

Os executáveis não são de plataforma cruzada. Elas são específicas para um sistema operacional e arquitetura de CPU. Ao publicar seu aplicativo e criar um executável, você pode publicar o [aplicativo como independente](#publish-self-contained) ou [dependente de estrutura](#publish-framework-dependent). A publicação de um aplicativo como independente inclui o tempo de execução do .NET Core com o aplicativo, e os usuários do aplicativo não precisam se preocupar com a instalação do .NET Core antes de executar o aplicativo. Os aplicativos publicados como dependentes da estrutura não incluem o tempo de execução e as bibliotecas do .NET Core; somente o aplicativo e as dependências de terceiros são incluídos.

Os comandos a seguir produzem um executável:

| Type                                                                                     | SDK 2.1 | SDK 3. x | Comando |
| ---------------------------------------------------------------------------------------- | ------- | ------- | ------- |
| [executável dependente de estrutura](#publish-framework-dependent) para a plataforma atual. |         | ✔️      | [`dotnet publish`](../tools/dotnet-publish.md) |
| [executável dependente de estrutura](#publish-framework-dependent) para uma plataforma específica.  |         | ✔️      | [`dotnet publish -r <RID> --self-contained false`](../tools/dotnet-publish.md) |
| [executável independente](#publish-self-contained).                                    | ✔️      | ✔️      | [`dotnet publish -r <RID>`](../tools/dotnet-publish.md) |

## <a name="produce-a-cross-platform-binary"></a>Produzir um binário de plataforma cruzada

Binários de plataforma cruzada são criados quando você publica seu aplicativo como [dependente de estrutura](#publish-framework-dependent), na forma de um arquivo *dll* . O arquivo *dll* é nomeado após o seu projeto. Por exemplo, se você tiver um aplicativo chamado **word_reader**, um arquivo chamado *word_reader.dll* será criado. Os aplicativos publicados dessa maneira são executados com o `dotnet <filename.dll>` comando e podem ser executados em qualquer plataforma.

Os binários de plataforma cruzada podem ser executados em qualquer sistema operacional, desde que o tempo de execução do .NET Core de destino já esteja instalado. Se o tempo de execução do .NET Core de destino não estiver instalado, o aplicativo poderá ser executado usando um tempo de execução mais recente se o aplicativo estiver configurado para ser revertido. Para obter mais informações, consulte [roll forward de aplicativos dependentes da estrutura](../versions/selection.md#framework-dependent-apps-roll-forward).

O comando a seguir produz um binário de plataforma cruzada:

| Type                                                                                 | SDK 2.1 | SDK 3. x | Comando |
| -----------------------------------------------------------------------------------  | ------- | ------- | ------- |
| [binário de plataforma cruzada dependente de estrutura](#publish-framework-dependent).           | ✔️      | ✔️      | [`dotnet publish`](../tools/dotnet-publish.md) |

## <a name="publish-framework-dependent"></a>Publicar a estrutura dependente

Os aplicativos publicados como dependentes da estrutura são de plataforma cruzada e não incluem o tempo de execução do .NET Core. O usuário do seu aplicativo é necessário para instalar o tempo de execução do .NET Core.

A publicação de um aplicativo como dependente de estrutura produz um [binário de plataforma cruzada](#produce-a-cross-platform-binary) como um arquivo *dll* e um [executável específico da plataforma](#produce-an-executable) destinado à sua plataforma atual. A *dll* é de plataforma cruzada enquanto o executável não está. Por exemplo, se você publicar um aplicativo chamado **word_reader** e o Windows de destino, um *word_reader.exe* executável será criado junto com *word_reader.dll*. Ao direcionar para Linux ou macOS, um *word_reader* executável é criado junto com *word_reader.dll*. Para obter mais informações sobre RIDs, consulte [Catálogo de RID do .NET Core](../rid-catalog.md).

> [!IMPORTANT]
> SDK do .NET Core 2,1 não produz executáveis específicos da plataforma quando você publica um dependente do App Framework.

O binário de plataforma cruzada do seu aplicativo pode ser executado com o `dotnet <filename.dll>` comando e pode ser executado em qualquer plataforma. Se o aplicativo usar um pacote NuGet que tenha implementações específicas de plataforma, as dependências de todas as plataformas serão copiadas para a pasta de publicação junto com o aplicativo.

Você pode criar um executável para uma plataforma específica passando os `-r <RID> --self-contained false` parâmetros para o [`dotnet publish`](../tools/dotnet-publish.md) comando. Quando o `-r` parâmetro é omitido, um executável é criado para sua plataforma atual. Todos os pacotes NuGet que têm dependências específicas da plataforma para a plataforma de destino são copiados para a pasta de publicação.

### <a name="advantages"></a>Vantagens

- **Implantação pequena**\
Somente seu aplicativo e suas dependências são distribuídos. O tempo de execução e as bibliotecas do .NET Core são instalados pelo usuário e todos os aplicativos compartilham o tempo de execução.

- **Plataforma cruzada**\
Seu aplicativo e qualquer. A biblioteca baseada em rede é executada em outros sistemas operacionais. Você não precisa definir uma plataforma de destino para seu aplicativo. Para obter informações sobre o formato de arquivo .NET, consulte [formato de arquivo de assembly .net](../../standard/assembly/file-format.md).

- **Usa o tempo de execução com patch mais recente**\
O aplicativo usa o tempo de execução mais recente (dentro da família principal do .NET Core de destino) instalada no sistema de destino. Isso significa que seu aplicativo usa automaticamente a versão mais recente com patches do tempo de execução do .NET Core. Esse comportamento padrão pode ser substituído. Para obter mais informações, consulte [roll forward de aplicativos dependentes da estrutura](../versions/selection.md#framework-dependent-apps-roll-forward).

### <a name="disadvantages"></a>Desvantagens

- **Requer a pré-instalação do tempo de execução**\
Seu aplicativo só poderá ser executado se a versão do .NET Core de destino do seu aplicativo já estiver instalada no sistema host. Você pode configurar o comportamento de roll-forward para que o aplicativo exija uma versão específica do .NET Core ou permitir uma versão mais recente do .NET Core. Para obter mais informações, consulte [roll forward de aplicativos dependentes da estrutura](../versions/selection.md#framework-dependent-apps-roll-forward).

- **O .NET Core pode ser alterado**\
É possível que as bibliotecas e o tempo de execução do .NET Core sejam atualizados no computador em que o aplicativo é executado. Em casos raros, isso pode alterar o comportamento do seu aplicativo se você usar as bibliotecas do .NET Core, que a maioria dos aplicativos faz. Você pode configurar como seu aplicativo usa versões mais recentes do .NET Core. Para obter mais informações, consulte [roll forward de aplicativos dependentes da estrutura](../versions/selection.md#framework-dependent-apps-roll-forward).

A desvantagem a seguir se aplica somente ao SDK do .NET Core 2,1.

- **Use o `dotnet` comando para iniciar o aplicativo**\
Os usuários devem executar o `dotnet <filename.dll>` comando para iniciar seu aplicativo. O SDK do .NET Core 2,1 não produz executáveis específicos da plataforma para aplicativos que dependem da estrutura publicada.

### <a name="examples"></a>Exemplos

Publicar um aplicativo dependente da estrutura de plataforma cruzada. Um executável que se destina à sua plataforma atual é criado junto com o arquivo *dll* .

```dotnet
dotnet publish
```

Publicar um aplicativo dependente da estrutura de plataforma cruzada. Um executável de 64 bits do Linux é criado junto com o arquivo *dll* . Este comando não funciona com SDK do .NET Core 2,1.

```dotnet
dotnet publish -r linux-x64 --self-contained false
```

## <a name="publish-self-contained"></a>Publicar autocontido

Publicar seu aplicativo como independente produz um executável específico da plataforma. A pasta de publicação de saída contém todos os componentes do aplicativo, incluindo as bibliotecas do .NET Core e o tempo de execução de destino. O aplicativo é isolado de outros aplicativos do .NET Core e não usa um tempo de execução compartilhado localmente instalado. O usuário do seu aplicativo não é necessário para baixar e instalar o .NET Core.

O binário executável é produzido para a plataforma de destino especificada. Por exemplo, se você tiver um aplicativo chamado **word_reader**e publicar um executável independente para o Windows, um arquivo de *word_reader.exe* será criado. Publicação para Linux ou macOS, um arquivo de *word_reader* é criado. A plataforma e a arquitetura de destino são especificadas com o `-r <RID>` parâmetro para o [`dotnet publish`](../tools/dotnet-publish.md) comando. Para obter mais informações sobre RIDs, consulte [Catálogo de RID do .NET Core](../rid-catalog.md).

Se o aplicativo tiver dependências específicas da plataforma, como um pacote NuGet que contém dependências específicas da plataforma, eles serão copiados para a pasta de publicação junto com o aplicativo.

### <a name="advantages"></a>Vantagens

- **Controlar versão do .NET Core**\
Você controla qual versão do .NET Core é implantada com seu aplicativo.

- **Direcionamento específico da plataforma**\
Como você precisa publicar seu aplicativo para cada plataforma, você sabe onde seu aplicativo será executado. Se o .NET Core introduzir uma nova plataforma, os usuários não poderão executar seu aplicativo nessa plataforma até que você libere uma versão direcionada a essa plataforma. Você pode testar seu aplicativo para problemas de compatibilidade antes que os usuários executem seu aplicativo na nova plataforma.

### <a name="disadvantages"></a>Desvantagens

- **Implantações maiores**\
Como seu aplicativo inclui o tempo de execução do .NET Core e todas as suas dependências de aplicativo, o tamanho do download e o espaço do disco rígido necessários são maiores que uma versão [dependente da estrutura](#publish-framework-dependent) .

  > [!TIP]
  > Você pode reduzir o tamanho da implantação em sistemas Linux em aproximadamente 28 MB usando o [*modo invariável de globalização*](https://github.com/dotnet/runtime/blob/master/docs/design/features/globalization-invariant-mode.md)do .NET Core. Isso força seu aplicativo a tratar todas as culturas como a [cultura invariável](xref:System.Globalization.CultureInfo.InvariantCulture?displayProperty=nameWithType).

  > [!TIP]
  > Há um [recurso de corte de visualização](trim-self-contained.md) que pode reduzir ainda mais o tamanho da sua implantação.

- **Mais difícil de atualizar a versão do .NET Core**\
O tempo de execução do .NET Core (distribuído com seu aplicativo) só pode ser atualizado com a liberação de uma nova versão do seu aplicativo. No entanto, o .NET Core atualizará os patches de segurança críticos conforme necessário para a biblioteca do Framework no computador em que seu aplicativo é executado. Você é responsável pela validação de ponta a ponta deste cenário de patch de segurança.

### <a name="examples"></a>Exemplos

Publicar um aplicativo independente. Um executável macOS 64-bit é criado.

```dotnet
dotnet publish -r osx-x64
```

Publicar um aplicativo independente. Um executável do Windows de 64 bits é criado.

```dotnet
dotnet publish -r win-x64
```

## <a name="see-also"></a>Consulte também

- [Implantando aplicativos .NET Core com o CLI do .NET Core.](deploy-with-cli.md)
- [Implantação de aplicativos .NET Core com o Visual Studio.](deploy-with-vs.md)
- [Catálogo de RID (identificador de tempo de execução) do .NET Core.](../rid-catalog.md)
- [Selecione a versão do .NET Core a ser usada.](../versions/selection.md)
