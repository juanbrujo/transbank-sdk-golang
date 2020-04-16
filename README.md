# Unofficial Transbank SDK Golang

Implementación de SDK no ofical para Golang.

## Requisitos:

- Golang X.Y.Z (zero dependencies)

# Instalación

```bash
go get -v github.com/microapis/transbank-sdk-golang/pkg/webpay
```

# Documentación

Puedes ver la documentación generada en [pkg.go.dev](https://pkg.go.dev/github.com/microapis/transbank-sdk-golang?tab=doc) para ver la implementación de la librería. También puedes consultar la [documentación oficial](https://www.transbankdevelopers.cl/documentacion/como_empezar).

# Ejemplos

## Iniciar Transacción con Webpay Plus Normal (Integración)

```golang
amount := float64(1000)
sessionID := "mi-id-de-sesion"
buyOrder := strconv.Itoa(rand.Intn(99999))
returnURL := "https://callback/resultado/de/transaccion"
finalURL := "https://callback/final/post/comprobante/webpay"

service := webpay.NewIntegrationPlusNormal()
transaction, err := service.InitTransaction(amount, sessionID, buyOrder, returnURL, finalURL)
if err != nil {
  log.Fatalln(err)
}

log.Println(transaction.URL) // https://webpay3gint.transbank.cl/webpayserver/initTransaction
log.Println(transaction.Token) // e95675887afd8c5ad7d7e146468452fc4bc896541688c78cd781ded0ddef0260
```

## Obtener Resultado de la Transacción con Webpay Patpass (Producción)

```golang
service, err := webpay.NewPlusNormal(privateCert, publicCert, commerceCode, commerceEmail, webpay.ServicePatpass, webpay.EnvironmentProduction)
if err != nil {
  log.Fatalln(err)
}

result, err := service.GetTransactionResult(token)
if err != nil {
  log.Fatalln(err)
}

log.Println("BuyOrder", result.BuyOrder) // ordenCompra12345678
log.Println("CardDetail.CardNumber", result.CardDetail.CardNumber) // 0568
...
log.Println("DetailOutput.Amount", result.DetailOutput.Amount) // 10000
log.Println("DetailOutput.ResponseCode", result.DetailOutput.ResponseCode) // -1
...
log.Println("VCI", result.VCI) // TSN
```

Puedes ver más ejemplos sobre la implementación los demás servicios en la carpeta `/cmd`

# Testing (WIP)

```bash
make test
```

# Montar WebService API HTTP con Docker (WIP)

También es posible correr con Docker un servidor HTPP que expone las siguientes rutas.

```
  POST /api/v1/plus-normal/transactions
  GET  /api/v1/plus-normal/transactions/:token

  POST /api/v1/patpass/transactions
  GET  /api/v1/patpass/transactions/:token
```

Para ello solo debes usar la imagen de docker `microapis/webpay-api`.

# Tareas Pendientes

- [x] SOAP: soporte a los posibles errores que pueda devolver el servidor.
- [ ] SOAP: verificar si la firma del XML en la respuesta es válida con los certificados designados.
- [x] Plus Normal: implementar método para `InitTransaction` con SOAP.
- [ ] Plus Normal: implementar test para `InitTransaction` con SOAP.
- [x] Plus Normal: implementar método para `GetTransactionResult` con SOAP.
- [ ] Plus Normal: implementar test para `GetTransactionResult` con SOAP.
- [ ] Plus Mall: implementar método para Plus Mall con SOAP/HTTP.
- [ ] Plus Mall: implementar test para Plus Mall con SOAP/HTTP.
- [x] Patpass: implementar método `InitTransaction` con SOAP.
- [ ] Patpass: implementar test para `InitTransaction` con SOAP.
- [x] Patpass: implementar método `GetTransactionResult` con SOAP.
- [ ] Patpass: implementar test para `GetTransactionResult` con SOAP.
- [ ] One Click: implementar métodos para OneClick usando SOAP/HTTP.
- [ ] One Click: implementar tests para OneClick usando SOAP/HTTP.
- [ ] One Click Mall: implementar métodos para OneClick Mall usando SOAP/HTTP.
- [ ] One Click Mall: implementar tests para OneClick Mall usando SOAP/HTTP.
- [ ] Capture: implementar métodos para Capture usando SOAP/HTTP.
- [ ] Capture: implementar tests para Capture usando SOAP/HTTP.
- [ ] Nullify: implementar métodos para Nullify usando SOAP/HTTP.
- [ ] Nullify: implementar tests para Nullify usando SOAP/HTTP.
- [ ] API Rest: implementar package http para montar un webservice usando un docker.
