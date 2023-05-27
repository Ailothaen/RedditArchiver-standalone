# RedditArchiver-standalone

<p align="center"><img src="https://github.com/ailothaen/RedditArchiver/blob/main/github/logo.png?raw=true" alt="RedditArchiver logo" width="500"></p>

RedditArchiver-standalone is the standalone version of RedditArchiver.

"Standalone" means that you do not need a web server: the function was reduced to a simple Python script.

For more information on RedditArchiver itself, see [the main repository](https://github.com/ailothaen/RedditArchiver).


## Running the script

(Replace `python3` by `py -3` if you are on Windows)

Install dependencies:

```bash
python3 -m pip install -r requirements.txt
```

Edit the file `config.yml` to put all the informations needed to connect (client ID, client secret and refresh token).

If you have no clue about what these options are, follow these steps:
1. [Go here](https://www.reddit.com/prefs/apps) and create an app
2. Use the "script" type, and put `http://localhost:8080` as redirect URI (the other options do not matter)
3. Take note of your client ID and client secret
4. To get your refresh token, you can use [this script](https://praw.readthedocs.io/en/stable/tutorials/refresh_token.html#obtaining-refresh-tokens) (you only need the "read" scope)
5. Edit the config file to put these three elements in it.

For information about the usage of the script, run `python3 RedditArchiver -h`.


## Licensing

This software is licensed [with MIT license](https://github.com/ailothaen/RedditArchiver/blob/main/LICENSE).
