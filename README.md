# Ponlysis

**ponlysis.bio** · an on-chain computer-aided drug design lab from Pons Lab, an honorable project of ponsfamily.com.
Plant compounds are docked into the binding pockets of cancer targets from the RCSB Protein Data Bank, and Robinhood
Chain (chain id 4663) scores every submitted pose with an integer Vina-style function. `SPEC.md` is the build contract.

Static pages plus vanilla ES modules, no framework, no bundler, no CDN, a strict CSP. 3Dmol.js is self-hosted in
`/lib`. Vercel functions live in `/api`. The shared head, nav and footer come from `partials/` and are stamped into
every page by `node tools/shell.mjs`.

## Where it comes from

Ponlysis is ponchem (github.com/shelbybrothers/ponchem at `5cb250f`) recreated under its own name. It shares
ponchem's live lab contract: `LAB` in `js/config.js` is the PonchemLab deployment recorded in
`contracts/deployments.json`, so runs, the leaderboard and prize epochs are common to both sites. The sources in
`contracts/src` are renamed to `PonlysisLab`; `contracts/deploy.sh` puts a lab of its own on chain and rewrites `LAB`.
That lab takes $PONCHEM as its token, so `labStatus()` in `js/chain.js` ignores the contract's token until `TOKEN.ca`
names the same coin: until then a docking test is paid in ETH only. The `ponchem-tables-v1` and `ponchem-vectors-v1`
format ids in `data/tables` and `data/vectors` are left as they are: they name the formats the live tables and the
engine's test vectors were built with.

## Run it

```bash
node tools/dev.mjs
```

Serves http://127.0.0.1:6130 with clean URLs, the API routes and the production headers (Node 22 or later).
`/__shell` shows the shared shell alone. It refuses to start when the port is taken: use `PORT=<port>`.

## Check it

```bash
node tools/shell.mjs --check     # every page carries the current partials
node tools/verify.mjs            # file gates, then every page in headless Chrome at seven widths
node tools/verify.mjs --files    # file gates only
node tools/wallet-test.mjs       # the connect wallet flow with a simulated MetaMask, against the dev server
node tools/fixtures/3dmol-check.mjs   # 3Dmol loads 1M17 from RCSB under the production CSP in headless Chrome
```

## Ship it

```bash
tools/ship.sh
```

Deploys the committed HEAD to the Vercel project `ponlysis` from a `git archive` export (no `.git`). Link once with
`vercel link --project ponlysis`. Uncommitted changes are refused.

## $PONLYSIS

The coin launches later on ponsfamily.com. `TOKEN.ca` and `TOKEN.buyUrl` in `js/config.js` are null until then: the
Buy control reads `Buy $PONLYSIS · soon` and Copy CA is disabled. Set both, run `node tools/shell.mjs`, commit, ship.

X: https://x.com/ponlysis
