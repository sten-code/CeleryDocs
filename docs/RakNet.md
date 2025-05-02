# RakNet

The **rnet** library provides an interface for the RakNet networking framework within Roblox.

---

## Data Types

### Packet

| Field    | Type     | Description                                  |
| -------- | -------- | -------------------------------------------- |
| `id`     | `number` | The packet id.                               |
| `sub_id` | `number` | The packet sub id.                           |
| `size`   | `number` | The size of the data in bytes.               |
| `data`   | `array`  | The data of the packet in an array of bytes. |

---

### Object

| Field   | Type     | Description                        |
| ------- | -------- | ---------------------------------- |
| `Value` | `any`    | The raw lua value of the object.   |
| `Type`  | `string` | The type of the value as a string. |

---

## Globals

### rnet.sendQuantity

```lua
rnet.sendQuantity: number
```

### rnet.propertyTypes

`🔒 Read-only`

```lua
rnet.propertyTypes: dictionary<string, number>
```

Contains the Id byte for each type.

## Singletons

### rnet.packetWriter

This singleton is used to build packets, which can be sent by the **RakNet** library.

---

#### Example
This code will simulate a chat packet.
```lua
local pw = genv.rnet.packetWriter;
pw:start();
pw:writeByte(0x87);
pw:writeInstance(game:GetService("Players").LocalPlayer);
pw:writeUInt32BE(string.len(str));
pw:writeString(str);
pw:writeByte(1);
pw:writeByte(1);
rnet.send(pw:finish())
```

---

#### rnet.packetWriter:get

```lua
function rnet.packetWriter:get(): array
```

Gets the bytes that are currently within the writer.

---

#### rnet.packetWriter:start

```lua
function rnet.packetWriter:start()
```

Clears the bytes stored inside the packet writer in order to begin creating a new packet.

---

#### rnet.packetWriter:finish

```lua
function rnet.packetWriter:finish(): array
```

Finishes the currently generated packet and returns the packet in an array of bytes.

---

#### rnet.packetWriter:writeByte

```lua
function rnet.packetWriter:writeByte(byte: number)
```

Append a single byte to the current packet.

---

#### rnet.packetWriter:writeChar

```lua
function rnet.packetWriter:writeChar(c: char)
```

Converts the `char` to a byte and appends it to the current packet.

---

#### rnet.packetWriter:writeBytes

```lua
function rnet.packetWriter:writeBytes(bytes: array)
```

Appends the given bytes to the current packet.

---

#### rnet.packetWriter:writeString

```lua
function rnet.packetWriter:writeString(str: string)
```

Iterates over each character in the string and uses `rnet.packetWriter:writeChar` to append the character to the current packet.

---

#### rnet.packetWriter:writeVarString

```lua
function rnet.packetWriter:writeVarString(str: string)
```

First writes the length of the string as a 64-bit integer, then iterates over each character in the string and uses `rnet.packetWriter:writeChar` to append the character to the current packet.

---

#### rnet.packetWriter:writeUInt16LE

```lua
function rnet.packetWriter:writeUInt16LE(int: number)
```

Converts the number into 2 individual bytes in little endian format and appends them to the current packet.

---

#### rnet.packetWriter:writeUInt16BE

```lua
function rnet.packetWriter:writeUInt16BE(int: number)
```

Converts the number into 2 individual bytes in big endian format and appends them to the current packet.

---

#### rnet.packetWriter:writeUInt32LE

```lua
function rnet.packetWriter:writeUInt32LE(int: number)
```

Converts the number into 4 individual bytes in little endian format and appends them to the current packet.

---

#### rnet.packetWriter:writeUInt32BE

```lua
function rnet.packetWriter:writeUInt32BE(int: number)
```

Converts the number into 4 individual bytes in big endian format and appends them to the current packet.

---

#### rnet.packetWriter:writeUInt64LE

```lua
function rnet.packetWriter:writeUInt64LE(int: number)
```

Converts the number into 8 individual bytes in little endian format and appends them to the current packet.

---

#### rnet.packetWriter:writeUInt64BE

```lua
function rnet.packetWriter:writeUInt64BE(int: number)
```

Converts the number into 8 individual bytes in big endian format and appends them to the current packet.

---

#### rnet.packetWriter:writeVarInt64

```lua
function rnet.packetWriter:writeVarInt64(int: number)
```

Encodes the given 64-bit unsigned integer using variable-length encoding and appends the resulting bytes to the current packet. Each byte stores 7 bits of the value, with the most significant bit (MSB) used as a continuation flag.

---

#### rnet.packetWriter:writeFloatLE

```lua
function rnet.packetWriter:writeFloatLE(float: number)
```

Converts the given floating-point number into 4 bytes in little-endian format and appends them to the current packet.

---

#### rnet.packetWriter:writeFloatBE

```lua
function rnet.packetWriter:writeFloatBE(float: number)
```

Converts the given floating-point number into 4 bytes in big-endian format and appends them to the current packet.

---

#### rnet.packetWriter:writeDoubleBE

```lua
function rnet.packetWriter:writeDoubleBE(double: number)
```

Converts the given floating-point number (double precision) into 8 bytes in big-endian format and appends them to the current packet.

---

#### rnet.packetWriter:writeStreamInfo

```lua
function rnet.packetWriter:writeStreamInfo(is32Bit: boolean, x: number, y: number, z: number)
```

Appends a single byte to the current packet: `1` if `is32Bit` is `true`, or `0` if `is32Bit` is `false`.

Next, appends the values of `x`, `y`, and `z` to the packet. If `is32Bit` is `true`, these values are written using `rnet.packetWriter:writeUInt32BE`. Otherwise, they are written using `rnet.packetWriter:writeByte`.

---

#### rnet.packetWriter:writeInstance

```lua
function rnet.packetWriter:writeInstance(instance: Instance | dictionary | nil)
```

If an `Instance` is provided, `rnet.fetchObject` is used to retrieve the corresponding object and processes it as if a `dictionary` was supplied.

If a `dictionary` is provided, the following steps are performed:
1. The `peerId` is appended to the current packet using `rnet.packetWriter:writeVarInt64`.
2. The `id` is appended to the current packet using `rnet.packetWriter:writeUInt32LE`.

If `nil` is provided, `0` is written using `rnet.packetWriter:writeVarInt64`.

---

#### rnet.packetWriter:writeEnum

<!-- TODO: Document this function -->

> **Warning**
> Currently non-functional in Celery

```lua
function rnet.packetWriter:writeEnum(enumItem: EnumItem)
```

---

#### rnet.packetWriter:writeTable

```lua
function rnet.packetWriter:writeTable(t: array)
```

Appends a `table` as an `array` to the current packet.

1. First, writes the number of entries in the array using `rnet.packetWriter:writeVarInt64`.
2. Then, iterates over each entry in the array and converts it into the [Object](#object) data type.
3. Finally, appends the object to the current packet using `rnet.packetWriter:writeObject`.

---

#### rnet.packetWriter:writeDictionary

```lua
function rnet.packetWriter:writeDictionary(t: dictionary)
```

Appends a `table` as a `dictionary` to the current packet.

1. First writes the number of entries in the dictionary using `rnet.packetWriter:writeVarInt64`.
2. Then, iterates over each entry in the dictionary and converts the key and value the [Object](#object) data type.
3. Finally, appends both objects for key and value to the current packet using `rnet.packetWriter:writeObject`.

---

#### rnet.packetWriter:writeVector3

```lua
function rnet.packetWriter:writeVector3(v3: Vector3)
```

Append a `Vector3` to the current packet.

#### rnet.packetWriter:writeRotation3

```lua
function rnet.packetWriter:writeRotation3(cf: CFrame)
```

Append the 3x3 rotation matrix from a `CFrame` to the current packet.

---

#### rnet.packetWriter:writeCFrame

```lua
function rnet.packetWriter:writeCFrame(cf: CFrame)
```

Appends the position of a `CFrame` to the current packet.

1. Writes the `X`, `Y`, and `Z` coordinates of the `CFrame` using `rnet.packetWriter:writeUInt32BE`.
2. Adds the value `2` as a single byte to the packet.

---

#### rnet.packetWriter:writeObject

```lua
function rnet.packetWriter:writeObject(obj: Object, includeType: boolean?)
```

Writes the provided object to the stream, adhering to Roblox's type-specific conventions. For instance, if a Lua number is supplied but a `float` is expected, the function will parse the number and write it as a `float` to the stream.

**Parameters**
* `obj`: The object to be written to the stream. Refer to the [Object](#object) data type for details on its structure.
* `includeType`: A boolean indicating whether to write the type identifier (as a byte) before the object. If `true`, the type number is included.

---

### rnet.packetReader

This singleton is used to parse packets that are in byte array form.

---

#### Example
```lua
local packet = rnet.nextpacket()
rnet.packetReader:use(packet.data)
for i = 1, #packet.data do
    print(rnet.packetReader:nextByte())
end
```

#### rnet.packetReader:use

```lua
function rnet.packetReader:use(packet: array)
```

Initializes a new reader stream using the provided data array.

**Parameters**
* `packet`: An array of bytes to be used as the input stream for the packet reader.

---

#### rnet.packetReader:pos

```lua
function rnet.packetReader:pos(): number
```

Returns the current position of the reader inside the stream.

**Returns**
A number representing the current position in the stream.

---

#### rnet.packetReader:nextByte

```lua
function rnet.packetReader:nextByte(): number
```

Returns the byte at the current position in the stream and increments the position by 1.

**Returns**
A number representing the byte value (0-255).

---

#### rnet.packetReader:nextChar

```lua
function rnet.packetReader:nextChar(): char
```

Returns the byte at the current position in the stream as a single character and increments the position by 1.

**Returns**
A single-character string representing the byte.

---

#### rnet.packetReader:nextUInt16BE

```lua
function rnet.packetReader:nextUInt16BE(): number
```

Reads the next 2 bytes from the stream as an unsigned 16-bit integer in big-endian order.

**Returns**
A number representing the unsigned 16-bit integer.

---

#### rnet.packetReader:nextUInt16LE

```lua
function rnet.packetReader:nextUInt16LE(): number
```

Reads the next 2 bytes from the stream as an unsigned 16-bit integer in little-endian order.

**Returns**
A number representing the unsigned 16-bit integer.

---

#### rnet.packetReader:nextUInt32BE

```lua
function rnet.packetReader:nextUInt32BE(): number
```

Reads the next 4 bytes from the stream as an unsigned 32-bit integer in big-endian order.

**Returns**
A number representing the unsigned 32-bit integer.

---

#### rnet.packetReader:nextUInt32LE

```lua
function rnet.packetReader:nextUInt32LE(): number
```

Reads the next 4 bytes from the stream as an unsigned 32-bit integer in little-endian order.

**Returns**
A number representing the unsigned 32-bit integer.

---

#### rnet.packetReader:nextUInt64LE

```lua
function rnet.packetReader:nextUInt64LE(): number
```

Reads the next 8 bytes from the stream as an unsigned 64-bit integer in little-endian order.

**Returns**
A number representing the unsigned 64-bit integer.

---

#### rnet.packetReader:nextVarInt64

```lua
function rnet.packetReader:nextVarInt64(): number
```

Reads a variable-length integer (up to 64 bits) from the stream. Each byte contributes 7 bits to the value, with the most significant bit indicating whether more bytes follow.

**Returns**
A number representing the variable-length integer.

---

#### rnet.packetReader:nextString

```lua
function rnet.packetReader:nextString(): string
```

Reads a string from the stream. The string length is first read as a variable-length integer, followed by that many characters.

**Returns**
A string containing the read characters.

---

#### rnet.packetReader:nextFloat

```lua
function rnet.packetReader:nextFloat(): number
```

Reads the next 4 bytes from the stream as a 32-bit floating-point number in little-endian order.

**Returns**
A number representing the floating-point value.

---

#### rnet.packetReader:nextDouble

```lua
function rnet.packetReader:nextDouble(): number
```

Reads the next 8 bytes from the stream as a 64-bit floating-point number in big-endian order.

**Returns**
A number representing the double-precision floating-point value.

---

#### rnet.packetReader:nextStreamInfo

<!-- TODO: Validate this documentation -->

```lua
function rnet.packetReader:nextStreamInfo(): dictionary
```

Reads a stream info object from the stream. If the first byte is non-zero, reads three 32-bit unsigned integers (x, y, z) in big-endian order. Otherwise, reads three bytes (x, y, z).

**Returns**
A table with fields `x`, `y`, and `z` representing the coordinates.

---

#### rnet.packetReader:nextTable

```lua
function rnet.packetReader:nextTable(): array
```

Reads an `array` from the stream. The `array` length is first read as a variable-length integer, followed by that many objects read using `nextObject`.

**Returns**
A table containing the read objects.

---

#### rnet.packetReader:nextObject

```lua
function rnet.packetReader:nextObject(type: number?): any?
```

Reads the next object from the stream, either based on a provided type or by determining the type from the stream. Supports various data types including Roblox-specific types and basic network data types.

**Parameters**
* `type` (optional): A number indicating the type of the next value to read. If not provided, the type is read from the stream as a byte.

**Returns**
The decoded value, which can be:
* An instance table (for `Instance` type).
* `table` (for `array` type)
* `number` (for `float` or `number` types)
* `Vector2`
* `Vector3`
* `boolean` (for `boolean` type)
* `string` (for `string` or `StringNotCached` types)
* `table` with `peerId`, `id`, and `bytes` fields set to 0 (for `nil` type)

---

#### rnet.packetReader:nextInstance

```lua
function rnet.packetReader:nextInstance(): table
```

Reads an instance identifier from the stream, consisting of a variable-length peer ID and a 32-bit instance ID in little-endian order.

**Returns**
* A table with the following fields:
  * `peerId`: A number representing the peer ID (0 if no instance).
  * `id`: A number representing the instance ID (0 if no instance).
  * If `peerId` is 0, returns a table with both `peerId` and `id` set to 0.

---

## Functions

### rnet.ack

<!-- TODO: Verify that this function is correctly documented -->

```lua
function rnet.ack(instance: Instance, version: number?)
```

Send an `ack` packet

#### Parameters
* `instance`: The instance to send the `ack` for.
* `version` (optional): The version of the instance.

---

### rnet.blockcreates

```lua
function rnet.blockcreates(toggle: boolean)
-- Alias: rnet.blockCreates
```

Enables or disables filtering of `"ID_NEW_INSTANCE"` packets, currently limited to blocking `RightGrip` creation.

#### Parameters
* `toggle`: A boolean indicating whether to filter create instance packets (`true` to enable, `false` to disable).

#### Example
```lua
rnet.blockcreates(true) -- Blocks RightGrip creation packets.
```

---

### rnet.blockdeletes

```lua
function rnet.blockdeletes(toggle: boolean)
-- Alias: rnet.blockDeletes
```

Enables or disables filtering of `"ID_DELETE_INSTANCE"` packets, preventing client-side deletions from being sent to the server.

#### Parameters
* `toggle`: A boolean indicating whether to filter delete instance packets (`true` to enable, `false` to disable).

#### Example
```lua
local player = game:GetService("Players").LocalPlayer
local character = player.Character

rnet.blockdeletes(true)
character["Left Leg"]:Destroy() -- Deletion is not sent to the server.
```

---

### rnet.Capture:Connect

<!-- TODO: Verify this function is correctly documented -->

```lua
function rnet.Capture:Connect(callback: function(packet: Packet)): RBXScriptConnection
```

Creates an `RBXScriptConnection`, which gets called every time a packet gets received.

Refer to the [Packet](#packet) data type to see what it contains.

#### Parameters
* `callback`: A function that gets called every time a packet gets received with the packet data type.

#### Example
```lua
local con = rnet.Capture:Connect(function(packet)
    for i = 1,#packet.data do
        print(packet.data[i])
    end
end)
```

---

### rnet.chat

```lua
function rnet.chat(message: string)
```

Sends a simulated chat message packet.

#### Parameters
* `message`: The message to be sent in the chat.

#### Example
```lua
rnet.chat("Hello, World!")
```

---

### rnet.clearfilter

```lua
function rnet.clearfilter()
-- Alias: rnet.clearFilter
```

Clears the currently applied packet filter.

This function is equivalent to the following:
```lua
rnet.setfilter({})
```

---

### rnet.destroy

```lua
function rnet.destroy(instance: Instance)
```

Sends a `"DELETE_INSTANCE"` packet to destroy the specified `instance`.

#### Parameters
* `instance`: The instance to destroy.

#### Example
```lua
local player = game:GetService("Players").LocalPlayer
local character = player.Character

rnet.destroy(character.Torso.Neck) -- Destroys the neck weld
```

---

### rnet.disconnect

```lua
function rnet.disconnect()
```

Destroys the client's replicator, disconnecting the client from the server.

#### Example
```lua
local player = game:GetService("Players").LocalPlayer
local character = player.Character

character.Humanoid.Died:Connect(function()
    rnet.disconnect() -- Disconnects the client upon death.
end)
```

---

### rnet.equiptool

```lua
function rnet.equiptool(tool: Tool, weld: Weld?, alsoUnequip: boolean?)
-- Alias: rnet.equipTool
```

Equips the specified `tool` to the character.

#### Parameters
* `tool`: The tool to equip.
* `weld`: An optional custom weld for the tool.
* `alsoUnequip`: An optional boolean indicating whether to unequip the tool afterward.

#### Example
```lua
local player = game:GetService("Players").LocalPlayer
local backpack = player.Backpack

rnet.equiptool(backpack:GetChildren()[1]) -- Equips the first tool in the backpack.
```

---

### rnet.fetchobject

<!-- TODO: Document rnet.fetchobject -->

```lua
function rnet.fetchobject(...: any): any?
-- Alias: rnet.fetchObject
```

#### Parameters
* `...`: 

---

### rnet.fetchobjectbytes

<!-- TODO: Document rnet.fetchobjectbytes -->

```lua
function rnet.fetchobjectbytes(...: any): array
-- Alias: rnet.fetchObjectBytes
```

#### Parameters
* `...`: 

---

### rnet.fireevent

```lua
function rnet.fireevent(instance: Instance, eventName: string, ...: any)
-- Alias: rnet.fireEvent
```

Triggers the specified event (`eventName`) on the given `instance` with the provided arguments. This function is equivelent to Synapse/SW's `firesignal`.

#### Parameters
* `instance`: The target instance on which to fire the event.
* `eventName`: The name of the event to trigger.
* `...`: Variable arguments to pass to the event.

#### Example
```lua
local player = game:GetService("Players").LocalPlayer
local backpack = player.Backpack
local character = player.Character

-- Equips the first tool in the player's backpack on the server.
rnet.fireevent(character.Humanoid, "ServerEquipTool", backpack:GetChildren()[1])
```

---

### rnet.fireremote

```lua
function rnet.fireremote(remote: RemoteEvent | RemoteFunction, count: number, ...)
-- Alias: rnet.fireRemote
```

Fires the specified `remote` (RemoteEvent or RemoteFunction) `count` times with the given arguments.

#### Parameters
* `remote`: The remote to fire.
* `count`: The number of times to fire the remote.
* `...`: Arguments to pass to the remote.

#### Example
```lua
local remote = game:GetService("ReplicatedStorage").DefaultMakeChatSystemEvents.SayMessageRequest

rnet.fireremote(remote, 5, "message", "All") -- Sends the message "message" to all players 5 times.
```

---

### rnet.geteventid

```lua
function rnet.geteventid(name: string): number
-- Alias: rnet.getEventId
```

Retrieves the Id associated with the specified event name.

#### Parameters
* `name`: The event name of which to get the id from.

#### Example
```lua
local eventId = rnet.geteventid("ServerEquipTool")
print("Event ID:", eventId)
```

---

### rnet.getevents

> **Warning**
> Currently unimplemented in Celery

```lua
function rnet.getevents(instance: Instance): array
-- Alias: rnet.getEvents
```

Retrieves a table containing the names and metadata of events associated with the specified `instance`.

#### Parameters
* `instance`: The instance to query for events.

#### Example
```lua
local player = game:GetService("Players").LocalPlayer
local character = player.Character
local eventNames = rnet.getevents(character.Humanoid)

table.foreach(eventNames, print) -- Prints the list of event names.
```

---

### rnet.getproperties

> **Warning**
> Currently unimplemented in Celery

```lua
function rnet.getproperties(instance: Instance): array
```

Returns a table of hidden properties for the specified `instance`.

#### Parameters
* `instance`: The instance to query for hidden properties.

#### Example
```lua
local player = game:GetService("Players").LocalPlayer
local character = player.Character
local hiddenProperties = rnet.getproperties(character.Humanoid)

table.foreach(hiddenProperties, print) -- Prints the list of hidden properties.
```

---

### rnet.getpropertyid

```lua
function rnet.getpropertyid(name: string): number
-- Alias: rnet.getPropertyId
```

Retrieves the Id associated with the specified property name.

#### Parameters
* `name`: The property name of which to get the id from.

#### Example
```lua
local propertyId = rnet.getpropertyid("Transparency")
print("Property Id for Transparency:", propertyId)
```

---

### rnet.getInstance

<!-- TODO: Document this function -->

> **Warning**
> Currently non-functional in Celery

```lua
function rnet.getInstance()
```

---

### rnet.getInstanceFromId

<!-- TODO: Verify this function is correctly documented -->

> **Warning**
> Currently non-functional in Celery

```lua
function rnet.getInstanceFromId(id: number, peer_id: number): Instance?
```

Gets the instance based on it's Id and Peer Id. If no instance is found, `nil` is returned.

#### Parameters
* `id`: The Id of the instance.
* `peer_id`: The peer from which to get the instance.

---

### rnet.newinstance

```lua
function rnet.newinstance(instance: Instance, parent: Instance)
-- Alias: rnet.newInstance
```

Registers a new instance on the server.

> **Note**
> Only supports RightGrip weld for tools right now

#### Parameters
* `instance`: The `Instance` to register.
* `parent`: The parent of the `Instance` to register.

---

### rnet.nextevent

<!-- TODO: Verify this function is correctly documented -->

```lua
function rnet.nextevent(): Packet
-- Alias: rnet.nextEvent
```

Retrieves the next packet that is an event by verifying the following condition:
```lua
packet.id == 0x83 or packet.data[1] == 3 or packet.data[2] == 1
```

#### Example
```lua
local packetNumber = 0

while true do
    task.wait()
    local packet = rnet.nextevent()
    
    print(string.format("Packet #%d: Event %d", 
        packetNumber, 
        packet.id, 
    ))

    packetNumber += 1
end
```

---

### rnet.nextpacket

```lua
function rnet.nextpacket(): Packet
-- Alias: rnet.nextPacket
```

Retrieves the most recent packet sent by the client to the server.

Refer to the [Packet](#packet) data type to see what it contains.

#### Example
```lua
local packetNumber = 0

while true do
    task.wait()
    local packetData = rnet.nextpacket()
    
    if packetData.id == 0x83 and packetData.sub_id == 0x7 then
        pcall(function()
            local packetInfo = rnet.readeventpacket(packetData)
            print(string.format("Packet #%d: Event [%s] on %s", 
                packetNumber, 
                packetInfo.name, 
                packetInfo.source:GetFullName()
            ))
        end)
    end
    packetNumber += 1
end
```

---

### rnet.playanimation

<!-- TODO: Document this function -->

```lua
function rnet.playanimation(id: string | number | nil, float1: number?, float2: number?, float3: number?, float4: number?)
-- Alias: rnet.playAnimation
```

---

### rnet.readeventpacket

> **Warning**
> Currently unimplemented in Celery

```lua
function rnet.readeventpacket(packet: Packet): dictionary
-- Alias: rnet.readEventPacket
```

Decodes a packet and returns a dictionary containing event information. This function is useful for analyzing packet-based events.

Refer to the [Packet](#packet) data type to see what it contains.

#### Return Value
A dictionary with the following fields:
| Field       | Type     | Description                             |
| ----------- | -------- | --------------------------------------- |
| `source`    | Instance | The instance associated with the event. |
| `name`      | string   | The name of the triggered event.        |
| `arguments` | array    | The arguments passed to the event.      |

#### Parameters
* `packet`: The packet to decode.

#### Example
```lua
local packetNumber = 0

while true do
    task.wait()
    local packetData = rnet.nextpacket()
    
    if packetData.id == 0x83 and packetData.sub_id == 0x7 then
        pcall(function()
            local packetInfo = rnet.readeventpacket(packetData)
            print(string.format("Packet #%d: Event [%s] on %s", 
                packetNumber, 
                packetInfo.name, 
                packetInfo.source:GetFullName()
            ))
        end)
    end
    packetNumber += 1
end
```

**Note**: This function may exhibit inconsistent behavior in certain scenarios.

---

### rnet.replay

```lua
function rnet.replay()
```

Replay the last packet that was generated using the packet writer.

#### Example
```lua
rnet.chat("Hello")
rnet.replay() -- Sends "Hello" again
```

### rnet.send

```lua
function rnet.send(packetsList: array)
-- Aliases: rnet.sendpacket, rnet.sendPacket and rnet.send_packet
```

Sends custom packets specified in `packetsList`. Packet values must be in byte-hex format.

> **Warning**
> Sending arbitrary packets may result in a ban. Use with caution.

#### Parameters
* `packetsList`: An array of packets to send.

#### Example
```lua
rnet.send({{0x2, 0x65, 0x108, 0x69}}) -- Sends a custom packet (example).
```

---

### rnet.sendphysics

```lua
function rnet.sendphysics(position: CFrame)
-- Alias: rnet.sendPhysics
```

Sends physics packets to update the character's position (as a `CFrame`) to other clients.

#### Parameters
* `position`: The `CFrame` representing the character's new position.

#### Example
```lua
local position = Vector3.new(0, -69420, 0)

while true do
    task.wait()
    -- Teleports the character to (0, -69420, 0), making you invisible to other players.
    rnet.sendphysics(CFrame.identity + position)
end
```

---

### rnet.setfilter

```lua
function rnet.setfilter(filteringPacketIds: array)
-- Alias: rnet.setFilter
```

Filters packets by their IDs or sub-IDs, preventing the client from sending packets that match the specified criteria.

#### Parameters
* `filteringPacketIds`: An array of packet IDs or sub-IDs to filter.

#### Example
```lua
-- Filters packets starting with the specified ID sequence.
rnet.setfilter({{0x2, 0x65, 0x108, 0x69}})
```

---

### rnet.setparent

```lua
function rnet.setparent(instance: Instance, newParent: Instance)
-- Alias: rnet.setParent
```

Sets the parent of the specified `instance` to `newParent`.

#### Parameters
* `instance`: The instance to reparent.
* `newParent`: The new parent instance.

#### Example
```lua
local player = game:GetService("Players").LocalPlayer
local character = player.Character
local humanoid = character.Humanoid

rnet.setparent(humanoid, workspace) -- Sets the humanoid's parent to the workspace.
```

---

### rnet.setphysicsrootpart

> **Warning**
> Currently unimplemented in Celery

```lua
function rnet.setphysicsrootpart(instance: BasePart)
```

Attempts to set the specified `instance` as the root part for character physics calculations.

**Issue**: This function is currently non-functional.

#### Parameters
* `instance`: The instance to set as the physics root part.

#### Example
```lua
-- No example available until the function is operational.
```

---

### rnet.setproperty

```lua
function rnet.setproperty(instance: Instance, propertyName: string, value: any)
-- Alias: rnet.setProperty
```

Sets the value of a hidden property (`propertyName`) on the specified `instance`.

#### Parameters
* `instance`: The instance to modify.
* `propertyName`: The name of the hidden property to set.
* `value`: The value to assign to the property.

#### Example
```lua
local player = game:GetService("Players").LocalPlayer
local character = player.Character
local humanoid = character.Humanoid

rnet.setproperty(humanoid, "AHiddenProperty", true) -- Sets a hidden property (example).
```

---

### rnet.sit

```lua
function rnet.sit(seat: SeatPart, humanoid: Humanoid)
```

Forces the specified `humanoid` to sit on the given `seat`, creating a weld on the server.

#### Parameters
* `seat`: The `SeatPart` to sit on.
* `humanoid`: The humanoid to seat.

#### Example
```lua
local player = game:GetService("Players").LocalPlayer
local character = player.Character
local seat = workspace.SeatPart

rnet.sit(seat, character.Humanoid) -- Seats the player on the specified seat.
```

---

### rnet.touch

```lua
function rnet.touch(part1: BasePart, part2: BasePart)
```

Simulates a touch interaction between `part1` and `part2`.

#### Parameters
* `part1`: The part being touched.
* `part2`: The part initiating the touch.

#### Example
```lua
local player = game:GetService("Players").LocalPlayer
local character = player.Character
local killbrick = workspace.KillBrick

rnet.touch(killbrick, character.HumanoidRootPart) -- Simulates a touch, potentially killing the character.
```

---

### rnet.unequiptool

```lua
function rnet.unequiptool(tool: Tool)
```

Unequips the specified `tool` from the character.

#### Parameters
* `tool`: The tool to unequip.

#### Example
```lua
local player = game:GetService("Players").LocalPlayer
local character = player.Character

rnet.unequiptool(character:FindFirstChildOfClass("Tool")) -- Unequips the equipped tool.
```

---

### rnet.unsit

```lua
function rnet.unsit(seat: SeatPart)
```

Forces the currently seated `humanoid` that is on the given `seat` to unsit.

#### Parameters
* `seat`: The seat of which to unseat the `humanoid` from.

#### Example
```lua
rnet.unsit(workspace.SeatPart)
```

---

### rnet.walk

```lua
function rnet.walk(speed: number?)
```

Force the `LocalPlayer` to walk.

#### Parameters
* `speed` (optional): The speed the `LocalPlayer` will walk at.

---
