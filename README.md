# RedditArchiver-standalone

<p align="center"><img src="https://github.com/ailothaen/RedditArchiver/blob/main/github/logo.png?raw=true" alt="RedditArchiver logo" width="500"></p>

RedditArchiver-standalone is the standalone version of RedditArchiver.

"Standalone" means that you do not need a web server: the function was reduced to a simple Python script.

For more information on RedditArchiver itself, see [the main repository](https://github.com/ailothaen/RedditArchiver).


## Installing dependencies and setting up tokens

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
4. Run the script `authentication.py` to get your refresh token
5. Edit the config file to put the client ID, client secret and refresh token in it.


## Running the script

You can select the submissions you want to download with several methods:

- `-i`: specify a submission ID or an URL to download it. `-i` can be written several times to download several submissions, like that:

```bash
python3 RedditArchiver.py \
    -i https://www.reddit.com/r/Superbowl/comments/14hczkk/elf_owl_enjoying_our_pond/ \
    -i https://www.reddit.com/r/Superbowl/comments/14gozc4/adult_and_hungry_juvenile_great_horned_owl_norcal/ \
    -i 14iard6
```

- `-s`: Download all the submissions you saved. If you want to include as well the submissions which you saved a comment from, pass `-S` instead.

- `-u`: Download all the submissions you upvoted.

- `-a`: Download all the submissions you posted. If you want to include as well the submissions which you posted a comment in, pass `-A` instead.  
  You can also specify a name to download the submissions from another redditor. Here, you will download the submissions posted by you and by u/iamthatis:

```bash
python3 RedditArchiver.py -a -a iamthatis
```

You can combine these options to download a lot of things at once:

```bash
python3 RedditArchiver.py \
    -i https://www.reddit.com/r/Superbowl/comments/14hczkk/elf_owl_enjoying_our_pond/ \
    -i https://www.reddit.com/r/Superbowl/comments/14gozc4/adult_and_hungry_juvenile_great_horned_owl_norcal/ \
    -i 14iard6 \
    -s -u -A -l 10
```

RedditArchiver has more options to control its behavior (limit of submissions retrieved, config file...). To see more, display help with the `-h` option.


## Licensing

This software is licensed [with MIT license](https://github.com/ailothaen/RedditArchiver/blob/main/LICENSE).
