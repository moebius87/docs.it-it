---
title: WHERE (Entity SQL)
ms.date: 03/30/2017
ms.assetid: a8e1061e-0028-4a6f-8f19-b9f48e96c4b8
ms.openlocfilehash: 939d4c0ec2c30bc71b22fb65ab36644e063f97de
ms.sourcegitcommit: 155012a8a826ee8ab6aa49b1b3a3b532e7b7d9bd
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 06/04/2019
ms.locfileid: "66489843"
---
# <a name="where-entity-sql"></a>WHERE (Entity SQL)
La clausola WHERE viene applicata direttamente dopo il [FROM](../../../../../../docs/framework/data/adonet/ef/language-reference/from-entity-sql.md) clausola.  
  
## <a name="syntax"></a>Sintassi  
  
```  
[ WHERE expression ]  
```  
  
## <a name="arguments"></a>Argomenti  
 `expression`  
 Tipo Boolean.  
  
## <a name="remarks"></a>Note  
 La clausola WHERE presenta la stessa semantica descritta per Transact-SQL. Questa clausola consente di ridurre gli oggetti prodotti dall'espressione di query limitando gli elementi delle raccolte di origine a quelli che soddisfano la condizione.  
  
```  
select c from cs as c where e  
```  
  
 L'espressione `e` deve essere di tipo Boolean.  
  
 La clausola WHERE viene applicata direttamente dopo la clausola FROM e prima che avvenga qualsiasi operazione di  raggruppamento, ordinamento o proiezione. Tutti i nomi degli elementi definiti nella clausola FROM sono visibili all'espressione della clausola WHERE.  
  
## <a name="see-also"></a>Vedere anche

- [Riferimento a Entity SQL](../../../../../../docs/framework/data/adonet/ef/language-reference/entity-sql-reference.md)
- [Espressioni di query](../../../../../../docs/framework/data/adonet/ef/language-reference/query-expressions-entity-sql.md)
