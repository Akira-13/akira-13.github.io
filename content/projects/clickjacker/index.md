
+++
date = '2026-01-22'
title = 'Clickjacker Analyzer and PoC Generator'
categories = ["Project"]
showTaxonomies = true
showAuthor = false
showDate = false
showReadingTime = false
showWordCount = false
+++

So, while learning about web vulnerabilities, I learned a bit about **clickjacking**. It's a very straightforward vulnerability: if a site can be iframed and the user can commit state-changing actions with a click or a few clicks, then you can trick the user into doing said state-changing actions with a decoy page and an invisble iframe. A classic example of this is the "WIN A PRIZE!" flashy site that required you to click on a button while an iframe with a Facebook page would be over it, right over where the "Like" button would be. It's so direct, the only HTML code it requires most of the time is the following:

```html
<head>
	<style>
		#target_website {
			position:relative;
			width:{{some_width}};
			height:{{some_height}};
			opacity:0.00001;
			z-index:2;
			}
		#decoy_website {
			position:absolute;
			top:{{decoy_width}};
			left:{{decoy_height}};
			z-index:1;
			}
	</style>
</head>

<body>
	<div id="decoy_website">
    BIG FLASHY BUTTON HERE!!! CLICK ME!! YOU MUST CLICK ME!!
	</div>
	<iframe id="target_website" src="https://vulnerable-website.com">
	</iframe>
</body>
```

Even though it is very straightforward, building a proof of concept for this vulnerability can be a pain since it requires a lot of manual callibration with CSS, as can be seen from the code. For this reason, I built a visual callibrator for easy PoC generation. This way, instead of adjusting CSS, you can adjust the iframe visually right over the decoy button.

![Clickjack Analyzer and PoC Generator GIF](https://github.com/user-attachments/assets/7aecbf00-436d-41f2-a0b5-59c457c7faa5)

## Features

### Vulnerability analyzer

I've included a script that detects if a site is vulnerable to clickjacking by analyzing its HTTP headers. Specifically, its `X-Frame-Options` and `Content-Security-Policy` to verify if the site can be framed. Also, it analyzes its cookies to verify if requests generated from a frame are considered valid.

### PoC generator

A visual PoC generator where you can drag and resize the iframe with the target site right over the decoy.

### Decoy templates

- Generic button with customizable text.
- Fake reCAPTCHA with failing animation.
- Fake redirecting message with hyperlink.

### Sandboxing

Allows to sandbox the iframe in order to bypass frame busters.

## Conclusions
 
Overall, I really liked working on this. I learned a lot about HTTP headers, social engineering, Hopefully it'll be useful for anyone who wants to build a quick PoC for clickjacking. 

Check out the source [here {{<icon "github">}}](https://github.com/Akira-13/clickjacker-poc) 