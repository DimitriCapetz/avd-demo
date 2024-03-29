# Container Image
FROM python:3.9

# Install additional utils
RUN apt update --allow-insecure-repositories && apt install -y sudo \
                                 vim \
                                 git \
                                 zip \
                                 cloc \
                                 dnsutils \
                                 zsh \
                                 zsh-syntax-highlighting \
                                 zsh-doc \
                                 less \
                                 liquidprompt \
                                 software-properties-common

RUN apt -y dist-upgrade

ARG USERNAME=vscode
ARG USER_UID=1000
ARG USER_GID=$USER_UID

# Create the user
RUN groupadd --gid $USER_GID $USERNAME \
    && useradd --uid $USER_UID --gid $USER_GID -m $USERNAME \
    #
    # [Optional] Add sudo support. Omit if you don't need to install software after connecting.
    # && apt-get update \ # redundant
    # && apt-get install -y sudo \ # redundant
    && echo $USERNAME ALL=\(root\) NOPASSWD:ALL > /etc/sudoers.d/$USERNAME \
    && chmod 0440 /etc/sudoers.d/$USERNAME

# Install Python Packages
RUN pip install --upgrade pip
RUN pip install dotbot docutils rcvpapi Pyyaml lxml jsonrpclib ansible-core==2.14.8 cvprac pyeapi jsonschema treelib

# Install AVD and CVP Collections
RUN ansible-galaxy collection install arista.avd community.general
RUN export ARISTA_AVD_DIR=$(ansible-galaxy collection list arista.avd --format yaml | head -1 | cut -d: -f1)
#RUN pip3 install -r ${ARISTA_AVD_DIR}/arista/avd/requirements.txt
