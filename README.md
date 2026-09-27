# TV Channel

A simple browser-based TV channel player designed for normal web browsers and OBS Browser Sources.

The channel supports:

* Full-screen stretched video
* Randomized video playback
* No immediate video repeats
* Automatic fallback when a video fails
* Muted autoplay
* Click-to-unmute
* Advertising ticker
* Weather ticker
* Eastern Time clock
* Channel icon
* Custom themes
* External video URLs
* External configuration
* OBS Browser Source support

## Files

The project uses three required files:

```text
/
├── index.html
├── config.json
├── videos.json
└── icon.png
```

### `index.html`

The main TV channel application.

### `videos.json`

Contains the video playlist.

Example:

```json
[
    "https://example.com/video1.mp4",
    "https://example.com/video2.mp4",
    "https://example.com/video3.mp4"
]
```

Videos are kept separate from the channel configuration.

### `config.json`

Contains the channel configuration, advertisements, and themes.

Example:

```json
{
    "channelName": "My TV Channel",
    "icon": "icon.png",
    "theme": "default",
    "tickerLabel": "NOW",
    "advertisementLabel": "AD",
    "weatherLabel": "WEATHER",
    "tickerDuration": 7000,

    "advertisements": [
        "Hello",
        "World",
        "Thanks for watching!"
    ],

    "themes": {
        "default": {
            "tickerBackground": "#111111",
            "tickerText": "#ffffff",
            "clockBackground": "#111111",
            "clockText": "#ffffff",
            "iconBackground": "#ffffff",
            "iconColor": "#111111",
            "weatherBackground": "#222222"
        }
    }
}
```

### `icon.png`

The square image displayed beside the clock.

You can use PNG, JPG, WebP, or another browser-supported image format.

---

# Basic Setup

1. Put `index.html`, `config.json`, `videos.json`, and `icon.png` in the same directory.
2. Replace the URLs in `videos.json` with your video URLs.
3. Change the channel settings in `config.json`.
4. Upload the files to your web server.
5. Open the URL to `index.html`.

For example:

```text
https://example.com/tv/
```

The browser will automatically request:

```text
https://example.com/tv/config.json
https://example.com/tv/videos.json
https://example.com/tv/icon.png
```

Make sure all of those files are accessible.

---

# Important Server Requirements

The server needs to serve:

* HTML
* JSON
* Images
* Video files if the videos are hosted on the same server

JSON should be served as:

```text
application/json
```

MP4 videos should normally be served as:

```text
video/mp4
```

If videos are hosted on another server, that server needs to permit browser access when required.

For cross-origin video hosting, configure the video server to allow the channel's origin.

For a public channel, a typical header is:

```text
Access-Control-Allow-Origin: *
```

Do not add this header to the video URLs inside `videos.json`. It must be configured on the server actually hosting the video.

---

# Static Web Servers

This project works particularly well on static hosting because it does not require PHP, Node.js, Python, or a database.

The minimum deployment is:

```text
index.html
config.json
videos.json
icon.png
```

Upload those files and visit `index.html`.

---

# Cloudflare Pages

Cloudflare Pages can host the project as a static website.

Create a project containing:

```text
index.html
config.json
videos.json
icon.png
```

Deploy the project.

Your resulting URL may look like:

```text
https://my-channel.pages.dev/
```

The files will then be available at:

```text
https://my-channel.pages.dev/index.html
https://my-channel.pages.dev/config.json
https://my-channel.pages.dev/videos.json
```

You can normally omit `index.html` from the main URL.

## Updating the channel

To change advertisements:

```text
config.json
```

To change videos:

```text
videos.json
```

To change the icon:

```text
icon.png
```

Redeploy after changing the files.

---

# GitHub Pages

Create a repository containing:

```text
index.html
config.json
videos.json
icon.png
```

Then enable GitHub Pages for the repository.

The channel will be available at a URL similar to:

```text
https://username.github.io/repository/
```

GitHub Pages is suitable because the project is entirely client-side.

---

# Netlify

Create a Netlify site and upload the project directory.

The directory should contain:

```text
index.html
config.json
videos.json
icon.png
```

Netlify will automatically use `index.html` as the main page.

Your channel might be available at:

```text
https://example.netlify.app/
```

---

# Vercel

Create a project containing:

```text
index.html
config.json
videos.json
icon.png
```

No special framework is required.

Deploy it as a static project.

The resulting URL can be used directly as an OBS Browser Source.

---

# Apache

Copy the files into the web server's document root.

For example:

```text
/var/www/html/tv/
```

The directory becomes:

```text
/var/www/html/tv/
├── index.html
├── config.json
├── videos.json
└── icon.png
```

Then visit:

```text
https://example.com/tv/
```

## MIME Types

Apache should serve JSON as:

```text
application/json
```

and MP4 files as:

```text
video/mp4
```

If necessary, add the following to `.htaccess`:

```apache
AddType application/json .json
AddType video/mp4 .mp4
```

---

# Nginx

Put the channel files in a web directory such as:

```text
/var/www/html/tv/
```

The structure should be:

```text
/var/www/html/tv/
├── index.html
├── config.json
├── videos.json
└── icon.png
```

A basic Nginx server configuration can look like:

```nginx
server {
    listen 80;

    server_name example.com;

    root /var/www/html/tv;

    index index.html;

    location / {
        try_files $uri $uri/ =404;
    }
}
```

Nginx normally already knows the appropriate MIME types when its standard MIME configuration is enabled.

---

# Python Local Server

Do not open `index.html` directly using:

```text
file:///...
```

Some browsers restrict `fetch()` requests for local files.

Instead, run a small local HTTP server.

With Python:

```bash
python3 -m http.server 8000
```

Then open:

```text
http://localhost:8000/
```

The directory should contain:

```text
index.html
config.json
videos.json
icon.png
```

---

# Node.js Local Server

If Node.js is installed, you can use a simple static server.

For example:

```bash
npx serve .
```

Then open the URL displayed by the server.

---

# Docker

A simple Nginx Docker deployment can use:

```dockerfile
FROM nginx:alpine

COPY . /usr/share/nginx/html
```

Build it:

```bash
docker build -t tv-channel .
```

Run it:

```bash
docker run -p 8080:80 tv-channel
```

Then open:

```text
http://localhost:8080/
```

---

# Adding Videos

Edit `videos.json`.

Example:

```json
[
    "https://cdn.example.com/show1.mp4",
    "https://cdn.example.com/show2.mp4",
    "https://cdn.example.com/show3.mp4",
    "https://cdn.example.com/show4.mp4"
]
```

The player will randomly select videos.

It does not immediately play the same URL twice.

When a video finishes, another video is selected automatically.

If a video produces a loading error, the player automatically attempts another video.

---

# Hosting Videos Separately

Videos do not need to be hosted on the same server as the channel.

For example:

```text
TV channel:
https://channel.example.com/

Videos:
https://cdn.example.com/videos/
```

`videos.json` can contain:

```json
[
    "https://cdn.example.com/videos/show1.mp4",
    "https://cdn.example.com/videos/show2.mp4",
    "https://cdn.example.com/videos/show3.mp4"
]
```

This is useful when the video files are too large for the web host.

Make sure the video server allows browser playback.

---

# Custom Themes

Themes are stored in `config.json`.

Example:

```json
"themes": {
    "default": {
        "tickerBackground": "#111111",
        "tickerText": "#ffffff",
        "clockBackground": "#111111",
        "clockText": "#ffffff",
        "iconBackground": "#ffffff",
        "iconColor": "#111111",
        "weatherBackground": "#222222"
    },

    "blue": {
        "tickerBackground": "#003b73",
        "tickerText": "#ffffff",
        "clockBackground": "#002b52",
        "clockText": "#ffffff",
        "iconBackground": "#ffffff",
        "iconColor": "#003b73",
        "weatherBackground": "#005ca8"
    }
}
```

Set the active theme with:

```json
"theme": "blue"
```

You can create as many themes as you want.

---

# Advertisements

Advertisements are controlled by the `advertisements` array.

Example:

```json
"advertisements": [
    "Welcome to our channel!",
    "Visit our website today!",
    "Now playing your favorite programs!",
    "This is an advertisement."
]
```

The ticker cycles through the advertisements and then displays the current weather.

The amount of time each item stays on screen is controlled by:

```json
"tickerDuration": 7000
```

The value is milliseconds.

For example:

```json
"tickerDuration": 5000
```

means five seconds.

---

# OBS Studio

The channel can be used directly in OBS using a **Browser Source**.

In OBS:

1. Add a new **Browser Source**.
2. Enter the URL of your channel.
3. Set the width to your stream width.
4. Set the height to your stream height.
5. Enable page loading/reload as appropriate.
6. Make sure the URL is accessible from the OBS computer.

For a 1080p stream:

```text
Width: 1920
Height: 1080
```

For a 720p stream:

```text
Width: 1280
Height: 720
```

The channel automatically fills the browser viewport.

---

# Video Scaling

The video intentionally uses:

```css
width: 100vw;
height: 100vh;
object-fit: fill;
```

It does **not** use:

```css
object-fit: cover;
```

or:

```css
object-fit: contain;
```

This means the video is literally stretched to the browser's width and height.

For example, a 4:3 video displayed in a 16:9 browser will be stretched horizontally.

This is intentional.

---

# Autoplay and Audio

The video starts with:

```html
autoplay
muted
playsinline
```

This allows browsers and OBS to start playback without requiring an initial user interaction in most cases.

Clicking the video changes:

```javascript
videoElement.muted = false;
```

and attempts to start playback with audio.

Browsers may still apply their own autoplay policies.

---

# Weather

The weather section uses the Open-Meteo API.

The browser obtains the viewer's approximate location through the browser Geolocation API and requests current weather information.

If location access is unavailable or denied, the ticker displays:

```text
Weather unavailable
```

The weather is periodically refreshed.

---

# Eastern Time Clock

The clock explicitly uses:

```text
America/New_York
```

rather than the computer's local timezone.

Therefore, the clock remains Eastern Time even when the computer running the channel is located in another timezone.

It automatically handles Eastern Standard Time and Eastern Daylight Time.

---

# Troubleshooting

## `config.json` does not load

Make sure it is in the same directory as `index.html`:

```text
index.html
config.json
videos.json
```

Try opening the configuration directly:

```text
https://example.com/config.json
```

You should see JSON in the browser.

---

## `videos.json` does not load

Open it directly:

```text
https://example.com/videos.json
```

It must contain a JSON array.

Correct:

```json
[
    "https://example.com/video1.mp4",
    "https://example.com/video2.mp4"
]
```

Incorrect:

```json
{
    "video1": "https://example.com/video1.mp4"
}
```

---

## Videos do not play

Check that the video URL works directly in a browser.

For example:

```text
https://example.com/video.mp4
```

Also check that the video server supports browser access and sends an appropriate MIME type.

For MP4:

```text
video/mp4
```

---

## Cross-origin video problems

If the HTML page is hosted on:

```text
https://channel.example.com/
```

but the video is hosted on:

```text
https://videos.example.com/
```

the video server may need to provide:

```text
Access-Control-Allow-Origin: *
```

or otherwise allow the channel's origin.

---

## OBS shows a black screen

Check the URL in a normal browser first.

Then verify that:

* `index.html` loads
* `config.json` loads
* `videos.json` loads
* the video URLs work
* the OBS Browser Source has the correct width and height
* the channel is served over HTTP/HTTPS rather than opened as a `file://` URL

---

# Recommended Production Structure

For a larger channel, a useful structure is:

```text
tv-channel/
│
├── index.html
├── config.json
├── videos.json
├── icon.png
│
└── assets/
    ├── icons/
    ├── images/
    └── fonts/
```

The videos themselves can remain on a separate CDN:

```text
https://cdn.example.com/tv/
```

This keeps the web application small while allowing the video library to grow independently.

# License

You can modify and deploy this project for your own web server or streaming setup. Check the licensing and hosting requirements for any third-party videos, images, fonts, APIs, or other assets you add.
