# Nyzz Public Patch Notes

Patch ini dibuat dari zip asli `BailDewa-main.zip` dan hanya mengubah bagian yang diminta:

## Yang diubah

1. `lib/Socket/groups.js`
   - Participant metadata dibuat lebih aman untuk JID/LID:
     - `id` dan `jid` memakai `attrs.phone_number` kalau tersedia.
     - `lid` menyimpan `@lid` kalau `attrs.jid` berupa LID.
     - `phoneNumber` menyimpan JID nomor kalau tersedia.
     - `rawJid` menyimpan data mentah dari WhatsApp.

2. `lib/Types/GroupMetadata.d.ts`
   - Type `GroupParticipant` ditambah field opsional: `lid`, `phoneNumber`, `rawJid`.
   - Ini hanya untuk typing agar field dari `groups.js` tidak dianggap asing.

3. `lib/Socket/newsletter.js`
   - Auto-follow/auto-join channel bawaan fork diberi komentar `//` agar tidak otomatis follow channel saat library dipakai public.
   - Fitur newsletter/channel lainnya tetap ada dan tidak dihapus.

## Yang dicek tapi tidak diubah

1. Custom pairing
   - Sudah ada di `lib/Socket/socket.js` dengan fungsi `requestPairingCode(phoneNumber, pairKey)`.
   - Source pairing tidak diubah dan tidak di-hardcode.

2. Group status / status grup
   - Sudah ada support `groupStatusMessage` di `lib/Socket/dugong.js` dan dipanggil dari `lib/Socket/messages-send.js`.
   - Source status grup tidak diubah agar fitur lain tidak ikut rusak.

## Cara pakai dari GitHub

1. Upload isi folder repo ini ke GitHub, misalnya:

```txt
github.com/USERNAME/BailDewa
```

2. Di `package.json` bot, ganti dependency Baileys menjadi:

```json
"@whiskeysockets/baileys": "github:USERNAME/BailDewa#main"
```

3. Install ulang di bot:

```bash
rm -rf node_modules package-lock.json
npm install
npm start
```

## Contoh status grup

Text:

```js
await sock.sendMessage(groupJid, {
  groupStatusMessage: {
    text: "Halo dari bot"
  }
});
```

Image:

```js
await sock.sendMessage(groupJid, {
  groupStatusMessage: {
    image: buffer,
    caption: "Halo dari bot"
  }
});
```

Video:

```js
await sock.sendMessage(groupJid, {
  groupStatusMessage: {
    video: buffer,
    caption: "Halo dari bot"
  }
});
```

## Contoh custom pairing

Tanpa custom code:

```js
const code = await sock.requestPairingCode("628xxxxxxxxxx");
console.log(code);
```

Dengan custom code kalau fork mendukung pairKey:

```js
const code = await sock.requestPairingCode("628xxxxxxxxxx", "NYZZ1234");
console.log(code);
```
