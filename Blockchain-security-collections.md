# Blockchain-security-collections


# 1. Application layer
## 1.1 Network Bandwidth
### 1.1.1 Denial of Service Attack
The attacker first makes the platform inaccessible through DDOS. At this time,usually there will  be relevant news to report the incident. However, the general public can't distinguish the difference between denying access attacks and intrusions. After the "trading" of the trading platform used by them, they usually choose to switch to other platforms for their own fund security, resulting in the loss of funds and users of this platform.

## 1.2 Account System
### 1.2.1 Collision attack
Due to the current lack of general security awareness among netizens, they often use common usernames and passwords and log in with the same account and password on different websites.

The attacker attempts to batch try through the program on the website to be attacked through the public or unpublished user name, email address, password and other information on the mobile phone Internet.

- Scenes:
The attacker hacks or collects user data  (including usernames, passwords, etc.) of the publicly available blockchain-related websites on the network, and uses the program automation to try one by one on the target trading platform, resulting in great threat to account security.

### 1.2.2 Exhaustive attack
If the website does not request restrictions or risk control on the login interface, it will cause the attacker to send requests infinitely to test the possible values ​​one by one to brute force some key information.

- Scenes:
1. In SMS verification, if you do not limit the validity period of the SMS verification code or limit the authentication interface, it is easy to crack the SMS verification code.

2. If the login interface does not make a request limit, the attacker can brute force the password of an account through a large number of password dictionaries. In other words, an attacker can violently crack a user whose password is a certain value through a large number of user name dictionaries, such as a user whose password is 123456.

### 1.2.3 Single Sign-On Vulnerability
In the account system, such vulnerabilities are relatively hidden. Attackers can steal the user's login ticket through CSRF, XSS, etc., resulting in theft of user accounts.

There are mainly the following attack surfaces:

● Unmanned hijacking caused by not using HTTPS

● Jsonp interface leaked ticket

● CSRF vulnerability stealing ticket

● XSS vulnerability stealing ticket

### 1.2.4 oAuth Protocol Vulnerability
The oAuth protocol to 2.0 is actually safe enough, but only the protocol security does not mean that its final implementation is no problem. In the case of insufficient security awareness, it is easy to cause some potential threats, which can cause attackers to use CSRF and other means. More than the right to log in to someone else's account.

There are some attack surfaces as follows:

● Utilize CSRF vulnerability binding hijacking

● Authorize hijacking with redirect_uri

● Use scope permissions to control improper access

## 1.3 Payment System
### 1.3.1 Payment Vulnerability
When it comes to payment, there may be a payment loophole, and the payment loophole directly relates to the security of the fund. It is a high risk for the platform or the user and must be treated with caution. The following summarizes the common problems in the payment system:

1. Modify the payment price problem: The back-end verification of the payment price is not made at the time of payment, so that the price can be lowered or even set to a negative number to obtain income through the transaction.

2. Modify the purchase quantity problem: In the process of payment, the quantity also determines the price at the same time. For example, 1 quantity of goods corresponds to 100, 2 pieces of data is 200, then when you modify the value of this value to be negative, then The amount will also become negative, which will eventually lead to payment problems.

3. Maximum payment problem: By purchasing a large number of goods, the final payment amount is very large, and there may be a large integer overflow vulnerability in the back end. When the value exceeds a certain threshold, the result will be 0 or a negative number.

4. Over-authorization payment problem: The lack of verification on the back-end leads to the modification of the current user ID by using the change of the package to use the balance of others to pay.

## 1.4 Business Logic
  - Override vulnerability
  - Captcha video vulnerability
  - Conditional Competitive Vulnerability
  - Authentication vulnerability

## 1.5 ICO
### 1.5.1 Tampering Attack
When ICO raises funds, it usually hangs the collection address on the project official website, and then the investor will transfer money to this address in exchange for the corresponding token.

Attack scenario: Hackers tamper with the collection address on the official website of the project through attack methods such as domain name `hijacking`, web vulnerability, or social engineering. After that, the funds raised by the project fell into the hands of hackers.

### 1.5.2 Phishing attack
The attacker uses social engineering and other means to impersonate the official, allowing the user to transfer money to the attacker's wallet address.

- Attack scenario:
1. Defraud investors with approximate domain names + highly phishing websites

2. Use email to walk fake information, such as ICO project receipt address change notifications, etc.

3. Take a walk on social software and media to scam information to defraud investors

# 2. Contract layer

## 2.1 Integer Overflow Attack
In common programming languages, variables of integer type generally have maximum and minimum values. A smart contract is essentially a program code, and the integers in the contract will have corresponding maximum and minimum values. Once the value stored in the variable exceeds the maximum value, an integer overflow error occurs, resulting in the last value stored in the variable being 0. Otherwise, it is an integer underflow error, and the last value stored in the variable is the maximum value of the variable. Of course, the overflow situation is not limited to the above integer overflow or integer underflow, and may also overflow during calculation, conversion, and the like.

- Scenario: Suppose the balance in a smart contract is an unsigned integer type. The range of this type is 0~65535. When the attacker makes the balance less than 0 by some means, its balance in the smart contract will underflow to 65535. When the balance is greater than 65535, its balance in the smart contract will overflow to zero.

## 2.2 Resource Abuse Attack
An attacker can deploy a malicious code on a virtual machine, consuming system resources, storage resources, computing resources, and memory resources.

Therefore, there must be a corresponding restriction mechanism in the virtual machine to prevent the system resources from being abused.

## 2.3 Stack overflow
An attacker can write a malicious code to let the virtual machine parse the execution. Eventually, the depth of the stack exceeds the maximum depth allowed by the virtual machine, or the system memory is continuously occupied, causing memory overflow. The most serious is a command execution vulnerability.

In the operating system, `&,|,||` can be used as a command connector. The user submits an execution command through the browser. Since the server does not filter the execution function, it is executed without specifying an absolute path. Commands that may allow the user to execute a maliciously constructed code by changing `$PATH` or other aspects of the program's execution environment.

<b>Command Execution Vulnerability Classification:</b>

1.Code layer filtering is not strict

Some of the core code for business applications is packaged in a binary file, which is called by the system function in the web application:
`System("/bin/program --arg$arg");`

2.System vulnerabilities cause command injection
`Bash shelling vulnerability (CVE-2014-6271)`

3.A third-party component called has a code execution vulnerability

Such as ImageMagick component used to process images in WordPress, command execution vulnerability in JAVA (struts2/ElasticsearchGroovy, etc.), ThinkPHP command execution

<b>Use of command functions:</b>

1.The System:system function can be used to execute an external application and output the corresponding execution result. The function prototype is as follows:
`String system(string command, int&return_var)`

Among them, command is the command to be executed, and return_var stores the state value after the execution of the execution command.

2.Exec: exec function can be used to execute an external application
`String exec (string command, array&output, int &return_var)`

Among them, command is the command to be executed, output is the string of each line that gets the output of the execution command, and return_var stores the state value after executing the command.

3.Passthru: The passthru function can be used to execute a UNIX system command and display the original output. When the output of the UNIX system command is binary data and you need to return the value directly to the browser, you need to use the passthru function instead of system and exec. function. The prototype of the Passthru function is as follows:
`Void passthru (string command, int&return_var)`

Among them, command is the command to be executed, and return_var stores the state value after executing the command.

4.Shell_exec: Execute the shell command and return the output string. The function prototype is as follows:
`String shell_exec (string command)`

Among them, command is the command to be executed.
## 2.4 Override access
Improper handling of access control in smart contracts may result in excessive currency risk

<b>ERC223 smart contract ATEN currency owner stealing vulnerability:</b>

In order for the ATN receiving contract to have post-transfer processing, the ATN Token contract uses the extended standard ERC223 that is compatible with the ERC20 Token standard and uses the dapphub/ds-auth library. There is no problem when using the ERC223 or ds-auth libraries alone, but when combined, the hacker uses the custom callback function to call back the setOwner method to get advanced permissions.

By exploiting this ERC223 method and the ds-auth library's hybrid vulnerability, the hacker changed the owner of the ATN Token contract to its own controlled address. After obtaining the owner permission, the hacker initiates another transaction to attack the ATN contract, and calls the mint method to issue a token to another address. Finally, the hacker calls the setOwner method to restore the permissions.

## 2.5 Denial of service
Incorrect handling of loop statements, recursive functions, external contract calls, etc., may lead to denial of service risks such as infinite loops, recursive stack exhaustion, etc.

## 2.6 Logic error

When a virtual machine finds that the data or code does not conform to the specification, it may do some "fault-tolerant processing" on the data, which may lead to some logic problems, the most typical being "Ethernet short address attack."

The general ERC-20 TOKEN standard token will implement the transfer method. This method is defined in the ERC-20 tag as:
`Function transfer(address to, uint tokens) public returns (bool success);`

The first parameter is the destination address of the sending token, and the second parameter is the number of tokens sent. When we call the transfer function to send N ERC-20 tokens to an address, the input data of the transaction is divided into three parts:
<pre>
4 bytes, is the hash of the method name: a9059cbb

32 bytes, put the Ethereum address, the current Ethereum address is 20 bytes, high-risk 0
000000000000000000000000abcabcabcabcabcabcabcabcabcabcabcabcabca

32 bytes, is the number of tokens that need to be transferred, here is 1*10^18 GNT
0000000000000000000000000000000000000000000000000de0b6b3a7640000

All of this together is the transaction data:
A9059cbb000000000000000000000000abcabcabcabcabcabcabcabcabcabcabcabcabca0000000000000000000000000000000000000000000000000de0b6b3a7640000
</pre>

When the transfer method is called to raise coins, if the user is allowed to enter a short address, it is usually not processed here by the exchange, for example, it is not verified whether the length of the address input by the user is legal.

If an Ethereum address is as follows, notice that the end is 0:
`0x12345678901234567890123456789012345678*00*`

When we omit the following 00, the EVM will be replenished from the high position of the next parameter to the 00 position. At this time, the token number parameter will actually be one byte less, that is, the number of tokens is shifted by one byte to the left. Make the contract send a lot of tokens.

## 2.7 Information disclosure
Hard-coded addresses, etc., may lead to the disclosure of important information. The disclosure of such information may be exploited by hackers as useful information for attacks, such as stack information.

## 2.8 Function misuse
The call and delegatecall functions let Ethereum developers modularize their code (Modularise). When the call function is used to handle an external standard message call (Standard MessageCall) for a contract, the code runs in the context of the external function. The delegate function is also a standard message call, but the code in the target address will run in the context of the calling contract, that is, keep msg.sender and msg.value unchanged. This feature supports implementation libraries, and developers can create reusable code for future contracts. In layman's terms, it's a way for a contract to call some function of another contract. However, hackers use the cross-contract process to replace the original correct function with a hacker-defined function, and a specific function can perform some dangerous operations.

Specific analysis: https://blog.sigmaprime.io/solidity-security.html#delegatecall

## 2.8 Function misuse

The call and delegatecall functions let Ethereum developers modularize their code. When the call function is used to handle an external standard message call for a contract, the code runs in the context of the external function. The delegate function is also a standard message call, but the code in the target address will run in the context of the calling contract, that is, keep msg.sender and msg.value unchanged. This feature supports implementation libraries, and developers can create reusable code for future contracts. In layman's terms, it's a way for a contract to call some function of another contract. However, hackers use the cross-contract process to replace the original correct function with a hacker-defined function, and a specific function can perform some dangerous operations.

Specific analysis: https://blog.sigmaprime.io/solidity-security.html#delegatecall

## 2.9 Escape Vulnerability

A virtual machine is a "completely isolated guest operating system installation within a normal host operating system". virtual machine escape is a process of breaking out of a virtual machine and interacting with host operating system. The virtual machine will run the bytecode Provide a sandbox environment, the general user can only execute the corresponding code in the sandbox limit, this type of vulnerability will allow the attacker to exit the sandbox environment and execute other code that could not be executed.

[For further reading, there are some articles.](https://github.com/xairy/vmware-exploitation)

## 2.10 Call depth attack

In the contract virtual machine, the threshold of the mutual call of the smart contract is set to a threshold. If the depth exceeds this threshold, the call will fail. For example, in Ethereum EVM, the call depth is limited to 1024.

Calling a deep attack can make the contract call fail, even if the call has no logical problem, but it is not allowed at the virtual machine level, because the call depth reaches the threshold in the virtual machine, and the threshold is no longer executed.

An attacker can control certain call operations by controlling the depth of the call, such as transfer, balance clearing, and so on.

## 2.11 Reentrancy Attack

One of the features of Ethereum smart contracts is their ability to call and utilize code from other external contracts. Contracts also typically handle ether, and as such often send ether to various external user addresses. These operations require the contracts to submit external calls. External calls can be hijacked by attackers, who can force the contracts to execute further code (through a fallback function), including calls back into themselves. Attacks of this kind were used in the infamous DAO hack.

One feature of the Ethereum Smart Contract is the ability to call and use code from other external contracts. Contracts can also usually handle Ethereum, so Ethanol is often transferred to the addresses of various external users. An operation that calls an external contract or sends an Ether to an address requires the contract to submit an external call. However, these external calls may be hijacked by the attacker, forcing the contract to execute further code.

[For further reading, there is an example.](https://medium.com/@gus_tavo_guim/reentrancy-attack-on-smart-contracts-how-to-identify-the-exploitable-and-an-example-of -an-attack-4470a2d8dfe4)

## 2.12 Transaction-Ordering Attack

It takes a certain amount of time for a transaction to be spread out and recognized by the miner in a block. If an attacker is listening to a transaction in the corresponding contract on the network, then send his own transaction to change the current contract status, for example Rewarding contracts and reducing contract returns have a chance to keep the two transactions under the same block and to complete the attack before another transaction. This means that if a user is revealing a solution to a puzzle or other valuable secret, a malicious user can steal the solution and copy its transaction at a higher cost to preempt the original solution.

1. The attacker submits a prize-winning contract to let the user find out the solution to the problem and promises to give a generous reward.

2. After the attacker submits the contract, he will continue to listen to the network and see if anyone has submitted a solution to the answer.

3. Someone submits the answer. At this time, the transaction that submitted the answer has not been confirmed, and the attacker immediately launches a transaction to reduce the amount of the bonus to make it infinitely close to zero.

4. The attacker provides a higher Gas, and his own transaction is first processed by the miner.

5. When the miner first processes the transaction that submits the answer, the rewards received by the answer submitter will be extremely low, and the attacker will be able to get the correct answer almost free of charge.

For example, approve() in ERC20 can lead to potential transaction order dependency problems, as follows:

1. Alice allows Bob to transmit N Alice's tokens (N > 0) by calling the approve method to pass Bob's address and N as method parameters on the Token smart contract;

2. After a while, Alice decides to change from N to M (M > 0). Alice's token Bob is allowed to transfer, so she calls the approval method again, this time passing Bob's address and M as method parameters;

3. Bob notices that Alice's second transaction calls the transferFrom method to transfer N Alice's tokens before mining and sending it quickly;

4. If Bob's transaction will be executed before Alice's transaction, then Bob will perform the successful transfer of N Alice's tokens and gain the ability to transfer another M tokens;

5. Before Alice noticed a problem, Bob called the transferFrom method, this time transferring M Alice's tokens.

## 2.13 Timestamp Attack

Some smart contracts use the timestamp of the block as a trigger for certain operations. Usually, the miner's local time is used as the timestamp, and this time can fluctuate in a range of about 900 seconds. When other nodes accept a new block, they only need to verify whether the timestamp is later than the previous block and is local. The time error is within 900 seconds. Therefore, a miner can profit from the conditions that are favorable to him by setting the time stamp of the block.

## 2.14 Misoperation Abnormal Attack

When Contract A calls another operation of Contract B, the execution of Contract B operation may fail due to various reasons, and then return to the state before execution. If Contract A does not check the result of Contract B execution, it will continue. Execution will cause many problems. For example, Contract A invokes the withdrawal operation of Contract B and adds the same value as the withdrawal amount in the balance of Contract A. At this time, if the return value of the execution withdrawal operation of the contract B is not checked, the balance in the contract B may not be reduced, and the balance in the contract A has been increased.

# 3. Consensus layer
## 3.1 Short-range attack 
It mainly affects the POS mechanism. Examples of attacks are as follows:
1. An attacker buys a good or service

2. The merchant starts waiting for the network to confirm the transaction

3. At this point, the attacker began to claim for the first time in the network that the current relatively longest chain that does not contain the transaction is rewarded.

4. When the main chain is long enough, the attacker begins to give out more rewards, rewarding miners who include mining in the trading chain.

5. After the six confirmations are reached, give up the reward

6. The goods arrive at the same time, while giving up the chain selected by the attacker.

7. As long as the cost of the bribery attack is less than the cost of the goods or services, the attack is successful.

In contrast, the bribery mechanism in the POW mechanism requires bribing most miners at very high cost.

## 3.2 Long distance attack
This type of attack is typically a "51%" attack.

In PoS, the speed at which each block is generated is much faster than the PoW. Therefore, a few unscrupulous nodes will think about rewriting the entire blockchain consensus book.

This is a classic 51% problem in PoW, that is, when a node controls 51% or more of the computing power, it has the ability to tamper with the books, but achieving 51% of computing power is extremely difficult. In the absence of constraints on computing power in PoS, there is a potential for falsification of the books.

## 3.3 Cumulative cumulative attack
In the earliest Peercoin version, the difficulty of mining was not only related to the current account balance, but also to the currency holding time of each currency. This results in some nodes having the ability to use the increase in Age to control the entire network after waiting for a long time, which has a very significant impact.

## 3.4 Precomputation Attack
When a node in PoS occupies a certain amount of computing power, the node that has a certain power in PoS has the ability to control Hprev to make its own power range to calculate Hnext (hash of next block).

- PoS mechanism:
In the PoW mechanism, since it is often necessary to spend a lot of power and time costs to find a qualified nonce, in order for each block to be generated faster, the PoS mechanism removes the process of exhaustive nonce, and then adopts the following Fast algorithm:

         H(HBprev), A, t) ≤ balance(A)m

- H is still a hash function
- t is the UTC timestamp
- Bprev refers to the previous block
- balance(A) represents the balance of account A's account
- m still represents a number that is considered to be defined

On the left side of the equation, the only parameter that can be continuously adjusted is t. The right side of the equation m is a fixed real number. Therefore, when the balance(A) is larger, the probability of finding a reasonable t is larger. In the network, there is a general limit on the range of t. For example, the timestamp that can be tried cannot exceed the standard timestamp by 1 hour. That is to say, a node can try 7200 times to find a qualified t. If it is not found, Can give up. Therefore, in PoS, the more balance of an account, the easier it is to find the next block under the same power.
## 3.5 Witch Attack
Sybil Attack, an online cybersecurity system threat, refers to an individual trying to control the network by creating multiple account identities, multiple nodes or computer coordinates.

If the attacker creates enough false identities (or Sybil identities), they can repel the real nodes on the network with a majority vote.

Then in this case, they can refuse to receive or transmit blocks, effectively blocking other users from entering the network.

In the larger Sybil attack, the premise is that when the attacker has controlled most of the computer network or hash rate, they can cover 51% of system attacks. In this case, they can easily change the order of the transactions and prevent the transaction from being confirmed. They can even pick up and reverse transactions, leading to double spending problems.

# 4. Network Layer

## 4.1 Transaction Malleability

Transaction Malleability, means that a Bitcoin payment transaction can be modified after it is issued (accurately, it is forged copy).

Why is it possible to tampere a transaction after it has been issued, even with a signature? One of the reasons is that most mining programs use the openssl library to verify user signatures, while openssl is compatible with multiple encoding formats. Also, it is the elliptic curve digital signature algorithm (ECDSA) itself, signature (r, s) and signature (r). , -s(mod n)) are all valid. Therefore, some modifications are made to the representation of the signature string itself, which is still a valid signature.

For a post-broadcast transaction, the scriptSig in its input may be modified by the attacker. This modification will not cause a functional change in the transaction - it cannot change the input and output of the transaction, but it will affect the txid of the transaction because Txid is the hash of the entire transaction, making txid different from what the creator of the transaction expects, and the changed transaction is likely to be included in the revenue block.

1. The hacker has one account, opens an account on the exchange, and transfers his bitcoin.

2. Apply for withdrawal, the exchange initiates a Transaction.

3. The transaction was broadcast to the network and was not packaged before the blockchain. The hacker received the Transaction, slightly changed the format of scriptSig, and generated a new transaction to broadcast, and the Transaction id has changed.

4. The new transaction of the hacker was received by the blockchain. Then complained to the exchange and said that it did not receive the money. The exchange queried the transaction according to the Transaction Id generated by itself, and found that it could not be queried on the network, and then transferred to the hacker again, that is, double withdraw - the same money, was hacked 2 times, or even many times.

## 4.2 Block Withholding Attack

The block attack is mainly for the mining pool. After a miner digs into the block, he can keep the verified hash and not broadcast it. After doing so, the result is that no reward is given. But the loss of miners and mines is not the same. The loss of miners is only a reward, and usually the amount is not that big. But the loss of the pool is to calculate all the rewards for the block.

## 4.3 Denial of Service Attack

Attacking nodes in a P2P network through large traffic or loopholes, causing some nodes in the network to smash, which means that the total power in the chain is damaged, making it more vulnerable to 51% attacks, and currently the cost of denial of service attacks. Also low, a large number of attack tool platforms can be easily purchased for attack on the black market. (eg DDoS attacks with low cost)

## 4.4 Centralization Problem

In the blockchain project reward mechanism, when the nodes' work costs are less than or close to the benefits, they tend to choose not to work for this blockchain, which can easily lead to centralization problems. A large number of miners are chained, and attackers can launch 51% attacks at a lower cost.

## 4.5 Weak Password Attack

At present, the mining machine system in the market is based on the B/S structure. The access to the mining machine system is usually through the web or other means. If the weak password is used on the mining machine, it will be vulnerable to invasion.

## 4.6 0day Attack

Most of the mining machine systems are general-purpose systems and are rarely custom developed. Generally, when the manufacturer sells the mining machine, many manufacturers will use the same system, but the hardware with different OEM configurations.

However, once a mining machine system is found to have a 0day vulnerability, the attacker can use the vulnerability to obtain the modification permission and then reward the receiving address for tampering and then hijack the user's reward.

## 4.7 Penetration Attack

APT (Advanced Persistent Threat) is called a high-level persistent threat, and it is a targeted attack. It is a form of attack for long-term continuous cyber attacks on specific targets. APT needs to accurately collect the target business processes and target systems before launching an attack. In the process of collecting, this attack will actively exploit the vulnerabilities of the attacked system and the application, and use these vulnerabilities to build the network required by the attacker and jointly exploit the 0day vulnerability for attack.

## 4.8 Eclipse Attack

An eclipse attack is a network-level attack implemented by other nodes. The purpose of this attack is to prevent the latest block information from entering the attacked node and thus isolate the node.

An attacker can control a sufficient number of IP addresses to monopolize a valid connection between all victim nodes. The attacker can then requisition the victim's ability to mine and use it to attack the blockchain's consistency algorithm or for "repetitive payments and private mining", or trick the seller into handing the item to the attacker when the transaction has not yet been completed.

The eclipse attack can also attack the Ethereum contract by making the victim node unable to see the blockchain information, thus delaying the node to see the various parameters that may be used in the internal calculation of the smart contract, resulting in incorrect smart contract output, thus attacking Those can make a big profit.
    
# 5. Data Layer

## 5.1 Malicious Information Attack

Write malicious information, such as virus signatures, politically sensitive topics, etc., in the blockchain. With the undelete feature of blockchain data, it is difficult to delete information after it is written to the blockchain. If malicious information appears in the blockchain, it will be subject to various problems such as anti-virus software and political sensitivity.

## 5.2 Resource Abuse Attack

Over time, block data may explode (malicious interactions between nodes) and may grow linearly, depending on the design of the blockchain application and rely on existing computer storage technologies. If the block data grows explosively, it may cause the node to be unable to accommodate or slow down the blockchain, so that fewer and fewer stable nodes will be used. The fewer the nodes, the more centralized, causing the blockchain crisis.

Attack scene distance: If there is no corresponding operation limit in the chain, the attacker can block the entire blockchain by sending a large amount of spam, so that the real information in the blockchain can not be processed slowly, or the block can be made. The storage nodes in the chain are overloaded.

## 5.3 Collision Attack

Its attack principle is to weaken the characteristics of the algorithm by finding the weakness of the algorithm and disintegrating its strong anti-collision property, so that the hash function originally has to find two values ​​with the same value of different hashes for a long time. In a shorter time, one can find two values ​​with different values ​​but the same hash.

This means that User B can read and change User A's information, which undoubtedly poses a great security risk.

## 5.4 Length Extension Attack

This type of attack mainly acts on the hash function, which is based on the summary algorithm constructed by Merkle–Damgård. The principle is to derive the hash calculated by ciphertext and another message splicing in the case of known ciphertext hash and ciphertext length by algorithm weakness. (even if you don't know the entire contents of the original ciphertext)

## 5.5 Backdoor

This type of attack works on all open source encryption algorithm libraries. The RSA algorithm is the cornerstone of identity verification in the blockchain. The RSA algorithm itself is no problem, but in reality, people may choose to have others already written. Instead of implementing a set of encryption functions yourself, the "wheels" are used directly.

This brings up a problem. In the "wheels" that others have already written, it may be inserted into the backdoor. The typical case is that the NSA inserts the backdoor in the RSA algorithm, so that the attacker can directly calculate the private key through the public key.
