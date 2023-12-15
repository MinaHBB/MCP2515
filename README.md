# MCP2515 Library
[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)
## Overview

This library offers a C implementation for the MCP2515 controller, originally designed for STM32 microcontrollers. It can be adapted for other MCUs by modifying specific "HAL functions".

## Source

This library is based on [autowp/arduino-mcp2515](https://github.com/autowp/arduino-mcp2515/tree/master) with additional improvements and modifications to support C and STM32 MCUs.

## Usage

### Initialization

To start working with the MCP2515, you can use the below code:

```c
MCP_reset();    // reset the MCP2515
MCP_setBitrate(CAN_500KBPS, MCP_20MHZ); // specify the CAN bitrate and oscillator frequency
MCP_setNormalMode(); // switch to Normal mode
```

### Sending Messages

To transmit messages on the CAN line, you need to define a CAN frame using the can.h header file and configure the message in this frame. Below function can be used to send the configured message:

```c
canFrame.id = 0x01;
canFrame.dlc = 1;
canFrame.data[0] = 1;
MCP_sendMessage(&canFrame);
```

### Receiving Messages

#### Interrupt-driven method

To receive messages using this method, you can use the I̅N̅T̅ pin. The falling edge of this pin, announces an interrupt. You can use the below algorithm as a sample for the ISR:

```c
if (GPIO_Pin == SPI_INT_Pin)
{
    while (MCP_checkReceive()) // Add timeout if needed
    {
        if (MCP_readMessage(&canFrame) == ERROR_OK)
        {
            // Process data or set a flag
        }
    }
    MCP_clearInterrupts();
}
```

#### Polling method
Alternatively, polling can be used for receiving messages.

### Setting Filters and Masks
Configure filters and masks for received messages using these functions:

```c
// filter messages with ID = 0x01:
MCP_setFilterMask(MASK0, false, 0xFF);
MCP_setFilterMask(MASK1, false, 0xFF);
MCP_setFilter(RXF0, false, 0x01);
```

Make sure to adapt the "HAL functions" when porting the library to different MCUs.

For more details on usage, refer to the library documentation.

Feel free to customize the content further based on your specific requirements.