# Tron Protobuf protocol
## The protocol of TRON is defined by Google Protobuf and contains a range of layers, from account, block to transfer.
    
 + There are 3 types of account—basic account, asset release account and contract account, and attributes included in each account are name, types, address, balance and related asset.
 + A basic account is able to apply to be a validation node, which has serval parameters, including extra attributes, public key, URL, voting statistics, history performance, etc.
    
      There are three different `Account types`: `Normal`, `AssetIssue`, `Contract`.
    
          enum AccountType {   
             Normal = 0;   
             AssetIssue = 1;   
             Contract = 2;
            }
    
      An `Account` contains 6 parameters:  
         `account_name`: the name for this account – e.g. “_BillsAccount_”.  
         `type`: what type of this account is – e.g. _0_ stands for type `Normal`.  
         `balance`: balance of this account – e.g. _4213312_.  
         `votes`: received votes on this account – e.g. _{(“0x1b7w…9xj3”,323), (“0x8djq…j12m”,88),…,(“0x82nd…mx6i”,10001)}_.  
         `asset`: other assets expect TRX in this account – e.g. _{<“WishToken”,66666>,<”Dogie”,233>}_.
          
          // Account 
          message Account {   
            message Vote {     
               bytes vote_address = 1;     
               int64 vote_count = 2;   }   
            bytes accout_name = 1;   
            AccountType type = 2;   
            bytes address = 3;   
            int64 balance = 4;   
            repeated Vote votes = 5;   
            map<string, int64> asset = 6; 
           }
    
      A `Witness` contains 8 parameters:  
         `address`: the address of this witness – e.g. “_0xu82h…7237_”.  
         `voteCount`: number of received votes on this witness – e.g. _234234_.  
         `pubKey`: the public key for this witness – e.g. “_0xu82h…7237_”.  
         `url`: the url for this witness – e.g. “_https://www.noonetrust.com_”.  
         `totalProduced`: the number of blocks this witness produced – e.g. _2434_.  
         `totalMissed`: the number of blocks this witness missed – e.g. _7_.  
         `latestBlockNum`: the latest height of block – e.g. _4522_.
    
          // Witness 
          message Witness{   
            bytes address = 1;   
            int64 voteCount = 2;   
            bytes pubKey = 3;   
            string url = 4;   
            int64 totalProduced = 5;   
            int64 totalMissed = 6;   
            int64 latestBlockNum = 7; 
           }
    
 +	A block typically contains transaction data and a blockheader, which is a list of basic block information, including timestamp, signature, parent hash, root of Merkle tree and so on.
    
         A block contains `transactions` and a `block_header`.  
         `transactions`: transaction data of this block.   
         `block_header`: one part of a block.
          
             // block
              message Block {   
               repeated Transaction transactions = 1;   
               BlockHeader block_header = 2; 
              }
    
         A `BlockHeader` contains `raw_data` and `witness_signature`.  
         `raw_data`: a `raw` message.  
         `witness_signature`: signature for this block header from witness node.
    
         A message `raw` contains 6 parameters:  
         `timestamp`: timestamp of this message – e.g. _14356325_.  
         `txTrieRoot`: the root of Merkle Tree in this block – e.g. “_7dacsa…3ed_.”  
         `parentHash`: the hash of last block – e.g. “_7dacsa…3ed_.”  
         `number`: the height of this block – e.g. _13534657_.  
         `witness_id`: the id of witness which packed this block – e.g. “_0xu82h…7237_”.  
         `witness_address`: the adresss of the witness packed this block – e.g. “_0xu82h…7237_”.
    
             message BlockHeader {   
               message raw {     
                 int64 timestamp = 1;     
                 bytes txTrieRoot = 2;     
                 bytes parentHash = 3;     
                 //bytes nonce = 5;     
                 //bytes difficulty = 6;     
                 uint64 number = 7;     
                 uint64 witness_id = 8;     
                 bytes witness_address = 9;   
              }   
              raw raw_data = 1;   
              bytes witness_signature = 2; 
              }
    
 +	Transaction contracts mainly includes account creation contract, transfer contract, transfer asset contract, vote asset contract, vote witness contract, witness creation contract, asset issue contract and deploy contract.
    
         An `AccountCreateContract` contains 3 parameters:  
         `type`: What type this account is – e.g. _0_ stands for `Normal`.  
         `account_name`: the name for this account – e.g.”_Billsaccount_”.  
         `owner_address`: the address of contract owner – e.g. “_0xu82h…7237_”.
             
             message AccountCreateContract {   
               AccountType type = 1;   
               bytes account_name = 2;   
               bytes owner_address = 3; 
              }
    
         A `TransferContract` contains 3 parameters:  
         `amount`: the amount of TRX – e.g. _12534_.  
         `to_address`: the receiver address – e.g. “_0xu82h…7237_”.  
         `owner_address`: the address of contract owner – e.g. “_0xu82h…7237_”.
    
             message TransferContract {   
               bytes owner_address = 1;   
               bytes to_address = 2;   
               int64 amount = 3;
              }
    
         A `TransferAssetContract` contains 4 parameters:  
         `asset_name`: the name for asset – e.g.”_Billsaccount_”.  
         `to_address`: the receiver address – e.g. “_0xu82h…7237_”.  
         `owner_address`: the address of contract owner – e.g. “_0xu82h…7237_”.  
         `amount`: the amount of target asset - e.g._12353_.
    
             message TransferAssetContract {   
               bytes asset_name = 1;   
               bytes owner_address = 2;   
               bytes to_address = 3;   
               int64 amount = 4; 
              }
    
         A `VoteAssetContract` contains 4 parameters:  
         `vote_address`: the voted address of the asset.  
         `support`: is the votes supportive or not – e.g. _true_.  
         `owner_address`: the address of contract owner – e.g. “_0xu82h…7237_”.  
         `count`: the count number of votes- e.g. _2324234_.
    
             message VoteAssetContract {   
               bytes owner_address = 1;   
               repeated bytes vote_address = 2;   
               bool support = 3;   
               int32 count = 5; 
              }
    
         A `VoteWitnessContract` contains 4 parameters:  
         `vote_address`: the addresses of those who voted.  
         `support`: is the votes supportive or not - e.g. _true_.  
         `owner_address`: the address of contract owner – e.g. “_0xu82h…7237_”.  
         `count`: - e.g. the count number of vote – e.g. _32632_.
    
             message VoteWitnessContract {   
               bytes owner_address = 1;   
               repeated bytes vote_address = 2;   
               bool support = 3;   
               int32 count = 5;
               }
    
         A `WitnessCreateContract` contains 3 parameters:  
         `private_key`: the private key of contract– e.g. “_0xu82h…7237_”.  
         `owner_address`: the address of contract owner – e.g. “_0xu82h…7237_”.  
         `url`: the url for the witness – e.g. “_https://www.noonetrust.com_”.
    
             message WitnessCreateContract {   
               bytes owner_address = 1;   
               bytes private_key = 2;   
               bytes url = 12; 
              }
    
         An `AssetIssueContract` contains 11 parameters:  
         `owner_address`: the address for contract owner – e.g. “_0xu82h…7237_”.  
         `name`: the name for this contract – e.g. “Billscontract”.  
         `total_supply`: the maximum supply of this asset – e.g. _1000000000_.  
         `trx_num`: the number of TRONIX – e.g._232241_.  
         `num`: number of corresponding asset.  
         `start_time`: the starting date of this contract – e.g._20170312_.  
         `end_time`: the expiring date of this contract – e.g. _20170512_.  
         `decay_ratio`: decay ratio.  
         `vote_score`: the vote score of this contract received – e.g. _12343_.  
         `description`: the description of this contract – e.g.”_trondada_”.  
         `url`: the url of this contract – e.g. “_https://www.noonetrust.com_”.
    
             message AssetIssueContract {   
               bytes owner_address = 1;   
               bytes name = 2;   
               int64 total_supply = 4;   
               int32 trx_num = 6;   
               int32 num = 8;   
               int64 start_time = 9;   
               int64 end_time = 10;   
               int32 decay_ratio = 15;   
               int32 vote_score = 16;   
               bytes description = 20;   
               bytes url = 21; 
              }
    
         A `DeployContrac` contains 2 parameters:  
         `script`: the script of this contract.  
         `owner_address`: the address of contract owner – e.g. “_0xu82h…7237_”.
    
             message DeployContract {   
               bytes owner_address = 1;   
               bytes script = 2;
               }
    
 +	Each transaction contains several TXInputs, TXOutputs and other related qualities.
    Input, transaction and head block all require signature.
    
         message `Transaction` contains `raw_data` and `signature`.  
         `raw_data`: message `raw`.  
         `signature`: signatures form all input nodes.
    
      `raw` contains 7 parameters:  
        `type`: the transaction type of `raw` message.  
        `vin`: input values.  
        `vout`: output values.  
        `expiration`: the expiration date of transaction – e.g._20170312_.  
        `data`: data.  
        `contract`: contracts in this transaction.  
        `scripts`:scripts in the transaction.
    
        message `Contract` contains `type` and `parameter`.  
        `type`: what type of the message contract.  
        `parameter`: It can be any form.
    
        There are 8 different of contract types: `AccountCreateContract`, `TransferContract`, `TransferAssetContract`, `VoteAssetContract`, `VoteWitnessContract`,`WitnessCreateContract`, `AssetIssueContract` and `DeployContract`.  
        `TransactionType` have two types: `UtxoType` and `ContractType`.
    
          message Transaction {   
            enum TranscationType {     
              UtxoType = 0;     
              ContractType = 1;   
             }   
             message Contract {     
               enum ContractType {       
                 AccountCreateContract = 0;       
                 TransferContract = 1;       
                 TransferAssetContract = 2;       
                 VoteAssetContract = 3;       
                 VoteWitnessContract = 4;       
                 WitnessCreateContract = 5;       
                 AssetIssueContract = 6;       
                 DeployContract = 7;     
                }     
                ContractType type = 1;     
                google.protobuf.Any parameter = 2;   
              }   
              message raw {     
                TranscationType type = 2;     
                repeated TXInput vin = 5;     
                repeated TXOutput vout = 7;     
                int64 expiration = 8;     
                bytes data = 10;     
                repeated Contract contract = 11;     
                bytes scripts = 16;   
               }   
               raw raw_data = 1;   
               repeated bytes signature = 5;
            }
    
        message `TXOutputs` contains `outputs`.  
        `outputs`: an array of `TXOutput`.  
    
          message TXOutputs {   
            repeated TXOutput outputs = 1; 
           }
    
        message `TXOutput` contains `value` and `pubKeyHash`.  
        `value`: output value.  
        `pubKeyHash`: Hash of public key
    
          message TXOutput {   
            int64 value = 1;   
            bytes pubKeyHash = 2; 
           }
    
        message `TXInput` contains `raw_data` and `signature`.  
        `raw_data`: a message `raw`.  
        `signature`: signature for this `TXInput`.
    
        message `raw` contains `txID`, `vout` and `pubKey`.  
        `txID`: transaction ID.  
        `vout`: value of last output.  
        `pubKey`: public key.
    
          message TXInput {   
            message raw {     
              bytes txID = 1;     
              int64 vout = 2;     
              bytes pubKey = 3;   
             }   
             raw raw_data = 1;   
             bytes signature = 4;
            }
    
 +	Inventory is mainly used to inform peer nodes the list of items.  
    
        `Inventory` contains `type` and `ids`.  
        `type`: what type this `Inventory` is. – e.g. _0_ stands for `TRX`.  
        `ids`: ID of things in this `Inventory`.
    
        Two `Inventory` types: `TRX` and `BLOCK`.  
        `TRX`: transaction.  
        `BLOCK`: block.
    
          // Inventory 
          message Inventory {   
            enum InventoryType {     
              TRX = 0;     
              BLOCK = 1;   
             }   
             InventoryType type = 1;   
             repeated bytes ids = 2; 
           }
    
        message `Items` contains 4 parameters:  
        `type`: type of items – e.g. _1_ stands for `TRX`.  
        `blocks`: blocks in `Items` if there is any.  
        `block_headers`: block headers if there is any.  
        `transactions`: transactions if there is any.
    
        `Items` have four types: `ERR`, `TRX`, `BLOCK` and `BLOCKHEADER`.  
        `ERR`: error.  
        `TRX`: transaction.  
        `BLOCK`: block.  
        `BLOCKHEADER`: block header.
    
          message Items {   
            enum ItemType {     
              ERR = 0;     
              TRX = 1;    
              BLOCK = 2;     
              BLOCKHEADER = 3;  
             }   
             ItemType type = 1;   
             repeated Block blocks = 2;   
             repeated BlockHeader 
             block_headers = 3;   
             repeated Transaction transactions = 4;
           }
    
        `InventoryItems` contains `type` and `items`.  
        `type`: what type of item.  
        `items`: items in an `InventoryItems`.
    
          message InventoryItems {   
            int32 type = 1;   
            repeated bytes items = 2;
            }
    
 +	Wallet Service RPC
    
        `Wallet` service contains several RPCs.  
        __`GetBalance`__ :  
        Return balance of an `Account`.  
        __`CreateTransaction`__ ：  
        Create a transaction by giving a `TransferContract`. A Transaction containing a transaction creation will be returned.  
        __`BroadcastTransaction`__ :  
        Broadcast a `Transaction`. A `Return` will be returned indicating if broadcast is success of not.  
        __`CreateAccount`__ :  
        Create an account by giving a `AccountCreateContract`.  
        __`CreatAssetContract`__ :  
        Issue an asset by giving a `AssetIssueContract`.
    
          service Wallet {    
          
            rpc GetBalance (Account) returns (Account) {    
            
            };   
            rpc CreateTransaction (TransferContract) returns 
           (Transaction) {    
           
             };    
             
             rpc BroadcastTransaction (Transaction) returns (Return) {   
              
             };    
             
             rpc CreateAccount(AccountCreateContract) returns 
           (Transaction) {    
           
             };    
             
             rpc CreateAssetIssue(AssetIssueContract) returns 
           (Transaction) {    
             
              };  
              
           };
    
        message `Return` has only one parameter:  
        `result`: a bool flag.  
        message `Return` {   bool result = 1; }
    
    
    # Please check detailed protocol document that may change with the iteration of the program at any time. Please refer to the latest version.
    
ON Protobuf protocol

## The protocol of TRON is defined by Google Protobuf and contains a range of layers, from account, block to transfer.

+ There are 3 types of account—basic account, asset release account and contract account, and attributes included in each account are name, types, address, balance and related asset.
+ A basic account is able to apply to be a validation node, which has serval parameters, including extra attributes, public key, URL, voting statistics, history performance, etc.

     There are three different `Account types`: `Normal`, `AssetIssue`, `Contract`.

      enum AccountType {   
         Normal = 0;   
         AssetIssue = 1;   
         Contract = 2;
        }

     An `Account` contains 6 parameters:  
     `account_name`: the name for this account – e.g. “_BillsAccount_”.  
     `type`: what type of this account is – e.g. _0_ stands for type `Normal`.  
     `balance`: balance of this account – e.g. _4213312_.  
     `votes`: received votes on this account – e.g. _{(“0x1b7w…9xj3”,323), (“0x8djq…j12m”,88),…,(“0x82nd…mx6i”,10001)}_.  
     `asset`: other assets expect TRX in this account – e.g. _{<“WishToken”,66666>,<”Dogie”,233>}_.
      
      // Account 
      message Account {   
        message Vote {     
           bytes vote_address = 1;     
           int64 vote_count = 2;   }   
        bytes accout_name = 1;   
        AccountType type = 2;   
        bytes address = 3;   
        int64 balance = 4;   
        repeated Vote votes = 5;   
        map<string, int64> asset = 6; 
       }

     A `Witness` contains 8 parameters:  
     `address`: the address of this witness – e.g. “_0xu82h…7237_”.  
     `voteCount`: number of received votes on this witness – e.g. _234234_.  
     `pubKey`: the public key for this witness – e.g. “_0xu82h…7237_”.  
     `url`: the url for this witness – e.g. “_https://www.noonetrust.com_”.  
     `totalProduced`: the number of blocks this witness produced – e.g. _2434_.  
     `totalMissed`: the number of blocks this witness missed – e.g. _7_.  
     `latestBlockNum`: the latest height of block – e.g. _4522_.

      // Witness 
      message Witness{   
        bytes address = 1;   
        int64 voteCount = 2;   
        bytes pubKey = 3;   
        string url = 4;   
        int64 totalProduced = 5;   
        int64 totalMissed = 6;   
        int64 latestBlockNum = 7; 
       }

+	A block typically contains transaction data and a blockheader, which is a list of basic block information, including timestamp, signature, parent hash, root of Merkle tree and so on.

     A block contains `transactions` and a `block_header`.  
     `transactions`: transaction data of this block.   
     `block_header`: one part of a block.
      
         // block
          message Block {   
           repeated Transaction transactions = 1;   
           BlockHeader block_header = 2; 
          }

     A `BlockHeader` contains `raw_data` and `witness_signature`.  
     `raw_data`: a `raw` message.  
     `witness_signature`: signature for this block header from witness node.

     A message `raw` contains 6 parameters:  
     `timestamp`: timestamp of this message – e.g. _14356325_.  
     `txTrieRoot`: the root of Merkle Tree in this block – e.g. “_7dacsa…3ed_.”  
     `parentHash`: the hash of last block – e.g. “_7dacsa…3ed_.”  
     `number`: the height of this block – e.g. _13534657_.  
     `witness_id`: the id of witness which packed this block – e.g. “_0xu82h…7237_”.  
     `witness_address`: the adresss of the witness packed this block – e.g. “_0xu82h…7237_”.

         message BlockHeader {   
           message raw {     
             int64 timestamp = 1;     
             bytes txTrieRoot = 2;     
             bytes parentHash = 3;     
             //bytes nonce = 5;     
             //bytes difficulty = 6;     
             uint64 number = 7;     
             uint64 witness_id = 8;     
             bytes witness_address = 9;   
          }   
          raw raw_data = 1;   
          bytes witness_signature = 2; 
          }

+	Transaction contracts mainly includes account creation contract, transfer contract, transfer asset contract, vote asset contract, vote witness contract, witness creation contract, asset issue contract and deploy contract.

     An `AccountCreateContract` contains 3 parameters:  
     `type`: What type this account is – e.g. _0_ stands for `Normal`.  
     `account_name`: the name for this account – e.g.”_Billsaccount_”.  
     `owner_address`: the address of contract owner – e.g. “_0xu82h…7237_”.
         
         message AccountCreateContract {   
           AccountType type = 1;   
           bytes account_name = 2;   
           bytes owner_address = 3; 
          }

     A `TransferContract` contains 3 parameters:  
     `amount`: the amount of TRX – e.g. _12534_.  
     `to_address`: the receiver address – e.g. “_0xu82h…7237_”.  
     `owner_address`: the address of contract owner – e.g. “_0xu82h…7237_”.

         message TransferContract {   
           bytes owner_address = 1;   
           bytes to_address = 2;   
           int64 amount = 3;
          }

     A `TransferAssetContract` contains 4 parameters:  
     `asset_name`: the name for asset – e.g.”_Billsaccount_”.  
     `to_address`: the receiver address – e.g. “_0xu82h…7237_”.  
     `owner_address`: the address of contract owner – e.g. “_0xu82h…7237_”.  
     `amount`: the amount of target asset - e.g._12353_.

         message TransferAssetContract {   
           bytes asset_name = 1;   
           bytes owner_address = 2;   
           bytes to_address = 3;   
           int64 amount = 4; 
          }

     A `VoteAssetContract` contains 4 parameters:  
     `vote_address`: the voted address of the asset.  
     `support`: is the votes supportive or not – e.g. _true_.  
     `owner_address`: the address of contract owner – e.g. “_0xu82h…7237_”.  
     `count`: the count number of votes- e.g. _2324234_.

         message VoteAssetContract {   
           bytes owner_address = 1;   
           repeated bytes vote_address = 2;   
           bool support = 3;   
           int32 count = 5; 
          }

     A `VoteWitnessContract` contains 4 parameters:  
     `vote_address`: the addresses of those who voted.  
     `support`: is the votes supportive or not - e.g. _true_.  
     `owner_address`: the address of contract owner – e.g. “_0xu82h…7237_”.  
     `count`: - e.g. the count number of vote – e.g. _32632_.

         message VoteWitnessContract {   
           bytes owner_address = 1;   
           repeated bytes vote_address = 2;   
           bool support = 3;   
           int32 count = 5;
           }

     A `WitnessCreateContract` contains 3 parameters:  
     `private_key`: the private key of contract– e.g. “_0xu82h…7237_”.  
     `owner_address`: the address of contract owner – e.g. “_0xu82h…7237_”.  
     `url`: the url for the witness – e.g. “_https://www.noonetrust.com_”.

         message WitnessCreateContract {   
           bytes owner_address = 1;   
           bytes private_key = 2;   
           bytes url = 12; 
          }

     An `AssetIssueContract` contains 11 parameters:  
     `owner_address`: the address for contract owner – e.g. “_0xu82h…7237_”.  
     `name`: the name for this contract – e.g. “Billscontract”.  
     `total_supply`: the maximum supply of this asset – e.g. _1000000000_.  
     `trx_num`: the number of TRONIX – e.g._232241_.  
     `num`: number of corresponding asset.  
     `start_time`: the starting date of this contract – e.g._20170312_.  
     `end_time`: the expiring date of this contract – e.g. _20170512_.  
     `decay_ratio`: decay ratio.  
     `vote_score`: the vote score of this contract received – e.g. _12343_.  
     `description`: the description of this contract – e.g.”_trondada_”.  
     `url`: the url of this contract – e.g. “_https://www.noonetrust.com_”.

         message AssetIssueContract {   
           bytes owner_address = 1;   
           bytes name = 2;   
           int64 total_supply = 4;   
           int32 trx_num = 6;   
           int32 num = 8;   
           int64 start_time = 9;   
           int64 end_time = 10;   
           int32 decay_ratio = 15;   
           int32 vote_score = 16;   
           bytes description = 20;   
           bytes url = 21; 
          }

     A `DeployContrac` contains 2 parameters:  
     `script`: the script of this contract.  
     `owner_address`: the address of contract owner – e.g. “_0xu82h…7237_”.

         message DeployContract {   
           bytes owner_address = 1;   
           bytes script = 2;
           }

+	Each transaction contains several TXInputs, TXOutputs and other related qualities.
Input, transaction and head block all require signature.

     message `Transaction` contains `raw_data` and `signature`.  
     `raw_data`: message `raw`.  
     `signature`: signatures form all input nodes.

    `raw` contains 7 parameters:  
    `type`: the transaction type of `raw` message.  
    `vin`: input values.  
    `vout`: output values.  
    `expiration`: the expiration date of transaction – e.g._20170312_.  
    `data`: data.  
    `contract`: contracts in this transaction.  
    `scripts`:scripts in the transaction.

    message `Contract` contains `type` and `parameter`.  
    `type`: what type of the message contract.  
    `parameter`: It can be any form.

    There are 8 different of contract types: `AccountCreateContract`, `TransferContract`, `TransferAssetContract`, `VoteAssetContract`, `VoteWitnessContract`,`WitnessCreateContract`, `AssetIssueContract` and `DeployContract`.  
    `TransactionType` have two types: `UtxoType` and `ContractType`.

      message Transaction {   
        enum TranscationType {     
          UtxoType = 0;     
          ContractType = 1;   
         }   
         message Contract {     
           enum ContractType {       
             AccountCreateContract = 0;       
             TransferContract = 1;       
             TransferAssetContract = 2;       
             VoteAssetContract = 3;       
             VoteWitnessContract = 4;       
             WitnessCreateContract = 5;       
             AssetIssueContract = 6;       
             DeployContract = 7;     
            }     
            ContractType type = 1;     
            google.protobuf.Any parameter = 2;   
          }   
          message raw {     
            TranscationType type = 2;     
            repeated TXInput vin = 5;     
            repeated TXOutput vout = 7;     
            int64 expiration = 8;     
            bytes data = 10;     
            repeated Contract contract = 11;     
            bytes scripts = 16;   
           }   
           raw raw_data = 1;   
           repeated bytes signature = 5;
        }

    message `TXOutputs` contains `outputs`.  
    `outputs`: an array of `TXOutput`.  

      message TXOutputs {   
        repeated TXOutput outputs = 1; 
       }

    message `TXOutput` contains `value` and `pubKeyHash`.  
    `value`: output value.  
    `pubKeyHash`: Hash of public key

      message TXOutput {   
        int64 value = 1;   
        bytes pubKeyHash = 2; 
       }

    message `TXInput` contains `raw_data` and `signature`.  
    `raw_data`: a message `raw`.  
    `signature`: signature for this `TXInput`.

    message `raw` contains `txID`, `vout` and `pubKey`.  
    `txID`: transaction ID.  
    `vout`: value of last output.  
    `pubKey`: public key.

      message TXInput {   
        message raw {     
          bytes txID = 1;     
          int64 vout = 2;     
          bytes pubKey = 3;   
         }   
         raw raw_data = 1;   
         bytes signature = 4;
        }

+	Inventory is mainly used to inform peer nodes the list of items.  

    `Inventory` contains `type` and `ids`.  
    `type`: what type this `Inventory` is. – e.g. _0_ stands for `TRX`.  
    `ids`: ID of things in this `Inventory`.

    Two `Inventory` types: `TRX` and `BLOCK`.  
    `TRX`: transaction.  
    `BLOCK`: block.

      // Inventory 
      message Inventory {   
        enum InventoryType {     
          TRX = 0;     
          BLOCK = 1;   
         }   
         InventoryType type = 1;   
         repeated bytes ids = 2; 
       }

    message `Items` contains 4 parameters:  
    `type`: type of items – e.g. _1_ stands for `TRX`.  
    `blocks`: blocks in `Items` if there is any.  
    `block_headers`: block headers if there is any.  
    `transactions`: transactions if there is any.

    `Items` have four types: `ERR`, `TRX`, `BLOCK` and `BLOCKHEADER`.  
    `ERR`: error.  
    `TRX`: transaction.  
    `BLOCK`: block.  
    `BLOCKHEADER`: block header.

      message Items {   
        enum ItemType {     
          ERR = 0;     
          TRX = 1;    
          BLOCK = 2;     
          BLOCKHEADER = 3;  
         }   
         ItemType type = 1;   
         repeated Block blocks = 2;   
         repeated BlockHeader 
         block_headers = 3;   
         repeated Transaction transactions = 4;
       }

    `InventoryItems` contains `type` and `items`.  
    `type`: what type of item.  
    `items`: items in an `InventoryItems`.

      message InventoryItems {   
        int32 type = 1;   
        repeated bytes items = 2;
        }

+	Wallet Service RPC

    `Wallet` service contains several RPCs.  
    __`GetBalance`__ :  
    Return balance of an `Account`.  
    __`CreateTransaction`__ ：  
    Create a transaction by giving a `TransferContract`. A Transaction containing a transaction creation will be returned.  
    __`BroadcastTransaction`__ :  
    Broadcast a `Transaction`. A `Return` will be returned indicating if broadcast is success of not.  
    __`CreateAccount`__ :  
    Create an account by giving a `AccountCreateContract`.  
    __`CreatAssetContract`__ :  
    Issue an asset by giving a `AssetIssueContract`.

      service Wallet {    
      
        rpc GetBalance (Account) returns (Account) {    
        
        };   
        rpc CreateTransaction (TransferContract) returns 
       (Transaction) {    
       
         };    
         
         rpc BroadcastTransaction (Transaction) returns (Return) {   
          
         };    
         
         rpc CreateAccount(AccountCreateContract) returns 
       (Transaction) {    
       
         };    
         
         rpc CreateAssetIssue(AssetIssueContract) returns 
       (Transaction) {    
         
          };  
          
       };

    message `Return` has only one parameter:  
    `result`: a bool flag.  
    message `Return` {   bool result = 1; }


# Please check detailed protocol document that may change with the iteration of the program at any time. Please refer to the latest version.
