# Experimental libspotify PHP lib

## Important notes

  - Read the [libspotify license][1], there are some important things to note; for example you CAN NOT "attempt to embed or integrate the API into any website or otherwise allow access to the Service via the web rather than via the Application.".
  - The behaviour and API might change; this library is not production ready.

[1]: http://developer.spotify.com/en/libspotify/terms-of-use/ "libspotify terms of use"

## Installation

  - Install the libspotify dev files and binary that fits your system, get them from the [Spotify developer website][2].
  - Run these commands: phpize; ./configure --enable-spotify; make && make install

[2]: http://developer.spotify.com/

## Usage

	// These two lines are only required if you want to use non-default paths.
	ini_set("spotify.cache_location", "cache_location/");
	ini_set("spotify.settings_location", "settings_location/");

    $spotify = new Spotify("/path/to/key.file", "username", "password");
    
	$coolTrack = $spotify->getTrackByURI('spotify:track:6JEK0CvvjDjjMUBFoXShNZ');
     
	// List all playlists
	$playlists = $spotify->getPlaylists(); // returns array of SpotifyPlaylist
	foreach ($playlists as $playlist) {
		printf("%s (%d tracks, by %s)\n", $playlist, $playlist->getNumTracks(), $playlist->getOwner());

		foreach ($playlist->getTracks() as $track) {
			$duration = $track->getDuration();
			printf("  -> %s - %s [%02d:%02d]\n", $track->getArtist(), $track,
					$duration/60, $duration%60);
		}

		// and add a important piece of music
		$playlist->addTrack($coolTrack, 0 /*position*/);
	}

## Troubleshooting

  - Make sure that the process executing the PHP script has access to the cache- and settings-folders.

## Credits

Vilhelm K. Vardøy <vilhelmkv@gmail.com>

### Thanks to

Jeroen Flamman

## TODO

  - Instead of returning arrays of objects, change to iterators and/or arrayaccess.
  - Remove private functions and replace them with pure C functions.
  - Add possibility to receive tracks in inbox. Due to obvious reasons (spam) we SHOULD NOT implement a way to send tracks.
  - Add possibility to search for tracks.
