 Universal Asynchronous Receiver-Transmitter (UART) – Verilog Implementation


The Universal Asynchronous Receiver-Transmitter (UART) is one of the most widely used hardware communication protocols for serial data exchange.
It provides a simple way to transmit and receive data bit by bit over a single line, without requiring a separate clock signal between the sender and receiver.

UART is asynchronous → meaning no shared clock is transmitted. Instead, both devices agree on a baud rate (speed of communication) beforehand.

Data Format:

Idle Line → High (`1`)
Start Bit → Low (`0`)
Data Bits → Typically 8 bits (LSB first)
Parity Bit (optional) → For error checking
Stop Bit(s)→ High (`1`), usually 1 or 2 bits

Example for sending byte `0xA5 (10100101)` at 1 stop bit:

Idle (1) → Start (0) → 10100101 → Stop (1)  


Baud Rate

Baud rate defines how many symbols (bits) per second are transmitted.

Formula:

Baud Rate= fclk/Clock Divisor

fclk	​= System clock frequency (e.g., 50 MHz)

Clock Divisor = Number of clock cycles per UART bit​


Example:

System Clock = 50 MHz

Baud Rate = 9600 bps

Clock Divisor = 50,000,000/9600 ~ 5208
So, UART must hold each bit for 5208 clock cycles before shifting the next.

UART Transmitter (TX) Working

1. Wait until new data is available (`newd = 1`).
2. Load the parallel data (`tx_data`) into a shift register.
3. Append start bit (0), then transmit each bit LSB first.
4. After all bits, append stop bit (1).
5. Raise `donetx = 1` to indicate completion.

UART Receiver (RX) Working

1. Wait for the line to go low → detect start bit.
2. Wait half bit-time to sample at the center of each bit.
3. Shift received bits into an 8-bit register.
4. After 8 bits, check stop bit.
5. Raise `donerx = 1` and output data (`rx_data`).

Features of This Project

Configurable Parameters

  1.System clock frequency (`clk_freq`)
  2.Baud rate (`baud_rate`)
  3.Transmitter and Receiver Modules
  4.Top-Level Integration (`uart_top`) for loopback test
  5.Start & Stop bit generation/checking
  6.Flags for TX/RX completion (`donetx`, `donerx`)

 Applications

* Communication between FPGA and PC (via RS-232/USB).
* Debugging and printing logs from FPGA/MCU.
* Sensor/Peripheral data transfer.
* Embedded systems and IoT devices.

 Future Enhancements

* Add parity bit support.
* Add multiple stop bits option.
* Implement FIFO buffers for continuous data flow.
* Add oversampling in RX for noise tolerance.
* Wrap inside AXI/APB interface for SoC integration.


