# Tessera

Tessera lives entirely inside the browser using local storage on your device. It never transmits over the network, never uploads to any server, and never submits your content for processing by any external service. All `encryption` and `decryption` operations are performed locally in the browser using the `Web Crypto API`. For your enjoyment, you may save this HTML file and run locally on your own system or use it on this domain, the effect is the same. Because storage is local and offline, the security of the notes is equivalent to post-its in front of you on your desk.

Tessera is intended for easy use as a copy-paste base. Use `#` for non-clipboarded meta-content, and `!` to occlude your secrets. Secrets show as 8 circle bullets on screen but will enter your clipboard as needed.

When a PIN is set, the PIN is not stored directly. Instead, the PIN is used as input to a `key-derivation function` (`PBKDF2` with `SHA-256`) along with a randomly generated `salt`. This process produces a `cryptographic key` that cannot be feasibly reversed to recover the original PIN. The derived key is then used with `authenticated encryption` (`AES-GCM`) to encrypt the note data before it is written to local storage. The `encrypted data`, `initialization vector`, and `salt` are stored locally, but the PIN itself is never saved.

To read stored notes, the user must provide the correct PIN. The browser derives the same `encryption key` from the entered PIN and the stored `salt`, then attempts to `decrypt` the locally stored data. If the PIN is correct, `decryption` succeeds and the notes are reconstructed in memory. If the PIN is incorrect, `decryption` fails and no note content is revealed. At no point is the PIN transmitted or shared; it exists only temporarily in memory during the unlock operation.

After a PIN is set or an existing PIN is successfully loaded, the browser derives an `encryption key` from the PIN and immediately begins using that key to transparently protect all note data. Any change made while notes are in edit mode is automatically `re-encrypted` in memory and saved back to local storage at regular intervals without further user action. The `encrypted data` stored on disk is continuously kept in sync with the current contents of the notes, while the `decrypted` form exists only in memory for the duration of the session. If the page is closed or reloaded, only the `encrypted data` remains, and access to the notes again requires re-entering the correct PIN.

Because local storage is isolated per browser and per device, `encrypted` notes cannot be accessed from another machine, another browser, or another user account. Clearing browser data permanently removes the `encrypted` notes. There is an Import/Export mechanism which will let you store and retrieve a `JSON` payload under `encryption` which requires the same PIN.

To set a PIN, click the **Secure** button and enter a four-digit PIN when prompted.

To load notes, click the **Load** button and enter the previously set PIN.
