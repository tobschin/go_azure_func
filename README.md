# Azure Functions in Go mit Function Core CLI 
Source https://www.thorsten-hans.com/azure-functions-with-go/

## Create 

```sh
# Init func
func init --worker-runtime custom

# Init go module
go mod init fn
go get -u github.com/gin-gonic/gin

# create folder
mkdir cmd
touch cmd/api.go # dann inhalt einfugen

# build gin tonic api
go build ./cmd/api.go

# add new endpoints
func new -l Custom -t HttpTrigger -n products -a anonymous
func new -l Custom -t HttpTrigger -n product-by-id -a anonymous # ==> change path in function.json

## run locally
func start

### cross compile (needed, that app can run on Azure Cloud)
GOOS=linux GOARCH=amd64 go build cmd/api.go

```

## Deploy to Azure
```sh
# set variables
STORAGE_ACCOUNT=nofearbrate1234 \
RESOURCE_GROUP=rg-functions-with-go \
LOCATION=germanywestcentral \
FUNCTIONAPP_NAME=nofearbrate

# create rg
az group create \
--name $RESOURCE_GROUP \ 
--location $LOCATION

# create storage account
az storage account create \
--name $STORAGE_ACCOUNT \
--location $LOCATION \
--resource-group $RESOURCE_GROUP \
--sku Standard_LRS

# create function
az functionapp create \
-n $FUNCTIONAPP_NAME \
-g $RESOURCE_GROUP  \
--consumption-plan-location $LOCATION \
--os-type Linux \
--runtime custom  \
--functions-version 3 \
--storage-account $STORAGE_ACCOUNT

# deploy
func azure functionapp publish $FUNCTIONAPP_NAME

# destroy 
az group delete -n $RESOURCE_GROUP

```
