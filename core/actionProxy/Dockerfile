#
# Licensed to the Apache Software Foundation (ASF) under one or more
# contributor license agreements.  See the NOTICE file distributed with
# this work for additional information regarding copyright ownership.
# The ASF licenses this file to You under the Apache License, Version 2.0
# (the "License"); you may not use this file except in compliance with
# the License.  You may obtain a copy of the License at
#
#     http://www.apache.org/licenses/LICENSE-2.0
#
# Unless required by applicable law or agreed to in writing, software
# distributed under the License is distributed on an "AS IS" BASIS,
# WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
# See the License for the specific language governing permissions and
# limitations under the License.
#

# Dockerfile for docker skeleton (useful for running blackbox binaries, scripts, or Python 3 actions) .
FROM ubuntu:22.04
#CMD ["tail", "-f", "/dev/null"]

RUN apt -y update
#RUN apt -y upgrade
RUN ln -s /usr/share/zoneinfo/Europe/Bratislava /etc/localtime
#RUN apt -y install tzdata software-properties-common
#RUN apt -y install python2 python3
RUN apt -y install python3
#RUN update-alternatives --install /usr/bin/python python /usr/bin/python2 1
RUN update-alternatives --install /usr/bin/python python /usr/bin/python3 2
#RUN apt -y install perl jq zip git curl wget openssl ca-certificates sed openssh-client
#RUN update-ca-certificates
#RUN apt -y install bzip2 libbz2-dev g++ libc-dev
RUN apt -y install pip
#RUN pip install --upgrade pip setuptools six
COPY requirements.txt .
# COPY ubuntuhandbook1-ubuntu-apps-jammy.list /etc/apt/sources.list.d/
RUN pip install --no-cache-dir -r requirements.txt
#RUN DEBIAN_FRONTEND=noninteractive apt-get install keyboard-configuration
#RUN apt install -y software-properties-common
RUN echo 'deb [signed-by=/usr/share/keyrings/ubuntuhandbook1.gpg] https://ppa.launchpadcontent.net/ubuntuhandbook1/apps/ubuntu jammy main' > /etc/apt/sources.list.d/ubuntuhandbook1-ubuntu-apps-jammy.list
RUN apt-key --keyring /usr/share/keyrings/ubuntuhandbook1.gpg adv --keyserver hkp://keyserver.ubuntu.com:80 --recv-keys 4c1cbe14852541cb
RUN apt update
# RUN add-apt-repository -y ppa:ubuntuhandbook1/apps
RUN apt -y install hugin hugin-data hugin-tools

ENV FLASK_PROXY_PORT 8080

RUN mkdir -p /actionProxy/owplatform
ADD actionproxy.py /actionProxy/
ADD owplatform/__init__.py /actionProxy/owplatform/
ADD owplatform/knative.py /actionProxy/owplatform/
ADD owplatform/openwhisk.py /actionProxy/owplatform/

RUN mkdir -p /action
ADD action.py /action/exec
ADD settings.py /action/
RUN chmod a+x /action/exec

CMD ["/bin/bash", "-c", "cd actionProxy && python -u actionproxy.py"]
