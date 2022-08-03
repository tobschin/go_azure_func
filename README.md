# Azure Functions in Go mit Function Core CLI 
Source https://www.thorsten-hans.com/azure-functions-with-go/

## Create 

```sh
  func init --worker-runtime custom
  go mod init fn
  go get -u github.com/gin-gonic/gin
  mkdir cmd
  touch cmd/api.go # dann inhalt einfugen
  go build ./cmd/api.go
  func new -l Custom -t HttpTrigger -n products -a anonymous
  func start
  func new -l Custom -t HttpTrigger -n product-by-id -a anonymous

  ## run locally
  func start
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