FROM node:18-alpine
# Install Python and essential build tools
RUN apk add --update --no-cache python3 make g++ && ln -sf python3 /usr/bin/python
RUN python3 -m ensurepip

# Using douban pipy mirror
RUN pip3 install -i https://pypi.douban.com/simple/ -U pip 
RUN pip3 config set global.index-url https://pypi.douban.com/simple/
RUN pip3 install --no-cache --upgrade pip setuptools

# Create the directory on the node image 
# where our Next.js app will live
RUN mkdir -p /app

# Set /app as the working directory
WORKDIR /app

# Copy package.json and yarn.lock
# to the /app working directory
COPY package*.json yarn.lock ./

# Install dependencies in /app
RUN yarn --registry=https://registry.npm.taobao.org/ install --network-timeout 1000000

# Copy the rest of our Next.js folder into /app
COPY . .

# Build the Next.js application
RUN yarn build

# Ensure port 3000 is accessible to our system
EXPOSE 3000

# Run yarn start, as we would via the command line 
CMD ["yarn", "start"]
