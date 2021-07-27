# LINE Blockchain Developers SDK for JavaScript
This is a library to help developers to easily use the LINE Blockchain Developers API. As it's written by Typescript, it supports both Typescript and JavaScript.

It's a subproject of LINE Blockchain Developers SDK, see [README](../README.md) for an overview of the SDK.

# How to use
## Installation
Before getting started, you need to install the library to your project. 
To make installation easy, use package managers as the follows:

### npm
```
npm install @ryukato79/link-developers-sdk
```

### yarn
```
yarn add @ryukato79/link-developers-sdk
```

> Instead of using package managers, you can clone this repository as well. See [download and build](#download-and-build) for the details.

## Key objects and usage
### `HttpClient`
This class represents an HTTP client to connect and interact with the LINE Blockchain Developers API server. It provides functions to call the endpoints of the API with mandatory and optional parameters.
It's an entry point for this library, every dApp for LINE Blockchain Developers should have an instance of `HttpClient`.

Create an instance with your connection and authentication information as follows:

```javascript
import { HttpClient } from './lib/http-client-base';
const httpClient = new HttpClient(baseUrl, apiKey, apiSecret);
```

- `baseUrl` is the address of API server. Find one for the chain your service runs on in [API guide](https://docs-blockchain.line.biz/api-guide/).
- `apiKey` is your service's API key.
- `apiSecret` is your service's API secret. **Never** use the secret hard-coded in the source code.

Now, you can call any endpoints via the functions of the instance. A simple example is to get the server time:

```javascript
(async() => {
  const response = await httpClient.time();
  console.log(response['statusCode']);
})();
```

Remember that you must handle it in an asynchronous way.

### Request and response
When requesting, you can use predefined request data classes in `lib/request.ts`. Try to send a memo save request for example as follows:

```javascript
import { MemoRequest } from './lib/request';

(async() => {
  const request = new MemoRequest('my first memo', walletAddress, walletSecret);
  const response = await httpClient.createMemo(request);
})();
```

When you need to parse a JSON-formatted `responseData` in a response, find and use the proper response data class in `lib/response.ts`. To get the `txhash` or the above request for example:

```javascript
import { GenericResponse, TxResultResponse } from './lib/response';

(async() => {
  const request = new MemoRequest('my first memo', walletAddress, walletSecret);
  let response: GenericResponse<TxResultResponse> = await httpClient.createMemo(servcieId);
  console.log(response.responseData.txhash);
})();
```

### `SignatureGenerator`
This class provides a functionality to [generate signatures](https://docs-blockchain.line.biz/api-guide/Generating-Signature) for a request.

All API requests, except for the endpoint to retrieve the server time, must pass authentication information and be signed. Signing is a bit annoying, but never mind, fortunately, `HttpClient` itself will import this and generate signatures before sending a request. 

If you do want to study how LINE Blockchain signature created, it's okay to dive into the source code.

# How to contribute

## Download and build
If you're interested in contributing to this repository, download the source code and then build it using the following commands.

### npm
```
npm install
npm run build
```

### yarn
```
yarn
yarn run build
```

Once a build succeed, you're ready to implement your ideas.
## Test
After any modification, you must test the library to check whether to work correctly. 
We provide scripts for unit tests and integration test.

### Unit tests
Run the following command to run all unit tests.

```
yarn run test
```

### Integration tests
To run integration tests, `integration-test.env` is required with following properties.
```
HOST_URL=[api-url]
SERVICE_ID=[your service-id]
SERVICE_API_KEY=[your service-api-key]
SERVICE_API_SECRET=[your service-api-secret]
OWNER_ADDRESS=[your service wallet address]
OWNER_SECRET=[your service wallet secret]
OWNER_ADDRESS2=[your another service wallet address]
SERVICE_TOKEN_CONTRACT_ID=[your service-token contract-id]
ITEM_TOKEN_CONTRACT_ID=[your item-token contract-id]
LINE_USER_ID=[your line user id]
LINE_USER_WALLET_ADDRESS=[BitMax wallet address of the user]
```

If you have `integration-test.env` ready, then run an integration test by the following command.

```
yarn run test:integration
```


