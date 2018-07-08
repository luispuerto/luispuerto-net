---
id: 700
title: Installing PGP signing for Git on macOS
date: 2017-11-04T23:16:39+00:00
guid: http://luisspuerto.net/?p=700
permalink: /2017/11/installing-pgp-signing-for-git-on-macos/
image: /wp-content/uploads/2017/11/11407107023_b52fa108f7_b.jpg
categories:
  - Professional
  - Technology
tags:
  - coding
  - git
  - pgp
---
<div id="attachment_701" style="width: 264px" class="wp-caption alignleft">
  <a href="http://luisspuerto.net/wp-content/uploads/2017/11/commits.png"><img class=" wp-image-701" src="http://luisspuerto.net/wp-content/uploads/2017/11/commits.png" alt="" width="254" height="298" srcset="http://luisspuerto.net/wp-content/uploads/2017/11/commits.png 594w, http://luisspuerto.net/wp-content/uploads/2017/11/commits-256x300.png 256w, http://luisspuerto.net/wp-content/uploads/2017/11/commits-214x250.png 214w" sizes="(max-width: 254px) 100vw, 254px" /></a>

  <p class="wp-caption-text">
    Only some commits were "verified"
  </p>
</div>

I've been committing on [Git](https://git-scm.com) a lot lately and I've been uploading those commits to [GitHub](https://github.com). At the same time, I've been doing some changes in the repository directly on GitHub and I noticed that the commits that I've done in GitHub itself were verified, but the ones that I was uploading from my computer were not. So, I decided to investigated how to "verify" the commits I upload from my computer. Turns out, that GitHub —and I suppose the rest of the online repositories— and Git are able to sign with a PGP key the commits you make to verify your identity against other people. It's a way to be sure you, and only you, are the one that are committing, thus responsible for the things are doing.

If you what to set up the PGP signing is pretty easy in principle, but could have some caveats. To be honest, I struggled with it on the beginning and every time I committed after I set if up in the beginning I got the following message:

```sh 
error: gpg failed to sign the data
fatal: failed to write commit object
```

<div id="attachment_702" style="width: 282px" class="wp-caption alignright">
  <a href="http://luisspuerto.net/wp-content/uploads/2017/11/Screen-Shot-2017-11-04-at-19.33.59.png"><img class="wp-image-702 size-full" src="http://luisspuerto.net/wp-content/uploads/2017/11/Screen-Shot-2017-11-04-at-19.33.59.png" alt="" width="272" height="177" /></a>

  <p class="wp-caption-text">
    Verified Signature in GitHub
  </p>
</div>

You can check the knowledge base that GitHub has about the topic [here](https://help.github.com/articles/signing-commits-with-gpg/). However, I found that some of the topics are perhaps a little bit outdated and doesn't give you clear directions about how you can really do it. I also checked this sources to get my solution post:

  * [Github : Signing commits using GPG (Ubuntu/Mac)](https://gist.github.com/ankurk91/c4f0e23d76ef868b139f3c28bde057fc)
  * [Git error - gpg failed to sign data](https://stackoverflow.com/questions/41052538/git-error-gpg-failed-to-sign-data)
  * [Github GPG + Keybase PGP](https://www.ahmadnassri.com/blog/github-gpg-keybase-pgp/)
  * [Automatic Git commit signing with GPG on OSX](https://gist.github.com/bmhatfield/cc21ec0a3a2df963bffa3c1f884b676b)
  * [Git Tools - Signing Your Work](https://git-scm.com/book/tr/v2/Git-Tools-Signing-Your-Work)

### How I've done it

First of all, you need to install <span class="lang:sh highlight:0 decode:true crayon-inline">GPG (GNU PGP)</span>  —if you don't already have it—, the <span class="lang:sh highlight:0 decode:true crayon-inline ">gpg agent</span> , and probably you are going to need to install <span class="lang:sh highlight:0 decode:true crayon-inline ">pinentry</span>  for mac. I've installed all of them using [homebrew](https://brew.sh).

```sh 
$ brew install gpg gpg-agent pinentry-mac 

```

Now, you need to created a PGP key runnnig the command:

```sh 
$ gpg --full-generate-key
```

You can do it also with:

```sh 
$ gpg --gen-key 

```

However, if you do like the latter command is not going to give you the option to change the key size, as suggested by [GitHub](https://help.github.com/articles/generating-a-new-gpg-key/).

Answer all the prompted questions and be specially careful with your email since GitHub is going to recognize you by your verify email. You can also create a comment to identify the key e.g. _GitHut Key_. Finally create a passphrase that you can remember, or note down in a password manager.

Now, you can have to list all your keys with the command:

```sh 
$ gpg --list-secret-keys --keyid-format LONG

/Users/lpuerto/.gnupg/pubring.kbx
---------------------------------
sec   rsa4096/&lt;YOUR_LONG_KEY_ID&gt; 2017-11-04 [SC]
      349340ARAFKAJHFA93O8024O02JQ9304OQIRF0QU
uid                 [ultimate] Your Name (GitHub Key) &lt;youremail@domine.com&gt;
ssb   rsa4096/62E5B29EEA7145E 2017-11-04 [E]

```

You have to copy to your clipboard <span class="lang:sh highlight:0 decode:true crayon-inline "><YOUR_LONG_KEY_ID></span> , with is your key id, and paste in the following commands to configure you Git with your key.

```sh 
$ git config --global user.signingKey &lt;YOUR_LONG_KEY_ID&gt;
$ git config --global commit.gpgsign true
```

You can certainly not pass the command

```sh 
$ git config --global commit.gpgsign true
```

that configures your Git to always sign your commits with your signature and sign just certain commits with adding the flag <span class="lang:sh highlight:0 decode:true crayon-inline ">-S</span>  to the <span class="lang:sh highlight:0 decode:true crayon-inline ">git commit</span>  [command](https://help.github.com/articles/signing-commits-using-gpg/). It's an option that some people for security matters and to not be prompted every time you commit to enter your passphrase. However, as you can are going to see latter, you can keep your passphrase in your keychain using <span class="lang:sh highlight:0 decode:true crayon-inline ">pinentry</span> .

You also need to copy your long key id again to get your PGP key with the following command.

```sh 
$ gpg --armor --export &lt;YOUR_LONG_KEY&gt;
```

Which print you full GPG key, beginning with <span class="lang:sh highlight:0 decode:true crayon-inline ">——BEGIN PGP PUBLIC KEY BLOCK——</span>  and ending with <span class="lang:sh highlight:0 decode:true crayon-inline">——END PGP PUBLIC KEY BLOCK——</span> . Copy it and now you can paste it on your GitHub following [this instructions](https://help.github.com/articles/adding-a-new-gpg-key-to-your-github-account/).

Now, to make it work you need to config pinentry for mac as your dialog to enter your passphrase. To do that you have to use the following command to write <span class="lang:sh highlight:0 decode:true crayon-inline ">pinentry-program /usr/local/bin/pinentry-mac</span> in <span class="lang:sh highlight:0 decode:true crayon-inline ">gpg agent</span> config file.

```sh 
$ echo "pinentry-program /usr/local/bin/pinentry-mac" &gt;&gt; ~/.gnupg/gpg-agent.conf

```

You can also do it manually.

```sh 
$ open ~/.gnupg/gpg-agent.conf
```

Finally, you need to restart the <span class="lang:sh highlight:0 decode:true crayon-inline ">gpg agent</span>  doing the following. This is really important and it's was one of the reason because I took me so long to finally configure Git with the PGP. Since I haven't restarted the gpg-agent it hasn't pick up the configuration.

```sh 
$ gpgconf --kill gpg-agent
```

It's done!

I recommend you to test it doing a test commit. First time it should to prompt you with a dialog like this

<div id="attachment_710" style="width: 1053px" class="wp-caption alignnone">
  <a href="http://luisspuerto.net/wp-content/uploads/2017/11/pinentry-mac.png"><img class="size-full wp-image-710" src="http://luisspuerto.net/wp-content/uploads/2017/11/pinentry-mac.png" alt="" width="1043" height="501" srcset="http://luisspuerto.net/wp-content/uploads/2017/11/pinentry-mac.png 1043w, http://luisspuerto.net/wp-content/uploads/2017/11/pinentry-mac-300x144.png 300w, http://luisspuerto.net/wp-content/uploads/2017/11/pinentry-mac-768x369.png 768w, http://luisspuerto.net/wp-content/uploads/2017/11/pinentry-mac-1024x492.png 1024w, http://luisspuerto.net/wp-content/uploads/2017/11/pinentry-mac-520x250.png 520w" sizes="(max-width: 1043px) 100vw, 1043px" /></a>

  <p class="wp-caption-text">
    pinentry prompt
  </p>
</div>

Where, if you tick <span class="lang:sh highlight:0 decode:true crayon-inline ">save in keychain</span> , it isn't going to prompt you again.

Note that if you want to sign commits outside of the shell, not all the apps can sign commits. With my setup I've tried Tower and GitHub Desktop and they are able to sign without any problem. Keep  in mind that I've committed first in the shell and then I've clicked <span class="lang:sh highlight:0 decode:true crayon-inline ">save in keychain</span>  so I don't know how is going to behave the first time you commit and you haven't ticked that option or if it's the first time you sign a commit. In case you had problems with Tower, there are a couple of tutorials out there including Tower:

  * [Signing GitHub Commits](https://www.fabianehlert.com/post/signingcommits/)
  * [Signed git commits with Tower](https://aaronparecki.com/2016/07/29/10/git-tower)

Enjoy your Git!

Note: I've follow this setup in Mac OS X El Capitan 10.11.6 and with Git 2.15.
