---
title: Media Buttons Behavior Has Changed In macOS High Sierra
date: 2017-11-10 16:52:53
header:
  overlay_image: assets/images/blog/2018/File-Oct-17-10-34-21.jpeg
  teaser: assets/images/blog/2018/File-Oct-17-10-34-21.jpeg
categories:
  - Professional
  - Technology
tags:
  - high-sierra
  - how-to
  - macOS
  - tweaks
---
macOS High Sierra has changed the behavior of the media buttons on macOS —AKA those buttons you use to play/stop music or go forwards or backwards. Before, they were reserved to manage music, whether you where playing in iTunes or Spotify, with iTunes having preference (usually). They also worked with some video player.

Now, Safari has stolen that preference, and if a video or some kind of media is played in Safari —whether in background or foreground, those buttons are going to control it. In other words, you are playing music in iTunes, you find a video in YouTube that you want to watch, so you hit play and push the play/stop key in your keyboard to stop the iTunes music and be able to listen the video audio, but now that is not he behavior. You are going to stop the video, while the music is still playing. And this is not the worse behavior, [some people](https://discussions.apple.com/message/32506712#message32506712) have reported that they are listening to music, receive a call in the iPhone, ringing in macOS —as expected in the last versions of macOS— and when you press play/stop button to stop the music, you answer the call, while the music is still playing. [People isn't happy](https://discussions.apple.com/message/32506712).

{% include figure image_path="assets/images/blog/2018/Screen-Shot-2017-11-10-at-16.42.50.png" alt="High Sierra Media Key Enabler for iTunes" caption="High Sierra Media Key Enabler for iTunes" %}{: .align-left}  

[Some users](https://discussions.apple.com/message/32306332#message32306332) have reported that you can fix the issue —or the [feature](https://www.urbandictionary.com/define.php?term=It%27s%20not%20a%20bug%2C%20it%27s%20a%20feature)— if you log in your Mac on Safe Mode —pressing shift key when the Mac starts— and use the keys with iTunes and then you return to the normal logging. However, that hasn't worked for me and I have to say that in my experience iTunes doesn't behave as they describe. I'm perfectly able to play music on iTunes in safe mode and if I open Safari with a video, the play/stop key behaves in the same way and in normal mode.

What I've found helpful is this [little app](http://milgra.com/high-sierra-media-key-enabler.html) called Media Key Enable that someone recommended in [Ask Different](https://apple.stackexchange.com/questions/300811/high-sierra-mediaplay-button-changes). It just turn back the behavior as in earlier releases. You can check the project in [GitHub](https://github.com/milgra/highsierramediakeyenabler) if you want. If you're a [Homebrew](https://brew.sh) user you can install it with Homebrew cask:

```shell 
brew cask install highsierramediakeyenabler
```

If you are as annoyed as the rest of us for the change in behavior, I recommend you to complain to Apple using  [Apple Feedback](https://www.apple.com/feedback/).
