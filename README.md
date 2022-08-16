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
# create rg
az group create -n rg-functions-with-go  -l germanywestcentral

# create function
az functionapp create -n nofearbrate   -g rg-functions-with-go  --consumption-plan-location germanywestcentral   --os-type Linux --runtime custom  --functions-version 3 --storage-account nofearbrate1234

# deploy
func azure functionapp publish nofearbrate

```
