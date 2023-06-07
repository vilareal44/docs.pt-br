---
title: Determinar quais versões do .NET Framework estão instaladas
description: Use o código, regedit.exe ou PowerShell para detectar quais versões do .NET Framework estão instaladas em um computador consultando o registro do Windows.
ms.date: 02/03/2020
dev_langs:
- csharp
- vb
helpviewer_keywords:
- versions, determining for .NET Framework
- .NET Framework, determining version
ms.assetid: 40a67826-e4df-4f59-a651-d9eb0fdc755d
ms.openlocfilehash: 1591aacf5496852609ae571a52aaea2d5c09a15b
ms.sourcegitcommit: 8bfeb5930ca48b2ee6053f16082dcaf24d46d221
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/18/2020
ms.locfileid: "88558550"
---
# <a name="how-to-determine-which-net-framework-versions-are-installed"></a>Como determinar quais versões do .NET Framework estão instaladas

Os usuários podem [instalar](../install/index.md) e executar várias versões do .NET Framework em seus computadores. Quando você desenvolve ou implanta um aplicativo, é necessário saber qual versão do .NET Framework está instalada no computador do usuário. O registro contém uma lista das versões de .NET Framework instaladas em um computador.

O .NET Framework consiste em dois componentes principais, que têm o controle de versão feito separadamente:

- Um conjunto de assemblies que são coleções de tipos e recursos que fornecem a funcionalidade para os aplicativos. O .NET Framework e assemblies que compartilham o mesmo número de versão. Por exemplo, versões do .NET Framework incluem 4.5, 4.6.1 e 4.7.2.

- O Common Language Runtime (CLR) que gerencia e executa o código dos aplicativos. Uma única versão do CLR geralmente dá suporte a várias versões do .NET Framework. Por exemplo, o CLR versão 4.0.30319. *xxxxx* em que *xxxxx* é menor que 42000, dá suporte a .NET Framework versões 4 por meio de 4.5.2. A versão do CLR maior ou igual ao 4.0.30319.42000 dá suporte a versões .NET Framework a partir do .NET Framework 4,6.

As ferramentas mantidas pela Comunidade estão disponíveis para ajudar a detectar quais versões do .NET Framework estão instaladas:

- [https://github.com/jmalarcon/DotNetVersions](https://github.com/jmalarcon/DotNetVersions)

  Uma ferramenta de linha de comando do .NET 2,0.

- [https://github.com/EliteLoser/DotNetVersionLister](https://github.com/EliteLoser/DotNetVersionLister)

  Um módulo do PowerShell 2,0.

Para obter informações sobre como detectar as atualizações instaladas para cada versão do .NET Framework, consulte [como: determinar quais .NET Framework atualizações estão instaladas](how-to-determine-which-net-framework-updates-are-installed.md).

## <a name="detect-net-framework-45-and-later-versions"></a>Detectar .NET Framework 4,5 e versões posteriores

A versão do .NET Framework (4,5 e posterior) instalada em um computador está listada no registro em **HKEY_LOCAL_MACHINE \\ software \\ Microsoft \\ NET Framework Setup \\ NDP \\ v4 \\ Full**. Se a subchave **completa** estiver ausente, .NET Framework 4,5 ou superior não estará instalado.

> [!NOTE]
> A subchave de **instalação do NET Framework** no caminho do registro *não começa com* um ponto.

O valor da **versão** REG_DWORD no registro representa a versão do .NET Framework instalado.

<a name="version_table"></a>

| Versão do .NET Framework | Valor da **versão** |
| ---------------------- | -------------------------- |
| .NET Framework 4.5     | Todos os sistemas operacionais Windows: 378389 |
| .NET Framework 4.5.1   | No Windows 8.1 e no Windows Server 2012 R2:378675<br />Em todos os outros sistemas operacionais Windows: 378758 |
| .NET Framework 4.5.2   | Todos os sistemas operacionais Windows: 379893 |
| .NET Framework 4.6     | No Windows 10:393295<br />Em todos os outros sistemas operacionais Windows: 393297 |
| .NET Framework 4.6.1   | Em sistemas com a Atualização de novembro do Windows 10: 394254<br />Em todos os outros sistemas operacionais Windows (incluindo o Windows 10): 394271 |
| .NET Framework 4.6.2   | Na Atualização de Aniversário do Windows 10 e Windows Server 2016: 394802<br />Em todos os outros sistemas operacionais Windows (incluindo outros sistemas operacionais Windows 10): 394806 |
| .NET Framework 4.7     | No Windows 10 Creators Update: 460798<br />Em todos os outros sistemas operacionais Windows (incluindo outros sistemas operacionais Windows 10): 460805 |
| .NET Framework 4.7.1   | Na atualização dos criadores de outono do Windows 10 e do Windows Server, versão 1709:461308<br/>Em todos os outros sistemas operacionais Windows (incluindo outros sistemas operacionais Windows 10): 461310 |
| .NET Framework 4.7.2   | Na atualização do Windows 10 de abril de 2018 e Windows Server, versão 1803:461808<br/>Em todos os sistemas operacionais Windows, exceto Windows 10 de abril de 2018 atualização e Windows Server, versão 1803:461814 |
| .NET Framework 4.8     | No Windows 10 maio 2019 atualização e Windows 10 de novembro de 2019 atualização: 528040<br/>No Windows 10, pode ser 2020 atualização: 528372<br/>Em todos os outros sistemas operacionais Windows (incluindo outros sistemas operacionais Windows 10): 528049 |

### <a name="minimum-version"></a>Versão mínima

Para determinar se uma versão *mínima* do .NET Framework está presente, use a menor **versão** REG_DWORD valor para essa versão da tabela anterior.

Por exemplo, se seu aplicativo for executado em .NET Framework 4,8 ou em uma versão posterior, teste para **obter uma versão** REG_DWORD valor que seja *maior ou igual a* 528040.

| Versão do .NET Framework | Valor mínimo |
| ---------------------- | ------------- |
| .NET Framework 4.5     | 378389 |
| .NET Framework 4.5.1   | 378675 |
| .NET Framework 4.5.2   | 379893 |
| .NET Framework 4.6     | 393295 |
| .NET Framework 4.6.1   | 394254 |
| .NET Framework 4.6.2   | 394802 |
| .NET Framework 4.7     | 460798 |
| .NET Framework 4.7.1   | 461308 |
| .NET Framework 4.7.2   | 461808 |
| .NET Framework 4.8     | 528040 |

### <a name="use-registry-editor"></a>Usar o editor do registro

01. No menu **Iniciar**, escolha **Executar**, insira *regedit* e, em seguida, selecione **OK**.

    Você precisará ter credenciais administrativas para executar o REGEDIT.

01. No editor do registro, abra a seguinte subchave: **HKEY_LOCAL_MACHINE \\ software \\ Microsoft \\ NET Framework Setup \\ NDP \\ v4 \\ Full**. Se a subchave **Full** não estiver presente, você não terá o .NET Framework 4.5 ou posterior instalado.

01. Verifique se há uma entrada de REG_DWORD chamada **versão**. Se ele existir, você terá .NET Framework 4,5 ou posterior instalado. Seu valor corresponde a uma versão específica do .NET Framework. Na figura a seguir, por exemplo, o valor da entrada de **versão** é 528040, que é a chave de versão para .NET Framework 4,8.

    ![Entrada de registro para o .NET Framework 4,5](./media/clr-installdir.png "Entrada de registro para o .NET Framework 4,5")

### <a name="use-powershell-to-check-for-a-minimum-version"></a>Usar o PowerShell para verificar se há uma versão mínima

Use os comandos do PowerShell para verificar o valor da entrada de **versão** do **HKEY_LOCAL_MACHINE \\ software \\ Microsoft \\ NET Framework Setup \\ NDP \\ v4 \\ completa** subkey.

Os exemplos a seguir verificam o valor da entrada **Release** para determinar se o .NET Framework 4.6.2 ou posterior está instalado. Esse código retornará `True` se ele estiver instalado; caso contrário, retornará `False`.

```PowerShell
(Get-ItemProperty "HKLM:SOFTWARE\Microsoft\NET Framework Setup\NDP\v4\Full").Release -ge 394802
```

### <a name="query-the-registry-using-code"></a>Consultar o registro usando código

01. Use os <xref:Microsoft.Win32.RegistryKey.OpenBaseKey%2A?displayProperty=nameWithType> <xref:Microsoft.Win32.RegistryKey.OpenSubKey%2A?displayProperty=nameWithType> métodos e para acessar o **HKEY_LOCAL_MACHINE \\ software \\ Microsoft \\ NET Framework instalação \\ NDP \\ v4 \\ completa** subkey no registro do Windows.

    > [!IMPORTANT]
    > Se o aplicativo que você está executando for de 32 bits e estiver em execução no Windows de 64 bits, os caminhos do registro serão diferentes dos listados anteriormente. O registro de 64 bits está disponível na subchave **HKEY_LOCAL_MACHINE \\ software \\ Wow6432Node \\ ** . Por exemplo, a subchave do registro para .NET Framework 4,5 é **HKEY_LOCAL_MACHINE \\ software \\ Wow6432Node \\ Microsoft \\ NET Framework Setup \\ NDP \\ v4 \\ Full**.

01. Verifique o valor da **versão** REG_DWORD para determinar a versão instalada. Para compatibilidade com a versão atual, verifique se há um valor superior ou igual ao valor listado na [tabela de versões do .NET Framework](#version_table).

O seguinte exemplo verifica o valor da entrada **Release** no Registro para encontrar o .NET Framework 4.5 e versões posteriores que estão instaladas:

[!code-csharp[ListVersions#5](../../../samples/snippets/csharp/framework/migration-guide/versions-installed3.cs)]
[!code-vb[ListVersions#5](../../../samples/snippets/visualbasic/framework/migration-guide/versions-installed3.vb)]

Este exemplo segue a prática recomendada para a verificação de versão:

- Ele verifica se o valor da entrada **Release** é *superior ou igual* ao valor das chaves de versão conhecidas.
- Ele verifica na ordem da versão mais recente para a versão mais antiga.

## <a name="detect-net-framework-10-through-40"></a>Detectar .NET Framework 1,0 a 4,0

Cada versão do .NET Framework de 1,1 a 4,0 é listada como uma subchave em **HKEY_LOCAL_MACHINE \\ software \\ Microsoft \\ NET Framework Setup \\ NDP**. A tabela a seguir lista o caminho para cada versão de .NET Framework. Para a maioria das versões, há um valor de REG_DWORD de **instalação** de `1` para indicar que esta versão está instalada. Nessas subchaves, há também uma **versão** REG_SZ valor que contém uma cadeia de caracteres de versão.

> [!NOTE]
> A subchave de **instalação do NET Framework** no caminho do registro *não começa com* um ponto.

| Versão do Framework  | Subchave do registro | Valor |
| ------------------ | --------------- | ----- |
| 1.0                | **HKLM \\ software \\ Microsoft \\ . Política de NETFramework \\ \\ v 1.0 \\ 3705**     | **Instalar** o REG_SZ igual a `1` |
| 1,1                | **HKLM \\ software \\ Microsoft \\ NET Framework Setup \\ NDP \\ v 1.1.4322**   | **Instalar** o REG_DWORD igual a `1` |
| 2.0                | **HKLM \\ software \\ Microsoft \\ NET Framework Setup \\ NDP \\ v 2.0.50727**  | **Instalar** o REG_DWORD igual a `1` |
| 3.0                | **HKLM \\ software \\ Microsoft \\ NET Framework Setup \\ NDP \\ v 3.0 \\ instalação** | **InstallSuccess** REG_DWORD igual a `1` |
| 3,5                | **HKLM \\ software \\ Microsoft \\ NET Framework Setup \\ NDP \\ v 3.5**        | **Instalar** o REG_DWORD igual a `1` |
| 4,0 perfil do cliente | **HKLM \\ software \\ Microsoft \\ NET Framework Setup \\ NDP \\ v4 \\ Client**  | **Instalar** o REG_DWORD igual a `1` |
| 4,0 perfil completo   | **HKLM \\ software \\ Microsoft \\ NET Framework Setup \\ NDP \\ v4 \\ completo**    | **Instalar** o REG_DWORD igual a `1` |

> [!IMPORTANT]
> Se o aplicativo que você está executando for de 32 bits e estiver em execução no Windows de 64 bits, os caminhos do registro serão diferentes dos listados anteriormente. O registro de 64 bits está disponível na subchave **HKEY_LOCAL_MACHINE \\ software \\ Wow6432Node \\ ** . Por exemplo, a subchave do registro para .NET Framework 3,5 é **HKEY_LOCAL_MACHINE \\ software \\ Wow6432Node \\ Microsoft \\ NET Framework Setup \\ NDP \\ v 3.5**.

Observe que o caminho do registro para a subchave .NET Framework 1,0 é diferente dos outros.

### <a name="use-registry-editor-older-framework-versions"></a>Usar o editor do registro (versões mais antigas do Framework)

01. No menu **Iniciar**, escolha **Executar**, insira *regedit* e, em seguida, selecione **OK**.

    Você precisará ter credenciais administrativas para executar o REGEDIT.

01. Abra a subchave que corresponde à versão que você deseja verificar. Use a tabela na seção [detectar .NET Framework 1,0 a 4,0](#detect-net-framework-10-through-40) .

    A figura a seguir mostra a subchave e seu valor de **versão** para .NET Framework 3,5.

    ![A entrada do registro para o .NET Framework 3,5.](./media/net-4-and-earlier.png ".NET Framework 3,5 e versões anteriores")

### <a name="query-the-registry-using-code-older-framework-versions"></a>Consultar o registro usando código (versões mais antigas do Framework)

Use a <xref:Microsoft.Win32.RegistryKey?displayProperty=nameWithType> classe para acessar a subchave de **instalação HKEY_LOCAL_MACHINE do \\ \\ NDP software Microsoft \\ NET Framework Setup \\ ** no registro do Windows.

> [!IMPORTANT]
> Se o aplicativo que você está executando for de 32 bits e estiver em execução no Windows de 64 bits, os caminhos do registro serão diferentes dos listados anteriormente. O registro de 64 bits está disponível na subchave **HKEY_LOCAL_MACHINE \\ software \\ Wow6432Node \\ ** . Por exemplo, a subchave do registro para .NET Framework 3,5 é **HKEY_LOCAL_MACHINE \\ software \\ Wow6432Node \\ Microsoft \\ NET Framework Setup \\ NDP \\ v 3.5**.

O exemplo a seguir localiza as .NET Framework de 1 a 4 versões instaladas:

[!code-csharp[ListVersions](../../../samples/snippets/csharp/framework/migration-guide/versions-installed1.cs)]
[!code-vb[ListVersions](../../../samples/snippets/visualbasic/framework/migration-guide/versions-installed1.vb)]

## <a name="find-clr-versions"></a>Encontrar versões do CLR

O .NET Framework CLR instalado com .NET Framework tem controle de versão separadamente. Há duas maneiras de detectar a versão do .NET Framework CLR:

- **A ferramenta de Clrver.exe**

  Use a [ferramenta de versão do CLR (Clrver.exe)](../tools/clrver-exe-clr-version-tool.md) para determinar quais versões do CLR estão instaladas em um computador. Abra o [prompt de comando do desenvolvedor para o Visual Studio](../tools/developer-command-prompt-for-vs.md) e digite `clrver` .

  Saída de exemplo:

  ```console
  Versions installed on the machine:
  v2.0.50727
  v4.0.30319
  ```

- **A `Environment` classe**

  > [!IMPORTANT]
  > Para .NET Framework 4,5 e versões posteriores, não use a <xref:System.Environment.Version%2A?displayProperty=nameWithType> propriedade para detectar a versão do CLR. Em vez disso, consulte o registro conforme descrito em [detectar .NET Framework 4,5 e versões posteriores](#detect-net-framework-45-and-later-versions).
  
  01. Consulte a propriedade <xref:System.Environment.Version?displayProperty=nameWithType> para recuperar um objeto <xref:System.Version>.
  
      O objeto `System.Version` retornado identifica a versão do runtime que está executando o código. Ele não retorna versões de assembly nem outras versões do runtime que possam ter sido instaladas no computador.
  
      Para .NET Framework versões 4, 4,5, 4.5.1 e 4.5.2, a representação de cadeia de caracteres do <xref:System.Version> objeto retornado tem o formato 4.0.30319.* xxxxx*, em que *xxxxx* é menor que 42000. Para .NET Framework 4,6 e versões posteriores, ele tem o formato 4.0.30319.42000.
  
  01. Depois de ter o objeto **version** , consulte o seguinte:
  
      - Para o identificador de versão principal (por exemplo, *4* para a versão 4.0), use a propriedade <xref:System.Version.Major%2A?displayProperty=nameWithType>.
  
      - Para o identificador de versão secundária (por exemplo, *0* para a versão 4.0), use a propriedade <xref:System.Version.Minor%2A?displayProperty=nameWithType>.
  
      - Para a cadeia de caracteres de versão inteira (por exemplo, *4.0.30319.18010*), use o método <xref:System.Version.ToString%2A?displayProperty=nameWithType>. Esse método retorna um único valor que reflete a versão do runtime que está executando o código. Ele não retorna versões de assembly nem outras versões do runtime que possam ter sido instaladas no computador.

  O seguinte exemplo usa a propriedade <xref:System.Environment.Version%2A?displayProperty=nameWithType> para recuperar informações de versão do CLR:
  
  [!code-csharp[ListVersions](../../../samples/snippets/csharp/framework/migration-guide/versions-installed2.cs)]
  [!code-vb[ListVersions](../../../samples/snippets/visualbasic/framework/migration-guide/versions-installed2.vb)]

## <a name="see-also"></a>Confira também

- [Como determinar quais .NET Framework atualizações estão instaladas](how-to-determine-which-net-framework-updates-are-installed.md)
- [Instalar o .NET Framework para desenvolvedores](../install/guide-for-developers.md)
- [Versões e dependências do .NET Framework](versions-and-dependencies.md)
