# spotifyRadioPlaylist

Putting the latest tracks from a radio station with online trackservice (like [this one](http://fm4.orf.at/trackservicepopup/main)) onto a spotify playlist.

More on the background of this project in my [blog post](http://blog.chrisrohrer.de/radio-to-spotify-playlist/).

## Demo

Currently running every 20min via cronjob on [this playlist](https://play.spotify.com/user/radiolistenerbot/playlist/6prQq7S9saObANcQOddSTh)

## Usage

Before running it on the server you need to authenticate via oAuth (as the user who owns the specified playlist) locally.

1. Clone this repo into a local directory
2. Copy `config.example.json` to `config.json` and insert your own credentials & radio station.
You can obtain a clientId & clientSecret by [registering a new application](https://developer.spotify.com/my-applications/#!/applications).  
There are some pre-defined and tested radio stations, they can be found in `station-examples.md`. You can define your own schemes, all you need is the URL of the playlist page of the station and three jQuery selectors: one for the playlist entry element, one for the title and one for the artist. Some radio stations (like FM4) have special markup that requires linear instead of nested search; this behaviour can be set with the `searchLinear` flag.
3. Run `npm install`
4. Run `node main.js`
5. Open the displayed URL in your browser & grant permission for your app to change your playlists. This will open a page on localhost which you can close. Now you find two new files: `accessToken` and `refreshToken`. They contain the secret information to authenticate the user with spotify, so handle with care!

This should have added the first tracks to your playlist already. Every time you run `node main.js` again, the script will check if there are new tracks that have not been added to the playlist yet.

You may want to run this on a server via cronjob every X minutes or so (depending on how many results your trackservice delivers in one page).

1. Clone this repo on your server into a _non-public_ directory
2. Copy your local `accessToken` and `refreshToken` onto the server
3. Copy your local `config.json` onto the server and change the last entry to `"localEnvironment": false`
4. Run `npm install`
5. Configure your cronjob to run `node main.js` every X minutes (don't forget to change to the correct directory first!)
