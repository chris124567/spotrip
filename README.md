Due to potential legal problems from circumventing Spotify's content protection, I have closed this repository.

# spotrip
This is an open source Spotify song ripping application written in Golang (using a modified version of [Librespot](http://github.com/librespot-org/librespot-golang)).  It works with both Spotify free and premium accounts.  It can download individual tracks, albums, or entire artist discographies.   It downloads the audio files served by Spotify's servers and does not work by recording audio outputs.

## Note
Use of this tool may result in your account getting banned by Spotify.  **Please** do not use it on accounts you have spent any money on.  I am not liable for how you use this tool in any way.  From my experience using this tool, I have observed that Spotify has rate limiting measures and must have some idea of what is going on. 

## Dependencies
At least Go 1.11 to allow for module support.

For adding metadata to the downloaded files, the ```taglib``` developer libraries are required.  Learn how to install them [here](https://github.com/wtolson/go-taglib#dependencies).

## Build
Get the package:

    $ go get github.com/chris124567/spotrip

There will be a warning about some undefined functions.  It can safely be ignored.  This is because we have a local version of librespot, which adds additional functionality.  This project uses the "replace" directive in go.mod to avoid having to replace all the import paths, but [the Go developers](https://github.com/golang/go/issues/30354) do not want to make ```go get``` support the replace directive.  Just make sure to run the next step to be able to use the program.

    $ cd $GOPATH/src/github.com/chris124567/spotrip
    $ scripts/build.sh

You should now see an executable file called ```spotrip```.  This is the program.

## Usage
A typical session would look something like (replacing credentials obviously):

    $ spotrip --username example@example.com --password password --search "bing crosby"

    Artist: Bing Crosby (6ZjFtWeHP9XN7FeKSUe80S)
    Artist: Bing Crosby & Ella Fitzgerald (12vDf3ySm1HSBb9xFmmFdG)
    Artist: Bing Crosby and Fred Astaire (2adeNkZOTG985M6JcXaJgV)
    ...
    Album: Bing Crosby - Bing Crosby - Christmas Classics (3My4DXwRjAS5HXontsJx1A)
    Album: Bing Crosby - Bing Crosby Sings Christmas Songs (3tKDUoAPs0fEgR5GSqXXNl)
    Album: Bing Crosby - Bing Crosby And Some Jazz Friends (1rQ6SUk3ktdbL0cAdKYqIM)
    ...
    Track: Bing Crosby - Silent Night (4eJPzDcuWmXMedrwAbUeCt)
    Track: Bing Crosby - Little Angel (6CbXPzY82Jcoq2sBUzMgbq)
    Track: Bing Crosby - Paradise Isle (7bp5088fuDSi5D0VhDa6PE)

The IDs are the 22 character identifiers listed in parentheses.

To download all tracks by "Bing Crosby," you would run:

    $ spotrip --username example@example.com --password password --artists 6ZjFtWeHP9XN7FeKSUe80S

To download the "Christmas Classics" album, you would run:

    $ spotrip --username example@example.com --password password --albums 3My4DXwRjAS5HXontsJx1A

To download "Silent Night," you would run

    $ spotrip --username example@example.com --password password --tracks 4eJPzDcuWmXMedrwAbUeCt

```--artists```, ```--albums```, and ```--tracks``` all accept multiple IDs as arguments.  Just separate them by comma, like so:

    $ spotrip --username example@example.com --password password --tracks "4eJPzDcuWmXMedrwAbUeCt,6CbXPzY82Jcoq2sBUzMgbq"

Also, any of the arguments can be used simultaneously.  ``--albums`` can be used when ``--tracks`` or ```--artists``` is used without any problems (this is also preferable to running the commands separately to prevent repeated Spotify authentications).

## Help

      -albums string
            List of album IDs to download, separated by commas.
      -artist_info string
            Specify an artist ID in this field to get JSON formatted information about that artist.
      -artists string
            List of artist IDs to download (downloads all of their albums, singles, and compilations), separated by commas.
      -password string
            Password of the spotify account. Required.
      -search string
            Search query.
      -tracks string
            List of track IDs to download, separated by commas.
      -username string
            Username of the spotify account. Required.

## Additional Note on Search
To prevent yourself from generating a "suspicious" amount of logins from this tool, you may want to use the search interface available at [https://open.spotify.com/search](https://open.spotify.com/search).  Simply right click the album, track, or artist you wish to download and press "Copy Song Link." 
The ID follows the last slash of the URL, for instance, in:

    https://open.spotify.com/track/4qOPuARt2HNHAOlgXBezoT

```4qOPuARt2HNHAOlgXBezoT``` would be the track ID.

## Todo
This application currently has several limitations that will be addressed in future versions, notably:

    * Only 160 KBPS OGG downloads are supported (even though Spotify offers 320 KBPS to Premium members and also gives 96 KBPS to free members as a low bandwith option).  This will be a configurable option in the future with priority system for qualities (in case one is not available).
    * There is no way to customize output file names
    * There is no playlist downloading support
    * There is no podcast/show support

## Credits
Thanks to [Librespot](https://github.com/librespot-org) for reverse engineering the Spotify protocol and releasing their [tools](https://github.com/librespot-org/spotify-analyze) for free. They are the ones that make third party Spotify clients possible.