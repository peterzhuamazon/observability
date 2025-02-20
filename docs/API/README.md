# API Mock Tests
The [Swagger](swagger.yaml) describes the API used by the Integration repository to support the `registry` and `store` functionalities.

Its possible to visualize the REST API using any on the numerous [online editors](https://editor.swagger.io/)
![](API.png)

### Setup mock server
In order to test the API, we can utilize a swagger based mock library which allows ['contract based testing'](https://github.com/stoplightio/prism).

#### running the swagger mock server
````
npm install -g @stoplight/prism-cli
````
Once this tool is installed, the server can be run using the next command 
```
prism mock swagger.yaml
```
The next endpoints are presented:
```
[10:32:12 a.m.] › [CLI] ℹ  info      GET        http://127.0.0.1:4010/repository?limit=%5Bobject%20Object%5D
[10:32:12 a.m.] › [CLI] ℹ  info      POST       http://127.0.0.1:4010/repository
[10:32:12 a.m.] › [CLI] ℹ  info      GET        http://127.0.0.1:4010/repository/%5Bobject%20Object%5D
[10:32:12 a.m.] › [CLI] ℹ  info      GET        http://127.0.0.1:4010/store?limit=25
[10:32:12 a.m.] › [CLI] ℹ  info      POST       http://127.0.0.1:4010/store
[10:32:12 a.m.] › [CLI] ℹ  info      GET        http://127.0.0.1:4010/store/cum
[10:32:12 a.m.] › [CLI] ℹ  info      POST       http://127.0.0.1:4010/store/nihil/validate
[10:32:12 a.m.] › [CLI] ℹ  info      POST       http://127.0.0.1:4010/store/laudantium/upload
[10:32:12 a.m.] › [CLI] ℹ  info      PUT        http://127.0.0.1:4010/store/nihil/activate
[10:32:12 a.m.] › [CLI] ▶  start     Prism is listening on http://127.0.0.1:4010
```

Once the server has started, a CURL request can be initiated with any of the above endpoints:

`GET http://localhost:4010/repository`

Would show in the traffic logs:

`[10:32:23 a.m.] › [HTTP SERVER] get /repository ℹ  info      Request received`

And will result with :

```json5
    {
  "catalog": "observability",
  "version": "1.0",
  "url": "https://github.com/opensearch-project/opensearch-catalog/tree/main/schema/observability",
  "categories": [
    {
      "category": "logs",
      "version": "1.0",
      "url": "https://github.com/opensearch-project/opensearch-catalog/tree/main/schema/observability/logs",
      "components": [
        {
          "component": "log",
          "version": "1.0",
          "url": "https://github.com/opensearch-project/opensearch-catalog/tree/main/schema/observability/logs/logs",
          "container": true,
          "tags": []
        },
        {
          "component": "http",
          "version": "1.0",
          "url": "https://github.com/opensearch-project/opensearch-catalog/tree/main/schema/observability/logs/http",
          "tags": [],
          "container": false
        },
        {
          "component": "communication",
          "version": "1.0",
          "url": "https://github.com/opensearch-project/opensearch-catalog/tree/main/schema/observability/logs/communication",
          "tags": ["web"],
          "container": false
        }]
    },
    {
      "category": "traces",
      "version": "1.0",
      "url": "https://github.com/opensearch-project/opensearch-catalog/tree/main/schema/observability/traces",
      "components": [
        {
          "component": "span",
          "version": "1.0",
          "url": "https://github.com/opensearch-project/opensearch-catalog/tree/main/schema/observability/traces/traces",
          "tags": [],
          "container": true
        }]
    },
    {
      "category": "metrics",
      "version": "1.0",
      "url": "https://github.com/opensearch-project/opensearch-catalog/tree/main/schema/observability/metrics",
      "components": [
        {
          "component": "metric",
          "version": "1.0",
          "url": "https://github.com/opensearch-project/opensearch-catalog/tree/main/schema/observability/metrics/metrics",
          "tags": [],
          "container": true
        }]
    }
  ]
}
```



####  running the swagger mock server as docker

One-liner docker command (assuming running from repository root):

```text
docker run -p 9200:4010 -v "$(pwd)/docs/API:/api" -v "$(pwd)/schema:/schema" --name integ-prism -t stoplight/prism mock -h 0.0.0.0 /api/swagger.yaml
```