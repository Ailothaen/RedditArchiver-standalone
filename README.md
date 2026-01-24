# RedditArchiver-standalone

<p align="center"><img src="https://github.com/ailothaen/RedditArchiver/blob/main/github/logo.png?raw=true" alt="RedditArchiver logo" width="500"></p>

RedditArchiver-standalone is the standalone version of RedditArchiver.

"Standalone" means that you do not need a web server: the function was reduced to a simple Python script.

For more information on RedditArchiver itself, see [the main repository](https://github.com/ailothaen/RedditArchiver).

> [!WARNING]
> In November 2025, Reddit decided to require all new API token creations to be approved by Reddit admins. Since this change was primarily made to deter scrapers and LLMs from using Reddit data, it is very unlikely you will get a token approved if you tell "I want to use a software to archive Reddit posts" as a justification.
>
> In practice, this means that Reddit API is now gone and that this project is no longer usable if you did not have an API token registered before. This is another move towards the enshittification of this platform (that already began in 2023 when hard limits were imposed on the APIs). If you are lucky enough to have an API token already registered, **DO NOT DELETE IT**. 
>
> It may be still possible to save posts and comments using the API unauthenticated endpoints, but this project is not doing that. I suggest looking into other projects if you are in this case.


## Installing dependencies and setting up tokens

(Replace `python3` by `py -3` if you are on Windows)

Install dependencies:

```bash
python3 -m pip install -r requirements.txt
```

Copy `config.yml.example` to `config.yml` and put all the information needed to connect (client ID, client secret and refresh token). If you have no clue about what these options are, follow these steps:
1. [Go here](https://www.reddit.com/prefs/apps) and create an app. Use the "script" type, and put `http://localhost:8080` as redirect URI (the other options do not matter).

> [!WARNING]
> If you have a message telling "In order to create an application or use our API you can read our full policies here...", you are likely out of luck (read the warning message at the beginning of this README)

2. Finish creating the app. Take note of your client ID and client secret (the client ID is below the 'personal use script' label, the secret is beside the 'secret' label).
3. Run the script `authentication.py` to get your refresh token.
4. Edit the config file to put the client ID, client secret and refresh token in it. (You can avoid putting the refresh token in the config file; see [Shell Function](#shell-function) for details.)

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

## Shell Function
The file `reddit-archive.sh` defines a shell function that makes it simpler to use RedditArchiver for a single post. It also adds support for customization with environment variables, and better security for the refresh token.

### Installation
You can add this function to your .bashrc or .zshrc with:

```bash
source ~/RedditArchiver-standalone/reddit-archive.sh
```

(Use the directory you cloned the repository if it's not in your home directory.)

### Usage
```bash
reddit_archive https://www.reddit.com/r/Superbowl/comments/14hczkk/elf_owl_enjoying_our_pond/
reddit_archive 14iard6
```

Unlike the RedditArchiver.py script, you can only specify *one* submission ID or URL.

### Customization
There are several environment variables to allow customization of paths/files:

* `REDDIT_ARCHIVER_SCRIPT_DIR`: Directory where RedditArchiver.py lives. If not set, this defaults to the location of the `reddit-archive.sh` script (zsh/bash).
* `REDDIT_ARCHIVER_SCRIPT_NAME`: Name of the script itself, normally `RedditArchiver.py`.
* `REDDIT_ARCHIVER_VENV_PATH`: Path to the Python virtual environment, if one is in use
* `REDDIT_ARCHIVER_OUTPUT_DIR`: Directory where the post will be saved. This defaults to `$HOME/RedditPosts`.
* `REDDIT_ARCHIVER_CONFIG_FILE`: Path and name of config file. This defaults to the location of the script plus `config.yml`. 
* `REDDIT_ARCHIVER_REFRESH_TOKEN_SECRET_FILE`: Path and name of file containing encrypted refresh token. By default this is not set. If set, Actually Good Encryption (age) is used to decrypt the refresh token using a passkey. See https://github.com/FiloSottile/age for more. If you use something else such as gpg, make appropriate changes to the shell script. See [Security](### Security) for more.

### Security

The combination of a custom filepath to the config file and filepath to an encrypted refresh token provides for more security than the out-of-the-box RedditArchiver.py provides. After running `authentication.py` to get the client ID, secret, and refresh token:

1. Copy the `config.yml.example` file to e.g. `~/.secrets/RedditArchiver.config.yml`. Set `REDDIT_ARCHIVER_CONFIG_FILE` to this filepath. Do *not* edit the file in its original location.
2. Edit this file and set the client ID and secret values from your Reddit app. (If you aren't sure, the client ID is the string under the 'personal use script' label, and the secret is next to the 'secret' label.)
3. Assuming you're using age as your encryption tool, you can use the `reddit_archiver_encrypt_refresh_token` function to encrypt your refresh token (path is an example):
```bash
reddit_archiver_encrypt_refresh_token ~/.secrets/RedditArchiver.refreshtoken.age
```
Then in your `.bashrc` or `.zshrc`:
```bash
export REDDIT_ARCHIVER_REFRESH_TOKEN_SECRET_FILE=~/.secrets/RedditArchiver.refreshtoken.age
```

This keeps your refresh token out of the filesystem. Additionally, the reddit_archive function passes the decrypted refresh token to the Python script as an environment variable, to keep it off the command line.

## Licensing

This software is licensed [with MIT license](https://github.com/ailothaen/RedditArchiver/blob/main/LICENSE).
