FROM ubuntu:16.04

RUN dpkg --add-architecture i386
RUN apt-get update 

#ssh 서버 설치
RUN apt-get install -y openssh-server gdb

#32bit 파일 실행
RUN apt-get install -y libc6:i386 libncurses5:i386 libstdc++6:i386

#pwntools 설치
RUN apt-get update
RUN apt-get install -y python3 python3-pip python3-dev git libssl-dev libffi-dev build-essential
RUN python3 -m pip install --upgrade pip
RUN python3 -m pip install --upgrade pwntools

#vim 설치
RUN apt-get update
RUN apt-get install -y vim

RUN mkdir /var/run/sshd

#ssh 설정
RUN echo 'root:root' |chpasswd
RUN sed -ri 's/^#?PermitRootLogin\s+.*/PermitRootLogin yes/' /etc/ssh/sshd_config
RUN sed -ri 's/UsePAM yes/#UsePAM yes/g' /etc/ssh/sshd_config
RUN mkdir /root/.ssh
RUN apt-get clean && \
    rm -rf /var/lib/apt/lists/* /tmp/* /var/tmp/*
EXPOSE 22

#vimrc 설정
ADD ./setting/vimrc /etc/vim/vimrc

#fd설정
RUN mkdir /home/fd
RUN useradd -d /home/fd fd -s /bin/bash
RUN useradd -d /home/fd_pwn fd_pwn -s /bin/bash
RUN echo 'fd:guest' |chpasswd

ADD ./fd/fd /home/fd/fd
ADD ./fd/fd.c /home/fd/fd.c
ADD ./fd/flag /home/fd/flag

RUN chown root:fd /home/fd
RUN chmod 750 /home/fd

RUN chown fd_pwn:fd /home/fd/fd
RUN chmod 4550 /home/fd/fd

RUN chown root:root /home/fd/fd.c
RUN chmod 644 /home/fd/fd.c

RUN chown fd_pwn:root /home/fd/flag
RUN chmod 440 /home/fd/flag

CMD ["/usr/sbin/sshd", "-D"]

