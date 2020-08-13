# [`twint`](https://github.com/twintproject/twint) Documentation

---

## Installation

```bash
# git:
git clone https://github.com/twintproject/twint.git
cd twint
pip3 install . -r requirements.txt

# or pip:
pip3 install twint

# or
pip3 install --user --upgrade git+https://github.com/twintproject/twint.git@origin/master#egg=twint

# or pipenv:
pipenv install git+https://github.com/twintproject/twint.git#egg=twint
```

---

## Use examples

#### 1. Get the 20 most recent tweets from mkbhd including the word 'lemon'

```bash
# CLI
twint -s 'lemon from:mkbhd' --limit 20
```

```py
# Python
import twint

c = twint.Config()

c.Search = "lemon from:mkbhd"
c.Limit = 20

twint.run.Search(c)
```

#### 2. Get the most recent 1000 tweets from verified users, that include at least one of 'lemon' and 'juice'

```bash
# CLI
twint -s 'lemon OR juice' --limit 1000 --verified
```

```py
# Python
import twint

c = twint.Config()

c.Search = "lemon OR juice"
c.Verified = True
c.Limit = 1000

twint.run.Search(c)
```

#### 3. Get the 20 most recent tweets that include 'house', have at least a photo or video, are geocoded near the white house, and save them to 'output.txt' in csv format

```bash
# CLI
twint -s 'house' -g "38.896479,-77.0369865,1mi" --limit 20 --media -o output.txt --csv
```

```py
# Python
import twint

c = twint.Config()

c.Search = 'house'
c.Limit = 20
c.Geo = '38.896479,-77.0369865,1mi'
c.Media = True
c.Output = 'output.txt'
c.Store_csv = True

twint.run.Search(c)
```

---

## Options

***Minimum use flags [min 1 required]***

Name | Required&nbsp;flag&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; | Example&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; | Explanation
---|---|---|---
Search | `-s` `--search` <br><br> `Search (string)` | `twint -s lemon`<br> `twint -s "lemon juice good"` <br> `twint -s " 'lemon juice' good"` <br><br> `c.Search = "lemon"` <br> `c.Search = "lemon juice good"` <br> `c.Search = " 'lemon juice' good"` |  Matches the search query. Use `""` to enclose multiple terms, and `''` for phrases inside the arg string. [All operators](https://developer.twitter.com/en/docs/tweets/search/guides/standard-operators)
Geocode | `-g` `--geo` <br><br> `Geo (string)` | `twint -g 48.880048,2.385939,1km` <br><br> `c.Geo = '48.880048,2.385939,1km'` | Matches geocoded tweets. `LONG,LAT,RAD` format, use either `km` or `mi`
Near | `--near` <br><br> `Near (string)` | `twint --near london` <br><br> `c.Near = 'london'` | Matches near a city
Username | `-u` `--username` <br><br> `Username (string)` | `twint -u mkbhd` <br><br> `c.Username = 'mkbhd'` | Matches tweets from this user. Limited to 3200 results. Very slow, use `from:username` in a search query instead
Userlist | `--userlist` | `twint --userlist "bobby jack mkbhd"` | Matches tweets from listed users

***Output flags***

Name | Flag&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; | Example&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; | Explanation
---|---|---|---
Output | `-o` `--output` <br><br> `Output (string)` | `twint -s lemon -o output.txt` <br><br> `c.Search = 'lemon'` <br> `c.Output = 'output.txt'` | Writes output to file
CSV | `--csv` <br><br> `Store_csv (bool)` | `twint -s lemon --csv` <br><br> `c.Search = 'lemon'` <br> `c.Store_csv = True` | If `-o` set, write file in csv format
JSON | `--json` <br><br> `Store_json (bool)`| `twint -s lemon --json` <br><br> `c.Search = 'lemon'` <br> `c.Store_json = True` | If `-o` set, write file in json format
Limit | `--limit` <br><br> `Limit (int)` | `twint -s lemon --limit 1020` <br><br> `c.Search = 'lemon'` <br> `c.Limit = 1020` | Limits any output type amount to next multiple of 20
Format | `--format` <br><br> `Format (string)` | `twint -s lemon --format "user: {username}, tweet: {tweet}"` <br><br> `c.Search = 'lemon'` <br> `c.Format = "user: {username}, tweet: {tweet}"` | Formats the text output. [All format keys](https://github.com/twintproject/twint/wiki/Tweet-attributes). Newlines are removed
Database | `-db` `--database`<br><br> `Database (string)` | `twint -s lemon -db out.db` <br><br> `c.Search = 'lemon'` <br> `c.Database = 'out.db'` | Stores results in sqlite3 database
Hide output | `-ho` `--hide-output` <br><br> `Hide_output (bool)` | `twint -s lemon -ho` <br><br> `c.Search = 'lemon'` <br> `c.Hide_output = True` | Doesn't print terminal output
Translate | `--translate` | `twint -s lemon --translate --lang --translate-dest` | Enable translating tweets with Google translate. Must be used with `--lang`
Translation <br> destination | `--translate-dest` | `twint -s lemon --translate --translate-dest` | Sets destination translation language, see ISO2 [language codes](https://github.com/twintproject/twint/wiki/Langauge-codes). Must be used with `--lang`
Lowercase | `--lowercase` <br><br> `Lowercase (bool)` | `twint -s lemon --lowercase` <br><br> `c.Search = 'lemon'` <br> `c.Lowercase = True` | Automatically makes output lowercase

***Specifier flags***

Name | Flag&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; | Example&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; | Explanation
---|---|---|---
Verified | `--verified` <br><br> `Verified (bool)` | `twint -s lemon --verified` <br><br> `c.Search = 'lemon'` <br> `c.Verified = True` | Matches if user is verified. Must use with `-s`
Native <br> retweets | `-nr` `--native-retweets` <br><br> `Native_retweets (bool)` | `twint -s lemon -nr` <br><br> `c.Search = 'lemon'` <br> `c.Native_retweets = True` | Matches if retweet
Filter <br> retweets | `-fr` `--filter-retweets` <br><br> `Filter_retweets (bool)` | `twint -s lemon -fr` <br><br> `c.Search = 'lemon'` <br> `c.Filter_retweets = True` | Matches if not retweet
Resume | `--resume` <br><br> `Resume (string)` | `twint -s lemon --resume 'scroll_id.txt'` <br><br> `c.Search = 'lemon'` <br> `c.Resume = 'scroll_id.txt'` | Automatically saves last scroll ID, and resumes next search from the scroll ID in file
Min likes | `--min-likes` <br><br> `Min_likes (int)` | `twint -s lemon --min-likes 30` <br><br> `c.Search = 'lemon'` <br> `c.Min_likes = 30` | Matches if has more than minimum likes
Min retweets | `--min-retweets` <br><br> `Min_retweets (int)` | `twint -s lemon --min-retweets 30` <br><br> `c.Search = 'lemon'` <br> `c.Min_retweets = 30` | Matches if has more than minimum retweets
Min replies | `--min-replies` <br><br> `Min_replies (int)` | `twint -s lemon --min-replies 30` <br><br> `c.Search = 'lemon'` <br> `c.Min_replies = 30` | Matches if has more than minimum replies
Source | `--source` <br><br> `Source (string)` | `twint -s lemon --source "Twitter for Android"` <br><br> `c.Search = 'lemon'` <br> `c.Source = 'Twitter for Android'` | Matches twitter client name
Links | `--links` <br><br> `Links (string)` | `twint -s lemon --links include` <br> `twint -s lemon --links exclude` <br><br> `c.Search = 'lemon'` <br> `c.Links = 'include'` <br> `c.links = 'exclude'` | Matches tweets with or without links
Popular <br> tweets | `-pt` `--popular-tweets` <br><br> `Popular_tweets (bool)` | `twint -s lemon -pt` <br><br> `c.Search = 'lemon'` <br> `c.Popular_tweets = True` | Sorts by popular instead of recent
Language | `-l` `--lang` <br><br> `Lang (string)` | `twint -s lemon -l no` <br><br> `c.Search = 'lemon'` <br> `c.Lang = 'no'` | Matches given language, see ISO2 [language codes](https://github.com/twintproject/twint/wiki/Langauge-codes)
Email | `--email` <br><br> `Email (bool)` | `twint -s lemon --email` <br><br> `c.Search = 'lemon'` <br> `c.Email = True` | Matches if *might* have an email (not reliable)
Phone | `--phone` <br><br> `Phone (bool)` | `twint -s lemon --phone` <br><br> `c.Search = 'lemon'` <br> `c.Phone = True` | Matches if *might* have a phone number (not reliable)
All | `--all` <br><br> `All (string)` | `twint --all mkbhd` <br><br> `c.All = 'mkbhd'` | Gets all tweets associated with user
Since | `--since` <br><br> `Since (string)` | `twint -s lemon --since 2015-12-21` <br><br> `c.Search = 'lemon'` <br> `c.Since = '2015-12-21'` | Matches tweets after date. `YYYY-MM-DD` format, can just use `since:YYYY-MM-DD` in query string instead.
Until | `--until` <br><br> `Until (string)` | `twint -s lemon --until 2015-12-21` <br><br> `c.Search = 'lemon'` <br> `c.Until = '2015-12-21'` | Matches tweets before date. `YYYY-MM-DD` format, can just use `until:YYYY-MM-DD` in query string instead.
Year | `--year` <br><br> `Year (string)` | `twint -s lemon --year 2015` <br><br> `c.Search = 'lemon'` <br> `c.Year = '2015'` | Matches tweets before the year
Videos | `--videos` <br><br> `Videos (bool)` | `twint -s lemon --videos` <br><br> `c.Search = 'lemon'` <br> `c.Videos = True` | Matches tweets with videos
Images | `--images` <br><br> `Images (bool)` | `twint -s lemon --images` <br><br> `c.Search = 'lemon'` <br> `c.Images = True` | Matches tweets with images
Media | `--media` <br><br> `Media (bool)` | `twint -s lemon --media` <br><br> `c.Search = 'lemon'` <br> `c.Media = True` | Matches tweets with images or videos
Custom <br> query | `--cq` `--customquery` | `twint -s lemon -cq juice` | Custom search query, not sure how its different to `-s`. Must be used with a required flag

***User flags***

Name | Flag | Example | Explanation
---|---|---|---
Followers | `--followers` | `twint -u mkbhd --followers` | Gets users followers. Very slow. Must use with `-u`
Following | `--following` | `twint -u mkbhd --following` | Get who user follows. Very slow. Must use with `-u`

***Connection flags***

Name | Flag | Example | Explanation
---|---|---|---
Proxy type | `--proxy-type` <br><br> `Proxy_type (string)` | `twint -s lemon --proxy-type HTTP --proxy-host 192.168.1.10 --proxy-port 8080` <br><br> `c.Search = 'lemon'` <br> `c.Proxy_type = 'HTTP'` <br> `c.Proxy_host = '192.168.1.10'` <br> `c.Proxy_port = 8080` | Sets proxy type
Proxy host | `--proxy-host` <br><br> `Proxy_host (string)` | `twint -s lemon --proxy-type HTTP --proxy-host 192.168.1.10 --proxy-port 8080` <br><br> `c.Search = 'lemon'` <br> `c.Proxy_type = 'HTTP'` <br> `c.Proxy_host = '192.168.1.10'` <br> `c.Proxy_port = 8080` | Sets proxy hostname or IP
Proxy port | `--proxy-port` <br><br> `Proxy_port (int)` | `twint -s lemon --proxy-type HTTP --proxy-host 192.168.1.10 --proxy-port 8080` <br><br> `c.Search = 'lemon'` <br> `c.Proxy_type = 'HTTP'` <br> `c.Proxy_host = '192.168.1.10'` <br> `c.Proxy_port = 8080` | Sets proxy port
Tor proxy | `--proxy-host` | `twint -s lemon --proxy-host tor` | Specific proxy settings for tor
? | `Tor_control_port (?)` | ? | ?
? | `Tor_control_password (?)`  | ? | ?

***Broken flags***

Flag | Example | Issue | Explanation
---|---|---|---
`--retweets` | `twint -u mkbhd --retweets` | Timeouts | Includes users retweets. Limited. Very slow. Must use with `-u`
`--userid` | `twint --userid 29873662` | Timeouts | Matches tweets from this user id. Limited to 3200 results
`--favorites` | `twint -u mkbhd --favorites` | Timeouts | Gets tweets user has liked. American spelling. Must use with `-u`
`--members-list` | ? | Doesn't do anything | Matches tweets by users in list
`--user-full` | `twint -u mkbhd --user-full` | Timeouts | Get all info on user. followers or following
`--store-pandas` | `twint -s lemon --store-pandas out.txt` | Doesn't work in CLI | Store results in pandas dataframe
`--pandas-type` | `twint -s lemon --store-pandas out.txt --pandas-type HDF5` | Doesn't work in CLI | Sets pandas dataframe type, HDF5 (default) or Pickle
