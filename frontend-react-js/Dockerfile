# Build stage
FROM node:16.18 AS builder

WORKDIR /frontend-react-js
COPY package*.json ./
RUN npm install --production
COPY . .
RUN npm run build

# Runtime stage
FROM node:16.18-alpine AS runtime

WORKDIR /frontend-react-js
ENV PORT=3000

COPY --from=builder /frontend-react-js/build ./build
COPY --from=builder /frontend-react-js/start_npm.sh .
RUN chmod +x start_npm.sh

EXPOSE ${PORT}
ENTRYPOINT ["/frontend-react-js/start_npm.sh"]
