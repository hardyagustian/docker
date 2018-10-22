FROM centos:latest
## Add repo Chrome
ARG REPO_CHROME=/etc/yum.repos.d/google-chrome.repo
RUN printf '[google-chrome] \
\nname=google-chrome \
\nbaseurl=http://dl.google.com/linux/chrome/rpm/stable/$basearch \
\nenabled=1 \ngpgcheck=1 \
\ngpgkey=https://dl-ssl.google.com/linux/linux_signing_key.pub' > $REPO_CHROME
RUN yum update -y
RUN yum update -y
RUN yum install epel-release -y
RUN yum install -y \
    wget \
    tar \
    gzip \
    unzip \
    google-chrome-stable \
    java-1.8.0-openjdk-devel

### Clear Disk
RUN yum clean all -y
RUN rm -rf /var/tmp/yum-*
RUN package-cleanup --quiet --leaves --exclude-bin
RUN rm -rf /var/cache/yum
RUN package-cleanup --oldkernels --count=1

## Install NodeJS
RUN curl --silent --location https://rpm.nodesource.com/setup_8.x | bash -
RUN yum install nodejs -y \
    gcc-c++ \
    make

# Set Java Home Location
ENV JAVA_HOME=/usr/lib/jvm/java-1.8.0-openjdk-1.8.0.181-3.b13.el7_5.x86_64
# Install Maven
RUN mkdir /usr/local/src/apache-maven
RUN wget https://archive.apache.org/dist/maven/maven-3/3.5.4/binaries/apache-maven-3.5.4-bin.tar.gz \
    && tar -xf apache-maven-3.5.4-bin.tar.gz \
    && rm -rf apache-maven-3.5.4-bin.tar.gz \
    && mv apache-maven-3.5.4/* /usr/local/src/apache-maven/

# Set Maven Home Location
ENV M2_HOME=/usr/local/src/apache-maven
ENV PATH=${M2_HOME}/bin:${PATH}
ENV CHROME_DRIVER=/opt/webdriver/chromedriver/linux64/2.42
ENV PATH=${CHROME_DRIVER}:${PATH}
#ENV http_proxy=http://acc.mobile%40hcg.homecredit.net:password1%24@proxy.homecredit.id:3128
#ENV https_proxy=http://acc.mobile%40hcg.homecredit.net:password1%24@proxy.homecredit.id:3128
# Install Chrome Driver
RUN mkdir /opt/webdriver
ARG CHROME_DRIVER_VERSION=2.42
ARG WEBDRIVER_LOCATION=/opt/webdriver
 
RUN curl -s -S -o /tmp/chromedriver_linux64.zip https://chromedriver.storage.googleapis.com/$CHROME_DRIVER_VERSION/chromedriver_linux64.zip \
    && unzip /tmp/chromedriver_linux64.zip -d /tmp \
    && rm /tmp/chromedriver_linux64.zip \
    && mkdir -p $WEBDRIVER_LOCATION/chromedriver/linux64/$CHROME_DRIVER_VERSION \
    && mv /tmp/chromedriver $WEBDRIVER_LOCATION/chromedriver/linux64/$CHROME_DRIVER_VERSION
 
# Add trust certs in java
#RUN update-ca-trust force-enable
RUN mkdir /root/.m2
COPY ./resources/country-id-nexus-settings.xml /root/
COPY ./resources/ /tmp/res/
RUN keytool -import \
	-trustcacerts \
	-storepass changeit \
	-noprompt \
	-alias nexusid \
	-file /tmp/res/nexusid.cer \
	-keystore "$JAVA_HOME/jre/lib/security/cacerts"
RUN keytool -import \
	-trustcacerts \
	-storepass changeit \
	-noprompt \
	-alias sonarid \
	-file /tmp/res/sonarid.cer \
	-keystore "$JAVA_HOME/jre/lib/security/cacerts"
RUN mkdir /opt/weblogic
COPY ./resources/wlfullclient.jar /opt/weblogic
#RUN mkdir /etc/pki/ca-trust/source/anchors
Copy ./resources/nexushomecreditid.crt /etc/pki/ca-trust/source/anchors
Copy ./resources/nexushomecreditid2.crt /etc/pki/ca-trust/source/anchors
RUN update-ca-trust
#RUN update-ca-trust extract && find /etc/pki/ca-trust/source/anchors/ -type f -exec chmod a+r {} \;
## Set Timezone
ENV TZ=Asia/Bangkok
RUN ln -snf /usr/share/zoneinfo/$TZ /etc/localtime && echo $TZ > /etc/timezone
## Add Proxy in image container
ENV http_proxy=http://acc.mobile%40hcg.homecredit.net:xxxxxxxxxx%24@proxy.homecredit.id:3128
ENV https_proxy=http://acc.mobile%40hcg.homecredit.net:xxxxxxxxxx%24@proxy.homecredit.id:3128
ENV no_proxy="127.0.0.1, localhost"
