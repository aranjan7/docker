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
 openssl \
 python2.7 \
 python-pip

# build and install vim (with lua && python)
# install pagkages
RUN apt-get remove --purge vim vim-runtime vim-gnome vim-tiny vim-common vim-gui-common && \
    apt-get update 
RUN    apt-get install -y  ncurses-dev  pandoc lynx                          \
    build-essential liblua5.3-0 liblua5.3-dev python-dev python3-dev         \
    ruby-dev libperl-dev libncurses5-dev libgnome2-dev                       \
    libgnomeui-dev libgtk2.0-dev libatk1.0-dev vim-gnome                     \
    libbonoboui2-dev libcairo2-dev libx11-dev libxpm-dev libxt-dev        && \
    mkdir -p /usr/include/lua5.3/include                                  && \
    mkdir -p /usr/include/lua5.3/lib                                      && \
    cp /usr/include/lua5.3/*.h /usr/include/lua5.3/include/               && \
    ln -sf /usr/lib/x86_64-linux-gnu/liblua5.3.so /usr/include/lua5.3/lib/liblua.so && \
    ln -sf /usr/lib/x86_64-linux-gnu/liblua5.3.a /usr/include/lua5.3/lib/liblua.a

RUN cd /tmp                                                             && \
    git clone https://github.com/vim/vim.git                            && \
    cd vim && git checkout v8.0.0503 && make distclean                  && \
    ./configure --with-features=huge --enable-luainterp                    \
            --enable-rubyinterp \
            --enable-largefile \
            --disable-netbeans \
            --enable-pythoninterp \
            --with-python-config-dir=/usr/lib/python2.7/config-x86_64-linux-gnu \
            --enable-python3interp \
            --with-python3-config-dir=/usr/lib/python3.5/config-3.5m-x86_64-linux-gnu \
            --enable-perlinterp \
            --enable-luainterp \
            --enable-gui=auto \
            --enable-fail-if-missing \
            --with-lua-prefix=/usr/include/lua5.3 \
            --enable-cscope \
            --enable-multibyte \
            --prefix=/usr/local                                         && \
    make                                                                && \
    make install                                                        && \
    apt-get remove -y ncurses-dev                                       && \
    apt-get autoremove -y                                               && \
    apt-get clean && rm -rf /var/lib/apt/lists/* /tmp/* /var/tmp/*

CMD ["/bin/bash"]
