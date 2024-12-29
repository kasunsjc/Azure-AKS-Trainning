# Azure Storage Files Documentation

This documentation provides an overview of the files and their purposes within the `Azure-AKS-Training/AKS Storage/Azure Storage Files` directory.

## Table of Contents
- [Azure Storage Files Documentation](#azure-storage-files-documentation)
  - [Table of Contents](#table-of-contents)
  - [Introduction](#introduction)
  - [Files Overview](#files-overview)
  - [Steps by Step Guide](#steps-by-step-guide)
  - [References](#references)

## Introduction
This directory contains resources and scripts related to Azure Storage Files, which are used in conjunction with Azure Kubernetes Service (AKS) for managing storage needs.

## Files Overview
Below is a list of the files in this directory and their descriptions:

- **README.md**: README for the section.
- **Azure-files.azcli**: Commands required for the Demo.


## Steps by Step Guide

1. Export the following environment variables:
    ```bash
    export RESOURCE_GROUP=aks-training-demo
    export AKS_CLUSTER_NAME=aks-training-demo-01
    export LOCATION=northeurope
    export STORAGE_ACCOUNT_NAME=akssakasun
    ```
2. Create Resource Group
    ```bash
    az group create --name $RESOURCE_GROUP --location $LOCATION
    ```
3. Create Storage Account
    ```bash
    az storage account create --name $STORAGE_ACCOUNT_NAME --resource-group $RESOURCE_GROUP --location $LOCATION --sku Standard_LRS
    ```
4. Create AKS Cluster
    ```bash
    az aks create --resource-group $RESOURCE_GROUP --name $AKS_CLUSTER_NAME --node-count 1 --generate-ssh-keys
    ```
5. Get AKS Credentials
    ```bash
    az aks get-credentials --resource-group $RESOURCE_GROUP --name $AKS_CLUSTER_NAME
    ```
6. Create a Storage Account 
    ```bash
    az storage account create --name $STORAGE_ACCOUNT_NAME --resource-group $RESOURCE_GROUP --location $LOCATION --sku Standard_LRS
    ```
7. Get the Storage Account Connection String
    ```bash
    export AZURE_STORAGE_CONNECTION_STRING=$(az storage account show-connection-string -n $STORAGE_ACCOUNT_NAME -g $RESOURCE_GROUP --query connectionString -o tsv)
    ```
8. Create a Storage Account File Share
    ```bash
    az storage share create --name $STORAGE_ACCOUNT_NAME --connection-string $AZURE_STORAGE_CONNECTION_STRING
    ```
9. Get the Storage Account Key 
    ```bash
    export AZURE_STORAGE_KEY=$(az storage account keys list --resource-group $RESOURCE_GROUP --account-name $STORAGE_ACCOUNT_NAME --query "[0].value" -o tsv)
    ```
10. Create a Kubernetes Secret
    ```bash
    kubectl create secret generic azure-secret --from-literal=azurestorageaccountname=$STORAGE_ACCOUNT_NAME --from-literal=azurestorageaccountkey=$AZURE_STORAGE_KEY
    ```
11. Get the Storage Account File Share ID
    ```bash
    export SA_SHARE_ID=$(az storage share-rm show -g $RESOURCE_GROUP --storage-account akssakasun --name aksshare -o tsv --query id)
    ```







## References
For more information on Azure Storage Files and AKS, refer to the following resources:
- [Azure Storage Files Documentation](https://docs.microsoft.com/en-us/azure/storage/files/)
- [Azure Kubernetes Service Documentation](https://docs.microsoft.com/en-us/azure/aks/)
