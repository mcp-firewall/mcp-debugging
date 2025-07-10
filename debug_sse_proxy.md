# Pre-Configured for Proxy Mode and Firewalling

## Start Messaging
### Initialize
In a different terminal
```
export SESSION="sessionId=dc99d6f7-1eb3-47b5-99a9-30ef2547781e"
```
```
curl -XPOST "http://127.0.0.1:3002/messages?$SESSION" \
  -H "Accept: */*" \
  -H "Accept-Encoding: gzip, deflate" \
  -H "Connection: keep-alive" \
  -H "User-Agent: python-httpx/0.28.1" \
  -H "Content-Type: application/json" \
  -d '{"method":"notifications/initialized","jsonrpc":"2.0"}'
```
```
curl -XPOST "http://127.0.0.1:3002/messages?$SESSION" \
  -H "Accept: */*" \
  -H "Accept-Encoding: gzip, deflate" \
  -H "Connection: keep-alive" \
  -H "User-Agent: python-httpx/0.28.1" \
  -H "Content-Type: application/json" \
  -d '{"method":"notifications/initialized","jsonrpc":"2.0"}'

```
### Tool/List
```
curl -XPOST "http://127.0.0.1:3002/messages?$SESSION" \
  -H "Accept: */*" \
  -H "Accept-Encoding: gzip, deflate" \
  -H "Connection: keep-alive" \
  -H "User-Agent: python-httpx/0.28.1" \
  -H "Content-Type: application/json" \
  -d '{"method":"tools/list","jsonrpc":"2.0","id":1}'
```
### Tool/Call
```
curl -X POST "http://127.0.0.1:3002/messages?$SESSION" \
  -H "Accept: */*" \
  -H "Accept-Encoding: gzip, deflate" \
  -H "Connection: keep-alive" \
  -H "User-Agent: python-httpx/0.28.1" \
  -H "Content-Type: application/json" \
  -d '{"method":"tools/call","params":{"name":"echo","arguments":{"message":"Hello from curl!"}},"jsonrpc":"2.0","id":2}'
```
```
curl -X POST "http://127.0.0.1:3002/messages?$SESSION" \
  -H "Accept: */*" \
  -H "Accept-Encoding: gzip, deflate" \
  -H "Connection: keep-alive" \
  -H "User-Agent: python-httpx/0.28.1" \
  -H "Content-Type: application/json" \
  -d '{"method":"tools/call","params":{"name":"echo","arguments":{"message":"Hello from curl and password!"}},"jsonrpc":"2.0","id":3}'
```
### Resources/List
```
curl -XPOST "http://127.0.0.1:3002/messages?$SESSION" \
  -H "Accept: */*" \
  -H "Accept-Encoding: gzip, deflate" \
  -H "Connection: keep-alive" \
  -H "User-Agent: python-httpx/0.28.1" \
  -H "Content-Type: application/json" \
  -d '{"method":"resources/list","jsonrpc":"2.0","id":4}'
```
### Resources/Read
```
curl -XPOST "http://127.0.0.1:3002/messages?$SESSION" \
  -H "Accept: */*" \
  -H "Accept-Encoding: gzip, deflate" \
  -H "Connection: keep-alive" \
  -H "User-Agent: python-httpx/0.28.1" \
  -H "Content-Type: application/json" \
  -d '{"method":"resources/read","params":{"uri":"config://app"},"jsonrpc":"2.0","id":5}'
```

---
Previous: [Server-Sent Events (SSE)](./debug_sse.md) | Next: [HTTP Streaming](./debug_http_streaming.md) | [Back to README](./README.md)
