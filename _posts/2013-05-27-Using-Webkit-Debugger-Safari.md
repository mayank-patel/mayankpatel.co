---
layout: post
title: "Using webkit developer tools in safari"
description: "Replace the safari web inspector with the webkit default developer tools"
category:
keywords: ["tools", "web-dev"]
---

The Safari Web Inspector is a nightmare for developers. But its no more a pain because its possible to use webkit web inspector in safari. Here are the steps to do that:

1. Download and install the <a href="http://nightly.webkit.org/" target="_blank">Webkit Nightly Builds</a>
2. Open the terminal and type in the following commands:
<pre>
<code class="language-javascript">
defaults write com.apple.Safari IncludeDevelopMenu -bool true
defaults write com.apple.Safari WebKitDeveloperExtrasEnabledPreferenceKey -bool true
defaults write com.apple.Safari com.apple.Safari.ContentPageGroupIdentifier.WebKit2DeveloperExtrasEnabled -bool true
</code>
</pre>
3. Close safari and open Webkit.
4. Notice, safari opens up with Develop Menu enabled. Select "Use Webkit Inspector" if not selected.
5. You are all set to use the webkit web inspector with safari.
