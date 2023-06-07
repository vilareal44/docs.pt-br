---
title: Execução lado a lado em processo
description: Use a hospedagem no processo lado a lado para executar muitas versões do Common Language Runtime (CLR) em um único processo .NET.
ms.date: 03/30/2017
helpviewer_keywords:
- in-process side-by-side execution
- side-by-side execution, in-process
ms.assetid: 18019342-a810-4986-8ec2-b933a17c2267
ms.openlocfilehash: 078f2eaada8fac57138bef22d46218ef2ccda835
ms.sourcegitcommit: e02d17b2cf9c1258dadda4810a5e6072a0089aee
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/01/2020
ms.locfileid: "85622595"
---
# <a name="in-process-side-by-side-execution"></a>Execução lado a lado em processo
A partir do .NET Framework 4, você pode usar a hospedagem lado a lado em processo para executar várias versões do CLR (Common Language Runtime) em um único processo. Por padrão, os componentes COM gerenciados são executados com a versão do .NET Framework com a qual eles foram criados, independentemente da versão do .NET Framework carregada para o processo.  
  
## <a name="background"></a>Segundo plano  
 O .NET Framework sempre forneceu a hospedagem lado a lado para aplicativos de código gerenciado, mas antes do .NET Framework 4, ele não fornecia essa funcionalidade para componentes COM gerenciados. No passado, os componentes COM gerenciados que eram carregados em um processo eram executados com a versão do runtime que já estava carregada ou com a versão mais recente instalada do .NET Framework. Se essa versão não era compatível com o componente COM, ele falhava.  
  
 O .NET Framework 4 fornece uma nova abordagem para hospedagem lado a lado que garante o seguinte:  
  
- Instalar uma nova versão do .NET Framework não tem nenhum efeito em aplicativos existentes.  
  
- Os aplicativos são executados na versão do .NET Framework com a qual foram criados. Eles não usar a nova versão do .NET Framework, exceto quando expressamente determinado para fazer isso. No entanto, é mais fácil para os aplicativos fazerem a transição para usar uma nova versão do .NET Framework.  
  
## <a name="effects-on-users-and-developers"></a>Efeitos em usuários e desenvolvedores  
  
- **Usuários finais e administradores de sistema**. Esses usuários agora podem ter uma confiança maior de que quando instalarem uma nova versão do runtime, independentemente ou com um aplicativo, ela não terá impacto em seus computadores. Os aplicativos existentes continuarão a ser executados como eram antes.  
  
- **Desenvolvedores de aplicativos**. A hospedagem lado a lado quase não tem efeito sobre os desenvolvedores de aplicativos. Por padrão, os aplicativos sempre são executados na versão do .NET Framework em que foram criados, isso não mudou. No entanto, os desenvolvedores podem substituir esse comportamento e direcionar o aplicativo para ser executado em uma versão mais recente do .NET Framework (consulte o [cenário 2](#scenarios)).  
  
- **Desenvolvedores de bibliotecas e consumidores**. A hospedagem lado a lado não resolve os problemas de compatibilidade que os desenvolvedores de bibliotecas enfrentam. Uma biblioteca que é carregada diretamente por um aplicativo, por meio de uma referência direta ou uma chamada <xref:System.Reflection.Assembly.Load%2A?displayProperty=nameWithType>, continua a usar o runtime do <xref:System.AppDomain> em que foi carregada. Você deve testar suas bibliotecas em todas as versões do .NET Framework para as quais você deseja dar suporte. Se um aplicativo for compilado usando o runtime do .NET Framework 4, mas incluir uma biblioteca que foi criada usando um runtime anterior, essa biblioteca também usará o runtime do .NET Framework 4. No entanto, se você tiver um aplicativo que foi criado usando um runtime mais recente e a biblioteca que foi criada usando o .NET Framework 4, será necessário forçar o aplicativo a também usar o .NET Framework 4 (consulte o [cenário 3](#scenarios)).  
  
- **Desenvolvedores de componente COM gerenciado**. No passado, componentes COM gerenciados eram executados automaticamente usando a versão mais recente do runtime instalado no computador. Agora você pode executar componentes COM na versão do runtime em que eles foram criados.  
  
     Conforme mostrado pela tabela a seguir, os componentes que foram criados com o .NET Framework versão 1.1 podem ser executados lado a lado com os componentes da versão 4, mas não podem ser executados com componentes da versão 2.0, 3.0 ou 3.5, pois a hospedagem lado a lado não está disponível para essas versões.  
  
    |Versão do .NET Framework|1,1|2.0 – 3.5|4|  
    |----------------------------|---------|----------------|-------|  
    |1,1|Não Aplicável|Não|Sim|  
    |2.0 – 3.5|Não|Não aplicável|Sim|  
    |4|Sim|Sim|Não Aplicável|  
  
> [!NOTE]
> As versões 3.0 e 3.5 do .NET Framework são criadas de forma incremental na versão 2.0 e não precisam ser executadas lado a lado. Elas são inerentemente a mesma versão.  
  
<a name="scenarios"></a>
## <a name="common-side-by-side-hosting-scenarios"></a>Cenários comuns de hospedagem lado a lado  
  
- **Cenário 1:** aplicativo nativo que usa componentes COM criados com versões anteriores do .NET Framework.  
  
     .NET Framework versões instaladas: a .NET Framework 4 e todas as outras versões do .NET Framework usadas pelos componentes COM.  
  
     O que fazer: nesse cenário, não faça nada. Os componentes COM serão executados com a versão do .NET Framework com a qual foram registrados.  
  
- **Cenário 2**: aplicativo gerenciado criado com o .NET Framework 2,0 SP1 que você prefere executar com o .NET Framework 2,0, mas que está disposto a ser executado no .NET Framework 4 se a versão 2,0 não estiver presente.  
  
     .NET Framework versões instaladas: uma versão anterior do .NET Framework e o .NET Framework 4.  
  
     O que fazer: no [arquivo de configuração do aplicativo](../configure-apps/index.md) no diretório do aplicativo, use o [ \<startup> elemento](../configure-apps/file-schema/startup/startup-element.md) e o [ \<supportedRuntime> elemento](../configure-apps/file-schema/startup/supportedruntime-element.md) definido da seguinte maneira:  
  
    ```xml  
    <configuration>  
      <startup >  
        <supportedRuntime version="v2.0.50727" />  
        <supportedRuntime version="v4.0" />  
      </startup>  
    </configuration>  
    ```  
  
- **Cenário 3:** Aplicativo nativo que usa componentes COM criados com versões anteriores do .NET Framework que você deseja executar com o .NET Framework 4.  
  
     .NET Framework versões instaladas: o .NET Framework 4.  
  
     O que fazer: no arquivo de configuração de aplicativo no diretório do aplicativo, use o elemento `<startup>` com o atributo `useLegacyV2RuntimeActivationPolicy` definido como `true` e o elemento `<supportedRuntime>` definido da seguinte forma:  
  
    ```xml  
    <configuration>  
      <startup useLegacyV2RuntimeActivationPolicy="true">  
        <supportedRuntime version="v4.0" />  
      </startup>  
    </configuration>  
    ```  
  
## <a name="example"></a>Exemplo  
 O exemplo a seguir demonstra um host COM não gerenciado que está executando um componente COM gerenciado usando a versão do .NET Framework que o componente foi compilado para usar.  
  
 Para executar o exemplo a seguir, compile e registre o seguinte componente COM gerenciado usando o .NET Framework 3.5. Para registrar o componente, no menu **Projeto**, clique em **Propriedades**, clique na guia **Compilar** e marque a caixa de seleção **Registrar para interoperabilidade COM**.  
  
```csharp
using System;  
using System.Collections.Generic;  
using System.Linq;  
using System.Text;  
using System.Runtime.InteropServices;  
  
namespace BasicComObject  
{  
    [ComVisible(true), Guid("9C99C4B5-CA54-4c58-8988-49B6811BA53B")]  
    public class MyObject : SimpleObjectModel.IPrintInfo  
    {  
        public MyObject()  
        {  
        }  
        public void PrintInfo()  
        {  
            Console.WriteLine("MyObject was activated in {0} runtime in:\n\tAppDomain {1}:{2}", System.Runtime.InteropServices.RuntimeEnvironment.GetSystemVersion(), AppDomain.CurrentDomain.Id, AppDomain.CurrentDomain.FriendlyName);  
        }  
    }  
}  
```  
  
 Compile o seguinte aplicativo C++ não gerenciado, que ativa o objeto COM criado pelo exemplo anterior.  
  
```cpp
#include "stdafx.h"  
#include <string>  
#include <iostream>  
#include <objbase.h>  
#include <string.h>  
#include <process.h>  
  
using namespace std;  
  
int _tmain(int argc, _TCHAR* argv[])  
{  
    char input;  
    CoInitialize(NULL) ;  
    CLSID clsid;  
    HRESULT hr;  
    HRESULT clsidhr = CLSIDFromString(L"{9C99C4B5-CA54-4c58-8988-49B6811BA53B}",&clsid);  
    hr = -1;  
    if (FAILED(clsidhr))  
    {  
        printf("Failed to construct CLSID from String\n");  
    }  
    UUID id = __uuidof(IUnknown);  
    IUnknown * pUnk = NULL;  
    hr = ::CoCreateInstance(clsid,NULL,CLSCTX_INPROC_SERVER,id,(void **) &pUnk);  
    if (FAILED(hr))  
    {  
        printf("Failed CoCreateInstance\n");  
    }else  
    {  
        pUnk->AddRef();  
        printf("Succeeded\n");  
    }  
  
    DISPID dispid;  
    IDispatch* pPrintInfo;  
    pUnk->QueryInterface(IID_IDispatch, (void**)&pPrintInfo);  
    OLECHAR FAR* szMethod[1];  
    szMethod[0]=OLESTR("PrintInfo");
    hr = pPrintInfo->GetIDsOfNames(IID_NULL,szMethod, 1, LOCALE_SYSTEM_DEFAULT, &dispid);  
    DISPPARAMS dispparams;  
    dispparams.cNamedArgs = 0;  
    dispparams.cArgs = 0;  
    VARIANTARG* pvarg = NULL;  
    EXCEPINFO * pexcepinfo = NULL;  
    WORD wFlags = DISPATCH_METHOD ;  
;  
    LPVARIANT pvRet = NULL;  
    UINT * pnArgErr = NULL;  
    hr = pPrintInfo->Invoke(dispid,IID_NULL, LOCALE_USER_DEFAULT, wFlags,  
        &dispparams, pvRet, pexcepinfo, pnArgErr);  
    printf("Press Enter to exit");  
    scanf_s("%c",&input);  
    CoUninitialize();  
    return 0;  
}  
```  
  
## <a name="see-also"></a>Consulte também

- [\<startup>Elementos](../configure-apps/file-schema/startup/startup-element.md)
- [\<supportedRuntime>Elementos](../configure-apps/file-schema/startup/supportedruntime-element.md)
