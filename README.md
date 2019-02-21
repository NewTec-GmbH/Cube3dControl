# Cube3dControl

[![License](https://img.shields.io/badge/license-MIT-blue.svg)](http://choosealicense.com/licenses/mit/)

PC program to control a 3d LED cube.

## Cinema Protocol

Version 0.1

The Cinema protocol is used between a client, like a PC and a 3d LED cube server. The client sends commands to the server and the server answer with a corresponding response. The protocol shall be as short as possible for a fast communication. The communication byte order is little endian, which means in the following frames byte 0 is sent first.

### Command

A command is defined by a command id and optional data. The data size depends on the command.

* Byte 0: Command id
* Byte 1-N: Data

### Response

A response is defined by a status and optional data. The data size depends on the command.

* Byte 0: Status
    * 0: Ok
    * 1: Error
* Byte 1-N: Data

### Commands

#### Id 0: SYNC command

The SYNC command is used to get in synchronization with the server. It shall be send by the client after opening the serial connection and repeated periodically every 1s until a response is received.

| Byte | 0 | 1 | 2 | 3 | 4 |
|---|---|---|---|---|---|
|| 0x00 | 0x55 | 0xaa | 0x55 | 0xaa |

Response:

| Byte | 0 |
|---|---|
|| Status |

#### Id 1: Get Cinema protocol version

This command is used to get the Cinema protocol version of the server. It shall be requested right after the serial communication is synchronized. If the version is not supported by the client, the communication shall be stopped.

| Byte | 0 |
|---|---|
|| 0x01 |

Response:

| Byte | 0 | 1 | 2 |
|---|---|---|---|
|| Status | Major version | Minor version |

#### Id 2: Clear cube

This command is used to clear the cube (set all LED colors to black).

| Byte | 0 |
|---|---|
|| 0x02 |

Response:

| Byte | 0 |
|---|---|
|| Status |

#### Id 3: Set pen color

This command is used to set the pen color, which is used to draw.

| Byte | 0 | 1 | 2 | 3 |
|---|---|---|---|---|
|| 0x03 | Red | Green | Blue |

Response:

| Byte | 0 |
|---|---|
|| Status |

#### Id 4: Plot

This command is used to plot a point with the pen inside the cube.

| Byte | 0 | 1 | 2 | 3 |
|---|---|---|---|---|
|| 0x04 | X | Y | Z |

Response:

| Byte | 0 |
|---|---|
|| Status |

#### Id 5: Move to

This command is used to set the pen to a position inside the cube.

| Byte | 0 | 1 | 2 | 3 |
|---|---|---|---|---|
|| 0x05 | X | Y | Z |

Response:

| Byte | 0 |
|---|---|
|| Status |

#### Id 6: Line to

This command is used to draw a line from the current position to the given one with the pen.

| Byte | 0 | 1 | 2 | 3 |
|---|---|---|---|---|
|| 0x06 | X | Y | Z |

Response:

| Byte | 0 |
|---|---|
|| Status |

#### Id 7: Draw a 3d picture

This command is used to draw a complete 3d picture inside the cube, independed of the pen color.

| Byte | 0 | 1 | 2 | 3 | 4 | ... | 375 |
|---|---|---|---|---|---|---|---|
|| 0x07 | Red(0, 0, 0) | Green(0, 0, 0) | Blue(0, 0, 0) | Red(1, 0, 0) | ... | Blue(4, 4, 4) |

Response:

| Byte | 0 |
|---|---|
|| Status |
