# NFSLibrary

A .NET NFS (Network File System) client library supporting NFS v2, v3, and v4.1 protocols.

## About This Repository

This repository is a maintained fork published because the upstream library appeared to be abandoned and required maintenance updates. The library has been updated to target modern .NET frameworks while preserving full backward compatibility.

### Target Frameworks
- .NET Standard 2.1
- .NET 8.0
- .NET 10.0

## Features

- **NFS Protocol Support**: Full client implementation for NFS v2, v3, and v4.1
- **File Operations**: Read, write, create, delete, and move files
- **Directory Operations**: Create, delete, and list directories
- **Mount/Unmount**: Connect to and disconnect from NFS exports
- **Permissions**: Full Unix permission (mode) support for file and directory creation
- **Attributes**: Read file/directory attributes including size, timestamps, and type
- **Streaming**: Support for stream-based read/write operations
- **Events**: Data transfer events for progress monitoring

## Basic Usage

```csharp
using NFSLibrary;
using System.Net;

// Create an NFS v3 client
var client = new NfsClient(NfsClient.NfsVersion.V3);

// Connect to the server
client.Connect(IPAddress.Parse("192.168.1.100"));

// List available exports
var exports = client.GetExportedDevices();

// Mount an export
client.MountDevice("/export/share");

// List directory contents
var items = client.GetItemList(".");

// Read a file
client.Read("path/to/file.txt", "C:\\local\\file.txt");

// Write a file
client.Write("path/to/remote.txt", "C:\\local\\source.txt");

// Unmount and disconnect
client.UnMountDevice();
client.Disconnect();
```

## License

This library is licensed under the **GNU Lesser General Public License v3.0 (LGPL-3.0)**.

Per the LGPL-3.0 license terms:

1. **Attribution**: Original copyrights, license notices, and author attributions must be preserved in all copies and derivative works.
2. **Source Code**: Any modifications to this library must be made available to the community under the same LGPL-3.0 license terms.
3. **Notice**: You must give prominent notice that the library is used in your application and that it is covered by the LGPL license.
4. **Combined Works**: Applications using this library may be distributed under different license terms, provided the library portion remains under LGPL and can be replaced by users.

See the [LICENSE.txt](LICENSE.txt) file for the complete license text.

## Attribution and Copyright Chain

This library has been developed through multiple forks and contributions. All original copyrights are preserved as required by the license.

### NekoDrive / NFSLibrary
The core NFS client implementation.

- **Original Project**: NekoDrive
- **Original Source**: https://code.google.com/p/nekodrive (archived)
- **GitHub Mirror**: https://github.com/nekoni/nekodrive

### NFS-Client Fork
Improvements and modernization of the NFSLibrary.

- **Author**: Randy von der Weide (SonnyX)
- **Repository**: https://github.com/SonnyX/NFS-Client
- **Copyright**: See original repository

### Remote Tea ONC/RPC for .NET

This library includes the Remote Tea ONC/RPC implementation for .NET, which provides the underlying RPC communication layer.

```
Copyright (c) 1999, 2000
Lehrstuhl fuer Prozessleittechnik (PLT), RWTH Aachen
D-52064 Aachen, Germany.
All rights reserved.

Authors: Harald Albrecht, Jay Walters
```

- **Original Project**: Remote Tea
- **License**: GNU Library General Public License (LGPL)
- **Source**: The Remote Tea project was originally a Java implementation, with .NET port included in NekoDrive

## API Reference

### NfsClient Class

#### Constructor
```csharp
NfsClient(NfsVersion version)
```
Creates a new NFS client for the specified protocol version (V2, V3, or V4).

#### Connection Methods
- `Connect(IPAddress address)` - Connect with default settings
- `Connect(IPAddress address, int userId, int groupId, int commandTimeout)` - Connect with credentials
- `Connect(IPAddress address, int userId, int groupId, int commandTimeout, Encoding characterEncoding, bool useSecurePort, bool useCache)` - Full connection options
- `Disconnect()` - Close the connection

#### Mount Methods
- `GetExportedDevices()` - List available NFS exports
- `MountDevice(string deviceName)` - Mount an NFS export
- `UnMountDevice()` - Unmount the current export

#### File Operations
- `Read(string sourceFileFullName, string destinationFileFullName)` - Copy remote file to local
- `Read(string sourceFileFullName, ref Stream outputStream)` - Read remote file to stream
- `Write(string destinationFileFullName, string sourceFileFullName)` - Copy local file to remote
- `Write(string destinationFileFullName, Stream inputStream)` - Write stream to remote file
- `CreateFile(string fileFullName)` - Create an empty file
- `CreateFile(string fileFullName, NFSPermission mode)` - Create file with permissions
- `DeleteFile(string fileFullName)` - Delete a file
- `Move(string sourceFileFullName, string targetFileFullName)` - Move/rename a file
- `SetFileSize(string fileFullName, long size)` - Set file size

#### Directory Operations
- `GetItemList(string directoryFullName)` - List directory contents
- `CreateDirectory(string directoryFullName)` - Create a directory
- `CreateDirectory(string directoryFullName, NFSPermission mode)` - Create with permissions
- `DeleteDirectory(string directoryFullName)` - Delete directory (recursive by default)
- `IsDirectory(string directoryFullName)` - Check if path is a directory

#### Attributes
- `GetItemAttributes(string itemFullName)` - Get file/directory attributes
- `FileExists(string fileFullName)` - Check if file/directory exists

#### Properties
- `IsMounted` - Whether an export is currently mounted
- `IsConnected` - Whether connected to a server
- `Mode` - Default permissions for new files/directories
- `blockSize` - Transfer block size

#### Events
- `DataEvent` - Fired during data transfer with byte count

## Contributing

Contributions are welcome. Please ensure that:

1. All original copyright notices and attributions are preserved
2. Any modifications are made available under the LGPL-3.0 license
3. New code follows the existing coding style

## See Also

- [NFS Protocol (RFC 1813 - NFSv3)](https://tools.ietf.org/html/rfc1813)
- [NFS Protocol (RFC 7530 - NFSv4)](https://tools.ietf.org/html/rfc7530)
- [NFS Protocol (RFC 8881 - NFSv4.1)](https://tools.ietf.org/html/rfc8881)
