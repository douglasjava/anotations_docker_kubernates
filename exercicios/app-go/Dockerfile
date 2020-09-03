#Actor: Douglas Dias
#Criar processos separados (uma imagem de build e uma imagem de execução)
#Funciona para aplicações que não são interpretadas, mas para aplicações compiladas (ex: java, .Net, Go)

FROM golang:1.7.3 as build
WORKDIR /build
COPY main.go .
RUN CGO_ENABLED=0 GOOS=linux go build -a -installsuffix cgo -o main .
CMD ["./main"]

FROM alpine:latest
WORKDIR /app
COPY --from=build /build/main .
CMD [ "./main" ]