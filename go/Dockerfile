# start with Ubuntu 16.04
FROM ubuntu:16.04
MAINTAINER Ashish Ranjan (aranjan@live.com)

RUN apt-get update && apt-get install -y \
 cmake \
 curl \
 g++ \
 git \
 wget \
 make \
 man-db \
 ssh \
 sudo \
 unzip \
 tcsh \
 exuberant-ctags \
 bash \
 openssl

# build and install vim (with lua)
# install pagkages
RUN apt-get update && apt-get install -y      \
    ncurses-dev libtolua-dev pandoc lynx                                && \
    ln -s /usr/include/lua5.2 /usr/include/lua                          && \
    ln -s /usr/lib/x86_64-linux-gnu/liblua5.2.so /usr/lib/liblua.so       

RUN cd /tmp                                                             && \
    git clone https://github.com/vim/vim.git                            && \
    cd vim                                                              && \
    ./configure --with-features=huge --enable-luainterp                    \
                      --enable-cscope --with-compiledby="aranjan"          \
                      --enable-gui=no                                   && \
    make                                                                && \
    make install                                                        && \
    apt-get remove -y ncurses-dev                                       && \
    apt-get autoremove -y                                               && \
    apt-get clean && rm -rf /var/lib/apt/lists/* /tmp/* /var/tmp/*


# Standarize on golang 1.10.3
ENV GOLANG_VERSION 1.10.3
ENV GOROOT /usr/local/go
ENV GOPATH /go
ENV PATH $GOROOT/bin:/usr/local/bin:$GOPATH/bin:$PATH

RUN wget -nv https://dl.google.com/go/go$GOLANG_VERSION.linux-amd64.tar.gz  && \
        tar -v -C /usr/local -xzf go$GOLANG_VERSION.linux-amd64.tar.gz      && \
        rm go$GOLANG_VERSION.linux-amd64.tar.gz                             && \
        mkdir -p $GOPATH/src                                                && \
        mkdir -p $GOPATH/bin                                                && \
        go get golang.org/x/tools/cmd/godoc                                 && \
        go get github.com/nsf/gocode                                        && \
        go get github.com/derekparker/delve/cmd/dlv                         && \
        go get golang.org/x/tools/cmd/goimports                             && \
        go get github.com/rogpeppe/godef                                    && \
        go get golang.org/x/tools/cmd/gorename                              && \
        go get github.com/golang/lint/golint                                && \
        go get github.com/kisielk/errcheck                                  && \
        go get github.com/jstemmer/gotags                                   && \
        mv $GOPATH/bin/* /usr/local/go/bin                                  && \
# cleanup
        rm -rf $GOPATH
#curl https://glide.sh/get | sh                                      && \


# end golang install

# add user
ENV DEV godev
ENV DEVHOME /home/$DEV

RUN useradd -ms /bin/bash $DEV                                          && \
    mkdir -p $DEVHOME                                                   && \
    echo "ALL            ALL = (ALL) NOPASSWD: ALL" >> /etc/sudoers     && \
    adduser $DEV sudo                                                   && \
    chown -R $DEV $DEVHOME 

# switch to $DEV as user 
USER $DEV
WORKDIR $DEVHOME
ENV HOME /home/$DEV
ENV GOPATH $HOME/go
ENV GOROOT /usr/local/go
ENV GOOS="linux"
ENV PATH $GOROOT/bin:$GOPATH/bin:$PATH

CMD ["/bin/bash"]
