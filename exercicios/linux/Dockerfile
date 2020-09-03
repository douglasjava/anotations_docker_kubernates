FROM ubuntu

#Forma incorreta, pois se update ficar em cache, se o pacote novo que for instalra não contemplar ele vai dar erro
RUN apt-get update
RUN apt-get install --yes curl

#Forma certa, executar sequencialmente
RUN apt-get update && apt-get install --yes curl
