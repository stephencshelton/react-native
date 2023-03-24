FROM ubuntu:22.04

# Update package lists and install necessary dependencies
RUN apt-get update && apt-get install -y curl apt-transport-https gnupg

# Add the Yarn repository to your sources list
RUN curl -sS https://dl.yarnpkg.com/debian/pubkey.gpg | apt-key add - && \
    echo "deb https://dl.yarnpkg.com/debian/ stable main" | tee /etc/apt/sources.list.d/yarn.list

# Update package lists again and install Yarn
RUN apt-get update && apt-get install -y yarn