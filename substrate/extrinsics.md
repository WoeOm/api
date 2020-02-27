## Extrinsics

The following sections contain Extrinsics methods are part of the default Substrate runtime. On the api, these are exposed via `api.tx.<module>.<method>`. 

(NOTE: These were generated from a static/snapshot view of a recent Substrate master node. Some items may not be available in older nodes, or in any customized implementations.)

- **[authorship](#authorship)**

- **[balances](#balances)**

- **[contracts](#contracts)**

- **[council](#council)**

- **[democracy](#democracy)**

- **[elections](#elections)**

- **[finalityTracker](#finalitytracker)**

- **[grandpa](#grandpa)**

- **[identity](#identity)**

- **[imOnline](#imonline)**

- **[indices](#indices)**

- **[recovery](#recovery)**

- **[session](#session)**

- **[society](#society)**

- **[staking](#staking)**

- **[sudo](#sudo)**

- **[system](#system)**

- **[technicalCommittee](#technicalcommittee)**

- **[technicalMembership](#technicalmembership)**

- **[timestamp](#timestamp)**

- **[treasury](#treasury)**

- **[utility](#utility)**

- **[vesting](#vesting)**


___


## authorship
 
### setUncles(new_uncles: `Vec<T::Header>`)
- **interface**: api.tx.authorship.setUncles
- **summary**:   Provide a set of uncles. 

___


## balances
 
### forceTransfer(source: `<T::Lookup as StaticLookup>::Source`, dest: `<T::Lookup as StaticLookup>::Source`, value: `Compact<T::Balance>`)
- **interface**: api.tx.balances.forceTransfer
- **summary**:   Exactly as `transfer`, except the origin must be root and the source account may be specified. 
 
### setBalance(who: `<T::Lookup as StaticLookup>::Source`, new_free: `Compact<T::Balance>`, new_reserved: `Compact<T::Balance>`)
- **interface**: api.tx.balances.setBalance
- **summary**:   Set the balances of a given account. 

  This will alter `FreeBalance` and `ReservedBalance` in storage. it will also decrease the total issuance of the system (`TotalIssuance`). If the new free or reserved balance is below the existential deposit, it will reset the account nonce (`frame_system::AccountNonce`). 

  The dispatch origin for this call is `root`. 

  \# \<weight>

   

  - Independent of the arguments.

  - Contains a limited number of reads and writes.

  \# \</weight> 
 
### transfer(dest: `<T::Lookup as StaticLookup>::Source`, value: `Compact<T::Balance>`)
- **interface**: api.tx.balances.transfer
- **summary**:   Transfer some liquid free balance to another account. 

  `transfer` will set the `FreeBalance` of the sender and receiver. It will decrease the total issuance of the system by the `TransferFee`. If the sender's account is below the existential deposit as a result of the transfer, the account will be reaped. 

  The dispatch origin for this call must be `Signed` by the transactor. 

  \# \<weight>

   

  - Dependent on arguments but not critical, given proper implementations for  input config types. See related functions below. 

  - It contains a limited number of reads and writes internally and no complex computation.

  Related functions: 

    - `ensure_can_withdraw` is always called internally but has a bounded complexity. 

    - Transferring balances to accounts that did not exist before will cause     `T::OnNewAccount::on_new_account` to be called. 

    - Removing enough funds from an account will trigger `T::DustRemoval::on_unbalanced`.

    - `transfer_keep_alive` works the same way as `transfer`, but has an additional    check that the transfer will not kill the origin account. 

  

  \# \</weight> 
 
### transferKeepAlive(dest: `<T::Lookup as StaticLookup>::Source`, value: `Compact<T::Balance>`)
- **interface**: api.tx.balances.transferKeepAlive
- **summary**:   Same as the [`transfer`] call, but with a check that the transfer will not kill the origin account. 

  99% of the time you want [`transfer`] instead. 

  [`transfer`]: struct.Module.html#method.transfer 

___


## contracts
 
### call(dest: `<T::Lookup as StaticLookup>::Source`, value: `Compact<BalanceOf<T>>`, gas_limit: `Compact<Gas>`, data: `Vec<u8>`)
- **interface**: api.tx.contracts.call
- **summary**:   Makes a call to an account, optionally transferring some balance. 

  * If the account is a smart-contract account, the associated code will be executed and any value will be transferred. 

  * If the account is a regular account, any value will be transferred.

  * If no account exists and the call value is not less than `existential_deposit`,a regular account will be created and any value will be transferred. 
 
### claimSurcharge(dest: `T::AccountId`, aux_sender: `Option<T::AccountId>`)
- **interface**: api.tx.contracts.claimSurcharge
- **summary**:   Allows block producers to claim a small reward for evicting a contract. If a block producer fails to do so, a regular users will be allowed to claim the reward. 

  If contract is not evicted as a result of this call, no actions are taken and the sender is not eligible for the reward. 
 
### instantiate(endowment: `Compact<BalanceOf<T>>`, gas_limit: `Compact<Gas>`, code_hash: `CodeHash<T>`, data: `Vec<u8>`)
- **interface**: api.tx.contracts.instantiate
- **summary**:   Instantiates a new contract from the `codehash` generated by `put_code`, optionally transferring some balance. 

  Instantiation is executed as follows: 

  - The destination address is computed based on the sender and hash of the code. 

  - The smart-contract account is created at the computed address.

  - The `ctor_code` is executed in the context of the newly-created account. Buffer returned  after the execution is saved as the `code` of the account. That code will be invoked   upon any call received by this account. 

  - The contract is initialized.
 
### putCode(gas_limit: `Compact<Gas>`, code: `Vec<u8>`)
- **interface**: api.tx.contracts.putCode
- **summary**:   Stores the given binary Wasm code into the chain's storage and returns its `codehash`. You can instantiate contracts only with stored code. 
 
### updateSchedule(schedule: `Schedule`)
- **interface**: api.tx.contracts.updateSchedule
- **summary**:   Updates the schedule for metering contracts. 

  The schedule must have a greater version than the stored schedule. 

___


## council
 
### execute(proposal: `Box<<T as Trait<I>>::Proposal>`)
- **interface**: api.tx.council.execute
- **summary**:   Dispatch a proposal from a member using the `Member` origin. 

  Origin must be a member of the collective. 
 
### propose(threshold: `Compact<MemberCount>`, proposal: `Box<<T as Trait<I>>::Proposal>`)
- **interface**: api.tx.council.propose
- **summary**:   \# \<weight>

   

  - Bounded storage reads and writes.

  - Argument `threshold` has bearing on weight.

  \# \</weight> 
 
### setMembers(new_members: `Vec<T::AccountId>`)
- **interface**: api.tx.council.setMembers
- **summary**:   Set the collective's membership manually to `new_members`. Be nice to the chain and provide it pre-sorted. 

  Requires root origin. 
 
### vote(proposal: `T::Hash`, index: `Compact<ProposalIndex>`, approve: `bool`)
- **interface**: api.tx.council.vote
- **summary**:   \# \<weight>

   

  - Bounded storage read and writes.

  - Will be slightly heavier if the proposal is approved / disapproved after the vote.

  \# \</weight> 

___


## democracy
 
### cancelQueued(which: `ReferendumIndex`)
- **interface**: api.tx.democracy.cancelQueued
- **summary**:   Cancel a proposal queued for enactment. 
 
### cancelReferendum(ref_index: `Compact<ReferendumIndex>`)
- **interface**: api.tx.democracy.cancelReferendum
- **summary**:   Remove a referendum. 
 
### clearPublicProposals()
- **interface**: api.tx.democracy.clearPublicProposals
- **summary**:   Veto and blacklist the proposal hash. Must be from Root origin. 
 
### delegate(to: `T::AccountId`, conviction: `Conviction`)
- **interface**: api.tx.democracy.delegate
- **summary**:   Delegate vote. 

  \# \<weight>

   

  - One extra DB entry.

  \# \</weight> 
 
### emergencyCancel(ref_index: `ReferendumIndex`)
- **interface**: api.tx.democracy.emergencyCancel
- **summary**:   Schedule an emergency cancellation of a referendum. Cannot happen twice to the same referendum. 
 
### externalPropose(proposal_hash: `T::Hash`)
- **interface**: api.tx.democracy.externalPropose
- **summary**:   Schedule a referendum to be tabled once it is legal to schedule an external referendum. 
 
### externalProposeDefault(proposal_hash: `T::Hash`)
- **interface**: api.tx.democracy.externalProposeDefault
- **summary**:   Schedule a negative-turnout-bias referendum to be tabled next once it is legal to schedule an external referendum. 

  Unlike `external_propose`, blacklisting has no effect on this and it may replace a pre-scheduled `external_propose` call. 
 
### externalProposeMajority(proposal_hash: `T::Hash`)
- **interface**: api.tx.democracy.externalProposeMajority
- **summary**:   Schedule a majority-carries referendum to be tabled next once it is legal to schedule an external referendum. 

  Unlike `external_propose`, blacklisting has no effect on this and it may replace a pre-scheduled `external_propose` call. 
 
### fastTrack(proposal_hash: `T::Hash`, voting_period: `T::BlockNumber`, delay: `T::BlockNumber`)
- **interface**: api.tx.democracy.fastTrack
- **summary**:   Schedule the currently externally-proposed majority-carries referendum to be tabled immediately. If there is no externally-proposed referendum currently, or if there is one but it is not a majority-carries referendum then it fails. 

  - `proposal_hash`: The hash of the current external proposal. 

  - `voting_period`: The period that is allowed for voting on this proposal. Increased to  `EmergencyVotingPeriod` if too low. 

  - `delay`: The number of block after voting has ended in approval and this should be  enacted. This doesn't have a minimum amount. 
 
### noteImminentPreimage(encoded_proposal: `Vec<u8>`)
- **interface**: api.tx.democracy.noteImminentPreimage
- **summary**:   Register the preimage for an upcoming proposal. This requires the proposal to be in the dispatch queue. No deposit is needed. 
 
### notePreimage(encoded_proposal: `Vec<u8>`)
- **interface**: api.tx.democracy.notePreimage
- **summary**:   Register the preimage for an upcoming proposal. This doesn't require the proposal to be in the dispatch queue but does require a deposit, returned once enacted. 
 
### propose(proposal_hash: `T::Hash`, value: `Compact<BalanceOf<T>>`)
- **interface**: api.tx.democracy.propose
- **summary**:   Propose a sensitive action to be taken. 

  \# \<weight>

   

  - O(1).

  - Two DB changes, one DB entry.

  \# \</weight> 
 
### proxyVote(ref_index: `Compact<ReferendumIndex>`, vote: `Vote`)
- **interface**: api.tx.democracy.proxyVote
- **summary**:   Vote in a referendum on behalf of a stash. If `vote.is_aye()`, the vote is to enact the proposal; otherwise it is a vote to keep the status quo. 

  \# \<weight>

   

  - O(1).

  - One DB change, one DB entry.

  \# \</weight> 
 
### reapPreimage(proposal_hash: `T::Hash`)
- **interface**: api.tx.democracy.reapPreimage
- **summary**:   Remove an expired proposal preimage and collect the deposit. 

  This will only work after `VotingPeriod` blocks from the time that the preimage was noted, if it's the same account doing it. If it's a different account, then it'll only work an additional `EnactmentPeriod` later. 
 
### removeProxy(proxy: `T::AccountId`)
- **interface**: api.tx.democracy.removeProxy
- **summary**:   Clear the proxy. Called by the stash. 

  \# \<weight>

   

  - One DB clear.

  \# \</weight> 
 
### resignProxy()
- **interface**: api.tx.democracy.resignProxy
- **summary**:   Clear the proxy. Called by the proxy. 

  \# \<weight>

   

  - One DB clear.

  \# \</weight> 
 
### second(proposal: `Compact<PropIndex>`)
- **interface**: api.tx.democracy.second
- **summary**:   Propose a sensitive action to be taken. 

  \# \<weight>

   

  - O(1).

  - One DB entry.

  \# \</weight> 
 
### setProxy(proxy: `T::AccountId`)
- **interface**: api.tx.democracy.setProxy
- **summary**:   Specify a proxy. Called by the stash. 

  \# \<weight>

   

  - One extra DB entry.

  \# \</weight> 
 
### undelegate()
- **interface**: api.tx.democracy.undelegate
- **summary**:   Undelegate vote. 

  \# \<weight>

   

  - O(1).

  \# \</weight> 
 
### unlock(target: `T::AccountId`)
- **interface**: api.tx.democracy.unlock
 
### vetoExternal(proposal_hash: `T::Hash`)
- **interface**: api.tx.democracy.vetoExternal
- **summary**:   Veto and blacklist the external proposal hash. 
 
### vote(ref_index: `Compact<ReferendumIndex>`, vote: `Vote`)
- **interface**: api.tx.democracy.vote
- **summary**:   Vote in a referendum. If `vote.is_aye()`, the vote is to enact the proposal; otherwise it is a vote to keep the status quo. 

  \# \<weight>

   

  - O(1).

  - One DB change, one DB entry.

  \# \</weight> 

___


## elections
 
### removeMember(who: `<T::Lookup as StaticLookup>::Source`)
- **interface**: api.tx.elections.removeMember
- **summary**:   Remove a particular member from the set. This is effective immediately and the bond of the outgoing member is slashed. 

  If a runner-up is available, then the best runner-up will be removed and replaces the outgoing member. Otherwise, a new phragmen round is started. 

  Note that this does not affect the designated block number of the next election. 

  \# \<weight>

   #### State Reads: O(do_phragmen) Writes: O(do_phragmen) 

  \# \</weight> 
 
### removeVoter()
- **interface**: api.tx.elections.removeVoter
- **summary**:   Remove `origin` as a voter. This removes the lock and returns the bond. 

  \# \<weight>

   #### State Reads: O(1) Writes: O(1) 

  \# \</weight> 
 
### renounceCandidacy()
- **interface**: api.tx.elections.renounceCandidacy
- **summary**:   Renounce one's intention to be a candidate for the next election round. 3 potential outcomes exist: 

  - `origin` is a candidate and not elected in any set. In this case, the bond is  unreserved, returned and origin is removed as a candidate. 

  - `origin` is a current runner up. In this case, the bond is unreserved, returned and  origin is removed as a runner. 

  - `origin` is a current member. In this case, the bond is unreserved and origin is  removed as a member, consequently not being a candidate for the next round anymore.   Similar to [`remove_voter`], if replacement runners exists, they are immediately used. 
 
### reportDefunctVoter(target: `<T::Lookup as StaticLookup>::Source`)
- **interface**: api.tx.elections.reportDefunctVoter
- **summary**:   Report `target` for being an defunct voter. In case of a valid report, the reporter is rewarded by the bond amount of `target`. Otherwise, the reporter itself is removed and their bond is slashed. 

  A defunct voter is defined to be: 

    - a voter whose current submitted votes are all invalid. i.e. all of them are no    longer a candidate nor an active member. 

  \# \<weight>

   #### State Reads: O(NLogM) given M current candidates and N votes for `target`. Writes: O(1) 

  \# \</weight> 
 
### submitCandidacy()
- **interface**: api.tx.elections.submitCandidacy
- **summary**:   Submit oneself for candidacy. 

  A candidate will either: 

    - Lose at the end of the term and forfeit their deposit.

    - Win and become a member. Members will eventually get their stash back.

    - Become a runner-up. Runners-ups are reserved members in case one gets forcefully    removed. 

  \# \<weight>

   #### State Reads: O(LogN) Given N candidates. Writes: O(1) 

  \# \</weight> 
 
### vote(votes: `Vec<T::AccountId>`, value: `Compact<BalanceOf<T>>`)
- **interface**: api.tx.elections.vote
- **summary**:   Vote for a set of candidates for the upcoming round of election. 

  The `votes` should: 

    - not be empty.

    - be less than the number of candidates.

  Upon voting, `value` units of `who`'s balance is locked and a bond amount is reserved. It is the responsibility of the caller to not place all of their balance into the lock and keep some for further transactions. 

  \# \<weight>

   #### State Reads: O(1) Writes: O(V) given `V` votes. V is bounded by 16. 

  \# \</weight> 

___


## finalityTracker
 
### finalHint(hint: `Compact<T::BlockNumber>`)
- **interface**: api.tx.finalityTracker.finalHint
- **summary**:   Hint that the author of this block thinks the best finalized block is the given number. 

___


## grandpa
 
### reportMisbehavior(_report: `Vec<u8>`)
- **interface**: api.tx.grandpa.reportMisbehavior
- **summary**:   Report some misbehavior. 

___


## identity
 
### addRegistrar(account: `T::AccountId`)
- **interface**: api.tx.identity.addRegistrar
- **summary**:   Add a registrar to the system. 

  The dispatch origin for this call must be `RegistrarOrigin` or `Root`. 

  - `account`: the account of the registrar. 

  Emits `RegistrarAdded` if successful. 

  \# \<weight>

   

  - `O(R)` where `R` registrar-count (governance-bounded).

  - One storage mutation (codec `O(R)`).

  - One event.

  \# \</weight> 
 
### cancelRequest(reg_index: `RegistrarIndex`)
- **interface**: api.tx.identity.cancelRequest
- **summary**:   Cancel a previous request. 

  Payment: A previously reserved deposit is returned on success. 

  The dispatch origin for this call must be _Signed_ and the sender must have a registered identity. 

  - `reg_index`: The index of the registrar whose judgement is no longer requested. 

  Emits `JudgementUnrequested` if successful. 

  \# \<weight>

   

  - `O(R + X)`.

  - One balance-reserve operation.

  - One storage mutation `O(R + X)`.

  - One event.

  \# \</weight> 
 
### clearIdentity()
- **interface**: api.tx.identity.clearIdentity
- **summary**:   Clear an account's identity info and all sub-account and return all deposits. 

  Payment: All reserved balances on the account are returned. 

  The dispatch origin for this call must be _Signed_ and the sender must have a registered identity. 

  Emits `IdentityCleared` if successful. 

  \# \<weight>

   

  - `O(R + S + X)`.

  - One balance-reserve operation.

  - `S + 2` storage deletions.

  - One event.

  \# \</weight> 
 
### killIdentity(target: `<T::Lookup as StaticLookup>::Source`)
- **interface**: api.tx.identity.killIdentity
- **summary**:   Remove an account's identity and sub-account information and slash the deposits. 

  Payment: Reserved balances from `set_subs` and `set_identity` are slashed and handled by `Slash`. Verification request deposits are not returned; they should be cancelled manually using `cancel_request`. 

  The dispatch origin for this call must be _Root_ or match `T::ForceOrigin`. 

  - `target`: the account whose identity the judgement is upon. This must be an account   with a registered identity. 

  Emits `IdentityKilled` if successful. 

  \# \<weight>

   

  - `O(R + S + X)`.

  - One balance-reserve operation.

  - `S + 2` storage mutations.

  - One event.

  \# \</weight> 
 
### provideJudgement(reg_index: `Compact<RegistrarIndex>`, target: `<T::Lookup as StaticLookup>::Source`, judgement: `Judgement<BalanceOf<T>>`)
- **interface**: api.tx.identity.provideJudgement
- **summary**:   Provide a judgement for an account's identity. 

  The dispatch origin for this call must be _Signed_ and the sender must be the account of the registrar whose index is `reg_index`. 

  - `reg_index`: the index of the registrar whose judgement is being made. 

  - `target`: the account whose identity the judgement is upon. This must be an account  with a registered identity. 

  - `judgement`: the judgement of the registrar of index `reg_index` about `target`.

  Emits `JudgementGiven` if successful. 

  \# \<weight>

   

  - `O(R + X)`.

  - One balance-transfer operation.

  - Up to one account-lookup operation.

  - Storage: 1 read `O(R)`, 1 mutate `O(R + X)`.

  - One event.

  \# \</weight> 
 
### requestJudgement(reg_index: `Compact<RegistrarIndex>`, max_fee: `Compact<BalanceOf<T>>`)
- **interface**: api.tx.identity.requestJudgement
- **summary**:   Request a judgement from a registrar. 

  Payment: At most `max_fee` will be reserved for payment to the registrar if judgement given. 

  The dispatch origin for this call must be _Signed_ and the sender must have a registered identity. 

  - `reg_index`: The index of the registrar whose judgement is requested. 

  - `max_fee`: The maximum fee that may be paid. This should just be auto-populated as:

  ```nocompile Self::registrars(reg_index).uwnrap().fee ``` 

  Emits `JudgementRequested` if successful. 

  \# \<weight>

   

  - `O(R + X)`.

  - One balance-reserve operation.

  - Storage: 1 read `O(R)`, 1 mutate `O(X + R)`.

  - One event.

  \# \</weight> 
 
### setAccountId(index: `Compact<RegistrarIndex>`, new: `T::AccountId`)
- **interface**: api.tx.identity.setAccountId
- **summary**:   Change the account associated with a registrar. 

  The dispatch origin for this call must be _Signed_ and the sender must be the account of the registrar whose index is `index`. 

  - `index`: the index of the registrar whose fee is to be set. 

  - `new`: the new account ID.

  \# \<weight>

   

  - `O(R)`.

  - One storage mutation `O(R)`.

  \# \</weight> 
 
### setFee(index: `Compact<RegistrarIndex>`, fee: `Compact<BalanceOf<T>>`)
- **interface**: api.tx.identity.setFee
- **summary**:   Set the fee required for a judgement to be requested from a registrar. 

  The dispatch origin for this call must be _Signed_ and the sender must be the account of the registrar whose index is `index`. 

  - `index`: the index of the registrar whose fee is to be set. 

  - `fee`: the new fee.

  \# \<weight>

   

  - `O(R)`.

  - One storage mutation `O(R)`.

  \# \</weight> 
 
### setFields(index: `Compact<RegistrarIndex>`, fields: `IdentityFields`)
- **interface**: api.tx.identity.setFields
- **summary**:   Set the field information for a registrar. 

  The dispatch origin for this call must be _Signed_ and the sender must be the account of the registrar whose index is `index`. 

  - `index`: the index of the registrar whose fee is to be set. 

  - `fields`: the fields that the registrar concerns themselves with.

  \# \<weight>

   

  - `O(R)`.

  - One storage mutation `O(R)`.

  \# \</weight> 
 
### setIdentity(info: `IdentityInfo`)
- **interface**: api.tx.identity.setIdentity
- **summary**:   Set an account's identity information and reserve the appropriate deposit. 

  If the account already has identity information, the deposit is taken as part payment for the new deposit. 

  The dispatch origin for this call must be _Signed_ and the sender must have a registered identity. 

  - `info`: The identity information. 

  Emits `IdentitySet` if successful. 

  \# \<weight>

   

  - `O(X + X' + R)` where `X` additional-field-count (deposit-bounded and code-bounded).

  - At most two balance operations.

  - One storage mutation (codec-read `O(X' + R)`, codec-write `O(X + R)`).

  - One event.

  \# \</weight> 
 
### setSubs(subs: `Vec<(T::AccountId, Data)>`)
- **interface**: api.tx.identity.setSubs
- **summary**:   Set the sub-accounts of the sender. 

  Payment: Any aggregate balance reserved by previous `set_subs` calls will be returned and an amount `SubAccountDeposit` will be reserved for each item in `subs`. 

  The dispatch origin for this call must be _Signed_ and the sender must have a registered identity. 

  - `subs`: The identity's sub-accounts. 

  \# \<weight>

   

  - `O(S)` where `S` subs-count (hard- and deposit-bounded).

  - At most two balance operations.

  - At most O(2 * S + 1) storage mutations; codec complexity `O(1 * S + S * 1)`);  one storage-exists. 

  \# \</weight> 

___


## imOnline
 
### heartbeat(heartbeat: `Heartbeat<T::BlockNumber>`, _signature: `<T::AuthorityId as RuntimeAppPublic>::Signature`)
- **interface**: api.tx.imOnline.heartbeat

___


## indices
 
### claim(index: `T::AccountIndex`)
- **interface**: api.tx.indices.claim
- **summary**:   Assign an previously unassigned index. 

  Payment: `Deposit` is reserved from the sender account. 

  The dispatch origin for this call must be _Signed_. 

  - `index`: the index to be claimed. This must not be in use. 

  Emits `IndexAssigned` if successful. 

  \# \<weight>

   

  - `O(1)`.

  - One storage mutation (codec `O(1)`).

  - One reserve operation.

  - One event.

  \# \</weight> 
 
### forceTransfer(new: `T::AccountId`, index: `T::AccountIndex`)
- **interface**: api.tx.indices.forceTransfer
- **summary**:   Force an index to an account. This doesn't require a deposit. If the index is already held, then any deposit is reimbursed to its current owner. 

  The dispatch origin for this call must be _Root_. 

  - `index`: the index to be (re-)assigned. 

  - `new`: the new owner of the index. This function is a no-op if it is equal to sender.

  Emits `IndexAssigned` if successful. 

  \# \<weight>

   

  - `O(1)`.

  - One storage mutation (codec `O(1)`).

  - Up to one reserve operation.

  - One event.

  \# \</weight> 
 
### free(index: `T::AccountIndex`)
- **interface**: api.tx.indices.free
- **summary**:   Free up an index owned by the sender. 

  Payment: Any previous deposit placed for the index is unreserved in the sender account. 

  The dispatch origin for this call must be _Signed_ and the sender must own the index. 

  - `index`: the index to be freed. This must be owned by the sender. 

  Emits `IndexFreed` if successful. 

  \# \<weight>

   

  - `O(1)`.

  - One storage mutation (codec `O(1)`).

  - One reserve operation.

  - One event.

  \# \</weight> 
 
### transfer(new: `T::AccountId`, index: `T::AccountIndex`)
- **interface**: api.tx.indices.transfer
- **summary**:   Assign an index already owned by the sender to another account. The balance reservation is effectively transfered to the new account. 

  The dispatch origin for this call must be _Signed_. 

  - `index`: the index to be re-assigned. This must be owned by the sender. 

  - `new`: the new owner of the index. This function is a no-op if it is equal to sender.

  Emits `IndexAssigned` if successful. 

  \# \<weight>

   

  - `O(1)`.

  - One storage mutation (codec `O(1)`).

  - One transfer operation.

  - One event.

  \# \</weight> 

___


## recovery
 
### asRecovered(account: `T::AccountId`, call: `Box<<T as Trait>::Call>`)
- **interface**: api.tx.recovery.asRecovered
- **summary**:   Send a call through a recovered account. 

  The dispatch origin for this call must be _Signed_ and registered to be able to make calls on behalf of the recovered account. 

  Parameters: 

  - `account`: The recovered account you want to make a call on-behalf-of.

  - `call`: The call you want to make with the recovered account.

  \# \<weight>

   

  - The weight of the `call`.

  - One storage lookup to check account is recovered by `who`. O(1)

  \# \</weight> 
 
### claimRecovery(account: `T::AccountId`)
- **interface**: api.tx.recovery.claimRecovery
- **summary**:   Allow a successful rescuer to claim their recovered account. 

  The dispatch origin for this call must be _Signed_ and must be a "rescuer" who has successfully completed the account recovery process: collected `threshold` or more vouches, waited `delay_period` blocks since initiation. 

  Parameters: 

  - `account`: The lost account that you want to claim has been successfully  recovered by you. 

  \# \<weight>

   Key: F (len of friends in config), V (len of vouching friends) 

  - One storage read to get the recovery configuration. O(1), Codec O(F)

  - One storage read to get the active recovery process. O(1), Codec O(V)

  - One storage read to get the current block number. O(1)

  - One storage write. O(1), Codec O(V).

  - One event.

  Total Complexity: O(F + V) 

  \# \</weight> 
 
### closeRecovery(rescuer: `T::AccountId`)
- **interface**: api.tx.recovery.closeRecovery
- **summary**:   As the controller of a recoverable account, close an active recovery process for your account. 

  Payment: By calling this function, the recoverable account will receive the recovery deposit `RecoveryDeposit` placed by the rescuer. 

  The dispatch origin for this call must be _Signed_ and must be a recoverable account with an active recovery process for it. 

  Parameters: 

  - `rescuer`: The account trying to rescue this recoverable account.

  \# \<weight>

   Key: V (len of vouching friends) 

  - One storage read/remove to get the active recovery process. O(1), Codec O(V)

  - One balance call to repatriate reserved. O(X)

  - One event.

  Total Complexity: O(V + X) 

  \# \</weight> 
 
### createRecovery(friends: `Vec<T::AccountId>`, threshold: `u16`, delay_period: `T::BlockNumber`)
- **interface**: api.tx.recovery.createRecovery
- **summary**:   Create a recovery configuration for your account. This makes your account recoverable. 

  Payment: `ConfigDepositBase` + `FriendDepositFactor` * #_of_friends balance will be reserved for storing the recovery configuration. This deposit is returned in full when the user calls `remove_recovery`. 

  The dispatch origin for this call must be _Signed_. 

  Parameters: 

  - `friends`: A list of friends you trust to vouch for recovery attempts.  Should be ordered and contain no duplicate values. 

  - `threshold`: The number of friends that must vouch for a recovery attempt  before the account can be recovered. Should be less than or equal to   the length of the list of friends. 

  - `delay_period`: The number of blocks after a recovery attempt is initialized  that needs to pass before the account can be recovered. 

  \# \<weight>

   

  - Key: F (len of friends)

  - One storage read to check that account is not already recoverable. O(1).

  - A check that the friends list is sorted and unique. O(F)

  - One currency reserve operation. O(X)

  - One storage write. O(1). Codec O(F).

  - One event.

  Total Complexity: O(F + X) 

  \# \</weight> 
 
### initiateRecovery(account: `T::AccountId`)
- **interface**: api.tx.recovery.initiateRecovery
- **summary**:   Initiate the process for recovering a recoverable account. 

  Payment: `RecoveryDeposit` balance will be reserved for initiating the recovery process. This deposit will always be repatriated to the account trying to be recovered. See `close_recovery`. 

  The dispatch origin for this call must be _Signed_. 

  Parameters: 

  - `account`: The lost account that you want to recover. This account  needs to be recoverable (i.e. have a recovery configuration). 

  \# \<weight>

   

  - One storage read to check that account is recoverable. O(F)

  - One storage read to check that this recovery process hasn't already started. O(1)

  - One currency reserve operation. O(X)

  - One storage read to get the current block number. O(1)

  - One storage write. O(1).

  - One event.

  Total Complexity: O(F + X) 

  \# \</weight> 
 
### removeRecovery()
- **interface**: api.tx.recovery.removeRecovery
- **summary**:   Remove the recovery process for your account. 

  NOTE: The user must make sure to call `close_recovery` on all active recovery attempts before calling this function else it will fail. 

  Payment: By calling this function the recoverable account will unreserve their recovery configuration deposit. (`ConfigDepositBase` + `FriendDepositFactor` * #_of_friends) 

  The dispatch origin for this call must be _Signed_ and must be a recoverable account (i.e. has a recovery configuration). 

  \# \<weight>

   Key: F (len of friends) 

  - One storage read to get the prefix iterator for active recoveries. O(1)

  - One storage read/remove to get the recovery configuration. O(1), Codec O(F)

  - One balance call to unreserved. O(X)

  - One event.

  Total Complexity: O(F + X) 

  \# \</weight> 
 
### setRecovered(lost: `T::AccountId`, rescuer: `T::AccountId`)
- **interface**: api.tx.recovery.setRecovered
- **summary**:   Allow ROOT to bypass the recovery process and set an a rescuer account for a lost account directly. 

  The dispatch origin for this call must be _ROOT_. 

  Parameters: 

  - `lost`: The "lost account" to be recovered.

  - `rescuer`: The "rescuer account" which can call as the lost account.

  \# \<weight>

   

  - One storage write O(1)

  - One event

  \# \</weight> 
 
### vouchRecovery(lost: `T::AccountId`, rescuer: `T::AccountId`)
- **interface**: api.tx.recovery.vouchRecovery
- **summary**:   Allow a "friend" of a recoverable account to vouch for an active recovery process for that account. 

  The dispatch origin for this call must be _Signed_ and must be a "friend" for the recoverable account. 

  Parameters: 

  - `lost`: The lost account that you want to recover.

  - `rescuer`: The account trying to rescue the lost account that you  want to vouch for. 

  The combination of these two parameters must point to an active recovery process. 

  \# \<weight>

   Key: F (len of friends in config), V (len of vouching friends) 

  - One storage read to get the recovery configuration. O(1), Codec O(F)

  - One storage read to get the active recovery process. O(1), Codec O(V)

  - One binary search to confirm caller is a friend. O(logF)

  - One binary search to confirm caller has not already vouched. O(logV)

  - One storage write. O(1), Codec O(V).

  - One event.

  Total Complexity: O(F + logF + V + logV) 

  \# \</weight> 

___


## session
 
### setKeys(keys: `T::Keys`, proof: `Vec<u8>`)
- **interface**: api.tx.session.setKeys
- **summary**:   Sets the session key(s) of the function caller to `keys`. Allows an account to set its session key prior to becoming a validator. This doesn't take effect until the next session. 

  The dispatch origin of this function must be signed. 

  \# \<weight>

   

  - O(log n) in number of accounts.

  - One extra DB entry.

  \# \</weight> 

___


## society
 
### bid(value: `BalanceOf<T, I>`)
- **interface**: api.tx.society.bid
- **summary**:   A user outside of the society can make a bid for entry. 

  Payment: `CandidateDeposit` will be reserved for making a bid. It is returned when the bid becomes a member, or if the bid calls `unbid`. 

  The dispatch origin for this call must be _Signed_. 

  Parameters: 

  - `value`: A one time payment the bid would like to receive when joining the society.

  \# \<weight>

   Key: B (len of bids), C (len of candidates), M (len of members), X (balance reserve) 

  - Storage Reads:

  	- One storage read to check for suspended candidate. O(1)

  	- One storage read to check for suspended member. O(1)

  	- One storage read to retrieve all current bids. O(B)

  	- One storage read to retrieve all current candidates. O(C)

  	- One storage read to retrieve all members. O(M)

  - Storage Writes:

  	- One storage mutate to add a new bid to the vector O(B) (TODO: possible optimization w/ read)

  	- Up to one storage removal if bid.len() > MAX_BID_COUNT. O(1)

  - Notable Computation:

  	- O(B + C + log M) search to check user is not already a part of society.

  	- O(log B) search to insert the new bid sorted.

  - External Module Operations:

  	- One balance reserve operation. O(X)

  	- Up to one balance unreserve operation if bids.len() > MAX_BID_COUNT.

  - Events:

  	- One event for new bid.

  	- Up to one event for AutoUnbid if bid.len() > MAX_BID_COUNT.

  Total Complexity: O(M + B + C + logM + logB + X) 

  \# \</weight> 
 
### defenderVote(approve: `bool`)
- **interface**: api.tx.society.defenderVote
- **summary**:   As a member, vote on the defender. 

  The dispatch origin for this call must be _Signed_ and a member. 

  Parameters: 

  - `approve`: A boolean which says if the candidate should beapproved (`true`) or rejected (`false`). 

  \# \<weight>

   

  - Key: M (len of members)

  - One storage read O(M) and O(log M) search to check user is a member.

  - One storage write to add vote to votes. O(1)

  - One event.

  Total Complexity: O(M + logM) 

  \# \</weight> 
 
### found(founder: `T::AccountId`, max_members: `u32`, rules: `Vec<u8>`)
- **interface**: api.tx.society.found
- **summary**:   Found the society. 

  This is done as a discrete action in order to allow for the module to be included into a running chain and can only be done once. 

  The dispatch origin for this call must be from the _FounderSetOrigin_. 

  Parameters: 

  - `founder` - The first member and head of the newly founded society.

  - `max_members` - The initial max number of members for the society.

  - `rules` - The rules of this society concerning membership.

  \# \<weight>

   

  - Two storage mutates to set `Head` and `Founder`. O(1)

  - One storage write to add the first member to society. O(1)

  - One event.

  Total Complexity: O(1) 

  \# \</weight> 
 
### judgeSuspendedCandidate(who: `T::AccountId`, judgement: `Judgement`)
- **interface**: api.tx.society.judgeSuspendedCandidate
- **summary**:   Allow suspended judgement origin to make judgement on a suspended candidate. 

  If the judgement is `Approve`, we add them to society as a member with the appropriate payment for joining society. 

  If the judgement is `Reject`, we either slash the deposit of the bid, giving it back to the society treasury, or we ban the voucher from vouching again. 

  If the judgement is `Rebid`, we put the candidate back in the bid pool and let them go through the induction process again. 

  The dispatch origin for this call must be from the _SuspensionJudgementOrigin_. 

  Parameters: 

  - `who` - The suspended candidate to be judged.

  - `judgement` - `Approve`, `Reject`, or `Rebid`.

  \# \<weight>

   Key: B (len of bids), M (len of members), X (balance action) 

  - One storage read to check `who` is a suspended candidate.

  - One storage removal of the suspended candidate.

  - Approve Logic

  	- One storage read to get the available pot to pay users with. O(1)

  	- One storage write to update the available pot. O(1)

  	- One storage read to get the current block number. O(1)

  	- One storage read to get all members. O(M)

  	- Up to one unreserve currency action.

  	- Up to two new storage writes to payouts.

  	- Up to one storage write with O(log M) binary search to add a member to society.

  - Reject Logic

  	- Up to one repatriate reserved currency action. O(X)

  	- Up to one storage write to ban the vouching member from vouching again.

  - Rebid Logic

  	- Storage mutate with O(log B) binary search to place the user back into bids.

  - Up to one additional event if unvouch takes place.

  - One storage removal.

  - One event for the judgement.

  Total Complexity: O(M + logM + B + X) 

  \# \</weight> 
 
### judgeSuspendedMember(who: `T::AccountId`, forgive: `bool`)
- **interface**: api.tx.society.judgeSuspendedMember
- **summary**:   Allow suspension judgement origin to make judgement on a suspended member. 

  If a suspended member is forgiven, we simply add them back as a member, not affecting any of the existing storage items for that member. 

  If a suspended member is rejected, remove all associated storage items, including their payouts, and remove any vouched bids they currently have. 

  The dispatch origin for this call must be from the _SuspensionJudgementOrigin_. 

  Parameters: 

  - `who` - The suspended member to be judged.

  - `forgive` - A boolean representing whether the suspension judgement origin              forgives (`true`) or rejects (`false`) a suspended member. 

  \# \<weight>

   Key: B (len of bids), M (len of members) 

  - One storage read to check `who` is a suspended member. O(1)

  - Up to one storage write O(M) with O(log M) binary search to add a member back to society.

  - Up to 3 storage removals O(1) to clean up a removed member.

  - Up to one storage write O(B) with O(B) search to remove vouched bid from bids.

  - Up to one additional event if unvouch takes place.

  - One storage removal. O(1)

  - One event for the judgement.

  Total Complexity: O(M + logM + B) 

  \# \</weight> 
 
### payout()
- **interface**: api.tx.society.payout
- **summary**:   Transfer the first matured payout for the sender and remove it from the records. 

  NOTE: This extrinsic needs to be called multiple times to claim multiple matured payouts. 

  Payment: The member will receive a payment equal to their first matured payout to their free balance. 

  The dispatch origin for this call must be _Signed_ and a member with payouts remaining. 

  \# \<weight>

   Key: M (len of members), P (number of payouts for a particular member) 

  - One storage read O(M) and O(log M) search to check signer is a member.

  - One storage read O(P) to get all payouts for a member.

  - One storage read O(1) to get the current block number.

  - One currency transfer call. O(X)

  - One storage write or removal to update the member's payouts. O(P)

  Total Complexity: O(M + logM + P + X) 

  \# \</weight> 
 
### setMaxMembers(max: `u32`)
- **interface**: api.tx.society.setMaxMembers
- **summary**:   Allows root origin to change the maximum number of members in society. Max membership count must be greater than 1. 

  The dispatch origin for this call must be from _ROOT_. 

  Parameters: 

  - `max` - The maximum number of members for the society.

  \# \<weight>

   

  - One storage write to update the max. O(1)

  - One event.

  Total Complexity: O(1) 

  \# \</weight> 
 
### unbid(pos: `u32`)
- **interface**: api.tx.society.unbid
- **summary**:   A bidder can remove their bid for entry into society. By doing so, they will have their candidate deposit returned or they will unvouch their voucher. 

  Payment: The bid deposit is unreserved if the user made a bid. 

  The dispatch origin for this call must be _Signed_ and a bidder. 

  Parameters: 

  - `pos`: Position in the `Bids` vector of the bid who wants to unbid.

  \# \<weight>

   Key: B (len of bids), X (balance unreserve) 

  - One storage read and write to retrieve and update the bids. O(B)

  - Either one unreserve balance action O(X) or one vouching storage removal. O(1)

  - One event.

  Total Complexity: O(B + X) 

  \# \</weight> 
 
### unfound()
- **interface**: api.tx.society.unfound
- **summary**:   Anull the founding of the society. 

  The dispatch origin for this call must be Signed, and the signing account must be both the `Founder` and the `Head`. This implies that it may only be done when there is one member. 

  \# \<weight>

   

  - Two storage reads O(1).

  - Four storage removals O(1).

  - One event.

  Total Complexity: O(1) 

  \# \</weight> 
 
### unvouch(pos: `u32`)
- **interface**: api.tx.society.unvouch
- **summary**:   As a vouching member, unvouch a bid. This only works while vouched user is only a bidder (and not a candidate). 

  The dispatch origin for this call must be _Signed_ and a vouching member. 

  Parameters: 

  - `pos`: Position in the `Bids` vector of the bid who should be unvouched.

  \# \<weight>

   Key: B (len of bids) 

  - One storage read O(1) to check the signer is a vouching member.

  - One storage mutate to retrieve and update the bids. O(B)

  - One vouching storage removal. O(1)

  - One event.

  Total Complexity: O(B) 

  \# \</weight> 
 
### vote(candidate: `<T::Lookup as StaticLookup>::Source`, approve: `bool`)
- **interface**: api.tx.society.vote
- **summary**:   As a member, vote on a candidate. 

  The dispatch origin for this call must be _Signed_ and a member. 

  Parameters: 

  - `candidate`: The candidate that the member would like to bid on.

  - `approve`: A boolean which says if the candidate should be             approved (`true`) or rejected (`false`). 

  \# \<weight>

   Key: C (len of candidates), M (len of members) 

  - One storage read O(M) and O(log M) search to check user is a member.

  - One account lookup.

  - One storage read O(C) and O(C) search to check that user is a candidate.

  - One storage write to add vote to votes. O(1)

  - One event.

  Total Complexity: O(M + logM + C) 

  \# \</weight> 
 
### vouch(who: `T::AccountId`, value: `BalanceOf<T, I>`, tip: `BalanceOf<T, I>`)
- **interface**: api.tx.society.vouch
- **summary**:   As a member, vouch for someone to join society by placing a bid on their behalf. 

  There is no deposit required to vouch for a new bid, but a member can only vouch for one bid at a time. If the bid becomes a suspended candidate and ultimately rejected by the suspension judgement origin, the member will be banned from vouching again. 

  As a vouching member, you can claim a tip if the candidate is accepted. This tip will be paid as a portion of the reward the member will receive for joining the society. 

  The dispatch origin for this call must be _Signed_ and a member. 

  Parameters: 

  - `who`: The user who you would like to vouch for.

  - `value`: The total reward to be paid between you and the candidate if they becomea member in the society. 

  - `tip`: Your cut of the total `value` payout when the candidate is inducted intothe society. Tips larger than `value` will be saturated upon payout. 

  \# \<weight>

   Key: B (len of bids), C (len of candidates), M (len of members) 

  - Storage Reads:

  	- One storage read to retrieve all members. O(M)

  	- One storage read to check member is not already vouching. O(1)

  	- One storage read to check for suspended candidate. O(1)

  	- One storage read to check for suspended member. O(1)

  	- One storage read to retrieve all current bids. O(B)

  	- One storage read to retrieve all current candidates. O(C)

  - Storage Writes:

  	- One storage write to insert vouching status to the member. O(1)

  	- One storage mutate to add a new bid to the vector O(B) (TODO: possible optimization w/ read)

  	- Up to one storage removal if bid.len() > MAX_BID_COUNT. O(1)

  - Notable Computation:

  	- O(log M) search to check sender is a member.

  	- O(B + C + log M) search to check user is not already a part of society.

  	- O(log B) search to insert the new bid sorted.

  - External Module Operations:

  	- One balance reserve operation. O(X)

  	- Up to one balance unreserve operation if bids.len() > MAX_BID_COUNT.

  - Events:

  	- One event for vouch.

  	- Up to one event for AutoUnbid if bid.len() > MAX_BID_COUNT.

  Total Complexity: O(M + B + C + logM + logB + X) 

  \# \</weight> 

___


## staking
 
### bond(controller: `<T::Lookup as StaticLookup>::Source`, value: `Compact<BalanceOf<T>>`, payee: `RewardDestination`)
- **interface**: api.tx.staking.bond
- **summary**:   Take the origin account as a stash and lock up `value` of its balance. `controller` will be the account that controls it. 

  `value` must be more than the `minimum_balance` specified by `T::Currency`. 

  The dispatch origin for this call must be _Signed_ by the stash account. 

  \# \<weight>

   

  - Independent of the arguments. Moderate complexity.

  - O(1).

  - Three extra DB entries.

  NOTE: Two of the storage writes (`Self::bonded`, `Self::payee`) are _never_ cleaned unless the `origin` falls below _existential deposit_ and gets removed as dust. 

  \# \</weight> 
 
### bondExtra(max_additional: `Compact<BalanceOf<T>>`)
- **interface**: api.tx.staking.bondExtra
- **summary**:   Add some extra amount that have appeared in the stash `free_balance` into the balance up for staking. 

  Use this if there are additional funds in your stash account that you wish to bond. Unlike [`bond`] or [`unbond`] this function does not impose any limitation on the amount that can be added. 

  The dispatch origin for this call must be _Signed_ by the stash, not the controller. 

  \# \<weight>

   

  - Independent of the arguments. Insignificant complexity.

  - O(1).

  - One DB entry.

  \# \</weight> 
 
### cancelDeferredSlash(era: `EraIndex`, slash_indices: `Vec<u32>`)
- **interface**: api.tx.staking.cancelDeferredSlash
- **summary**:   Cancel enactment of a deferred slash. Can be called by either the root origin or the `T::SlashCancelOrigin`. passing the era and indices of the slashes for that era to kill. 

  \# \<weight>

   

  - One storage write.

  \# \</weight> 
 
### chill()
- **interface**: api.tx.staking.chill
- **summary**:   Declare no desire to either validate or nominate. 

  Effects will be felt at the beginning of the next era. 

  The dispatch origin for this call must be _Signed_ by the controller, not the stash. 

  \# \<weight>

   

  - Independent of the arguments. Insignificant complexity.

  - Contains one read.

  - Writes are limited to the `origin` account key.

  \# \</weight> 
 
### forceNewEra()
- **interface**: api.tx.staking.forceNewEra
- **summary**:   Force there to be a new era at the end of the next session. After this, it will be reset to normal (non-forced) behaviour. 

  \# \<weight>

   

  - No arguments.

  \# \</weight> 
 
### forceNewEraAlways()
- **interface**: api.tx.staking.forceNewEraAlways
- **summary**:   Force there to be a new era at the end of sessions indefinitely. 

  \# \<weight>

   

  - One storage write

  \# \</weight> 
 
### forceNoEras()
- **interface**: api.tx.staking.forceNoEras
- **summary**:   Force there to be no new eras indefinitely. 

  \# \<weight>

   

  - No arguments.

  \# \</weight> 
 
### forceUnstake(stash: `T::AccountId`)
- **interface**: api.tx.staking.forceUnstake
- **summary**:   Force a current staker to become completely unstaked, immediately. 
 
### nominate(targets: `Vec<<T::Lookup as StaticLookup>::Source>`)
- **interface**: api.tx.staking.nominate
- **summary**:   Declare the desire to nominate `targets` for the origin controller. 

  Effects will be felt at the beginning of the next era. 

  The dispatch origin for this call must be _Signed_ by the controller, not the stash. 

  \# \<weight>

   

  - The transaction's complexity is proportional to the size of `targets`,which is capped at `MAX_NOMINATIONS`. 

  - Both the reads and writes follow a similar pattern.

  \# \</weight> 
 
### rebond(value: `Compact<BalanceOf<T>>`)
- **interface**: api.tx.staking.rebond
- **summary**:   Rebond a portion of the stash scheduled to be unlocked. 

  \# \<weight>

   

  - Time complexity: O(1). Bounded by `MAX_UNLOCKING_CHUNKS`.

  - Storage changes: Can't increase storage, only decrease it.

  \# \</weight> 
 
### setController(controller: `<T::Lookup as StaticLookup>::Source`)
- **interface**: api.tx.staking.setController
- **summary**:   (Re-)set the controller of a stash. 

  Effects will be felt at the beginning of the next era. 

  The dispatch origin for this call must be _Signed_ by the stash, not the controller. 

  \# \<weight>

   

  - Independent of the arguments. Insignificant complexity.

  - Contains a limited number of reads.

  - Writes are limited to the `origin` account key.

  \# \</weight> 
 
### setInvulnerables(validators: `Vec<T::AccountId>`)
- **interface**: api.tx.staking.setInvulnerables
- **summary**:   Set the validators who cannot be slashed (if any). 
 
### setPayee(payee: `RewardDestination`)
- **interface**: api.tx.staking.setPayee
- **summary**:   (Re-)set the payment target for a controller. 

  Effects will be felt at the beginning of the next era. 

  The dispatch origin for this call must be _Signed_ by the controller, not the stash. 

  \# \<weight>

   

  - Independent of the arguments. Insignificant complexity.

  - Contains a limited number of reads.

  - Writes are limited to the `origin` account key.

  \# \</weight> 
 
### setValidatorCount(new: `Compact<u32>`)
- **interface**: api.tx.staking.setValidatorCount
- **summary**:   The ideal number of validators. 
 
### unbond(value: `Compact<BalanceOf<T>>`)
- **interface**: api.tx.staking.unbond
- **summary**:   Schedule a portion of the stash to be unlocked ready for transfer out after the bond period ends. If this leaves an amount actively bonded less than T::Currency::minimum_balance(), then it is increased to the full amount. 

  Once the unlock period is done, you can call `withdraw_unbonded` to actually move the funds out of management ready for transfer. 

  No more than a limited number of unlocking chunks (see `MAX_UNLOCKING_CHUNKS`) can co-exists at the same time. In that case, [`Call::withdraw_unbonded`] need to be called first to remove some of the chunks (if possible). 

  The dispatch origin for this call must be _Signed_ by the controller, not the stash. 

  See also [`Call::withdraw_unbonded`]. 

  \# \<weight>

   

  - Independent of the arguments. Limited but potentially exploitable complexity.

  - Contains a limited number of reads.

  - Each call (requires the remainder of the bonded balance to be above `minimum_balance`)  will cause a new entry to be inserted into a vector (`Ledger.unlocking`) kept in storage.   The only way to clean the aforementioned storage item is also user-controlled via `withdraw_unbonded`. 

  - One DB entry.</weight> 
 
### validate(prefs: `ValidatorPrefs`)
- **interface**: api.tx.staking.validate
- **summary**:   Declare the desire to validate for the origin controller. 

  Effects will be felt at the beginning of the next era. 

  The dispatch origin for this call must be _Signed_ by the controller, not the stash. 

  \# \<weight>

   

  - Independent of the arguments. Insignificant complexity.

  - Contains a limited number of reads.

  - Writes are limited to the `origin` account key.

  \# \</weight> 
 
### withdrawUnbonded()
- **interface**: api.tx.staking.withdrawUnbonded
- **summary**:   Remove any unlocked chunks from the `unlocking` queue from our management. 

  This essentially frees up that balance to be used by the stash account to do whatever it wants. 

  The dispatch origin for this call must be _Signed_ by the controller, not the stash. 

  See also [`Call::unbond`]. 

  \# \<weight>

   

  - Could be dependent on the `origin` argument and how much `unlocking` chunks exist. It implies `consolidate_unlocked` which loops over `Ledger.unlocking`, which is  indirectly user-controlled. See [`unbond`] for more detail. 

  - Contains a limited number of reads, yet the size of which could be large based on `ledger`.

  - Writes are limited to the `origin` account key.

  \# \</weight> 

___


## sudo
 
### setKey(new: `<T::Lookup as StaticLookup>::Source`)
- **interface**: api.tx.sudo.setKey
- **summary**:   Authenticates the current sudo key and sets the given AccountId (`new`) as the new sudo key. 

  The dispatch origin for this call must be _Signed_. 

  \# \<weight>

   

  - O(1).

  - Limited storage reads.

  - One DB change.

  \# \</weight> 
 
### sudo(proposal: `Box<T::Proposal>`)
- **interface**: api.tx.sudo.sudo
- **summary**:   Authenticates the sudo key and dispatches a function call with `Root` origin. 

  The dispatch origin for this call must be _Signed_. 

  \# \<weight>

   

  - O(1).

  - Limited storage reads.

  - One DB write (event).

  - Unknown weight of derivative `proposal` execution.

  \# \</weight> 
 
### sudoAs(who: `<T::Lookup as StaticLookup>::Source`, proposal: `Box<T::Proposal>`)
- **interface**: api.tx.sudo.sudoAs
- **summary**:   Authenticates the sudo key and dispatches a function call with `Signed` origin from a given account. 

  The dispatch origin for this call must be _Signed_. 

  \# \<weight>

   

  - O(1).

  - Limited storage reads.

  - One DB write (event).

  - Unknown weight of derivative `proposal` execution.

  \# \</weight> 

___


## system
 
### fillBlock()
- **interface**: api.tx.system.fillBlock
- **summary**:   A big dispatch that will disallow any other transaction to be included. 
 
### killPrefix(prefix: `Key`)
- **interface**: api.tx.system.killPrefix
- **summary**:   Kill all storage items with a key that starts with the given prefix. 
 
### killStorage(keys: `Vec<Key>`)
- **interface**: api.tx.system.killStorage
- **summary**:   Kill some items from storage. 
 
### remark(_remark: `Vec<u8>`)
- **interface**: api.tx.system.remark
- **summary**:   Make some on-chain remark. 
 
### setChangesTrieConfig(changes_trie_config: `Option<ChangesTrieConfiguration>`)
- **interface**: api.tx.system.setChangesTrieConfig
- **summary**:   Set the new changes trie configuration. 
 
### setCode(code: `Vec<u8>`)
- **interface**: api.tx.system.setCode
- **summary**:   Set the new runtime code. 
 
### setCodeWithoutChecks(code: `Vec<u8>`)
- **interface**: api.tx.system.setCodeWithoutChecks
- **summary**:   Set the new runtime code without doing any checks of the given `code`. 
 
### setHeapPages(pages: `u64`)
- **interface**: api.tx.system.setHeapPages
- **summary**:   Set the number of pages in the WebAssembly environment's heap. 
 
### setStorage(items: `Vec<KeyValue>`)
- **interface**: api.tx.system.setStorage
- **summary**:   Set some items of storage. 

___


## technicalCommittee
 
### execute(proposal: `Box<<T as Trait<I>>::Proposal>`)
- **interface**: api.tx.technicalCommittee.execute
- **summary**:   Dispatch a proposal from a member using the `Member` origin. 

  Origin must be a member of the collective. 
 
### propose(threshold: `Compact<MemberCount>`, proposal: `Box<<T as Trait<I>>::Proposal>`)
- **interface**: api.tx.technicalCommittee.propose
- **summary**:   \# \<weight>

   

  - Bounded storage reads and writes.

  - Argument `threshold` has bearing on weight.

  \# \</weight> 
 
### setMembers(new_members: `Vec<T::AccountId>`)
- **interface**: api.tx.technicalCommittee.setMembers
- **summary**:   Set the collective's membership manually to `new_members`. Be nice to the chain and provide it pre-sorted. 

  Requires root origin. 
 
### vote(proposal: `T::Hash`, index: `Compact<ProposalIndex>`, approve: `bool`)
- **interface**: api.tx.technicalCommittee.vote
- **summary**:   \# \<weight>

   

  - Bounded storage read and writes.

  - Will be slightly heavier if the proposal is approved / disapproved after the vote.

  \# \</weight> 

___


## technicalMembership
 
### addMember(who: `T::AccountId`)
- **interface**: api.tx.technicalMembership.addMember
- **summary**:   Add a member `who` to the set. 

  May only be called from `AddOrigin` or root. 
 
### changeKey(new: `T::AccountId`)
- **interface**: api.tx.technicalMembership.changeKey
- **summary**:   Swap out the sending member for some other key `new`. 

  May only be called from `Signed` origin of a current member. 
 
### removeMember(who: `T::AccountId`)
- **interface**: api.tx.technicalMembership.removeMember
- **summary**:   Remove a member `who` from the set. 

  May only be called from `RemoveOrigin` or root. 
 
### resetMembers(members: `Vec<T::AccountId>`)
- **interface**: api.tx.technicalMembership.resetMembers
- **summary**:   Change the membership to a new set, disregarding the existing membership. Be nice and pass `members` pre-sorted. 

  May only be called from `ResetOrigin` or root. 
 
### swapMember(remove: `T::AccountId`, add: `T::AccountId`)
- **interface**: api.tx.technicalMembership.swapMember
- **summary**:   Swap out one member `remove` for another `add`. 

  May only be called from `SwapOrigin` or root. 

___


## timestamp
 
### set(now: `Compact<T::Moment>`)
- **interface**: api.tx.timestamp.set
- **summary**:   Set the current time. 

  This call should be invoked exactly once per block. It will panic at the finalization phase, if this call hasn't been invoked by that time. 

  The timestamp should be greater than the previous one by the amount specified by `MinimumPeriod`. 

  The dispatch origin for this call must be `Inherent`. 

___


## treasury
 
### approveProposal(proposal_id: `Compact<ProposalIndex>`)
- **interface**: api.tx.treasury.approveProposal
- **summary**:   Approve a proposal. At a later time, the proposal will be allocated to the beneficiary and the original deposit will be returned. 

  \# \<weight>

   

  - O(1).

  - Limited storage reads.

  - One DB change.

  \# \</weight> 
 
### closeTip(hash: `T::Hash`)
- **interface**: api.tx.treasury.closeTip
- **summary**:   Close and payout a tip. 

  The dispatch origin for this call must be _Signed_. 

  The tip identified by `hash` must have finished its countdown period. 

  - `hash`: The identity of the open tip for which a tip value is declared. This is formed   as the hash of the tuple of the original tip `reason` and the beneficiary account ID. 

  \# \<weight>

   

  - `O(T)`

  - One storage retrieval (codec `O(T)`) and two removals.

  - Up to three balance operations.

  \# \</weight> 
 
### proposeSpend(value: `Compact<BalanceOf<T>>`, beneficiary: `<T::Lookup as StaticLookup>::Source`)
- **interface**: api.tx.treasury.proposeSpend
- **summary**:   Put forward a suggestion for spending. A deposit proportional to the value is reserved and slashed if the proposal is rejected. It is returned once the proposal is awarded. 

  \# \<weight>

   

  - O(1).

  - Limited storage reads.

  - One DB change, one extra DB entry.

  \# \</weight> 
 
### rejectProposal(proposal_id: `Compact<ProposalIndex>`)
- **interface**: api.tx.treasury.rejectProposal
- **summary**:   Reject a proposed spend. The original deposit will be slashed. 

  \# \<weight>

   

  - O(1).

  - Limited storage reads.

  - One DB clear.

  \# \</weight> 
 
### reportAwesome(reason: `Vec<u8>`, who: `T::AccountId`)
- **interface**: api.tx.treasury.reportAwesome
- **summary**:   Report something `reason` that deserves a tip and claim any eventual the finder's fee. 

  The dispatch origin for this call must be _Signed_. 

  Payment: `TipReportDepositBase` will be reserved from the origin account, as well as `TipReportDepositPerByte` for each byte in `reason`. 

  - `reason`: The reason for, or the thing that deserves, the tip; generally this will be   a UTF-8-encoded URL. 

  - `who`: The account which should be credited for the tip.

  Emits `NewTip` if successful. 

  \# \<weight>

   

  - `O(R)` where `R` length of `reason`.

  - One balance operation.

  - One storage mutation (codec `O(R)`).

  - One event.

  \# \</weight> 
 
### retractTip(hash: `T::Hash`)
- **interface**: api.tx.treasury.retractTip
- **summary**:   Retract a prior tip-report from `report_awesome`, and cancel the process of tipping. 

  If successful, the original deposit will be unreserved. 

  The dispatch origin for this call must be _Signed_ and the tip identified by `hash` must have been reported by the signing account through `report_awesome` (and not through `tip_new`). 

  - `hash`: The identity of the open tip for which a tip value is declared. This is formed   as the hash of the tuple of the original tip `reason` and the beneficiary account ID. 

  Emits `TipRetracted` if successful. 

  \# \<weight>

   

  - `O(T)`

  - One balance operation.

  - Two storage removals (one read, codec `O(T)`).

  - One event.

  \# \</weight> 
 
### tip(hash: `T::Hash`, tip_value: `BalanceOf<T>`)
- **interface**: api.tx.treasury.tip
- **summary**:   Declare a tip value for an already-open tip. 

  The dispatch origin for this call must be _Signed_ and the signing account must be a member of the `Tippers` set. 

  - `hash`: The identity of the open tip for which a tip value is declared. This is formed   as the hash of the tuple of the hash of the original tip `reason` and the beneficiary   account ID. 

  - `tip_value`: The amount of tip that the sender would like to give. The median tip  value of active tippers will be given to the `who`. 

  Emits `TipClosing` if the threshold of tippers has been reached and the countdown period has started. 

  \# \<weight>

   

  - `O(T)`

  - One storage mutation (codec `O(T)`), one storage read `O(1)`.

  - Up to one event.

  \# \</weight> 
 
### tipNew(reason: `Vec<u8>`, who: `T::AccountId`, tip_value: `BalanceOf<T>`)
- **interface**: api.tx.treasury.tipNew
- **summary**:   Give a tip for something new; no finder's fee will be taken. 

  The dispatch origin for this call must be _Signed_ and the signing account must be a member of the `Tippers` set. 

  - `reason`: The reason for, or the thing that deserves, the tip; generally this will be   a UTF-8-encoded URL. 

  - `who`: The account which should be credited for the tip.

  - `tip_value`: The amount of tip that the sender would like to give. The median tip  value of active tippers will be given to the `who`. 

  Emits `NewTip` if successful. 

  \# \<weight>

   

  - `O(R + T)` where `R` length of `reason`, `T` is the number of tippers. `T` is  naturally capped as a membership set, `R` is limited through transaction-size. 

  - Two storage insertions (codecs `O(R)`, `O(T)`), one read `O(1)`.

  - One event.

  \# \</weight> 

___


## utility
 
### approveAsMulti(threshold: `u16`, other_signatories: `Vec<T::AccountId>`, maybe_timepoint: `Option<Timepoint<T::BlockNumber>>`, call_hash: `[u8; 32]`)
- **interface**: api.tx.utility.approveAsMulti
- **summary**:   Register approval for a dispatch to be made from a deterministic composite account if approved by a total of `threshold - 1` of `other_signatories`. 

  Payment: `MultisigDepositBase` will be reserved if this is the first approval, plus `threshold` times `MultisigDepositFactor`. It is returned once this dispatch happens or is cancelled. 

  The dispatch origin for this call must be _Signed_. 

  - `threshold`: The total number of approvals for this dispatch before it is executed. 

  - `other_signatories`: The accounts (other than the sender) who can approve thisdispatch. May not be empty. 

  - `maybe_timepoint`: If this is the first approval, then this must be `None`. If it isnot the first approval, then it must be `Some`, with the timepoint (block number and transaction index) of the first approval transaction. 

  - `call_hash`: The hash of the call to be executed.

  NOTE: If this is the final approval, you will want to use `as_multi` instead. 

  \# \<weight>

   

  - `O(S)`.

  - Up to one balance-reserve or unreserve operation.

  - One passthrough operation, one insert, both `O(S)` where `S` is the number of  signatories. `S` is capped by `MaxSignatories`, with weight being proportional. 

  - One encode & hash, both of complexity `O(S)`.

  - Up to one binary search and insert (`O(logS + S)`).

  - I/O: 1 read `O(S)`, up to 1 mutate `O(S)`. Up to one remove.

  - One event.

  - Storage: inserts one item, value size bounded by `MaxSignatories`, with a  deposit taken for its lifetime of   `MultisigDepositBase + threshold * MultisigDepositFactor`. 

  \# \</weight> 
 
### asMulti(threshold: `u16`, other_signatories: `Vec<T::AccountId>`, maybe_timepoint: `Option<Timepoint<T::BlockNumber>>`, call: `Box<<T as Trait>::Call>`)
- **interface**: api.tx.utility.asMulti
- **summary**:   Register approval for a dispatch to be made from a deterministic composite account if approved by a total of `threshold - 1` of `other_signatories`. 

  If there are enough, then dispatch the call. 

  Payment: `MultisigDepositBase` will be reserved if this is the first approval, plus `threshold` times `MultisigDepositFactor`. It is returned once this dispatch happens or is cancelled. 

  The dispatch origin for this call must be _Signed_. 

  - `threshold`: The total number of approvals for this dispatch before it is executed. 

  - `other_signatories`: The accounts (other than the sender) who can approve thisdispatch. May not be empty. 

  - `maybe_timepoint`: If this is the first approval, then this must be `None`. If it isnot the first approval, then it must be `Some`, with the timepoint (block number and transaction index) of the first approval transaction. 

  - `call`: The call to be executed.

  NOTE: Unless this is the final approval, you will generally want to use `approve_as_multi` instead, since it only requires a hash of the call. 

  Result is equivalent to the dispatched result if `threshold` is exactly `1`. Otherwise on success, result is `Ok` and the result from the interior call, if it was executed, may be found in the deposited `MultisigExecuted` event. 

  \# \<weight>

   

  - `O(S + Z + Call)`.

  - Up to one balance-reserve or unreserve operation.

  - One passthrough operation, one insert, both `O(S)` where `S` is the number of  signatories. `S` is capped by `MaxSignatories`, with weight being proportional. 

  - One call encode & hash, both of complexity `O(Z)` where `Z` is tx-len.

  - One encode & hash, both of complexity `O(S)`.

  - Up to one binary search and insert (`O(logS + S)`).

  - I/O: 1 read `O(S)`, up to 1 mutate `O(S)`. Up to one remove.

  - One event.

  - The weight of the `call`.

  - Storage: inserts one item, value size bounded by `MaxSignatories`, with a  deposit taken for its lifetime of   `MultisigDepositBase + threshold * MultisigDepositFactor`. 

  \# \</weight> 
 
### asSub(index: `u16`, call: `Box<<T as Trait>::Call>`)
- **interface**: api.tx.utility.asSub
- **summary**:   Send a call through an indexed pseudonym of the sender. 

  The dispatch origin for this call must be _Signed_. 

  \# \<weight>

   

  - The weight of the `call`.

  \# \</weight> 
 
### batch(calls: `Vec<<T as Trait>::Call>`)
- **interface**: api.tx.utility.batch
- **summary**:   Send a batch of dispatch calls. 

  This will execute until the first one fails and then stop. 

  May be called from any origin. 

  - `calls`: The calls to be dispatched from the same origin. 

  \# \<weight>

   

  - The sum of the weights of the `calls`.

  - One event.

  \# \</weight> 

  This will return `Ok` in all circumstances. To determine the success of the batch, an event is deposited. If a call failed and the batch was interrupted, then the `BatchInterrupted` event is deposited, along with the number of successful calls made and the error of the failed call. If all were successful, then the `BatchCompleted` event is deposited. 
 
### cancelAsMulti(threshold: `u16`, other_signatories: `Vec<T::AccountId>`, timepoint: `Timepoint<T::BlockNumber>`, call_hash: `[u8; 32]`)
- **interface**: api.tx.utility.cancelAsMulti
- **summary**:   Cancel a pre-existing, on-going multisig transaction. Any deposit reserved previously for this operation will be unreserved on success. 

  The dispatch origin for this call must be _Signed_. 

  - `threshold`: The total number of approvals for this dispatch before it is executed. 

  - `other_signatories`: The accounts (other than the sender) who can approve thisdispatch. May not be empty. 

  - `timepoint`: The timepoint (block number and transaction index) of the first approvaltransaction for this dispatch. 

  - `call_hash`: The hash of the call to be executed.

  \# \<weight>

   

  - `O(S)`.

  - Up to one balance-reserve or unreserve operation.

  - One passthrough operation, one insert, both `O(S)` where `S` is the number of  signatories. `S` is capped by `MaxSignatories`, with weight being proportional. 

  - One encode & hash, both of complexity `O(S)`.

  - One event.

  - I/O: 1 read `O(S)`, one remove.

  - Storage: removes one item.

  \# \</weight> 

___


## vesting
 
### vest()
- **interface**: api.tx.vesting.vest
- **summary**:   Unlock any vested funds of the sender account. 

  The dispatch origin for this call must be _Signed_ and the sender must have funds still locked under this module. 

  Emits either `VestingCompleted` or `VestingUpdated`. 

  \# \<weight>

   

  - `O(1)`.

  - One balance-lock operation.

  - One storage read (codec `O(1)`) and up to one removal.

  - One event.

  \# \</weight> 
 
### vestOther(target: `<T::Lookup as StaticLookup>::Source`)
- **interface**: api.tx.vesting.vestOther
- **summary**:   Unlock any vested funds of a `target` account. 

  The dispatch origin for this call must be _Signed_. 

  - `target`: The account whose vested funds should be unlocked. Must have funds still locked under this module. 

  Emits either `VestingCompleted` or `VestingUpdated`. 

  \# \<weight>

   

  - `O(1)`.

  - Up to one account lookup.

  - One balance-lock operation.

  - One storage read (codec `O(1)`) and up to one removal.

  - One event.

  \# \</weight> 