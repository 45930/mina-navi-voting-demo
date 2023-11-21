# Mina Navigators Voting Demo

Welcome to a voting demo, showcasing advanced usage of o1js to power a voting app.

The design decisions for this demo prioritize L1 data availability and robustness with respect to concurrent users.  Anyone with some Mina to pay fees can join this election and submit votes; it requires no access to some off-chain merkle tree data, since all of the data is stored on chain.

## Patterns Used

### o1js-pack
Used for packing multiple values into a single field to store on chain, this tool helps to solve the data storage limitations of the L1.

### Custom Tokens
Used for determining access to, and counting the votes of the actual election.  In this demo, anyone who wants to participate can join and mint some tokens.  This pattern could be used with a whitelist as well, or to issue different people different amounts of voting power based on some other factors.  We don't need to track a nullifier because token balance represents the right to cast a vote, and running out of tokens means you no longer have the right.

### Actions
Used for concurrent updates.  This allows multiple users to cast a vote during the same block, and get rolled up into a single state update.

## How to participate

### Install the project and its dependencies
```sh
npm i
```

### Add your Mina account
If you have an account funded on the Berkeley network, feel free to manually add the keys to the `keys/` folder.  Otherwise

```sh
zk config
```

Make sure your account is funded before continuing.

### Join the election
```sh
npm run build
node ./build/src/joinElection.js
```

### Vote in the election
In order to view what is on the ballot, you'll have to decode the zkapp state.

```sh
npm run build
node ./build/src/getElectionDetails.js
```

After you've reviewed the options, edit the file `src/votes.json` with your preferences, then:

```sh
npm run build
node ./build/src/submitVotes.js
```

Congratulations, you just submitted your vote!

## Next Steps

My objective with this solution is to establish a baseline of performance using the L1.  While I believe this solution works, the block times on Mina are slow, paying the fee for custom tokens is expensive, there is a limit to on chain state even using packing, and if too many actions are submitted before they are reduced, the app could reach a bricked state.  I'd like to continue to think about these issues with an eye toward new releases to o1js, but also begin to explore how an L2 system would help.

## License

[Apache-2.0](LICENSE)
