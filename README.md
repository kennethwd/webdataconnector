# webdataconnector

Beta Zone
---------------
Currently in development is the WDC API version 2.0.  The biggest change with this version is the addition of support for multiple tables.  We have also siginficiantly refactored the underlying structure of the web data connector API. 

This is a very early version of this feature, we are still under heavy iteration with the design of this API.  There will be some errors and incomplete functionality in the current version.  Please open an issue for any bug you encounter and also please leave feedback through issues as well!  


API Overview
---------------

There are two major changes in version 2 of thew WDC API:
* The API now allows the web data connector to independently bring back multiple tables
* The API waits for the developer to tell it when it's finished gathering data, rather than repeatedly calling getData.

In version 1, at a high level, a connector followed this flow:
 1. WDC loads and runs its interactive phase.  When finished, it calls tableau.submit().
 2. In the data gathering phase, Tableau/Simulator calls connector.getColumnHeaders(), which defines the schema for a single table.  The WDC calls a predefined callback when it finishes defining the schema.
 3. Tableau/Simulator calls connector.getTableData() which returns data through the predefined tableau.dataCallback.
 4. Step 3 is repeated until dataCallback passes a flag to tell Tableau it has no more data.


In contrast, at a high level, version 2 of the API follows this flow:
 1. WDC loads and runs its interactive phase.  When finished, it calls tableau.submit().
 2. In the data gathering phase, Tableau/Simulator calls connector.getSchema, which defines schemas for multiple tables. getSchema is passed a callback that the WDC calls when it has finished defining schemas. 
 3. Tableau/Simulator calls connector.getData.  getData is passed both a callback and list of tables that Tableau/Simulator has requested data for.  Within getData, the WDC developer can add data to any table at any time using a new method called appendRows. 
 4. Whenever the WDC has finished getting all of its data for the requested tables, it calls the passed in doneCallback.


Please consult the beta docs for new API definitions: [Beta docs](https://connectors.tableau.com/docs/index.html).

Please see the StockQuotesConnector_final sample as an example of a WDC built with the new API.


Sample Status and ConvertConnector
---------------
The samples have not yet been fully ported over.  Currently, only the StockQuotesConnector_final.html example has been ported to the new API.  This is a work in progress. Additionally, the IncrementalUpdateConnector and StockQuoteConnector_advanced samples have been modified to use a utility script that converts old connectors to the new version of the Library.  To use this convert connector utility in an old WDC, include the js/convert-connector.js script and call _convertConnectorFor12(myConnector) before calling tableau.registerConnector(myConnector).  

We are in the process of making new documentation and step by step tutorials for building WDCs on the new API. 


