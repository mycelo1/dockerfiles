FROM ubuntu:bionic

ENV DEBIAN_FRONTEND='noninteractive'
ENV TZ='America/Sao_Paulo'

WORKDIR /tmp

RUN apt-get update
RUN apt-get -y install \
  build-essential \
  curl \
  git \
  libasound2 \
  net-tools \
  software-properties-common \
  sudo \
  vim-gtk3 \
  wget \
  xserver-xorg-core

# netcore
RUN wget -q https://packages.microsoft.com/config/ubuntu/18.04/packages-microsoft-prod.deb -O packages-microsoft-prod.deb
RUN dpkg -i packages-microsoft-prod.deb
RUN add-apt-repository universe
RUN apt-get update
RUN apt-get install -y apt-transport-https
RUN apt-get update
RUN apt-get install -y dotnet-sdk-3.1

# vscode + csharp
RUN wget -q https://go.microsoft.com/fwlink/?LinkID=760868 -O vscode.deb
RUN dpkg -i vscode.deb >> /dev/null || exit 0
RUN apt-get -f -y install
RUN wget -q https://github.com/OmniSharp/omnisharp-vscode/releases/download/v1.21.17/csharp-1.21.17.vsix

# add user mycelo
RUN useradd -ms /bin/bash mycelo
RUN echo "mycelo:mycelo" | chpasswd
RUN echo "mycelo ALL=(ALL) ALL" >> /etc/sudoers

# login
USER mycelo
ENV DISPLAY='host.docker.internal:0.0'
WORKDIR /home/mycelo

# user config
RUN git clone --recurse-submodules https://github.com/mycelo1/vimfiles .vim
RUN code --install-extension /tmp/csharp-1.21.17.vsix
