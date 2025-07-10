# MCP Tool Debugging

## @mcp-get-community/server-curl

### Initialize

<details>
<summary>Initialize Response</summary>

```
echo '{"jsonrpc":"2.0","id":1,"method":"initialize","params":{"protocolVersion":"2025-03-26","clientInfo":{"name":"bash-tester","version":"0.1.0"},"capabilities":{}}}' | npx @mcp-get-community/server-curl
```
```
{"result":{"protocolVersion":"2024-11-05","capabilities":{"tools":{"curl":{"description":"Make an HTTP request to any URL with customizable method, headers, and body.","inputSchema":{"type":"object","properties":{"url":{"type":"string","format":"uri","description":"The URL to make the request to"},"method":{"type":"string","enum":["GET","POST","PUT","DELETE","PATCH","HEAD","OPTIONS"],"default":"GET","description":"HTTP method to use"},"headers":{"type":"object","additionalProperties":{"type":"string"},"description":"HTTP headers to include in the request"},"body":{"type":"string","description":"Request body (for POST, PUT, PATCH requests)"},"timeout":{"type":"number","minimum":0,"maximum":300000,"default":30000,"description":"Request timeout in milliseconds (max 300000ms/5min)"}},"required":["url"],"additionalProperties":false,"$schema":"http://json-schema.org/draft-07/schema#"}}}},"serverInfo":{"name":"@mcp-get-community/server-curl","version":"0.1.0","author":"MCP Community"}},"jsonrpc":"2.0","id":1}```
```
</details>

### Tools/List

<details>
<summary>Tools/List Response</summary>

```
echo '{"jsonrpc":"2.0","id":2,"method":"tools/list"}' | npx @mcp-get-community/server-curl | jq -r 
```
```
{
  "result": {
    "tools": [
      {
        "name": "curl",
        "description": "Make an HTTP request to any URL with customizable method, headers, and body.",
        "inputSchema": {
          "type": "object",
          "properties": {
            "url": {
              "type": "string",
              "format": "uri",
              "description": "The URL to make the request to"
            },
            "method": {
              "type": "string",
              "enum": [
                "GET",
                "POST",
                "PUT",
                "DELETE",
                "PATCH",
                "HEAD",
                "OPTIONS"
              ],
              "default": "GET",
              "description": "HTTP method to use"
            },
            "headers": {
              "type": "object",
              "additionalProperties": {
                "type": "string"
              },
              "description": "HTTP headers to include in the request"
            },
            "body": {
              "type": "string",
              "description": "Request body (for POST, PUT, PATCH requests)"
            },
            "timeout": {
              "type": "number",
              "minimum": 0,
              "maximum": 300000,
              "default": 30000,
              "description": "Request timeout in milliseconds (max 300000ms/5min)"
            }
          },
          "required": [
            "url"
          ],
          "additionalProperties": false,
          "$schema": "http://json-schema.org/draft-07/schema#"
        }
      }
    ]
  },
  "jsonrpc": "2.0",
  "id": 2
}
```
</details>

### Tools/Call

<details>
<summary>Tools/Call Response</summary>

```
echo '{"jsonrpc":"2.0","id":2, "method": "tools/call","params": {"name": "curl","arguments": {"url": "http://www.radarhack.com","method": "GET"},"_meta": {"progressToken": 2}}}' | npx @mcp-get-community/server-curl | jq -r
```
```
echo '{"jsonrpc":"2.0","id":2, "method": "tools/call","params": {"name": "curl","arguments": {"url": "http://www.radarhack.com","method": "GET","headers": {"test": "test"}},"_meta": {"progressToken": 2}}}' | npx @mcp-get-community/server-curl | jq -r
```
</details>



## Terminal (locally)
See https://mcp.so/server/terminal/stat-guy

First, you'll need to clone the `terminal` repository and build it:
```bash
git clone https://github.com/stat-guy/terminal.git
cd terminal
npm install
npm run build
# Now you can run the server (replace <path_to_terminal_repo> with the actual path to the cloned 'terminal' directory)
```
```
PERMISSION_REQUIRED=true node <path_to_terminal_repo>/dist/index.js
```
### Initialize

<details>
<summary>Initialize Response</summary>

```
echo '{"jsonrpc":"2.0","id":1,"method":"initialize","params":{"protocolVersion":"2025-03-26","clientInfo":{"name":"bash-tester","version":"0.1.0"},"capabilities":{}}}' | node /Users/xxradar/Dropbox/dev/dev_llm_ai/mcp/terminal/dist/index.js
```
```
{"result":{"protocolVersion":"2024-11-05","capabilities":{"tools":{}},"serverInfo":{"name":"terminal-server","version":"0.1.0"}},"jsonrpc":"2.0","id":1}
```
</details>

### Tools/List

<details>
<summary>Tools/List Response</summary>

```
echo '{"jsonrpc":"2.0","id":2,"method":"tools/list"}' | node /Users/xxradar/Dropbox/dev/dev_llm_ai/mcp/terminal/dist/index.js | jq -r 
```
```
{
  "result": {
    "tools": [
      {
        "name": "execute_command",
        "description": "Execute a terminal command with arguments and options.",
        "inputSchema": {
          "type": "object",
          "properties": {
            "command": {
              "type": "string",
              "description": "The command to execute"
            },
            "args": {
              "type": "array",
              "items": {
                "type": "string"
              },
              "default": [],
              "description": "Command arguments"
            },
            "options": {
              "type": "object",
              "properties": {
                "cwd": {
                  "type": "string",
                  "description": "Working directory"
                },
                "timeout": {
                  "type": "number",
                  "description": "Command timeout in milliseconds"
                },
                "env": {
                  "type": "object",
                  "additionalProperties": {
                    "type": "string"
                  },
                  "description": "Additional environment variables"
                }
              },
              "additionalProperties": false,
              "default": {}
            }
          },
          "required": [
            "command"
          ],
          "additionalProperties": false,
          "$schema": "http://json-schema.org/draft-07/schema#"
        }
      },
      {
        "name": "change_directory",
        "description": "Change the current working directory for subsequent commands.",
        "inputSchema": {
          "type": "object",
          "properties": {
            "path": {
              "type": "string",
              "description": "Directory path to change to"
            }
          },
          "required": [
            "path"
          ],
          "additionalProperties": false,
          "$schema": "http://json-schema.org/draft-07/schema#"
        }
      },
      {
        "name": "get_current_directory",
        "description": "Get the current working directory path.",
        "inputSchema": {
          "type": "object",
          "properties": {},
          "additionalProperties": false,
          "$schema": "http://json-schema.org/draft-07/schema#"
        }
      },
      {
        "name": "get_terminal_info",
        "description": "Get information about the terminal environment.",
        "inputSchema": {
          "type": "object",
          "properties": {},
          "additionalProperties": false,
          "$schema": "http://json-schema.org/draft-07/schema#"
        }
      },
      {
        "name": "lacework_alerts_list",
        "description": "List Lacework alerts filtered by severity level in JSON format.",
        "inputSchema": {
          "type": "object",
          "properties": {
            "severity": {
              "type": "string",
              "description": "Severity level for filtering alerts"
            }
          },
          "required": [
            "severity"
          ],
          "additionalProperties": false,
          "$schema": "http://json-schema.org/draft-07/schema#"
        }
      },
      {
        "name": "lacework_alerts_show",
        "description": "Show details for a specific Lacework alert in JSON format.",
        "inputSchema": {
          "type": "object",
          "properties": {
            "alertid": {
              "type": "string",
              "description": "Alert ID to show details for"
            }
          },
          "required": [
            "alertid"
          ],
          "additionalProperties": false,
          "$schema": "http://json-schema.org/draft-07/schema#"
        }
      }
    ]
  },
  "jsonrpc": "2.0",
  "id": 2
}
```
</details>

### Tools/Call

<details>
<summary>Tools/Call Response</summary>

```
echo '{"jsonrpc":"2.0","id":3,"method":"tools/call","params":{"name":"execute_command","arguments":{"command":"echo","args":["echo Hello MCP from xxradar", ">textxxradar.txt"]}}}' | node /Users/xxradar/Dropbox/dev/dev_llm_ai/mcp/terminal/dist/index.js
```
```
{"result":{"content":[{"type":"text","text":"Exit Code: 0"}]},"jsonrpc":"2.0","id":3}
```
</details>

## @playwright/mcp@latest 

### Initialize

<details>
<summary>Initialize Response</summary>

```
echo '{"jsonrpc":"2.0","id":1,"method":"initialize","params":{"protocolVersion":"2025-03-26","clientInfo":{"name":"bash-tester","version":"0.1.0"},"capabilities":{}}}' | npx @playwright/mcp@latest
```
```
{"result":{"protocolVersion":"2024-11-05","capabilities":{"tools":{},"resources":{}},"serverInfo":{"name":"Playwright","version":"0.0.10"}},"jsonrpc":"2.0","id":1}```
```
</details>

### Tools/List

<details>
<summary>Tools/List Response</summary>

```
echo '{"jsonrpc":"2.0","id":2,"method":"tools/list"}' | npx @playwright/mcp@latest | jq -r 
```
```
{
  "result": {
    "tools": [
      {
        "name": "browser_close",
        "description": "Close the page",
        "inputSchema": {
          "type": "object",
          "properties": {},
          "additionalProperties": false,
          "$schema": "http://json-schema.org/draft-07/schema#"
        }
      },
      {
        "name": "browser_wait",
        "description": "Wait for a specified time in seconds",
        "inputSchema": {
          "type": "object",
          "properties": {
            "time": {
              "type": "number",
              "description": "The time to wait in seconds"
            }
          },
          "required": [
            "time"
          ],
          "additionalProperties": false,
          "$schema": "http://json-schema.org/draft-07/schema#"
        }
      },
      {
        "name": "browser_file_upload",
        "description": "Upload one or multiple files",
        "inputSchema": {
          "type": "object",
          "properties": {
            "paths": {
              "type": "array",
              "items": {
                "type": "string"
              },
              "description": "The absolute paths to the files to upload. Can be a single file or multiple files."
            }
          },
          "required": [
            "paths"
          ],
          "additionalProperties": false,
          "$schema": "http://json-schema.org/draft-07/schema#"
        }
      },
      {
        "name": "browser_install",
        "description": "Install the browser specified in the config. Call this if you get an error about the browser not being installed.",
        "inputSchema": {
          "type": "object",
          "properties": {},
          "additionalProperties": false,
          "$schema": "http://json-schema.org/draft-07/schema#"
        }
      },
      {
        "name": "browser_press_key",
        "description": "Press a key on the keyboard",
        "inputSchema": {
          "type": "object",
          "properties": {
            "key": {
              "type": "string",
              "description": "Name of the key to press or a character to generate, such as `ArrowLeft` or `a`"
            }
          },
          "required": [
            "key"
          ],
          "additionalProperties": false,
          "$schema": "http://json-schema.org/draft-07/schema#"
        }
      },
      {
        "name": "browser_navigate",
        "description": "Navigate to a URL",
        "inputSchema": {
          "type": "object",
          "properties": {
            "url": {
              "type": "string",
              "description": "The URL to navigate to"
            }
          },
          "required": [
            "url"
          ],
          "additionalProperties": false,
          "$schema": "http://json-schema.org/draft-07/schema#"
        }
      },
      {
        "name": "browser_navigate_back",
        "description": "Go back to the previous page",
        "inputSchema": {
          "type": "object",
          "properties": {},
          "additionalProperties": false,
          "$schema": "http://json-schema.org/draft-07/schema#"
        }
      },
      {
        "name": "browser_navigate_forward",
        "description": "Go forward to the next page",
        "inputSchema": {
          "type": "object",
          "properties": {},
          "additionalProperties": false,
          "$schema": "http://json-schema.org/draft-07/schema#"
        }
      },
      {
        "name": "browser_pdf_save",
        "description": "Save page as PDF",
        "inputSchema": {
          "type": "object",
          "properties": {},
          "additionalProperties": false,
          "$schema": "http://json-schema.org/draft-07/schema#"
        }
      },
      {
        "name": "browser_snapshot",
        "description": "Capture accessibility snapshot of the current page, this is better than screenshot",
        "inputSchema": {
          "type": "object",
          "properties": {},
          "additionalProperties": false,
          "$schema": "http://json-schema.org/draft-07/schema#"
        }
      },
      {
        "name": "browser_click",
        "description": "Perform click on a web page",
        "inputSchema": {
          "type": "object",
          "properties": {
            "element": {
              "type": "string",
              "description": "Human-readable element description used to obtain permission to interact with the element"
            },
            "ref": {
              "type": "string",
              "description": "Exact target element reference from the page snapshot"
            }
          },
          "required": [
            "element",
            "ref"
          ],
          "additionalProperties": false,
          "$schema": "http://json-schema.org/draft-07/schema#"
        }
      },
      {
        "name": "browser_drag",
        "description": "Perform drag and drop between two elements",
        "inputSchema": {
          "type": "object",
          "properties": {
            "startElement": {
              "type": "string",
              "description": "Human-readable source element description used to obtain the permission to interact with the element"
            },
            "startRef": {
              "type": "string",
              "description": "Exact source element reference from the page snapshot"
            },
            "endElement": {
              "type": "string",
              "description": "Human-readable target element description used to obtain the permission to interact with the element"
            },
            "endRef": {
              "type": "string",
              "description": "Exact target element reference from the page snapshot"
            }
          },
          "required": [
            "startElement",
            "startRef",
            "endElement",
            "endRef"
          ],
          "additionalProperties": false,
          "$schema": "http://json-schema.org/draft-07/schema#"
        }
      },
      {
        "name": "browser_hover",
        "description": "Hover over element on page",
        "inputSchema": {
          "type": "object",
          "properties": {
            "element": {
              "type": "string",
              "description": "Human-readable element description used to obtain permission to interact with the element"
            },
            "ref": {
              "type": "string",
              "description": "Exact target element reference from the page snapshot"
            }
          },
          "required": [
            "element",
            "ref"
          ],
          "additionalProperties": false,
          "$schema": "http://json-schema.org/draft-07/schema#"
        }
      },
      {
        "name": "browser_type",
        "description": "Type text into editable element",
        "inputSchema": {
          "type": "object",
          "properties": {
            "element": {
              "type": "string",
              "description": "Human-readable element description used to obtain permission to interact with the element"
            },
            "ref": {
              "type": "string",
              "description": "Exact target element reference from the page snapshot"
            },
            "text": {
              "type": "string",
              "description": "Text to type into the element"
            },
            "submit": {
              "type": "boolean",
              "description": "Whether to submit entered text (press Enter after)"
            },
            "slowly": {
              "type": "boolean",
              "description": "Whether to type one character at a time. Useful for triggering key handlers in the page. By default entire text is filled in at once."
            }
          },
          "required": [
            "element",
            "ref",
            "text"
          ],
          "additionalProperties": false,
          "$schema": "http://json-schema.org/draft-07/schema#"
        }
      },
      {
        "name": "browser_select_option",
        "description": "Select an option in a dropdown",
        "inputSchema": {
          "type": "object",
          "properties": {
            "element": {
              "type": "string",
              "description": "Human-readable element description used to obtain permission to interact with the element"
            },
            "ref": {
              "type": "string",
              "description": "Exact target element reference from the page snapshot"
            },
            "values": {
              "type": "array",
              "items": {
                "type": "string"
              },
              "description": "Array of values to select in the dropdown. This can be a single value or multiple values."
            }
          },
          "required": [
            "element",
            "ref",
            "values"
          ],
          "additionalProperties": false,
          "$schema": "http://json-schema.org/draft-07/schema#"
        }
      },
      {
        "name": "browser_take_screenshot",
        "description": "Take a screenshot of the current page. You can't perform actions based on the screenshot, use browser_snapshot for actions.",
        "inputSchema": {
          "type": "object",
          "properties": {
            "raw": {
              "type": "boolean",
              "description": "Whether to return without compression (in PNG format). Default is false, which returns a JPEG image."
            }
          },
          "additionalProperties": false,
          "$schema": "http://json-schema.org/draft-07/schema#"
        }
      },
      {
        "name": "browser_tab_list",
        "description": "List browser tabs",
        "inputSchema": {
          "type": "object",
          "properties": {},
          "additionalProperties": false,
          "$schema": "http://json-schema.org/draft-07/schema#"
        }
      },
      {
        "name": "browser_tab_new",
        "description": "Open a new tab",
        "inputSchema": {
          "type": "object",
          "properties": {
            "url": {
              "type": "string",
              "description": "The URL to navigate to in the new tab. If not provided, the new tab will be blank."
            }
          },
          "additionalProperties": false,
          "$schema": "http://json-schema.org/draft-07/schema#"
        }
      },
      {
        "name": "browser_tab_select",
        "description": "Select a tab by index",
        "inputSchema": {
          "type": "object",
          "properties": {
            "index": {
              "type": "number",
              "description": "The index of the tab to select"
            }
          },
          "required": [
            "index"
          ],
          "additionalProperties": false,
          "$schema": "http://json-schema.org/draft-07/schema#"
        }
      },
      {
        "name": "browser_tab_close",
        "description": "Close a tab",
        "inputSchema": {
          "type": "object",
          "properties": {
            "index": {
              "type": "number",
              "description": "The index of the tab to close. Closes current tab if not provided."
            }
          },
          "additionalProperties": false,
          "$schema": "http://json-schema.org/draft-07/schema#"
        }
      }
    ]
  },
  "jsonrpc": "2.0",
  "id": 2
}
```
</details>

### Tools/Call

<details>
<summary>Tools/Call Response</summary>

```
npx @modelcontextprotocol/inspector  npx @playwright/mcp@latest
```
Looks like npx via cli cannot open a browser ...
</details>

## Other examples
```
echo '{"jsonrpc":"2.0","id":2,"method":"tools/list"}' | docker run -i --rm mcp/desktop-commander  | jq -r . >tools_x.json
```
Servers with @smithery/cli@latest run --> remove @smithery/cli@latest run from the config
```
echo '{"jsonrpc":"2.0","id":2,"method":"tools/list"}' | SHODAN_API_KEY=XXXXXXXXXXXXXX  npx -y  @burtthecoder/mcp-shodan 
```

---
Next: [Server-Sent Events (SSE)](./debug_sse.md) | [Back to README](./README.md)
