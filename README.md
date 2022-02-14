# betting-app
gui app for betting system

### *English version*

<details>
<summary>Table of contents</summary>

- [Installation](#installation)
- [Initialization](#initialization)
- [Examples of use](#examples-of-use)
  * [Example 1: Bonus received in all cases if you bet on a given match](#example-1-bonus-received-in-all-cases-if-you-bet-on-a-given-match)
  * [Example 2: Bonus received in all cases if you bet on any match, possibly from a given competition / sport](#example-2-bonus-received-in-all-cases-if-you-bet-on-any-match-possibly-from-a-given-competition--sport)
  * [Example 3: Bonus received if at least *n* matches are won](#example-3-bonus-received-if-at-least-n-matches-are-won)
  * [Example 4: Bonus received if a bet is lost.](#example-4-bonus-received-if-a-bet-is-lost)
- [Converting freebets to cash](#converting-freebets-to-cash)
  * [Method 1: If you only have fractionable freebets](#method-1-if-you-only-have-fractionable-freebets)
  * [Method 2: If you have a few non-fractionable freebets and enough fractionable freebets](#method-2-if-you-have-a-few-non-fractionable-freebets-and-enough-fractionable-freebets)
  * [Method 3: If you have non-fractionable freebets and not enough fractionable freebets](#method-3-if-you-have-non-fractionable-freebets-and-not-enough-fractionable-freebets)
</details>

## Installation
- Download the repository
- Go to the root of the repository
```bash
pip install -r requirements.txt
```
- Launch python3
```python
>>> import sportsbetting
>>> from sportsbetting.user_functions import *
```
- You can now use any function from [user_functions.py](https://github.com/pretrehr/Sports-betting/blob/master/sportsbetting/user_functions.py)



## Initialization
Before you can fully use all the functions in the [user_functions.py](https://github.com/pretrehr/Sports-betting/blob/master/sportsbetting/user_functions.py) file, it is necessary to initialize the database of matches you can potentially bet on. For example, if you only want to focus on French Ligue 1 matches, and you only want to bet on Betclic and Winamax, you would write :
```python
>>> parse_competition("ligue 1 france", "football", "betclic", "winamax")
```
Note that if you do not specify a bookmaker, the algorithm will retrieve the odds of the matches of the competition from most of the bookmakers approved by the ARJEL (French Regulatory authority for online games).
If you do not have a specific competition or target sport, it is advisable to call
```python
>>> parse_football()
```
or, for example,
```python
>>> parse_football("betclic", "winamax")
```
These functions will retrieve the odds of the matches available at the various bookmakers for the 5 major European football championships (Ligue 1, Premier League, LaLiga, Serie A, Bundesliga), as these competitions generally generate the highest returns.
It should be pointed out that in the case of tennis where there is no fixed competition, it is necessary to call the function:
```python
>>> parse_tennis()
```
or, for example,
```python
>>> parse_tennis("betclic", "winamax")
```
This command will retrieve the odds of tennis matches from the competitions currently being played.

You will find below a summary table of the different French bookmakers with the associated string in the package.

| Bookmaker | String |
| ------------- | ------------- |
| [Betclic](https://www.betclic.fr/sport/) | ``"betclic"" |
| [BetStars](https://www.betstars.fr/) | ``betstars"` |
| [Bwin](https://sports.bwin.fr/fr/sports) | "bwin" |
| [France Pari](https://www.france-pari.fr/) | `"france_pari"` |
| [JOA](https://www.joa-online.fr/fr/sport) | "joa"` |
| [NetBet](https://www.netbet.fr/) | `"netbet"` |
| [Pariels Sport](https://www.enligne.parionssport.fdj.fr/) |
| [PasinoBet](https://www.pasinobet.fr/) ||"pasinobet"` |
| [PMU](https://paris-sportifs.pmu.fr/) |"pmu"` |
| [Unibet](https://www.unibet.fr/sport) |
| [Winamax](https://www.winamax.fr/paris-sportifs/sports/) |
| [Zebet](https://www.zebet.fr/fr/) |

*Note bene*: It is currently not possible to use the package for the following bookmakers:
- [Feelingbet](https://feelingbet.fr/) which offers very few promotions. But if necessary, the odds available on this site are identical to those available on [France Pari](https://www.france-pari.fr/).
- [Genybet](https://sport.genybet.fr/) which focuses more on horse betting rather than sports betting.
- Vbet](https://www.vbet.fr/paris-sportifs) which offers numerous but very restrictive and therefore unprofitable promotions.


## Examples of use

The bonuses received are almost always freebets (or paris gratuits). Such a bonus means that when you bet, for example, on an odds equals to 3 with a €10 freebet, if the bet turns out to be winning, then you get 3×10-10 = €20 (compared to €30 for a standard bet), so it is equivalent to betting with a normal bet on an odds reduced by 1.
We will see later that it is possible to win back 80% of the value of a freebet with certainty. Thus, a €10 freebet is equivalent to 10 × 0.8 = €8.

The examples below are examples of promotions that regularly appear at the different bookmakers. You will find a description of how to get the most out of these promotions. This list is naturally not exhaustive and it is up to you to adapt these examples to the conditions of the promotions you will encounter in the future.
### Example 1: Bonus received in all cases if you bet on a given match
France-pari very often offers a promotion which consists in betting a €20 bet on a specific Ligue 1 match at a minimum odds of 2 to receive a €5 freebet.
For example, for the 20th round of Ligue 1 2019/2020, the proposed match was Dijon - Metz.
You just have to execute:
```python
>>> parse_competition("ligue 1", "football") #If not previously executed
>>> best_stakes_match("Dijon - Metz", "france_pari", 20, 2, "football")
```
If the minimum raise is more than 5 × 0.8 = €4, then this promotion is profitable and we can split our bets as specified.


### Example 2: Bonus received in all cases if you bet on any match, possibly from a given competition / sport
If there are no conditions on the game to be played, we can execute
```python
>>> parse_football()
```
because, as mentioned above, it is these matches that will generate the most returns.

If we're betting on an NBA game, we can execute...
```python
>>> parse_competition("nba", "basketball")
```

If we're going to bet on a tennis match, we can execute
```python
>>> parse_tennis()
```

It should also be noted that it is generally not possible to retrieve the odds of all the matches of a given sport, as this can be very costly in terms of execution time. It is therefore necessary to choose the most popular competition(s) for the chosen sport, as it is these matches that will generate the most returns.
The table below indicates which function to call for each sport.



| Sport | Function to call |
| ------------- | ------------- |
| Football | `parse_football()` |
| Basketball | `parse_nba()` (short for `parse_competition("nba", "basketball")`)|
| Tennis | `parse_tennis()` |
| Rugby | `parse_competitions(["top 14", "champions cup", "six nations"], "rugby")` |
| Ice Hockey | `parse_nhl()` (short for `parse_competition("nhl", "hockey-sur-glace")`) |

Once you have chosen your set of matches, you can then use the `best_match_under_conditions` function.

For example, France-pari regularly offers a promotion which consists in reimbursing, in freebet, 10% of the stakes engaged on odds higher than 1.70 and within a limit of €100 reimbursed. The objective is then to bet €1000 in order to recover the maximum bonus. Thus, if we assume that the promotion takes place between January 3, 2020 at midnight and January 12, 2020 at 11:59 pm, we can execute :
```python
>>> best_match_under_conditions("france_pari", 1.7, 1000, "football", "3/1/2020", "0h00", "12/1/2020", "23h59")
```
If the displayed loss is less than 100 × 0.8 = €80, then this promotion is profitable and we can distribute our stakes as described.

### Example 3: Bonus received if at least *n* matches are won
Please note: From this example, we'll assume that we previously called a function to initialize the desired matches (of the type `parse_...`), according to the needs of the promotion.

Sometimes it is necessary to win a certain number of bets to receive a bonus. In these cases, it is necessary to bet on the same bookmaker on each of the outcomes of the same match. For example, Betclic sometimes offers to receive a €10 freebet for 3 bets won, of €5 each, bet on odds of at least 1.7 and placed on 3 different matches. You can then execute
```python
>>> best_match_pari_gagnant("betclic", 1.7, 5, "tennis")
```
This feature will give you the best match to bet on at any given time. If the displayed loss is less than (10 × 0.8)/3, then we can assume that this promotion is profitable and we can distribute our stakes as described. This will be the first winning bet. A little later (e.g. when the match in question has been played), we can repeat this procedure to find out which match we need to play to win the 2nd bet in the series and so on until we reach 3 winning bets. Note that in this case, it is necessary to re-execute the `parse_tennis` function once you have won a bet, because otherwise the result returned by the `best_match_pari_gagnant` function would be identical to the result of the previous execution.

### Example 4: Bonus received if a bet is lost.
This is a very common type of promotion, especially but not only for welcome offers.
For example, in the summer of 2018, Winamax offered its users to pay back in cash (not freebet) 100% of their first bet of €200 maximum if they lost. It should also be noted that on Winamax (as on most other bookmakers), you can only bet on odds greater than (or equal to) 1.10. You can then run
```python
>>> best_match_cashback("winamax", 1.1, 200, "football", freebet=False)
```

## Converting freebets to cash
### Method 1: If you only have fractionable freebets
At the following bookmakers: Betclic, Unibet, ParionsSport and Zebet, it is possible to split the won freebets into several bets. For example, if you have won €10 in freebets, you can bet €8 and then €2.
To optimize the win, the idea will be to cover all the outcomes with freebets on a combination of 2 football matches (which represents 9 outcomes to cover).
For example, if you have €10 of freebet, you will then run
```python
>>> best_matches_freebet_one_site("betclic", 10)
```

### Method 2: If you have a few non-fractionable freebets and enough fractionable freebets
At all bookmakers except Betclic, Unibet, ParionsSport and Zebet, it is not possible to split a freebet into several bets. 
Let's suppose that you have €100 fractionable freebets on Betclic, 1 freebet of €10 on Winamax and 2 freebets of €5 on France-pari. 
The idea would then be to spread our €100 of fractionable freebets over a combination of 2 games as in method 1, then replace (totally or partially) some stakes with non-fractionable freebets from other bookmakers. In this way we spread the non-fractionable freebets over several outcomes of a handset and compensate the differences with fractionable freebets. So you are likely to have some outcomes covered only by a non-fractionable freebet, others covered partly by a non-fractionable freebet and a fractionable freebet, and others covered only by a fractionable freebet. In this example, we then execute
```python
>>> best_matches_freebet(["betclic"], [[10, "winamax"], [5, "france_pari"], [5, "france_pari"])
```
This second method should, where possible, be the preferred method when non-fractionable freebets are available, as it offers the best return. The first method is also very profitable but it is preferable to keep the fractionable freebets in order to apply the second method when you have non-fractionable freebets.

### Method 3: If you have non-fractionable freebets and not enough fractionable freebets
At all bookmakers except Betclic, Unibet and Zebet, freebets have a time limit, ranging from 2-3 days for Bwin, to 1 month for France-pari. It may therefore be necessary to bet them quickly and there may not be enough fractionable freebets to be able to apply the 2nd method. In this situation, it is more efficient to bet on a single match rather than on a combination. It is also necessary to cover a freebet with real money. Furthermore, with this method, you can only play one freebet at a time. So if you have a €15 freebet at Betstars, then you can execute
```python
>>> best_match_freebet("betstars", 15, "football")
```
Please note: Be careful not to confuse `best_match_freebet` with `best_matches_freebet`.

Note that some sites like NetBet or PMU sometimes offer freebets that are only playable on a single sport. In this case, you have to adapt the sport to the situation.
This method is on average much less profitable and much more volatile than the first two. The first two methods ensure a return rate of between 77 and 85% of the sum of freebets placed.  With the third method, a return rate between 55 and 70% is to be expected.


## Disclaimer
This project aims to help the user to free up money by getting as close as possible to the absence of risk. Nevertheless, it is important to specify that zero risk does not exist and that the odds published by bookmakers are intended to evolve over time. It is therefore your responsibility to make sure that the information displayed by the Sports-betting application is reliable. As the creator, I cannot be held responsible for any loss of capital that may occur during the use of the application.
