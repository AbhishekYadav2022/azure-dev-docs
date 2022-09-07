---
title: Use JavaScript with Cassandra on Azure Cosmos DB
description: To create or move your Cassandra database to Azure, you need a Cosmos DB resource. 
ms.topic: how-to
ms.date: 08/08/2022
ms.custom: devx-track-js, devx-track-azurecli
---

# Develop a JavaScript application with Cassandra on Azure


To create, move, or use a Cassandra DB database to Azure, you need a Cosmos DB resource. Learn how to create the resource and use your database.

## Locally develop with the CosmosDB emulator

Learn how to install the [CosmosDB emulator](/azure/cosmos-db/local-emulator) and [start the emulator for Cassandra development](/azure/cosmos-db/local-emulator?tabs=cli%2Cssl-netstd21#cassandra-api). 

## Create a Cosmos DB resource for a Cassandra DB database

You can create a resource with:

* Azure CLI
* [Azure portal](https://portal.azure.com)
* Visual Studio Code [extension](https://marketplace.visualstudio.com/items?itemName=ms-azuretools.vscode-cosmosdb)

[!INCLUDE [Azure CLI commands](../../includes/azure-cli-cassandra-db.md)]

## View and use your Cassandra DB on Azure Cosmos DB

While developing your Cassandra DB database with JavaScript, use [Cosmos explorer](https://cosmos.azure.com/) to work with your database. 

:::image type="content" source="../../media/howto-database/cosmos-explorer-cassandra-add-table-row.png" alt-text="Use the Cosmos explorer, found at https://cosmos.azure.com/, to view and work with your Cassandra DB database.":::


The Cosmos explorer is also available in the Azure portal, for your resource, as the **Data Explorer**.


:::image type="content" source="../../media/howto-database/cosmos-explorer-azure-portal.png" alt-text="The Cosmos explorer is also available in the Azure portal, for your resource, as the `Data Explorer`.":::

## Use native SDK packages to connect to Cassandra DB on Azure

The Cassandra DB database on Cosmos DB uses npm packages already available, such as:

* [cassandra-driver](https://www.npmjs.com/package/cassandra-driver)

**localDataCenter** using cassandra-driver:

* V3, use the default of `dataCenter1`
* V4, you must specify the data center, such as `Central US` in the following code block. 

```javascript
  let client = new cassandra.Client({
    contactPoints: [`${config.contactPoint}:10350`],
    authProvider: authProvider,
    localDataCenter: 'Central US',
    sslOptions: {
      secureProtocol: 'TLSv1_2_method',
      rejectUnauthorized: false,
    },
  });
``` 

If you are unsure of your localDataCenter, remove the property, run the sample code, and the value of the property is returned in the error text. 

```text
NoHostAvailableError: All host(s) tried for query failed. First host tried, xxx.xxx.xxx.xxx:10350: ArgumentError: localDataCenter was configured as 'dataCenter1', but only found hosts in data centers: [Central US]
```

## Use cassandra-driver SDK to connect to Cassandra DB on Azure

To connect and use your Cassandra DB on Azure Cosmos DB with JavaScript and cassandra-driver, use the following procedure.

1. Make sure Node.js and npm are installed.
1. Create a Node.js project in a new folder:

    ```bash
    mkdir DataDemo && \
        cd DataDemo && \
        npm init -y && \
        npm install cassandra-driver && \
        touch index.js && \
        code .
    ```

    The command:
    * creates a project folder named `DataDemo`
    * changes the Bash terminal into that folder
    * initializes the project, which creates the `package.json` file
    * adds the cassandra-driver npm SDK to the project
    * creates the `index.js` script file
    * opens the project in Visual Studio Code

1. Copy the following JavaScript code into `index.js`:

    :::code language="JavaScript" source="~/../js-e2e/database/cassandra/index.js" :::
 
1. Replace the following in the script with your Cosmos DB Cassandra connection information:

    * YOUR-RESOURCE-NAME
    * YOUR-USERNAME - replace with YOUR-RESOURCE-NAME
    * YOUR-PASSWORD

1. Run the script.

    ```bash
    node index.js
    ```

    The results are:

    ```console
    connected
    created keyspace
    created table
    insert
    Obtained row: Joan Smith | JSmith | northus 
    Obtained row: Tim Jones | TJones | centralus 
    Obtained row: Bob Wright | BWright | westus
    Getting by region
    Obtained row: Bob Wright | BWright | westus 
    done
    ```

## Next steps

* How to [deploy a JavaScript web app](../deploy-web-app.md)
* [Cosmos DB for Cassandra DB documentation](/azure/cosmos-db/cassandra-introduction)
* [Cosmos DB for Cassandra DB quickstart](/azure/cosmos-db/create-cassandra-nodejs)
* [Migration guide to move to Cosmos DB for Cassandra DB](/azure/cosmos-db/cassandra-migrate-cosmos-db-databricks)