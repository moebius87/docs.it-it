---
title: Composizione di query Entity SQL annidate
ms.date: 03/30/2017
ms.assetid: 685d4cd3-2c1f-419f-bb46-c9d97a351eeb
ms.openlocfilehash: 4d6892e96cfbc9c5ba9d389aa03588c5133c7943
ms.sourcegitcommit: 9b552addadfb57fab0b9e7852ed4f1f1b8a42f8e
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/23/2019
ms.locfileid: "61606225"
---
# <a name="composing-nested-entity-sql-queries"></a>Composizione di query Entity SQL annidate
[!INCLUDE[esql](../../../../../../includes/esql-md.md)] è un linguaggio funzionale completo. Il blocco predefinito di [!INCLUDE[esql](../../../../../../includes/esql-md.md)] è un'espressione. A differenza, linguaggio SQL convenzionale [!INCLUDE[esql](../../../../../../includes/esql-md.md)] non è limitato a un set di risultati tabulari: [!INCLUDE[esql](../../../../../../includes/esql-md.md)] supporta la creazione di espressioni complesse che possono avere valori letterali, parametri o espressioni annidate. Un valore nell'espressione può essere parametrizzato o composto da altre espressioni.  
  
## <a name="nested-expressions"></a>Espressioni annidate  
 Un'espressione annidata può essere inserita in una posizione qualsiasi in cui è accettato un valore del tipo restituito dall'espressione. Ad esempio:  
  
```  
-- Returns a hierarchical collection of three elements at top-level.   
-- x must be passed in the parameter collection.  
ROW(@x, {@x}, {@x, 4, 5}, {@x, 7, 8, 9})  
  
-- Returns a hierarchical collection of one element at top-level.  
-- x must be passed in the parameter collection.  
{{{@x}}};  
```  
  
 Una query annidata può essere inserita in una clausola di proiezione. Ad esempio:  
  
```  
-- Returns a collection of rows where each row contains an Address entity.  
-- and a collection of references to its corresponding SalesOrderHeader entities.  
SELECT address, (SELECT DEREF(soh)   
                    FROM NAVIGATE(address, AdventureWorksModel.FK_SalesOrderHeader_Address_BillToAddressID) AS soh)   
                    AS salesOrderHeader FROM AdventureWorksEntities.Address AS address  
```  
  
 In [!INCLUDE[esql](../../../../../../includes/esql-md.md)] le query annidate devono sempre essere racchiuse tra parentesi:  
  
```  
-- Pseudo-Entity SQL  
( SELECT …  
FROM … )  
UNION ALL  
( SELECT …  
FROM … );  
```  
  
 Nell'esempio seguente viene illustrato come annidare correttamente le espressioni in [!INCLUDE[esql](../../../../../../includes/esql-md.md)]: [Procedura: Ordinare l'unione di due query](https://docs.microsoft.com/previous-versions/dotnet/netframework-4.0/bb896299(v=vs.100)).  
  
## <a name="nested-queries-in-projection"></a>Query annidate nella proiezione  
 Le query annidate nella clausola del progetto potrebbero essere tradotte in query del prodotto cartesiano sul server. In alcuni server di back-end, tra cui Server SLQ, questo può provocare l'aumento delle dimensioni della tabella TempDB, con effetti negativi sulle prestazioni del server.  
  
 Di seguito è riportato un esempio di questo tipo di query:  
  
```  
SELECT c, (SELECT c, (SELECT c FROM AdventureWorksModel.Vendor AS c  ) As Inner2 FROM AdventureWorksModel.JobCandidate AS c  ) As Inner1 FROM AdventureWorksModel.EmployeeDepartmentHistory AS c  
```  
  
## <a name="ordering-nested-queries"></a>Ordinamento di query annidate  
 In Entity Framework un'espressione annidata può essere inserita in un punto qualsiasi della query. Poiché Entity SQL rende molto flessibile la scrittura di query, è possibile scrivere una query contenente una serie di query annidate in ordine. L'ordine di una query annidata non viene tuttavia mantenuto.  
  
```  
-- The following query will order the results by last name.  
SELECT C1.FirstName, C1.LastName  
        FROM AdventureWorksModel.Contact as C1  
        ORDER BY C1.LastName  
```  
  
```  
-- In the following query, ordering of the nested query is ignored.  
SELECT C2.FirstName, C2.LastName  
    FROM (SELECT C1.FirstName, C1.LastName  
        FROM AdventureWorksModel.Contact as C1  
        ORDER BY C1.LastName) as C2  
```  
  
## <a name="see-also"></a>Vedere anche

- [Panoramica di Entity SQL](../../../../../../docs/framework/data/adonet/ef/language-reference/entity-sql-overview.md)
