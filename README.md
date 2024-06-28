| GENERIC NAME                          | TYPE                   | default                | DESCRIPTION                                                                                                                                                                                                                                                                                                                                                                                              |
|---------------------------------------|------------------------|------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| NUM_OF_ENGINES                        | Natural                | 4                      | Total Number of I2C engines . This will replicate corresponding registers and state machine to support multiple I2C engines on the same PIB responder interface. As of now it supports only upto 4 and minimum should be 1.If value = 2 then i2c ports corresponding to NUM_OF_PORTS_2 should be unused.If value = 1 then i2c ports corresponding to NUM_OF_PORTS_2 and NUM_OF_PORTS_1 should be unused. |
| NUM_OF_PORTS_0                        | Natural                | 14                     | This says number of ports which I2C Engine 0 should drive(supports) . Maximum vlaue is 64 and minmum should be 1.NUM_OF_PORTS_<ENGINE_NUM>                                                                                                                                                                                                                                                               |                                                                                                                                                                                                                                                                                                                                                                       
| NUM_OF_PORTS_1                        | Natural                | 14                     | This says number of ports which I2C Engine 1 should drive(supports) . Maximum vlaue is 64 and minmum should be 1                                                                                                                                                                                                                                                                                         |
| NUM_OF_PORTS_2                        | Natural                | 1                      | This says number of ports which I2C Engine 2 should drive(supports) . Maximum vlaue is 64 and minmum should be 1                                                                                                                                                                                                                                                                                         |                                                                                                                                                                                                                                                                                                     
| NUM_OF_PORTS_3                        | Natural                | 4                      | This says number of ports which I2C Engine 3 should drive(supports) . Maximum vlaue is 64 and minmum should be 1                                                                                                                                                                                                                                                                                         |                                                                                                                                                                                                                                                                                                                                      
| unaut_responder_<IDX>_< ENGINE_NUM>   | std_ulogic_vect to 7)  | or(0”FF”               | Should have responder_id which doesnot need access through this engine ENGINE NUM.                                                                                                                                                                                                                                                                                                                       |
| PIBrsp_BASE_ADDR                      | std_ulogic_vector      | 0x00040000             | This needs to get proper value of base address on PIB responder                                                                                                                                                                                                                                                                                                                                          |
| unaut_rsp0<IDX>_ptr_len_< ENGINE_NUM> | std_ulogic_vect to 3)  | or(0”0”                | Describes the width of address pointers in the responder corresponding to engine <ENGINE_NUM>.                                                                                                                                                                                                                                                                                                           |
| unaut_rsp(X)_port_vector(I)           | std_ulogic_vect to 63) | or(00xFFFFFFFFFFFFFFFF | The vector says which are all ports enabled for the corresponding responder_id checking i.e. – unaut_rsp(X)_port_vector(I) = ’1’ means - - - > unaut_responder(X) id is not secure id in port_number I. So access should be denied – unaut_rsp(X)_port_vector(I) = ’0’ means - - > unaut_responder(X) id is secure id in port_number I. So access should not be denied By Default Security check is enabled for all IDs all PORTs                                                                                                                                                                                                                                                                                                             |






Second table


| FSI i2cc dress Ad-   |   PIB i2cc Ad- dress | Read                 | Write                                  |
|---------------------|--------------|-------------------------------|----------------------------------------|                                                                                                    
| x’00’               | 0x04         | FIFO                          |                                        |
| x’01’               | 0x05         | Command Register              |                                        |
| x’02’               | 0x06         | Mode Register                 |                                        |
| x’03’               | 0x07         | Watermark Register            |                                        |
| x’04’               | 0x08         | Interrupt Mask Register       |                                        |
| x’05’               | 0x09         | Interrupt Conditions          | Write Set to Interrupt Mask Register   |
| x’06’               | 0x0A         | Interrupts                    | Write Clear to Interrupt Mask Register |
| x’07’               | 0x0B         | Status                        | Immediate Reset I2C                    |
| x’08’               | 0x0C         | Extended Status               | Immediate Reset Errors                 |
| x’09’               | 0x0D         | Residual Front End / Back End Length | Immediate Set S_SCL             |                                
| x’0A’               | 0x0E         | Port busy register            |                                        |
| x’0B’               | 0x0F         | Not used                      | Immediate Reset S_SCL                  |
| x’0C’               | 0x10         | Not used                      | Immediate Set S_SDA                    |
| x’0D’               | 0x11         | Not used                      | Immediate Reset S_SDA                  |


Third Table

| Reset State | 0x00000000                                                                                                         |
|-------------|--------------------------------------------------------------------------------------------------------------------|
| Bit         | Description                                                                                                        |
| 0           | With Start                                                                                                         |
| 1           | With Address                                                                                                       |
| 2           | Read Continue                                                                                                      |
| 3           | With Stop                                                                                                          |
| 04-05      | Not used                                                                                                           |
| 6-7      | Legacy_mode_interrupt steering bits : This decides where the interrupt generated by the I2C engine needs to go . It supports up to 4 controllers. Means we can drive interrupt to 4 different controllers. |        | 8-14      | Device Address                                                                                                     |
| 15          | Read / not Write                                                                                                   |
| 16 - 31     | Length in Bytes                                                                                                    |


Forth Table

| Command | With Start | With Stop  | With Ad-dress | Address   | Read / not Write    | Read Continue    | Length    |
|---------|------------|------|----------|-----------|-----------|----------|-----------|
| First   | 1          | 0    | 1        | 7 bit Addr.    | 1         | 1        | 64k Bytes |                                
| Second  | 0          | 0    | 0        | dont care | 1         | 1        | 64k Bytes |
| Third   | 0          | 0    | 0        | dont care | 1         | 1        | 64k Bytes |
| Last    | 0          | 1    | 0        | dont care | 1         | 0        | 8k Bytes  |






|  Feature                    | Description                                                       | Reference                                                                        |
|:----------------------------|:------------------------------------------------------------------|----------------------------------------------------------------------------------|
|  PIB responder interface    | The SPI controller unit provides a generic request / acknowledge  | [Host Bus Interfaces](#anchor-1)                                                 |
|                             | handshake interface which may interface to other bus systems.     |                                                                                  |
|                             | A project specific bus interface component may be required.       |                                                                                  |
|  Configurable SPI serial    | The SPI serial clock (SCK) is derived from the internal operating | [Clock Configuration, Trace Select, Reset Control, ECC enable](#anchor-2)        |
|  clock                      | clock. Using a configurable clock divider SCK can be configured   |                                                                                  |
|                             | to adapt to a wide range of clock frequency requirements.         |                                                                                  |
|  Highly Configurable        | Multiple shift counters and delay counters and elements enable    | [Sequencer Basic Operations](#anchor-3)                                          |
|                             | the SPI controller to handle a broad range of SPI responder       |                                                                                  |
|                             | devices. An internal sequencer enables flexible combinations of   |                                                                                  |
|                             | basic SPI operations.                                             |                                                                                  |
|  Hard and soft reset        | Resetting the SPI controller can be done via digital signal input | [Reset](#anchor-4)                                                               |
|                             | or via command interface.                                         |                                                                                  |
|  All functions available    | All SPI function control is mapped into the SPI register space    | [Programmer's Reference](#anchor-5)                                              |
|  via register interface     | to enable full control by firmware.                               |                                                                                  |
|  Support for serial         | In single mode data is shifted out serially on a single data line | [SPI Transmit and Receive Modes](#anchor-6)                                      |
|  MISO/MOSI                  | and received serially on a single data line. SPI clock mode 0 is  | [SPI Clock Modes](#anchor-7)                                                     |
|                             | implemented.                                                      |                                                                                  |
|  Single Controller          | One SPI controller is implemented and supported, there is no      |                                                                                  |
|  bus structure              | support for multiple controllers on the SPI bus.                  |                                                                                  |
|  Multiple SPI responders    | Each SPI controller is capable to drive up to 4 SPI chip select   | [Select Responders](#anchor-8)                                                   |
|                             | signals. Hence one controller may drive up to 4 SPI responders    |                                                                                  |
|                             | directly.                                                         |                                                                                  |
|  Internal parity protection | SPI internal registers are parity protected.                      | [Internal Parity Protection](#anchor-9)                                          |
|  Loop back mode             | Internal loop back mode is supported for testing purposes.        | [Clock Configuration, Trace Select, Reset Control, ECC enable](#anchor-2)        |
|  Pacing                     | Pacing mode is supported.                                         | [Pacing, Transmit Mode](#anchor-10)                                              |
|                             |                                                                   | [Pacing, Receive Mode](#anchor-11)                                               |
|  Secure Access control      | Security related aspects of external attached EEPROM memory are   | [SPI Security component](#anchor-12)                                             |
|                             | implemented by the Secure Access Control component.               |                                                                                  |
|  Memory mapped serial memory| When Flash or SEEPROM memory is attached to the SPI bus, its      | [Memory Mapped SPI attached Serial Memory](#anchor-13)                           |
|                             | content may be mapped directly to the corresponding address space.|                                                                                  |
|  ECC support                | When reading from SPI responder memory, ECC support is available. | [ECC Support](#anchor-14)                                                        |
|                             | Address correction applies when ECC is active.                    |                                                                                  |







new

| Register Name  |     |                    | SPI: Error Register  |        |                                                                                                                                                                                                                                                                                                  |
|----------------|-----|--------------------|----------------------|--------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Mnemonic       |     |                    | spictl_error_inject_ |        |                                                                                                                                                                                                                                                                                                  |
| Address offset |     |                    | 0x00                 |        |                                                                                                                                                                                                                                                                                                  |
| Description    |     |                    | SPI: error register  |        |                                                                                                                                                                                                                                                                                                  |
| Position       | Len | Field Mnemonic     | Type                 | Init   | Description                                                                                                                                                                                                                                                                                      |
|                |     |                    |                      | Value  |                                                                                                                                                                                                                                                                                                  |
| 0 to 7         | 8   | error_inject_pulse | RWX                  | ’x00’  | Error injection                                                                                                                                                                                                                                                                                  |
|                |     |                    |                      |        | bit 0: counter configuration register parity error bit 1: clock configuration register parity error bit 2: sequencer configuration register parity error                                                                                                                                         |
|                |     |                    |                      |        | bit 3: sequencer fsm error bit 4: shifter fsm error bit 5: pattern match reg parity error bit 6: transmit data reg parity error bit 7: receive data reg parity error                                                                                                                             |
| 8 to 15        | 8   | error_inject_mask  | RWX                  | ’x00’  | Error injection mask:                                                                                                                                                                                                                                                                            |
|                |     |                    |                      |        | bit 0: counter configuration register parity error bit 1: clock configuration register parity error bit 2: sequencer configuration register parity error                                                                                                                                         |
|                |     |                    |                      |        | bit 3: sequencer fsm error bit 4: shifter fsm error bit 5: pattern match reg parity error bit 6: transmit data reg parity error bit 7: receive data reg parity error                                                                                                                             |
| Position       | Len | Field Mnemonic     | Type                 | Init   | Description                                                                                                                                                                                                                                                                                      |
|                |     |                    |                      | Value  |                                                                                                                                                                                                                                                                                                  |
| 16 to 31       | 16  | error_mask         | RWX                  | 0x0000 | General error mask for fir:                                                                                                                                                                                                                                                                      |
|                |     |                    |                      |        | bit 0: counter configuration register parity error bit 1: clock configuration register parity error bit 2: sequencer configuration register parity error                                                                                                                                         |
|                |     |                    |                      |        | bit 3: sequencer fsm error bit 4: shifter fsm error bit 5: pattern match reg parity error bit 6: transmit data reg parity error bit 7: receive data reg parity error bit 8: configuration register 1 parity error bit 9: sequencer fsm error mask bit 10: shifter fsm error mask bit 11: bit 12: |
|                |     |                    |                      |        | bit 13 to 15: unused                                                                                                                                                                                                                                                                             |
| 32 to 63       | 32  | error_unused       | RWX                  | 0x0000 | Bit 0 to 31: unused                                                                                                                                                                                                                                                                              |



