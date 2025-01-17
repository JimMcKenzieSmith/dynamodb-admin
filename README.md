# Running dynamodb-admin on a Mac

## Usage with Docker
```bash
# pull in the dynamodb local image
$ docker pull amazon/dynamodb-local
```

Configure dynamodb-local by creating a file at `~/.aws/credentials` with the following contents:
```
[default]
aws_access_key_id = fake_access_key_id
aws_secret_access_key = fake_secret_access_key
```

And another file at `~/.aws/config` with the following contents:
```
[default]
region = us-east-1
output = json
```

### Run the DB and run the admin portal
```bash
# run dynamodb on port 8000
$ docker run -p 8000:8000 amazon/dynamodb-local

# run dynamodb-admin on port 8001
$ docker run -p 8001:8001 dynamodb-admin
```


[![npm](https://img.shields.io/npm/v/dynamodb-admin.svg)](https://www.npmjs.com/package/dynamodb-admin)
[![CircleCI](https://circleci.com/gh/aaronshaf/dynamodb-admin/tree/master.svg?style=svg)](https://circleci.com/gh/aaronshaf/dynamodb-admin/tree/master)
> GUI for [DynamoDB Local](https://aws.amazon.com/blogs/aws/dynamodb-local-for-desktop-development/) or [dynalite](https://github.com/mhart/dynalite).

## Usage

### Use as a globally installed app

```bash
npm install dynamodb-admin -g
export DYNAMO_ENDPOINT=http://localhost:8000
dynamodb-admin
```

Options:
 - --open / -o - opens server URL in a default browser on start
 - --port PORT / -p PORT -  Port to run on (default: 8001)

You can also specify port to run on by setting environment variable `PORT` to given number. This will override value specified on the command line. This is legacy way to specify PORT.

If you use a local dynamodb that cares about credentials, you can configure them by using the following environment variables `AWS_REGION` `AWS_ACCESS_KEY_ID` `AWS_SECRET_ACCESS_KEY`

For example with the `amazon/dynamodb-local` docker image you can launch `dynamodb-admin` with:

```bash
AWS_REGION=eu-west-1 AWS_ACCESS_KEY_ID=local AWS_SECRET_ACCESS_KEY=local dynamodb-admin
```

### Use as a library in your project

```js
const AWS = require('aws-sdk');
const {createServer} = require('dynamodb-admin');

const dynamodb = new AWS.DynamoDB();
const dynClient = new AWS.DynamoDB.DocumentClient({service: dynamodb});

const app = createServer(dynamodb, dynClient);

const port = 8001;
const server = app.listen(port);
server.on('listening', () => {
  const address = server.address();
  console.log(`  listening on http://0.0.0.0:${address.port}`);
});
```

## See also

* [aaronshaf/dynamodb-admin](https://hub.docker.com/r/aaronshaf/dynamodb-admin/) - docker image with dynamodb-admin only for integrating with your own stack
* [instructure/dynamo-local-admin-docker](https://github.com/instructure/dynamo-local-admin-docker) - docker file with integrated dynamodb-admin and dynamodb

## Screencast

![Screencast](https://d3vv6lp55qjaqc.cloudfront.net/items/2S1m213N1o2L231e011o/Screen%20Recording%202016-10-17%20at%2001.11%20PM.gif?X-CloudApp-Visitor-Id=ab2071d5f76f8504ab6d3070d8a2c5c3&v=e6056da9)
