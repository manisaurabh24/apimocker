# Reference Mock Server

Reference mock server implemented using
[Node.js](https://nodejs.org/),
[express](https://github.com/expressjs/express), and
[swagger-express-middleware](https://github.com/BigstickCarpet/swagger-express-middleware).

For details on usage, see the readme for the
[TPP Reference Server](https://github.com/OpenBankingUK/tpp-reference-server#readme).

## Use latest release

Use the latest release [v0.7.0](https://github.com/OpenBankingUK/reference-mock-server/releases/tag/v0.7.0).

To obtain the latest release:

```sh
git clone https://github.com/OpenBankingUK/reference-mock-server
git checkout v0.7.0
```

Note: latest `master` branch code is actively under development and may not be stable.

## To run

Install npm packages:

```sh
npm install
```

Several environment variables are used to configure the mock server.

The mock server reads swagger files to generate endpoints for Account Information
and Payment Initiation resources. The path or URI to JSON swagger specification
files are passed on startup using environment variables `ACCOUNT_SWAGGER` and
`PAYMENT_SWAGGER`.

To run, copy the `.env.sample` file to a local `.env` file, and run using foreman:

```sh
cp .env.sample .env
npm run foreman
# [OKAY] Loaded ENV .env File as KEY=VALUE Format
# web.1 | log running on localhost:8001 ...
```

The `.env` file configures the following variables:

* `ACCESS_TOKEN=<access-token-value>`
* `ACCOUNT_SWAGGER=<JSON swagger spec URI or file path>`
* `AUTHORISATION_CODE=<auth-code-value>`
* `BANK_DATA_DIRECTORY=abcbank`
* `CLIENT_ID=<client-id-value>`
* `CLIENT_SECRET=<client-secret-value>`
* `DEBUG =error,log`
* `HOST=http://localhost:8001`
* `OPENID_ASPSP_AUTH_HOST=http://localhost:8001`
* `OPENID_CONFIG_ENDPOINT_URL=http://localhost:8001/openid/config`
* `PAYMENT_SWAGGER=<JSON swagger spec URI or file path>`
* `PORT=8001`
* `USER_DATA_DIRECTORY=alice`
* `VERSION=v1.1`

## ASPSP resource server mock data

Currently mock data is read off the file system.

For example, given the file
`./data/abcbank/alice/accounts.json` exists, then
GET requesting `/accounts` returns the JSON in that file.

```sh
curl -H "Authorization: Bearer 2YotnFZFEjr1zCsicMWpAA" \
     -H "Accept: application/json" \
     -H "x-fapi-financial-id: bbbUB4fPIYB0k1m" \
     http://localhost:8001/open-banking/v1.1/accounts

# {"Data":[{"AccountId":"22290","Currency"...
```

## Deploy to heroku

To deploy to heroku for the first time from a Mac:

```sh
brew install heroku

heroku login

heroku create --region eu

heroku apps:rename <newname>

heroku config:set ACCESS_TOKEN=<access-token-value>

heroku config:set ACCOUNT_SWAGGER=<JSON swagger spec URI>

heroku config:set AUTHORISATION_CODE=<auth-code-value>

heroku config:set BANK_DATA_DIRECTORY=abcbank

heroku config:set CLIENT_ID=<client-id-value>

heroku config:set CLIENT_SECRET=<client-secret-value>

heroku config:set DEBUG=error,log

heroku config:set HOST=https://<heroku-host-domain>

heroku config:set
OPENID_ASPSP_AUTH_HOST=https://<heroku-host-domain>

heroku config:set OPENID_CONFIG_ENDPOINT_URL=https://<heroku-host-domain>/openid/config

heroku config:set PAYMENT_SWAGGER=<JSON swagger spec URI>

heroku config:set USER_DATA_DIRECTORY=alice

heroku config:set VERSION=v1.1

git push heroku master
```

To test (basic):
```sh
curl https://<newname>.herokuapp.com/health
```

To test (advanced):
```sh
curl -H "Authorization: Bearer 2YotnFZFEjr1zCsicMWpAA" \
     -H "Accept: application/json" \
     -H "x-fapi-financial-id: bbbUB4fPIYB0k1m" \
     https://<newname>.herokuapp.com/open-banking/v1.1/accounts

# {"Data":[{"AccountId":"22290","Currency"...
```
