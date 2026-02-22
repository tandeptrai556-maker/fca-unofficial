<div align="center">

<br>

```
  ███████╗ ██████╗ █████╗       ██╗   ██╗███╗   ██╗ ██████╗ ███████╗███████╗
  ██╔════╝██╔════╝██╔══██╗      ██║   ██║████╗  ██║██╔═══██╗██╔════╝██╔════╝
  █████╗  ██║     ███████║      ██║   ██║██╔██╗ ██║██║   ██║█████╗  █████╗
  ██╔══╝  ██║     ██╔══██║      ██║   ██║██║╚██╗██║██║   ██║██╔══╝  ██╔══╝
  ██║     ╚██████╗██║  ██║      ╚██████╔╝██║ ╚████║╚██████╔╝██║     ██║
  ╚═╝      ╚═════╝╚═╝  ╚═╝       ╚═════╝ ╚═╝  ╚═══╝ ╚═════╝ ╚═╝     ╚═╝
```

### `@dongdev/fca-unofficial`

**Unofficial Facebook Messenger API for Node.js**
Automate chat on personal accounts — without the official API restrictions.

<br>

[![npm](https://img.shields.io/npm/v/@dongdev/fca-unofficial?style=flat-square&color=cb3837&label=npm)](https://www.npmjs.com/package/@dongdev/fca-unofficial)
[![downloads](https://img.shields.io/npm/dm/@dongdev/fca-unofficial?style=flat-square&color=4f6ef7&label=downloads%2Fmo)](https://www.npmjs.com/package/@dongdev/fca-unofficial)
[![license](https://img.shields.io/badge/license-MIT-22c55e?style=flat-square)](./LICENSE-MIT)
[![node](https://img.shields.io/badge/node-%3E%3D12.0.0-eab308?style=flat-square&logo=nodedotjs&logoColor=eab308)](https://nodejs.org/)

<br>

[**Quick Start**](#-quick-start) · [**API Reference**](#-api-reference) · [**AppState**](#-appstate) · [**Examples**](#-examples) · [**Projects**](#-built-with-this-api)

<br>

</div>

---

## ⚠️ Disclaimer

> **Use responsibly. We are not liable for account bans.**

Accounts may be flagged or banned for:

- Sending bulk messages to unknown users
- Messaging at an unusually high rate
- Sharing spammy-looking URLs
- Logging in and out repeatedly

**Recommendations:** use `AppState` instead of credentials, implement rate limiting in your bots, and comply with [Facebook's Terms of Service](https://www.facebook.com/terms.php). For iOS users experiencing session drops, use **Firefox** or visit [fca.dongdev.id.vn](https://fca.dongdev.id.vn).

---

## ✦ Why this exists

Facebook's [official Messenger API](https://developers.facebook.com/docs/messenger-platform) only supports **Pages**, not personal accounts.

`@dongdev/fca-unofficial` fills that gap — it emulates browser requests to give you full programmatic access to a personal Facebook account. No auth tokens required, just credentials or a saved AppState.

**What you can build:**

- 🤖 Chatbots and auto-responders
- 🔔 Custom notification systems
- 🎮 Interactive group chat games
- 🌉 Bridges to other platforms (Discord, Matrix, IRC, Telegram)
- 📊 Message analytics and monitoring tools

---

## 📦 Installation

```bash
npm install @dongdev/fca-unofficial@latest
```

**Requirements:** Node.js `≥ 12.0.0` · Active Facebook account

---

## 🚀 Quick Start

### Echo bot

```js
const login = require("@dongdev/fca-unofficial");

login({ appState: [] }, (err, api) => {
  if (err) return console.error(err);

  api.listenMqtt((err, event) => {
    if (err) return console.error(err);
    api.sendMessage(event.body, event.threadID);
  });
});
```

### Send a message

```js
const login = require("@dongdev/fca-unofficial");

login({ appState: [] }, (err, api) => {
  if (err) return console.error(err);

  api.sendMessage("Hey! 👋", "000000000000000", err => {
    if (err) return console.error(err);
    console.log("✓ Sent!");
  });
});

// 💡 Find your Facebook ID in cookies under the key `c_user`
```

### Send a file or image

```js
const login = require("@dongdev/fca-unofficial");
const fs    = require("fs");

login({ appState: [] }, (err, api) => {
  if (err) return console.error(err);

  api.sendMessage(
    {
      body:       "Check this out 📷",
      attachment: fs.createReadStream(`${__dirname}/image.jpg`),
    },
    "000000000000000",
    err => {
      if (err) return console.error(err);
      console.log("✓ Sent!");
    }
  );
});
```

### Full bot with stop command

```js
const login = require("@dongdev/fca-unofficial");
const fs    = require("fs");

login({ appState: JSON.parse(fs.readFileSync("appstate.json", "utf8")) }, (err, api) => {
  if (err) return console.error(err);

  api.setOptions({ listenEvents: true });

  const stopListening = api.listenMqtt((err, event) => {
    if (err) return console.error(err);

    api.markAsRead(event.threadID);

    switch (event.type) {
      case "message":
        if (event.body?.trim().toLowerCase() === "/stop") {
          api.sendMessage("Goodbye 👋", event.threadID);
          return stopListening();
        }
        api.sendMessage(`🤖 ${event.body}`, event.threadID);
        break;

      case "event":
        console.log("Thread event:", event);
        break;
    }
  });
});
```

---

## 💾 AppState

Using AppState lets you persist your session without re-entering credentials every time.

### Save AppState

```js
const login = require("@dongdev/fca-unofficial");
const fs    = require("fs");

login({ appState: [] }, (err, api) => {
  if (err) return console.error(err);
  fs.writeFileSync("appstate.json", JSON.stringify(api.getAppState(), null, 2));
  console.log("✓ AppState saved");
});
```

### Load AppState

```js
const login = require("@dongdev/fca-unofficial");
const fs    = require("fs");

login(
  { appState: JSON.parse(fs.readFileSync("appstate.json", "utf8")) },
  (err, api) => {
    if (err) return console.error(err);
    console.log("✓ Logged in via AppState");
  }
);
```

> **Alternative:** Use [c3c-fbstate](https://github.com/c3cbot/c3c-fbstate) to export `fbstate.json` directly.

---

## 📨 Message Types

| Type | Object |
|---|---|
| Text | `{ body: "hello" }` or just `"hello"` |
| Sticker | `{ sticker: "369239263222822" }` |
| File / Image | `{ attachment: fs.createReadStream("file.jpg") }` |
| URL with preview | `{ url: "https://example.com" }` |
| Large emoji | `{ emoji: "👍", emojiSize: "large" }` |

> A message can have `body` plus **one of** `sticker`, `attachment`, or `url`.
> Emoji sizes: `small` · `medium` · `large`

---

## ⚙️ Options

```js
api.setOptions({
  listenEvents: true,   // Receive group events (join/leave, rename, etc.)
  selfListen:   false,  // Receive your own sent messages
  logLevel:     "warn", // silent | error | warn | info | verbose
});
```

---

## 📡 Events

Received through `api.listenMqtt()` when `listenEvents: true`:

| Event type | Description |
|---|---|
| `message` | New message received |
| `event` | Thread event (join, leave, title change…) |
| `typ` | Typing indicator |
| `read_receipt` | Someone read your message |
| `delivery_receipt` | Message delivered |
| `read` | Message read status update |
| `presence` | User online/offline status |

---

## 📖 API Reference

### Messaging

```
api.sendMessage(msg, threadID, [callback])
api.editMessage(body, messageID, [callback])
api.deleteMessage(messageID, [callback])
api.unsendMessage(messageID, [callback])
api.setMessageReaction(reaction, messageID, [callback])
api.sendTypingIndicator(threadID, [callback])
api.getMessage(threadID, limit, [callback])
api.forwardAttachment(attachmentID, threadID, [callback])
api.uploadAttachment(attachment, [callback])
api.createPoll(question, options, threadID, [callback])
api.shareContact(contactID, threadID, [callback])
```

### Read & Delivery

```
api.markAsRead(threadID, [callback])
api.markAsReadAll([callback])
api.markAsDelivered(threadID, [callback])
api.markAsSeen(threadID, [callback])
```

### Thread Management

```
api.getThreadInfo(threadID, [callback])
api.getThreadList(limit, timestamp, [callback])
api.getThreadHistory(threadID, amount, timestamp, [callback])
api.getThreadPictures(threadID, limit, [callback])
api.searchForThread(name, [callback])
api.deleteThread(threadID, [callback])
api.muteThread(threadID, muteSeconds, [callback])
api.changeArchivedStatus(threadID, archived, [callback])
api.handleMessageRequest(threadID, accept, [callback])
```

### Thread Customization

```
api.setTitle(title, threadID, [callback])
api.changeThreadColor(color, threadID, [callback])
api.changeThreadEmoji(emoji, threadID, [callback])
api.changeGroupImage(image, threadID, [callback])
api.changeNickname(nickname, userID, threadID, [callback])
```

### Users

```
api.getUserInfo(userID, [callback])
api.getUserInfoV2(userID, [callback])
api.getUserID(username, [callback])
api.getFriendsList([callback])
api.getCurrentUserID([callback])
api.handleFriendRequest(userID, accept, [callback])
api.unfriend(userID, [callback])
api.changeBlockedStatus(userID, block, [callback])
api.changeAvatar(image, [callback])
api.changeBio(bio, [callback])
```

### Group Management

```
api.createNewGroup(participantIDs, groupTitle, [callback])
api.addUserToGroup(userID, threadID, [callback])
api.removeUserFromGroup(userID, threadID, [callback])
api.changeAdminStatus(userID, threadID, admin, [callback])
```

### Auth & Config

```
api.getAppState()
api.setOptions(options)
api.logout([callback])
api.listenMqtt(callback)         → returns stopListening()
api.refreshFb_dtsg([callback])
```

### Media & Misc

```
api.resolvePhotoUrl(photoID, [callback])
api.getEmojiUrl(emoji, size, [callback])
api.getThemePictures(threadID, [callback])
api.createThemeAI(threadID, [callback])
api.setPostReaction(postID, reaction, [callback])
```

---

## 🛠 Built with this API

| Project | Description |
|---|---|
| [c3c](https://github.com/lequanglam/c3c) | Plugin-based bot supporting Facebook & Discord |
| [Miraiv2](https://github.com/miraiPr0ject/miraiv2) | Simple, extensible Messenger bot |
| [Messer](https://github.com/mjkaufer/Messer) | Command-line Messenger client |
| [messen](https://github.com/tomquirk/messen) | Rapidly build Messenger apps in Node.js |
| [Concierge](https://github.com/concierge/Concierge) | Modular bot with built-in package manager |
| [Botyo](https://github.com/ivkos/botyo) | Modular bot for group chat |
| [matrix-puppet-facebook](https://github.com/matrix-hacks/matrix-puppet-facebook) | Facebook bridge for Matrix |
| [Miscord](https://github.com/Bjornskjald/miscord) | Facebook ↔ Discord bridge |
| [chat-bridge](https://github.com/rexx0520/chat-bridge) | Messenger, Telegram & IRC bridge |
| [Messenger-CLI](https://github.com/AstroCB/Messenger-CLI) | Terminal interface for Messenger |
| [BotCore](https://github.com/AstroCB/BotCore) | Tools for writing Messenger bots |

---

## 🤝 Contributing

Pull requests are welcome!

```bash
git checkout -b feature/your-feature
git commit -m "feat: add your feature"
git push origin feature/your-feature
# → open a Pull Request
```

Please follow the existing code style, add tests for new features, and update docs where needed.

---

## 📄 License

[MIT](./LICENSE-MIT) © [DongDev](https://github.com/Donix-VN)

---

<div align="center">

**Links**

[NPM](https://www.npmjs.com/package/@dongdev/fca-unofficial) · [GitHub](https://github.com/Donix-VN/fca-unofficial) · [Issues](https://github.com/Donix-VN/fca-unofficial/issues) · [Support](https://www.facebook.com/mdong.dev)

<br>

*This is an unofficial library and is not affiliated with or endorsed by Meta Platforms, Inc.*

</div>
