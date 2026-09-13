# Navidrome Kodi Plugin

**plugin.kodi.navidrome** is a Kodi addon that allows you to stream your music collection directly from a [Navidrome](https://www.navidrome.org/) server. It provides a seamless integration with Kodi's music interface, supporting browsing, streaming, and library management features.

## Features

*   **Music Streaming**: Stream your entire music library from Navidrome to Kodi.
*   **Browsing**:
    *   **Albums**: Browse by All, Random, Favourites, Top Rated, Recently Added, Recently Played, and Most Played.
    *   **Artists**: Browse all artists with cover art.
    *   **Songs**: Browse individual tracks.
    *   **Genres**: Browse albums and songs by genre.
    *   **Playlists**: Access and play your existing Navidrome playlists.
    *   **Radios**: Listen to internet radio stations configured in Navidrome.
*   **Search**: Search for Artists, Albums, and Songs.
*   **Library Management**:
    *   **Star/Unstar**: Mark albums and songs as favourites directly from Kodi.
    *   **Ratings**: Set a 0-5 star rating on any song, album or artist from the
        context menu; existing ratings are shown in Kodi's own rating column.
    *   **Playlists**: Create new playlists or add tracks to existing ones.
*   **Offline Playback (VFS)**: Download tracks, albums or whole playlists into a
    local cache built on Kodi's Virtual File System (`xbmcvfs`), browse them under
    **Offline**, and play them with the server unreachable. Playback transparently
    prefers a cached copy over the network stream.
*   **Scrobbling**: Fully functional scrobbling and "Now Playing" status updates to your Navidrome server.
*   **Transcoding**: Supports server-side transcoding with configurable bitrates and formats (MP3, etc.) for bandwidth management.
*   **HTTPS**: Works with public certificates, a private CA, or a self-signed
    certificate.

## Installation

1.  Download the latest release zip file from the [Releases](https://github.com/colinfredynand/plugin.kodi.navidrome/releases) page (or clone the repo and zip it).
2.  Open Kodi and go to **Settings** > **Add-ons**.
3.  Select **Install from zip file**.
4.  Navigate to the downloaded zip file and select it.
5.  Wait for the "Add-on enabled" notification.

## Configuration

After installation, you must configure the addon to connect to your Navidrome server:

1.  Go to **Add-ons** > **Music add-ons**.
2.  Right-click (or long-press) on **Navidrome** and select **Settings**.
3.  Enter your connection details:
    *   **Server URL**: The full URL to your Navidrome instance (e.g., `https://music.mydomain.com` or `http://192.168.1.10:4533`).
    *   **Username**: Your Navidrome username.
    *   **Password**: Your Navidrome password.
4.  (Optional) Configure **TLS**, under the same **Server** tab:
    *   **Verify TLS certificate** (default: on). Leave this on whenever you can.
    *   **CA certificate**: point this at the CA (or the certificate itself) that
        signed your server's certificate. This is the right way to use a private
        CA or a self-signed certificate — verification stays on.
    *   Turning **Verify TLS certificate** off disables certificate checking for
        both the addon's own API calls and Kodi's streaming/artwork requests. It
        makes a self-signed server work with no further setup, at the cost of no
        protection against an intercepted connection — prefer the CA option.
5.  (Optional) Configure **Transcoding**:
    *   **Enable Transcoding**: Toggle on/off.
    *   **Max Bitrate**: Select your preferred quality (e.g., 320 kbps, 128 kbps).
    *   **Format**: Choose the transcoding format (default: mp3).
6.  (Optional) Configure **Offline**:
    *   **Enable Offline Cache** (default: on) adds the download context-menu
        entries and the **Offline** item on the main menu.
    *   **Offline Cache Size (MB)**: once the cache exceeds this, the least
        recently downloaded tracks are dropped.
    *   **Clear Offline Cache**: delete every downloaded track.
7.  (Optional) Configure **Display**:
    *   **Items Per Page**: how many rows each page of a long list holds. Pages
        larger than 500 are assembled from several requests, because that is the
        most the server will return at once.

## Known Issues & Roadmap

*   **VFS Implementation**: *Done.* Playback now resolves through the plugin
    (`setResolvedUrl`) rather than handing Kodi a bare HTTP URL, and everything
    the addon stores — the offline media cache, its metadata sidecars, the cached
    login and the now-playing marker — goes through `xbmcvfs`, so it works on
    platforms where the addon data directory is not a plain local path.
*   **Show Track Numbers in Title**: the setting exists but is not wired up yet.

## Development

This addon is written in Python and uses the standard Kodi Addon API.

### Structure
*   `default.py`: Main addon entry point and routing logic.
*   `service.py`: Background service (scrobbling and "Now Playing" updates).
*   `lib/navidrome_api.py`: Wrapper for the Navidrome native, Subsonic and
    OpenSubsonic APIs, including TLS setup and page assembly.
*   `lib/vfs.py`: Kodi Virtual File System layer — paths, JSON/binary IO and the
    offline media cache.
*   `resources/`: Settings, language files, and images.

## License

This project is licensed under the [GPL-3.0 License](LICENSE).

## Acknowledgments

*   Thanks to the [Navidrome](https://www.navidrome.org/) team for the excellent music server.
*   Developed by [colinfredynand](https://github.com/colinfredynand).
