# The network
## Identities and Discovery
* each node has a single well-known identity
* Corda nodes discover each other via a network map service, which maps each well-known identity to an IP address
## Admission to the network
* Corda networks are semi-private
* to join, a node must obtain a certificate from the network operator. The certificate maps a well-known node identity to
  * a real-world legal identity
  * a public key
# The ledger
## Ledger data
* **no single central store of data**, each node maitains its own db (**vault**) of those facts that it is aware of
  * no peer is aware of the ledger in its entirety
  * not all on-ledger facts are shared. unshared fact is called **unilateral fact**
  * it's possible to broadcast a basic fact to all wished participants

<img src="https://docs.corda.net/en/images/ledger-venn.png" alt="facts" width="500"/>

# States
* is an **immutable** object representing a fact known by one or more Corda nodes at a specific point in time
* **state sequence** - when a stte is updated, create a new version of the state -> mark the existing state as historic
## The vault
* a database

<img src="https://docs.corda.net/en/images/vault-simple.png" alt="vault" width="500"/>

## Reference states
* used but not updated by other parties -> reference states

# Transactions
* Transactions update the ledger by marking zero or more existing ledger states as historic (inputs), and producing zero or more new ledger states (outputs)  

e.g. 2 inputs & 2 outputs  
<img src="https://docs.corda.net/en/images/basic-tx.png" alt="transaction" width="500"/>

* can include different state types (cash, bonds, etc)
* can be issuance (0 inputs) or exits (0 outputs)
* merge or split assets
* **atomic** - either all proposals accepted or none are
* two types
  * notary-change trx (change a state's notary)
  * general trx

## Transaction chains

<img src="https://docs.corda.net/en/images/tx-chain.png" alt="transaction-chain" width="500"/>

## Commiting transactions
* a transaction initially is just a **proposal** to update the ledger
* to become **reality** must receive signatures from all of the *required signers*
* trx's inputs marked as historic cannot be used in any future trx
* trx's outputs become current state

## Transaction validity
* transaction validity: 
  * *digitally signed* by all the required parties
  * *contractually valid*
* transaction uniqueness: no other committed trx exists that has cosumed any of the inputs of the proposed trx

## Reference states
* when a state is added to the references list of a trx, instead of the inputs or outputs, it's treated as a *ref state*
* 2 differences between regular states & ref states:
  * notary checks whether the ref states are current. However ref states are not consumed when trx is committed
  * the contracts for ref states are not executed

## Commands
* indicate transaction's intent
* associated with a list of one or more signers

<img src="https://docs.corda.net/en/images/commands.png" alt="transaction-commands" width="500"/>

## Attachments
* a large piece of data that can be reused across many trx, e.g. supporting legal docs, a table of currency codes
* ZIP/JAR files
* can be used when checking the trx's validity

## Time-window
* specifies the time period during which the trx can be committed

## Notary
* a notary pool is a network service that provides uniqueness consensus
  * for a given trx, inputs are not consumed by any signed trxs
* if the notary entity is abset, the trx is not notarized at all