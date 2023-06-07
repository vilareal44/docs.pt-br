---
title: 'Como: Mapear HRESULTs e exceções'
description: Examine como mapear valores HRESULT retornados de métodos COM para exceções geradas por métodos .NET. O tempo de execução manipula a transição entre COM e .NET.
ms.date: 03/30/2017
dev_langs:
- cpp
helpviewer_keywords:
- interoperation with unmanaged code, HRESULTs
- exceptions, HRESULTs
- interoperation with unmanaged code, exceptions
- HRESULTs
- COM interop, HRESULTs
- COM interop, exceptions
ms.assetid: 610b364b-2761-429d-9c4a-afbc3e66f1b9
ms.openlocfilehash: 827e79bdefcde7ae94567e5341ade76097dc8eaa
ms.sourcegitcommit: e02d17b2cf9c1258dadda4810a5e6072a0089aee
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/01/2020
ms.locfileid: "85619098"
---
# <a name="how-to-map-hresults-and-exceptions"></a>Como: Mapear HRESULTs e exceções
Métodos COM relatam erros retornando HRESULTs; métodos .NET os relatam gerando exceções. O runtime manipula a transição entre os dois. Cada classe de exceção do .NET Framework mapeia para um HRESULT.  
  
 Classes de exceção definidas pelo usuário podem especificar qualquer HRESULT apropriado. Essas classes de exceção podem alterar dinamicamente o HRESULT a ser retornado quando a exceção é gerada, definindo o campo **HResult** no objeto de exceção. Informações adicionais sobre a exceção são fornecidas ao cliente por meio da interface **IErrorInfo**, que é implementada no objeto .NET no processo não gerenciado.  
  
 Se você criar uma classe que estenda **System.Exception**, você deverá definir o campo HRESULT durante a construção. Caso contrário, a classe base atribui o valor de HRESULT. Você pode mapear as novas classes de exceção para um HRESULT existente, fornecendo o valor no construtor da exceção.  
  
 Observe que o runtime, às vezes, ignorará um `HRESULT` em casos nos quais um `IErrorInfo` estiver presente no thread.  Esse comportamento poderá ocorrer nos casos em que o `HRESULT` e o `IErrorInfo` não representarem o mesmo erro.  
  
### <a name="to-create-a-new-exception-class-and-map-it-to-an-hresult"></a>Para criar uma nova classe de exceção e mapeá-la para um HRESULT  
  
1. Use o código a seguir para criar uma nova classe de exceção chamada `NoAccessException` e mapeá-la para o HRESULT `E_ACCESSDENIED`.  
  
    ```cpp  
    Class NoAccessException : public ApplicationException  
    {  
        NoAccessException () {  
        HResult = E_ACCESSDENIED;
    }  
    }  
    CMyClass::MethodThatThrows  
    {  
    throw new NoAccessException();  
    }  
    ```  
  
 Você pode encontrar um programa (em qualquer linguagem de programação) que usa o código gerenciado e não gerenciado simultaneamente. Por exemplo, o marshaler personalizado no exemplo de código a seguir usa o método **Marshal.ThrowExceptionForHR(int HResult)** para lançar uma exceção com um valor de HRESULT específico. O método pesquisa o HRESULT e gera o tipo de exceção apropriado. Por exemplo, o HRESULT no fragmento de código a seguir gera **ArgumentException**.  
  
```cpp  
CMyClass::MethodThatThrows  
{  
    Marshal.ThrowExceptionForHR(COR_E_ARGUMENT);  
}  
```  
  
 A tabela a seguir fornece o mapeamento completo de cada HRESULT para a respectiva classe de exceção comparável no .NET Framework.  
  
|HRESULT|Exceção do .NET|  
|-------------|--------------------|  
|**MSEE_E_APPDOMAINUNLOADED**|**AppDomainUnloadedException**|  
|**COR_E_APPLICATION**|**ApplicationException**|  
|**COR_E_ARGUMENT ou E_INVALIDARG**|**ArgumentException**|  
|**COR_E_ARGUMENTOUTOFRANGE**|**ArgumentOutOfRangeException**|  
|**COR_E_ARITHMETIC ou ERROR_ARITHMETIC_OVERFLOW**|**ArithmeticException**|  
|**COR_E_ARRAYTYPEMISMATCH**|**ArrayTypeMismatchException**|  
|**COR_E_BADIMAGEFORMAT ou ERROR_BAD_FORMAT**|**BadImageFormatException**|  
|**COR_E_COMEMULATE_ERROR**|**COMEmulateException**|  
|**COR_E_CONTEXTMARSHAL**|**ContextMarshalException**|  
|**COR_E_CORE**|**Coreexception**|  
|**NTE_FAIL**|**CryptographicException**|  
|**COR_E_DIRECTORYNOTFOUND ou ERROR_PATH_NOT_FOUND**|**DirectoryNotFoundException**|  
|**COR_E_DIVIDEBYZERO**|**DivideByZeroException**|  
|**COR_E_DUPLICATEWAITOBJECT**|**DuplicateWaitObjectException**|  
|**COR_E_ENDOFSTREAM**|**EndOfStreamException**|  
|**COR_E_TYPELOAD**|**EntryPointNotFoundException**|  
|**COR_E_EXCEPTION**|**Exceção**|  
|**COR_E_EXECUTIONENGINE**|**ExecutionEngineException**|  
|**COR_E_FIELDACCESS**|**FieldAccessException**|  
|**COR_E_FILENOTFOUND ou ERROR_FILE_NOT_FOUND**|**FileNotFoundException**|  
|**COR_E_FORMAT**|**FormatException**|  
|**COR_E_INDEXOUTOFRANGE**|**IndexOutOfRangeException**|  
|**COR_E_INVALIDCAST ou E_NOINTERFACE**|**InvalidCastException**|  
|**COR_E_INVALIDCOMOBJECT**|**InvalidComObjectException**|  
|**COR_E_INVALIDFILTERCRITERIA**|**InvalidFilterCriteriaException**|  
|**COR_E_INVALIDOLEVARIANTTYPE**|**InvalidOleVariantTypeException**|  
|**COR_E_INVALIDOPERATION**|**InvalidOperationException**|  
|**COR_E_IO**|**IOException**|  
|**COR_E_MEMBERACCESS**|**AccessException**|  
|**COR_E_METHODACCESS**|**MethodAccessException**|  
|**COR_E_MISSINGFIELD**|**MissingFieldException**|  
|**COR_E_MISSINGMANIFESTRESOURCE**|**MissingManifestResourceException**|  
|**COR_E_MISSINGMEMBER**|**MissingMemberException**|  
|**COR_E_MISSINGMETHOD**|**MissingMethodException**|  
|**COR_E_MULTICASTNOTSUPPORTED**|**MulticastNotSupportedException**|  
|**COR_E_NOTFINITENUMBER**|**NotFiniteNumberException**|  
|**E_NOTIMPL**|**NotImplementedException**|  
|**COR_E_NOTSUPPORTED**|**NotSupportedException**|  
|**COR_E_NULLREFERENCE ou E_POINTER**|**NullReferenceException**|  
|**COR_E_OUTOFMEMORY ou**<br /><br /> **E_OUTOFMEMORY**|**OutOfMemoryException**|  
|**COR_E_OVERFLOW**|**OverflowException**|  
|**COR_E_PATHTOOLONG ou ERROR_FILENAME_EXCED_RANGE**|**PathTooLongException**|  
|**COR_E_RANK**|**RankException**|  
|**COR_E_REFLECTIONTYPELOAD**|**ReflectionTypeLoadException**|  
|**COR_E_REMOTING**|**RemotingException**|  
|**COR_E_SAFEARRAYTYPEMISMATCH**|**SafeArrayTypeMismatchException**|  
|**COR_E_SECURITY**|**SecurityException**|  
|**COR_E_SERIALIZATION**|**SerializationException**|  
|**COR_E_STACKOVERFLOW ou ERROR_STACK_OVERFLOW**|**StackOverflowException**|  
|**COR_E_SYNCHRONIZATIONLOCK**|**SynchronizationLockException**|  
|**COR_E_SYSTEM**|**SystemException**|  
|**COR_E_TARGET**|**TargetException**|  
|**COR_E_TARGETINVOCATION**|**TargetInvocationException**|  
|**COR_E_TARGETPARAMCOUNT**|**TargetParameterCountException**|  
|**COR_E_THREADABORTED**|**ThreadAbortException**|  
|**COR_E_THREADINTERRUPTED**|**ThreadInterruptedException**|  
|**COR_E_THREADSTATE**|**ThreadStateException**|  
|**COR_E_THREADSTOP**|**ThreadStopException**|  
|**COR_E_TYPELOAD**|**TypeLoadException**|  
|**COR_E_TYPEINITIALIZATION**|**TypeInitializationException**|  
|**COR_E_VERIFICATION**|**VerificationException**|  
|**COR_E_WEAKREFERENCE**|**WeakReferenceException**|  
|**COR_E_VTABLECALLSNOTSUPPORTED**|**VTableCallsNotSupportedException**|  
|**Todos os outros HRESULTs**|**COMException**|  
  
 Para recuperar informações sobre erros estendidos, o cliente gerenciado deve examinar os campos do objeto de exceção gerado. Para o objeto de exceção fornecer informações úteis sobre o erro, o objeto COM deve implementar a interface **IErrorInfo**. O runtime usa as informações fornecidas por **IErrorInfo** para inicializar o objeto de exceção.  
  
 Se o objeto COM não dá suporte a **IErrorInfo**, o runtime inicializa um objeto de exceção com valores padrão. A tabela a seguir lista cada campo associado a um objeto de exceção e identifica a origem das informações padrão quando o objeto COM dá suporte a **IErrorInfo**.  
  
 Observe que o runtime, às vezes, ignorará um `HRESULT` em casos nos quais um `IErrorInfo` estiver presente no thread.  Esse comportamento poderá ocorrer nos casos em que o `HRESULT` e o `IErrorInfo` não representarem o mesmo erro.  
  
|Campo de exceção|Origem das informações do COM|  
|---------------------|------------------------------------|  
|**ErrorCode**|HRESULT retornado da chamada.|  
|**HelpLink**|Se **IErrorInfo->HelpContext** é diferente de zero, a cadeia de caracteres é formada pela concatenação de **IErrorInfo->GetHelpFile** e "#" e **IErrorInfo->GetHelpContext**. Caso contrário, a cadeia de caracteres é retornada de **IErrorInfo->GetHelpFile**.|  
|**InnerException**|Sempre uma referência nula (**Nothing** no Visual Basic).|  
|**Message**|Cadeia de caracteres retornada de **IErrorInfo->GetDescription**.|  
|**Origem**|Cadeia de caracteres retornada de **IErrorInfo->GetSource**.|  
|**Pilha**|O rastreamento de pilha.|  
|**TargetSite**|O nome do método que retornou o HRESULT com falha.|  
  
 Campos de exceção, tais como **Message**, **Source** e **StackTrace**, não estão disponíveis para o **StackOverflowException**.  
  
## <a name="see-also"></a>Consulte também

- [Interoperabilidade COM avançada](https://docs.microsoft.com/previous-versions/dotnet/netframework-4.0/bd9cdfyx(v=vs.100))
- [Exceções](../../standard/exceptions/index.md)
