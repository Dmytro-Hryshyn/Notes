## YouTube-dl

youtube-dl is an open-source download manager for video and audio from YouTube and over 1000 other video hosting websites. It is released under the Unlicense software license. As of March 2021, youtube-dl is one of the most starred projects on GitHub, with over 91,300 stars. According to libraries.io, 84 other packages and 1.43k repositories depend on it



### Install on Arch, Manjaro

```sh
# Youtube-dl uses ffmpeg, it needs to be installed
sudo packman -S ffmpeg
sudo pacman -S youtube-dl
```

### Download mp3 with best quality

```sh
youtube-dl --extract-audio --audio-format --prefer-ffmpeg mp3 <video URL>
```

### Download mp3 play list by URL

```sh
youtube-dl -i -x --audio-format mp3 --prefer-ffmpeg <Play list URL>
```

### Download best video and audio by URL

```sh
youtube-dl -f 'bestvideo[ext=mp4]+bestaudio[ext=m4a]/mp4' <URL>
```

