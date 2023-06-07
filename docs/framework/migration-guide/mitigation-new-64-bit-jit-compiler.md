---
title: 'Mitigação: novo compilador JIT de 64 bits'
description: Saiba mais sobre o novo compilador JIT de 64 bits incluído no .NET Framework 4,6 e comportamento inesperado ou exceções que podem ocorrer durante a compilação.
ms.date: 03/30/2017
helpviewer_keywords:
- JIT compiler, 64-bit
- JIT compilation, 64-bit
- RyuJIT compiler
ms.assetid: 0332dabc-72c5-4bdc-8975-20d717802b17
ms.openlocfilehash: f059cbdd3b2a66ac8a668b7b8a80d9ad1551fa64
ms.sourcegitcommit: cf5a800a33de64d0aad6d115ffcc935f32375164
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/20/2020
ms.locfileid: "86475223"
---
# <a name="mitigation-new-64-bit-jit-compiler"></a>Mitigação: novo compilador JIT de 64 bits
A partir do .NET Framework 4,6, o tempo de execução inclui um novo compilador JIT de 64 bits para compilação just-in-time. Essa alteração não afeta a compilação com o compilador JIT de 32 bits.  
  
## <a name="unexpected-behavior-or-exceptions"></a>Comportamento ou exceções inesperadas  
 Em alguns casos, a compilação com o novo compilador JIT de 64 bits resulta em uma exceção de runtime ou em um comportamento não observado durante a execução do código compilado pelo compilador JIT de 64 bits mais antigo. Os diferenças conhecidas incluem o seguinte:  
  
> [!IMPORTANT]
> Todos esses problemas conhecidos foram resolvidos no novo compilador de 64 bits lançado com o .NET Framework 4.6.2. A maioria também foi resolvida nas versões de serviço do .NET Framework 4.6 e 4.6.1, incluídos com o Windows Update. Você pode eliminar esses problemas garantindo que sua versão do Windows esteja atualizada, ou atualizando para o .NET Framework 4.6.2.  
  
- Sob certas condições, uma operação de conversão unboxing pode gerar uma <xref:System.NullReferenceException> em compilações de Versão com a otimização ativada.  
  
- Em alguns casos, a execução do código de produção em um corpo de método grande pode gerar uma <xref:System.StackOverflowException>.  
  
- Sob certas condições, as estruturas passadas para um método são tratadas como tipos de referência em vez de tipos de valor em compilações de Versão. Um das manifestações desse problema é que os itens individuais em uma coleção aparecem em uma ordem inesperada.  
  
- Sob determinadas condições, a comparação de valores <xref:System.UInt16> com seu conjunto de bits alto será incorreta se a otimização estiver habilitada.  
  
- Sob certas condições, especialmente ao inicializar uma matriz de valores, a inicialização da memória pela instrução IL <xref:System.Reflection.Emit.OpCodes.Initblk?displayProperty=nameWithType> pode inicializar a memória com um valor incorreto. Isso pode resultar em uma saída incorreta ou uma exceção sem tratamento.  
  
- Sob certas condições raras, um teste de bits condicional poderá retornar o valor <xref:System.Boolean> incorreto ou gerar uma exceção se as otimizações do compilador estiverem habilitadas.  
  
- Sob certas condições, se uma instrução `if` for usada para testar uma condição antes de entrar em um bloco `try`, e na saída do bloco `try`, e a mesma condição for avaliada no bloco `catch` ou `finally`, o novo compilador JIT de 64 bits removerá a condição `if` do bloco `catch` ou `finally` durante a otimização do código. Como resultado, o código dentro da instrução `if` no bloco `catch` ou `finally` será executado incondicionalmente.  
  
<a name="General"></a>
## <a name="mitigation-of-known-issues"></a>Mitigação dos problemas conhecidos  
 Se você encontrar os problemas listados acima, solucione-os seguindo um destes procedimentos:  
  
- Atualizar para o .NET Framework 4.6.2. O novo compilador de 64 bits incluído com o .NET Framework 4.6.2 resolve cada um desses problemas conhecidos.  
  
- Verifique se a sua versão do Windows está atualizada executando o Windows Update. Atualizações de serviço para o .NET Framework 4.6 e 4.6.1 resolvem cada um desses problemas, exceto a <xref:System.NullReferenceException> em uma operação de conversão unboxing.  
  
- Compilar com o compilador JIT de 64 bits mais antigo. Veja a seção [Mitigação de outros problemas](#Other) para saber mais sobre como fazer isso.  
  
<a name="Other"></a>
## <a name="mitigation-of-other-issues"></a>Mitigação de outros problemas  
 Se você encontrar qualquer outra diferença de comportamento entre o código compilado com o compilador de 64 bits mais antigo e o novo compilador JIT de 64 bits, ou entre as versões de depuração e de versão de seu aplicativo, ambas compiladas com o novo compilador JIT de 64 bits, faça o seguinte para compilar seu aplicativo com o compilador JIT de 64 bits mais antigo:  
  
- Em uma base por aplicativo, você pode adicionar o [\<useLegacyJit>](../configure-apps/file-schema/runtime/uselegacyjit-element.md) elemento ao arquivo de configuração do aplicativo. Veja a seguir como desabilitar a compilação com o novo compilador JIT de 64 bits e usar, em vez disso, o compilador JIT de 64 bits herdado.  
  
    ```xml  
    <?xml version ="1.0"?>  
    <configuration>  
        <runtime>  
           <useLegacyJit enabled="1" />  
        </runtime>  
    </configuration>  
    ```  
  
- Adicione a cada usuário um valor `REG_DWORD` denominado `useLegacyJit` para a chave `HKEY_CURRENT_USER\SOFTWARE\Microsoft\.NETFramework` do Registro. Um valor 1 habilita o compilador JIT de 64 bits herdado; um valor 0 o desabilita e habilita o novo compilador JIT de 64 bits.  
  
- Adicione a cada máquina um valor `REG_DWORD` denominado `useLegacyJit` para a chave `HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\.NETFramework` do Registro. Um valor 1 habilita o compilador JIT de 64 bits herdado; um valor 0 o desabilita e habilita o novo compilador JIT de 64 bits.  
  
 Avise-nos sobre o problema relatando um bug no [Microsoft Connect](https://connect.microsoft.com/VisualStudio).  
  
## <a name="see-also"></a>Consulte também

- [Compatibilidade de aplicativos](application-compatibility.md)
- [\<useLegacyJit>Elementos](../configure-apps/file-schema/runtime/uselegacyjit-element.md)
