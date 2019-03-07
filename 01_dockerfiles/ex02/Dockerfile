# git clone https://bitbucket.org/railstutorial/sample_app_4th_ed.git App
# docker image build -t ft-rails:on-build .
# create a child_img Dockerfile from the subject:
# echo $'FROM ft-rails:on-build\nEXPOSE 3000\nCMD ["rails", "s", "-b", "0.0.0.0", "-p" ,"3000"]' > Child_img
# docker image build -t sample -f Child_img .
# delete Abathur container
# docker container rm -f Abathur
# docker run -it -p 3000:3000 sample
# see it at http://192.168.99.100:3000/

FROM ruby:2.5.1

RUN apt-get update \
	&& apt-get install -y nodejs \
	&& gem install rails \
	&& gem install bundler

COPY App /opt/app
WORKDIR /opt/app

ONBUILD RUN bundle install --without production
ONBUILD RUN rake db:migrate
ONBUILD RUN bundle update
