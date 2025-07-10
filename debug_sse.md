# Debug MCP SSE
Based on https://github.com/kubiosec-ai/mcp-sse-typescript.<br>
You can also use the `client2.js` from this repo [https://github.com/kubiosec-codecamp/](https://github.com/kubiosec-codecamp/learning-sse)
## Start SSE Channel
```
curl -N "http://127.0.0.1:3001/sse" \
  -H "Accept-Encoding: gzip, deflate" \
  -H "Connection: keep-alive" \
  -H "User-Agent: python-httpx/0.28.1" \
  -H "Accept: text/event-stream" \
  -H "Cache-Control: no-store"
```
Keep this channel open, it will receive the responses

<details>
<summary>Event Stream Output</summary>
  
```
event: endpoint
data: /messages?sessionId=d3c8c6d8-6eb9-43b0-9104-00c4fa274466

event: message
data: {"result":{"protocolVersion":"2024-11-05","capabilities":{"resources":{},"tools":{},"prompts":{}},"serverInfo":{"name":"example-server","version":"1.0.0"}},"jsonrpc":"2.0","id":0}

event: message
data: {"result":{"tools":[{"name":"echo","inputSchema":{"type":"object","properties":{"message":{"type":"string"}},"required":["message"],"additionalProperties":false,"$schema":"http://json-schema.org/draft-07/schema#"}}]},"jsonrpc":"2.0","id":1}

event: message
data: {"result":{"resources":[{"uri":"config://app","name":"agent1"}]},"jsonrpc":"2.0","id":2}

event: message
data: {"result":{"contents":[{"uri":"config://app","text":"{\"name\":\"Reimbursement Agent\",\"description\":\"This agent handles the reimbursement process for the employees given the amount and purpose of the reimbursement.\",\"url\":\"http://localhost:10002/\",\"version\":\"1.0.0\",\"capabilities\":{\"streaming\":true,\"pushNotifications\":false,\"stateTransitionHistory\":false},\"defaultInputModes\":[\"text\",\"text/plain\"],\"defaultOutputModes\":[\"text\",\"text/plain\"],\"skills\":[{\"id\":\"process_reimbursement\",\"name\":\"Process Reimbursement Tool\",\"description\":\"Helps with the reimbursement process for users given the amount and purpose of the reimbursement.\",\"tags\":[\"reimbursement\"],\"examples\":[\"Can you reimburse me $20 for my lunch with the clients?\"]}]}","mimeType":"application/json"}]},"jsonrpc":"2.0","id":3}
...
```
</details>

## Start Messaging
### Initialize
In a different terminal
```
export SESSION="sessionId=dc99d6f7-1eb3-47b5-99a9-30ef2547781e"
```
```
curl -XPOST "http://127.0.0.1:3001/messages?$SESSION" \
  -H "Accept: */*" \
  -H "Accept-Encoding: gzip, deflate" \
  -H "Connection: keep-alive" \
  -H "User-Agent: python-httpx/0.28.1" \
  -H "Content-Type: application/json" \
  -d '{"method":"initialize","params":{"protocolVersion":"2024-11-05","capabilities":{"roots":{"listChanged":true}},"clientInfo":{"name":"mcp","version":"0.1.0"}},"jsonrpc":"2.0","id":0}'
```
```
curl -XPOST "http://127.0.0.1:3001/messages?$SESSION" \
  -H "Accept: */*" \
  -H "Accept-Encoding: gzip, deflate" \
  -H "Connection: keep-alive" \
  -H "User-Agent: python-httpx/0.28.1" \
  -H "Content-Type: application/json" \
  -d '{"method":"notifications/initialized","jsonrpc":"2.0"}'

```
### Tool/List
```
curl -XPOST "http://127.0.0.1:3001/messages?$SESSION" \
  -H "Accept: */*" \
  -H "Accept-Encoding: gzip, deflate" \
  -H "Connection: keep-alive" \
  -H "User-Agent: python-httpx/0.28.1" \
  -H "Content-Type: application/json" \
  -d '{"method":"tools/list","jsonrpc":"2.0","id":1}'
```
### Tool/Call
```
curl -X POST "http://127.0.0.1:3001/messages?$SESSION" \
  -H "Accept: */*" \
  -H "Accept-Encoding: gzip, deflate" \
  -H "Connection: keep-alive" \
  -H "User-Agent: python-httpx/0.28.1" \
  -H "Content-Type: application/json" \
  -d '{"method":"tools/call","params":{"name":"echo","arguments":{"message":"Hello from curl!"}},"jsonrpc":"2.0","id":4}'
```
### Resources/List
```
curl -XPOST "http://127.0.0.1:3001/messages?$SESSION" \
  -H "Accept: */*" \
  -H "Accept-Encoding: gzip, deflate" \
  -H "Connection: keep-alive" \
  -H "User-Agent: python-httpx/0.28.1" \
  -H "Content-Type: application/json" \
  -d '{"method":"resources/list","jsonrpc":"2.0","id":2}'
```
### Resources/Read
```
curl -XPOST "http://127.0.0.1:3001/messages?$SESSION" \
  -H "Accept: */*" \
  -H "Accept-Encoding: gzip, deflate" \
  -H "Connection: keep-alive" \
  -H "User-Agent: python-httpx/0.28.1" \
  -H "Content-Type: application/json" \
  -d '{"method":"resources/read","params":{"uri":"config://app"},"jsonrpc":"2.0","id":3}'
```

---
Previous: [Standard I/O (stdio)](./debug_stdio.md) | Next: [SSE Proxy Config](./debug_sse_proxy.md) | [Back to README](./README.md)
