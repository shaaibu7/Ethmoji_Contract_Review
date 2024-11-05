| Client         | Ethmoji Composable Art                               |
| :------------- | :----------------------------------------------------|
| Title          | Ethmoji composable art contract review               |
| Target         | Ethmoji composable art                               |
| Version        | 1.0                                                  |
| Author         | [Shaaibu Suleiman](https://github.com/shaaibu7)      |
| Classification | Public                                               |
| Status         | Draft                                                |
| Date Created   | November 1, 2024                                     |

## Table of contents

- <a href="#intro"> 1. INTRODUCTION</a>

  - <a href="#Disclaim"> 1.1 Disclaimer</a>
  - <a href="#About"> 1.2 About Me </a>
  - <a href="#Skills"> 1.3 Skills</a>
  - <a href="#Cpg"> 1.5 Ethmoji Composable Art</a>
  - <a href="#scope"> 1.7 Scope</a>
  - <a href="#roles"> 1.8 Roles</a>
  - <a href="#overview"> 1.9 System Overview</a>

- <a href="#review"> 2.0 CONTRACT REVIEW</a>

- <a href="#findings"> 3.0 FINDINGS</a>

  - <a href="#Qanalysis"> 3.1 Qualitative Analysis</a>
  - <a href="#summary"> 3.2 Summary</a>
  - <a href="#recom"> 3.2 Recommendations</a>

- <a href="#conclusion"> 4.0 CONCLUSION</a>

<h2 id="intro">1.0 INTRODUCTION </h2>

### <h3 id="Disclaim">1.1 Disclaimer <h3>

My audit of this smart contract is based on the provided information and my expertise in the field. It's essential to clarify that this audit does not serve as a guarantee for the security or functionality of the smart contract, nor does it eliminate all potential risks. Investing in NFT involves inherent risks, and I cannot be held accountable for any financial losses that may arise from investing in this smart contract or engaging in cryptocurrency-related activities. Investors are strongly advised to conduct their own research and due diligence before making any investment decisions.

### <h3 id="About">1.2 🚀 About Me <h3>

Hi, I'm Shaaibu Suleiman, a Smart Contract Developer. Currently, I am advancing my knowledge in Blockchain development at web3bridge.

As a smart contract developer i have passion for creating secure and scalable smart contract through the use of bests practices in design patterns along with comprehensive and continuous smart contract security auditing. 

### <h3 id="Skills">1.3 🛠 Skills <h3>

Solidity, Diamond Standard, Foundry, Hardhat, Wagmi, Ethers.js, ...

### <h3 id="Cpg">1.5 Ethmoji Composable Art<h3>

Ethmoji is an Ethereum-based project where users could create, own, and trade unique, customizable avatars by combining different art components (like eyes, mouth, hair, etc.).

These components were stored as NFTs, making each avatar composable and upgradable on-chain. Users could assemble or change their Ethmojis by swapping in new parts, which created a dynamic form of digital art that each user owned and controlled.

The contracts ensured ownership of both the assembled avatar and individual components, allowing for trading and creativity. This concept was an early exploration of composability in NFTs, enabling modular digital assets on the blockchain.

#### Key aspects of Ethmoji Composable Art include:

1. Modular Design: Each avatar (Ethmoji) was made up of individual, customizable NFT components like eyes, mouth, and hairstyle, allowing unique combinations.

2. Ownership and Control: Users can own composable ERC721 NFTs, making each avatar entirely user-controlled.

3. On-Chain Composition: The composability was managed on-chain, ensuring transparency and verifiability for the assembled avatars and their parts.

4. Early Composability in NFTs: Ethmoji was an innovative example of NFT composability, showcasing how digital assets could be combined, modified, and expanded on the blockchain.


### <h3 id="scope">1.7 Scope <h3>

_(**Table: 1.7**: Ethmoji Composable Art Audit Scope)_
| Files in scope | SLOC |
| :-------- | :------- |
| Contract: 1 | |
| `Composable.sol` | `432` |
| | |
| contracts: 7 | |
| `Avatar.sol` | `67` |
| `Ethmoji.sol` | `7` |
| `EthmojiProxy.sol` | `60` |
| `Migrations.sol` | `10` |
| `OwnableProxy.sol` | `33` |
| `Proxy.sol` | `6` |


### <h3 id="overview"> 1.9 System Overview <h3>

- #### Composable.sol

  The Composable contract is an ERC721 NFT system where users can create unique compositions by layering multiple tokens. Each composition has a price and an optional increase rate, allowing the price to rise with use. Owners earn royalties when their tokens are used in compositions. The contract enforces uniqueness by tracking image hashes and layers to prevent duplicates. Users can mint base tokens and layered compositions, but the total layers are limited to ensure control. It includes pause functionality, enabling the owner to suspend certain actions. Admins can set fees and manage payments.

- #### Avatar.sol

  The Avatar contract allows users to set and retrieve an avatar represented by an ERC721 token. Each user can link a specific NFT (address and ID) as their avatar if the token's contract address is valid and they own it. Only the contract owner can manage the list of valid avatar contracts by adding or removing them. The getAvatar function checks token ownership and returns the avatar's ID and contract address. Users can update their avatars only if the contract address is approved, and they hold the token. The contract helps enforce ownership and validity for avatar assignments.

- #### Ethmoji.sol

  The Ethmoji contract enables users to mint and create custom emojis by combining different NFT layers. It extends Composable and uses SafeMath for secure arithmetic. The initialize function sets up the contract with a specified owner and a minimum fee for compositions. The compose function allows users to mint a composed emoji using an array of token IDs as layers, instantly distributing payments to the layer owners. _withdrawTo handles secure withdrawal of payments to each layer owner. This contract introduces a creative and monetizable way to build custom NFTs with layered ownership.

- #### Migrations.sol

  The Migrations contract is a utility contract for managing and tracking deployment migrations. It sets the contract creator as the owner and stores the last completed migration. The setCompleted function, restricted to the owner, updates the migration status. The upgrade function allows the owner to link a new migration contract, transferring the last migration status to it.

- #### EthmojiProxy.sol:

  The EthmojiProxy contract is a proxy contract that enables upgradability by delegating calls to a separate implementation contract. It stores the address of the current implementation, which can be upgraded by the proxy owner. The upgradeTo function allows updating the implementation address, while upgradeToAndCall also allows calling a function on the new implementation immediately after upgrading. The _upgradeTo function finalizes the upgrade, ensuring that only new addresses are set, and emits an Upgraded event.

  - #### OwnableProxy.sol

  The OwnableProxy contract defines an upgradable proxy with ownership control. It stores the owner's address in a specific storage slot, allowing the owner to be changed. The onlyProxyOwner modifier restricts certain functions to the current owner, while transferProxyOwnership allows the owner to transfer ownership to another address, emitting a ProxyOwnershipTransferred event. The proxyOwner function retrieves the current owner's address, and setUpgradeabilityOwner sets a new owner in storage.

- #### Proxy.sol

  The Proxy contract allows delegating calls to another contract's implementation. It defines an abstract implementation function that returns the address of the current implementation. The fallback function uses delegatecall to forward all incoming calls to the implementation address, ensuring the called contract’s logic is executed while retaining the proxy’s state. If the delegate call fails, it reverts; otherwise, it returns the result from the implementation.


## <h2 id="review"> 2.0 CONTRACT REVIEW <h2>

- The contract uses pragma solidity ^0.4.21, which is outdated and potentially insecure. It is      recommended to upgrade to Solidity ^0.8.x for enhanced security features like automatic overflow/underflow checks though the contract made use of safemath library.

- The contract uses transfer which can fail unexpectedly and is not recommended in modern Solidity. Consider using call with proper checks to avoid reentrancy issues.

```bash
  if (msg.value > price) {
            uint256 purchaseExcess = SafeMath.sub(msg.value, price);
            msg.sender.transfer(purchaseExcess);          
  }

```

- The external calls made in the compose function create potential reentrancy attacks to the contract expecially at the point of calling transfer.

```bash
  require(ownerOf(compositionLayerId) != address(0));
  asyncSend(ownerOf(compositionLayerId), tokenIdToCompositionPrice[compositionLayerId]);
  emit RoyaltiesPaid(compositionLayerId, tokenIdToCompositionPrice[compositionLayerId], ownerOf(compositionLayerId));
  tokenIdToCompositionPrice[compositionLayerId] = tokenIdToCompositionPrice[compositionLayerId].add(tokenIdToCompositionPriceChangeRate[compositionLayerId]);

```

- The compose function has too much nested loops which is not very efficient in gas optimizations and still makes external calls which is not very safe and prone to reentrancy attacks.

```bash
function compose(uint256[] _tokenIds,  uint256 _imageHash) public payable whenNotPaused {
        require(_tokenIds.length > 1);
        uint256 price = getTotalCompositionPrice(_tokenIds);
        require(msg.sender != address(0) && msg.value >= price);
        require(_tokenIds.length <= MAX_LAYERS);

        uint256[] memory layers = new uint256[](MAX_LAYERS);
        uint actualSize = 0; 

        for (uint i = 0; i < _tokenIds.length; i++) { 
            uint256 compositionLayerId = _tokenIds[i];
            require(_tokenLayersExist(compositionLayerId));
            uint256[] memory inheritedLayers = tokenIdToLayers[compositionLayerId];
            if (isCompositionOnlyWithBaseLayers) { 
                require(inheritedLayers.length == 1);
            }
            require(inheritedLayers.length < MAX_LAYERS);
            for (uint j = 0; j < inheritedLayers.length; j++) { 
                require(actualSize < MAX_LAYERS);
                for (uint k = 0; k < layers.length; k++) { 
                    require(layers[k] != inheritedLayers[j]);
                    if (layers[k] == 0) { 
                        break;
                    }
                }
                layers[actualSize] = inheritedLayers[j];
                actualSize += 1;
            }
            require(ownerOf(compositionLayerId) != address(0));
            asyncSend(ownerOf(compositionLayerId), tokenIdToCompositionPrice[compositionLayerId]);
            emit RoyaltiesPaid(compositionLayerId, tokenIdToCompositionPrice[compositionLayerId], ownerOf(compositionLayerId));
            tokenIdToCompositionPrice[compositionLayerId] = tokenIdToCompositionPrice[compositionLayerId].add(tokenIdToCompositionPriceChangeRate[compositionLayerId]);
        }
    
        uint256 newTokenIndex = _getNextTokenId();
        
        tokenIdToLayers[newTokenIndex] = _trim(layers, actualSize);
        require(_isUnique(tokenIdToLayers[newTokenIndex], _imageHash));
        compositions[keccak256(tokenIdToLayers[newTokenIndex])] = true;
        imageHashes[_imageHash] = newTokenIndex;
        tokenIdToImageHash[newTokenIndex] = _imageHash;
    
        _mint(msg.sender, newTokenIndex);

        if (msg.value > price) {
            uint256 purchaseExcess = SafeMath.sub(msg.value, price);
            msg.sender.transfer(purchaseExcess);          
        }

        if (!isCompositionOnlyWithBaseLayers) { 
            _setCompositionPrice(newTokenIndex, minCompositionFee);
        }
   
        emit CompositionTokenCreated(newTokenIndex, tokenIdToLayers[newTokenIndex], msg.sender);
    }


```


- In the _isUnique function the uniqueness check compositions[keccak256(_layers)] == false might be susceptible to collisions, particularly if _layers contain predictable data or zero values. Ensure that _layers is adequately sanitized and normalized before hashing.


```bash
    function _isUnique(uint256[] _layers, uint256 _imageHash) private view returns (bool) { 
        return compositions[keccak256(_layers)] == false && imageHashes[_imageHash] == 0;
    }

```


- Functions like mintTo and compose lack comprehensive input validation. For example, parameters such as _imageHash could be manipulated if not properly validated. Implement efficient validation for all inputs to avoid unexpected behaviors or potential exploits.

```bash
function mintTo(address _to, uint256 _compositionPrice, uint256 _changeRate,  bool              _changeableCompPrice, uint256 _imageHash) public onlyOwner {
        uint256 newTokenIndex = _getNextTokenId();
        _mint(_to, newTokenIndex);
        tokenIdToLayers[newTokenIndex] = [newTokenIndex];
        require(_isUnique(tokenIdToLayers[newTokenIndex], _imageHash));
        compositions[keccak256([newTokenIndex])] = true;
        imageHashes[_imageHash] = newTokenIndex;
        tokenIdToImageHash[newTokenIndex] = _imageHash; 
        emit BaseTokenCreated(newTokenIndex);
        _setCompositionPrice(newTokenIndex, _compositionPrice);
        _setCompositionPriceChangeRate(newTokenIndex, _changeRate);
        _setCompositionPriceChangePermission(newTokenIndex, _changeableCompPrice);
    }

```


## <h2 id="findings">3.0 FINDINGS </h2>

### <h3 id="Qanalysis"> 3.1 Qualitative Analysis<h3>

_(**Table: 3.1**: GovernorBravoDelegate G2 Qualitative Analysis)_
| Metric | Rating | Comment |
| :-------- | :------- | :----- |
| Code Complexity | Medium | Proper validations and security can be added |
| Documentation | Moderate | Documentation could be improved |
| Best Practices | Moderate | Make use of recent solidity versions and with underflow and overflow checks |

### <h3 id="summary">3.2 Summary<h3>

In summary, recent versions of solidity should be used with built in underflow and overflow checks, proper input validation and security measures should be implemented especially replacing the use of transfer with low level call to send ether because transfer does not allow the specification of custom gas in sending the call as it forwards only 2300 gas.

### <h3 id="recom">3.2 Recommendations<h3>

Consider using the updated solidity version and low level call instead of transfer.

## <h2 id="conclusion">4.0 CONCLUSION </h2>

In this audit, I have conducted an analysis of the "Composable" Solidity smart contract, focusing on its architecture and functionality. The "Composable" contract extends ERC721, implementing mechanisms for creating complex NFT compositions, enabling token minting, ownership transfers, and royalty payments. This contract incorporates features from OpenZeppelin, including SafeMath, Ownable, Pausable, and PullPayment, which enhance security, modularity, and payment management.


The use of mappings and private functions ensures efficient data storage and encapsulation. Security measures, including checks for token ownership, valid token layer composition, and the prevention of duplicate compositions, bolster the contract's reliability and prevent potential vulnerabilities.

However, considering the evolving nature of blockchain technology and smart contract security, continued testing, auditing, and potential updates are recommended to maintain and enhance the contract's reliability and functionality.

In conclusion, the "Composable" contract serves as a robust framework for minting and managing complex NFT compositions. Its use of industry-standard libraries and well-defined logic positions it as a valuable asset within the NFT ecosystem. The suggested enhancements and security considerations outlined in this audit can guide future updates and contribute to more secure, scalable, and maintainable implementations.