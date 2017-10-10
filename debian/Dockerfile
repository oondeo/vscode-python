FROM python:3

#bashrc
#export PATH=/var/lib/trytond/.npm-packages/bin:/developer/.env/bin:/developer/trytond/bin:$PATH
#source /developer/.env/bin/activate

#sudo echo 0 > /proc/sys/kernel/yama/ptrace_scope 
#--security-opt seccomp=unconfined
#--cap-add sys_ptrace
#deb http://apt.llvm.org/jessie/ llvm-toolchain-jessie-4.0 main
#deb-src http://apt.llvm.org/jessie/ llvm-toolchain-jessie main
#wget -O - http://apt.llvm.org/llvm-snapshot.gpg.key|sudo apt-key add -

USER root
RUN apt-get update && apt-get install --no-install-recommends -y dash curl apt-transport-https \
        libpq-dev \
        libldap2-dev \
        libsasl2-dev \
        libxslt-dev \
        libffi-dev \
        libxml2-dev \
    && rm -rf /var/cache/apt/* /var/lib/apt/* /var/tmp/* /tmp/* /var/log/* /usr/share/doc/* /usr/share/man/* 

RUN curl https://packages.microsoft.com/keys/microsoft.asc | gpg --dearmor > microsoft.gpg \
    && mv microsoft.gpg /etc/apt/trusted.gpg.d/microsoft.gpg \
    && echo "deb [arch=amd64] https://packages.microsoft.com/repos/vscode stable main" > /etc/apt/sources.list.d/vscode.list 

#latest version: code-insiders
RUN curl -sL https://deb.nodesource.com/setup_4.x | bash - \
    && apt-get update && apt-get install --no-install-recommends -y socat code nodejs firefox-esr gcc vim zsh mercurial libc6-dev libgtk2.0-0 libgtk-3-0 libpango-1.0-0 libcairo2 libfontconfig1 libgconf2-4 libnss3 libasound2 libxtst6 unzip libglib2.0-bin libcanberra-gtk-module libgl1-mesa-glx curl build-essential gettext libstdc++6 git automake libtool autogen nodejs libnotify-bin aspell aspell-en gvfs-bin libxss1 x11-xserver-utils \
#    python-pip python-virtualenv virtualenv python-dev ipython pylint python-ipdb \
#  && ln -s /usr/bin/nodejs /usr/bin/node \
    && npm install -g grunt bower \
    && pip3 install pylint ipython ipdb \
    && && mkdir -p /usr/share/zsh/share && curl -L git.io/antigen > /usr/share/zsh/share/antigen.zsh \ 
    && rm -rf /var/cache/apt/* /var/lib/apt/* /var/tmp/* /tmp/* /var/log/* /usr/share/doc/* /usr/share/man/* /root/.cache

#ENV VSCODE_VERSION=1.16.1
#ENV VSCODE_VERSION=latest
#RUN curl -o /tmp/vscode.tgz https://vscode-update.azurewebsites.net/${VSCODE_VERSION}/linux-x64/stable \
#    && tar -zxf /tmp/vscode.tgz

#echo prefix = ${HOME}/.npm-packages > /home/bitnami/.npmrc
#PATH=${HOME}/.npm-packages/bin:$PATH
#flake8 yapf


# install flat plat theme
run wget 'https://github.com/nana-4/Flat-Plat/releases/download/3.20.20160404/Flat-Plat-3.20.20160404.tar.gz'
run tar -xf Flat-Plat*
run mv Flat-Plat /usr/share/themes
run rm Flat-Plat*gz
run mv /usr/share/themes/Default /usr/share/themes/Default.bak
run ln -s /usr/share/themes/Flat-Plat /usr/share/themes/Default

# install hack font
run wget 'https://github.com/chrissimpkins/Hack/releases/download/v2.020/Hack-v2_020-ttf.zip'
run unzip Hack*.zip
run mkdir /usr/share/fonts/truetype/Hack
run mv Hack* /usr/share/fonts/truetype/Hack
run fc-cache -f -v

# create our developer user
workdir /root
run groupadd -r users -g 100 || true
run groupadd -r users || true
run useradd -o -u 1000 -r -g users -d /developer -s /bin/bash -c "Software Developer" developer
copy /developer /developer
copy ep /usr/local/bin/ep
RUN echo "prefix = ~/.npm" > /developer/.npmrc
workdir /developer
RUN ln -s /usr/bin/firefox /bin/xdg-open
run chown -R developer:users /developer


#additional install
RUN apt-get -y update; apt-get install --no-install-recommends -y \
        freetds-dev \
        libldap2-dev \
        libsasl2-dev \
        libpq-dev \
        libmariadb-client-lgpl-dev \
        libmariadb-client-lgpl-dev-compat \
        sqlite sqlite3 \
        libsqlite3-dev libmagic-dev \
        graphviz \
        libgraphviz-dev \
        gsfonts \
        fonts-liberation \
        fonts-dejavu fonts-freefont-ttf ttf-bitstream-vera fonts-freefont-otf fonts-lyx xfonts-100dpi xfonts-75dpi texlive-extra-utils texlive-math-extra texlive-fonts-extra texlive-font-utils texlive ttf-aenigma fonts-roboto \
        libxml2-dev \
        libxslt-dev \
        libjpeg-dev \
        libreadline-dev \
        libffi-dev \
        libncurses5-dev \
        librabbitmq-dev \
        rabbitmq-server \
        libreoffice-script-provider-python \
        libreoffice-writer \
        libreoffice-calc \
        redis-tools \
        cifs-utils \
    && curl -L https://raw.githubusercontent.com/dagwieers/unoconv/master/unoconv > /usr/local/bin/unoconv \
    && sed -i '1 s/env.*$/python/' /usr/local/bin/unoconv && chmod +x /usr/local/bin/unoconv \
    && pip3 install pyuno \
    && rm -rf /var/cache/apt/* /var/lib/apt/* /var/tmp/* /tmp/* /var/log/* /usr/share/doc/* /usr/share/man/* /root/.cache



user developer

#install plugins
RUN /usr/bin/code --install-extension donjayamanne.python
RUN /usr/bin/code --install-extension msjsdiag.debugger-for-chrome

# set environment variables
env PATH /developer/.npm/bin:$PATH
env NODE_PATH /developer/.npm/lib/node_modules:$NODE_PATH
env BROWSER /usr/bin/firefox
env SHELL /bin/bash
env DISPLAY=:0
#to make it working:
#- xhost +
#- edit lightdm.conf xserver-allow-tcp=true
#set DISPLAY
#socat EXEC:'docker exec -i -u root vscode socat unix-l\:/tmp/.X11-unix/X0 -' unix:/tmp/.X11-unix/X0


CMD ["code --verbose"]

##mount .gitconfig and .ssh dirs 
#docker run -v ~/.ssh:/developer/.ssh -v ~/.gitconfig:/developer -e DISPLAY=172.17.0.1:6000 oondeo/vscode-python
#pypy3 -m venv env
