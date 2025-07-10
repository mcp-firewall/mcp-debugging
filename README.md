# MCP Debugging Guide

This repository contains documentation and examples for debugging Model Context Protocol (MCP) implementations across different transport mechanisms and server types.

## Who Is This For?
This guide is intended for developers working with the Model Context Protocol (MCP) who need to debug their implementations or understand various MCP server behaviors across different transport layers. A basic understanding of MCP concepts and familiarity with command-line tools (like curl) will be beneficial.

## Table of Contents

- [Overview](#overview)
- [Debugging Methods](#debugging-methods)
  - [Standard I/O (stdio)](#standard-io-stdio)
  - [Server-Sent Events (SSE)](#server-sent-events-sse)
  - [SSE Proxy Config](#sse-proxy-config)
  - [HTTP Streaming](#http-streaming)
  - [MCP Inspector](#mcp-inspector)
  - [Wireshark and Stratoshark](#wireshark-and-stratoshark)
- [Example Captures](#example-captures)

## Overview

The Model Context Protocol (MCP) is a communication protocol that enables interaction between AI models and external tools or resources. This repository provides documentation and examples for debugging MCP implementations across different transport mechanisms.

---
*Note: This documentation may contain links to external projects and resources. While these are provided for comprehensive understanding, their availability and content are maintained by their respective owners. Please verify any external links if you encounter issues.*

## Debugging Methods

### Standard I/O (stdio)

MCP can also be implemented over standard input/output (stdio) for direct communication with local processes. The documentation covers several stdio-based MCP servers.

For detailed examples, see [debug_stdio.md](debug_stdio.md).
### Server-Sent Events (SSE)

MCP can be implemented using Server-Sent Events (SSE) for real-time communication. This approach establishes a persistent connection where the server can push updates to the client. The documentation covers setting up SSE channels, initializing sessions, and performing operations like tool listing, tool calling, and resource management.

For detailed examples, see [debug_sse.md](debug_sse.md).

### SSE Proxy Config
Details for debugging with SSE when a proxy is involved.
For detailed examples, see [debug_sse_proxy.md](debug_sse_proxy.md).

### HTTP Streaming

MCP can be implemented over HTTP streaming, with or without session management. The documentation includes examples for initializing connections, managing sessions, and performing common operations like listing tools and calling tools.

For detailed examples, see [debug_http_streaming.md](debug_http_streaming.md).

### MCP Inspector

The MCP Inspector is a tool for debugging MCP servers. It provides a convenient interface for interacting with MCP servers and inspecting their behavior.

For usage information, see [debug_inspector.md](debug_inspector.md).

### Wireshark and Stratoshark

For network-level debugging, you can use Wireshark and Stratoshark to capture and analyze MCP traffic. Example captures are available in the [captures](./captures) directory.

For more information, see [debug_wireshark.md](debug_wireshark.md).

## Example Captures

The [captures](./captures) directory contains various files to aid in understanding and debugging MCP traffic:
- `mcp-sse.pcap`: A packet capture of MCP over Server-Sent Events, viewable with Wireshark or Stratoshark.
- `mcp-stdio.png`: An image illustrating an MCP interaction over stdio (e.g., a screenshot).
- `mcp-stdio.scap`: A capture file for stdio, likely for use with a specific tool like `syscap` or a similar stdio capture utility.

Refer to [debug_wireshark.md](debug_wireshark.md) for more on using network capture tools.

