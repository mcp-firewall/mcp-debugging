## Debugging MCP Using Inspector
```
npx @modelcontextprotocol/inspector
```
or
```
npx @modelcontextprotocol/inspector <command>
```
## Inspector Examples
```
npx @modelcontextprotocol/inspector node $PWD/dist/index.js -e PERMISSION_REQUIRED=true
```
```
npx @modelcontextprotocol/inspector deno run -N -R=node_modules -W=node_modules --node-modules-dir=auto  jsr:@pydantic/mcp-run-python stdio
```
You can also run (see https://ai.pydantic.dev/mcp/run-python/?query=default+port+mcp+python)
```
deno run -N -R=node_modules -W=node_modules --node-modules-dir=auto  jsr:@pydantic/mcp-run-python sse
```
and use Inspector to connect to
```
http://127.0.0.1:3001/sse
```
## Inspector CLI Examples
```
SLACK_BOT_TOKEN="xxxxxxxx" SLACK_TEAM_ID="xxxxxxx" npx @modelcontextprotocol/inspector --cli --method tools/list  npx -y @modelcontextprotocol/server-slack >tools.json
```
```
npx @modelcontextprotocol/inspector --cli --method tools/list docker run  --rm -i alpine/socat STDIO TCP:host.docker.internal:8811
```
For streamable-http (TBC)
```
npx @modelcontextprotocol/inspector --cli http://127.0.0.1:8001 --method tools/list
```
For SSE:
```
npx @modelcontextprotocol/inspector --cli http://127.0.0.1:8000 --method tools/list
```

---
Previous: [HTTP Streaming](./debug_http_streaming.md) | Next: [Wireshark and Stratoshark](./debug_wireshark.md) | [Back to README](./README.md)
