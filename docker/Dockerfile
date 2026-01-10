# define base image
FROM ubuntu:latest

# Update
RUN apt update && apt upgrade -y

# # Install needed items
# RUN apt install -y \
#   ca-certificates fonts-liberation gconf-service \
#   libappindicator1 libasound2 libatk-bridge2.0-0 libatk1.0-0 libc6 libcairo2  \
#   libcups2 libdbus-1-3 libexpat1 libfontconfig1 libgbm1 libgcc1 libgconf-2-4 \
#   libgdk-pixbuf2.0-0 libglib2.0-0 libgtk-3-0 libnspr4 libnss3 libpango-1.0-0 \
#   libpangocairo-1.0-0 libstdc++6 libx11-6 libx11-xcb1 libxcb1 libxcomposite1 \
#   libxcursor1 libxdamage1 libxext6 libxfixes3 libxi6 libxrandr2 libxrender1 \
#   libxss1 libxtst6 lsb-release libxshmfence1 chromium -y
# RUN apt install -y apt-transport-https lsb-release ca-certificates curl dirmngr software-properties-common

# Add utilities
RUN apt install -y sudo openssh-server git rsync wget curl gnupg ca-certificates

# Install MySQL
RUN apt install -y mysql-server
RUN apt install -y mysql-client

# Configure MySQL
RUN echo 'sql-mode = "STRICT_ALL_TABLES,NO_UNSIGNED_SUBTRACTION"' >> /etc/mysql/mysql.conf.d/mysqld.cnf
RUN echo 'authentication-policy=mysql_native_password' >> /etc/mysql/mysql.conf.d/mysqld.cnf

# Start MySQL
# RUN systemctl enable mysql-server
# RUN mkdir /var/run/mysqld; chown mysql.mysql /var/run/mysqld
# RUN mkdir /var/lib/mysql; chown mysql.mysql /var/lib/mysql
# RUN usermod -d /var/lib/mysql mysql
# RUN service mysql start

# Install redis
RUN apt install -y redis-server

# Install node 22
RUN NODE_MAJOR=22  # Hardcoded into noudesource.list line below
RUN sudo mkdir -p /etc/apt/keyrings
RUN curl -fsSL https://deb.nodesource.com/gpgkey/nodesource-repo.gpg.key | sudo gpg --dearmor -o /etc/apt/keyrings/nodesource.gpg
RUN echo "deb [signed-by=/etc/apt/keyrings/nodesource.gpg] https://deb.nodesource.com/node_22.x nodistro main" | sudo tee /etc/apt/sources.list.d/nodesource.list
RUN apt update
RUN apt install -y nodejs
RUN npm install -g npm@11.7.0

# Install yarn (without re-installing NodeJS)
RUN curl -sS https://dl.yarnpkg.com/debian/pubkey.gpg | sudo apt-key add -
RUN echo "deb https://dl.yarnpkg.com/debian/ stable main" | sudo tee /etc/apt/sources.list.d/yarn.list
RUN apt update && sudo apt install yarn --no-install-recommends

RUN npx playwright install chromium

# define working directory inside the container
WORKDIR /usr/src/app

# Copy all the source code from host machine to the container project directory
COPY . .

# # Install the node dependenicies
# RUN npm install --omit=dev  # SLOW > 300s

# Create the test user
RUN groupadd bhima
RUN useradd -m -d /home/bhima -s /bin/bash -g bhima bhima

# make sure the bhima test user is the owner of all the underlying files.
# RUN chown -R bhima:bhima /usr/src/app  # SLOW!

# # # # # yarn build creates the bin/ folder
# # # # COPY .env bin/
# # # 
# # change directory to the bin diretory
# WORKDIR /usr/src/app/bin/
# 
# # ensure this container runs as the user "node"
# # USER node
# USER bhima
