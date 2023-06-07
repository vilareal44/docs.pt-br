---
title: -errorreport
ms.date: 08/14/2018
helpviewer_keywords:
- -errorreport compiler option [Visual Basic]
- /errorreport compiler option [Visual Basic]
- errorreport compiler option [Visual Basic]
ms.assetid: a7fe83a2-a6d8-460c-8dad-79a8f433f501
ms.openlocfilehash: b6a1c8fce17e3e5a54366c2ff4dff4e6aa668f56
ms.sourcegitcommit: f8c270376ed905f6a8896ce0fe25b4f4b38ff498
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/04/2020
ms.locfileid: "84408654"
---
# <a name="-errorreport"></a>-errorreport

Especifica como o compilador de Visual Basic deve relatar erros internos do compilador.

## <a name="syntax"></a>Sintaxe

```console
-errorreport:{ prompt | queue | send | none }
```

## <a name="remarks"></a>Comentários

Essa opção fornece uma maneira conveniente de relatar um erro de compilador interno do Visual Basic (ICE) para a equipe de Visual Basic da Microsoft. Por padrão, o compilador não envia informações à Microsoft. No entanto, se você encontrar um erro interno do compilador, essa opção permitirá que você relate o erro à Microsoft. Essas informações ajudarão os engenheiros da Microsoft a identificar a causa e podem ajudar a melhorar a próxima versão do Visual Basic.

A capacidade do usuário de enviar relatórios depende das permissões de política de computador e de usuário.

A tabela a seguir resume o efeito da `-errorreport` opção.

|Opção|Comportamento|
|---|---|
|`prompt`|Se ocorrer um erro de compilador interno, uma caixa de diálogo aparecerá para que você possa exibir os dados exatos que o compilador coletou. Você pode determinar se há alguma informação confidencial no relatório de erros e tomar uma decisão sobre se deseja enviá-la à Microsoft. Se você decidir enviá-lo e as configurações de política de computador e de usuário permitirem, o compilador enviará os dados para a Microsoft.|
|`queue`|Enfileira o relatório de erros. Ao fazer logon com privilégios de administrador, você pode relatar quaisquer falhas desde a última vez que você fez logon (não será solicitado que você envie relatórios para falhas mais de uma vez a cada três dias). Esse é o comportamento padrão quando a `-errorreport` opção não é especificada.|
|`send`|Se ocorrer um erro de compilador interno e as configurações de política de computador e de usuário permitirem, o compilador enviará os dados para a Microsoft.<br /><br /> A opção `-errorreport:send` tentará enviar automaticamente informações de erro à Microsoft se o relatório estiver habilitado pelo [relatório de erros do Windows](/windows/desktop/wer/windows-error-reporting) configurações do sistema. |
|`none`|Se ocorrer um erro interno do compilador, ele não será coletado nem enviado à Microsoft.|

O compilador envia dados que incluem a pilha no momento do erro, que geralmente inclui um código-fonte. Se `-errorreport` for usado com a opção [-bugreport](bugreport.md) , o arquivo de origem inteiro será enviado.

Essa opção é melhor usada com a opção [-bugreport](bugreport.md) , pois permite que os engenheiros da Microsoft reproduzam o erro mais facilmente.

> [!NOTE]
> A `-errorreport` opção não está disponível no ambiente de desenvolvimento do Visual Studio; ela está disponível somente durante a compilação na linha de comando.

## <a name="example"></a>Exemplo

O código a seguir tenta compilar `T2.vb` e, se o compilador encontrar um erro de compilador interno, ele solicitará que você envie o relatório de erros à Microsoft.

```console
vbc -errorreport:prompt t2.vb
```

## <a name="see-also"></a>Confira também

- [Compilador de linha de comando do Visual Basic](index.md)
- [Linhas de Comando de Compilação de Exemplo](sample-compilation-command-lines.md)
- [-bugreport](bugreport.md)
