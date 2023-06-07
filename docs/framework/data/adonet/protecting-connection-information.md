---
title: Protegendo informações de conexão
description: Saiba mais sobre vulnerabilidades de segurança em cadeias de conexão, que podem surgir devido à forma como as cadeias de conexão são construídas e persistentes e o tipo de autenticação.
ms.date: 03/30/2017
ms.assetid: 1471f580-bcd4-4046-bdaf-d2541ecda2f4
ms.openlocfilehash: 0e693fd99384a2808a621b358f8e70c6777c3930
ms.sourcegitcommit: 33deec3e814238fb18a49b2a7e89278e27888291
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/02/2020
ms.locfileid: "84286644"
---
# <a name="protecting-connection-information"></a>Protegendo informações de conexão
A proteção do acesso à fonte de dados é essencial para a segurança do aplicativo. Uma cadeia de conexão apresenta uma vulnerabilidade potencial se não estiver protegida. Armazenar as informações de conexão em texto sem formatação ou persistir-la na memória pode comprometer seu sistema inteiro. As cadeias de conexão inseridas em seu código-fonte podem ser lidas usando o [ILDASM. exe (desmontador de Il)](../../tools/ildasm-exe-il-disassembler.md) para exibir o MSIL (Microsoft intermediator Language) em um assembly compilado.  
  
 As vulnerabilidades de segurança envolvendo cadeias de conexão podem surgir com base no tipo de autenticação usado, em como as cadeias de conexão são persistidas na memória e no disco e nas técnicas usadas para construí-los em tempo de execução.  
  
## <a name="use-windows-authentication"></a>Usar Autenticação do Windows  
 Para ajudar a limitar o acesso a sua fonte de dados, você deverá proteger as informações de conexão como a identificação do usuário, senha e nome da fonte de dados. Para evitar a exposição de informações do usuário, é recomendável usar a autenticação do Windows (às vezes conhecida como *segurança integrada*) sempre que possível. A autenticação do Windows é especificada em uma cadeia de conexão usando as palavras-chave `Integrated Security` ou `Trusted_Connection`, eliminando a necessidade de usar uma identificação de usuário e senha. Ao usar a autenticação do Windows, os usuários são autenticados pelo Windows e o acesso ao servidor e os recursos de banco de dados são determinados concedendo permissões a usuários e grupos do Windows.  
  
 Para situações onde não é possível usar a autenticação do Windows, você deverá ter cuidado adicional porque as credenciais do usuário são expostas na cadeia de conexão. Em um aplicativo do ASP.NET, você pode configurar uma conta do Windows como uma identidade fixa que é usada para se conectar a bancos de dados e outros recursos de rede. Você habilita a representação no elemento Identity no arquivo **Web. config** e especifica um nome de usuário e uma senha.  
  
```xml  
<identity impersonate="true"
        userName="MyDomain\UserAccount"
        password="*****" />  
```  
  
 A conta de identidade fixa deve ser uma conta de privilégios baixos que tem concedidas somente as permissões necessárias no banco de dados. Além disso, você deve criptografar o arquivo de configuração de modo que o nome de usuário e a senha não sejam expostos em texto não criptografado.  
  
## <a name="do-not-use-universal-data-link-udl-files"></a>Não use arquivos UDL (Universal Data Link)  
 Evite armazenar cadeias de conexão para um <xref:System.Data.OleDb.OleDbConnection> em um arquivo UDL (Universal Data Link). UDLs são armazenados em texto não criptografado e não podem ser criptografados. Um arquivo UDL é um recurso externo com base em arquivo para o seu aplicativo e ele não poderá ser protegido ou criptografado usando o .NET Framework.  
  
## <a name="avoid-injection-attacks-with-connection-string-builders"></a>Evite ataques de injeção com construtores de cadeia de conexão  
 Um ataque de injeção de cadeia de conexão pode ocorrer quando a concatenação dinâmica de cadeia de caracteres é usada para criar cadeias de conexão com base na entrada do usuário. Se a entrada do usuário não for validada e o texto mal-intencionado ou caracteres não forem escapados, um invasor poderá acessar dados confidenciais ou outros recursos no servidor. Para resolver esse problema, o ADO.NET 2.0 introduziu novas classes de construtor de cadeia de conexão para validar a sintaxe da cadeia de conexão e garantir que os parâmetros adicionais não sejam introduzidos. Para obter mais informações, confira [Construtores de cadeias de conexão](connection-string-builders.md).  
  
## <a name="use-persist-security-infofalse"></a>Use segurança de persistência Info=False  
 O valor padrão para `Persist Security Info` é falso; nós recomendamos usar esta opção em todas as cadeias de conexão. Configurar `Persist Security Info` como `true` ou `yes` permite informações confidenciais de segurança, incluindo a identificação de usuário e a senha, para serem obtidas de uma conexão depois que ela tiver sido aberta. Quando `Persist Security Info` for definido como `false` ou `no`, as informações de segurança serão descartadas após serem usadas para abrir a conexão, garantindo que uma fonte não confiável não tenha acesso a informações confidenciais de segurança.  
  
## <a name="encrypt-configuration-files"></a>Criptografe arquivos de configuração  
 Você também pode armazenar cadeias de conexão em arquivos de configuração, o que elimina a necessidade de inseri-las no código do aplicativo. Os arquivos de configuração são arquivos XML padrão para os quais o .NET Framework definiu um conjunto comum de elementos. As cadeias de conexão em arquivos de configuração normalmente são armazenadas dentro do **\<connectionStrings>** elemento no **app. config** de um aplicativo do Windows ou no arquivo **Web. config** de um aplicativo ASP.net. Para obter mais informações sobre as noções básicas de armazenamento, recuperação e criptografia de cadeias de conexão de arquivos de configuração, consulte [cadeias de conexão e arquivos de configuração](connection-strings-and-configuration-files.md).  
  
## <a name="see-also"></a>Veja também

- [Protegendo aplicativos ADO.NET](securing-ado-net-applications.md)
- [Criptografando informações de configuração usando a configuração protegida](https://docs.microsoft.com/previous-versions/aspnet/53tyfkaw(v=vs.100))
- [Segurança no .NET](../../../standard/security/index.md)
- [Visão geral do ADO.NET](ado-net-overview.md)
