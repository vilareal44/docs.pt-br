---
ms.openlocfilehash: 5e2e8d1ec5d698d1c1649c2a0a1b4b77dbdf4022
ms.sourcegitcommit: e02d17b2cf9c1258dadda4810a5e6072a0089aee
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/01/2020
ms.locfileid: "85621028"
---
### <a name="wpf-printing-stack-update"></a>Atualização da pilha de impressão do WPF

#### <a name="details"></a>Detalhes

Agora, as APIs de Impressão do WPF que usam <xref:System.Printing.PrintQueue?displayProperty=fullName>chamam a API de Impressão Pacote de Documentos do Windows em vez da API de Impressão XPS, que foi preterida. A alteração foi feita para fins de manutenção. Nem usuários nem os desenvolvedores devem ver todas as alterações no comportamento ou no uso da API. A nova pilha de impressão é habilitada por padrão ao ser executada na Atualização do Windows 10 para Criadores. A pilha de impressão antiga continuará a funcionar como antes nas versões mais antigas do Windows.

#### <a name="suggestion"></a>Sugestão

Para usar a pilha antiga na Atualização do Windows 10 para Criadores, defina o valor REG_DWORD <code>UseXpsOMPrinting</code> da chave do Registro <code>HKEY_CURRENT_USER\Software\Microsoft\.NETFramework\Windows Presentation Foundation\Printing</code> como <code>1</code>.

| Name    | Valor       |
|:--------|:------------|
| Escopo   |Microsoft Edge|
|Versão|4.7|
|Type|Runtime|
