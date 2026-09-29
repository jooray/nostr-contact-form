# nostr-contact-form

A privacy-respecting contact form that uses Nostr relays to send encrypted direct messages without requiring a traditional backend server.

<!-- jooray-links:start -->
### More from me

**Related projects**

- [nostr-emanator](https://github.com/jooray/nostr-emanator): schedule Nostr posts, paired over NIP-46 and Amber
- [nostrautica](https://github.com/jooray/nostrautica): Nostr-native event organizer with end-to-end encrypted data
- [nsite-clay](https://github.com/jooray/nsite-clay): a self-editable site on Nostr, all in one HTML file
- [oracolo](https://github.com/jooray/oracolo): a Nostr blog in a single HTML file
- [anonmicroblog](https://github.com/jooray/anonmicroblog): anonymous microblogs on Nostr

**Full project showcase:** [Nostr Contact Form in my project showcase](https://juraj.bednar.io/showcase/#PUB-05), or [all my projects](https://juraj.bednar.io/showcase/).

I write about building things on [my blog](https://juraj.bednar.io/en/blog-en/). I also wrote a cypherpunk novel, [Tamers of Entropy](https://tamersofentropy.net/), and there is a [trailer](https://tamersofentropy.net/#trailer).
<!-- jooray-links:end -->

## Features

- **No backend required**: Uses Nostr's decentralized relay network
- **End-to-end encrypted**: Messages are encrypted using NIP-04
- **Privacy-first**: Generates ephemeral keypairs for each session
- **Configurable**: Recipient and relays can be configured via HTML meta tags
- **Spam prevention**: Optional NIP-13 proof of work to deter spam
- **Contact validation**: Optional sender contact field with validation for email, Nostr identities, or SimpleX contacts
- **Theme support**: Light, dark, and auto themes with grayscale color palette

## Quick Start

## Just use

You can edit the contact-selfcontained.html, which contains both HTML and JavaScript. Just edit the meta tags at the top of the file and you are good to go.

## Alternative: Full build

If you want to build from source, you can:

```bash
npm install
npx webpack
```

Then serve over https. If you use [my dotfiles](https://github.com/jooray/dotfiles), you can just run "server 8000" in this directory.

## Configuration

Configure the contact form by adding meta tags to `contact.html`:

### Recipient Public Key (Required)

```html
<meta name="nostr:recipient" content="YOUR_NOSTR_PUBLIC_KEY_HEX">
```

Replace `YOUR_NOSTR_PUBLIC_KEY_HEX` with the hex format of your Nostr public key (the recipient of the messages).

### Relays (Optional)

```html
<meta name="nostr:relays" content='["wss://relay1.example.com","wss://relay2.example.com"]'>
```

If not specified, the following default relays are used:
- wss://nos.lol
- wss://nostr.bitcoiner.social
- wss://relay.nostr.band
- wss://relay.damus.io
- wss://nostr.einundzwanzig.space
- wss://relay.nostrplebs.com

### Contact Field Requirement (Optional)

```html
<meta name="nostr:contact-optional" content="true">
```

Set to `"true"` to make the contact field optional, or `"false"` (or omit the tag) to make it required. Default is `false` (contact field is required).

### Proof of Work (Optional)

```html
<meta name="nostr:pow" content="20">
```

Sets the NIP-13 proof of work difficulty (number of leading zero bits required in the event ID). Set to `"0"` or omit the tag to disable proof of work. Default is `0` (no PoW).

**Recommended values:**
- `0`: No PoW (default)
- `16-20`: Light spam prevention (takes seconds)
- `21-24`: Moderate spam prevention (may take minutes)
- `25+`: Strong spam prevention (may take significant time)

The form will display mining progress including current difficulty achieved, number of attempts, and elapsed time.

### Theme (Optional)

```html
<meta name="nostr:theme" content="auto">
```

Sets the color theme for the contact form. Allowed values:
- `"auto"`: Automatically follow system dark/light theme preference (default)
- `"light"`: Always use light theme (white-ish background, dark text)
- `"dark"`: Always use dark theme (pitch black #000000 background, light text)

Users can toggle between themes using the icon in the top-right corner. The theme preference is saved in localStorage and persists across sessions. Both themes use a grayscale color palette for a clean, minimal appearance.

## How It Works

1. The web form generates a random ephemeral Nostr keypair for each session
2. User fills in a contact information field (optional or required based on configuration) and their message
3. The message (with contact info if provided) is encrypted using NIP-04
4. If proof of work is configured, the event is mined to meet the target difficulty (NIP-13)
5. An encrypted direct message (kind 4) is sent to the configured recipient via Nostr relays
6. The form displays a success or failure message after attempting to publish to the relays

## Contact Field Validation

The "Sender contact" field (optional or required based on configuration) accepts:
- Valid email addresses (e.g., `user@example.com`)
- Nostr identities starting with `npub` or `nprofile`
- SimpleX contact links starting with `https://`

## Receiving Messages

To receive messages from this contact form, you need:

1. A Nostr client that supports direct messages (kind 4)
2. The private key corresponding to the public key configured in the meta tag
3. Optionally, a bot like [nostr-ai-bot](https://github.com/jooray/nostr-ai-bot) to automatically process messages

## Goal

The goal of this project is to provide a contact form solution that:
- Respects user privacy
- Doesn't require traditional backend infrastructure
- Uses decentralized technology (Nostr)
- Can be easily embedded in static websites

## Use Cases

- Contact forms on static websites
- Anonymous feedback collection
- Privacy-focused communication channels
- Decentralized support systems

## License

Public domain

## Contributing

Contributions are welcome! Feel free to open issues or submit pull requests.
