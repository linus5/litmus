FROM ubuntu

LABEL maintainer="OpenEBS"

#Installing necessary ubuntu packages
RUN rm -rf /var/lib/apt/lists/* && \
    apt-get clean && \
    apt-get update --fix-missing || true && \
    apt-get install -y python-minimal python-pip netcat iproute2 jq\
    curl openssh-client

#Installing ansible
RUN pip install ansible

#Installing Kubectl
ENV KUBE_LATEST_VERSION="v1.12.0"
RUN curl -L https://storage.googleapis.com/kubernetes-release/release/${KUBE_LATEST_VERSION}/bin/linux/amd64/kubectl -o /usr/local/bin/kubectl && \
    chmod +x /usr/local/bin/kubectl && \
    curl -o /usr/local/bin/aws-iam-authenticator https://amazon-eks.s3-us-west-2.amazonaws.com/1.10.3/2018-07-26/bin/linux/amd64/aws-iam-authenticator && \chmod +x /usr/local/bin/aws-iam-authenticator
    
#Adding hosts entries and making ansible folders
RUN mkdir /etc/ansible/ /ansible && \
    echo "[local]" >> /etc/ansible/hosts && \
    echo "127.0.0.1" >> /etc/ansible/hosts

#Copying Necessary Files
COPY providers/openebs/installers/ ./
COPY providers/cstor/ ./
COPY providers/ ./
COPY ./apps/ ./
COPY ./chaoslib ./chaoslib/
COPY ./funclib ./funclib/
COPY ./executor/ansible/plugins/callback/actionable.py /usr/local/lib/python2.7/dist-packages/ansible/plugins/callback/
COPY ./executor/ansible/plugins/callback/timestamp.py /usr/local/lib/python2.7/dist-packages/ansible/plugins/callback/
COPY ./hack/litmus-result.j2 ./
COPY ./common/utils ./common/utils/
