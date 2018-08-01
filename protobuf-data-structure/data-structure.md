# Data Structure

The following section describes the various ProtoBuf data structures that might be used in transactions and their uses for your reference.

1.Transaction

This data structure is for complete transactions.

```
    message Transaction {
        enum Limit{
            UNKNOWN = 0;
            OPERATIONS = 1000;
        };
        string source_address = 1;                 // Account address of the transaction initiator
        int64 nonce = 2;                           // Transaction sequence number
        int64  fee_limit = 3;                      // The transaction fee, by default is 1000Gas; the unit is MO, 1 BU = 10^8 MO
        int64  gas_price = 4;                       // The packaging fee of transactions, by default is 1000; the unit is MO,1 BU = 10^8 MO
        int64 ceil_ledger_seq = 5;                 // Block bound 
        bytes metadata = 6;                        // Transaction metadata
        repeated Operation operations = 7;         // Operation list
    }
```

2.Operation

This data structure is for operations in transactions.

```
    message Operation {
        enum Type {
            UNKNOWN = 0;
            CREATE_ACCOUNT = 1;
            ISSUE_ASSET = 2;
            PAY_ASSE = 3;
            SET_METADATA = 4;
            SET_SIGNER_WEIGHT = 5;
            SET_THRESHOLD = 6;
            PAY_COIN = 7;
            LOG = 8;
            SET_PRIVILEGE = 9;
        };
        Type type = 1;                             // Operation type 
        string source_address = 2;                 // Source account address for the operation
        bytes metadata = 3;                        // Operation metadata

        OperationCreateAccount create_account = 4;            // Create an account operation
        OperationIssueAsset issue_asset = 5;                  // Issue assets operation
        OperationPayAsset pay_asset = 6;                      // Transfer assets operation
        OperationSetMetadata set_metadata = 7;                // Set metadata
        OperationSetSignerWeight set_signer_weight = 8;       // Set privilege for signer
        OperationSetThreshold     set_threshold = 9;           // Set transaction threshold
        OperationPayCoin pay_coin = 10;                       // Transfer coin
        OperationLog log = 11;                                // Record log
        OperationSetPrivilege set_privilege = 12;             // Set privilege
     }
```

3.OperationCreateAccount

This data structure is for creating accounts.

```
    message OperationCreateAccount{
        string dest_address = 1;                  // Target account address to be created
        Contract contract = 2;                    // Contract
        AccountPrivilege priv = 3;                // Privilege
        repeated KeyPair metadatas = 4;           // Additional info
        int64    init_balance = 5;                 // Initiation balance
        string  init_input = 6;                   // Input parameter for contracts
    }
```

4.Contract

This data structure is for setting contracts.

```
    message Contract{
        enum ContractType{
            JAVASCRIPT = 0;
        }
        ContractType type = 1;                   // Contract type
        string payload = 2;                      // Contract code
    }
```

5.AccountPrivilege

This data structure is for setting account privilege.

```
    message AccountPrivilege {
        int64 master_weight = 1;                 // Account weight
        repeated Signer signers = 2;             // Signer weight list
        AccountThreshold thresholds = 3;         // Threshold
    }
```

6.Signer

This data structure is for setting signer weight.

```
    message Signer {
        enum Limit{
            SIGNER_NONE = 0;
            SIGNER = 100;
        };
        string address = 1;                      // Signer account address
        int64 weight = 2;                        // Signer weight
    }
```

7.AccountThreshold

This data structure is for setting account threshold.

```
    message AccountThreshold{
        int64 tx_threshold = 1;                                 // Transaction threshold
        repeated OperationTypeThreshold type_thresholds = 2;    // Specify the transaction threshold list for the operations. The threshold for the transactions with unspecified operation is set by tx_threshold
    }
```

8.OperationTypeThreshold

This data structure is for operation threshold of specified types.

```
    message OperationTypeThreshold{
        Operation.Type type = 1;                 // Operation type
        int64 threshold = 2;                     // Corresponding threshold of this operation
    }
```

9.OperationIssueAsset

This data structure is for issuing assets.

```
    message OperationIssueAsset{
        string code = 1;                         // Asset encoding to be issued
        int64 amount = 2;                        // Asset amount to be issued
    }
```

10.OperationPayAsset

This data structure is for transferring assets.

```
    message OperationPayAsset {
        string dest_address = 1;                 // Target account address
        Asset asset = 2;                         // Asset
        string input = 3;                        // Input parameter for contracts
    }
```

11.Asset

This data structure is for asset.

```
    message Asset{
        AssetKey    key = 1;                      // Asset identification
        int64        amount = 2;                   // Asset amount
    }
```

12.AssetKey

This data structure is for identifying the uniqueness of asset.

```
    message AssetKey{
        string issuer = 1;                       //  Account address of asset issuer
        string code = 2;                         //  Asset encoding
        int32 type = 3;                          //  Asset type(by default is 0, which indicates the amount is not limited)
    }
```

13.OperationSetMetadata

This data structure is for setting Metadata.

```
    message OperationSetMetadata{
        string    key = 1;                         // keyword, unique
        string  value = 2;                       // Content
        int64     version = 3;                     // Version control, optional
        bool    delete_flag = 4;                 // Whether it is deletable
    }
```

14.OperationSetSignerWeight

This data structure is for setting signer weight.

```
    message OperationSetSignerWeight{
        int64 master_weight = 1;                 // Self weight
        repeated Signer signers = 2;             // Signer weight list
    }
```

15.OperationSetThreshold

This data structure is for setting threshold.

```
    message OperationSetThreshold{
        int64 tx_threshold = 1;                               // Transaction threshold
        repeated OperationTypeThreshold type_thresholds = 2;  // The transaction threshold list for specified operations. The threshold for the transactions with unspecified operation is set by tx_threshold

    }
```

16.OperationPayCoin

This data structure is for sending coin.

```
    message OperationPayCoin{
        string dest_address = 1;                 // Target account address
        int64 amount = 2;                        // Coin amount
        string input = 3;                        // Input parameter for contracts
    }
```

17.OperationLog

This data structure is for recording log information.

```
    message OperationLog{
        string topic = 1;                        // Log theme
        repeated string datas = 2;               // Log content
    }
```

18.OperationSetPrivilege

This data structure is for setting account privilege.

```
    message OperationSetPrivilege{
        string master_weight = 1;                            // Account weight
        repeated Signer signers = 2;                         // Signer weight list
        string tx_threshold = 3;                             // Transaction threshold
        repeated OperationTypeThreshold type_thresholds = 4; // The transaction threshold list for specified operations. The threshold for the transactions with unspecified operation is set by tx_threshold


    }
```



