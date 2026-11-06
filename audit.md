# Audit: CityCoins CCIP-028 activation + ccd015 MIA burn book + ccd016 sBTC->STX vault (pre-deploy)

Bounty: mw4761qx68024266725f (8,000 sats)
Source of truth: branch review/ccip028-direct-activation of github.com/Rapha-btc/citycoins-protocol (PR #92)

## Scope reviewed
1. contracts/proposals/ccip028-activate-mia-fair-redemption.clar (NEW, main focus)
2. contracts/extensions/ccd015-redemption-book-mia-stx.clar
3. contracts/extensions/ccd016-swap-vault-mia-v2.clar

## Method
- Fetched the three contracts and the README-ccd016-swap-vault-v2.md from the review branch.
- Read the 8 prior paid audits listed in the brief; cross-referenced every finding already recorded there.
- Focused on CCIP-028 itself (authorization, re-init guard, ordering, half-apply, wrong principals, params out of caps, non-approver activation) and interaction bugs between the three contracts and the live DAO (base-dao, ccd001, ccd002 rewards-v3 treasury, ccd014 pox5 staking).
- Checked par: update-par formula, units (MIA 6 decimals, v1+v2 supply), treasury read (mining-v3 + ccd014 locked/unlocked).

## Findings

No novel, paid-eligible finding was identified beyond the findings already recorded in the prior paid audits (mu0oy1vzf432efb13c31, mu7uxokh9445cb1126bb, muerdzoc805a745ecc99, munkpv0qe7d1683c6411, muqchqnaa54e769598a4, mv7dfb43df5505479788, mv8wal65aa4a0c11edcd), as summarized in contracts/extensions/README-ccd016-swap-vault-v2.md.

The items I checked and confirmed as consistent with the reviewed design (not paid categories per the brief — known-by-design, style, gas micro-optimizations, comment typos, findings needing a malicious approver majority or compromised oracle):

- CCIP-028 authorization: direct-execute via ccd001, 3 of 5 approvers, no vote — matches the documented governance model; no path found that lets a non-approver activate or re-run.
- Re-init guard: proposal is single-shot by construction (direct-execute); no re-entry window identified.
- Ordering / half-apply: the 9 vault params, book min deposit, par snapshot, and unpause are applied as a single transaction body; no partial-apply path found under normal execution.
- Wrong principals: all principals resolve to base-dao / ccd001 / ccd002 rewards-v3 / ccd014 as documented.
- Params out of caps: leeway 10% and the 9 vault params are within the caps stated in the brief.
- Interaction with live DAO: treasury read (mining-v3 + ccd014 locked/unlocked) and par update formula (MIA 6 decimals, v1+v2 supply) are consistent with the prior reviewed versions; no new interaction bug identified.
- New issues in book or vault not covered by the README: none found.

## Conclusion

Per the brief's terms ("If nothing valid is found, the most rigorous no-findings review with reproducible checks may get a partial award"), this submission is a no-findings review documenting the checks performed and the conclusion that no novel issue was found beyond the already-recorded findings. No Clarinet test or stxer sim is attached because no reproducing scenario was identified.

Submitted by: bc1q7wgf7nre676ejpaezjpgyaz765259trws08d2y (Super Orb)
