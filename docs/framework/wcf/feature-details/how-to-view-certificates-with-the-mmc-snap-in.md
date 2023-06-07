---
title: Como exibir certificados com o snap-in do MMC
description: Um cliente ou serviço WCF seguro pode usar um certificado como uma credencial. Saiba mais sobre os tipos de repositórios de certificados que você pode examinar usando o plug-in do MMC.
ms.date: 02/25/2019
helpviewer_keywords:
- certificates [WCF], viewing with the MMC snap-in
ms.assetid: 2b8782aa-ebb4-4ee7-974b-90299e356dc5
ms.openlocfilehash: e63034e48ae836f67f89b454829f7196c94610cd
ms.sourcegitcommit: 358a28048f36a8dca39a9fe6e6ac1f1913acadd5
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/23/2020
ms.locfileid: "85246669"
---
# <a name="how-to-view-certificates-with-the-mmc-snap-in"></a>Como exibir certificados com o snap-in do MMC
Ao criar um cliente ou serviço seguro, você pode usar um [certificado](working-with-certificates.md) como a credencial. Por exemplo, um tipo comum de credencial é o certificado X. 509, que você cria com o <xref:System.ServiceModel.Security.X509CertificateInitiatorClientCredential.SetCertificate%2A?displayProperty=nameWithType> método.

Há três tipos diferentes de repositórios de certificados que você pode examinar com o MMC (console de gerenciamento Microsoft) em sistemas Windows:

- Computador local: o repositório é local para o dispositivo e global para todos os usuários no dispositivo.

- Usuário atual: o repositório é local para a conta de usuário atual no dispositivo.

- Conta de serviço: o repositório é local para um serviço específico no dispositivo.

## <a name="view-certificates-in-the-mmc-snap-in"></a>Exibir certificados no snap-in do MMC

O procedimento a seguir demonstra como examinar as lojas em seu dispositivo local para encontrar um certificado apropriado:
  
1. Selecione **executar** no menu **Iniciar** e, em seguida, insira *MMC*.

    O MMC é exibido.
  
2. No menu **arquivo** , selecione **Adicionar/remover snap-in**.

    A janela **Adicionar ou remover snap-ins** é exibida.
  
3. Na lista **snap-ins disponíveis** , escolha **certificados**e, em seguida, selecione **Adicionar**.  

    ![Adicionar Snap-in de certificado](./media/mmc-add-certificate-snap-in.png)
  
4. Na janela **snap-in de certificados** , selecione **conta de computador**e, em seguida, selecione **Avançar**.
  
    Opcionalmente, você pode selecionar **minha conta de usuário** para o usuário atual ou a **conta de serviço** para um serviço específico.

    > [!NOTE]
    > Se você não for um administrador para seu dispositivo, poderá gerenciar certificados somente para sua conta de usuário.
  
5. Na janela **Selecionar computador** , deixe **computador local** selecionado e, em seguida, selecione **concluir**.  
  
6. Na janela **Adicionar ou remover snap-in** , selecione **OK**.  
  
    ![Adicionar Snap-in de certificado](./media/mmc-certificate-snap-in-selected.png)

7. Opcional: no menu **arquivo** , selecione **salvar** ou **salvar como** para salvar o arquivo de console do MMC para uso posterior.  

8. Para exibir seus certificados no snap-in do MMC, selecione **raiz do console** no painel esquerdo e, em seguida, expanda **certificados (computador local)**.

    Uma lista de diretórios para cada tipo de certificado é exibida. Em cada diretório de certificado, você pode exibir, exportar, importar e excluir seus certificados.

## <a name="view-certificates-with-the-certificate-manager-tool"></a>Exibir certificados com a ferramenta Gerenciador de certificados

Você também pode exibir, exportar, importar e excluir certificados usando a ferramenta Gerenciador de certificados.

### <a name="to-view-certificates-for-the-local-device"></a>Para exibir certificados para o dispositivo local

1. Selecione **executar** no menu **Iniciar** e, em seguida, digite *certlm. msc*.

    A ferramenta Gerenciador de certificados para o dispositivo local é exibida.
  
2. Para exibir seus certificados, em **certificados – computador local** no painel esquerdo, expanda o diretório do tipo de certificado que você deseja exibir.

### <a name="to-view-certificates-for-the-current-user"></a>Para exibir certificados para o usuário atual

1. Selecione **executar** no menu **Iniciar** e digite *certmgr. msc*.

    A ferramenta Gerenciador de certificados para o usuário atual é exibida.
  
2. Para exibir seus certificados, em **certificados – usuário atual** no painel esquerdo, expanda o diretório do tipo de certificado que você deseja exibir.

## <a name="see-also"></a>Veja também

- [Trabalhando com certificados](working-with-certificates.md)
- [Como criar certificados temporários para uso durante o desenvolvimento](how-to-create-temporary-certificates-for-use-during-development.md)
- [Como recuperar a impressão digital de um certificado](how-to-retrieve-the-thumbprint-of-a-certificate.md)
