---
ms.openlocfilehash: 0237c848d71150aaea1f9de4f4b16a0cbbc4ab3a
ms.sourcegitcommit: e02d17b2cf9c1258dadda4810a5e6072a0089aee
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/01/2020
ms.locfileid: "85615591"
---
### <a name="new-64-bit-jit-compiler-in-the-net-framework-46"></a>Novo compilador JIT de 64 bits no .NET Framework 4.6

#### <a name="details"></a>Detalhes

A partir do .NET Framework 4.6, um novo compilador JIT de 64 bits é usado para compilação Just-In-Time. Em alguns casos, será gerada uma exceção inesperada ou um comportamento diferente será observado se um aplicativo for executado usando o compilador de 32 bits ou o compilador JIT de 64 bits antigo. Essa alteração não afeta o compilador JIT de 32 bits. Os diferenças conhecidas incluem o seguinte:

- Sob certas condições, uma operação de conversão unboxing pode gerar uma <xref:System.NullReferenceException> em compilações de Versão com a otimização ativada.
- Em alguns casos, a execução do código de produção em um corpo de método grande pode gerar uma <xref:System.StackOverflowException>.
- Em certas condições, as estruturas passadas para um método são tratadas como tipos de referência em vez de tipos de valor em de builds de Versão. Um das manifestações desse problema é que os itens individuais em uma coleção aparecem em uma ordem inesperada.
- Sob determinadas condições, a comparação de valores <xref:System.UInt16> com seu conjunto de bits alto será incorreta se a otimização estiver habilitada.
- Sob certas condições, especialmente ao inicializar uma matriz de valores, a inicialização da memória pela instrução IL <xref:System.Reflection.Emit.OpCodes.Initblk?displayProperty=nameWithType> pode inicializar a memória com um valor incorreto. Isso pode resultar em uma saída incorreta ou uma exceção sem tratamento.
- Sob certas condições raras, um teste de bits condicional poderá retornar o valor <xref:System.Boolean> incorreto ou gerar uma exceção se as otimizações do compilador estiverem habilitadas.
- Sob certas condições, se uma instrução `if` for usada para testar uma condição antes de entrar em um bloco `try`, e na saída do bloco `try`, e a mesma condição for avaliada no bloco `catch` ou `finally`, o novo compilador JIT de 64 bits removerá a condição `if` do bloco `catch` ou `finally` durante a otimização do código. Como resultado, o código dentro da instrução `if` no bloco `catch` ou `finally` será executado incondicionalmente.

#### <a name="suggestion"></a>Sugestão

**Mitigação dos problemas conhecidos** <br/> Se você encontrar os problemas listados acima, solucione-os seguindo um destes procedimentos:

- Atualizar para o .NET Framework 4.6.2. O novo compilador de 64 bits incluído com o .NET Framework 4.6.2 resolve cada um desses problemas conhecidos.
- Verifique se a sua versão do Windows está atualizada executando o Windows Update. Atualizações de serviço para o .NET Framework 4.6 e 4.6.1 resolvem cada um desses problemas, exceto a <xref:System.NullReferenceException> em uma operação de conversão unboxing.
- Compilar com o compilador JIT de 64 bits mais antigo. Veja a seção **Mitigação de outros problemas** para saber mais sobre como fazer isso.
**Mitigação de outros problemas** <br/> Se você encontrar qualquer outra diferença de comportamento entre o código compilado com o compilador de 64 bits mais antigo e o novo compilador JIT de 64 bits, ou entre as versões de depuração e de versão de seu aplicativo, ambas compiladas com o novo compilador JIT de 64 bits, faça o seguinte para compilar seu aplicativo com o compilador JIT de 64 bits mais antigo:

- Em uma base por aplicativo, você pode adicionar o [<](~/docs/framework/configure-apps/file-schema/runtime/uselegacyjit-element.md) elemento ao arquivo de configuração do aplicativo. Veja a seguir como desabilitar a compilação com o novo compilador JIT de 64 bits e usar, em vez disso, o compilador JIT de 64 bits herdado.

    ```xml
    <?xml version ="1.0"?>
    <configuration>
      <runtime>
       <useLegacyJit enabled="1" />
      </runtime>
    </configuration>
    ```

- Adicione a cada usuário um valor `REG_DWORD` denominado `useLegacyJit` para a chave `HKEY_CURRENT_USER\SOFTWARE\Microsoft\.NETFramework` do Registro. Um valor 1 habilita o compilador JIT de 64 bits herdado; um valor 0 o desabilita e habilita o novo compilador JIT de 64 bits.
- Adicione a cada máquina um valor `REG_DWORD` denominado `useLegacyJit` para a chave `HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\.NETFramework` do Registro. Um valor de `1` habilita o compilador JIT de 64 bits herdado; um valor de `0` o desabilita e habilita o novo compilador JIT de 64 bits.
Avise-nos sobre o problema relatando um bug no [Microsoft Connect](https://connect.microsoft.com/VisualStudio).

| Name    | Valor       |
|:--------|:------------|
| Escopo   | Microsoft Edge        |
| Versão | 4.6         |
| Type    | Redirecionando |
