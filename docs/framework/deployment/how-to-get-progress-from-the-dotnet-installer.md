---
title: Como acompanhar o progresso do Instalador do .NET Framework 4.5
description: Saiba como obter o progresso do instalador do .NET 4,5. Se você desenvolver aplicativos para esta versão do .NET, poderá incluir a instalação do (Chain) .NET 4,5 na instalação do seu aplicativo.
ms.date: 03/30/2017
dev_langs:
- cpp
helpviewer_keywords:
- progress information, .NET Framework installer
- .NET Framework, installing
ms.assetid: 0a1a3ba3-7e46-4df2-afd3-f3a8237e1c4f
ms.openlocfilehash: 501fcaa7636d586ddfff8606768d4639fdc010d7
ms.sourcegitcommit: 3824ff187947572b274b9715b60c11269335c181
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/17/2020
ms.locfileid: "84904254"
---
# <a name="how-to-get-progress-from-the-net-framework-45-installer"></a>Como acompanhar o progresso do Instalador do .NET Framework 4.5

O .NET Framework 4.5 é um runtime redistribuível. Se você desenvolver aplicativos para esta versão do .NET Framework, poderá incluir (encadear) a instalação do .NET Framework 4.5 como uma parte de pré-requisito da instalação do seu aplicativo. Para apresentar uma experiência de instalação personalizada ou unificada, talvez você queira iniciar silenciosamente a instalação do .NET Framework 4.5 e acompanhar seu progresso, mostrando simultaneamente o progresso da instalação do aplicativo. Para habilitar o acompanhamento silencioso, a instalação do .NET Framework 4.5 (que pode ser inspecionada) define um protocolo usando uma MMIO (E/S mapeada em memória) para se comunicar com a instalação (o observador ou encadeador). Esse protocolo define uma maneira para um encadeador obter informações sobre o progresso, obter resultados detalhados, responder às mensagens e cancelar a instalação do .NET Framework 4.5.

- **Invocação**. Para chamar a instalação do .NET Framework 4.5 e receber informações sobre o progresso da seção da MMIO, seu programa de instalação deve fazer o seguinte:

    1. Chame o programa redistribuível do .NET Framework 4,5:

        `dotNetFx45_Full_x86_x64.exe /q /norestart /pipe section-name`

        Em que o *nome da seção* é qualquer nome que você deseja usar para identificar seu aplicativo. A instalação do .NET Framework lê e grava na seção MMIO assincronamente, portanto, talvez seja conveniente usar eventos e mensagens durante esse período. No exemplo, o processo de instalação do .NET Framework é criado por um construtor que aloca a seção MMIO (`TheSectionName`) e define um evento (`TheEventName`):

        ```cpp
        Server():ChainerSample::MmioChainer(L"TheSectionName", L"TheEventName")
        ```

        Substitua esses nomes por nomes que são exclusivos para seu programa de instalação.

    2. Leia a seção MMIO. No .NET Framework 4,5, as operações de download e instalação são simultâneas: uma parte do .NET Framework pode estar sendo instalada enquanto outra parte está sendo baixada. Como resultado, o progresso é enviado novamente (isto é, gravado) para a seção MMIO como dois números (`m_downloadSoFar` e `m_installSoFar`) que aumentam de 0 a 255. Quando 255 é gravado e o .NET Framework sai, a instalação está concluída.

- **Códigos de saída**. Os códigos de saída a seguir do comando para chamar o programa redistribuível .NET Framework 4.5 indicam se a instalação teve êxito ou falhou:

  - 0 – Instalação concluída com êxito.

  - 3010 – Instalação concluída com êxito. É necessário reiniciar o sistema.

  - 1602 – A instalação foi cancelada.

  - Todos os outros códigos – A instalação encontrou erros. Examine os arquivos de log criados em %temp% para obter detalhes.

- **Cancelando a instalação**. Você pode cancelar a instalação a qualquer momento usando o método `Abort` para definir os sinalizadores `m_downloadAbort` e `m_ installAbort` na seção MMIO.

## <a name="chainer-sample"></a>Exemplo de encadeador

O exemplo de encadeador inicializa e controla silenciosamente a instalação do .NET Framework 4.5 enquanto mostra o progresso. Este exemplo é semelhante ao exemplo de encadeador fornecido para o .NET Framework 4. No entanto, além disso, ele pode evitar reinicializações do sistema processando a caixa de mensagem para fechar os aplicativos do .NET Framework 4. Para obter informações sobre essa caixa de mensagens, consulte [Redução de reinicializações do sistema durante instalações do .NET Framework 4.5](reducing-system-restarts.md). Você pode usar este exemplo com o instalador do .NET Framework 4. Nesse cenário, a mensagem simplesmente não é enviada.

> [!WARNING]
> Você deve executar o exemplo como um administrador.

Você pode baixar a solução completa do Visual Studio para o [.NET Framework 4.5 Chainer Sample](https://code.msdn.microsoft.com/NET-Framework-45-Developer-e416a0ba) na Galeria de Exemplos do MSDN.

As seções a seguir descrevem os arquivos significativos neste exemplo: MMIOChainer.h, ChainingdotNet4.cpp e IProgressObserver.h.

#### <a name="mmiochainerh"></a>MMIOChainer.h

- O arquivo MMIOChainer.h (consulte o [código completo](https://code.msdn.microsoft.com/NET-Framework-45-Developer-e416a0ba/sourcecode?fileId=47345&pathId=663039622)) contém a definição da estrutura de dados e a classe base da qual a classe do encadeador deve ser derivada. O .NET Framework 4.5 estende a estrutura de dados MMIO para lidar com os dados que instalador do .NET Framework 4.5 precisa. As alterações na estrutura MMIO têm compatibilidade com versões anteriores, portanto, um encadeador do .NET Framework 4 pode funcionar com a instalação do .NET Framework 4.5 sem a necessidade de recompilação. No entanto, esse cenário não dá suporte ao recurso para reduzir as reinicializações do sistema.

    Um campo de versão fornece um meio de identificar as revisões do formato de mensagem e de estrutura. A instalação do .NET Framework determina a versão da interface do encadeador chamando a função `VirtualQuery` para determinar o tamanho do mapeamento de arquivo. Se o tamanho for grande o suficiente para acomodar o campo de versão, a instalação do .NET Framework usa o valor especificado. Se o mapeamento de arquivo for muito pequeno para conter um campo de versão, que é o caso com o .NET Framework 4, o processo de instalação assume a versão 0 (4). Se o encadeador não dá suporte à versão da mensagem que o instalador do .NET Framework deseja enviar, a instalação do .NET Framework assume uma resposta de ignorar.

    A estrutura de dados MMIO é definida da seguinte maneira:

    ```cpp
    // MMIO data structure for interprocess communication
        struct MmioDataStructure
        {
            bool m_downloadFinished;               // Is download complete?
            bool m_installFinished;                // Is installation complete?
            bool m_downloadAbort;                  // Set to cause downloader to abort.
            bool m_installAbort;                   // Set to cause installer to abort.
            HRESULT m_hrDownloadFinished;          // Resulting HRESULT for download.
            HRESULT m_hrInstallFinished;           // Resulting HRESULT for installation.
            HRESULT m_hrInternalError;
            WCHAR m_szCurrentItemStep[MAX_PATH];
            unsigned char m_downloadSoFar;         // Download progress 0-255 (0-100% done).
            unsigned char m_installSoFar;          // Installation progress 0-255 (0-100% done).
            WCHAR m_szEventName[MAX_PATH];         // Event that chainer creates and chainee opens to sync communications.

            BYTE m_version;                        // Version of the data structure, set by chainer:
                                                   // 0x0: .NET Framework 4
                                                   // 0x1: .NET Framework 4.5

            DWORD m_messageCode;                   // Current message sent by the chainee; 0 if no message is active.
            DWORD m_messageResponse;               // Chainer's response to current message; 0 if not yet handled.
            DWORD m_messageDataLength;             // Length of the m_messageData field, in bytes.
            BYTE m_messageData[1];                 // Variable-length buffer; content depends on m_messageCode.
        };
    ```

- A estrutura de dados `MmioDataStructure` não deve ser usada diretamente, use a classe `MmioChainer` em vez disso para implementar o encadeador. Derive da classe `MmioChainer` para encadear o .NET Framework 4.5 redistribuível.

#### <a name="iprogressobserverh"></a>IProgressObserver.h

- O arquivo IProgressObserver.h implementa um observador de progresso ([veja o código completo](https://code.msdn.microsoft.com/NET-Framework-45-Developer-e416a0ba/sourcecode?fileId=47345&pathId=1263700592)). Este observador é notificado sobre o progresso do download e da instalação (especificado como um `char` sem sinal, 0 a 255, que indica 1 a 100% concluído). O observador também é notificado quando o encadeador envia uma mensagem e o observador deve enviar uma resposta.

    ```cpp
        class IProgressObserver
        {
        public:
            virtual void OnProgress(unsigned char) = 0; // 0 - 255:  255 == 100%
            virtual void Finished(HRESULT) = 0;         // Called when operation is complete
            virtual DWORD Send(DWORD dwMessage, LPVOID pData, DWORD dwDataLength) = 0; // Called when a message is sent
        };
    ```

#### <a name="chainingdotnet45cpp"></a>ChainingdotNet4.5.cpp

- O arquivo [ChainingdotNet4.5.cpp](https://code.msdn.microsoft.com/NET-Framework-45-Developer-e416a0ba/sourcecode?fileId=47345&pathId=1757268882) implementa a classe `Server`, que deriva da classe `MmioChainer` e substitui os métodos apropriados para exibir informações sobre o progresso. O MmioChainer cria uma seção com o nome da seção especificado e inicializa o encadeador com o nome do evento especificado. O nome do evento é salvo na estrutura de dados mapeada. Você deve tornar os nomes da seção e do evento exclusivos. A classe `Server` no código a seguir inicia o programa de instalação especificado, monitora o progresso e retorna um código de saída.

    ```cpp
    class Server : public ChainerSample::MmioChainer, public ChainerSample::IProgressObserver
    {
    public:
        …………….
        Server():ChainerSample::MmioChainer(L"TheSectionName", L"TheEventName") //customize for your event names
        {}
    ```

    A instalação é iniciada no método Main.

    ```cpp
    // Main entry point for program
    int __cdecl main(int argc, _In_count_(argc) char **_argv)
    {
        int result = 0;
        CString args;
        if (argc > 1)
        {
            args = CString(_argv[1]);
        }

        if (IsNetFx4Present(NETFX45_RC_REVISION))
        {
            printf(".NET Framework 4.5 is already installed");
        }
        else
        {
            result = Server().Launch(args);
        }

        return result;
    }
    ```

- Antes de iniciar a instalação, o encadeador verifica se o .NET Framework 4.5 já está instalado chamando `IsNetFx4Present`:

    ```cpp
    ///  Checks for presence of the .NET Framework 4.
    ///    A value of 0 for dwMinimumRelease indicates a check for the .NET Framework 4 full
    ///    Any other value indicates a check for a specific compatible release of the .NET Framework 4.
    #define NETFX40_FULL_REVISION 0
    // TODO: Replace with released revision number
    #define NETFX45_RC_REVISION MAKELONG(50309, 5)   // .NET Framework 4.5
    bool IsNetFx4Present(DWORD dwMinimumRelease)
    {
        DWORD dwError = ERROR_SUCCESS;
        HKEY hKey = NULL;
        DWORD dwData = 0;
        DWORD dwType = 0;
        DWORD dwSize = sizeof(dwData);

        dwError = ::RegOpenKeyExW(HKEY_LOCAL_MACHINE, L"SOFTWARE\\Microsoft\\NET Framework Setup\\NDP\\v4\\Full", 0, KEY_READ, &hKey);
        if (ERROR_SUCCESS == dwError)
        {
            dwError = ::RegQueryValueExW(hKey, L"Release", 0, &dwType, (LPBYTE)&dwData, &dwSize);

            if ((ERROR_SUCCESS == dwError) && (REG_DWORD != dwType))
            {
                dwError = ERROR_INVALID_DATA;
            }
            else if (ERROR_FILE_NOT_FOUND == dwError)
            {
                // Release value was not found, let's check for 4.0.
                dwError = ::RegQueryValueExW(hKey, L"Install", 0, &dwType, (LPBYTE)&dwData, &dwSize);

                // Install = (REG_DWORD)1;
                if ((ERROR_SUCCESS == dwError) && (REG_DWORD == dwType) && (dwData == 1))
                {
                    // treat 4.0 as Release = 0
                    dwData = 0;
                }
                else
                {
                    dwError = ERROR_INVALID_DATA;
                }
            }
        }

        if (hKey != NULL)
        {
            ::RegCloseKey(hKey);
        }

        return ((ERROR_SUCCESS == dwError) && (dwData >= dwMinimumRelease));
    }
    ```

- Você pode alterar o caminho do executável (Setup.exe no exemplo) no método `Launch` para apontar para o local correto ou personalizar o código para determinar o local. A classe base `MmioChainer` fornece um método `Run()` de bloqueio que a classe derivada chama.

    ```cpp
    bool Launch(const CString& args)
    {
    CString cmdline = L"dotNetFx45_Full_x86_x64.exe -pipe TheSectionName " + args; // Customize with name and location of setup .exe that you want to run
    STARTUPINFO si = {0};
    si.cb = sizeof(si);
    PROCESS_INFORMATION pi = {0};

    // Launch the Setup.exe that installs the .NET Framework 4.5
    BOOL bLaunchedSetup = ::CreateProcess(NULL,
     cmdline.GetBuffer(),
     NULL, NULL, FALSE, 0, NULL, NULL,
     &si,
     &pi);

    // If successful
    if (bLaunchedSetup != 0)
    {
    IProgressObserver& observer = dynamic_cast<IProgressObserver&>(*this);
    Run(pi.hProcess, observer);

    ……………………..
    return (bLaunchedSetup != 0);
    }
    ```

- O método `Send` intercepta e processa as mensagens. Nesta versão do .NET Framework, a única mensagem com suporte é a mensagem de fechar o aplicativo.

    ```cpp
            // SendMessage
            //
            // Send a message and wait for the response.
            // dwMessage: Message to send
            // pData: The buffer to copy the data to
            // dwDataLength: Initially a pointer to the size of pBuffer. Upon successful call, the number of bytes copied to pBuffer.
            //--------------------------------------------------------------
        virtual DWORD Send(DWORD dwMessage, LPVOID pData, DWORD dwDataLength)
        {
            DWORD dwResult = 0;
            printf("received message: %d\n", dwMessage);
            // Handle message
            switch (dwMessage)
            {
            case MMIO_CLOSE_APPS:
                {
                    printf("    applications are holding files in use:\n");
                    IronMan::MmioCloseApplications* applications = reinterpret_cast<IronMan::MmioCloseApplications*>(pData);
                    for(DWORD i = 0; i < applications->m_dwApplicationsSize; i++)
                    {
                        printf("      %ls (%d)\n", applications->m_applications[i].m_szName, applications->m_applications[i].m_dwPid);
                    }

                    printf("    should appliations be closed? (Y)es, (N)o, (R)efresh : ");
                    while (dwResult == 0)
                    {
                        switch (toupper(getwchar()))
                        {
                        case 'Y':
                            dwResult = IDYES;  // Close apps
                            break;
                        case 'N':
                            dwResult = IDNO;
                            break;
                        case 'R':
                            dwResult = IDRETRY;
                            break;
                        }
                    }
                    printf("\n");
                    break;
                }
            default:
                break;
            }
            printf("  response: %d\n  ", dwResult);
            return dwResult;
        }
    };
    ```

- Os dados de progresso são um `char` sem sinal entre 0 (0%) e 255 (100%).

    ```cpp
    private: // IProgressObserver
        virtual void OnProgress(unsigned char ubProgressSoFar)
        {…………
       }
    ```

- O HRESULT é passado para o método `Finished`.

    ```cpp
    virtual void Finished(HRESULT hr)
    {
    // This HRESULT is communicated over MMIO and may be different than process
    // Exit code of the Chainee Setup.exe itself
    printf("\r\nFinished HRESULT: 0x%08X\r\n", hr);
    }
    ```

    > [!IMPORTANT]
    > O .NET Framework 4.5 redistribuível normalmente grava muitas mensagens de progresso e uma única mensagem que indica a conclusão (no lado do encadeador). Ele também lê assincronamente, procurando registros `Abort`. Se receber um registro `Abort`, ele cancelará a instalação e gravará um registro concluído com E_ABORT como seus dados depois que a instalação tiver sido cancelada e as operações de instalação tiverem sido revertidas.

Um servidor típico cria um nome de arquivo MMIO aleatório, cria o arquivo (conforme mostrado no exemplo de código anterior, na `Server::CreateSection`) e inicia o redistribuível usando o método `CreateProcess` e passando o nome do pipe com a opção `-pipe someFileSectionName`. O servidor deve implementar os métodos `OnProgress`, `Send` e `Finished` com código específico da interface do usuário do aplicativo.

## <a name="see-also"></a>Veja também

- [Guia de implantação para desenvolvedores](deployment-guide-for-developers.md)
- [Implantação](index.md)
