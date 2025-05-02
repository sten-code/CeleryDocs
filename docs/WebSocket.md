# WebSocket

The **WebSocket** class provides a simple interface for sending and receiving data over a WebSocket connection.

---

## Fields

### isOpen

```lua
isOpen: boolean
```

Indicates the current state of the WebSocket connection. Is `true` if the connection is active, and `false` if it is closed.

---

## Functions

### WebSocket.connect

`🏛️ Constructor`

```lua
function WebSocket.connect(url: string): WebSocket
```

Establishes a WebSocket connection to the specified URL.

#### Parameters
* `url`: The URL to connect to.

#### Aliases
* `WebSocket.Connect`

#### Example
```lua
local ws = WebSocket.connect("ws://localhost:8080")

ws.OnMessage:Connect(function(message)
	print(message)
end)

ws.OnClose:Connect(function()
	print("Connection closed")
end)

ws:Send("Hello, World!")
```

---

### Send

`⏹️ Method`

```lua
function Send(message: string)
```

Sends a message over the WebSocket connection.

#### Parameters
* `message`: The message to send through the WebSocket connection.

#### Example
```lua
local ws = WebSocket.connect("ws://localhost:8080")
ws:Send("Hello, World!")
```

---

### Close

`⏹️ Method`

```lua
function Close()
```

Closes the WebSocket connection.

#### Example
```lua
local ws = WebSocket.connect("ws://localhost:8080")
-- Do stuff with the connection here
ws:Close()
```

---

### OnMessage:Connect

`📅 Event`

```lua
function OnMessage:Connect(callback: function(message: string)): RBXScriptConnection
```

Creates a script connection and calls the callback with the message when a message is received from the WebSocket.

#### Example
```lua
local ws = WebSocket.connect("ws://localhost:8080")

ws.OnMessage:Connect(function(message)
    print(message)
end)
```

---

### OnClose:Connect

`📅 Event`

```lua
function OnClose:Connect(callback: function())
```

Creates a script connection and calls the callback when the WebSocket connection closes.

#### Example
```lua
local ws = WebSocket.connect("ws://localhost:8080")
ws.OnClose:Connect(function()
    print("Connection closed")
end)
ws:Close()
```

---
