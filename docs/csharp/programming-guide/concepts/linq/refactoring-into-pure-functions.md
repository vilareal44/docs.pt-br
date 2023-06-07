---
title: Refatoração em funções puras (C#)
description: Saiba como refatorar o código usando funções puras. Consulte exemplos de código e exiba recursos adicionais disponíveis.
ms.date: 07/20/2015
ms.assetid: 2944a0d4-fd33-4e2e-badd-abb0f9be2fcc
ms.openlocfilehash: cc5dd26923e2edaed34c8f1b742b3dfa1e935e68
ms.sourcegitcommit: 6f58a5f75ceeb936f8ee5b786e9adb81a9a3bee9
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/28/2020
ms.locfileid: "87300222"
---
# <a name="refactoring-into-pure-functions-c"></a>Refatoração em funções puras (C#)

Um aspecto importante de transformações e puras é aprender como o código do refatorar usando funções puras.  
  
> [!NOTE]
> A nomenclatura comuns em programação funcional é que você refatora programa usando funções puras. No Visual Basic e C++, isso alinha com o uso das funções em linguagens respectivos. No entanto, em C#, as funções são chamadas métodos. Para fins desta discussão, uma função pura é implementada como um método em C#.  
  
 Conforme observado anteriormente nesta seção, uma função pura tem duas características úteis:  
  
- Não tem efeito colateral. A função não altera quaisquer variáveis ou os dados de qualquer tipo fora da função.  
  
- É consistente. Dado o mesmo conjunto de dados de entrada, sempre retornará o mesmo valor de saída.  
  
 Uma maneira de fazer a transição para programação funcional é o código existente do refatorar para eliminar efeitos colaterais desnecessários e dependências externas. Dessa maneira, você pode criar versões puras de função do código existente.  
  
 Este tópico descreve o que é uma função pura e o que não é. O tutorial [Tutorial: manipulando conteúdo em um documento WordprocessingML](./shape-of-wordprocessingml-documents.md) mostra como manipular um documento WordprocessingML e inclui dois exemplos de como refatorar usando uma função pura.  
  
## <a name="eliminating-side-effects-and-external-dependencies"></a>Eliminando efeitos colaterais e dependências externas  
 Os seguintes exemplos contrastam duas funções não puras e uma função pura.  
  
### <a name="non-pure-function-that-changes-a-class-member"></a>Função não pura que altera um membro de classe  
 No código a seguir, a função de `HyphenatedConcat` não é uma função pura, porque altera o membro de dados `aMember` na classe:  
  
```csharp  
public class Program  
{  
    private static string aMember = "StringOne";  
  
    public static void HyphenatedConcat(string appendStr)  
    {  
        aMember += '-' + appendStr;  
    }  
  
    public static void Main()  
    {  
        HyphenatedConcat("StringTwo");  
        Console.WriteLine(aMember);  
    }  
}  
```  
  
 Este código produz a seguinte saída:  
  
```output  
StringOne-StringTwo  
```  
  
 Observe que é irrelevante se os dados modificados têm acesso a `public` ou `private`, ou são um membro de `static` ou um membro de instância. Uma função pura não altera quaisquer dados fora da função.  
  
### <a name="non-pure-function-that-changes-an-argument"></a>Função não pura que altera um argumento  
 Além disso, a seguinte versão dessa mesma função não é pura porque modifica o conteúdo do seu parâmetro, `sb`.  
  
```csharp  
public class Program  
{  
    public static void HyphenatedConcat(StringBuilder sb, String appendStr)  
    {  
        sb.Append('-' + appendStr);  
    }  
  
    public static void Main()  
    {  
        StringBuilder sb1 = new StringBuilder("StringOne");  
        HyphenatedConcat(sb1, "StringTwo");  
        Console.WriteLine(sb1);  
    }  
}  
```  
  
 Esta versão do programa gerenciar as mesmas saída que a primeira versão, porque a função de `HyphenatedConcat` alterou o valor (estado) do primeiro parâmetro chamar a função de membro de <xref:System.Text.StringBuilder.Append%2A> . Observe que essa mudança ocorre independentemente do fato de que `HyphenatedConcat` usa passar o parâmetro de atendimento-por- valor.  
  
> [!IMPORTANT]
> Para tipos de referência, se você passa um parâmetro por valor, ele resulta em uma cópia de referência a um objeto que está sendo passado. Esta cópia ainda está associada com os mesmos dados de instância que a referência original (até que a variável de referência é atribuído a um novo objeto). a Atendimento-por- referência não necessariamente é necessária para uma função modifique um parâmetro.  
  
### <a name="pure-function"></a>Função pura  
Esta próxima versão do programa mostra como implementar a função `HyphenatedConcat` como uma função pura.  
  
```csharp  
class Program  
{  
    public static string HyphenatedConcat(string s, string appendStr)  
    {  
        return (s + '-' + appendStr);  
    }  
  
    public static void Main(string[] args)  
    {  
        string s1 = "StringOne";  
        string s2 = HyphenatedConcat(s1, "StringTwo");  
        Console.WriteLine(s2);  
    }  
}  
```  
  
 Além disso, esta versão gerencia a mesma linha de saída: `StringOne-StringTwo`. Observe que para manter o valor concatenado, ele é armazenado em `s2`variável intermediária.  
  
 Uma abordagem que pode ser muito útil é escrever as funções que são localmente impuros (isto é, declare e alteram variáveis locais) mas é global pura. Tais funções têm muitas das características desejáveis de composability, mas impedem alguns de linguagem e mais complicados de programação, como ter que usar a recursão quando um loop simples realizaria a mesma coisa.  
  
## <a name="standard-query-operators"></a>Operadores de consulta padrão  
 Uma característica importante dos operadores de consulta padrão é que são implementados como funções puras.  
  
 Para obter mais informações, consulte [Visão geral de operadores de consulta padrão (C#)](./standard-query-operators-overview.md).  
  
## <a name="see-also"></a>Veja também

- [Introdução às transformações funcionais puras (C#)](./introduction-to-pure-functional-transformations.md)
- [Programação funcional versus programação imperativa (C#)](./functional-programming-vs-imperative-programming.md)
