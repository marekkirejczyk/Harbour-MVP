# UniversalLoginSDK

**Project Name**: UniversalLoginSDK

**Links of project**:

- https://github.com/ethworks/universalLoginSDK/

**Short paragraph on where your team is at...**:

UniversalLoginSDK is an SDK composed of smart contracts, a js lib, and a relayer that help build applications using ERC1077 and ERC1078.

**Video DEMO Link:**

- https://www.youtube.com/watch?v=F5t94cCg6XE

## Code

#### Key method
```js
/// @param to - destination address.
/// @param value - value in Ether.
/// @param data - data payload.
/// @param nonce - transaction nonce.
/// @param gasToken - token address (or 0x0 if ETH) that is used for the refund
/// @param gasPrice - gas price that should be used for this transaction.
/// @param gasLimit - maximum gas amount that should be used for this transaction
/// @param operationType 0 - call, 1 - delegate call, 2 - create
/// @param extraData - for future compatibility (for now always keccak256(bytes32(0x0)))
/// @return execution identifier: hash of arguments
    function executeSigned(
        address to,
        uint256 value,
        bytes data,
        uint nonce,
        uint gasPrice,
        address gasToken,
        uint gasLimit,
        OperationType operationType,
        bytes extraData,
        bytes signatures) public returns (bytes32);

```


### Hash calculation
```js
    function calculateMessageHash(
        address from,
        address to,
        uint256 value,
        bytes data,
        uint nonce,
        uint gasPrice,
        address gasToken,
        uint gasLimit,
        OperationType operationType,
        bytes extraData) public pure returns (bytes32)
    {
        return keccak256(
            abi.encodePacked(
                from,
                to,
                value,
                keccak256(data),
                nonce,
                gasPrice,
                gasToken,
                gasLimit,
                uint(operationType),
                keccak256(extraData)
        ));
    }
```

#### Interface 
```
contract IERC1077 {
    enum OperationType {CALL, DELEGATECALL, CREATE}

    event ExecutedSigned(bytes32 executionId, address from, uint nonce, bool success);

    function lastNonce() public view returns (uint nonce);

    function canExecute(
        address to,
        uint256 value,
        bytes data,
        uint nonce,
        uint gasPrice,
        address gasToken,
        uint gasLimit,
        OperationType operationType,
        bytes extraData,
        bytes signatures) public view returns (bool);

    function executeSigned(
        address to,
        uint256 value,
        bytes data,
        uint nonce,
        uint gasPrice,
        address gasToken,
        uint gasLimit,
        OperationType operationType,
        bytes extraData,
        bytes signatures) public returns (bytes32);
}
```

### REST API

POST /identity {managementKey, ensName}
POST /identity/execution {contractAddress, to, value, data, nonce, gasToken, gasPrice, gasLimit, signatures}

