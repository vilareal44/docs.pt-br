---
title: Guia de implantação do .NET Framework para administradores
description: Leia o guia de implantação do .NET para administradores. Use essas informações para implantar a versão 4,5 do .NET e suas dependências do sistema em uma rede.
ms.date: 04/10/2018
helpviewer_keywords:
- administrator's guide, deploying .NET Framework
- deployment [.NET Framework], administrator's guide
ms.assetid: bee14036-0436-44e8-89f5-4bc61317977a
ms.openlocfilehash: d58eac4f21e4f1069ac392aacb4e9818831e914c
ms.sourcegitcommit: e02d17b2cf9c1258dadda4810a5e6072a0089aee
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/01/2020
ms.locfileid: "85622647"
---
# <a name="net-framework-deployment-guide-for-administrators"></a>Guia de implantação do .NET Framework para administradores

Este artigo passo a passo descreve como um administrador do sistema pode implantar o .NET Framework 4,5 e suas dependências do sistema em uma rede usando o Microsoft Endpoint Configuration Manager. Este artigo pressupõe que todos os computadores clientes de destino atendem aos requisitos mínimos do .NET Framework. Para obter uma lista dos requisitos de hardware e software para instalar o .NET Framework 4.5, consulte [Requisitos do sistema](../get-started/system-requirements.md).

> [!NOTE]
> O software referenciado neste documento, incluindo, sem limitação, o .NET Framework 4,5, Configuration Manager e Active Directory, estão sujeitos aos termos e condições da licença. Essas instruções assumem que esses termos de licença e condições foram examinados e aceitos pelos licenciados apropriados do software. Essas instruções não renunciam nenhum dos termos e condições dos contratos de tal licença.
>
> Para obter informações sobre o suporte para o .NET Framework, consulte [.NET Framework a política de suporte oficial](https://dotnet.microsoft.com/platform/support/policy/dotnet-framework) no site do suporte da Microsoft.

Este tópico contém as seguintes seções:

- [O processo de implantação](#the_deployment_process)
- [Implantando o .NET Framework](#deploying_in_a_test_environment)
- [Criar uma coleção](#creating_a_collection)
- [Crie um pacote e programa](#creating_a_package)
- [Selecionar um ponto de distribuição](#select_dist_point)
- [Implantar o pacote](#deploying_package)
- [Recursos](#resources)
- [Solução de problemas](#troubleshooting)

<a name="the_deployment_process"></a>

## <a name="the-deployment-process"></a>O processo de implantação

Quando você tem a infraestrutura de suporte em vigor, usa Configuration Manager para implantar o pacote redistribuível .NET Framework em computadores na rede. Estabelecer uma infraestrutura envolve criar e definir cinco áreas principais: coleções, um pacote e programa para o software, pontos de distribuição e implantações.

- **Coleções** são grupos de recursos do Configuration Manager, como usuários, grupos de usuários ou computadores, nos quais o .NET Framework é implantado. Para obter mais informações, consulte [introdução às coleções em Configuration Manager](https://docs.microsoft.com/configmgr/core/clients/manage/collections/introduction-to-collections) na biblioteca de documentação do Configuration Manager.

- **Pacotes e programas** geralmente representam aplicativos de software a serem instalados em um computador cliente, mas também podem conter arquivos individuais, atualizações ou até mesmo comandos individuais. Para obter mais informações, consulte [pacotes e programas em Configuration Manager](https://docs.microsoft.com/configmgr/apps/deploy-use/packages-and-programs) na biblioteca de documentação do Configuration Manager.

- **Pontos de distribuição** são funções do sistema de sites do Configuration Manager que armazenam os arquivos necessários para que o software seja executado em computadores cliente. Quando o cliente do Configuration Manager recebe e processa uma implantação de software, ele contata um ponto de distribuição para baixar o conteúdo associado ao software e iniciar o processo de instalação. Para obter mais informações, veja [Conceitos fundamentais para gerenciamento de conteúdo no Configuration Manager](https://docs.microsoft.com/configmgr/core/plan-design/hierarchy/fundamental-concepts-for-content-management) na biblioteca de documentação do Configuration Manager.

- **Implantações** instruem os membros aplicáveis da coleção de destino especificada a instalarem o pacote de software.

> [!IMPORTANT]
> Os procedimentos neste tópico contém configurações comuns para criar e implantar um pacote e um programa e podem não abranger todas as configurações possíveis. Para obter outras opções de implantação do Configuration Manager, consulte a [Biblioteca de documentação do Configuration Manager](https://docs.microsoft.com/previous-versions/system-center/system-center-2012-R2/gg682041%28v=technet.10%29).

<a name="deploying_in_a_test_environment"></a>

## <a name="deploying-the-net-framework"></a>Implantando o .NET Framework

Você pode usar Configuration Manager para implantar uma instalação silenciosa do .NET Framework 4,5, em que os usuários não interagem com o processo de instalação. Siga estas etapas:

1. [Crie uma coleção](#creating_a_collection).

2. [Crie um pacote e um programa para o .NET Framework redistribuível](#creating_a_package).

3. [Selecione um ponto de distribuição](#select_dist_point).

4. [Implante o pacote](#deploying_package).

<a name="creating_a_collection"></a>

### <a name="create-a-collection"></a>Criar uma coleção

Nesta etapa, você selecionará os computadores nos quais pacote e o programa serão implantados e os agrupará em uma coleção de dispositivos. Para criar uma coleção no Configuration Manager, você pode usar regras de associação diretas (onde você especifica manualmente os membros da coleção) ou regras de consulta (onde o Configuration Manager determina os membros da coleção com base em critérios especificados). Para obter mais informações sobre regras de associação, incluindo consultas e regras diretas, consulte [introdução às coleções em Configuration Manager](https://docs.microsoft.com/configmgr/core/clients/manage/collections/introduction-to-collections) na biblioteca de documentação do Configuration Manager.

Para criar uma coleção:

1. No console do Configuration Manager, escolha **Ativos e Conformidade**.

2. No workspace **Ativos e Conformidade**, escolha **Coleções de Dispositivos**.

3. Na guia **Início** do grupo **Criar**, escolha **Criar Coleção de Dispositivos**.

4. Na página **Geral** do **Assistente de Criação de Coleção de Dispositivos**, insira um nome para a coleção.

5. Escolha **Procurar** para especificar uma coleção de restrição.

6. Na página **Regras de Associação**, escolha **Adicionar Regra** e escolha **Regra Direta** para abrir o **Assistente de Criação de Regra de Associação Direta**. Escolha **Próxima**.

7. Na página **Pesquisar Recursos**, na lista **Classe de recurso**, escolha **Recurso do Sistema**. Na lista **Nome do atributo**, escolha **Nome**. No campo **Valor**, insira `%` e escolha **Avançar**.

8. Na página **Selecionar Recursos**, marque a caixa de seleção para cada computador no qual você deseja implantar o .NET Framework. Escolha **Avançar** e conclua o assistente.

9. Na página **Regras de Associação** do **Assistente de Criação de Coleção de Dispositivos**, escolha **Avançar** e conclua o assistente.

<a name="creating_a_package"></a>

### <a name="create-a-package-and-program-for-the-net-framework-redistributable-package"></a>Crie um pacote e programa para o pacote .NET Framework redistribuível

As etapas a seguir criam, manualmente, um pacote para o .NET Framework redistribuível. O pacote conterá os parâmetros especificados para instalar o .NET Framework e o local de onde o pacote será distribuído para os computadores de destino.

Para criar um pacote:

1. No console do Configuration Manager, escolha **Biblioteca de Software**.

2. No workspace **Biblioteca de Software**, expanda **Gerenciamento de Aplicativos** e escolha **Pacotes**.

3. Na guia **Início**, no grupo **Criar**, escolha **Criar Pacote**.

4. Na página **Pacote** do **Assistente para Criar Pacote e Programa**, insira as seguintes informações:

    - Nome: `.NET Framework 4.5`

    - Fabricante: `Microsoft`

    - Idioma. `English (US)`

5. Escolha **Este pacote contém arquivos de origem** e **Procurar** para selecionar a pasta de rede ou local que contém os arquivos de instalação do .NET Framework. Após selecionar a pasta, escolha **OK** e **Avançar**.

6. Na página **Tipo de Programa** do assistente, escolha **Programa Padrão** e **Avançar**.

7. Na página **Programa** do **Assistente para Criar Pacote e Programa**, insira as seguintes informações:

    1. **Nome:**`.NET Framework 4.5`

    2. **Linha de comando:** `dotNetFx45_Full_x86_x64.exe /q /norestart /ChainingPackage ADMINDEPLOYMENT` (as opções de linha de comando são descritas na tabela após estas etapas)

    3. **Executar:** escolha **Oculto**.

    4. **O programa pode ser executado:** escolha a opção que especifica que o programa pode ser executado independentemente de um usuário estar conectado.

8. Na página **Requisitos**, escolha **Avançar** para aceitar os valores padrão e conclua o assistente.

A tabela a seguir descreve as opções de linha de comando especificadas na etapa 7.

|Opção|Descrição|
|------------|-----------------|
|**/q**|Define o modo silencioso. Nenhuma entrada do usuário é necessária e nenhuma saída é mostrada.|
|**/norestart**|Impede que o programa de instalação reinicialize automaticamente. Se você usar essa opção, o Configuration Manager deverá processar a reinicialização do computador.|
|**/chainingpackage** *PackageName*|Especifica o nome do pacote que está fazendo o encadeamento. Essa informação é relatada com outras informações da sessão de instalação para aqueles que se inscreveram no Programa de Aperfeiçoamento da Experiência do Usuário da Microsoft (CEIP). Se o nome do pacote contiver espaços, use aspas duplas como delimitadores: **/chainingpackage "Chaining Product"**.|

Estas etapas criam um pacote chamado .NET Framework 4.5. O programa implanta uma instalação silenciosa do .NET Framework 4.5. Em uma instalação silenciosa, os usuários não interagem com o processo de instalação e o aplicativo de encadeamento precisa capturar o código de retorno e manipular a reinicialização; consulte [Obtendo informações de progresso de um pacote de instalação](https://docs.microsoft.com/previous-versions/cc825975(v=vs.100)).

<a name="select_dist_point"></a>

### <a name="select-a-distribution-point"></a>Selecione um ponto de distribuição

Para distribuir o pacote e o programa para computadores clientes de um servidor, primeiro designe um sistema de site como um ponto de distribuição e distribua o pacote para o ponto de distribuição.

Use as etapas a seguir para selecionar um ponto de distribuição para o pacote .NET Framework 4.5 criado na seção anterior:

1. No console do Configuration Manager, escolha **Biblioteca de Software**.

2. No workspace **Biblioteca de Software**, expanda **Gerenciamento de Aplicativos** e escolha **Pacotes**.

3. Na lista de pacotes, selecione o pacote **.NET Framework 4.5** que você criou na seção anterior.

4. Na guia **Início** do grupo **Implantação**, escolha **Distribuir Conteúdo**.

5. Na guia **Geral** do **Assistente para Distribuir Conteúdo**, escolha **Avançar**.

6. Na página **Destino de Conteúdo** do assistente, escolha **Adicionar** e **Ponto de Distribuição**.

7. Na caixa de diálogo **Adicionar Pontos de Distribuição**, selecione os pontos de distribuição que hospedarão o pacote e o programa e escolha **OK**.

8. Conclua o assistente.

O pacote agora conterá todas as informações que você precisa para implantar silenciosamente o .NET Framework 4.5. Antes de implantar o pacote e o programa, verifique se ele foi instalado no ponto de distribuição; consulte a seção "monitoramento de status de conteúdo" do [Monitor de conteúdo que você distribui com o Configuration Manager](https://docs.microsoft.com/configmgr/core/servers/deploy/configure/monitor-content-you-have-distributed) na biblioteca de documentação Configuration Manager.

<a name="deploying_package"></a>

### <a name="deploy-the-package"></a>Implantar o pacote

Para implantar o pacote e o programa .NET Framework 4.5:

1. No console do Configuration Manager, escolha **Biblioteca de Software**.

2. No workspace **Biblioteca de Software**, expanda **Gerenciamento de Aplicativos** e escolha **Pacotes**.

3. Na lista de pacotes, selecione o pacote criado e denominado **.NET Framework 4.5**.

4. Na guia **Início**, no grupo **Implantação**, escolha **Implantar**.

5. Na página **Geral** do **Assistente de Implantação de Software**, escolha **Procurar** e selecione a coleção anteriormente criada. Escolha **Próxima**.

6. Na página **Conteúdo** do assistente, verifique se o ponto do qual você deseja distribuir o software é exibido e escolha **Avançar**.

7. Na página **Configurações de Implantação** do assistente, confirme se a **Ação** está definida como **Instalar** e a **Finalidade** como **Obrigatório**. Isso garantirá que o pacote de software seja uma instalação obrigatória nos computadores de destino. Escolha **Próxima**.

8. Na página **Agendamento** do assistente, especifique quando você deseja que o .NET Framework seja instalado. Você pode escolher **Nova** para atribuir uma hora de instalação ou instruir o software a ser instalado quando o usuário fizer logon, logoff ou assim que possível. Escolha **Próxima**.

9. Na página **Experiência do Usuário** do assistente, use os valores padrão e escolha **Avançar**.

    > [!WARNING]
    > O ambiente de produção pode ter políticas que exijam diferentes seleções de agendamento de implantação. Para obter informações sobre essas opções, confira [Propriedades de nome de anúncio: guia Agendamento](https://docs.microsoft.com/previous-versions/system-center/configuration-manager-2007/bb694016%28v=technet.10%29).

10. Na página **Pontos de Distribuição** do assistente, use os valores padrão e escolha **Avançar**.

11. Conclua o assistente. Você pode monitorar o progresso da implantação no nó **Implantações** do workspace **Monitoramento**.

O pacote agora será implantado na coleção de destino e a instalação silenciosa do .NET Framework 4.5 será iniciada. Para obter informações sobre códigos de erro de instalação do .NET Framework 4.5, consulte a seção [Códigos de retorno](#return_codes) posteriormente neste tópico.

<a name="resources"></a>

## <a name="resources"></a>Recursos

Para obter mais informações sobre a infraestrutura para testar a implantação do pacote redistribuível do .NET Framework 4.5, veja os recursos a seguir.

**Active Directory, DNS, DHCP:**

- [Active Directory Domain Services](/windows/desktop/ad/active-directory-domain-services)

- [Sistema de nome de domínio (DNS)](/windows-server/networking/dns/dns-top)

- [Protocolo DHCP](/windows-server/networking/technologies/dhcp/dhcp-top)

**SQL Server 2008:**

- [Instalando o SQL Server 2008 (vídeo do SQL Server)](https://docs.microsoft.com/previous-versions/sql/sql-server-2008/dd299415(v=sql.100))

- [Visão geral de segurança do SQL Server 2008 para administradores de banco de dados](https://download.microsoft.com/download/a/c/d/acd8e043-d69b-4f09-bc9e-4168b65aaa71/SQL2008SecurityOverviewforAdmins.docx)

**System Center 2012 Configuration Manager (ponto de gerenciamento, ponto de distribuição):**

- [Administração de site do System Center 2012 Configuration Manager](https://docs.microsoft.com/previous-versions/system-center/system-center-2012-R2/gg681983%28v=technet.10%29)

- [Planejando e implantando site único do Configuration Manager](https://docs.microsoft.com/previous-versions/system-center/configuration-manager-2007/bb680961%28v=technet.10%29)

**Cliente do System Center 2012 Configuration Manager para computadores Windows:**

- [Implantação de clientes do System Center 2012 Configuration Manager](https://docs.microsoft.com/previous-versions/system-center/system-center-2012-R2/gg699391%28v=technet.10%29)

<a name="troubleshooting"></a>

## <a name="troubleshooting"></a>Solução de problemas

### <a name="log-file-locations"></a>Localizações dos arquivos de log

Os seguintes arquivos de log são gerados durante a configuração do .NET Framework:

- %temp%\Microsoft .NET Framework *version* \* . txt
- %temp%\Microsoft .NET Framework *versão* \* . html

em que *versão* é a versão do .NET Framework que você está instalando, como 4.5 ou 4.7.2.

Também é possível especificar o diretório no qual os arquivos de log são gravados usando a opção de linha de comando `/log` no comando de instalação do .NET Framework. Para obter mais informações, consulte [Guia de implantação do .NET Framework para desenvolvedores](deployment-guide-for-developers.md#command-line-options).

Você pode usar a [ferramenta de coleta de logs](https://www.microsoft.com/download/details.aspx?id=12493) para coletar os arquivos de log do .NET Framework e criar um arquivo de gabinete compactado (.cab) que reduz o tamanho dos arquivos.

<a name="return_codes"></a>

### <a name="return-codes"></a>Códigos de retorno

A tabela a seguir lista os códigos de retorno mais comuns do programa de instalação redistribuível do .NET Framework 4.5. Os códigos de retorno são os mesmos para todas as versões do instalador.

Para obter links para informações detalhadas, consulte a próxima seção [Códigos de erro de download](#additional_error_codes).

|Código de retorno|Descrição|
|-----------------|-----------------|
|0|A instalação foi concluída com êxito.|
|1602|O usuário cancelou a instalação.|
|1603|Um erro fatal ocorreu durante a instalação.|
|1641|É necessário reiniciar para concluir a instalação. Esta mensagem indica êxito.|
|3010|É necessário reiniciar para concluir a instalação. Esta mensagem indica êxito.|
|5100|O computador do usuário não atende aos requisitos do sistema.|

<a name="additional_error_codes"></a>

### <a name="download-error-codes"></a>Códigos de erro de download

- [Códigos de erro do BITS (Serviço de Transferência Inteligente em Segundo Plano)](/windows/desktop/Bits/bits-return-values)

- [Códigos de erro do moniker de URL](https://docs.microsoft.com/previous-versions/windows/internet-explorer/ie-developer/platform-apis/ms775145%28v=vs.85%29)

- [Códigos de erro WinHttp](/windows/desktop/WinHttp/error-messages)

Outros códigos de erro:

- [Códigos de erro do Windows Installer](/windows/desktop/msi/error-codes)

- [Códigos de resultado do Windows Update Agent](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/cc720442(v=ws.10))

## <a name="see-also"></a>Consulte também

- [Guia de implantação para desenvolvedores](deployment-guide-for-developers.md)
- [Requisitos do sistema](../get-started/system-requirements.md)
