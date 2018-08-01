# Generating Transaction\_blobs by Yourself

Generating transaction\_blobs by yourself \(take Java as an example\) includes the following steps:

1.Obtain the nonce value of the account that is to initiate a transaction by GET.

```
    GET http://seed1.bumotest.io:26002/getAccount?address=buQsurH1M4rjLkfjzkxR9KXJ6jSu2r9xBNEw
```

Response message:

```
    {
            "error_code" : 0,
            "result" : {
                  "address" : "buQsurH1M4rjLkfjzkxR9KXJ6jSu2r9xBNEw",
                  "assets" : [
                  {
                       "amount" : 1000000000,
                       "key" : {
                           "code" : "HNC",
                           "issuer" : "buQBjJD1BSJ7nzAbzdTenAhpFjmxRVEEtmxH"
                       }
                  }
                  ],
                  "assets_hash" : "3bf279af496877a51303e91c36d42d64ba9d414de8c038719b842e6421a9dae0",
                  "balance" : 27034700,
                  "metadatas" : null,
                  "metadatas_hash" : "ad67d57ae19de8068dbcd47282146bd553fe9f684c57c8c114453863ee41abc3",
                  "nonce" : 5,
                  "priv" : {
                        "master_weight" : 1,
                        "thresholds" : [{
                              "tx_threshold" : 1
                        }
                        ]
                  }
           }
    }
    address: Current query account address
    assets: Account asset list
    assets_hash: Asset list hash
    balance: Account balance
    metadata: Account metadata in hexadecimal format
    metadatas_hash: Transaction metadata hash
    nonce: The sending transaction serial number, the nonce+1 returned by querying the account information interface

    priv: Privilege
    master_weight: Current account weight
    thresholds: Threshold
    tx_threshold: Transaction default threshold
```

2.Populate the transaction data structure and generate a transaction\_blob.

> Import package: import io.bumo.sdk.core.extend.protobuf.Chain;

```
    Chain.Transaction.Builder builder = Chain.Transaction.newBuilder();
    builder.setSourceAddress("buQsurH1M4rjLkfjzkxR9KXJ6jSu2r9xBNEw");
    builder.setNonce(7);

    builder.setFeeLimit(1000 * 1000);
    builder.setGasPrice(1000);
    builder.setCeilLedgerSeq(0);
    builder.setMetadata(ByteString.copyFromUtf8(""));

    Chain.Operation.Builder operation = builder.addOperationsBuilder();
    operation.setType(Chain.Operation.Type.CREATE_ACCOUNT);

    Chain.OperationCreateAccount.Builder operationCreateAccount = Chain.OperationCreateAccount.newBuilder();
    operationCreateAccount.setDestAddress("buQoP2eRymAcUm3uvWgQ8RnjtrSnXBXfAzsV");
    operationCreateAccount.setInitBalance(10000000);

    Chain.AccountPrivilege.Builder accountPrivilegeBuilder = Chain.AccountPrivilege.newBuilder();
    accountPrivilegeBuilder.setMasterWeight(1);

    Chain.AccountThreshold.Builder accountThresholdBuilder = Chain.AccountThreshold.newBuilder();
    accountThresholdBuilder.setTxThreshold(1);

    accountPrivilegeBuilder.setThresholds(accountThresholdBuilder);
    operationCreateAccount.setPriv(accountPrivilegeBuilder);
    operation.setCreateAccount(operationCreateAccount);
    String transaction_blob = HexFormat.byteToHex(builder.build().toByteArray());

    The transaction_blob obtained:
    0a2462755173757248314d34726a4c6b666a7a6b7852394b584a366a537532723978424e4577100718c0843d20e8073a37080122330a246275516f50326552796d4163556d33757657675138526e6a7472536e58425866417a73561a0608011a0208012880ade204
```

> **Attention**：The nonce value is not 6, so this transaction would fail.

3.Sign the transaction\_blob with the private key.

> Import package: import io.bumo.encryption.key.PrivateKey;

```
The private key:
privbvTuL1k8z27i9eyBrFDUvAVVCSxKeLtzjMMZEqimFwbNchnejS81

The sign_data after being signed:
9C86CE621A1C9368E93F332C55FDF423C087631B51E95381B80F81044714E3CE3DCF5E4634E5BE77B12ABD3C54554E834A30643ADA80D19A4A3C924D0B3FA601
```

4.Complete populating the transaction data.

```
    {
        "items" : [{
            "transaction_blob" : "0a2462755173757248314d34726a4c6b666a7a6b7852394b584a366a537532723978424e4577100718c0843d20e8073a37080122330a246275516f50326552796d4163556d33757657675138526e6a7472536e58425866417a73561a0608011a0208012880ade204",                        
            "signatures" : [{
                "sign_data" : "9C86CE621A1C9368E93F332C55FDF423C087631B51E95381B80F81044714E3CE3DCF5E4634E5BE77B12ABD3C54554E834A30643ADA80D19A4A3C924D0B3FA601",
                "public_key" : "b00179b4adb1d3188aa1b98d6977a837bd4afdbb4813ac65472074fe3a491979bf256ba63895"
             }
             ]
        }
        ]
    }
```

5.Submit the transaction by POST.

```
     POST http://seed1.bumotest.io/submitTransaction
```

Response message:

```
    {
        "results": [{
            "error_code": 0,
            "error_desc": "",
            "hash": "be4953bce94ecd5c5a19c7c4445d940c6a55fb56370f7f606e127776053b3b51"
        }
        ],
        "success_count": 1
    }
```

> **Note**: “success\_count”:1 represents that the submission succeeded.



