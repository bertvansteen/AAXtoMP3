# AAXtoMP3
Read the original complete instruction @ https://github.com/KrumpetPirate/AAXtoMP3

This is a (much) shorter summary of MacOS practice.

Essentially you want to find your 8 char long AuthCode by any means.
The AuthCode remains same for same Audible account, across all your purchased Audible audiobooks.
The next step is run 
```
./AAXtoMP3 -A XYZ12345 example.aax
```
where XYZ12345 is to be replaced with your AuthCode and example.aax is downloaded from Audible.com

## prerequisite
* ffmpeg
```
brew install ffmpeg
```

## AuthCode
https://github.com/inAudible-NG/audible-activator is an example of getting your AuthCode. It uses selenium to automate Chrome.
Essentially you want to manage yourself to get a link like:

```python3
url = "https://www.audible.com/license/licenseForCustomerToken?customer_token=ABCDEFG...="
```

The easiest way in my opinion is to ask iTunes to open the link for you. When first time example.aax is loaded into iTunes, iTunes is smart enough to ask you about authorizing iTunes from Audible by leading you to browse some url like:

```
https://www.audible.com/player-auth-token?playerId=abcdefg....&playerManufacturer=itunes&playerModel=mac&playerType=software&serial=&
```
In this page, a button <authorize iTunes> redirects you to 
    
```python3
url_itms = "itms://www.audible.com/license/licenseForCustomerToken?customer_token=ABCDEFG...="
```
change itms to https, you get ```url```

next step is to make a request:

```python3
import requests
r = requests.get(url, headers = {'User-Agent': "Audible Download Manager"})

import common
auth_code, _ = common.extract_activation_bytes(r.content)
```


