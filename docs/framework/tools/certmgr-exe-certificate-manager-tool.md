---
title: Certmgr.exe (ferramenta Gerenciador de Certificados)
description: Explore Certmgr.exe, a ferramenta Gerenciador de certificados. Essa ferramenta gerencia certificados, listas de certificados confiáveis (CTLs) e listas de certificados revogados (CRLs).
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
helpviewer_keywords:
- certificates, managing
- CRLs
- certificate trust lists
- Certmgr.exe
- Certificate Manager tool
- CTLs
- certificate revocation lists
ms.assetid: 7e953b43-1374-4bbc-814f-53ca1b6b52bb
ms.openlocfilehash: 43ab281e6ec28ff23ea584b03fd4278c6682e33e
ms.sourcegitcommit: 87cfeb69226fef01acb17c56c86f978f4f4a13db
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/24/2020
ms.locfileid: "87167264"
---
# <a name="certmgrexe-certificate-manager-tool"></a>Certmgr.exe (ferramenta Gerenciador de Certificados)
A ferramenta Gerenciador de Certificados (Certmgr.exe) gerencia certificados, CTLs (listas de certificados confiáveis) e CRLs (listas de certificados revogados).  
  
 O Gerenciador de Certificados é instalado automaticamente com o Visual Studio. Para iniciar a ferramenta, use os [Prompts de Comando](developer-command-prompt-for-vs.md).  
  
> [!NOTE]
> A ferramenta Gerenciador de Certificados (Certmgr.exe) é um utilitário de linha de comando, e Certificados (Certmgr.msc) é um snap-in MMC (Console de Gerenciamento Microsoft). Como Certmgr.msc costuma ser encontrado no diretório do sistema do Windows, a digitação de `certmgr` na linha de comando pode carregar o snap-in do MMC de Certificados, mesmo que você tenha aberto o Prompt de Comando do Desenvolvedor para Visual Studio. Isso ocorre porque o caminho para o snap-in precede o caminho para a ferramenta Gerenciador de Certificados na variável de ambiente PATH. Se encontrar esse problema, você poderá executar comandos de Certmgr.exe especificando-se o caminho do executável.  
  
 Essa ferramenta é instalada automaticamente com o Visual Studio. Para executar a ferramenta, use o Prompt de Comando do Desenvolvedor para Visual Studio (ou o Prompt de Comando do Visual Studio no Windows 7). Para obter mais informações, consulte [Prompts de Comando](developer-command-prompt-for-vs.md).  
  
 Para obter uma visão geral dos certificados X.509, consulte [Working with Certificates](../wcf/feature-details/working-with-certificates.md) (Trabalhando com certificados).  
  
 No prompt de comando, digite o seguinte:  
  
## <a name="syntax"></a>Sintaxe  
  
```console  
      certmgr [/add | /del | /put] [options]  
[/s[/r registryLocation]] [sourceStorename]  
[/s[/r registryLocation]] [destinationStorename]  
```  
  
## <a name="parameters"></a>parâmetros  
  
|Argumento|Descrição|  
|--------------|-----------------|  
|*sourceStorename*|O repositório de certificados que contém os certificados, as CTLs ou as CRLs existentes que serão adicionados, excluídos, salvos ou exibidos. Ele pode ser um arquivo de repositório ou um repositório de sistemas.|  
|*destinationStorename*|O repositório ou o arquivo de certificados de saída.|  
  
|Opção|DESCRIÇÃO|  
|------------|-----------------|  
|**/Add**|Adiciona certificados, CTLs e CRLs a um repositório de certificados.|  
|**/All**|Adiciona todas as entradas quando usadas com **/add**. Exclui todas as entradas quando usadas com **/del**. Exibe todas as entradas quando usadas sem as opções **/Add** ou **/del** . A opção **/all** não pode ser usada com **/put**.|  
|**/c**|Adiciona certificados quando usados com **/add**. Exclui certificados quando usados com **/del**. Salva certificados quando usados com **/Put**. Exibe certificados quando usados sem as opções **/add**, **/del** ou **/put**.|  
|**/CRL**|Adiciona CRLs quando usadas com **/add**. Exclui as CRLs quando usadas com **/del**. Salva as CRLs quando usadas com **/Put**. Exibe CRLs quando usadas sem a opção **/add**, **/del** ou **/put**.|  
|**/CTL**|Adiciona CTLs quando usadas com **/add**. Exclui as CTLs quando usadas com **/del**. Salva CTLs quando usada com **/Put**. Exibe CTLs quando usadas sem as opções **/add**, **/del** ou **/put**.|  
|**/del**|Exclui certificados, CTLs e CRLs de um repositório de certificados.|  
|**/e** *encodingType*|Especifica o tipo de codificação do certificado. O padrão é `X509_ASN_ENCODING`.|  
|**/f** *dwFlags*|Especifica o sinalizador de repositório aberto. Esse é o parâmetro *dwFlags* passado para **CertOpenStore**. O valor padrão é CERT_SYSTEM_STORE_CURRENT_USER. Essa opção só será ignorada se a opção **/y** for usada.|  
|**/h**[**elp**]|Exibe sintaxe de comando e opções para a ferramenta.|  
|**/n** *nam*|Especifica o nome comum do certificado a ser adicionado, excluído ou salvo. Essa opção só pode ser usada com certificados; ela não pode ser usada com CTLs ou CRLs.|  
|**/put**|Salva um certificado X.509, uma CTL ou uma CRL de um repositório de certificados em um arquivo. O arquivo é salvo no formato X.509. É possível usar a opção **/7** com a opção **/put** para salvar o arquivo no formato PKCS #7. A opção **/put** deve ser seguida por **/c**, **/CTL** ou **/CRL**. A opção **/all** não pode ser usada com **/put**.|  
|**/r** *location*|Identifica o local do Registro do repositório do sistema. Essa opção só será ignorada se você especificar a opção **/s**. *location* deve ser um dos seguintes:<br /><br /> -   `currentUser` indica que o repositório de certificados está na chave HKEY_CURRENT_USER. Esse é o padrão.<br />-   `localMachine` indica que o repositório de certificados está na chave HKEY_LOCAL_MACHINE.|  
|**/s**|Indica que o repositório de certificados é um repositório do sistema. Se você não especificar essa opção, o repositório será considerado um **StoreFile**.|  
|**/sha1** *sha1Hash*|Especifica o hash SHA1 do certificado, da CTL ou da CRL a ser adicionado, excluído ou salvo.|  
|**/v**|Especifica o modo detalhado; exibe informações detalhadas sobre certificados, CTLs e CRLs. Essa opção não pode ser usada com as opções **/add**, **/del** ou **/put**.|  
|**/y** *provider*|Especifica o nome do provedor de repositório.|  
|**/7**|Salva o repositório de destino como um objeto PKCS #7.|  
|**/?**|Exibe sintaxe de comando e opções para a ferramenta.|  
  
## <a name="remarks"></a>Comentários  
 Certmgr.exe realiza as seguintes funções básicas:  
  
- Exibe certificados, CTLs e CRLs para o console.  
  
- Adiciona certificados, CTLs e CRLs a um repositório de certificados.  
  
- Exclui certificados, CTLs e CRLs de um repositório de certificados.  
  
- Salva um certificado X.509, uma CTL ou uma CRL de um repositório de certificados em um arquivo.  
  
 Certmgr.exe trabalha com dois tipos de repositórios de certificados: **StoreFile** e repositório do sistema. Não é necessário especificar o tipo do repositório de certificados; Certmgr.exe pode identificar o tipo de repositório e realizar as operações apropriadas.  
  
 A execução de Certmgr.exe sem especificar opções inicia o snap-in certmgr.msc, que tem uma GUI que ajuda nas tarefas de gerenciamento do certificado também disponíveis na linha de comando. A GUI fornece um assistente de importação, que copia certificados, CTLs e CRLs do disco para um repositório de certificados.  
  
 É possível encontrar os nomes de repositórios X509Certificate para os parâmetros `sourceStorename` e `destinationStorename` compilando e executando-se o código a seguir.  
  
 [!code-csharp[Tools.CertMgr#1](../../../samples/snippets/csharp/VS_Snippets_CLR/tools.certmgr/cs/storenames1.cs#1)]
 [!code-vb[Tools.CertMgr#1](../../../samples/snippets/visualbasic/VS_Snippets_CLR/tools.certmgr/vb/storenames1.vb#1)]  
  
 Para obter mais informações sobre certificados, consulte [Working with Certificates](../wcf/feature-details/working-with-certificates.md) (Trabalhando com certificados).  
  
## <a name="examples"></a>Exemplos  
 O comando a seguir exibe um repositório de sistema padrão chamado `my` com saída detalhada.  
  
```console  
certmgr /v /s my  
```  
  
 O comando a seguir adiciona todos os certificados em um arquivo chamado `myFile.ext` a um novo arquivo chamado `newFile.ext`.  
  
```console  
certmgr /add /all /c myFile.ext newFile.ext  
```  
  
 O comando a seguir adiciona o certificado em um arquivo chamado `testcert.cer` no repositório do sistema `my`.  
  
```console  
certmgr /add /c testcert.cer /s my  
```  
  
 O comando a seguir adiciona o certificado em um arquivo chamado `TrustedCert.cer` ao repositório de certificados raiz.  
  
```console  
certmgr /c /add TrustedCert.cer /s root  
```  
  
 O comando a seguir salva um certificado com o nome comum `myCert` no repositório do sistema `my` em um arquivo chamado `newCert.cer`.  
  
```console  
certmgr /add /c /n myCert /s my newCert.cer  
```  
  
 O comando a seguir exclui todas as CTLs no repositório do sistema `my` e o repositório resultante em um arquivo chamado `newStore.str`.  
  
```console  
certmgr /del /all /ctl /s my newStore.str  
```  
  
 O comando a seguir salva um certificado no repositório do sistema `my` no arquivo `newFile`. Você deverá inserir o número do certificado em `my` a ser colocado em `newFile`.  
  
```console  
certmgr /put /c /s my newFile  
```  
  
## <a name="see-also"></a>Confira também

- [Ferramentas](index.md)
- [Makecert.exe (Ferramenta de Criação de Certificado)](/windows/desktop/SecCrypto/makecert)
- [Prompts de comando](developer-command-prompt-for-vs.md)
