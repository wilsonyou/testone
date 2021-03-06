# Generating Addresses

The address can be further generated by an algorithm after generating the private key and the public key. Generating an address includes the following steps:

1.Generate a 32-bit byte array \(raw public key\) by processing the raw private key with the ED25519 algorithm. For example, the raw public key of the private key \(_privbsGZFUoRv8aXZbSGd3bwzZWFn3L5QKq74RXAQYcmfXhhZ54CLr9z_\) is shown below:

_\[21,118,76,208,23,224,218,117,50,113,250,38,205,82,148,81,162,27,130,83,208,1,240,212,54,18,225,158,198,50,87,10\]_

2.Perform SHA256 calculation twice on the raw public key and take the last 20 bytes of the operation result as the byte array, as shown below:

_\[173,148,59,51,183,193,55,160,1,133,247,80,65,13,67,190,164,114,18,220\]_

3.Add a 2-byte prefix in the byte array generated in Step 2, and then add a 1-byte version number to get a new byte array, as shown below:

_\[1,86,1,173,148,59,51,183,193,55,160,1,133,247,80,65,13,67,190,164,114,18,220\]_

> **Note**：For the Prefix, Version and Checksum, please refer to Table 3。

4.Perform SHA256 calculation twice on the byte array in Step 3. Take the first 4 bytes of the operation result as the byte array of the Checksum, as shown below:

_\[167,127,34,35\]_

5.Combine the byte array in Step 3 and the Checksum byte array in Step 4 in order, resulting in a new byte array, as shown below:

_\[1,86,1,173,148,59,51,183,193,55,160,1,133,247,80,65,13,67,190,164,114,18,220,167,127,34,35\]_

6.Encode the byte array generated in Step 5 with Base58, and get the string starting with bu, namely the address, as shown below:

_buQmWJrdYJP5CPKTbkQUqscwvTGaU44dord8_

> **Note**: Now the address is generated.

Table 3

| Name | Data | Length |
| --- | --- | --- |
| Prefix | 0x01 0x56 | 2 Bytes |
| Version | 0x01 | 1 Byte |
| PublicKey | Take the last 20 bytes in raw public key | 20 Bytes |
| Checksum | After performing SHA256 calculation twice on the byte array obtained in step 3, take the first 4 bytes of the operation result | 4 Bytes |

This table illustrates the Prefix, Version and Checksum used in generating the address.

