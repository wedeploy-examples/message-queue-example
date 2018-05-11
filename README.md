[![Deploy](https://cdn.wedeploy.com/images/deploy.svg)](https://console.wedeploy.com/deploy?repo=https://github.com/wedeploy-examples/message-queue-example)

# WeDeploy Message Queue

An example of WeDeploy Message Queue.

## Instructions

1. Install the [WeDeploy CLI](https://wedeploy.com/docs/intro/using-the-command-line/).
2. Clone this repository.
3. Open the project with your command line and run `we deploy -p yourproject`.

## API Endpoints

* [Queue](#queue)
    * [Create Queue](#create-queue)
    * [Get a Queue](#get-a-queue)
    * [Delete a Queue](#delete-a-queue)
    * [List all Queues](#list-all-queues)
* [Message](#message)
  * [Send new Message to a Queue](#send-new-message-to-a-queue)
  * [Receive next Message from a Queue](#receive-next-message-from-a-queue)
  * [Change Message visibility](#change-message-visibility)

## Queue

### Create Queue

```http
POST /queues/:qname
```

##### Parameters

| Name          | Type    |     Required    | Options   |
| ------------- | ------- |  -------------  | --------- |
| qname         | string  |  ✓              | The Queue name. Maximum 80 characters; alphanumeric characters, hyphens (-), and underscores (_) are allowed. |
| vt            | number  |                 | The length of time, in seconds, that a message received from a queue will be invisible to other receiving components when they ask to receive messages. Allowed values: 0-9999999. Default: 30.  |
| delay         | number  |                 | The time in seconds that the delivery of all new messages in the queue will be delayed. Allowed values: 0-9999999. Default: 0. |
| maxsize       | string  |                 | The maximum message size in bytes. Allowed values: 1024-65536. Default: 65536. |

##### Request

```bash
curl -X "POST" "/queues/myqueue" \
     -H 'Content-Type: application/json; charset=utf-8' \
     -d $'{
  "vt": 30,
  "delay": 0,
  "maxsize": 2048
}'
```

##### Response `200 OK`

```js
{
  "result": 1
}
```

### Get a Queue

```http
GET /queues/:qname
```

##### Parameters

| Name          | Type    |     Required    | Options   |
| ------------- | ------- |  -------------  | --------- |
| qname         | string  |  ✓              | The Queue name.  |

##### Request

```bash
curl "/queues/myqueue"
```

##### Response `200 OK`

```js
{
  "vt": 30,
  "delay": 0,
  "maxsize": 2048,
  "totalrecv": 1,
  "totalsent": 1,
  "created": 1526062621,
  "modified": 1526062621,
  "msgs": 1,
  "hiddenmsgs": 0
}
```

### Delete a Queue

```http
DELETE /queues/:qname
```

##### Parameters

| Name          | Type    |     Required    | Options   |
| ------------- | ------- |  -------------  | --------- |
| qname         | string  |  ✓              | The Queue name.  |

##### Request

```bash
curl -X "DELETE" "/queues/myqueue"
```

##### Response `200 OK`

```js
{
  "result": 1
}
```

### List all Queues

```http
GET /queues
```

##### Request

```bash
curl "/queues"
```

##### Response `200 OK`

```js
{
  "queues": [
    "myqueue"
  ]
}
```

## Message

### Send new Message to a Queue

```http
POST /messages/:qname
```

##### Request

```bash
curl -X "POST" "/messages/myqueue" \
     -H 'Content-Type: application/json; charset=utf-8' \
     -d $'{
  "message": "Hello World"
}'
```

##### Response `200 OK`

```js
{
  "id": "f0y01ynxe3zAcfhFt15R7ehTwMntN9Zr"
}
```

### Receive next Message from a Queue

```http
GET /messages/:qname
```

##### Parameters

| Name          | Type    |     Required    | Options   |
| ------------- | ------- |  -------------  | --------- |
| vt            | string  |  ✓              | The length of time, in seconds, that a message received from a queue will be invisible to other receiving components when they ask to receive messages. Allowed values: 0-9999999. Default: 30 |

##### Request

```bash
curl "/messages/myqueue?vt=30"
```

##### Response `200 OK`

```js
{
  "id": "f0y01ynxe3zAcfhFt15R7ehTwMntN9Zr",
  "message": "Hello World",
  "rc": 2,
  "fr": 1526063521780,
  "sent": 1526063431732
}
```

### Change Message visibility

```http
PUT /message/:qname/:id
```

##### Parameters

| Name          | Type    |     Required    | Options   |
| ------------- | ------- |  -------------  | --------- |
| vt            | string  |  ✓              | The length of time, in seconds, that a message received from a queue will be invisible to other receiving components when they ask to receive messages. Allowed values: 0-9999999. Default: 30 |

##### Request

```bash
curl -X "PUT" "/messages/myqueue/f0y01ynxe3zAcfhFt15R7ehTwMntN9Zr?vt=30"
```

##### Response `200 OK`

```js
{
  "result": 1
}
```

## License

[BSD-3-Clause](./LICENSE.md), © Liferay, Inc.
