FROM ubuntu:16.04

ARG HOME=/home/vue

# Install basic packages to connect servers
RUN apt-get update && \
    apt-get install -y git openssh-client openssh-server curl

# Install Node.js from the Debian-based distributions repository
RUN curl -sL https://deb.nodesource.com/setup_8.x | bash - && \
    apt-get install -y nodejs && \
    npm install npm --global

# Remove redundancy
RUN apt-get -y autoremove && \
    apt-get -y clean && \
    rm -rf /var/lib/apt/lists/*

# Install yeoman, dependencies and generator
RUN npm install --global gulp-cli bower generator-chrome-extension

# Create an user to manage vue
RUN useradd --create-home -ms /bin/bash -d ${HOME} vueadmin

COPY ["app", "${HOME}/app"]
COPY ["test", "${HOME}/test"]
COPY [".babelrc", "bower.json", "gulpfile.babel.js", "package.json", "webpack.config.js", "${HOME}/"]

RUN chown -R vueadmin:vueadmin ${HOME}

USER vueadmin
WORKDIR ${HOME}

# Install packages
RUN npm install . && \
	npm install --save webpack webpack-stream babel-loader \
					vue vue-loader css-loader vue-template-compiler \
					aliasify babel babel-register 

RUN gulp babel && \
 	gulp build

CMD ['bash']
