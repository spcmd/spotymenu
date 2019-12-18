# spotymenu

spotymenu is a bash script which uses dmenu, dbus and the Spotify web API to control Spotify. It uses local files to store information so you don't need a premium account or a token to use it.

## Why?
Because Spotify's GUI is slow, and if you have a lots of albums, it's pain in the ass to view & select from them.

dmenu is fast and convenient way to access them.

## What you can do with spotymenu:
* add albums to the local album list (file)
* remove albums from the local album list (file)
* select and play albums from the local album list (file)
* add tracks to a "favorites" list (file)
* search for albums/artists/tracks
* log your searches to a search history (file) and reuse them quickly
* get the tracklist of the playing album and cache it locally
* show the currently playing song
* get some info (metadata) about the currently playing song
* you can customize it to your liking (colors, font, separator line etc.)

## What you CAN'T do:

* spotymenu doesn't use tokens, it won't sync to your Spotify account, and it won't modify it at any level.
* It doesn't use libspotify or any premium-only solution, so you can't use it without the GUI.
* No play/pause, prev/next integration (via dbus). This should be managed with key combos / hotkeys, you might already have taken care of these anyway.

## Screenshots

**Album list**

![](https://raw.githubusercontent.com/spcmd/spcmd.github.io/master/img/spotymenu_album_list.png)

**Tracklist**

![](https://raw.githubusercontent.com/spcmd/spcmd.github.io/master/img/spotymenu_tracklist.png)

**Delete album**

![](https://raw.githubusercontent.com/spcmd/spcmd.github.io/master/img/spotymenu_delete_album.png)

## Dependencies

* dmenu or dmenu2
* [jq](https://stedolan.github.io/jq/)
* Spotify with dbus support

## Setting up

### Installation (Arch Linux)

Clone this repo, change to the cloned directory and run `makepkg -si`.
If you are using dmenu2 instead of dmenu, don't forget to change its name in the depends array in the PKGBUILD:

**from:**
`depends=('spotify' 'dmenu' 'jq')`

**to:**
`depends=('spotify' 'dmenu2' 'jq')`

Then create the necessary directories in your HOME directory:

`mkdir -p ~/.spotymenu/tracklist_cache`

After that you might want to copy the default configuration file (`spotymenurc`) to your .config directory:

`cp -R /etc/spotymenu ~/.config`

The config file has comments, read them and set it up the way you like.

##### Optional Dependency:
- fzf ([Arch Community repo](https://www.archlinux.org/packages/community/x86_64/fzf/) , [github](https://github.com/junegunn/fzf) ): for running spotymenu from TTY/tmux without switching back to X (**Note:** not fully working: you can list/select/play albums, but other options don't seem to work.)

### Installation (manually)

By default spotymenu resides in `~/.spotymenu`.
You can change it in the configuration file.

Put the `spotymenu` script somewhere in you `$PATH`, e.g.: `/usr/bin`.

Put the configuration file (`spotymenurc`) inside `/etc` and/or inside `~/.config/spotymenu`.

Then create the necessary directories in your HOME directory:

`mkdir -p ~/.spotymenu/tracklist_cache`

The config file has comments, read them and set it up the way you like.

##### Optional Dependency:
- fzf ([github](https://github.com/junegunn/fzf) ): for running spotymenu from TTY/tmux without switching back to X (**Note:** not fully working: you can list/select/play albums, but other options don't seem to work.)

### Exporting Spotify album list
If you have a lots of albums, it's better to export it to a local album list. You can export your albums with this script: **[spotify-album-export](https://github.com/spcmd/Scripts/blob/master/spotify-album-export)**. You need a token for this, which you can get it from **[here](https://developer.spotify.com/web-api/console/get-current-user-saved-albums/)**. (Log in, Get a token, check the *read-user-library* option. The token will expire after a certain amount of time).

Then run the `spotify-album-export <token>` which will export your first 50 albums. If you have more, then you can run the script again with the `-o` option, which is the offset, so the command would be: `spotify-album-export -o 50 <token>`. If you still have more, run again with `-o 100` and so on...

By default it will save the album list to this file: `~/album_list` (Put it to spotymenu's directory).

It's a pipe delimited file, which stores basic infromation about the albums:

`artist|album_title|album_id`

One per line!

### Exporting Spotify playlists

Keep in mind, spotymenu doesn't use a token so it can't access to your playlists and it won't autoplay them either after you select one from spotymenu. The playlist feature is rather a "bookmark" or "quick link" to your Spotify playlists.

No export script from me yet, you might need to use something else or export them manually: right click on the playlist in Spotify, copy the `Spotify URI` and paste it to `~/.spotymenu/playlists` (by default). It needs to be a pipe delimited file too:

`playlist_name|spotify_uri`

One per line!

Then you can access this list from spotymenu with the `:p` or `:playlist` command (see the *Usage* section below!).

## Usage

You can run it from dmenu but it's faster to bind the `spotymenu` command to some hotkey.

There is a little help page inside spotymenu, you can get this by using the `:h` or `:help` command.

It will look like this:

```

/                   Search track (with or without artist)

/a   /artist        Search artist

/b   /album         Search album

:?                  Show search history

:p   :playlist      Switch to the exported playlists

:a   :add           Add current album to the album list

:d   :delete        Delete album from the album list

:t   :tracklist     Show tracklist of the current album

:f   :fav           Show favorite tracks' list

:fa  :fadd          Add track to the favorites list

:fd  :fdel          Delete track from the favorites list

:i   :info          Show detailed information about the played track/album

:q   :quit          Quit (close Spotify)

:c   :clean         Cleanup the unused tracklist cache files (IDs that are not in the album list)

spotify:album:<id>  Open album by pasting the album URI from clipboard (Ctrl-Y or Ctrl-y by default in dmenu)

spotify:track:<id>  Open track by pasting the track URI from clipboard (Ctrl-Y or Ctrl-y by default in dmenu)

:h   :help          This help

```

#### Some simple example:

* search for Metallica: `/a Metallica`
* search for the *Master of Puppets* album: `/b master of puppets`
* search for a track (song): `/metallica one`


## License

* MIT Copyright (c) 2016 spcmd

### Donation

Humbly accepting your kind donation

[![paypal donate](https://www.paypalobjects.com/en_US/i/btn/btn_donateCC_LG.gif)](https://www.paypal.com/cgi-bin/webscr?cmd=_s-xclick&hosted_button_id=HCPYH3LWZZR9Y
)
