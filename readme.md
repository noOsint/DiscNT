# DiscNT - Previously Discum (DISCORD-S.C.U.M)
A simple, easy-to-use, non-restrictive, synchronous Discord API Wrapper for Selfbots/Userbots written in Python.

### ⚠️ **Risky Actions**: [Issue #66](https://github.com/noOsint/DiscNT/issues/66#issue-876713938)

---

## 📌 Table of Contents
- [Key Features](#key-features)
- [About](#about)
- [Installation](#installation)
  - [Prerequisites](#prerequisites-installed-automatically-using-above-methods)
- [Documentation](#documentation)
- [Contributing](#contributing)
- [Example Usage](#example-usage)
- [Links](#links)
- [FAQ](#faq)
- [Notes](#notes)

---

## 🚀 Key Features
- ✅ Easy-to-use for selfbots/userbots
- 🔧 Simple to extend and modify
- 📖 Readable and well-organized
- 🎭 Mimics the official client while providing full control
- 🔍 Uses user/private APIs
- 🔄 Supports event-based (gateway) interactions
- 🔬 [Highly customizable `fetchMembers` function](docs/using/fetchingGuildMembers.md)
- 📡 Remote authentication capabilities
- 🐍 **Supports Python 2.7** (but Python 3+ recommended)

---

## ℹ️ About
**DiscNT** is a Discord API wrapper designed specifically for selfbots and userbots. It provides an interface to interact with Discord's **HTTP API** (requests) and **Gateway API** (websockets) just like the official client.

Unlike traditional libraries like `discord.py`, DiscNT is **built specifically for user accounts** and offers direct access to private APIs. This makes it an ideal tool for those looking to automate and extend their Discord experience.

⚠️ **Warning:** Using a selfbot is against Discord's Terms of Service and may result in a ban if not used carefully. **DiscNT does not include built-in rate limit handling** to ensure simplicity and give users full control. The developers are **not responsible** for any consequences resulting from the use of this tool.

---

## 🔧 Installation  
To install DiscNT, run:
```
python -m pip install --user --upgrade git+https://github.com/noOsint/DiscNT.git#egg=discum
```

For additional **remote authentication functions** (login using phone & QR code), install with:
```
python -m pip install --user --upgrade -e git+https://github.com/noOsint/DiscNT.git#egg=discum[ra]
```

### 📦 Prerequisites (installed automatically)
- `requests`
- `requests_toolbelt`
- `brotli`
- `websocket_client==0.59.0`
- `filetype`
- `ua-parser`
- `colorama`

#### 🔑 Remote Authentication Prerequisites
(If installing with `[ra]` flag)
- `pyqrcode`
- `pycryptodome`
- `pypng`

---

## 📚 Documentation
📖 [Read the Docs](https://github.com/noOsint/DiscNT/tree/master/docs)

---

## 🤝 Contributing
We welcome contributions! You can:
- Report issues 🐞
- Submit pull requests 🔀
- Suggest new features 💡

Not all suggestions may be implemented, as **DiscNT aims to remain a transparent and raw Discord user API wrapper**. See our [contribution guidelines](https://github.com/noOsint/DiscNT/blob/master/contributing.md) for details.

---

## 📝 Example Usage
Basic example:
```
import discum

TOKEN = "USER_TOKEN"
CHANNEL_ID = "CHANNEL_ID"

bot = discum.Client(token=TOKEN, log=False)


def send_message(channel_id: str, message: str) -> None:
    """Sends a message to the specified Discord channel."""
    bot.sendMessage(channel_id, message)


@bot.gateway.command
def handle_gateway_events(resp) -> None:
    """Handles events from the Discord Gateway."""
    if resp.event.ready_supplemental:  # Triggered after the ready event
        user = bot.gateway.session.user
        print(f"Logged in as {user['username']}#{user['discriminator']}")

    if resp.event.message:
        message_data = resp.parsed.auto()
        guild_id = message_data.get("guild_id", "DM")
        channel_id = message_data["channel_id"]
        username = message_data["author"]["username"]
        discriminator = message_data["author"]["discriminator"]
        content = message_data["content"]

        print(f"> Guild {guild_id} | Channel {channel_id} | {username}#{discriminator}: {content}")


if __name__ == "__main__":
    send_message(CHANNEL_ID, "Hello, world!")
    bot.gateway.run(auto_reconnect=True)
```

---

## 🔗 Links
- 📖 [Documentation](docs)  
- 💡 [More Examples](examples)  
- 📝 [Changelog](changelog.md)  
- 🔗 [GitHub Repository](https://github.com/noOsint/DiscNT)  

---

## ❓ FAQ

#### ❓ Does DiscNT support bot accounts?  
**No.** DiscNT only supports **user accounts**.

#### ❓ What's the difference between user/private API and bot API?  
- **User APIs** are used by the official Discord client and are mostly undocumented.  
- **Bot APIs** are used by bot accounts and are publicly documented.  
**DiscNT exclusively uses user APIs.**

#### ❓ How do I fix `[SSL: CERTIFICATE_VERIFY_FAILED]` errors?  
Check out this solution: [StackOverflow](https://stackoverflow.com/a/53310545/14776493)

#### ❓ Why am I getting `KeyError: 'members'` when running `bot.gateway.session.guild(guild_ID).members`?  
This occurred in older versions where the `members` key wasn't set until you manually fetched them using:  
```
bot.gateway.fetchMembers(...); bot.gateway.run()
```
The latest version now initializes `members` as an empty dictionary by default.

#### ❓ How to fix `ImportError: DLL load failed: The specified module could not be found` for `_brotli`?  
Check out: [Google Brotli Issue #782](https://github.com/google/brotli/issues/782)

#### ❓ "The owner of this website (discord.com) has banned your access based on your browser's signature..."  
This is likely due to your **user agent**. Try again with a different one: [StackOverflow](https://stackoverflow.com/a/24914742/14776493)

---

## 📝 Notes
With the rise of token logging, **always review the code before running it**. While many closed-source selfbots exist, **DiscNT remains open-source for full transparency**.  
Understanding DiscNT's code **not only helps with security but also enhances your knowledge of Discord's API**.

If you have questions after reading the documentation and previous issues, feel free to ask!

---
