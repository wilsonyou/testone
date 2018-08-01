# Generating Transaction\_blobs by Yourself

Generating the transaction\_blob by yourself, signing, and submitting the transaction include the following steps:

1.Call the getAccount interface to get the nonce value of the account that is to initiate a transaction. The code is shown below:

```
          HTTP GET host:port/getAccount?address=account address
```

2.Populate the transaction object \(Transaction\) of the protocol buffer and serialize it to get the transaction\_blob. For details of the specific transaction data structure, please refer to [ProtoBuf Data Structure](/protobuf-data-structure.md).

3.Sign the transaction and populate the transaction data. Generate a public key based on the private key, sign the transaction\_blob with the private key, and then populate the json data of the submitted transaction. The format is shown below:

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

4.By calling the submitTransaction interface, the json data generated in Step 3 is passed as a parameter to complete the transaction submission. The response result format is shown below:

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



