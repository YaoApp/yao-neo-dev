# Neo Dev

## Environment variables

| Name            | Required       | Description     | Example                 |
| --------------- | -------------- | --------------- | ----------------------- |
| OPENAI_TEST_KEY | OpenAI API key | OpenAI API Key  | sk-xxxx                 |
| all_proxy       |                | Proxy for all   | socks5://127.0.0.1:7890 |
| http_proxy      |                | Proxy for HTTP  | http://127.0.0.1:7890   |
| https_proxy     |                | Proxy for HTTPS | http://127.0.0.1:7890   |

## Initialization & Start

```bash
yao migrate --reset
yao run scripts.demo.Data
yao start
```

## API

### Chat Stream

`GET /api/__yao/neo`

**Query String Parameters**

| Name    | Required | Description         | Example                                                |
| ------- | -------- | ------------------- | ------------------------------------------------------ |
| token   | true     | JWT token           | `Bearer eyJhbGciOiJ...`                                |
| content | true     | User input question | `Bearer eyJhbGciOiJ...`                                |
| context | true     | Router              | `{"stack":"Table-Page-pet","pathname":"/x/Table/pet"}` |

**Response Status**

| Status | Description     |
| ------ | --------------- |
| 200    | Success         |
| 4xx    | Parameter Error |
| 5xx    | Internal Error  |

**Response Headers**

Success:

| Name         | Value                           |
| ------------ | ------------------------------- |
| Content-Type | text/event-stream;charset=utf-8 |

Failure:

| Name         | Value                           |
| ------------ | ------------------------------- |
| Content-Type | application/json; charset=utf-8 |

**Response Event Stream**

Text:

```json
{ "text": "Hello" }
```

Done:

```json
{ "text": "Hello" }
```

Done With Command:

```json
{
  "done": true,
  "command": {
    "id": "table.data",
    "name": "Generate test data for the table",
    "request": "89c151e1-02bd-467d-8186-540f6cb36e36"
  }
}
```

Done With Actions:

```json
{
  "done": true,
  "actions": [
    {
      "name": "Back",
      "type": "Common.historyPush",
      "payload": { "pathname": "/x/Form/1" }
    }
  ]
}
```

### Get Chat History

`GET /api/__yao/neo/history`

**Query String Parameters**

| Name  | Required | Description | Example                 |
| ----- | -------- | ----------- | ----------------------- |
| token | true     | JWT token   | `Bearer eyJhbGciOiJ...` |

**Response Status**

| Status | Description     |
| ------ | --------------- |
| 200    | Success         |
| 4xx    | Parameter Error |
| 5xx    | Internal Error  |

**Response Headers**

| Name         | Value                           |
| ------------ | ------------------------------- |
| Content-Type | application/json; charset=utf-8 |

**Response Data**

Success:

```json
[
  {
    "content": "Hello",
    "name": "qHg01CDjm9VTZVC-bgPR51683150191441",
    "role": "user"
  },
  {
    "content": "Hello the world",
    "name": "qHg01CDjm9VTZVC-bgPR51683150191441",
    "role": "assistant"
  }
]
```

Failure:

```json
{
  "code": 500,
  "message": "Error: Invalid token"
}
```

### Commands

`POST /api/__yao/neo`

**Query String Parameters**

| Name  | Required | Description | Example                 |
| ----- | -------- | ----------- | ----------------------- |
| token | true     | JWT token   | `Bearer eyJhbGciOiJ...` |

**Request Body**

```json
{
  "cmd": "ExitCommandMode"
}
```

| Command         | Description           |
| --------------- | --------------------- |
| ExitCommandMode | Exit the Command Mode |

**Request Headers**

| Name         | Value                           |
| ------------ | ------------------------------- |
| Content-Type | application/json; charset=utf-8 |

**Response Status**

| Status | Description     |
| ------ | --------------- |
| 200    | Success         |
| 4xx    | Parameter Error |
| 5xx    | Internal Error  |

**Response Headers**

| Name         | Value                           |
| ------------ | ------------------------------- |
| Content-Type | application/json; charset=utf-8 |

**Response Data**

Success:

```json
{
  "code": 200,
  "message": "success"
}
```

Failure:

```json
{
  "code": 500,
  "message": "Error: Invalid token"
}
```

## Command mode Test Prompts

| Prompt             | Description                  |
| ------------------ | ---------------------------- |
| Generate test data | Enter the table.data Command |
