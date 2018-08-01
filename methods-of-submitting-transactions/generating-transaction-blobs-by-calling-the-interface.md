# Generating Transaction\_blobs by Calling the Interface

> **Attention**: As the transaction\_blob is likely to be intercepted and tampered with, it is not recommended to generate transaction\_blobs in this way.

If you need to call the interface to generate transaction\_blobs, sign and submit transactions, please refer to the BUMO development documentation at the following address:

[https://github.com/bumoproject/bumo/blob/master/docs/develop.md](https://github.com/bumoproject/bumo/blob/master/docs/develop.md)

Calling the interface to generate a transaction\_blob includes the following steps:

1.Call the getAccount interface to get the nonce value of the account that is to initiate a transaction. The code is shown below:

```
    HTTP GET host:port/getAccount?address=account address
```

2.Populate the json data as needed and complete filling the transaction data. The format is shown below:

```
    {
        "source_address":"xxxxxxxxxxx", //The source transaction account, the originator of the transaction
        "nonce":2, //Nonce value
        "ceil_ledger_seq": 0, //Optional
        "fee_limit":1000, //Fee paid in transaction
        "gas_price": 1000, //Gas price (Not less than the configured value)
        "metadata":"0123456789abcdef", //Optional, metadata for the transaction given by users, in hexadecimal format
        "operations":[
        {
            //Populate according to specific operations
        },
        {
            //Populate according to specific operations
        }
            ......
        ]
    }
```

> **Attention**: The nonce value needs to be incremented by 1 based on the value obtained in Step 1.

3.By calling the getTransactionBlob interface, the json data generated in Step 2 is passed as a parameter, and a transaction hash and a transaction\_blob are obtained to implement transaction serialization. The format is shown below:

```
    {
        "error_code": 0,
        "error_desc": "",
        "result": {
            "hash": "xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx", //Transaction hash
            "transaction_blob": "xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx" //The hexadecimal representation after the transaction is serialized
        }
    }
```

4.Sign the transaction and populate the transaction data. Sign the transaction\_blob according to the previously generated private key, and then populate the json data of the submitted transaction. The format is shown below:

```
    {
        "items" : [{
            "transaction_blob" : "xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx", //The hexadecimal representation after the transaction is serialized
            "signatures" : [{//The first signature
                "sign_data" : "xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx", //Signature data
                "public_key" : "xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx" //Public key
            }, {//The second signature
                "sign_data" : "xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx", //Signature data
                "public_key" : "xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx" //Public key
            }
            ]
        }
        ]
    }
```

5.By calling the submitTransaction interface, the json data generated in Step 4 is passed as a parameter, the response result is obtained and transaction submission is completed. The format of the response result is shown below:

```
    {
        "results": [
        {
            "error_code": 0,
            "error_desc": "",
            "hash": "xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx" //Transaction hash
        }
        ],
        "success_count": 1
    }
```



