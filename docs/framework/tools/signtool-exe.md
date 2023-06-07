---
title: SignTool.exe (Ferramenta de Assinatura)
description: Saiba mais sobre SignTool.exe, a ferramenta de assinatura. Essa ferramenta de linha de comando assina digitalmente os arquivos, verifica as assinaturas em arquivos e aplica os carimbos de data/hora aos arquivos.
ms.date: 03/30/2017
helpviewer_keywords:
- Sign tool
- SignTool.exe
ms.assetid: 0c25ff6c-bff3-422e-b017-146a3ee86cb9
ms.openlocfilehash: f1254f345a8b3bb796217442cbad36d2e942b403
ms.sourcegitcommit: b4f8849c47c1a7145eb26ce68bc9f9976e0dbec3
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/03/2020
ms.locfileid: "87517198"
---
# <a name="signtoolexe-sign-tool"></a>SignTool.exe (Ferramenta de Assinatura)
A Ferramenta de Assinatura é uma ferramenta de linha de comando que assina digitalmente arquivos, verifica assinaturas em arquivos e em arquivos de carimbo de data/hora.  
  
 Essa ferramenta é instalada automaticamente com o Visual Studio. Para executar a ferramenta, use o Prompt de Comando do Desenvolvedor para Visual Studio (ou o Prompt de Comando do Visual Studio no Windows 7). Para obter mais informações, consulte [Prompts de Comando](developer-command-prompt-for-vs.md).  
  
 No prompt de comando, digite o seguinte:  
  
## <a name="syntax"></a>Sintaxe  
  
```console  
signtool [command] [options] [file_name | ...]  
```  
  
## <a name="parameters"></a>Parâmetros  
  
|Argumento|Descrição|  
|--------------|-----------------|  
|`command`|Um dos quatro comandos (`catdb`, `sign`, `Timestamp` ou `Verify`) que especifica uma operação a ser realizada em um arquivo. Para obter uma descrição de cada comando, consulte a próxima tabela.|  
|`options`|Uma opção que modifica um comando. Além das opções globais `/q` e `/v`, cada comando dá suporte a um conjunto exclusivo de opções.|  
|`file_name`|O caminho para um arquivo a ser assinado.|  
  
 Os seguintes comandos são compatíveis com a Ferramenta de Assinatura. Cada comando é usado com conjuntos distintos de opções, listadas em suas respectivas seções.  
  
|Comando|Descrição|  
|-------------|-----------------|  
|`catdb`|Adiciona ou remove um arquivo de catálogo de um banco de dados do catálogo. Os bancos de dados do catálogo são usados na pesquisa automática de arquivos de catálogo e são identificados por uma GUID. Para obter uma lista de opções com suporte pelo comando `catdb`, consulte [Opções do Comando catdb](signtool-exe.md#catdb).|  
|`sign`|Assina arquivos digitalmente. As assinaturas digitais protegem os arquivos contra violação e permitem aos usuários verificar o signatário com base em um certificado de assinatura. Para obter uma lista de opções com suporte pelo comando `sign`, consulte [Opções do Comando sign](signtool-exe.md#sign).|  
|`Timestamp`|Arquivos de carimbo de data/hora. Para obter uma lista das opções com suporte pelo comando `TimeStamp`, consulte [Opções de Comando TimeStamp](signtool-exe.md#TimeStamp).|  
|`Verify`|Verifica a assinatura digital dos arquivos determinando se o certificado de assinatura foi emitido por uma autoridade confiável, se o certificado de assinatura foi revogado e, como opção, se o certificado de assinatura é válido para uma política específica. Para obter uma lista das opções com suporte pelo comando `Verify`, consulte [Opções do Comando Verify](signtool-exe.md#Verify).|  
  
 As seguintes opções aplicam-se a todos os comandos da Ferramenta de Assinatura.  
  
|Opção global|Descrição|  
|-------------------|-----------------|  
|**/q**|Não exibirá nenhuma saída se o comando for executado com êxito e exibirá saída mínima se o comando falhar.|  
|**/v**|Exibe saída detalhada, independentemente de o comando ser executado com êxito ou falhar, além de exibir mensagens de aviso.|  
|**/debug**|Exibe informações de depuração.|  
  
<a name="catdb"></a>
## <a name="catdb-command-options"></a>Opções do Comando catdb  
 A tabela a seguir lista as opções que podem ser usadas com o comando `catdb`.  
  
|Opção Catdb|Descrição|  
|------------------|-----------------|  
|`/d`|Especifica se o banco de dados do catálogo padrão está atualizado. Se as opções `/d` ou `/g` não forem usadas, a Ferramenta de Assinatura atualizará o banco de dados do driver e do componente do sistema.|  
|`/g` *GUID*|Especifica se o banco de dados do catálogo identificado pelo identificador global exclusivo *GUID* está atualizado.|  
|`/r`|Remove os catálogos especificados do banco de dados do catálogo. Se essa opção não for especificada, a Ferramenta de Assinatura adicionará os catálogos especificados ao banco de dados do catálogo.|  
|`/u`|Especifica se um nome exclusivo é gerado automaticamente para os arquivos de catálogo adicionados. Se necessário, os arquivos do catálogo são renomeados para evitar conflitos de nome com os arquivos de catálogo existentes. Se essa opção não for especificada, a Ferramenta de Assinatura substituirá qualquer catálogo existente que tenha o mesmo nome do catálogo que está sendo adicionado.|  
  
<a name="sign"></a>
## <a name="sign-command-options"></a>Opções do Comando sign  
 A tabela a seguir lista as opções que podem ser usadas com o comando `sign`.  
  
|Opções do comando de entrada|Descrição|  
|-------------------------|-----------------|  
|`/a`|Seleciona automaticamente o melhor certificado de assinatura. A Ferramenta de Assinatura encontrará todos os certificados válidos que atendem às condições especificadas e selecionará aquele válido durante um tempo mais longo. Se essa opção não estiver presente, a Ferramenta de Assinatura deverá localizar apenas um certificado de assinatura válido.|  
|`/ac`  *Grupo*|Adiciona um certificado adicional de *file* ao bloco de assinatura.|  
|`/as`|Acrescenta esta assinatura. Se nenhuma assinatura primária estiver presente, essa assinatura será definida como a assinatura principal.|  
|`/c`  *CertTemplateName*|Especifica o Nome do Modelo de Certificado (uma extensão da Microsoft) para o certificado de assinatura.|  
|`/csp`  *CSPName*|Especifica o provedor de serviços de criptografia (CSP) que contém o contêiner de chave privada.|  
|`/d`  *Crescente*|Especifica uma descrição do conteúdo assinado.|  
|`/du`  *URL*|Especifica uma URL (Uniform Resource Locator) para obter a descrição expandida do conteúdo assinado.|  
|`/f`  *SignCertFile*|Especifica o certificado de assinatura em um arquivo. Se o arquivo estiver no formato PFX (Personal Information Exchange) e protegido por senha, use a opção `/p` para especificar a senha. Se o arquivo não contiver chaves privadas, use as opções `/csp` e `/kc` para especificar o CSP e o nome do contêiner de chave privada.|  
|`/fd`|Especifica o algoritmo de resumo do arquivo a ser usado na criação de assinaturas de arquivo. O padrão é SHA1.|  
|`/i`  *IssuerName*|Especifica o nome do emissor de certificado de assinatura. Esse valor pode ser uma subcadeia de caracteres do nome do emissor inteiro.|  
|`/kc`  *PrivKeyContainerName*|Especifica o nome do contêiner de chave privada.|  
|`/n`  *SubjectName*|Especifica o nome do assunto do certificado de assinatura. Esse valor pode ser uma subcadeia de caracteres do nome da entidade inteiro.|  
|`/nph`|Se compatível, suprime hashes de página para arquivos executáveis. O padrão é determinado pela variável de ambiente SIGNTOOL_PAGE_HASHES e pela versão de wintrust.dll. Essa opção é ignorada para arquivos não PE.|  
|`/p`  *Senha*|Especifica a senha a ser usada durante a abertura de um arquivo PFX. (Use a opção `/f` para especificar um arquivo PFX).|  
|`/p7` *Caminho*|Especifica se um arquivo PKCS (Public Key Cryptography Standards) #7 é produzido para cada arquivo de conteúdo especificado. Arquivos de #7 PKCS são nomeados *path* \\ *filename*. P7.|  
|`/p7ce` *Valor*|Especifica opções para o conteúdo de PKCS #7 assinado. Defina *Value* como “Embedded” para inserir o conteúdo assinado no arquivo PKCS #7 ou como “DetachedSignedData” para produzir a parte de dados assinada de um arquivo PKCS #7 desanexado. Se a opção `/p7ce` não for usada, o conteúdo assinado será inserido por padrão.|  
|`/p7co` *\<OID>*|Especifica o OID (identificador de objeto) que identifica o conteúdo assinado de PKCS #7.|  
|`/ph`|Se compatível, gera hashes de página para arquivos executáveis.|  
|`/r`  *RootSubjectName*|Especifica o nome do assunto do certificado raiz em que o certificado de assinatura deve ser encadeado. Esse valor pode ser uma subcadeia de caracteres do nome de entidade inteiro do certificado raiz.|  
|`/s`  *StoreName*|Especifica o armazenamento a ser aberto durante a pesquisa pelo certificado. Se essa opção não for especificada, o armazenamento `My` será aberto.|  
|`/sha1`  *Tralha*|Especifica o hash SHA1 do certificado de assinatura. O hash SHA1 costuma ser especificado quando vários certificados atendem aos critérios especificados pelas opções restantes.|  
|`/sm`|Especifica se um armazenamento do computador, em vez de um armazenamento de usuário, é usado.|  
|`/t`  *URL*|Especifica a URL do servidor de carimbo de data/hora. Se essa opção (ou `/tr`) não estiver presente, o arquivo assinado não receberá carimbo de data/hora. Um aviso será gerado se o carimbo de data/hora falhar. Essa opção não pode ser usada com a opção `/tr`.|  
|`/td`  *ALG*|Usado com a opção `/tr` para solicitar um algoritmo de resumo usado pelo servidor do carimbo de data/hora RFC 3161.|  
|`/tr`  *URL*|Especifica a URL do servidor do carimbo de data/hora RFC 3161. Se essa opção (ou `/t`) não estiver presente, o arquivo assinado não receberá carimbo de data/hora. Um aviso será gerado se o carimbo de data/hora falhar. Essa opção não pode ser usada com a opção `/t`.|  
|`/u`  *Uso*|Especifica o EKU (uso avançado de chave) que deve estar presente no certificado de assinatura. O valor de uso pode ser especificado por OID ou por cadeia de caracteres. O uso padrão é "Assinatura de Código" (1.3.6.1.5.5.7.3.3).|  
|`/uw`|Especifica o uso da "Verificação do Componente do Sistema Windows" (1.3.6.1.4.1.311.10.3.6).|  
  
 Para obter exemplos de uso, consulte [Using SignTool to Sign a File](/windows/desktop/SecCrypto/using-signtool-to-sign-a-file) (Usando a SignTool para assinar um arquivo).  
  
<a name="TimeStamp"></a>
## <a name="timestamp-command-options"></a>Opções de Comando TimeStamp  
 A tabela a seguir lista as opções que podem ser usadas com o comando `TimeStamp`.  
  
|Opção do carimbo de data/hora|Descrição|  
|----------------------|-----------------|  
|`/p7`|Arquivos PKCS #7 do carimbo de data/hora.|  
|`/t`  *URL*|Especifica a URL do servidor de carimbo de data/hora. O arquivo com carimbo de data/hora assinado anteriormente. A opção `/t` ou `/tr` é obrigatória.|  
|`/td`  *ALG*|Solicita um algoritmo de resumo usado pelo servidor do carimbo de data/hora RFC 3161. `/td` é usado com a opção `/tr`.|  
|`/tp`*índice* do|Marca com o carimbo de data/hora a assinatura em *index*.|  
|`/tr`  *URL*|Especifica a URL do servidor do carimbo de data/hora RFC 3161. O arquivo com carimbo de data/hora assinado anteriormente. A opção `/tr` ou `/t` é obrigatória.|  
  
 Para obter um exemplo de uso, consulte [Adding Time Stamps to Previously Signed Files](/windows/desktop/SecCrypto/adding-time-stamps-to-previously-signed-files) (Adicionar carimbos de data/hora aos arquivos assinados anteriormente).  
  
<a name="Verify"></a>
## <a name="verify-command-options"></a>Verificar Opções de Comando  
  
|Opção Verificar|Descrição|  
|-------------------|-----------------|  
|`/a`|Especifica que todos os métodos podem ser usados na verificação do arquivo. Primeiro, os bancos de dados do catálogo são pesquisados para determinar se o arquivo está assinado em um catálogo. Se o arquivo não estiver assinado em nenhum catálogo, a Ferramenta de Assinatura tentará verificar a assinatura inserida no arquivo. Essa opção é recomendada durante a verificação de arquivos que podem estar ou não assinados em um catálogo. Entre os exemplos desses arquivos estão arquivos ou drivers do Windows.|  
|`/ad`|Encontra o catálogo usando o banco de dados do catálogo padrão.|  
|`/ag` *CatDBGUID*|Encontra o catálogo no banco de dados de catálogos identificado pelo *CatDBGUID*.|  
|`/all`|Verifica todas as assinaturas em um arquivo que inclui várias assinaturas.|  
|`/as`|Encontra o catálogo usando o banco de dados do catálogo do componente de sistema (driver).|  
|`/c` *CatFile*|Especifica o arquivo de catálogo por nome.|  
|`/d`|Especifica se a Ferramenta de Assinatura deve imprimir a descrição e a URL da descrição.|  
|`/ds`  *Índice*|Verifica a assinatura em uma posição especificada.|  
|`/hash` (`SHA1`&#124;`SHA256`)|Especifica um algoritmo de hash opcional a ser usado durante a procura de um arquivo em um catálogo.|  
|`/kp`|Especifica se a verificação deve ser realizada com a política de assinatura do driver do modo kernel.|  
|`/ms`|Usa várias semânticas de verificação. Esse é o comportamento padrão de uma chamada [WinVerifyTrust](/windows/desktop/api/wintrust/nf-wintrust-winverifytrust) no Windows 8 e versões posteriores.|  
|`/o` *Versão*|Verifica o arquivo pela versão do sistema operacional. *Version* tem o seguinte formato: *PlatformID*:*VerMajor*.*VerMinor*.*BuildNumber*. *PlatformID* representa o valor subjacente de um membro de enumeração <xref:System.PlatformID>. **Importante:** o uso da opção `/o` é recomendado. Se `/o` não for especificado, SignTool.exe poderá retornar resultados inesperados. Por exemplo, se você não incluir a opção `/o`, os catálogos do sistema validados corretamente em um sistema operacional anterior poderá não ser validado corretamente em um sistema operacional mais novo.|  
|`/p7`|Verifica arquivos PKCS #7. Nenhuma política existente é usada na validação de PKCS #7. A assinatura é verificada e uma cadeia é compilada para o certificado de assinatura.|  
|`/pa`|Especifica se a Política de Verificação de Authenticode Padrão deve ser usada. Se a opção `/pa` não for especificada, a Ferramenta de Assinatura usará a Política de Verificação de Driver do Windows. Essa opção não pode ser usada com as opções `catdb`.|  
|`/pg` *PolicyGUID*|Especifica uma política de verificação por GUID. O *PolicyGUID* corresponde à ActionID da política de verificação. Essa opção não pode ser usada com as opções `catdb`.|  
|`/ph`|Especifica se a Ferramenta de Assinatura deve imprimir e verificar valores de hash da página.|  
|`/r` *RootSubjectName*|Especifica o nome do assunto do certificado raiz em que o certificado de assinatura deve ser encadeado. Esse valor pode ser uma subcadeia de caracteres do nome da entidade inteiro do certificado raiz.|  
|`/tw`|Especifica se um aviso deverá ser gerado se a assinatura não receber carimbo de data/hora.|  
  
 Para obter exemplos de uso, consulte [Using SignTool to Verify a File Signature](/windows/desktop/SecCrypto/using-signtool-to-verify-a-file-signature) (Usando SignTool para verificar a assinatura de um arquivo).  
  
## <a name="return-value"></a>Valor retornado  
 A Ferramenta de Assinatura retorna um dos códigos de saída a seguir quando é encerrada.  
  
|Código de saída|Descrição|  
|---------------|-----------------|  
|0|A execução foi bem-sucedida.|  
|1|A execução falhou.|  
|2|A execução foi concluída com avisos.|  
  
## <a name="examples"></a>Exemplos  
 O comando a seguir adiciona o arquivo de catálogo MyCatalogFileName.cat aos bancos de dados do componente e de driver do sistema. A opção `/u` gera um nome exclusivo, se necessário, para evitar a substituição de um arquivo de catálogo existente chamado `MyCatalogFileName.cat`.  
  
```console  
signtool catdb /v /u MyCatalogFileName.cat  
```  
  
 O comando a seguir assina um arquivo automaticamente usando-se o melhor certificado.  
  
```console  
signtool sign /a MyFile.exe  
```  
  
 O comando a seguir assina digitalmente um arquivo usando um certificado armazenado em um arquivo PFX protegido por senha.  
  
```console  
signtool sign /f MyCert.pfx /p MyPassword MyFile.exe  
```  
  
 O comando a seguir assina digitalmente e coloca carimbos de data/hora em um arquivo. O certificado usado para assinar o arquivo é armazenado em um arquivo PFX.  
  
```console  
signtool sign /f MyCert.pfx /t http://timestamp.digicert.com MyFile.exe  
```  
  
 O comando a seguir assina um arquivo usando um certificado localizado no armazenamento `My` que tem um nome de entidade de `My Company Certificate`.  
  
```console  
signtool sign /n "My Company Certificate" MyFile.exe  
```  
  
 O comando a seguir assina um controle ActiveX e fornece informações exibidas pelo Internet Explorer quando o usuário deve instalar o controle.  
  
```console  
Signtool sign /f MyCert.pfx /d: "MyControl" /du http://www.example.com/MyControl/info.html MyControl.exe  
```  
  
 O comando a seguir marca um arquivo com carimbos de data/hora já assinado digitalmente.  
  
```console  
signtool timestamp /t http://timestamp.digicert.com MyFile.exe  
```  
  
 O comando a seguir verifica se um arquivo foi assinado.  
  
```console  
signtool verify MyFile.exe  
```  
  
 O comando a seguir verifica um arquivo de sistema que pode ser assinado em um catálogo.  
  
```console  
signtool verify /a SystemFile.dll  
```  
  
 O comando a seguir verifica um arquivo de sistema assinado em um catálogo chamado `MyCatalog.cat`.  
  
```console  
signtool verify /c MyCatalog.cat SystemFile.dll  
```  
  
## <a name="see-also"></a>Consulte também

- [Ferramentas](index.md)
- [Prompts de comando](developer-command-prompt-for-vs.md)
