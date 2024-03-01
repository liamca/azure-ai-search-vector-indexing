# Azure AI Search - Python Based Content Vectorization and Enrichment

The goal of thie repo is to enable a code based approach for Chunking, Enriching and Vectorizing content into an Azure AI Search Index. It is meant to be easily extensible so that you can try different approaches of preparing your content for the purposes of doing Chat over your Data.

## Required Azure Services
This sample uses:
- Azure AI Search
- Azure OpenAI
- Azure Document Intelligence
- Azure Blob Storage

## Required Changes
- **config.json.sample**: You will need to update this config file with the details for your Azure services and rename this file to config.json.
- **schema.json**: This file is used to create an Index that leverages integrated vectorization. For this reason you will need to set this to use your Azure OpenAI Embeddings endpoint by updating:
          "resourceUri": "https://[OPENAI_SERVICE].openai.azure.com",
          "deploymentId": "[OPENAI deploymentId]",
          "apiKey": "[OPENAI API KEY]",

Ensure you install required packages:
```
pip install -r requirements.txt
```

This notebook has only been tested on Linux. If you have files other than PDF, you will need to install converters as follows:
```
sudo apt-get update
sudo apt-get install wkhtmltopdf
sudo apt-get install libreoffice
```

## Notebooks:
1) Create an Azure AI Search Index -- This can be modified using the schema.json file in the event you want to change langugage analyzers, add additional fields for filtering and personalization of content or to add support for document access control
2) Process Content -- This will take content stored in an Azure Blob Storage account and process it. It does all the chunking and vectorization of content for most common file types. This can be extended in the event you wish to add additional file types. It currently used Azure Document Intelligence for the processing of the file, but that could be changed if needed. All of the processed content is then stored in a set of JSON files which makes doing BCDR or Geo-replication much simpler
3) Index Content - This will take the content stored in the JSON files from the previous step and index them into Azure AI Search. You may wish to rather upload the JSON files to a separate Blob container and use the Azure AI Search indexer.
4) Test Queries - Notebook for executing search queries (Vector, Hybrid and Semantic) directly against Azure AI Search. This is useful for doing validation of different approaches.

## Future Goals
1) Create a set of Azure Functions that will be triggered automatically when new content is added or changed in Azure Blob to do processing and indexing
2) Add different approaches including support for chunking of content using GPT4

