# BELFAST

#### Hi Master. I'm Belfast your Assistant waiting to Proceed.


#### I am Ready to Deploy!

[![Deploy](https://www.herokucdn.com/deploy/button.svg)](https://heroku.com/deploy)

There we go! Now, Before start to use Things need to FULFILLED... Listed Below!

## TODO after deploy
#### 1. To get torrent download working. This is important to keep your bot alive otherwise the server will stop after `30 minutes of inactivity`.
### TO SET WEBSITE:
`SITE` | `https://<project name>.herokuapp.com` (Where Project Name is your Heroku App Name)
>

#### 2. If you want to Run a Bot on TG you should have some variables from your telegram.
### TO START BELFAST AS TG BOT:
`TELEGRAM_TOKEN` | `Bot Token` FROM [BotFather](https://t.me/Botfather)
>

#### 3. Now, Let's move on to G-Drive Things. Here Follow them carefully.
### TO SET API:
Go to [Quickstart NodeJs](https://developers.google.com/drive/api/v3/quickstart/nodejs) and Enable Drive. Create an O`AUTH` for `Desktop` and copy the `Client ID` and `Client Secret`.
### TO GET API VALUES:
Visit your `Heroku app website` and add `drivehelp` at the end of the address. This will open a page where you can paste both your `Client ID` and `Client Secret`. Follow the screen prompts to get your `AUTH CODE` and `TOKEN`. Use the above values to create your `Heroku Config variables` with Key: Value pair as explained below.
### TO SET VALUES ON HEROKU:
`CLIENT_ID` | Value is String of numbers copied as `Client ID` above.
`CLIENT_SECRET` | Value is String of numbers copied as `Client Secret` from above.
`AUTH_CODE` | Value is `Auth Code` gotten from the above step.
`TOKEN` | Value is `Token` gotten from the above step.
`GDRIVE_PARENT_FOLDER` | Value is `Download destination ID` From the Drive. 
>The folder id will be the last part of the url such as in url "https://drive.google.com/drive/folders/1rpk7tGWs_lv_kZ_W4EPaKj8brfFVLOH-" the folder id is "1rpk7tGWs_lv_kZ_W4EPaKj8brfFVLOH-".
>
>If you want team drive support open your teamdrive and copy the folder id from url eg. https://drive.google.com/drive/u/0/folders/0ABZHZpfYfdVCUk9PVA this is link of a team drive copy the last part "0ABZHZpfYfdVCUk9PVA" this will be your GDRIVE_PARENT_FOLDER. If you want them in a folder in teamdrive open the folder and use that folder's id instead.

#### 4. You're good to go. The gdrive status will be shown in `gdrive.txt` file when you click Open on the website downloads page. Bot wil automatically send you drive link when its uploaded.

#### 5. You can run Belfast as a TG Bot.
### TO DISABLE WEBSITE:
Set value of `DISABLE_WEB` env var to `true`.

### 6. BotFather BOT CMD SETTINGS:
After having completed setting up the bot, you may wish to set some custom commands for ease of use. Go to `Botfather` IN TELEGRAM, select to `/setcommands` and use the following commands for your bot:

```
search - Site query
details - Site query
download - Magnet link
status - Of download task
remove - A download task
```

## MISC SET-UPS
### To Get `SEARCH` working:
The library used for web scrapping the torrent sites requires a custom buildpack on heroku. By default the search will happen on your deployment and you will need to configure the buildpack as described below. But if you don't want to do that you can specify and env `SEARCH_SITE` and set value to `https://torrent-aio-bot.herokuapp.com/` . The frwd slash at end is necessary. This will make all the searches go thru my deployment and you don't need to configure buildpack.
Go to the build packs section in settings and click add buildpack and enter "https://github.com/jontewks/puppeteer-heroku-buildpack.git" as buildpack url then click save changes. And then do a dummy git commit so that heroku will buid it using the buildpack this time. Then set the `SEARCH_SITE` env to same value as `SITE`.

### TO CHECK G-Drive Upload:
> Use this torrent for testing or when downloading to setup drive it is well seeded and downloads in ~10s
>
> magnet:?xt=urn:btih:dd8255ecdc7ca55fb0bbf81323d87062db1f6d1c&dn=Big+Buck+Bunny

### To SET ENVironment Variable & Values:
To set a enviorment variable go to heroku dashboard open the app then go to Settings > Config vars > Reveal Config vars.

## Changing the sites used for searching
To change the pirate bay site, visit the site you would like to use search something there, copy the url eg. `https://thepiratebay.org/search/whatisearched` and replace the search with {term} so the url looks like `https://thepiratebay.org/search/{term}` ans set this to env var `PIRATEBAY_SITE`

Same, if you want to change the limetorrents site visit the site you want to use and search for something, then replace the thing you searched for with {term} so final url looks like `https://limetorrents.at/search?search={term}` and set this value to env var `LIMETORRENT_SITE`

For `1337x` env var name will be `O337X_SITE`

## API Endpoints
prefix: https://\<project name>.herokuapp.com/api/v1

### For Downloading:

| Endpoint          |    Params    |                                                                Return |
| :---------------- | :----------: | --------------------------------------------------------------------: |
| /torrent/download | link: string | { error: bool, link: string, infohash: string errorMessage?: string } |
| /torrent/list     |     none     |                    {error: bool, torrents: [ torrent, torrent, ... ]} |
| /torrent/remove   | link: string |                                { error: bool, errorMessage?: string } |
| /torrent/status   | link: string |                 {error: bool, status: torrent, errorMessage?: string} |

link is magnet uri of the torrent

```
torrent:  {
  magnetURI: string,
  speed: string,
  downloaded: string,
  total: string,
  progress: number,
  timeRemaining: number,
  redableTimeRemaining: string,
  downloadLink: string,
  status: string,
  done: bool
}
```

### For Searching:

| Endpoint        |            Params            |                                                          Return |
| :-------------- | :--------------------------: | --------------------------------------------------------------: |
| /search/{site}  | query: string, site?: string | {error: bool, results: [ result, ... ], totalResults: number, } |
| /details/{site} |        query: string         |                                         {error: bool, torrent } |

query is what you want to search for or the link of the torrent page
site is the link to homepage of proxy to use must have a trailing '/'

```
result: {
  name: string,
  link: string,
  seeds: number,
  details: string
}

torrent: {
  title: string,
  info: string,
  downloadLink: string,
  details: [ { infoTitle: string, infoText: string } ]
}
```

#### Sites Available `Piratebay`, `1337x`, `Limetorrent`.

>A Special Heartful Thanks to [PatheticGeek](https://github.com/patheticGeek) for His wonderful Contribution on This.
