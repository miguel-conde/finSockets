# finSockets

R package for querying and trading all kinds of financial products over the APIs of web services using a shared repository of API interfaces

## Introduction

The idea behind this initiative is sharing the know-how to operate financial APIs in a machine understandable way. Places like [programmableweb]( http://www.programmableweb.com/category/financial/apis?category=19968
) do a great job collecting information intended for programmers, but this tool is intended for analysts and investors. The intention is creating an abstraction of all services and products.

The following is valid code:

#### Example 1
```R
usd <- fins.product(currency.fiat, 'USD')
btc <- fins.product(currency.digital, 'BTC')

myservice <- fins.service('CoinDesk')

cat('The current bitcoin price is', fins.currentprice(myservice, btc, unit = usd), 'usd.')
```

Of course, changing 'CoinDesk' for 'Google' also works, completely isolating the programmer from the API details.

Furthermore, if I have 50 financial services (banks, fintechs, exchanges or just search engines) and want to check if any of them is offline, this works.

#### Example 2
```R
if (any(!fins.isonline(myservices))) warning('Some services are offline')
```

And not just that, the abstraction is also valid to trade in services as in:

#### Example 3
```R
mymoney  <- fins.service('Coinbase')
me_cb    <- fins.credentials(mymoney, 'Santiago')
lendserv <- fins.service('BTCjam')
me_bj    <- fins.credentials(lendserv, 'Santiago')

# By some means I find a product I want to buy. x, y, z represents whatever I need to make that choice 
# and bargain is a list including the fields product = some_ID, amount = 0.5 and unit = BTC.

bargain <- MyDigitalP2pRiskEvaluationFunction(lendserv, x, y, z)

# I can buy the product writing:

payserv <- fins.payment(mymoney, me_cb, amount = bargain$amount, unit = bargain$unit)
result  <- fins.buy(lendserv,
                    bargain$product, bargain$amount, unit = bargain$unit, 
					credential = me_bj, 
					payment = payserv)
```

And, if everything goes right, result will contain status = fins.OK and a product of class bond.private, location 'BTCjam', service 'BTCjam', ID ^some_code^ with properties including nominal_amount, nominal_unit, expiration, purchase_date and more.

Also when I check my assets at 'Coinbase' I will have 0.5 BTC less and when I check my assets at 'BTCjam' I will be the proud owner of that private bond. 

Also, the package will be updated like a normal R package (hopefully in Cran someday) but the information about products and services is kept in its own repositories. The latter is partially data, like: the list of all existing services, global products, the relation between them, what is supported by each service or where to find the documentation, etc. and partially code: R functions understanding APIs to buy/sell products, etc.

So the complete system will be:

- The package itself: finSockets
- Data about products, services and APIs stored in [Google Public Data](https://developers.google.com/public-data/)
- Driver functions operating the APIs stored in GitHub. For security reasons, these functions will be under public scrutiny and restricted write access.
- User credentials. Are local to each user. The package will support encrypting them with a keychain password that should never be stored in the same computer and treat them as sensitive material with an as-short-as-possible lifetime and never stored as plaintext.

As you may have noticed, this is a very ambitious goal. A task that requires collaboration! So this is an opportunity to share everything I know about financial services with other people who know even more and put everything together for the following rewards:

1. The package itself. Allowing to invest and create fintech business
2. The package itself. Allowing others to correctly and efficiently use our fintech business
3. Donations and license sales distributed in full among contributors on a pure meritocratic basis

## Product classes initially included

In the first version of this package we intend to focus on products that represent value and can be traded over the internet. We do not contemplate purely transactional systems like payment platforms or remittance in this first version, but do contemplate to include them in the future. This means, you can buy or sell products on a platform using funds you have on that same platform or using bitcoin. The package includes all necessary to sign and verify bitcoin payments, provide or withdraw funds in bitcoin and convert between bitcoin and other fiat or digital currencies.

The products fall in one of these categories:

- stock (equities and securities)
- bond (government, corporate or private)
- commodity (soft and hard)
- derivatives (forward contracts, futures, options, swaps)
- insurance (buy and sell)
- currency (fiat and digital)
- composed product (a set of any of the above)
- funds (you need to buy any of the above and get when you sell it)

(This list is a draft; see the source code for a full updated list.)

## Intended timeline

There is only one chance to make a first impression, so we do not intend to do any promotion of this package until we have a version 0.1 that provides a nice user experience to any evaluator. The list of services may be short, 
but it has to implement enough functionality to win its own case in terms of usefulness, documentation and consistency. I.e., move people into willing to use it and collaborate.

We expect to have this 0.1 version by the end of September 2015. In the meantime, before the version reaches 0.1, we will commit to this repo and update service data to Google (download the package to see where), but the package 
should not be considered complete work.

When version 0.1 is released, we will submit it to Cran and R-forge, try to promote it in related internet sites and contact the owners of services.

## Other languages

Especially Python, are welcome. We develop everything using the widest possible conventions to make the repositories of services and products language independent. We will have snippets connecting to the services for other languages (at least Python), but need extra help to elevate that into viable packages.

## License and decisions

GNU General Public License, version 2 [link](https://www.gnu.org/licenses/old-licenses/gpl-2.0.en.html)

Less restrictive forms of licensing may be provided upon request to reward contributors on a purely meritocratic basis.

Decisions on the development of this package will also be voted on a meritocratic basis.

## Contributors

Are welcome!!

In all possible areas, including (in no particular order):

- Core software development
- Migrating code to other languages (Python as the first candidate)
- Creating interfaces for new services
- Beta testing software and services
- Reviewing, suggesting features, helping improve usability, consistency and conventions
- Creating doc and examples
- Whatever services you may think of that use or promote finSockets
