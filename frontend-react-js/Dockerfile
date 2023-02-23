FROM node:16.18

ENV PORT=3000

# COPY command copies content from <host OS> to <container> 
# The entire front-end application code is copied to the container
COPY . /frontend-react-js

# Shows the working directory entry point of the container
# It creates the directory in th econtainer
WORKDIR /frontend-react-js

# NPM is a node package manager, it helps to manage the node packages for the application
RUN npm install

EXPOSE ${PORT}

CMD ["npm", "start"]