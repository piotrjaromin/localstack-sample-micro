# localstack-sample

Nodejs project with localstack integration tests. It uses dynamoDB, docker-compose allows for running integration tests in complete isolation from real AWS services.

docker-compose starts 3 containers:

- service with `POST` endpoint which saves data to dynamodb
- localstack container with dynamodb running
- aws setup container which creates dynamodb in localstack during startup

## Service

It is defined in `index.js`. It exposes single endpoint to which you can post json and it will be saved into dynamodb then you can read from that endpoint using aws-cli (or you can add get endpoint yourself)

You can save data in dynamodb by calling:

```bash
curl -X POST http://localhost:3000 -H "Content-type: application/json" -d '{"id": "test-id"}'
aws dynamodb scan --table-name items --endpoint-url http://localhost:4569  --output json
```

## How to run

docker, nodejs, npm must be installed

```bash
docker-compose build
docker-compose up
```

wait for everything to be set up and in separate terminal type:

```bash
npm run test:integration
```

You can check initialized services by going to http://localhost:8080 this will expose localstack console which should show sqs and dynamodb resources.
