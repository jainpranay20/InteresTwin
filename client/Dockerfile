FROM node:16

WORKDIR /usr/src/app

COPY package*.json ./

RUN npm ci --silent && npm install is-empty

COPY . .

EXPOSE 3000

CMD ["npm", "start"]
