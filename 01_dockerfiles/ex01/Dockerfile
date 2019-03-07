# docker build -t tp_img .
# virtual voice server on 9987
# file transfers on 30033
# ServerQuery on 10011
# docker run --name tp_cont -p 9987:9987/udp -p 30033:30033 -p 10011:10011 tp_img
# run with TS3 client using 192.168.99.100 and enter the ServerAdmin privilege key

FROM debian

ENV TS3SERVER_LICENSE=accept

RUN apt-get update \
	&& apt-get install -y bzip2 wget \
	&& wget https://files.teamspeak-services.com/releases/server/3.6.1/teamspeak3-server_linux_amd64-3.6.1.tar.bz2 \
	&& tar xvf *.bz2

EXPOSE 9987/udp 10011 30033

ENTRYPOINT ["./teamspeak3-server_linux_amd64/ts3server_minimal_runscript.sh"]
