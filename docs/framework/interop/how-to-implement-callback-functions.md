---
title: 'Como: Implementar funções de retorno de chamada'
description: Consulte como implementar funções de retorno de chamada. Neste exemplo, um aplicativo gerenciado, usando a invocação de plataforma, imprime o valor do identificador para cada janela em um computador.
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
- cpp
helpviewer_keywords:
- callback function, implementing
ms.assetid: e55b3712-b9ea-4453-bd9a-ad5cfa2f6bfa
ms.openlocfilehash: 31c657372e760c8d57f9714b20178967ad85fcd3
ms.sourcegitcommit: e02d17b2cf9c1258dadda4810a5e6072a0089aee
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/01/2020
ms.locfileid: "85619111"
---
# <a name="how-to-implement-callback-functions"></a>Como: Implementar funções de retorno de chamada
O procedimento e o exemplo a seguir demonstram como um aplicativo gerenciado, usando invocação de plataforma, pode imprimir o valor do identificador para cada janela no computador local. Especificamente, o procedimento e o exemplo usam a função **EnumWindows** para percorrer a lista de janelas e uma função de retorno de chamada gerenciada (chamada CallBack) para imprimir o valor do identificador da janela.  
  
### <a name="to-implement-a-callback-function"></a>Para implementar uma função de retorno de chamada  
  
1. Examine a assinatura para a função **EnumWindows** antes de continuar com a implementação. **EnumWindows** tem a seguinte assinatura:  
  
    ```cpp
    BOOL EnumWindows(WNDENUMPROC lpEnumFunc, LPARAM lParam)
    ```
  
     Uma indicação de que essa função requer um retorno de chamada é a presença do argumento **lpEnumFunc**. É comum ver o prefixo **lp** (ponteiro longo) combinado com o sufixo **Func** no nome de argumentos que levam um ponteiro para uma função de retorno de chamada. Para obter a documentação sobre funções do Win32, consulte o SDK da Plataforma Microsoft.  
  
2. Crie a função de retorno de chamada gerenciada. O exemplo declara um tipo de delegado chamado `CallBack` que leva dois argumentos (**hwnd** e **lparam**). O primeiro argumento é um identificador para a janela, enquanto o segundo argumento é definido pelo aplicativo. Nesta versão, ambos os argumentos devem ser números inteiros.  
  
     Funções de retorno de chamada normalmente retornam valores diferentes de zero para indicar êxito e zero para indicar falha. Este exemplo define explicitamente o valor retornado para **true** para continuar a enumeração.  
  
3. Crie um delegado e passe-o como um argumento para a função **EnumWindows**. A invocação de plataforma converte o delegado em um formato de retorno de chamada familiar automaticamente.  
  
4. Verifique se o coletor de lixo não recupera o delegado antes da função de retorno de chamada terminar seu trabalho. Quando você passa um delegado como um parâmetro ou passa um delegado contido como um campo em uma estrutura, ele permanece não coletado pela a duração da chamada. Portanto, como é o caso no exemplo de enumeração a seguir, a função de retorno de chamada termina o trabalho antes da chamada retornar e não requer nenhuma ação adicional pelo chamador gerenciado.  
  
     Se, no entanto, a função de retorno de chamada puder ser chamada depois que a chamada retornar, o chamador gerenciado deverá tomar medidas para garantir que o delegado permaneça não coletado até que a função de retorno de chamada seja concluída. Para obter informações detalhadas sobre como evitar a coleta de lixo, consulte [Marshaling de interoperabilidade](interop-marshaling.md) com invocação de plataforma.  
  
## <a name="example"></a>Exemplo  
  
```vb  
Imports System  
Imports System.Runtime.InteropServices  
  
Public Delegate Function CallBack( _  
hwnd As Integer, lParam As Integer) As Boolean  
  
Public Class EnumReportApp  
  
    Declare Function EnumWindows Lib "user32" ( _  
       x As CallBack, y As Integer) As Integer  
  
    Public Shared Sub Main()  
        EnumWindows(AddressOf EnumReportApp.Report, 0)  
    End Sub 'Main  
  
    Public Shared Function Report(hwnd As Integer, lParam As Integer) _  
    As Boolean  
        Console.Write("Window handle is ")  
        Console.WriteLine(hwnd)  
        Return True  
    End Function 'Report  
End Class 'EnumReportApp  
```  
  
```csharp  
using System;  
using System.Runtime.InteropServices;  
  
public delegate bool CallBack(int hwnd, int lParam);  
  
public class EnumReportApp  
{  
    [DllImport("user32")]  
    public static extern int EnumWindows(CallBack x, int y);
  
    public static void Main()
    {  
        CallBack myCallBack = new CallBack(EnumReportApp.Report);  
        EnumWindows(myCallBack, 0);  
    }  
  
    public static bool Report(int hwnd, int lParam)  
    {
        Console.Write("Window handle is ");  
        Console.WriteLine(hwnd);  
        return true;  
    }  
}  
```  
  
```cpp  
using namespace System;  
using namespace System::Runtime::InteropServices;  
  
// A delegate type.  
delegate bool CallBack(int hwnd, int lParam);  
  
// Managed type with the method to call.  
ref class EnumReport  
{  
// Report the window handle.  
public:  
    [DllImport("user32")]  
    static int EnumWindows(CallBack^ x, int y);  
  
    static void Main()  
    {  
        EnumReport^ er = gcnew EnumReport;  
        CallBack^ myCallBack = gcnew CallBack(&EnumReport::Report);  
        EnumWindows(myCallBack, 0);  
    }  
  
    static bool Report(int hwnd, int lParam)  
    {  
       Console::Write(L"Window handle is ");  
       Console::WriteLine(hwnd);  
       return true;  
    }  
};  
  
int main()  
{  
    EnumReport::Main();  
}  
```  
  
## <a name="see-also"></a>Confira também

- [Funções de retorno de chamada](callback-functions.md)
- [Chamando uma função de DLL](calling-a-dll-function.md)
