Install gotty via brew on Ubuntu

```
brew install yudai/gotty/gotty
```

Install tmux and zsh
```
brew install tmux zsh
```


Create the index page with a Hack font support in ~/.gotty.html . 

```
<!doctype html>
<html>
  <head>
    <title>zsh@media</title>
    <link rel="icon" type="image/png" href="favicon.png">
    <link rel="stylesheet" href="./css/index.css" />
    <link rel="stylesheet" href="./css/xterm.css" />
    <link rel="stylesheet" href="./css/xterm_customize.css" />
    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/hack-font/3.003/web/hack.min.css" />
    <style>
     .terminal {
       font-family: 'Hack', monospace;
       font-size: 13px;
     }
    </style>
  </head>
  <body>
    <div id="terminal"></div>
    <script src="./auth_token.js"></script>
    <script src="./config.js"></script>
    <script src="./js/gotty-bundle.js"></script>
  </body>
</html>
```

Create gotty config in ~/.gotty

```
address = "0.0.0.0"
port = "8022"
permit_write = true
enable_basic_auth = true
credential = "user:pass"
preferences {
    font_family = "'Hack', monospace"
    user_css = "https://cdnjs.cloudflare.com/ajax/libs/hack-font/3.003/web/hack.min.css"
}
```

Create user service file ~/.config/systemd/user/gotty.service

```
[Unit]
Description=Gotty
After=network.target network-online.target

[Service]
Environment="TERM=xterm"
Environment="GOTTY_TERM=hterm"
Environment="GOTTY_CREDENTIAL=username:password"
ExecStart=gotty --config %h/.gotty --term xterm --index %h/.gotty.html /usr/bin/tmux new -A -s gotty
Restart=always
CPUShares=256
MemoryLimit=256M

[Install]
WantedBy=default.target
```