# hitbtc_wss
HitBTC Websocket API 2.0 Client written in Python 3.

Forked from [Crypto-toolbox/hitbtc_wss](https://github.com/Crypto-toolbox/hitbtc_wss).  At the time of this writing,
Crypto-toolbox had not accepted a pull request into his repo in 7 months.  Seems like they might have
moved on to do other things.  I figured I'd pick the ball up and run with it.  

# Release History


| Date     | Description  |
| -------- | ------------ |
| 09/02/2018 | Published bug fix to PyPi so the `login` method works with HitBTC v2 login method. |

The client supplies data both visually via console, as well as python objects via its `HitBTC.recv()`.
It's important to note that this does not receive data from the API directly -
instead, the data is pulled from a `queue.Queue` object, which defaults to a length of
100 items. So only the last 100 messages will be cached - either make sure you process the messages
fast enough, or increase the length of the queue (can be done by passing the `q_maxsize` kwarg on
instantiation).

By default, data is unpacked - that means you will never see the raw `JSONRPC` message
(this, too, can be turned off by passing `raw=True` upon initialization). This will, however, also
turn off all handling of error messages etc.

For an in-depth description of the client and its methods, please see the documenation at
[readthedocs.org](http://hitbtc-websocket-api-20-client.readthedocs.io/en/latest/)


# Installation

Stable: `pip install hitbtc_wss`
Release Candidate: `pip install --pre hitbtc_wss`

# Example Usage

```python
import time
import queue
from hitbtc_wss import HitBTC
c = HitBTC()
c.start()  # start the websocket connection
time.sleep(2)  # Give the socket some time to connect
c.subscribe_ticker(symbol='ETHBTC') # Subscribe to ticker data for the pair ETHBTC

while True:
    try:
        data = c.recv()
    except queue.Empty:
        continue

    # process data from websocket
    ...

c.stop()
```




