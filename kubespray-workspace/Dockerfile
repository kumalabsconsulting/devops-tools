FROM redbeard28/ansible-terraform-workspace:0.2

USER root
RUN rm -rf /home/docs/*
COPY docs/. /home/docs/

ENV HELM_VERSION="v3.10.2" \
    KS9_VERSION="v0.26.7" \
    OCTANT_VERSION="0.25.1" \
    KUBESPRAY_VERSION="v2.20.0" \
    KREW_VERSION="v0.4.3"

# OCTAN_VERSION without the 'v'

USER abc
RUN sudo apt-get -y update \
    && sudo apt-get -y install python3-jinja2 \
    && echo "------------------------------------------------------ Kubectl" \
    && cd /tmp && curl -LO "https://dl.k8s.io/release/$(curl -L -s https://dl.k8s.io/release/stable.txt)/bin/linux/amd64/kubectl" \
    && install kubectl /home/abc/.local/bin/kubectl \
    && rm /tmp/kubectl \
    && echo "------------------------------------------------------ Helm" \
    && cd /tmp && wget https://get.helm.sh/helm-${HELM_VERSION}-linux-386.tar.gz \
    && tar -zxvf /tmp/helm-${HELM_VERSION}-linux-386.tar.gz \
    && chmod +x /tmp/linux-386/helm \
    && mv /tmp/linux-386/helm /home/abc/.local/bin/helm \
    && rm /tmp/helm-${HELM_VERSION}-linux-386.tar.gz \
    && rm -r /tmp/linux-386 \
    && echo "------------------------------------------------------ K9s" \
    && mkdir /tmp/k9s \
    && cd /tmp/k9s && wget https://github.com/derailed/k9s/releases/download/${KS9_VERSION}/k9s_Linux_x86_64.tar.gz \
    && tar -zxvf /tmp/k9s/k9s_Linux_x86_64.tar.gz \
    && chmod +x /tmp/k9s/k9s \
    && mv /tmp/k9s/k9s /home/abc/.local/bin/k9s \
    && rm -r /tmp/k9s \
    && echo "------------------------------------------------------ Octant" \
    && mkdir -p /tmp/octant \
    && cd /tmp/octant && wget https://github.com/vmware-tanzu/octant/releases/download/v${OCTANT_VERSION}/octant_${OCTANT_VERSION}_Linux-64bit.tar.gz \
    && tar -zxvf /tmp/octant/octant_${OCTANT_VERSION}_Linux-64bit.tar.gz \
    && chmod +x /tmp/octant/octant_${OCTANT_VERSION}_Linux-64bit/octant \
    && mv /tmp/octant/octant_${OCTANT_VERSION}_Linux-64bit/octant /home/abc/.local/bin/ \
    && rm -rf /tmp/octant 

RUN echo "------------------------------------------------------ kubespray" \
    && git clone -b ${KUBESPRAY_VERSION} --single-branch https://github.com/kubernetes-sigs/kubespray.git /home/project/kubespray \
    && pip install -r /home/project/kubespray/requirements-2.12.txt \
    && echo "------------------------------------------------------ Kube-shell" \
    && pipx install kube-shell \
    && echo "------------------------------------------------------ Kubescape" \
    && chmod 0750 /home/abc/.nix-profile/bin/ \
    && curl -s https://raw.githubusercontent.com/kubescape/kubescape/master/install.sh | /bin/bash \
    && echo "------------------------------------------------------ Krew" \
    && mkdir /tmp/krew \
    && cd /tmp/krew && wget https://github.com/kubernetes-sigs/krew/releases/download/${KREW_VERSION}/krew-linux_amd64.tar.gz \
    && tar -zxvf /tmp/krew/krew-linux_amd64.tar.gz \
    && chmod +x /tmp/krew/krew-linux_amd64 \
    && /tmp/krew/krew-linux_amd64 install krew \
    && rm -rf /tmp/krew/ \
    && echo "------------------------------------------------------ kubectl-aliases" \
    && cd /home/abc && wget https://raw.githubusercontent.com/ahmetb/kubectl-aliases/master/.kubectl_aliases \
    && echo '[ -f ~/.kubectl_aliases ] && source ~/.kubectl_aliases' >> /home/abc/.zshrc \
    && mkdir -p /home/abc/.ssh


# https://github.com/bloodorangeio/octant-helm

ENV OCTANT_DISABLE_OPEN_BROWSER=1 \ 
    OCTANT_LISTENER_ADDR=0.0.0.0:8035

ENV PATH="${KREW_ROOT:-$HOME/.krew}/bin:$PATH"

# Install additional codeserver extensions 
RUN echo "------------------------------------------------------ extensions" \
    && code-server --install-extension  ms-kubernetes-tools.vscode-kubernetes-tools \
    && code-server --install-extension  ipedrazas.kubernetes-snippets
