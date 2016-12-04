# Asp.net core on LINUX. It's the future. Let's dive right in!

.net core on Linux presentation from DDD Brisbane 2016 with Joseph Cooney


Abstract:

> Ever wanted to say goodbye to the registry, COM, group policy, Windows updates, 'anniversary editions' of software, and GDI handles? Does a production server where your code runs seem like the wrong place for internet explorer to be, even a bizzaro locked down secure mode version?

> If you're a .net dev with no Linux experience this session is for you. 

> You'll go from novice to confident with skillz in SSH, apt-get, nginx, bash, supervisor, fail2ban, kestrel and most of all .net core! Neck beards are entirely optional.


## To see the presentation...

Visit <https://secretgeek.github.io/DDD_Linux_Core/>

## To relive the demo

Go to the `demo` subfolder and view the `scripts.md` file.


## Tools

Why spend time preparing a demo when you can play with tools instead?

Tools required for this talk are:

1. Windows subsystem for linux <https://til.secretgeek.net/windows/bash_on_windows.html>
2. Cmder <http://cmder.net/> 
    - Paste: `shift insert` (`fn shift enter` on Macbook air)
    - Theme: Tomorrow (light) or Monokai (dark)
    - Font size: 32
    - Note arrow keys don't work in Bash
3. BigNote fork at <https://github.com/secretGeek/bignote> 
    - hotkey: `ctrl f1`
    - alt left and right arrows to move back and forward through snippets
4. Zoomit from sysinternals <https://technet.microsoft.com/en-us/sysinternals/zoomit.aspx> 
    - hotkey: `ctrl 1` to zoom
    - hotkey: `ctrl 2` to draw
5. Higgins (included with this repo)
6. A digital ocean account, which knows a public ssh youv'e generated previously


## Contributing...

Edit the `index.html` file. Content is written in markdown or html.

It uses a very minimal presenter called 'higgins' (for a quick higgins tutorial, view the `slides/higgins_example.html` file).

To add a slide, add another div with the class 'slide', and put the slide content inside that div.

Either put in raw html, or if you add a "md" class to the div.slide, you can write in markdown.

If there are notes associated with the slide, then add a span with a 'notes' class, inside that div.

Got new images? Add them to the `image` directory.

Want to improve the style? edit the `higgins.css` file.


