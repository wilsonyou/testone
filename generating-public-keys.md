## Generating Public Keys

The public key can be generated with the ED25519 algorithm after the private key is generated. Generating a public key includes the following steps:

1.Generate a 32-bit byte array \(raw public key\) by processing the raw private key with the ED25519 algorithm. For example, the raw public key of the private key \(_privbsGZFUoRv8aXZbSGd3bwzZWFn3L5QKq74RXAQYcmfXhhZ54CLr9z_\) is shown below:

_\[21,118,76,208,23,224,218,117,50,113,250,38,205,82,148,81,162,27,130,83,208,1,240,212,54,18,225,158,198,50,87,10\]_

2.Add a 1-byte prefix in the raw public key, and then add a 1-byte version number to get a new byte array, as shown below:

_\[176,1,21,118,76,208,23,224,218,117,50,113,250,38,205,82,148,81,162,27,130,83,208,1,240,212,54,18,225,158,198,50,87,10\]_

> **Note**：For the Prefix, Version and Checksum, please refer to Table 2.

3.Perform SHA256 calculation twice on the byte array in Step 2. Take the first 4 bytes of the operation result as the byte array of the Checksum, as shown below:

_\[116,171,22,107\]_

4.Combine the byte array in Step 2 and the checksum byte array in Step 3 in order, resulting in a new byte array, as shown below:

_\[176,1,21,118,76,208,23,224,218,117,50,113,250,38,205,82,148,81,162,27,130,83,208,1,240,212,54,18,225,158,198,50,87,10,116,171,22,107\]_

5.Encode the byte array in Step 4 into hexadecimal and get a hexadecimal string, namely the public key, as shown below:

_b00115764cd017e0da753271fa26cd529451a21b8253d001f0d43612e19ec632570a74ab166b_

> **Note**：Now the public key is generated.

Table 2

| Name | Data | Length |
| --- | --- | --- |
| Prefix | 0xB0 | 1 Byte |
| Version | 0x01 | 1 Byte |
| Checksum | After performing SHA256 calculation twice on the byte array obtained in Step 2, take the first 4 bytes of the operation result | 4 Bytes |

This table illustrates the Prefix, Version and Checksum used in generating the public key.

