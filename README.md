# creator-fund-report
 
## adbr.csv 

adbr.csv (all data, both rounds) is the full analytics dataset for rounds 3 and 4 with wallet flags attached to aid scoring

a flag is not necessarily an indicator of suspicion, just a binary variable

flags are provided in cases where:
- low_tx_wallet			- wallets have a combined transaction count between EVM and Cosmos < 10
- young_wallet 			- wallets were found to have been created after the kickoff time of both rounds
- sus_day_wallet 		- wallets were found to have been created on 2024-08-29, a day where a wildly abnormal amount of wallets were created (10x a normal day)
- lazy_bot 				- wallets that were found to have voted at the exact same second as other wallets for the same project
- prolific_funder		- wallet was initially funded by a wallet that also funded between 11 and 99 other wallets (and is not a known CEX)
- low_balance_wallet	- wallet has under 1.25 $Sei remaining in it since voting

two calculations are made off the above flags: model_score and model_score_count
- model_score 			- this is a calculated score between 0 and 1 based on the above flags, with weights for each. the scoring mechanism is forgiving for the prolific funder flag, while adding extra weight on low_tx_count and lazy_bot
- model_score_count		- this is just the sum of the binary flags to get a rough estimate of how many flags a wallet has, and for understanding whether distribution is fair


## dodgy_wallets.csv

dodgy_wallets.csv is a list of all wallets with a model_score_count > 2 and a model_score > 0.5. the 0.5 threshold can be considered a "more suspicious than not limit" for inclusion. the score count of 3 or more is there because it's possible for any organic wallet to raise combinations of any 2 flags in the wild, but 3 or more  would start to raise suspicion.

we have identified 2256 wallets which would be considered "worthy of investigation"


## dodgy_projects.csv

dodgy_projects.csv is the above data rolled up to a project level. it is recommended to sort this list by descending model score to see the most egregious offenders. a more compelling can be seen by sorting by descending score per donor. many of the same names appear at the top of both, but the score per donor option appears more effective as to our understanding of which projects were being manipulated more intentionally.

this csv also has a round_id filter to allow you to slice between round 3 (11) and round 4 (12)




notes:

- a high model score or score per donor is not a 100% accurate indicator of suspicion or malicious activity, but almost definitely means a project is "worth of investigation"
- more insights will be provided in the full fraud report to follow in the coming days