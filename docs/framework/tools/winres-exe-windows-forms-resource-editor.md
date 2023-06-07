---
title: Winres.exe (Windows Resource Localization Editor)
description: Use Winres.exe, o editor de localização de recursos do Windows. Essa ferramenta de layout visual ajuda os especialistas de localização a localizar Windows Forms recursos de interface do usuário usados por formulários.
ms.date: 08/15/2018
helpviewer_keywords:
- Winres.exe
- Windows Forms Resource Editor
- Windows Resource Localization Editor
- localization
- Windows Forms, localization
- forms, localizing
- resx files
- .resx files
ms.assetid: cb8bc835-9221-4888-af53-1a4f5fad6c48
ms.openlocfilehash: 8393eb44246ed1bbc0e4e6acc84ad7bd57041ec0
ms.sourcegitcommit: b4f8849c47c1a7145eb26ce68bc9f9976e0dbec3
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/03/2020
ms.locfileid: "87516899"
---
# <a name="winresexe-windows-resource-localization-editor"></a>Winres.exe (Windows Resource Localization Editor)

O Windows Resource Localization Editor, Winres.exe, é uma ferramenta de layout visual que ajuda especialistas na localização de recursos de interface do usuário do Windows Forms usados por formulários. Os arquivos .resx ou .resources usados como entrada em Winres.exe podem ser criados usando-se um ambiente de design visual como, por exemplo, Microsoft Visual Studio. Para obter informações sobre como implantar recursos em aplicativos do .NET Framework, consulte [Recursos nos aplicativos de área de trabalho](../resources/index.md).

O Winres.exe é instalado com o Visual Studio. Para executar a ferramenta, use o Prompt de Comando do Desenvolvedor para Visual Studio. Para obter mais informações, consulte [Prompts de Comando](developer-command-prompt-for-vs.md).

## <a name="syntax"></a>Sintaxe

```console
winres resourceFile
winres /?
```

## <a name="arguments"></a>Argumentos

|Argumento|Descrição|
|--------------|-----------------|
|`resourceFile`|O arquivo de recurso a ser localizado. Esse arquivo deve ser um arquivo .resx ou .resources de formulário do Windows Forms gerado pelo designer do Visual Studio. Winres.exe não pode abrir arquivos .resx ou .resources genéricos.|

|Opção|Descrição|
|------------|-----------------|
|**/?**|Exibe sintaxe de comando e opções para a ferramenta.|

## <a name="remarks"></a>Comentários

O estado de elementos de interface do usuário de um formulário em um projeto do Windows Forms costuma ser armazenado em arquivos de recurso, que são arquivos com base em XML com a extensão .resx ou as versões binárias compiladas correspondentes com a extensão .resources. Winres.exe é uma ferramenta que permite a edição limitada de qualquer tipo de arquivo fora do ambiente de design do Visual Studio. Mais especificamente, ela permite os seguintes tipos de operações de edição:

- Um arquivo de recurso de cultura neutro ou específico pode ser editado para alterar as propriedades de interface do usuário do formulário ou seus controles como, por exemplo, seu texto, tamanho ou posição.

- Os arquivos de recursos de cultura neutros ou específicos podem ser gerados com base no arquivo de recurso padrão.

- Um arquivo de recurso de cultura pode ser salvo como outro arquivo de recurso da cultura. Por exemplo, um arquivo de recurso em inglês (EUA) pode ser salvo como um arquivo de recurso em polonês. O novo arquivo normalmente seria editado na sequência para ser compatível com a nova cultura.

Consulte também [Organização hierárquica de recursos para localização](https://docs.microsoft.com/previous-versions/visualstudio/visual-studio-2012/756hydy4(v=vs.110)) ou [Organização hierárquica de recursos para localização](https://docs.microsoft.com/previous-versions/visualstudio/visual-studio-2013/756hydy4(v=vs.120)).

Winres.exe não pode converter um arquivo .resx em seu arquivo .resources correspondente; use a ferramenta Resgen.exe em vez disso. Para saber mais sobre o Resgen.exe, consulte [Resgen.exe (Gerador de Arquivo de Recurso)](resgen-exe-resource-file-generator.md).

Winres.exe é um aplicativo gráfico que recria uma versão de tempo de design de um formulário do Windows Forms apenas com base no arquivo de recurso, sem que seja necessário ter acesso ao código-fonte. O Winres.exe hospeda a janela **Designer de Formulários** e **Propriedades** do Windows Forms do Visual Studio. Esses recursos permitem a edição visual de um arquivo .resources ou .resx contendo um formulário do Windows Forms. Os localizadores normalmente usam Winres.exe para editar rótulos de controle e ajustar o local e o tamanho dos controles para acomodar os rótulos da cultura de destino.

Se não puder resolver o tipo de um controle, Winres.exe criará um controle de espaço reservado no arquivo .resx ou .resources localizado. O controle de espaço reservado é exibido no formulário do Windows Forms como uma janela com hachura. O tamanho e a posição da janela com hachura correspondem aos do controle real. Todas as propriedades localizáveis disponíveis para o controle de espaço reservado são exibidas na janela **Propriedades**. Todas as alterações feitas no controle de espaço reservado são salvas para o controle real.

## <a name="winresexe-versus-visual-studio"></a>Winres.exe x Visual Studio

Em geral, antes de começar a localizar formulários do Windows Forms de um aplicativo, você deve decidir se deseja usar o Visual Studio ou Winres.exe como a ferramenta de localização. A compatibilidade de versão, conforme descrita posteriormente, pode impedir a alternância de uma ferramenta para outra.

A vantagem do Visual Studio é que é possível usá-lo para desenvolver e localizar um aplicativo. Para localizar um formulário, após a conclusão do desenvolvimento, defina o <xref:System.ComponentModel.LocalizableAttribute> do formulário (a propriedade **Localizável** no editor de **Propriedades**) como `true` e altere a propriedade **Idioma** para a cultura de destino desejada. Em seguida, edite cadeias de caracteres e ajuste o local e o tamanho dos controles para acomodar as cadeias de caracteres para a cultura de destino. Quando você salva o arquivo .resx localizado, o Visual Studio grava apenas as propriedades localizáveis (propriedades que foram alteradas na cultura de destino) no arquivo. O Visual Studio cria automaticamente um assembly satélite para o arquivo .resx localizado no local do diretório correto.

Embora o Visual Studio ofereça um ambiente de desenvolvimento e localização integrado, o Winres.exe é a ferramenta recomendada a ser usada se a localização for feita por localizadores de terceiros. Como é uma ferramenta apenas de localização, Winres.exe permite uma separação mais clara do código de um aplicativo dos formulários que serão localizados, o que é mais prático para gerenciar grandes projetos.

## <a name="using-winresexe"></a>Usando Winres.exe

Para localizar usando o Winres.exe, você deve primeiro desenvolver um aplicativo usando um designer visual como o **Designer de Formulários do Windows** no Visual Studio. Quando o desenvolvimento for concluído, defina o <xref:System.ComponentModel.LocalizableAttribute> do formulário (a propriedade **Localizável** no editor de **Propriedades**) como `true` e, em seguida, entregue o arquivo .resx da cultura padrão para um localizador de terceiros. Esse arquivo .resx contém informações extras que Winres.exe usa para recriar uma versão do tempo de design do formulário original.

> [!NOTE]
> Winres.exe não pode ser usado para editar o arquivo de recurso padrão. Winres.exe interpreta todas as propriedades alteradas como propriedades localizadas e as salva no arquivo de recurso da cultura de destino.

As versões finais dos arquivos de recurso da cultura podem finalmente ser usadas para criar versões localizadas do aplicativo. Para saber mais, veja [Recursos em aplicativos da área de trabalho](../resources/index.md).

O Winres.exe tem os seguintes recursos e funcionalidades:

- Winres pode operar no SFM (Single File Mode) ou no VSFM (Visual Studio File Mode). SFM é o modo herdado em que informações completas sobre o formulário e seu conteúdo são armazenadas no arquivo de recurso. VSFM armazena apenas as alterações culturais no arquivo de recurso.

- Uma janela de relatório de erros, encaixada no canto inferior esquerdo da janela principal.

- As teclas de acesso podem ser verificadas em busca de duplicatas: no menu **Formato**, clique no comando **Verificar teclas de acesso**.

## <a name="version-compatibility"></a>Compatibilidade de versões

Você deve usar a versão de Winres.exe liberada com o .NET Framework que você está usando. A tabela a seguir lista as versões compatíveis:

|Visual Studio|.NET Framework|Winres.exe|
|-------------------|--------------------|----------------|
|Visual Studio .NET 2002|1.0|1.0|
|Visual Studio .NET 2003|1,1|1,1|
|Visual Studio 2005|2.0|2.0|
|Visual Studio 2008|3.0 e 3.5|3.0 e 3.5|
|Visual Studio 2010|4,0|4,0|
|Visual Studio 2017|4,6|4,6|

> [!NOTE]
> Embora VSFM tenha a vantagem de ser compatível com o Visual Studio, como armazena apenas valores alterados no arquivo de recurso, Winres.exe exige que os pais do arquivo de recurso atual estejam localizados no mesmo diretório. Por exemplo, a edição de `TestApp.de-DE.resources`, um arquivo de recurso em alemão na Alemanha, exige a presença do arquivo de recurso padrão, `TestApp.resx`, e possivelmente do arquivo de recurso de cultura neutra, `TestApp.de.resources`.

## <a name="examples"></a>Exemplos

### <a name="to-localize-a-resx-or-resources-file-associated-with-a-form"></a>Para localizar um arquivo .resx ou .resources associado a um formulário

1. Digite `winres` no prompt de comando do desenvolvedor para executar Winres.exe.

2. Para abrir os recursos padrão que serão localizados por um formulário, clique no comando **Abrir** do menu **Arquivo** e navegue até o arquivo para abri-lo.

     -ou-

     Especifique o arquivo a ser aberto na linha de comando ao iniciar Winres.exe.

     O comando a seguir inicia Winres.exe e carrega o formulário associado a `TestApp.resx` no Designer de Formulários.

    ```console
    winres TestApp.resx
    ```

     O comando a seguir inicia Winres.exe e carrega o formulário associado a `TestApp.resources` no Designer de Formulários.

    ```console
    winres TestApp.resources
    ```

    > [!NOTE]
    > Se o formulário cujos recursos você está editando for formulário herdado, o assembly apresentava o formulário herdado e o assembly contendo o formulário (derivado) deverá ser registrado no cache de assembly global (GAC) ou residir no mesmo diretório de WinRes.exe. Para obter mais informações sobre como instalar componentes do .NET Framework no GAC, consulte [Cache de Assembly Global](../app-domains/gac.md).

3. Selecione controles no formulário e altere <xref:System.Windows.Forms.Control.Text%2A> e outras propriedades para refletir a cultura localizada e seu idioma. Mova ou redimensione controles conforme necessário para acomodar o texto localizado.

4. Para salvar a versão localizada do arquivo .resx ou .resources, clique no ícone **Salvar** ou no mesmo comando do menu **Arquivo**. A ferramenta exibe a janela **Selecionar Cultura**.

5. Selecione a cultura apropriada e o modo de arquivo e clique em **OK**.

   A ferramenta salva o arquivo usando a convenção de nomenclatura esperada pelo tempo de execução para arquivos de recurso localizados. Por exemplo, se você localizar `TestApp.resources` para o alemão na Alemanha, a ferramenta salvará o arquivo como `TestApp.de-DE.resources`. Se você localizar `TestApp.resx` para o alemão na Alemanha, a ferramenta salvará o arquivo como `TestApp.de-DE.resx`. Para obter mais informações sobre as convenções de nomenclatura de recurso, consulte [Empacotando e implantando recursos](../resources/packaging-and-deploying-resources-in-desktop-apps.md). Para obter uma lista dos nomes de cultura predefinidos usados pelo tempo de execução, consulte a classe <xref:System.Globalization.CultureInfo>.

## <a name="see-also"></a>Consulte também

- <xref:System.ComponentModel.LocalizableAttribute>
- <xref:System.Globalization.CultureInfo>
- <xref:System.Resources.ResourceManager>
- <xref:System.Resources.ResourceReader>
- <xref:System.Resources.ResourceWriter>
- [Ferramentas](index.md)
- [Recursos em aplicativos da área de trabalho](../resources/index.md)
- [Globalização e localização](../../standard/globalization-localization/index.md)
