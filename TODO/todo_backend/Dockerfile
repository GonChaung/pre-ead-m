FROM node:22-alpine

WORKDIR /usr/src/app

COPY TODO/todo_frontend ./todo_frontend
COPY TODO/todo_backend ./todo_backend

WORKDIR /usr/src/app/todo_frontend
RUN npm install
RUN npm run build

WORKDIR /usr/src/app/todo_backend
RUN npm install
RUN mkdir -p static
RUN cp -r /usr/src/app/todo_frontend/build /usr/src/app/todo_backend/static/

EXPOSE 5000

CMD ["node", "server.js"]
