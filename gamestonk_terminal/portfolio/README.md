# PORTFOLIO

This page shows the available brokers for loading in data.  If yours is not listed, please submit a new issue and we will look at adding it.

Current brokers:
* rh : Robinhood 
* alp : Alpaca
* ally: Ally
* dg : Degiro


Once this screen is accessed, the first command to be run is 
````
login [brokers]
````

The brokers should just be your brokers from the list shown on the first menu.  To login to Robinhood, the command is 
````
login rh
````
To login to both robinhood and alpaca:
````
login rh alp
````
After logging in, the help menu will display which brokers you logged into.

Your  login information should be stored as environment variables in [config file](/gamestonk_terminal/config_terminal.py), but check each individual section for specifics.

[ROBINHOOD](#ROBINHOOD)
* [rhhold](#rhhold)
    * Look at current (stock only) holdings
* [rhhist](#rhhist)
    * Get and plot historical portfolio
  
[ALPACA](#ALPACA)
* [alphold](#alphold)
    * Look at current (stock only) holdings
* [alphist](#rhhist)
    * Get and plot historical portfolio

[ALLY](#ALLY)
* [allyhold](#allyhold)
  * Look at Ally Invest Holdings

[DEGIRO](#DEGIRO)
* [cancel](#cancel)
    * Cancel and `Order`
* [companynews](#companynews)
    * Command to get news about a company with it's isin
* [create](#create)
    * Create and `Order`
* [hold](#hold)
    * Command to look at current holdings
* [lastnews](#lastnews)
    * Command to get latest news
* [login](#login)
    * Connect to your Degiro account
* [logout](#login)
    * Disconnect from Degiro account
* [lookup](#lookup)
    * Command to search for a product by name
* [pending](#pending)
    * Command to list pending orders
* [topnews](#topnews)
    * Command to get top news preview
* [update](#update)
    * Update and `Order`

[Merge](#Merge)
* [hold](#hold)
  * View holdings across all brokers
  

## ROBINHOOD <a name="ROBINHOOD"></a>
Robinhood has Two Factor Authentication, so you will likely be prompted to enter a code that is texted/emailed to you.
###NOTE THAT LOGGING IN WILL SAVE A TOKEN TO `os.path.expanduser("~")/.tokens` WHICH CAN BE USED TO LOGIN EVEN IF USER CREDENTIALS ARE INCORRECT.

In order to login, the [config file](/gamestonk_terminal/config_terminal.py) requires two variables:
* "GT_RH_USERNAME"
* "GT_RH_PASSWORD"

### rhhold <a name="rhhold"></a>

Displays current holdings to the console:

````
usage: rhhold
````
There are no additional flags.  This just prints all current stonks, their last price, previous close, your equity and
the % change from previous close.

Example:

![hold](https://user-images.githubusercontent.com/18151143/111685384-3c6ab080-87fe-11eb-80ce-9b256c396bf2.png)

### rhhist <a name="rhhist"></a>
Display your RH portfolio based on provided interval and span.  Based on the API data availablility, plotted as a candlestick chart.
````
usage: rhhist [-s --span] [-i --interval]
````
* -s/--span : How long to look back.  Defaults to the day. Options:
    * [day, week, month, 3month, year, 5year, all]
* -i/--interval: Data resolution. Defaults to 10minute intervals. Options:
    * [5minute, 10minute, hour, day, week]
    
Example Default Output:

![rhhist](https://user-images.githubusercontent.com/18151143/111718919-36da8e00-8831-11eb-99e1-957c8eccb583.png)

## ALPACA <a name="ALPACA"></a>

Alpaca has a nicely maintained python API that is used here.  To login, the best approach is to just save the following environment variables:
* "APCA_API_BASE_URL"
* "APCA_API_KEY_ID"
* "APCA_API_SECRET_KEY"

If these are defined, then the login command does not explicitly need to be run, but the login command verifies all those keys are defined. 

Note that alpaca does support paper trading, so if your base_url is "https://paper-api.alpaca.markets", then this will be importing
your paper trading account.  

### alphold <a name="alphold"></a>

Displays current holdings to the console.  Same as rhhold

````
usage: alphold
````
There are no additional flags.  This just prints all current stonks, their last price, previous close, your equity and
the % change from previous close.

Example Output:
![](https://user-images.githubusercontent.com/18151143/112039340-536d1380-8b1a-11eb-99d1-44d1edd4d54a.png)

### alphist <a name="alphist"></a>
Plots historical portfolio.  The output plot in this case will not be a candle, as the api provides your equity at a gven time,
unlike the robin_stocks, which gives high/low at a given time.

````
usage: alphist [-p --period] [-t --timeframe]
````

* -p/--period The duration of the data in \<number> + \<unit>, such as 1D, where <unit> can be D for day, W for week, M for month and A for year. Defaults to 1M
* -t/--timeframe The resolution of time window. 1Min, 5Min, 15Min, 1H, or 1D. If omitted, 1Min for less than 7 days period, 15Min for less than 30 days, or otherwise 1D.

## ALLY <a name="ALLY"></a>

In order to access your Ally Invest account through the API, you must first acquire a key from 
[your Ally Invest Account](#https://www.ally.com/api/invest/documentation/getting-started/).

Once you have this, you want to load in the following environment variables (similar to alpaca, the api will access these
directly from the environment)
* ALLY_CONSUMER_KEY
* ALLY_CONSUMER_SECRET
* ALLY_OAUTH_TOKEN
* ALLY_OAUTH_SECRET
* ALLY_ACCOUNT_NBR
  * This is obtained from your actual account - not through the api application.

As of writing this, the ally API does not provide historical portfolio data, so only a holdings option is provided.
### allyhold <a name="allyhold"></a>
Show all Ally Invest holdings:
````
usage : allyhold
````
No additional flags.

## DEGIRO <a name="DEGIRO"></a>

**Sub-menu**
````
usage:
    pa
    degiro
    login
````

To use the `degiro` sub-menu you need to :
* Enter `portfolio analysis`, with the command : `pa`
* Enter `degiro` sub-menu using the command: `degiro`
* Connect to `degiro` by typing : `login`

**Credentials**

In order to login you need to provide the following credentials :

| Parameter | Description |
| :--- | :--- |
| DG_USERNAME | Username used to log into Degiro's website. |
| DG_PASSWORD | Password used to log into Degiro's website. |

**2FA**

In order to use 2FA you need to provide one of these parameters :

| Parameter | Description |
| :--- | :--- |
| DG_TOTP_SECRET | This secret key will let Gamestonk generate the `OneTimePassword` for you. |

If you provide your `DG_TOTP_SECRET` : you won't have to type your `OneTimePassword`, Gamestonk will generate it for you at each connection.

`DG_TOTP_SECRET` is the text representation of the `QRCODE` that Degiro's provide you when you enable 2FA.

More information on 2FA credentials, in the documentation of this library :

https://github.com/Chavithra/degiro-connector


### cancel <a name="cancel"></a>
````
usage: cancel ORDER_ID
````

### companynews <a name="companynews"></a>
````
usage: companynews NL0000235190
````

You need to provide the `ISIN` number of the company.
You can get this `ISIN` using the `dglookup` command for instance.

### create <a name="create"></a>
````
usage: create [-a {buy,sell}] (-prod PRODUCT | -sym SYMBOL) -p PRICE (-s SIZE | -up UP_TO) [-d {gtd,gtc}] [-t {limit,market,stop-limit,stop-loss}] [-h]
````

### hold <a name="hold"></a>
````
usage: hold
````

Displays current holdings to the console.


### lastnews <a name="lastnews"></a>
````
usage: lastnews [-l LIMIT] [-h]

optional arguments:
  -l LIMIT, --limit LIMIT
                        Number of news to display.
  -h, --help            show this help message
````

### login <a name="login"></a>
````
usage: login [-u USERNAME] [-p PASSWORD] [-o OTP] [-s TOPT_SECRET] [-h]

optional arguments:
  -u USERNAME, --username USERNAME
                        Username in Degiro's account.
  -p PASSWORD, --password PASSWORD
                        Password in Degiro's account.
  -o OTP, --otp OTP     One time password (2FA).
  -s TOPT_SECRET, --topt-secret TOPT_SECRET
                        TOTP SECRET (2FA).
  -h, --help            show this help message
````

### logout <a name="logout"></a>
````
usage: logout
````

### lookup <a name="lookup"></a>
````
usage: lookup TESLA
````

It will display a result like this :
```
                           name          isin        symbol productType currency  closePrice closePriceDate
0                         Tesla  US88160R1014          TSLA       STOCK      USD     571.690     2021-05-13
1                     Tesla Inc  US88160R1014           TL0       STOCK      EUR     478.000     2021-05-13
2                     Tesla Inc  US88160R1014           TL0       STOCK      EUR     467.650     2021-05-13
3     TurboC O.End Tesla 535,04  DE000VQ1S3R3          None     WARRANT      EUR       3.740     2021-05-13
4      TurboC O.End Tesla 591,4  DE000VQ5R6T9          None     WARRANT      EUR       0.001     2021-05-13
5     TurboP O.End Tesla 685,29  DE000VQ7HWU6          None     WARRANT      EUR       9.870     2021-05-13
6     TurboC O.End Tesla 448,63  DE000VQ1KYG8          None     WARRANT      EUR      10.890     2021-05-13
7       Call 17.12.21 Tesla 392  DE000VP53547  DE000VP53547     WARRANT      EUR       8.960     2021-05-13
8       Call 20.01.23 Tesla 720  DE000VQ68691          None     WARRANT      EUR       1.160     2021-05-13
9  Leverage Shares 1x Tesla ETP  IE00BKT6ZH01          STSL         ETF      GBX     135.550     2021-05-13
```

### pending <a name="pending"></a>
````
usage: pending
````

### topnews <a name="topnews"></a>
````
usage: topnews
````

### update <a name="update"></a>
````
usage: update ORDER_ID -p PRICE
````

## Merge <a name="Merge"></a>

When logged into 1 or more brokers - this option is available.  Currently, this allows for
viewing holding across all platforms.  The PnL is shown for all time (Market Value and your Cost Basis)

### hold <a name = "hold"></a>

Show holdings across all brokers that are logged in.
````
usage: hold
````
Currently no additional flags.  If not all brokers are represented, double check that they are logged in.