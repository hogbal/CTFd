#bof
FROM ubuntu:latest

RUN dpkg --add-architecture i386

RUN apt-get update

RUN apt-get install -y libc6:i386 libncurses5:i386 libstdc++6:i386

RUN apt-get install -y xinetd netcat

RUN mkdir /home/pwn
RUN useradd -d /home/pwn pwn -s /bin/bash
RUN useradd -d /home/pwn_flag pwn_flag -s /bin/bash

#home directory setting
ADD ./pwn/bof /home/pwn/bof
ADD ./pwn/flag /home/pwn/flag

#directory permission setting
RUN chown -R root:pwn /home/pwn
RUN chmod -R 750 /home/pwn

#file permission setting
RUN chmod 555 /home/pwn/bof

#flag permission setting
RUN chown root:pwn /home/pwn/flag
RUN chmod 440 /home/pwn/flag

#xinetd setting
ADD ./settings/bof.xinetd /etc/xinetd.d/bof

CMD ["/usr/sbin/xinetd","-dontfork"]
