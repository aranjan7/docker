# start with Ubuntu 16.04
FROM aranjan7/go-docker:latest
MAINTAINER Ashish Ranjan (aranjan@live.com)

# add user
ENV DEV ardev
ENV DEVHOME /home/$DEV

RUN sudo useradd -ms /bin/tcsh $DEV                                          && \
    sudo mkdir -p $DEVHOME                                                   && \
    sudo adduser $DEV sudo                                                   && \
    sudo chown -R $DEV $DEVHOME 

# switch to $DEV as user 
USER $DEV
WORKDIR $DEVHOME
ENV HOME /home/$DEV
ENV GOPATH $HOME/go
ENV GOROOT /usr/local/go
ENV GOOS="linux"
ENV PATH $GOPATH/bin:$PATH


# setup vimrc etc
RUN git clone https://github.com/aranjan7/aranjan-setup.git             && \
    cd aranjan-setup && ./envsetup                                      && \
    vim +GoInstallBinaries +qall

# start tcsh
CMD ["/bin/tcsh"]
