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


| FSI i2cc dress | Ad- | PIB i2cc Ad- | Read                          | Write                                  |
|----------------|-----|--------------|-------------------------------|----------------------------------------|
|                |     | dress        |                               |                                        |
| x’00’          |     | 0x04         | FIFO                          |                                        |
| x’01’          |     | 0x05         | Command Register              |                                        |
| x’02’          |     | 0x06         | Mode Register                 |                                        |
| x’03’          |     | 0x07         | Watermark Register            |                                        |
| x’04’          |     | 0x08         | Interrupt Mask Register       |                                        |
| x’05’          |     | 0x09         | Interrupt Conditions          | Write Set to Interrupt Mask Register   |
| x’06’          |     | 0x0A         | Interrupts                    | Write Clear to Interrupt Mask Register |
| x’07’          |     | 0x0B         | Status                        | Immediate Reset I2C                    |
| x’08’          |     | 0x0C         | Extended Status               | Immediate Reset Errors                 |
| x’09’          |     | 0x0D         | Residual Front End / Back End | Immediate Set S_SCL                    |
|                |     |              | Length                        |                                        |
| x’0A’          |     | 0x0E         | Port busy register            |                                        |
| x’0B’          |     | 0x0F         | Not used                      | Immediate Reset S_SCL                  |
| x’0C’          |     | 0x10         | Not used                      | Immediate Set S_SDA                    |
| x’0D’          |     | 0x11         | Not used                      | Immediate Reset S_SDA                  |


Third Table

| Reset State | 0x00000000                                                                                                         |
|-------------|--------------------------------------------------------------------------------------------------------------------|
| Bit         | Description                                                                                                        |
| 0           | With Start                                                                                                         |
| 1           | With Address                                                                                                       |
| 2           | Read Continue                                                                                                      |
| 3           | With Stop                                                                                                          |
| 04-May      | Not used                                                                                                           |
| 06-Jul      | Legacy_mode_interrupt steering bits : This decides where the interrupt generated by the                            
              I2C engine needs to go . It supports up to 4 controllers. Means we can drive interrupt to 4 different controllers. |
| Aug-14      | Device Address                                                                                                     |
| 15          | Read / not Write                                                                                                   |
| 16 - 31     | Length in Bytes                                                                                                    |


Forth Table

| Command   | With Start | With |           |           |     |   |           |
|-----------|------------|------|-----------|-----------|-----|---|-----------|
| Stop      | With       | Ad-  |           |           |     |   |           |
| dress     | Address    | Read | /         |           |     |   |           |
| not Write | Read       |      |           |           |     |   |           |
| Continue  | Length     |      |           |           |     |   |           |
| First     | 1          | 0    | 1         | 7         | bit |   |           |
| Addr.     | 1          | 1    | 64k Bytes |           |     |   |           |
| Second    | 0          | 0    | 0         | dont care | 1   | 1 | 64k Bytes |
| Third     | 0          | 0    | 0         | dont care | 1   | 1 | 64k Bytes |
| Last      | 0          | 1    | 0         | dont care | 1   | 0 | 8k Bytes  |




