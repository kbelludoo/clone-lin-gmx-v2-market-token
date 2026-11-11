# clone-lin-gmx-v2-market-token

Experimental LIN clone of GMX V2 `usdToMarketTokenAmount` (uint64-scale: three-branch first deposit — `floatToWei` when supply AND pool are empty, `floatToWei(pool+usd)` leftover, else floor `mulDiv`).

- **Upstream:** [gmx-io/gmx-synthetics](https://github.com/gmx-io/gmx-synthetics) `contracts/market/MarketUtils.sol` commit `a85ea3491c19c93bb4b5a002d9b358fb769b7849` (GPL-2.0-or-later after BUSL Change Date 2026-08-31).
- **LIN kernel:** [`src/lin_gmx_v2_market_token.lin`](src/lin_gmx_v2_market_token.lin)
- **C11 oracle copy:** [`test/oracles/gmx_v2_market_token_c11.c`](test/oracles/gmx_v2_market_token_c11.c)
- **Proofs (lin-open only):** [kbelludoo/lin-open](https://github.com/kbelludoo/lin-open) `examples/gmx_v2_market_token/` and `python3 test/prove_gmx_v2_market_token_external.py`
- **Class:** EXPERIMENTAL. Not Arbitrum GMX V2 uint256. Not FLOAT 1e30. Not FLOAT_TO_WEI_DIVISOR 1e12. Not a production replacement. Not Lido `getSharesByPooledEth`, not Rocket `getRethValue`, not 1:1 empty mint, and not `supply==0 || poolValue==0`.

Named goldens (uint64-scale, DIV'=1e6): first deposit **100** vs Lido/Rocket 1:1 **100000000**; leftover pool 2e7 **120** vs Lido **0** vs ignore-pool **100** vs forgot-wei **120000000**; subsequent **25000000** vs always-wei **50**; inverse **50000000** vs empty-supply 1:1 **100**; `supply>0 pool=0` ok=0 vs Lido **50000000** vs OR-empty wei **50**; remainder 3*4/5 **2** vs ceil **3**; dust **0**; leftover usd=0 **20**; 7e18+3e18 **ok=0** vs naive wrap **-8446744073709551616**; 7e9*2e9 **ok=0** vs wrap **-4446744073709551616**.
