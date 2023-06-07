---
title: MageUI.exe (Ferramenta de Geração e Edição de Manifesto, cliente gráfico)
description: Explore MageUI.exe, o Manifest Generation and Editing Tool, o cliente gráfico. Crie, edite e assine manifestos de aplicativo e implantação com esta ferramenta de GUI.
ms.date: 03/30/2017
helpviewer_keywords:
- Manifest Generation and Editing tool
- MageUI.exe
ms.assetid: f9e130a6-8117-49c4-839c-c988f641dc14
ms.openlocfilehash: 1ff7c10a478f0f0f53d7b00f34b228b9070f798d
ms.sourcegitcommit: b4f8849c47c1a7145eb26ce68bc9f9976e0dbec3
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/03/2020
ms.locfileid: "87516717"
---
# <a name="mageuiexe-manifest-generation-and-editing-tool-graphical-client"></a>MageUI.exe (Ferramenta de Geração e Edição de Manifesto, cliente gráfico)

MageUI.exe dá suporte à mesma funcionalidade que a ferramenta de linha de comando Mage.exe, mas com uma interface do usuário (UI) com base no Windows. Com essa ferramenta é possível criar, editar e assinar manifestos de implantação e aplicativo. Novos manifestos criados com MageUI.exe direcionam o perfil de cliente do .NET Framework 4. As versões anteriores de MageUI.exe devem ser usadas para segmentar versões do .NET Framework anteriores. Ao adicionar ou remover assemblies de um manifesto ou assinar novamente manifestos existentes, MageUI.exe não atualiza o manifesto para o perfil de cliente do .NET Framework 4. Para obter mais informações, consulte [Mage.exe (manifest Generation and Editing Tool)](mage-exe-manifest-generation-and-editing-tool.md).

 Essa ferramenta é instalada automaticamente com o Visual Studio. Para executar a ferramenta, use o Prompt de Comando do Desenvolvedor para Visual Studio (ou o Prompt de Comando do Visual Studio no Windows 7). Para obter mais informações, consulte [Prompts de Comando](developer-command-prompt-for-vs.md).

 Duas versões de Mage.exe e de MageUI.exe estão incluídas como um componente do Visual Studio. Para consultar informações da versão, execute MageUI.exe, selecione **Ajuda** e **Sobre**. Esta documentação descreve a versão 4.0.x.x de Mage.exe e de MageUI.exe.

> [!NOTE]
> MageUI.exe não dá suporte ao elemento [compatibleFrameworks](/visualstudio/deployment/compatibleframeworks-element-clickonce-deployment) ao salvar um manifesto do aplicativo já assinado com um certificado usando MageUI.exe. Em vez disso, você deve usar [Mage.exe](mage-exe-manifest-generation-and-editing-tool.md).  
  
## <a name="uielement-list"></a>Lista de elementos de interface do usuário  
 A tabela a seguir lista os itens de menu e da barra de ferramentas disponíveis.  
  
|Comando|Menu|Atalho|Descrição|  
|-------------|----------|--------------|-----------------|  
|**Manifesto do aplicativo**|**Arquivo, novo**||Cria um novo manifesto de aplicativo.|  
|**Manifesto de implantação**|**Arquivo, novo**||Cria um novo manifesto de implantação.|  
|**Abrir**|**Arquivo**|Ctrl+O|Abre um manifesto de implantação existente, o manifesto do aplicativo ou a licença de confiança para edição.|  
|**Fechar**|**Arquivo**|CTRL+F4|Fecha um arquivo aberto.<br /><br /> Se você modificar um arquivo antes de fechá-lo, MageUI.exe solicitará uma nova assinatura do arquivo com uma chave pública, um par de chaves ou um certificado armazenado.|  
|**Salvar**|**Arquivo**|Ctrl+S|Salva em disco o documento que tem o foco de entrada do usuário no momento.|  
|**Salvar Como**|**Arquivo**||Salva um arquivo em disco, permitindo para fornecer um novo nome de arquivo e/ou um local.|  
|**Salvar tudo**|**Arquivo**||Salva as alterações feitas em todos os arquivos abertos atualmente dentro de MageUI.exe.|  
|**Preferências**|**Arquivo**||Abre a caixa de diálogo **Preferências**. Consulte a seguinte seção para obter mais informações.|  
|**Sair**|**Arquivo**|ALT+F4|Fecha MageUI.exe.|  
|**Recortar**|**Editar**|CTRL+X|Remove o texto selecionado no momento do aplicativo e o move para a Área de Transferência do sistema.|  
|**Cópia**|**Editar**|CTRL+C|Copia o texto selecionado no momento para a Área de Transferência do sistema.|  
|**Colar**|**Editar**|CTRL+V|Cola o texto da Área de Transferência do sistema para o elemento de texto ativo no momento.|  
|**Excluir**|**Editar**||Exclui um elemento selecionado no momento em uma lista, como uma licença de confiança na guia **Manifesto de Implantação**.|  
|**Fechar tudo**|**Window**||Fecha todos os arquivos abertos no momento em MageUI.exe. Se um ou mais arquivos precisar de gravação, MageUI.exe solicitará que você os salve. MageUI.exe também solicita a seleção de uma chave de assinatura para cada arquivo não assinado ou alterado.|  
|**Sobre**|**Ajuda**||Exibe informações de versão e direitos autorais sobre MageUI.exe.|  
  
## <a name="preferences-dialog-box"></a>Caixa de Diálogo Preferências  
 A caixa de diálogo **Preferências** contém os elementos a seguir.  
  
|Elemento da interface do usuário|Descrição|  
|----------------|-----------------|  
|**Assinar ao salvar**|Solicita a assinatura de um arquivo sempre que você salva as modificações.|  
|**Usar certificado de assinatura padrão**|Usa a chave inserida na caixa de texto **Arquivo de certificado** para assinar todos os arquivos. Isso elimina o prompt de assinatura que normalmente é exibido quando você salva um arquivo e **Assinar ao Salvar** é selecionado. Use o botão de reticências (**…**) próximo à caixa de texto **Arquivo de certificado** para selecionar um arquivo de chave.|  
|Algoritmo de resumo|Especifica o algoritmo com o qual gerar resumos de dependência. O valor deve ser "sha256RSA" ou "sha1RSA". Usa SHA1 como o padrão. Usado em manifestos de aplicativo e implantação. Se o usuário fornecer um certificado ao salvar o manifesto, ele usará os algoritmos no certificado para produzir resumos de dependência.|  
  
## <a name="signing-options-dialog-box"></a>Caixa de Diálogo Opções de Assinatura  
 A caixa de diálogo **Opções de Assinatura** é exibida quando você salva um manifesto ou uma licença de confiança pela primeira vez ou quando você altera um manifesto ou uma licença de confiança. Ela só será exibida se a opção **Assinar ao Salvar** na caixa de diálogo **Preferências** for selecionada. Você deve estar conectado à Internet ao assinar um manifesto que especifique um valor na caixa de texto **URI de Carimbo de Data/Hora**.  
  
 Essa caixa de diálogo contém os elementos a seguir.  
  
|Elemento da interface do usuário|Descrição|  
|----------------|-----------------|  
|**Assinar com arquivo de certificado**|Assina o manifesto com um certificado digital armazenado no sistema de arquivos.|  
|**Arquivo**|Fornece uma área para digitar o caminho para o arquivo .pfx que representa o certificado.|  
|**...**|Abre uma caixa de diálogo **Escolher Arquivo** para selecionar um arquivo .pfx existente.|  
|**Novo**|Gera um novo .pfx que não é verificável por meio de uma CA (Autoridade de Certificação). Para obter mais informações sobre os tipos de certificados usados na assinatura de implantações ClickOnce, consulte [Visão geral sobre implantação de aplicativos confiáveis](/visualstudio/deployment/trusted-application-deployment-overview).|  
|**Senha**|Fornece uma área para digitar a senha usada na assinatura desse certificado. Se não for aplicável, poderá ser deixado em branco.|  
|**Assinar com certificado armazenado**|Exibe uma lista selecionável de certificados digitais armazenados no repositório de certificados do computador.|  
|**URI de Carimbo de Data/Hora**|Exibe a URL (Uniform Resource Identifier) de um serviço de carimbo de data/hora digital. O carimbo de data/hora dos manifestos evita que você precise assinar novamente os manifestos, caso o certificado digital expire antes de implantar a próxima versão do aplicativo. Para obter mais informações, consulte [Membros do programa de certificado raiz do Windows](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/dn265983(v=ws.11)) e [ClickOnce e Authenticode](/visualstudio/deployment/clickonce-and-authenticode).|  
|**Não Assinar**|Permite salvar o manifesto sem adicionar uma assinatura de um certificado digital.|  
  
## <a name="tab-and-panel-descriptions"></a>Descrições da Guia e do Painel  
 Quando você abre um documento com MageUI.exe, ele é exibido dentro de sua própria página da guia. Cada guia contém um conjunto de painéis da propriedade. Os painéis contêm subconjuntos de dados do documento agrupados.  
  
### <a name="application-manifest-tab"></a>Guia Manifesto do Aplicativo  
 A guia **Manifesto do Aplicativo** exibe o conteúdo de um manifesto do aplicativo. O manifesto do aplicativo descreve todos os arquivos incluídos com a implantação e as permissões necessárias para o aplicativo ser executado no cliente.  
  
 A guia **Manifesto do Aplicativo** contém as seguintes guias.  
  
|Elemento da interface do usuário|Descrição|  
|----------------|-----------------|  
|**Nome**|Especifica informações de identificação sobre essa implantação.|  
|**Descrição**|Especifica o publicador, o produto e as informações de suporte.|  
|**Opções do Aplicativo**|Especifica se esse é um aplicativo de navegador e se esse manifesto é a origem das informações de confiança.|  
|**Arquivos**|Especifica todos os arquivos que constituem essa implantação.|  
|**Permissões necessárias**|Especifica o conjunto de permissões mínimas exigido pelo aplicativo para ser executado em um cliente.|  
  
### <a name="name-tab"></a>Guia Nome  
 A guia **Nome** é exibida quando você primeiro cria ou abre um manifesto do aplicativo. Ela identifica exclusivamente a implantação e, opcionalmente, especifica uma plataforma de destino válida.  
  
|Elemento da interface do usuário|Descrição|  
|----------------|-----------------|  
|**Nome**|Obrigatórios. O nome do manifesto do aplicativo. Normalmente o mesmo que o nome do arquivo.|  
|**Versão**|Obrigatórios. O número de versão da implantação no formato *N.N.N.N*. Somente o primeiro número de build principal é necessário. Por exemplo, para a versão 1.0 de um aplicativo, os valores válidos incluiriam `1`, `1.0`, `1.0.0` e `1.0.0.0`.|  
|**Processador**|Opcional. A arquitetura do computador no qual essa implantação pode ser executada. O padrão é `msil` ou Microsoft Intermediate Language, que é o formato padrão de todos os assemblies gerenciados. Altere este campo se você tiver pré-compilado os assemblies em seu aplicativo para uma arquitetura específica. Para obter mais informações sobre a pré-compilação, consulte [Ngen.exe (Gerador de Imagens Nativas)](ngen-exe-native-image-generator.md).|  
|**Cultura**|Opcional. O código ISO de país e região composto por duas partes em que o aplicativo é executado. O padrão é `neutral`.|  
|**Token de chave pública**|Opcional. A chave pública com a qual o manifesto do aplicativo foi assinado. Se esse for um manifesto novo ou não assinado, esse campo aparecerá como `Unsigned`.|  
  
### <a name="description-tab"></a>Guia Descrição  
 Essas informações geralmente são fornecidas no manifesto de implantação. Esses campos poderão ser modificados apenas se a caixa de seleção **Use Application Manifest Trust Information (Usar Informações de Confiança do Manifesto do Aplicativo)** estiver marcada na guia **Opções do Aplicativo**.  
  
|Elemento da interface do usuário|Descrição|  
|----------------|-----------------|  
|**Publicador**|O nome da pessoa ou organização responsável pelo aplicativo. Esse valor é usado como o nome da pasta do menu Iniciar.|  
|**Product**|O nome completo do produto. Se você selecionou **Instalar Localmente** para o elemento **Tipo de Aplicativo** na guia **Opções de Implantação** do manifesto de implementação, esse nome será o que aparece no link do menu **Iniciar** e em **Adicionar ou Remover Programas** para esse aplicativo.|  
|**Local de Suporte**|A URL na qual os clientes podem obter ajuda e suporte para o aplicativo.|  
  
### <a name="application-options-tab"></a>Guia Opções do Aplicativo  
  
|Elemento da interface do usuário|Descrição|  
|----------------|-----------------|  
|**Aplicativo de Navegador do Windows Presentation Foundation**|Especifica se este é um aplicativo WPF que é executado no navegador como um XBAP (aplicativo de navegador XAML).|  
|**Usar Informações de Confiança do Manifesto do Aplicativo**|Especifica se esse manifesto contém informações de confiança.|  
  
### <a name="files-tab"></a>Guia Arquivos  
  
|Elemento da interface do usuário|Descrição|  
|----------------|-----------------|  
|**Diretório de aplicativo**|O diretório no qual residem os arquivos do aplicativo. Use o botão de reticências (**...**) para selecionar o diretório.|  
|**Popular**|Adiciona todos os arquivos no diretório e subdiretórios do aplicativo ao manifesto do aplicativo. Se MageUI.exe localizar um único arquivo executável no diretório, ele o marcará automaticamente como o Ponto de Entrada, que é o arquivo executado primeiro quando o aplicativo ClickOnce é inicializado no cliente.|  
|**Arquivos de aplicativo**|Lista todos os arquivos no aplicativo. Cada arquivo tem três atributos editáveis, discutidos abaixo.|  
|**Tipo de arquivo**|O Tipo de Arquivo pode ser um dos quatro valores:<br /><br /> – Nenhum.<br />– Ponto de Entrada. O executável principal do aplicativo. Apenas um arquivo executável pode ser marcado como o ponto de entrada.<br />– Arquivo de Dados. Um arquivo, como um arquivo XML, que fornece dados para o aplicativo.<br />– Arquivo de Ícone. Um ícone do aplicativo, como aparece na área de trabalho ou no canto da janela do aplicativo.|  
|**Opcional**|Arquivos marcados como opcionais não são baixados na instalação inicial ou na atualização, mas podem ser baixados em tempo de execução usando a API sob demanda System.Deployment. Para obter mais informações, consulte [Walkthrough: Downloading Assemblies on Demand with the ClickOnce Deployment API Using the Designer](/visualstudio/deployment/walkthrough-downloading-assemblies-on-demand-with-the-clickonce-deployment-api-using-the-designer) (Instruções passo a passo: baixando assemblies sob demanda com a API de implantação do ClickOnce usando o designer).|  
|**Grupo**|Um rótulo para um conjunto de arquivos opcionais. Você pode aplicar um rótulo Grupo a um conjunto de arquivos e usar a API sob demanda para baixar um lote de arquivos com uma única chamada à API.|  
  
### <a name="permissions-required-tab"></a>Guia Permissões Necessárias  
 Use a guia **Permissões Necessárias** se precisar conceder ao aplicativo mais acesso ao computador local do que é concedido por padrão. Para obter mais informações, consulte [Securing ClickOnce Applications](/visualstudio/deployment/securing-clickonce-applications) (Protegendo aplicativos ClickOnce).  
  
|Elemento da interface do usuário|Descrição|  
|----------------|-----------------|  
|**Tipo do conjunto de permissões**|O conjunto de permissões mínimas exigido por esse aplicativo para ser executado no cliente. Para obter uma descrição desses conjuntos de permissões e quais permissões eles exigem ou não, confira [Conjuntos de permissões nomeadas](https://docs.microsoft.com/previous-versions/dotnet/netframework-4.0/4652tyx7(v=vs.100)).|  
|**Detalhes**|O XML criado para o manifesto do aplicativo para representar o conjunto de permissões. A menos que tenha um bom entendimento do formato XML do manifesto do aplicativo, você não deve editar esse XML manualmente. Para obter mais informações, consulte [manifesto do aplicativo ClickOnce](/visualstudio/deployment/clickonce-application-manifest).|  
  
### <a name="deployment-manifest-tab"></a>Guia Manifesto de Implantação  
 A guia **Manifesto de Implantação** contém as seguintes guias.  
  
|Elemento da interface do usuário|Descrição|  
|----------------|-----------------|  
|**Nome**|Especifica informações de identificação sobre essa implantação.|  
|**Descrição**|Especifica o publicador, o produto e as informações de suporte.|  
|**Opções de implantação**|Especifica informações adicionais sobre a implantação, como o tipo de aplicativo e o local de início.|  
|**Opções de atualização**|Especifica com que frequência o ClickOnce deve verificar se há atualizações de aplicativo.|  
|**Referência do aplicativo**|Especifica o manifesto do aplicativo para essa implantação.|  
  
### <a name="name-tab"></a>Guia Nome  
 A guia **Nome** é exibida quando você primeiro cria ou abre um manifesto de implantação. Ela identifica exclusivamente a implantação e, opcionalmente, especifica uma plataforma de destino válida.  
  
|Elemento da interface do usuário|Descrição|  
|----------------|-----------------|  
|**Nome**|Obrigatórios. O nome do manifesto de implantação. Normalmente o mesmo que o nome do arquivo.|  
|**Versão**|Obrigatórios. O número de versão da implantação no formato *N.N.N.N*. Somente o primeiro número de build principal é necessário. Por exemplo, para a versão 1.0 de um aplicativo, os valores válidos incluiriam `1`, `1.0`, `1.0.0` e `1.0.0.0`.|  
|**Processador**|Opcional. A arquitetura do computador no qual essa implantação pode ser executada. O padrão é `msil` ou Microsoft Intermediate Language, o formato padrão de todos os assemblies gerenciados. Altere este campo se você tiver compilado os assemblies em seu aplicativo para uma arquitetura específica.|  
|**Cultura**|Opcional. O código ISO de país/região composto por duas partes em que o aplicativo é executado. O padrão é `neutral`.|  
|**Token de chave pública**|Opcional. A chave pública com a qual o manifesto de implantação foi assinado. Se esse for um manifesto novo ou não assinado, esse campo aparecerá como `Unsigned`.|  
  
### <a name="description-tab"></a>Guia Descrição  
  
|Elemento da interface do usuário|Descrição|  
|----------------|-----------------|  
|**Publicador**|Obrigatórios. O nome da pessoa ou organização responsável pelo aplicativo. Esse valor é usado como o nome da pasta do menu Iniciar.|  
|**Product**|Obrigatórios. O nome completo do produto. Se você selecionou **Instalar Localmente** para o elemento **Tipo de Aplicativo** na guia **Opções de Implantação**, esse nome será o que aparece no link do menu **Iniciar** e em **Adicionar ou Remover Programas** para esse aplicativo.|  
|**Local de Suporte**|Opcional. A URL na qual os clientes podem obter ajuda e suporte para o aplicativo.|  
  
### <a name="deployment-options-tab"></a>Guia Opções de Implantação  
  
|Elemento da interface do usuário|Descrição|  
|----------------|-----------------|  
|**Tipo de aplicativo**|Opcional. Especifica se este aplicativo se instala no computador cliente (**Instalar Localmente**), é executado online (**Somente Online**) ou é um aplicativo do WPF que é executado no navegador (**Aplicativo de Navegador WPF**). O padrão é **Instalar Localmente**.|  
|**Local inicial**|Opcional. A URL da qual o aplicativo realmente deve ser iniciado. Útil ao implantar um aplicativo de um CD que deve se atualizar da Web.|  
|**Incluir o Local Inicial (ProviderURL) no manifesto**|Opcional. Especifica a URL que o ClickOnce examinará em busca de atualizações de aplicativo.|  
|**Executar o aplicativo automaticamente após a instalação**|Obrigatórios. Especifica que o aplicativo ClickOnce deve ser executado imediatamente após a instalação inicial de uma URL. O padrão é a caixa de seleção estar marcada.|  
|**Permitir que parâmetros de URL sejam passados para o aplicativo**|Obrigatórios. Permite a transferência de dados do parâmetro para o aplicativo ClickOnce por meio de uma cadeia de caracteres de consulta acrescentada à URL do manifesto de implantação. O padrão é a caixa de seleção estar desmarcada.|  
|**Usar a extensão de arquivo .deploy**|Obrigatórios. Quando selecionada, todos os arquivos no manifesto do aplicativo devem ter a extensão .deploy. O padrão é a caixa de seleção estar desmarcada.|  
  
### <a name="update-options-tab"></a>Guia Opções de Atualização  
 A guia **Opções de Atualização** contém as opções mencionadas aqui apenas quando a caixa de seleção **Tipo de Aplicativo** na guia **Nome** está definida como **Instalar Localmente**.  
  
|Elemento da interface do usuário|Descrição|  
|----------------|-----------------|  
|**Este aplicativo deve procurar atualizações**|Especifica se o ClickOnce deve verificar se há atualizações de aplicativo. Se essa caixa de seleção não estiver selecionada, o aplicativo não verificará se há atualizações, a menos que você o atualize programaticamente usando as APIs no namespace <xref:System.Deployment.Application>.|  
|**Escolha quando o aplicativo deve verificar se há atualizações**|Fornece duas opções para verificações de atualização:<br /><br /> -   **Antes do aplicativo ser inicializado**. A verificação de atualização é executada antes da execução do aplicativo.<br />-   **Depois de o aplicativo ser inicializado**. A verificação de atualização começa quando o formulário principal do aplicativo é inicializado e será executada na próxima vez que o aplicativo for iniciado.|  
|**Frequência de verificação de atualização**|Determina a frequência com que o ClickOnce deve verificar se há atualizações:<br /><br /> -   **Verificar toda vez que o aplicativo for executado**. O ClickOnce executará uma verificação de atualização sempre que o usuário abrir o aplicativo.<br />-   **Verificar a cada**: selecione um intervalo e uma unidade (horas, dias ou semanas) que deve decorrer antes de verificar se há atualizações.|  
|**Especifique a versão mínima necessária para este aplicativo**|Opcional. Especifica que uma versão específica do seu aplicativo é uma instalação obrigatória, impedindo que os usuários trabalhem com uma versão anterior.|  
|**Versão**|Necessário se a caixa de seleção **Especifique a versão mínima necessária para este aplicativo** estiver marcada. O número de versão fornecido deve estar no formato *N.N.N.N*. Somente o primeiro número de build principal é necessário. Por exemplo, para a versão 1.0 de um aplicativo, os valores válidos incluiriam `1`, `1.0`, `1.0.0` e `1.0.0.0`.|  
  
### <a name="application-reference-tab"></a>Guia Referência do Aplicativo  
 A guia **Referência do Aplicativo** contém os mesmos campos que a guia **Nome** descrita anteriormente neste tópico. A única exceção é o campo a seguir.  
  
|Elemento da interface do usuário|Descrição|  
|----------------|-----------------|  
|**Selecionar Manifesto**|Permite que você escolha o manifesto do aplicativo. Todos os outros campos nesta página serão populados quando você escolher um manifesto do aplicativo.|  
  
## <a name="see-also"></a>Consulte também

- [Segurança e implantação do ClickOnce](/visualstudio/deployment/clickonce-security-and-deployment)
- [Walkthrough: Implantando manualmente um aplicativo ClickOnce](/visualstudio/deployment/walkthrough-manually-deploying-a-clickonce-application)
- [Mage.exe (Manifest Generation and Editing Tool)](mage-exe-manifest-generation-and-editing-tool.md)
