# Debug MCP HTTP Streaming
Based on https://github.com/ferrants/mcp-streamable-http-typescript-server <br>
Try FastMCP agent https://github.com/kubiosec-ai/mcp-fast-agent

```
export PORT=3000
```

## MCP without session management
### Initialize
```
curl -kvL -s -X POST http://localhost:$PORT/mcp \
  -H "Accept: application/json, text/event-stream" \
  -H "Accept-Encoding: gzip, deflate" \
  -H "Connection: keep-alive" \
  -H "User-Agent: python-httpx/0.28.1" \
  -H "Content-Type: application/json" \
  -d '{"method":"initialize","params":{"protocolVersion":"2025-03-26","capabilities":{"roots":{"listChanged":true}},"clientInfo":{"name":"mcp","version":"1.0.0"}},"jsonrpc":"2.0","id":0}'
```

<details>
<summary>Initialize Response</summary>
  
```
* Host localhost:3000 was resolved.
* IPv6: ::1
* IPv4: 127.0.0.1
*   Trying [::1]:3000...
* Connected to localhost (::1) port 3000
> POST /mcp HTTP/1.1
> Host: localhost:3000
> Accept: application/json, text/event-stream
> Accept-Encoding: gzip, deflate
> Connection: keep-alive
> User-Agent: python-httpx/0.28.1
> Content-Type: application/json
> Content-Length: 180
>
* upload completely sent off: 180 bytes
< HTTP/1.1 200 OK
< X-Powered-By: Express
< Content-Type: text/event-stream
< Cache-Control: no-cache
< Connection: keep-alive
< Date: Tue, 22 Apr 2025 09:36:31 GMT
< Transfer-Encoding: chunked
<
event: message
data: {"result":{"protocolVersion":"2024-11-05","capabilities":{"logging":{},"tools":{"listChanged":true}},"serverInfo":{"name":"streamable-mcp-server","version":"1.0.0"}},"jsonrpc":"2.0","id":0}

* Connection #0 to host localhost left intact
```

</details>

## MCP with `mcp-session-id`
### Initialize
```
curl -kvL -s -X POST http://localhost:$PORT/mcp \
  -H "Accept: application/json, text/event-stream" \
  -H "Accept-Encoding: gzip, deflate" \
  -H "Connection: keep-alive" \
  -H "User-Agent: python-httpx/0.28.1" \
  -H "Content-Type: application/json" \
  -d '{"method":"initialize","params":{"protocolVersion":"2025-03-26","capabilities":{"roots":{"listChanged":true}},"clientInfo":{"name":"mcp","version":"1.0.0"}},"jsonrpc":"2.0","id":0}'
```

<details>
<summary>Initialize Response</summary>

```
* Host localhost:3000 was resolved.
* IPv6: ::1
* IPv4: 127.0.0.1
*   Trying [::1]:3000...
* Connected to localhost (::1) port 3000
> POST /mcp HTTP/1.1
> Host: localhost:3000
> Accept: application/json, text/event-stream
> Accept-Encoding: gzip, deflate
> Connection: keep-alive
> User-Agent: python-httpx/0.28.1
> Content-Type: application/json
> Content-Length: 180
>
* upload completely sent off: 180 bytes
< HTTP/1.1 200 OK
< X-Powered-By: Express
< Content-Type: application/json
< mcp-session-id: 7be7a4f2-1c8e-4cb3-92dd-eb438fcd0e61
< Date: Tue, 22 Apr 2025 09:39:26 GMT
< Connection: keep-alive
< Keep-Alive: timeout=5
< Transfer-Encoding: chunked
<
* Connection #0 to host localhost left intact
{"result":{"protocolVersion":"2024-11-05","capabilities":{"logging":{},"tools":{"listChanged":true}},"serverInfo":{"name":"streamable-mcp-server","version":"1.0.0"}},"jsonrpc":"2.0","id":0}%
```

</details>

```
export SESSIONID="7be7a4f2-1c8e-4cb3-92dd-eb438fcd0e61"
```
*(Note: The session ID above is an example. In practice, you should use the method described below to capture the actual `mcp-session-id` returned by the server.)*
Or capture mcp-session-id using `bash` magic
```
TMP_HEADER=$(curl -kvL -s -X POST http://localhost:$PORT/mcp \
  -H "Accept: application/json, text/event-stream" \
  -H "Accept-Encoding: gzip, deflate" \
  -H "Connection: keep-alive" \
  -H "User-Agent: python-httpx/0.28.1" \
  -H "Content-Type: application/json" \
  -D - \
  -d '{"method":"initialize","params":{"protocolVersion":"2025-03-26","capabilities":{"roots":{"listChanged":true}},"clientInfo":{"name":"mcp","version":"1.0.0"}},"jsonrpc":"2.0","id":0}' | grep -i '^HTTP\|^[a-z-]*:')
```
```
SESSIONID=$(echo "$TMP_HEADER" | grep -i '^mcp-session-id:' | awk '{print $2}' | tr -d '\r')
```
```
echo $SESSIONID
```
```
curl -kvL -s -X POST http://localhost:$PORT/mcp \
  -H "Accept: application/json, text/event-stream" \
  -H "Accept-Encoding: gzip, deflate" \
  -H "mcp-session-id: $SESSIONID" \
  -H "Connection: keep-alive" \
  -H "User-Agent: python-httpx/0.28.1" \
  -H "Content-Type: application/json" \
  -d '{"method":"notifications/initialized","jsonrpc":"2.0"}'
```

<details>
<summary>Initialize Confirmation</summary>
  
```
* Host localhost:3000 was resolved.
* IPv6: ::1
* IPv4: 127.0.0.1
*   Trying [::1]:3000...
* Connected to localhost (::1) port 3000
> POST /mcp HTTP/1.1
> Host: localhost:3000
> Accept: application/json, text/event-stream
> Accept-Encoding: gzip, deflate
> mcp-session-id: 7be7a4f2-1c8e-4cb3-92dd-eb438fcd0e61
> Connection: keep-alive
> User-Agent: python-httpx/0.28.1
> Content-Type: application/json
> Content-Length: 54
>
* upload completely sent off: 54 bytes
< HTTP/1.1 202 Accepted
< X-Powered-By: Express
< Date: Tue, 22 Apr 2025 09:43:32 GMT
< Connection: keep-alive
< Keep-Alive: timeout=5
< Transfer-Encoding: chunked
<
* Connection #0 to host localhost left intact
```

</details>

### tools/list

```
curl -kvL -s -X POST http://localhost:$PORT/mcp \
  -H "Accept: application/json, text/event-stream" \
  -H "Accept-Encoding: gzip, deflate" \
  -H "mcp-session-id: $SESSIONID" \
  -H "Connection: keep-alive" \
  -H "User-Agent: python-httpx/0.28.1" \
  -H "Content-Type: application/json" \
  -d '{"method":"tools/list","jsonrpc":"2.0","id":1}'
```

<details>
<summary>Tool/List Response</summary>

```
* Host localhost:3000 was resolved.
* IPv6: ::1
* IPv4: 127.0.0.1
*   Trying [::1]:3000...
* Connected to localhost (::1) port 3000
> POST /mcp HTTP/1.1
> Host: localhost:3000
> Accept: application/json, text/event-stream
> Accept-Encoding: gzip, deflate
> mcp-session-id: 7be7a4f2-1c8e-4cb3-92dd-eb438fcd0e61
> Connection: keep-alive
> User-Agent: python-httpx/0.28.1
> Content-Type: application/json
> Content-Length: 46
>
* upload completely sent off: 46 bytes
< HTTP/1.1 200 OK
< X-Powered-By: Express
< Content-Type: application/json
< mcp-session-id: 7be7a4f2-1c8e-4cb3-92dd-eb438fcd0e61
< Date: Tue, 22 Apr 2025 09:44:35 GMT
< Connection: keep-alive
< Keep-Alive: timeout=5
< Transfer-Encoding: chunked
<
* Connection #0 to host localhost left intact
{
  "result": {
    "tools": [
      {
        "name": "greet",
        "description": "A simple greeting tool",
        "inputSchema": {
          "type": "object",
          "properties": {
            "name": {
              "type": "string",
              "description": "Name to greet"
            }
          },
          "required": [
            "name"
          ],
          "additionalProperties": false,
          "$schema": "http://json-schema.org/draft-07/schema#"
        }
      },
      {
        "name": "multi-greet",
        "description": "A tool that sends different greetings with delays between them",
        "inputSchema": {
          "type": "object",
          "properties": {
            "name": {
              "type": "string",
              "description": "Name to greet"
            }
          },
          "required": [
            "name"
          ],
          "additionalProperties": false,
          "$schema": "http://json-schema.org/draft-07/schema#"
        }
      }
    ]
  },
  "jsonrpc": "2.0",
  "id": 1
}
```
</details>

### tools/call
```
curl -kvL -s -X POST http://localhost:$PORT/mcp \
  -H "Accept: application/json, text/event-stream" \
  -H "Accept-Encoding: gzip, deflate" \
  -H "mcp-session-id: $SESSIONID" \
  -H "Connection: keep-alive" \
  -H "User-Agent: python-httpx/0.28.1" \
  -H "Content-Type: application/json" \
  -d '{"method":"tools/call","params":{"name":"greet","arguments":{"name":"xxradar"}},"jsonrpc":"2.0","id":4}'
```

<details>
<summary>Tool/Call Response</summary>

```
* Host localhost:3000 was resolved.
* IPv6: ::1
* IPv4: 127.0.0.1
*   Trying [::1]:3000...
* Connected to localhost (::1) port 3000
> POST /mcp HTTP/1.1
> Host: localhost:3000
> Accept: application/json, text/event-stream
> Accept-Encoding: gzip, deflate
> mcp-session-id: 7be7a4f2-1c8e-4cb3-92dd-eb438fcd0e61
> Connection: keep-alive
> User-Agent: python-httpx/0.28.1
> Content-Type: application/json
> Content-Length: 103
>
* upload completely sent off: 103 bytes
< HTTP/1.1 200 OK
< X-Powered-By: Express
< Content-Type: application/json
< mcp-session-id: 7be7a4f2-1c8e-4cb3-92dd-eb438fcd0e61
< Date: Tue, 22 Apr 2025 09:47:34 GMT
< Connection: keep-alive
< Keep-Alive: timeout=5
< Transfer-Encoding: chunked
<
* Connection #0 to host localhost left intact
{"result":{"content":[{"type":"text","text":"Hello, xxradar!"}]},"jsonrpc":"2.0","id":4}%
```

</details>

---
Previous: [SSE Proxy Config](./debug_sse_proxy.md) | Next: [MCP Inspector](./debug_inspector.md) | [Back to README](./README.md)
