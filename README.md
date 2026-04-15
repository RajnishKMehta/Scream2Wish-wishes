# Scream2Wish-wishes

Public JSON store for all wishes submitted through the **[Scream2Wish](https://github.com/RajnishKMehta/Scream2Wish)** Android app.

Every time someone screams at their phone and submits a wish, it ends up here permanently, publicly, for everyone to read.

---

## How It Works

```
Android App
    │  POST /  (wish + note + name)
    ▼
Cloudflare Worker  (validates API key)
    │  repository_dispatch event
    ▼
GitHub Actions  (wish-handler.yml)
    │  generates unique ID, writes JSON, updates index
    ▼
This repo  ← you are here
    │
    ▼
Website reads it publicly
```

The app never writes here directly. The Cloudflare Worker proxies the request, and GitHub Actions does the actual file writing.

---

## Data Format

### `wishes/index.json`

An array of all wish IDs in chronological order (oldest first):

```json
["7jlje7", "1pslr1", ...]
```

### `wishes/{id}.json`

One file per wish:

```json
{
  "wish": "I wish for infinite wishes.",
  "note": "Nothing is useless in this world except YOU👀",
  "from": "Rajnish Mehta",
  "at": 1776057109644
}
```

| Field | Type | Description |
|-------|------|-------------|
| `wish` | `string` | The wish they typed after breaking the lamp |
| `note` | `string` | The note left for a random stranger |
| `from` | `string` | Name the user entered on the login screen |
| `at` | `number` | Unix timestamp in milliseconds when the wish was submitted |

---

## Reading the Data

Everything is plain public JSON (no auth required):

```
https://raw.githubusercontent.com/RajnishKMehta/Scream2Wish-wishes/main/wishes/index.json
https://raw.githubusercontent.com/RajnishKMehta/Scream2Wish-wishes/main/wishes/{id}.json
```

---

## Related

| Repo | Description |
|------|-------------|
| [Scream2Wish](https://github.com/RajnishKMehta/Scream2Wish) | Android app + Cloudflare Worker + website |
| [Scream2Wish-wishes](https://github.com/RajnishKMehta/Scream2Wish-wishes) | This repo |
