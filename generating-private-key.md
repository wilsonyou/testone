# Generating Private Keys
Generating a private key requires multiple algorithms such as a random algorithm and SHA256. Generating a private key includes the following steps:

1.Generate a 256-bit random number (a private key in the mathematical sense) with a random algorithm and get a byte array, the raw private key, as shown below:

_[17,236,24,183,207,250,207,180,108,87,224,39,189,99,246,85,138,120,236,78,228,233,41,192,124,109,156,104,235,66,194,24]_

2.Add a 3-byte prefix in the raw private key, and then add a 1-byte version number to get a new byte array, as shown below:

_[218,55,159,1,17,236,24,183,207,250,207,180,108,87,224,39,189,99,246,85,138,120,236,78,228,233,41,192,124,109,156,104,235,66,194,24]_

>**Note**: For the Prefix, Version and Checksum, please refer to Table 1。

3.Perform SHA256 calculations twice on the byte array obtained in Step 2. Take the first 4 bytes of the operation result as the byte array of the Checksum, as shown below:

_[30,19,80,117]_

4.Combine the byte array in Step 2 and the checksum byte array in Step 3 in order, resulting in a new byte array, as shown below:

_[218,55,159,1,17,236,24,183,207,250,207,180,108,87,224,39,189,99,246,85,138,120,236,78,228,233,41,192,124,109,156,104,235,66,194,24,30,19,80,117]_

5.Encode the byte array generated in Step 4 with Base58, and get the string starting with priv, namely the private key, as shown below:

_privbsGZFUoRv8aXZbSGd3bwzZWFn3L5QKq74RXAQYcmfXhhZ54CLr9z_

>**Note**：Now the private key is generated.

Table 1

|   Name     |                Data                   | Length  |
|----------- | ----------------------------------------- | ----- |
|   Prefix   | 0xDA 0x37 0x9F                            | 3 bytes |
|   Version      | 0x01                                      | 1 byte |
|   Checksum | After performing SHA256 calculation twice on the byte array obtained in Step 2, take the first 4 bytes of the operation result | 4 bytes |
This table illustrates the Prefix, Version and Checksum used in generating the private key.