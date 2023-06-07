---
title: Ferramenta de registro de ServiceModel (ServiceModelReg.exe)
description: Use essa ferramenta de linha de comando para gerenciar o registro de componentes WCF e WF em um único computador se você tiver problemas com a ativação do serviço.
ms.date: 03/30/2017
ms.assetid: 396ec5ae-e34f-4c64-a164-fcf50e86b6ac
ms.openlocfilehash: 347b1b8071abe7d8eb7e16ffd879c1fdb9825bc7
ms.sourcegitcommit: 358a28048f36a8dca39a9fe6e6ac1f1913acadd5
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/23/2020
ms.locfileid: "85245889"
---
# <a name="servicemodel-registration-tool-servicemodelregexe"></a>Ferramenta de registro de ServiceModel (ServiceModelReg.exe)
Essa ferramenta de linha de comando fornece a capacidade de gerenciar o registro de componentes WCF e WF em um único computador. Em circunstâncias normais, não é necessário usar essa ferramenta, pois os componentes WCF e WF são configurados quando instalados. Mas se você estiver enfrentando problemas com a ativação do serviço, poderá tentar registrar os componentes usando essa ferramenta.  
  
## <a name="syntax"></a>Sintaxe  
  
```console  
ServiceModelReg.exe[(-ia|-ua|-r)|((-i|-u) -c:<command>)] [-v|-q] [-nologo] [-?]  
```  
  
## <a name="remarks"></a>Comentários  
 A ferramenta pode ser encontrada no seguinte local:  
  
 %SystemRoot%\Microsoft.Net\Framework\v3.0\Windows Communication Foundation \  
  
> [!NOTE]
> Quando a ferramenta de registro de ServiceModel é executada no Windows Vista, a caixa de diálogo **recursos do Windows** pode não refletir que a opção de **ativação de HTTP Windows Communication Foundation** em **Microsoft .NET Framework 3,0** está ativada. A caixa de diálogo **recursos do Windows** pode ser acessada clicando em **Iniciar**, depois em **executar** e digitando **OptionalFeatures**.  
  
 As tabelas a seguir descrevem as opções que podem ser usadas com ServiceModelReg.exe.  
  
|Opção|Descrição|  
|------------|-----------------|  
|`-ia`|Instala todos os componentes WCF e WF.|  
|`-ua`|Desinstala todos os componentes WCF e WF.|  
|`-r`|Repara todos os componentes WCF e WF.|  
|`-i`|Instala os componentes WCF e WF especificados com – c.|  
|`-u`|Desinstala os componentes WCF e WF especificados com – c.|  
|`-c`|Instala ou desinstala um componente:<br /><br /> -Httpnamespace – reserva de namespace HTTP<br />-tcpportsharing – serviço de compartilhamento de porta TCP<br />-tcpactivation – serviço de ativação TCP (sem suporte no perfil de cliente do .NET 4)<br />-namedpipeactivation – serviço de ativação de pipe nomeado (sem suporte no perfil de cliente do .NET 4<br />-MsmqActivation – serviço de ativação MSMQ (sem suporte no perfil do cliente .NET 4<br />-ETW – manifestos de rastreamento de eventos ETW (Windows Vista ou posterior)|  
|`-q`|Modo silencioso (somente exibir log de erros)|  
|`-v`|Modo detalhado.|  
|`-nologo`|Suprime a mensagem de direitos autorais e de cabeçalho.|  
|`-?`|Exibe o texto de ajuda|  
  
## <a name="fixing-the-fileloadexception-error"></a>Corrigindo o erro fileuploadexception  
 Se você instalou versões anteriores do WCF em seu computador, poderá receber um `FileLoadFoundException` erro ao executar a ferramenta ServiceModelReg para registrar uma nova instalação. Isso pode acontecer mesmo que você tenha removido manualmente os arquivos da instalação anterior, mas deixou as configurações de machine.config intactas.  
  
 A mensagem de erro é semelhante à seguinte.  
  
```console  
Error: System.IO.FileLoadException: Could not load file or assembly 'System.ServiceModel, Version=2.0.0.0, Culture=neutral, PublicKeyToken=b77a5c561934e089' or one of its dependencies. The located assembly's manifest definition does not match the assembly reference. (Exception from HRESULT: 0x80131040)  
File name: 'System.ServiceModel, Version=2.0.0.0, Culture=neutral, PublicKeyToken=b77a5c561934e089'  
```  
  
 Você deve observar a mensagem de erro informando que o assembly 2.0.0.0 da versão System. ServiceModel foi instalado por uma versão CTP (Customer Technology Preview) antecipada. A versão atual do assembly System. ServiceModel lançada é 3.0.0.0 em vez disso. Portanto, esse problema é encontrado quando você deseja instalar a versão oficial do WCF em um computador em que uma versão CTP inicial do WCF foi instalada, mas não completamente desinstalada.  
  
 ServiceModelReg.exe não pode limpar as entradas de versão anterior, nem registrar as entradas da nova versão. A única solução alternativa é editar manualmente machine.config. Você pode localizar esse arquivo no seguinte local.  
  
```console  
%windir%\Microsoft.NET\Framework\v2.0.50727\config\machine.config
```  
  
 Se você estiver executando o WCF em um computador de 64 bits, também deverá editar o mesmo arquivo neste local.  
  
```console  
%windir%\Microsoft.NET\Framework64\v2.0.50727\config\machine.config
```  
  
 Localize todos os nós XML neste arquivo que se referem a "System. ServiceModel, Version = 2.0.0.0", exclua-os e todos os nós filho. Salve o arquivo e execute novamente ServiceModelReg.exe resolver esse problema.  
  
## <a name="examples"></a>Exemplos  
 Os exemplos a seguir mostram como usar as opções mais comuns da ferramenta de ServiceModelReg.exe.  
  
```console  
ServiceModelReg.exe -ia  
  Installs all components  
ServiceModelReg.exe -i -c:httpnamespace -c:etw  
  Installs HTTP namespace reservation and ETW manifests  
ServiceModelReg.exe -u -c:etw  
  Uninstalls ETW manifests  
ServiceModelReg.exe -r  
  Repairs an extended install  
```
