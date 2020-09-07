FROM ubuntu
RUN apt-get update && apt-get install --yes sudo
RUN apt-get update && apt-get install --yes vim
RUN apt-get update && apt-get install --yes curl
RUN curl -LO "https://storage.googleapis.com/kubernetes-release/release/$(curl -s https://storage.googleapis.com/kubernetes-release/release/stable.txt)/bin/linux/amd64/kubectl"
RUN chmod +x ./kubectl
RUN sudo mv ./kubectl /usr/local/bin/kubectl
RUN kubectl version --client