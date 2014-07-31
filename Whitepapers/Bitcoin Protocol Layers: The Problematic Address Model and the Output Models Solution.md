####Bitcoin Protocol Layers: The Problematic Address Model and the Output Models Solution

Devon Armistead  
devonarmistead@gmail.com  
July 25, 2014  

**Abstract:**
> The model by which coins are stored and transacted on bitcoin protocol layers, namely Mastercoin and Counterparty, is subject to certain vulnerabilities that were revolutionarily solved by bitcoin. By associating coin quantities with bitcoin addresses instead of unspent transaction outputs, these protocols are made unnecessarily complicated, which threatens transaction security and renders coins on these layers non interoperable with bitcoin. This paper provides a framework by which these protocol layers can transition to a model in which coin quantities are associated with unspent transaction outputs. An output model inherits critical security features from bitcoin, and drastically improves basic protocol layer functions.

**Definitions:**

*Address model* - A model in which coins of bitcoin protocol layers are associated with bitcoin addresses. In a transaction, protocol specific rules must be followed in order to move the coins.

*Output model* - A model in which coins of bitcoin protocol layers are associated with transaction outputs. In a transaction, all coins represented by the input are spent. 

**Introduction:**  
It can be convenient to think of a bitcoin address as "having" the bitcoin sent to it, especially when familiarizing new users, similar to a bank account. This, however, can be misleading about the actual manner in which bitcoin is transacted. 

A typical bitcoin transaction output, P2PKH (pay-to-pubkey-hash), grants the person with the private key associated with the pubkey-hash the ability to send that output wherever he may wish. When he creates a transaction that proves that he has the right to spend that output, he may spend that output. If a person wants to spend multiple outputs in a single transaction, that’s no problem - but he must prove that he has the right to spend each individual output even if multiple outputs “come from the same address.” The significance of this is that multiple outputs to the same address are not pooled together, and thus there is no benefit (except maybe convenience) to receiving multiple payments to the same address.

When we frame this in the context of the address model, it’s suddenly apparent that there are two different models of coin storage going on here. The address model takes the familiar and convenient approach, considering an address a bank account, while bitcoin uses the novel concept of unspent outputs. It’s important to note that it is not wrong to use an address model, but its the burden of its proponents to demonstrate a reason to stray from the model proven reliable since 2009.

Instead of starting a discussion with the question, “should a bitcoin protocol layer switch from an address model to an output model?,” a baseline should be established for the feasibility of an address model. The reason for this is that the address model is an unprecedented concept, and should not be assumed to be reasonable without thorough analysis.

For example, we should consider that because the coins of an address model are not completely spent when some are spent, the address model often requires address reuse. Ignoring the fact that there is no benefit to reusing addresses with bitcoin transactions, reusing addresses is strongly discouraged due to privacy leaks and potential vulnerabilities.

**The relationship between Master Protocol and bitcoin:**  
The effects of using an address model are numerous. For instance, it’s possible to have coins in an address despite the bitcoin balance being 0. As a result we’ve created this interesting scenario in which bitcoin must be spent by the address to send the coins, however the transfer of those coins is not represented by the expenditure of the bitcoin spent in that transaction. This elucidates the relationship between protocols that use an address model and bitcoin. Address model protocols use bitcoin transactions to authenticate coin transfers by taking advantage of bitcoin’s public key encryption.

**On the challenge of Master Protocol Transactions:**  
The interesting thing about authenticating address model transactions with bitcoin transactions is the continued challenge that address models have with multisignature and P2SH (pay-to-script-hash) inputs and outputs. Despite the fact that bitcoin treats these different input and output types transparently, they don’t fit into the conceptual framework of address model protocols. To elaborate, bitcoin transactions only consider “how much is coming in and where is it going out?” whereas address model transactions must consider “what the input source is, and what the output destination is (as well as how much).” Since only addresses can have coins, where are the coins if they belong to m-of-n people? and where are the coins if they belong to the hash of a script?

**Resulting complication of transaction validation:**  
The result of this is the complication of the transaction validation process. It’s further exacerbated by the multitude of possible transaction types, ambiguity between a valid transaction and its double spend, and all possible fringe cases. As the complexity of the transaction validation process increases, as does the challenge of resolving a transaction to True or False.

**The effects of the address model:**  
The threat of this challenge is realized when we understand that an address model transaction is only secured by proof-of-work when the state of its validity can be proven at a block depth of 1 (i.e. at the moment of its first confirmation). The simple fact that a different interpretation of a transaction can lead to a different state of validity implies that there is no way to guarantee that the validity of a transaction between two arbitrary block heights did not change. Thus, the validity of address model transactions are not secured by proof-of-work.

(A digression: This leads to an interesting contrast in the role of interpretation between address model protocols and bitcoin. How an address model transaction is interpreted determines its validity, however the interpretation of a bitcoin transaction, even when displayed improperly, does not affect the validity of that transaction.)

The implication of this is that address model transactions are reversible, whether intentionally or unintentionally. This poses a real monetary risk when trading address model coins with bitcoins on the Master Protocol decentralized exchange. Since bitcoin transactions are irreversible and address model transactions are reversible, he who is sending bitcoin always carries the risk in the exchange, and is liable to lose the bitcoin sent to the other party as well as the coins he should have received in return. Because the validity of address model transactions are not secured by proof-of-work, the risk is carried by the recipient of those coins as long as he may have them. Of course, any number of address model transactions could change state of validity, however this is the only scenario in which there is a guaranteed monetary loss that cannot be reversed. 

It should also be pointed out that there are additional complications in address model coin trades with bitcoin that can result in irreversible loss of bitcoin. Since atomic trades are not possible, a time limit in blocks must be imposed - which when bitcoin payment is first confirmed after this limit is exceeded, the bitcoins have been irreversibly sent, but no coins are sent in return. Despite the intention to reduce this risk, the risk can only be mitigated, never eliminated.

**Conclusion:**  
The address model begins to emerge as a protocol implementation that has certain vulnerabilities. The transactions are not secured by proof-of-work, there is no inherent double spend protection, and there is no guaranteed state of transaction validity. 

The culmination of this exploration comes in the following statement: the vulnerabilities of address model protocols are specifically those that bitcoin is revolutionary for solving.

When considering the feasibility of a bitcoin protocol layer that uses an address model, it should be noted that it may be presumptuous to consider an address model protocol possible to be a bitcoin protocol layer. They are not interoperable with bitcoin, and share in common with bitcoin only a public database. The relevance of this can be significant depending on the various possible points of contact between an address model protocol and bitcoin i.e. the points at which a coin from one protocol is traded for that of another. 

**Solution:**  
Create a new transaction type that will convert Master Protocol tokens from the current address model to an output model. This transaction type is irreversible.

- All tokens of token type specified in data output are henceforth attached to the output that is sent to the reference address.
- A data output will include the currently valid chain of ownership from the exodus address all the way to the current transaction (merkle tree of tx hashes concatenated with current balance?)
- Outputs can only interact with outputs in all other Master Protocol  transactions, but never with an address (MSC on an output can never be sent to an address or exchanged with an address).

**The transaction model:**  
The transaction model is flexible and can be generalized by the following rules of transaction interpretation:
A transaction consumes the tokens represented by the input.
A transaction will send all tokens to the last output, unless otherwise specified by a data output.
If we spend an output that represents a quantity of tokens in a normal Bitcoin transaction, the tokens will typically be returned to the sender at a change address. By carefully crafting transactions, we can guarantee that an invalid transaction on this protocol layer will return all tokens to the sender. This basic mechanism of token transfer allows the protocol to stay in line with Bitcoin best practices by not reusing addresses, and provides a means by which the fungibility of an output is violated only insofar that it represents a quantity of tokens.
The data output specifies how the transaction is to be processed. The encoded data functions similarly to the data encoded in current Master Protocol transactions.
The Output model argument:  
The output model drastically reduces the ambiguity between valid and invalid transactions by reducing possible outcomes of a transaction. We only need to consider the question, “Where did the tokens go?” whereas other protocols require interpretation to answer these questions:
- Were tokens spent?
- How many were spent?
- Who were they sent to?
- Can that address receive tokens?
- Can the sender send tokens?
- If tokens weren’t sent, could it still be a valid transaction?
- How else can a transaction interact with the protocol?
- Under what circumstances does the protocol accept the senders input?
- Etc…

A core difference between the two methodologies is the manner in which actions or events can be organized on the blockchain. The utilization of an output model allows for the implicit movement of tokens and the explicit organization between untrusted parties via SIGHASH_ANYONECANPAY and SIGHASH_SINGLE transaction forms. In comparison, Master Protocol only use explicit movement of tokens, and thus must rely on a time or block count and fee specification to enable reasonably “safe” exchange of tokens for Bitcoin.
These unnecessary complications drastically increase the complexity of protocol specification and therefore affect ease of implementation, safety of interpretation, ability to scale, and overall transaction security.

**Appendix A - additional address model considerations:**

*Smart property:* One of the core features that bitcoin protocol layers, including Mastercoin, Counterparty and various colored coins implementations, provide is the creation of coins or tokens that represent something else. Thus far, the items represented by protocol layer coins have been relegated to the digital world. Though we must consider that the bitcoin ecosystem is in its infancy, and the protocol layers even younger, it’s important to look forward to the future and consider how the approaches of different protocol implementations affect a properties interaction with the physical world. The address model imposes resounding limitations due to the challenge in verifying transactions. Since address model coins aren’t “attached” to transaction outputs, the entire blockchain and internet access is, at a minimum, required for a smart object to verify ownership. This renders many smart object infeasible. For example, it’s not reasonable for smart door locks, smart cars, and smart phones to have these requirements - especially because internet connectivity is not universally available, cheap, or dependable. Another consideration is storage space. With the blockchain ever growing, it’s not feasible to create smart devices with enough storage space to store the blockchain for the indefinite future.

*Thin clients:* Since address model protocol layers require the entire blockchain to verify transactions, it’s not possible to develop a secure thin client like Electrum, Multibit, or mobile wallets.

*Cost to transact:* Address model protocols are currently limited to transferring a single type of coin from a single address to a single recipient. In a protocol layer transaction, the sender is not only paying the bitcoin transaction fee to the miners, but also a quantity of bitcoin for each data output (unless OP_RETURN is used), the exodus address (in the case of Mastercoin), and the recipient. For the average user this may not pose a significant cost in the day to day, however, the cost for businesses or heavy users may render the protocols unusable. Take for example a Mastercoin exchange. Instead of processing withdrawals in batches as they are able to do with bitcoin, each Mastercoin withdrawal requires its own transaction at a current cost of ~$0.20 USD.
