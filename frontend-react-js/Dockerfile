FROM node:16.18

ENV PORT=3000

COPY . /frontend-react-js
WORKDIR /frontend-react-js
RUN npm install
EXPOSE ${PORT}
#CMD ["npm", "start"]

COPY start_npm.sh /frontend-react-js
RUN chmod +x /frontend-react-js/start_npm.sh
ENTRYPOINT ["/frontend-react-js/start_npm.sh"]
