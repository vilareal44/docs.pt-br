---
title: Cert2spc.exe (Ferramenta de Teste de Certificado do Fornecedor do Software)
description: Use Cert2spc.exe, a ferramenta de teste de certificado do fornecedor de software. Essa ferramenta cria um certificado do fornecedor de software (SPC) de um ou mais certificados X. 509.
ms.date: 03/30/2017
helpviewer_keywords:
- SPC
- Software Publisher Certificate Test tool
- Software Publisher Certificate
- Cert2spc.exe
- certificates, Software Publisher's Certificate
ms.assetid: be434d7d-9c0d-46e7-8392-58a9b542d11d
ms.openlocfilehash: 2eb6339aa6f5d23a5b87986410cbeaac2dac2bec
ms.sourcegitcommit: 87cfeb69226fef01acb17c56c86f978f4f4a13db
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/24/2020
ms.locfileid: "87167318"
---
# <a name="cert2spcexe-software-publisher-certificate-test-tool"></a>Cert2spc.exe (Ferramenta de Teste de Certificado do Fornecedor do Software)
A ferramenta Teste de Certificado do Editor de Software cria um SPC (Software Publisher's Certificate) com base em um ou vários certificados X.509. Cert2spc.exe é apenas para fins de teste. É possível obter um SPC válido de uma Autoridade de Certificação como, por exemplo, Verisign ou Thawte. Para obter mais informações sobre a criação de certificados X.509, consulte [Makecert.exe (Ferramenta de Criação de Certificado)](/windows/desktop/SecCrypto/makecert).  
  
 Essa ferramenta é instalada automaticamente com o Visual Studio. Para executar a ferramenta, use o Prompt de Comando do Desenvolvedor para Visual Studio (ou o Prompt de Comando do Visual Studio no Windows 7). Para obter mais informações, consulte [Prompts de Comando](developer-command-prompt-for-vs.md).  
  
 No prompt de comando, digite o seguinte:  
  
## <a name="syntax"></a>Sintaxe  
  
```console  
cert2spc cert1.cer | crl1.crl [... certN.cer | crlN.crl] outputSPCfile.spc  
```  
  
## <a name="parameters"></a>parâmetros  
  
|Argumento|Descrição|  
|--------------|-----------------|  
|`certN.cer`|O nome de um certificado X.509 a ser incluído no arquivo SPC. É possível especificar vários nomes separados por espaços.|  
|`crlN.crl`|O nome de uma lista de revogações do certificado a ser incluída no arquivo SPC. É possível especificar vários nomes separados por espaços.|  
|`outputSPCfile.spc`|O nome do objeto PKCS #7 que conterá os certificados X.509.|  
  
|Opção|Descrição|  
|------------|-----------------|  
|**/?**|Exibe sintaxe de comando e opções para a ferramenta.|  
  
## <a name="examples"></a>Exemplos  
 O comando a seguir cria um SPC com base em `myCertificate.cer` e o coloca em `mySPCFile.spc`.  
  
```console
cert2spc myCertificate.cer mySPCFile.spc  
```  
  
 O comando a seguir cria um SPC com base em `oneCertificate.cer` e `twoCertificate.cer` e o coloca em `mySPCFile.spc`.  
  
```console
cert2spc oneCertificate.cer twoCertificate.cer mySPCFile.spc  
```  
  
## <a name="see-also"></a>Confira também

- [Ferramentas](index.md)
- [Makecert.exe (Ferramenta de Criação de Certificado)](/windows/desktop/SecCrypto/makecert)
- [Prompts de comando](developer-command-prompt-for-vs.md)
