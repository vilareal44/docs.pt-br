---
title: 'Passo a passo: Persistindo um objeto usando o C#'
description: Este exemplo cria um objeto de empréstimo básico em C# e mantém seus dados em um arquivo e, em seguida, cria um novo objeto com dados do arquivo.
ms.date: 04/26/2018
ms.openlocfilehash: 9f165addc5b9b0d056936458e8529ec1912c417b
ms.sourcegitcommit: 6f58a5f75ceeb936f8ee5b786e9adb81a9a3bee9
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/28/2020
ms.locfileid: "87302757"
---
# <a name="walkthrough-persisting-an-object-using-c"></a>Passo a passo: persistir um objeto usando o C\#

Você pode usar a serialização para manter os dados de um objeto entre instâncias, o que permite armazenar valores e recuperá-los na próxima vez que o objeto for instanciado.

Neste passo a passo, você criará um objeto `Loan` básico e persistirá seus dados em um arquivo. Em seguida, você recuperará os dados do arquivo quando recriar o objeto.

> [!IMPORTANT]
> Este exemplo criará um novo arquivo se o arquivo ainda não existir. Se um aplicativo precisar criar um arquivo, esse aplicativo precisará ter a permissão `Create` para a pasta. Permissões são definidas usando listas de controle de acesso. Se o arquivo já existir, o aplicativo precisará somente da permissão `Write`, que é uma permissão menor. Sempre que possível, é mais seguro criar o arquivo durante a implantação e somente conceder permissões `Read` a um único arquivo (em vez das permissões Create para uma pasta). Além disso, é mais seguro gravar dados em pastas de usuário do que na pasta raiz ou na pasta Arquivos de Programas.

> [!IMPORTANT]
> Este exemplo armazena dados em um arquivo de formato binário. Esses formatos não devem ser usados para dados confidenciais, como senhas ou informações de cartão de crédito.

## <a name="prerequisites"></a>Pré-requisitos

- Para criar e executar, instale o [SDK do .NET Core](https://dotnet.microsoft.com/download).

- Instale seu editor de código favorito, caso ainda não tenha um.

> [!TIP]
> Precisa instalar um editor de código? Experimente o [Visual Studio](https://visualstudio.com/downloads)!

- O exemplo exige C# 7.3. Confira [Selecionar a versão da linguagem C#](../../../language-reference/configure-language-version.md)

Examine o código de exemplo online [no repositório GitHub de amostras do .NET](https://github.com/dotnet/samples/tree/master/csharp/serialization).

## <a name="creating-the-loan-object"></a>Criando o objeto Loan

A primeira etapa é criar uma classe `Loan` e um aplicativo de console que usa a classe:

1. Crie um novo aplicativo. Digite `dotnet new console -o serialization` para criar um novo aplicativo de console em um subdiretório chamado `serialization`.
1. Abra o aplicativo no editor e adicione uma nova classe chamada `Loan.cs`.
1. Adicione o seguinte código à classe `Loan`:

[!code-csharp[Loan class definition](../../../../../samples/snippets/csharp/serialization/Loan.cs#1)]

Você também precisará criar um aplicativo que usa a classe `Loan`.

## <a name="serialize-the-loan-object"></a>Serializar o objeto Loan

1. Abra o `Program.cs`. Adicione o seguinte código:

[!code-csharp[Create a loan object](../../../../../samples/snippets/csharp/serialization/Program.cs#1)]

Adicione um manipulador de eventos ao evento `PropertyChanged` e algumas linhas para modificar o objeto `Loan` e exibir as alterações. Veja as adições no seguinte código:

[!code-csharp[Listening for the PropertyChanged event](../../../../../samples/snippets/csharp/serialization/Program.cs#2)]

Neste ponto, você pode executar o código e ver a saída atual:

```console
New customer value: Henry Clay
7.5
7.1
```

A execução desse aplicativo repetidamente grava os mesmos valores. Um novo objeto Loan é criado sempre que o programa é executado. No mundo real, as taxas de juros mudam periodicamente, mas não necessariamente toda vez que o aplicativo for executado. Código de serialização significa preservar a taxa de juros mais recente entre instâncias do aplicativo. Na próxima etapa, você fará exatamente isso adicionando a serialização à classe Loan.

## <a name="using-serialization-to-persist-the-object"></a>Usando a serialização para manter o objeto

Para manter os valores da classe Loan, primeiro você deve marcar a classe com o atributo `Serializable`. Adicione o seguinte código acima da definição da classe Loan:

[!code-csharp[Loan class definition](../../../../../samples/snippets/csharp/serialization/Loan.cs#2)]

O <xref:System.SerializableAttribute> informa ao compilador que tudo na classe pode ser persistido em um arquivo. Como o evento `PropertyChanged` não representa a parte do grafo do objeto que deve ser armazenada, ele não deve ser serializado. Ao fazer isso, todos os objetos anexados a esse evento serão serializados. Adicione o <xref:System.NonSerializedAttribute> à declaração de campo do manipulador de eventos `PropertyChanged`.

[!code-csharp[Disable serialization for the event handler](../../../../../samples/snippets/csharp/serialization/Loan.cs#3)]

Do C# 7.3 em diante, você pode anexar atributos ao campo de suporte de uma propriedade autoimplementada usando o valor de destino `field`. O seguinte código adiciona uma propriedade `TimeLastLoaded` e a marca como não serializável:

[!code-csharp[Disable serialization for an auto-implemented property](../../../../../samples/snippets/csharp/serialization/Loan.cs#4)]

A etapa seguinte é adicionar o código de serialização ao aplicativo LoanApp. Para serializar a classe e gravá-la em um arquivo, use os namespaces <xref:System.IO> e <xref:System.Runtime.Serialization.Formatters.Binary>. Para evitar a digitação dos nomes totalmente qualificados, você pode adicionar referências aos namespaces necessários, conforme mostrado no seguinte código:

[!code-csharp[Adding namespaces for serialization](../../../../../samples/snippets/csharp/serialization/Program.cs#3)]

A próxima etapa é adicionar código para desserializar o objeto do arquivo quando o objeto for criado. Adicione uma constante à classe do nome de arquivo dos dados serializados, conforme mostrado no seguinte código:

[!code-csharp[Define the name of the saved file](../../../../../samples/snippets/csharp/serialization/Program.cs#4)]

Em seguida, adicione o seguinte código após a linha que cria o objeto `TestLoan`:

[!code-csharp[Read from a file if it exists](../../../../../samples/snippets/csharp/serialization/Program.cs#5)]

Primeiro, é necessário verificar se o arquivo existe. Se ele existir, crie uma classe <xref:System.IO.Stream> para ler o arquivo binário e uma classe <xref:System.Runtime.Serialization.Formatters.Binary.BinaryFormatter> para converter o arquivo. Você também precisa converter do tipo de fluxo para o tipo de objeto Loan.

Em seguida, é necessário adicionar um código para serializar a classe em um arquivo. Adicione o seguinte código após o código existente no método `Main`:

[!code-csharp[Save the existing Loan object](../../../../../samples/snippets/csharp/serialization/Program.cs#6)]

Neste ponto, você pode compilar e executar o aplicativo novamente. Na primeira vez em que ele é executado, observe que as taxas de juros começam em 7,5 e, em seguida, são alteradas para 7,1. Feche o aplicativo e execute-o novamente. Agora, o aplicativo imprime a mensagem indicando que ele leu o arquivo salvo e a taxa de juros é 7,1, mesmo antes do código que a altera.

## <a name="see-also"></a>Veja também

- [Serialização (C#)](index.md)
- [Guia de programação C#](../../index.md)
