---
title: ICorProfilerCallback::JITCachedFunctionSearchFinished-Methode
ms.date: 03/30/2017
api_name:
- ICorProfilerCallback.JITCachedFunctionSearchFinished
api_location:
- mscorwks.dll
api_type:
- COM
f1_keywords:
- ICorProfilerCallback::JITCachedFunctionSearchFinished
helpviewer_keywords:
- JITCachedFunctionSearchFinished method [.NET Framework profiling]
- ICorProfilerCallback::JITCachedFunctionSearchFinished method [.NET Framework profiling]
ms.assetid: 3c325c82-cddd-4b00-b3da-e450c36abf62
topic_type:
- apiref
ms.openlocfilehash: 6efc9d407bb95f75a79252b2dfad85b396d2164a
ms.sourcegitcommit: da21fc5a8cce1e028575acf31974681a1bc5aeed
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 06/08/2020
ms.locfileid: "84500077"
---
# <a name="icorprofilercallbackjitcachedfunctionsearchfinished-method"></a>ICorProfilerCallback::JITCachedFunctionSearchFinished-Methode
Benachrichtigt den Profiler, dass eine Suche für eine Funktion abgeschlossen wurde, die zuvor mit dem Native Image Generator (Ngen. exe) kompiliert wurde.  
  
## <a name="syntax"></a>Syntax  
  
```cpp  
HRESULT JITCachedFunctionSearchFinished(  
    [in] FunctionID        functionId,  
    [in] COR_PRF_JIT_CACHE result);  
```  
  
## <a name="parameters"></a>Parameter

- `functionId`

  \[in] die ID der Funktion, für die die Suche durchgeführt wurde.

- `result`

  \[in] ein Wert der [COR_PRF_JIT_CACHE](cor-prf-jit-cache-enumeration.md) -Enumeration, die das Ergebnis der Suche angibt.

## <a name="remarks"></a>Bemerkungen  
 In der .NET Framework Version 2,0 werden die [ICorProfilerCallback:: JITCachedFunctionSearchStarted](icorprofilercallback-jitcachedfunctionsearchstarted-method.md) -und `JITCachedFunctionSearchFinished` Rückrufe nicht für alle Funktionen in regulären NGen-Images erstellt. Nur NGen-Images, die für einen Profiler optimiert sind, generieren Rückrufe für alle Funktionen im Image. Aufgrund des zusätzlichen Aufwands sollte ein Profiler jedoch nur Profiler-optimierte NGen-Images anfordern, wenn er diese Rückrufe verwenden soll, um zu erzwingen, dass eine Funktion Just-in-time (JIT) kompiliert wird. Andernfalls sollte der Profiler eine verzögerte Strategie zum Sammeln von Funktions Informationen verwenden.  
  
## <a name="requirements"></a>Requirements (Anforderungen)  
 **Plattformen:** Informationen finden Sie unter [Systemanforderungen](../../get-started/system-requirements.md).  
  
 **Header:** CorProf.idl, CorProf.h  
  
 **Bibliothek:** CorGuids.lib  
  
 **.NET Framework Versionen:**[!INCLUDE[net_current_v20plus](../../../../includes/net-current-v20plus-md.md)]  
  
## <a name="see-also"></a>Weitere Informationen:

- [ICorProfilerCallback-Schnittstelle](icorprofilercallback-interface.md)
