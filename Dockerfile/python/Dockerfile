FROM centos:latest
RUN yum install -y https://centos7.iuscommunity.org/ius-release.rpm
RUN yum update -y
RUN yum install epel-release -y
RUN yum install -y \
    wget \
    tar \
    gzip \
    unzip \
    which \
    openssh-clients \
    python-pip \
    yum-utils 

### Install Python
RUN yum install -y python36u python36u-libs python36u-devel python36u-pip

### Clear Disk
RUN yum clean all -y
RUN rm -rf /var/tmp/yum-*
RUN package-cleanup --quiet --leaves --exclude-bin
RUN rm -rf /var/cache/yum
RUN package-cleanup --oldkernels --count=1



RUN mkdir /opt/weblogic
COPY ./resources/wlfullclient.jar /opt/weblogic
Copy ./resources/nexushomecreditid.crt /etc/pki/ca-trust/source/anchors
Copy ./resources/nexushomecreditid2.crt /etc/pki/ca-trust/source/anchors
RUN update-ca-trust
#RUN update-ca-trust extract && find /etc/pki/ca-trust/source/anchors/ -type f -exec chmod a+r {} \;
## Set Timezone
ENV TZ=Asia/Bangkok
RUN ln -snf /usr/share/zoneinfo/$TZ /etc/localtime && echo $TZ > /etc/timezone
#ENV http_proxy=http://acc.mobile%40hcg.homecredit.net:password1%24@proxy.homecredit.id:3128
#ENV https_proxy=http://acc.mobile%40hcg.homecredit.net:password1%24@proxy.homecredit.id:3128
#ENV no_proxy="nexus.homecredit.id, 127.0.0.1, localhost"

