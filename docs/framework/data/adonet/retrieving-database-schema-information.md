---
title: Recupero di informazioni dello schema del database
ms.date: 03/30/2017
ms.assetid: 79038d52-f122-4fd4-9bfb-aaa22d6a114b
ms.openlocfilehash: 885d3c9ad61c9099c960ddb0c0f77fa8a98dbefa
ms.sourcegitcommit: 9b552addadfb57fab0b9e7852ed4f1f1b8a42f8e
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/23/2019
ms.locfileid: "61664246"
---
# <a name="retrieving-database-schema-information"></a>Recupero di informazioni dello schema del database
Il recupero di informazioni sullo schema da un database viene eseguito tramite il processo di individuazione dello schema. Individuazione dello schema consente alle applicazioni di richiedere che i provider gestiti trovano e restituiscono informazioni sullo schema di database, noto anche come *metadati*, di un determinato database. Nella raccolta di schemi vengono esposti vari elementi dello schema del database, quali tabelle, colonne e stored procedure. Ogni raccolta di schemi contiene una varietà di informazioni sullo schema specifiche del provider usato.  
  
 Ognuno di implementare il provider gestito .NET Framework la **GetSchema** metodo nella **connessione** classe e le informazioni sullo schema restituito dal **GetSchema**metodo è disponibile in forma di un <xref:System.Data.DataTable>. Il **GetSchema** metodo è un metodo di overload che fornisce i parametri facoltativi per specificare la raccolta di schema da restituire e limitare la quantità di informazioni restituite.  
  
 I provider di dati .NET Framework per OLE DB, ODBC, Oracle e SqlClient forniscono un **GetSchemaTable** metodo che restituisce un oggetto DataTable descrivente i metadati della colonna la **DataReader**.  
  
 Il provider di dati .NET Framework per OLE DB presenta le informazioni sullo schema usando anche il metodo <xref:System.Data.OleDb.OleDbConnection.GetOleDbSchemaTable%2A> dell'oggetto <xref:System.Data.OleDb.OleDbConnection>. Come argomenti **GetOleDbSchemaTable** accetta un <xref:System.Data.OleDb.OleDbSchemaGuid> che identifica le informazioni sullo schema da restituire e una matrice di restrizioni sulle colonne restituite. **GetOleDbSchemaTable** restituisce un <xref:System.Data.DataTable> popolato con le informazioni richieste sullo schema.  
  
## <a name="in-this-section"></a>In questa sezione  
 [Raccolte di schemi e GetSchema](../../../../docs/framework/data/adonet/getschema-and-schema-collections.md)  
 Descrive la **GetSchema** (metodo) e come può essere usato per recuperare e limitare le informazioni sullo schema da un database.  
  
 Restrizione dello schema  
 Descrive le restrizioni dello schema che possono essere usate con **GetSchema**.  
  
 [Raccolte di schemi comuni](../../../../docs/framework/data/adonet/common-schema-collections.md)  
 Vengono descritte tutte le raccolte di schemi comuni supportate dai provider .NET Framework gestiti.  
  
 [Raccolte di schemi SQL Server](../../../../docs/framework/data/adonet/sql-server-schema-collections.md)  
 Viene illustrata la raccolta di schemi supportata dal provider .NET Framework per SQL Server.  
  
 [Raccolte di schemi Oracle](../../../../docs/framework/data/adonet/oracle-schema-collections.md)  
 Viene illustrata la raccolta di schemi supportata dal provider .NET Framework per Oracle.  
  
 [Raccolte di schemi ODBC](../../../../docs/framework/data/adonet/odbc-schema-collections.md)  
 Vengono illustrate le raccolte di schemi per i driver ODBC.  
  
 [Raccolte di schemi OLE DB](../../../../docs/framework/data/adonet/ole-db-schema-collections.md)  
 Vengono illustrate le raccolte di schemi per i provider OLE DB.  
  
## <a name="reference"></a>Riferimenti  
 <xref:System.Data.Common.DbConnection.GetSchema%2A>  
 Descrive la **GetSchema** metodo il <xref:System.Data.Common.DbConnection> classe.  
  
 <xref:System.Data.Odbc.OdbcConnection.GetSchema%2A>  
 Descrive la **GetSchema** metodo il <xref:System.Data.Odbc.OdbcConnection> classe.  
  
 <xref:System.Data.OleDb.OleDbConnection.GetSchema%2A>  
 Descrive la **GetSchema** metodo il <xref:System.Data.OleDb.OleDbConnection> classe.  
  
 <xref:System.Data.OracleClient.OracleConnection.GetSchema%2A>  
 Descrive la **GetSchema** metodo il <xref:System.Data.OracleClient.OracleConnection> classe.  
  
 <xref:System.Data.SqlClient.SqlConnection.GetSchema%2A>  
 Descrive la **GetSchema** metodo il <xref:System.Data.SqlClient.SqlConnection> classe.  
  
 <xref:System.Data.Common.DbDataReader.GetSchemaTable%2A>  
 Descrive la **GetSchemaTable** metodo il <xref:System.Data.Common.DbDataReader> classe.  
  
 <xref:System.Data.Odbc.OdbcDataReader.GetSchemaTable%2A>  
 Descrive la **GetSchemaTable** metodo il <xref:System.Data.Odbc.OdbcDataReader> classe.  
  
 <xref:System.Data.OleDb.OleDbDataReader.GetSchemaTable%2A>  
 Descrive la **GetSchemaTable** metodo il <xref:System.Data.OleDb.OleDbDataReader> classe.  
  
 <xref:System.Data.OracleClient.OracleDataReader.GetSchemaTable%2A>  
 Descrive la **GetSchemaTable** metodo il <xref:System.Data.OracleClient.OracleDataReader> classe.  
  
 <xref:System.Data.SqlClient.SqlDataReader.GetSchemaTable%2A>  
 Descrive la **GetSchemaTable** metodo il <xref:System.Data.SqlClient.SqlDataReader> classe.  
  
## <a name="see-also"></a>Vedere anche

- [Recupero e modifica di dati in ADO.NET](../../../../docs/framework/data/adonet/retrieving-and-modifying-data.md)
- [Provider gestiti ADO.NET e Centro per sviluppatori di set di dati](https://go.microsoft.com/fwlink/?LinkId=217917)
