# docker image build -t gitlab_img .
# docker run --name gitlab_cont -it -p 443:443 -p 80:80 -p 2222:22 --privileged gitlab_img
# go to https://192.168.99.100
# get your ssh key with cat ~/.ssh/id_rsa.pub and paste in in the gitlab settings
# to clone a repo:
# GIT_SSL_NO_VERIFY=true gcl https://192.168.99.100/bboucher/project.git
# then to avoid having to launch the command every time, inside the cloned folder:
# git config http.sslVerify false

FROM debian

# Init
RUN apt-get update \
	&& apt-get install -yq curl openssh-server ca-certificates

# Install Gitlab-ce
RUN curl -s https://packages.gitlab.com/install/repositories/gitlab/gitlab-ce/script.deb.sh | bash \
	&& apt-get install -yq gitlab-ce

# Config SSL cert
RUN mkdir -p /etc/gitlab/ssl \
	&& chmod 700 /etc/gitlab/ssl \
	&& openssl req -x509 -nodes -newkey rsa:4096 -days 365 \ 
		-keyout /etc/gitlab/ssl/ssl.key -out /etc/gitlab/ssl/ssl.crt \
		-subj /C=FR/ST=42/L=Paris/O=42/OU=Org/CN=192.168.99.100 \
	&& echo "external_url \"https://192.168.99.100\"" >> /etc/gitlab/gitlab.rb \
	&& echo "nginx['redirect_http_to_https'] = true" >> /etc/gitlab/gitlab.rb \
	&& echo "nginx['ssl_certificate'] = \"/etc/gitlab/ssl/ssl.crt\"" >> /etc/gitlab/gitlab.rb \
	&& echo "nginx['ssl_certificate_key'] = \"/etc/gitlab/ssl/ssl.key\"" >> /etc/gitlab/gitlab.rb

# Config SSH 
RUN sed -i 's/#PermitRootLogin prohibit-password/PermitRootLogin yes/' /etc/ssh/sshd_config \
	&& echo 'root:root' | chpasswd \
	&& echo "gitlab_rails['gitlab_shell_ssh_port'] = 2222" >> /etc/gitlab/gitlab.rb

# Expose web & ssh
EXPOSE 443 80 22

# Launch
CMD (service ssh start \
	&& /opt/gitlab/embedded/bin/runsvdir-start &) \
	&& gitlab-ctl reconfigure \
	&& gitlab-ctl tail
