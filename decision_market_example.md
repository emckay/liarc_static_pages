# Decision Market Example

Any user can purchase **pseudo-shares** (`PS`) of LIARC. Every month each PS pays the user that holds it `1/N * Monthly Net Income` units of **pseudo-cash** (`PC`), where `N` is the total number of PS held by all users and by LIARC itself.

Jane thinks she would be a better administrator of LIARC than Eric, the current administrator.

Jane creates a market where any user can buy and sell potential-pseudo shares (PPS) with potential pseudo-cash (PPC).

The PPS/PPC class has the trigger condition: "This PPS/PPC converts if LIARC fires Eric and institutes Jane as administrator.'" She sets the end date on the policy as 00:00:00 UTC on January 1 next year.

Users begin buying and selling PPS and PPC. This is an example of how one pair of users (a Jane fan and an Eric fan) might make a decision on how much to spend.

Both the Jane fan, Alice, and the Eric fan, Bob, have 1000 PC and 100 PS in their accounts:

| Person | PS Balance | PC Balance | Fire Eric PPS Balance | Fire Eric PPC Balance |
|:------:|:----------:|:----------:|:---------------------:|:---------------------:|
|Alice   |100         |1000        |0                      |0                      |
|Bob     |100         |1000        |0                      |0                      |


The current price of `1 PS` is `100 PC`. The Jane fan, Alice, believes that Jane as administrator will increase net income by at least 5%. Therefore, she should be willing to spend more than `105 PC` to get `1 PS` if Jane becomes an administrator. The Jane fan submits a limit order to buy `1 PPS` for `105 PPC`.

The Eric fan, Bob, is certain that a Jane administration will not make as much net income as the current Eric administration. That is, Bob believes that the price of `1 PS` will go below `100 PC` if Jane becomes administrator. Bob decides that getting `105 PC` for `1 PS` is a great deal if Jane becomes the administrator, so he submits an order to sell `1 PPS` for `105 PPC`.

Alice's order and Bob's order are filled. Now Alice owns `1 PPS` and `-105 PPC`, and Bob owns `-1 PPS` and `105 PPC`. Neither of their PS or PC balances have been changed because the trigger condition has not been met.

| Person | PS Balance | PC Balance | Fire Eric PPS Balance | Fire Eric PPC Balance |
|:------:|:----------:|:----------:|:---------------------:|:---------------------:|
|Alice   |100         |1000        |1                      |-105                   |
|Bob     |100         |1000        |-1                     |105                    |

Other traders make similar decisions and the PPS price settles down. LIARC then compares the PPS price to the PS price.

#### The PPS price is consistently higher than the PS price

If the PPS price is higher than the PS price[^1], LIARC decides to fire Eric, as the aggregated wisdom of the market participants indicate that LIARC will earn more net income under Jane.

When Eric is fired, all PPS and PPC are converted. Alice and Bob's balances look like this:

| Person | PS Balance | PC Balance | Fire Eric PPS Balance | Fire Eric PPC Balance |
|:------:|:----------:|:----------:|:---------------------:|:---------------------:|
|Alice   |101         |895         |0                      |0                      |
|Bob     |99          |1105        |0                      |0                      |

#### The PPS remains lower than the PS price until Jan 1

In this case, LIARC decides not to fire Eric. At 00:00:00 UTC Jan 1 (the end date Jane set on the PPS/PPC market), the PPS and PPC are canceled. Alice and Bob's final balances look like this:

| Person | PS Balance | PC Balance | Fire Eric PPS Balance | Fire Eric PPC Balance |
|:------:|:----------:|:----------:|:---------------------:|:---------------------:|
|Alice   |100         |1000        |0                      |0                      |
|Bob     |100         |1000        |0                      |0                      |

[^1]: The PPS price does not have to be much higher to convince LIARC to adopt the proposal. If the proposal seems likely to be adopted soon, the price of actual PS will converge to the PPS price.
