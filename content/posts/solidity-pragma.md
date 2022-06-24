---
title: "Pragma"
date: 2022-06-22T21:20:47+05:00
author: "Haris Aqeel"
description: "The keyword 'pragma' is used in solidity programming language to enable certain compiler features and checks. Every solidity file has its own pragma directive which is local to it. "

---


The keyword 'pragma' is used in solidity programming language to enable certain compiler features and checks. The keyword pragma is used at the top-level (generally the first line) of the file before writting any Solidity code. The pragma keyword is followed by the version of Solidity we need to use. 

Since, Solidity is a rapidly changing and growing language, so there are changes and updates in it every few months. The code written now on Solidity might not be compatible or may introduce security issues with upcoming or future Solidity versions. Therefore, it is always a good idea to use pragma in the code. 


Now, there are three types of pragma used in Solidity programming language which are as following

1. Version Pragma
2. ABI Coder Pragma
3. Experimental Pragma

### 1. Version Pragma

The version pragma directive of Solidity instructs the compiler to ensure that the code complies only for the given version or version range of the Solidity. Otherwise, the compilation should be rejected. 

**Example:** 



```solidity   
// SPDX-License-Identifier: GPL-3.0

// ***USE ONE OF THE BELOW***

// 1)
pragma solidity >=0.6.0 <0.8.0;
// The above pragma directive will instruct the compiler to 
// compile code for all versions greater than 0.6.0, all the 
// way up but not including version 0.8.0

// 2)
pragma solidity ^0.6.0;
// The above pragma directive will instruct the compiler to 
// compile code for all versions equal to 0.6.0, but not
// any version below 0.6.0 like 0.5.9 or any version greater
// than 0.6.0 like 0.7.1. So, valid range is (0.6.0 -- 0.6.9)  


contract DemoContract 
{
   uint sampleData;
   
   function set(uint a) public 
   {
      sampleData = a;
   }
   
   function getData() public view returns (uint) 
   {
      return sampleData;
   }
}

```

### 2. ABI Coder Pragma

***What is ABI and Why it is necessary?***

The Contract Application Binary Interface (ABI) is the standard way to interact with contracts in the Ethereum ecosystem, both from outside the blockchain and for contract-to-contract interaction. Data is encoded according to ABI type. There are two implementations of it.

1. v1 ABI encoder
2. v2 ABI encoder

***What is difference between v1 and v2?***

The main difference is that v2 implementation supports nested struct and array types which are not supported in v1.

**What is default version?**

From Version 0.8.0, the pragma abi encoder v2 is default. Prior to this, abi encoder v1 was used.

***Example1***

If we use, the nested array and struct types in versions prior to 0.8.0 such as (version 0.7.0), then by default the abi encoder v1 is used which don't supports nested array and struct type, thus will result in a compilation error. 

```solidity 
// SPDX-License-Identifier: GPL-3.0

pragma solidity ^0.7.0;
// Since version is <0.8.0 therefore, if nothing is written(related to abi version)
// that means abi encoder v1 is used which does not support the struct S and 
// struct T used later in code, so compilation error will occur.

contract Test {
struct S { uint a; uint[] b; T[] c; }
struct T { uint x; uint y; }
function f(S memory, T memory, uint) public pure {}
function g() public pure returns (S memory, T memory, uint) {}
}

```

***Compiler Response:***

```
    contracts/Sample.sol:10:12: TypeError: This type is only supported in ABI coder v2. 
    Use "pragma abicoder v2;" to enable the feature.
    function f(S memory, T memory, uint) public pure {}
    ^------^
```

Now, to make the above code works we need to explicitly mention the abi encoder pragma as shown below in example,

```solidity 

// SPDX-License-Identifier: GPL-3.0

pragma solidity ^0.7.0;
// To compile it successfully, add the abi encoder pragma as shown in next line
pragma abicoder v2;

contract Test {
struct S { uint a; uint[] b; T[] c; }
struct T { uint x; uint y; }
function f(S memory, T memory, uint) public pure {}
function g() public pure returns (S memory, T memory, uint) {}
}

```



### 3. Experimental Pragma

Experimental Pragma in Solidity is used to enable experimental features of Solidity that are not enabled by default.

We discussed ABI coder above, however, it was just an experimental feature until 0.6.0 which is why the below code with struct types will throw a compilation error.


```solidity 

// SPDX-License-Identifier: GPL-3.0

pragma solidity ^0.6.0;
// To compile it successfully, add the abi encoder pragma as shown in next line
pragma abicoder v2;

contract Test {
struct S { uint a; uint[] b; T[] c; }
struct T { uint x; uint y; }
function f(S memory, T memory, uint) public pure {}
function g() public pure returns (S memory, T memory, uint) {}
}

```

***Compiler Response:***

```
    contracts/Sample.sol:12:12: TypeError: This type is only supported in ABIEncoderV2. 
    Use "pragma experimental ABIEncoderV2;" to enable the feature.
    function f(S memory, T memory, uint) public pure {}
    ^------^
```

To overcome this, the pragma experimental ABIEncoderV2 can be used as below that compiles the code without
any issues.



```solidity 

// SPDX-License-Identifier: GPL-3.0

pragma solidity ^0.6.0;

pragma experimental ABIEncoderV2;

contract Test {
struct S { uint a; uint[] b; T[] c; }
struct T { uint x; uint y; }
function f(S memory, T memory, uint) public pure {}
function g() public pure returns (S memory, T memory, uint) {}
}

```

