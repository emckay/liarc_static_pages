# Policies

NOTE: The company may add, remove, or modify any policy or proposed policy.

NOTE: Policies are in no way binding commitments by the company. The company, its employees, or any person affiliated with the company may choose not to follow or implement any policy at any time with no notice.

#### General

1. The moderator is Eric McKay.

1. The company's name is LIARC.

1. Any person may create an account on the company's website a person with an account is a `user`.

1. Each user has an email address (unique, matches regex `/\A[^@]+@([^@\.]+\.)+[^@\.]+\z/`), a password (8-72 chars), a pseudo-cash balance (`PCB`) (integer), and a pseudo-share count (`PSC`) (integer >= 0).

#### Pseudo-cash

1. The company has a USD balance (`UB`) which is the actual cash balance on the company's bank account (disregarding pending transactions) in USD multiplied by 100.

1. The company also has an account with a PCB and PSC. The company's PCB and PSC are referred to as CPCB and CPSC.

1. Users may increase their PCB by spending USD with a credit card.

1. For a transaction of X USD paid by credit card, the user's PCB increases by `floor(X * 100 * (0.971) - 30)`.

1. In each transaction, the company's PCB is decreased by the amount of the increase in the user's PCB.

1. The company will not issue refunds for any reason.

#### Pseudo-shares

1. The company's initial PSC is `900,000`.

1. The company's initial UB is `500,000`.

1. The company's initial PCB is `-500,000`.

1. The sum of all users' PSC (including the company) is total pseudo-shares outstanding (`TPSO`).

1. The company may sell pseudo-shares (`PS`) for pseudo-cash (`PC`). Each of these trasaction involves selling `q` PS at price `p` per share.

1. The first day of each month at `00:00:00 UTC`, the following calculations are performed (the subscript `m` is the number of the weeks that have elapsed since launch):

    1. (USD balance) `UB_m = UB`

    1. (Company pseudo-cash balance) `CPCB_m = CPCB`

    1. (Capital gains) `CG_m = sum across all PS sales in month m (q*p)`

    1. (Net income) `WNI_m = UB_m - UB_{m-1} + CPCB_m - CPCB_{m-1} - CG_m`

1. After performing the calculations, the following operation is applied to each account:

```
        PCB += floor(WNI_m * PSC_m / TPSO)
```

#### Potential pseudo-shares and potential pseudo-cash

1. A potential pseudo-share (`PPS`) is an object that converts into a pseudo-share (`PS`) under certain trigger conditions.

1. A unit of potential pseudo-cash (`PPC`) is an object that converts into a unit of pseudo-cash (`PC`) under certain trigger conditions.

1. A PPS/PPC class is the set of all PPS/PPC that have the same trigger condition. A unit of PPS/PPC of class `c` can be referred to as `PPS_c` or `PPC_c`.

1. Each user has an integer PPS count for each PPS class. A user's PPS count for class `c` is called `PPSC_c`.

1. Each user has an integer PPC balance for each PPC class. A user's PPC balance for class `c` is called `PPCB_c`.

1. For every class `c`, the sum of `PPSC_c` across all users is 0, and the sum of `PPCB_c` across all users is 0.

1. When class `c` triggers, the following operations are applied to each account:

```
        PSC += PPSC_c
        PCB += PPCB_c
```

1. Users have the following additional attributes:

    1. Reserved pseudo-cash balance (`RPCB`) and available pseudo-cash balance (`APCB`)
    1. Reserved pseudo-share count (`RPSC`) and available pseudo-share count (`APSC`)

1. These attributes are used to prevent insolvency by restricting the use of PC and PS that may be owed to other users if a class of PPC/PPS triggers.

1. A PPS/PPC class also has an end date/time.

1. If the trigger conditions have not been met for class `c` at c's end date/time, the `PPS_c` and `PPC_c` will be canceled.

#### Markets

1. A market is a limit order book that accepts limit orders in PC for PS (a PS market) or accepts orders in PPC for PPS (a PPS market).

1. Orders in the market are matched with price-time priority.

1. Each market has a last trade price (`LTP`) equal to the price in PC or PPC of the most recently executed trade.

1. Orders have priorities set by the user that submits them.

1. For a PS market:

    1. A user may submit a buy order (bid) to a market for `q` PS at price per share `p` only if the user's `PCB >= q*p`.

    1. A user may submit a sell order (offer) to a market for `q` PS a price per share `p` only if the user's `PSC >= q`.

1. For a PPS market:

    1. A user may submit a buy order (bid) to a market for q `PPS_c` at price per share p only if the user's `PCB + PPCB_c >= q*p`.

    1. A user may submit a sell order (offer) to a market for q `PPS_c` a price per share p only if the user's `PSC + PPSC_c >= q`.

    1. When an order is filled, the following operations are performed on the user that purchased `q` PS/PPS for `q*p` PC/PPC:

```
            RPCB += q*p
            while sum(q*p for all bid) > APCB:
                cancel lowest priority order
```

1. When an order is filled, the following operations are performed on the user that sold `q` PS/PPS for `q*p` PC/PPC:

```
            RPSC += q
            while sum(q for all offers) > APSC:
                cancel lowest priority order
```

1. If the trigger conditions have not been met by the end date for PPS/PPC class `c`, all users that purchased `PPS_c` with `PPC_c` have the following operation applied:

```
            RPCB -= PPCB_c
```

1. If the trigger conditions have not been met by the end date for PPS/PPC class `c`, all users that sold `PSC_c` for `PPC_c` have the following operation applied:

```
            RPSC -= PPSC_c
```

#### Creation of markets and pseudo-shares

1. There is a single market for PS.

1. Any user may create a market.

1. The creator of the market may specify any trigger conditions and end date.

1. Moderators may remove any markets and cancel the PPS/PPC already exchanged.

#### Company decision making

1. The company may choose to execute the policies proposed by any market at any time.

1. If the end date of a PPS/PPC class is reached and either the LTP or the highest bid is greater than the LTP of the PS/PC market, the company will explain why the policy was not adopted.

#### Initial pseudo-share auction

1. Every day at 00:00:00 UTC, starting on **LAUNCH DATE**, the company will submit a limit order on the PS market to sell `1,000 PS` at `50 PC` per share.

1. The auctions will continue until the company is left with 75% of TPSO.

#### Website details

1. The website is hosted on Heroku's Hobby Plan.

1. One web dyno and one worker dyno are enabled, which costs $14/month.

1. The website uses CloudFlare to provide a DNS and security services.

1. The domain name, liarc.com, is registered at namecheap.com ($5.88/year for the first year, then $10.69/year)