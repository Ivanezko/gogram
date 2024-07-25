<p align="center">
    <a href="https://github.com/ivanezko/gogram">
        <img src="https://i.imgur.com/RE1M0sM.png" alt="Gogram" width="256">
    </a>
    <br>
    <b>Telegram MTProto API Framework for Golang</b>
    <br>
    <b>
    <a href="/">
        HOME
    </a>
    •
    <a href="/examples/">
        DOCS
    </a>
    •
    <a href="https://github.com/ivanezko/gogram/releases">
        RELEASES
    </a>
    •
    <a href="https://t.me/rosexchat">
        SUPPORT
    </a>
    </b>
</p>

## <b>GoGram</b>

<p>Light Weight, Fast, Elegant Telegram <b><a href="https://core.telegram.org/api">MTProto API</a></b> framework in <b><a href="https://golang.org/">Golang</a></b> for building Telegram clients and bots.</p>

## Status

[![GoDoc](https://godoc.org/github.com/ivanezko/gogram?status.svg)](https://godoc.org/github.com/ivanezko/gogram)
[![Go Report Card](https://goreportcard.com/badge/github.com/ivanezko/gogram)](https://goreportcard.com/report/github.com/ivanezko/gogram)
[![License](https://img.shields.io/github/license/ivanezko/gogram.svg)](https://img.shields.io/github/license/ivanezko/gogram.svg)
[![GitHub stars](https://img.shields.io/github/stars/ivanezko/gogram.svg?style=social&label=Stars)](https://img.shields.io/github/license/ivanezko/gogram.svg?style=social&label=Stars)
[![GitHub forks](https://img.shields.io/github/forks/ivanezko/gogram.svg?style=social&label=Fork)](https://img.shields.io/github/license/ivanezko/gogram.svg?style=social&label=Fork)
[![GitHub issues](https://img.shields.io/github/issues/ivanezko/gogram.svg)](https://img.shields.io/github/license/ivanezko/gogram.svg)
[![GitHub pull requests](https://img.shields.io/github/issues-pr/ivanezko/gogram.svg)](https://img.shields.io/github/license/ivanezko/gogram.svg)

<p>⭐️ <b>Gogram</b> is a modern, elegant and concurrent <b><a href='https://core.telegram.org/api'>MTProto API</a></b>
framework. It enables you to easily interact with the main Telegram API through a user account (custom client) or a bot
identity (bot API alternative) using Go.</p>

## Setup

<p>Please note that Gogram requires Go <b>1.18</b> or later.</p>

```bash
go get -u github.com/ivanezko/gogram/telegram
```

## Getting Started

```golang
package main

import "github.com/ivanezko/gogram/telegram"

func main() {
	client, err := telegram.NewClient(telegram.ClientConfig{
		AppID: 6, AppHash: "<app-hash>",
		// StringSession: "<string-session>",
	})

	if err != nil {
		log.Fatal(err)
	}

	client.LoginBot("<bot-token>") // or client.Login("<phone-number>") for user account, or client.AuthPrompt() for interactive login

	client.On(telegram.OnMessage, func(message *telegram.NewMessage) error { // client.AddMessageHandler
			message.Reply("Hello from Gogram!")
        	return nil
	}, 
        telegram.FilterPrivate) // waits for private messages only

	client.Idle() // block main goroutine until client is closed
}
```

## Support

If you'd like to support Gogram, you can consider:

- [Become a GitHub sponsor](https://github.com/sponsors/ivanezko).

## Key Features

- **Ready**: Install Gogram with go get and you are ready to go!
- **Easy**: Makes the Telegram API simple and intuitive, while still allowing advanced usages.
- **Elegant**: Low-level details are abstracted and re-presented in a more convenient way.
- **Fast**: Backed by a powerful and concurrent library, Gogram can handle even the heaviest workloads.
- **Zero Dependencies**: No need to install anything else than Gogram itself.
- **Powerful**: Full access to Telegram's API to execute any official client action and more.
- **Feature-Rich**: Built-in support for file uploading, formatting, custom keyboards, message editing, moderation tools and more.
- **Up-to-date**: Gogram is always in sync with the latest Telegram API changes and additions (`tl-parser` is used to generate the API layer).

#### Current Layer - **184** (Updated on 2024-07-07)

## Doing Stuff

#### Sending a Message

```golang
client.SendMessage("username", "Hello from Gogram!")

client.SendDice("username", "🎲")

client.On("message:/start", func(m *telegram.NewMessage) error {
    m.Reply("Hello from Gogram!") // m.Respond("...")
    return nil
})
```

#### Sending Media

```golang
client.SendMedia("username", "<file-name>", &telegram.MediaOptions{ // filename/inputmedia,...
    Caption: "Hello from Gogram!",
    TTL: int32((math.Pow(2, 31) - 1)), //  TTL For OneTimeMedia
})

client.SendAlbum("username", []string{"<file-name>", "<file-name>"}, &telegram.MediaOptions{ // Array of filenames/inputmedia,...
    Caption: "Hello from Gogram!",
})

// with progress
var pm *telegram.ProgressManager
client.SendMedia("username", "<file-name>", &telegram.MediaOptions{
    Progress: func(a,b int) {
        if pm == nil {
            pm = telegram.NewProgressManager(a, 3) // 3 is edit interval
        }

        if pm.ShouldEdit(b) {
            fmt.Println(pm.GetStats(b)) // client.EditMessage("<chat-id>", "<message-id>", pm.GetStats())
        }
    },
})
```

#### Inline Queries

```golang
client.On("inline:<pattern>", func(iq *telegram.InlineQuery) error { // client.AddInlineHandler
	builder := iq.Builder()
	builder.Article("<title>", "<description>", "<text>", &telegram.ArticleOptions{
			LinkPreview: true,
	})

	return nil
})
```

#### Callback Queries

```golang
client.On("callback:<pattern>", func(cb *telegram.CallbackQuery) error { // client.AddCallbackHandler
    cb.Answer("This is a callback response", &CallbackOptions{
		Alert: true,
	})
    return nil
})
```

For more examples, check the [examples](examples) directory.

## Features TODO

- [x] Basic MTProto implementation (LAYER 184)
- [x] Updates handling system + Cache
- [x] HTML, Markdown Parsing, Friendly Methods
- [x] Support for Flag2.0, Layer 147
- [x] WebRTC Calls Support
- [ ] Documentation for all methods
- [x] Stabilize File Uploading
- [x] Stabilize File Downloading
- [ ] Secret Chats Support
- [ ] Cdn DC Support

## Known Issues

- [x] ~ Open Issues if Found :)

## Contributing

Gogram is an open-source project and your contribution is very much appreciated. If you'd like to contribute, simply fork the repository, commit your changes and send a pull request. If you have any questions, feel free to ask.

## Resources

- Documentation: [documentation](https://gogramd.vercel.app) (not finished yet)
- Support: [@rosexchat](https://t.me/rosexchat), [@EvieSupport](https://t.me/EvieSupport)

## License

This library is provided under the terms of the [GPL-3.0 License](LICENSE).
