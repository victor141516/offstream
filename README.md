# offstream

Record your favorite twitch streams automatically and watch them later.

- There is no web UI, just use curl and your favorite video player.
- Streams are recorded as is, without re-transcoding.
- Recordings are never muted.
- Ads are optional.
- Completely free. You can deploy it to Heroku or run on your own server.
- Recordings are stored on IPFS.
- TODO: Feed of all recordings in the RSS format. It works with
  youtube-dl and VLC.
- TODO: No playback performance issues
- TODO: Good option if you have slow or unreliable Internet connection.
- TODO: Recording is available while the recording is in progress. The delay is
  small and configurable.

## Installing

To deploy the app, click the button below and follow the instructions.

[![Deploy](https://www.herokucdn.com/deploy/button.svg)](https://heroku.com/deploy)

Once the app is ready, you will get your credentials to control the app. Please
add them to your `~/.netrc` file.

```
machine your-app-name.herokuapp.com
    login offstream
    password <your-password>
```

Next, add your favorite streamers.

```sh
$ curl https://your-app-name.herokuapp.com/streamers --netrc -d name=garybernhardt
$ curl https://your-app-name.herokuapp.com/streamers --netrc -d name=esl_sc2 -d max_quality=720p60
```

When any of the streamers is live, the app will record the stream.

Finally, to watch the latest recording, open the following URL in mpv, VLC,
QuickTime, or any other video player.

```sh
$ mpv https://your-app-name.herokuapp.com/latest/esl_sc2
```

An RSS feed of all recordings is available at `https://your-app-name.herokuapp.com/rss`.

## Summary

- `POST /streamers`
  Track a new streamer.
- `DELETE /streamers/<name>`
  Delete the streamer. WARNING: Deletes associated streams too.
- `GET /lastest/<name>`
  Watch the latest stream.
- `GET /rss`
  Feed of recordings.

## Configuration

The following environment variables are supported.

- `OFFSTREAM_AWAKE_END_HOUR`
- `OFFSTREAM_AWAKE_START_HOUR`
- `OFFSTREAM_FLUSH_THRESHOLD`
- `OFFSTREAM_CHECK_INTERVAL_SECONDS`
- `OFFSTREAM_IPFS_API_ADDR`
- `OFFSTREAM_IPFS_GATEWAY_URI_TEMPLATE`
- `OFFSTREAM_MAX_CONCURRENT_RECORDERS`
- `TZ` Your preferred timezone, e.g. `America/New_York`. Please see https://en.wikipedia.org/wiki/List_of_tz_database_time_zones#List

## FAQ

- Q. keepalive request failed for 'https://bafybeie3v6lomkfti2b4zsa4yj35nypojllvjrzpbzyxhn5tkfoqaswmbm.ipfs.infura-ipfs.io/18846.ts'

## Development

1. Create a virtual environment and activate it.
   ```sh
   python -m venv venv
   source venv/bin/activate
   ```
1. Install dependencies.
   ```sh
   pip install -r requirements-test.txt -c constraints.txt
   ```
1. Run tests.
   ```sh
   pytest
   ```
1. Setup a local SQLite database.
   ```sh
   flask offstream setup
   ```
1. Start the app.
   ```sh
   flask run
   ```
1. Start the stream recorder.
   ```sh
   flask offstream record
   ```

## Flask commands

This app has a few custom flask commands.

```sh
$ flask offstream --help
```

| command                  | description                    |
| ------------------------ | ------------------------------ |
| `flask run`              | Start the app.                 |
| `flask offstream record` | Start stream recorder.         |
| `flask offstream ping`   | Ping itself to prevent idling. |
| `flask offstream setup`  | Setup the database.            |
| `flask offstream create` | Create db tables.              |
