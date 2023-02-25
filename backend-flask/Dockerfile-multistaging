# Build stage
FROM python:3.10-slim-buster AS builder

WORKDIR /backend-flask

COPY requirements.txt requirements.txt
RUN pip3 install --no-cache-dir --user -r requirements.txt
ENV PATH=/root/.local/bin:$PATH

COPY . .

# Runtime stage
FROM python:3.10-slim-buster AS runtime

WORKDIR /backend-flask

COPY --from=builder /root/.local /root/.local
COPY --from=builder /backend-flask .

ENV FLASK_ENV=development
EXPOSE ${PORT}

RUN chmod +x start_flask.sh
ENTRYPOINT ["/backend-flask/start_flask.sh"]
