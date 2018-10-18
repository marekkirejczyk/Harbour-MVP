# UniversalLoginSDK

**Project Name**: UniversalLoginSDK

**Links of project**:

- https://github.com/ethworks/universalLoginSDK/

**Short paragraph on where your team is at...**:

UniversalLoginSDK is an SDK composed of smart contracts, a js lib, and a relayer that help build applications using ERC1077 and ERC1078.

**Video DEMO Link:**

- https://www.youtube.com/watch?v=F5t94cCg6XE

## Code

#### Tx 
```js
/// @param to Destination address.
/// @param value Ether value.
/// @param data Data payload.
/// @param nonce Transaction nonce.
/// @param gasToken Token address (or 0 if ETH) that is used for the refund
/// @param gasPrice Gas price that should be used for this transaction.
/// @param gasLimit Maximum gas amount that should be used for this transaction.
/// @param operationType 0 - call, 1 - delegate call, 2 - create
/// @param extraHash - for future compatibility (for now always keccak256(bytes32(0x0)))
/// @return execution identifier - keccak256(nonce, signatures)
function executeSigned(
    address to,
    uint256 value,
    bytes data,
    uint nonce,
    uint gasPrice,
    uint gasLimit,
    address gasToken,
    OperationType operationType,
    bytes32 extraHash,
    bytes messageSignatures) public;
```


### Hash calculation
```js
function calculateMessageHash(
  address from, 
  address to, 
  uint value, 
  bytes32 dataHash, 
  uint nonce, 
  uint gasPrice, 
  uint gasLimit, 
  address gasToken, 
  OperationType operationType, 
  bytes32 _extraHash) public pure returns (bytes32) {
    return keccak256(abi.encodePacked(
      from, 
      to, 
      value, 
      dataHash, 
      nonce, 
      gasPrice, 
      gasLimit, 
      gasToken, 
      uint(operationType), 
      keccak256(bytes32(0x0)
   )));
}

```

#### Interface 
```
contract IERC1077 {
    enum OperationType {CALL, DELEGATECALL, CREATE}

    event ExecutedSigned(bytes32 signHash, uint nonce, bool success);

    function lastNonce() public view returns (uint nonce);

    function canExecute(
        address to,
        uint256 value,
        bytes data,
        uint nonce,
        uint gasPrice,
        uint gasLimit,
        address gasToken,
        OperationType operationType,
        bytes32 extraHash,
        bytes signatures) public view returns (bool);

    function executeSigned(
        address to,
        uint256 value,
        bytes data,
        uint nonce,
        uint gasPrice,
        uint gasLimit,
        address gasToken,
        OperationType operationType,
        bytes32 extraHash,
        bytes messageSignatures) returns (bytes32);
}
```

### REST API

POST /identity {firstKey, ensName}
POST /identity/execution {from, to, value, data, nonce, gasToken, gasPrice, gasLimit, operationType, signatures}

