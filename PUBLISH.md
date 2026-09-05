# Publish plan — zerokfest.com

Same pattern as **9kfest.com** (`bgnipp/9k`) and **2kfest.com** (`bgnipp/hilltop`): a public GitHub repo, GitHub Pages from `main` at `/`, custom apex domain.

## Why a new repo (not a folder on hilltop)

`2kfest.com/zerok/` can stay as a redirect, but an apex domain (`zerokfest.com`) cannot be served from a subdirectory of another Pages site. Each custom domain needs its own Pages source. 9k already proved this: it is its own repo, not a folder under hilltop.

## Target shape

| Piece | Value |
|---|---|
| Domain | `zerokfest.com` + `www.zerokfest.com` |
| Registrar | GoDaddy |
| Repo | `https://github.com/bgnipp/zerokfest` |
| Pages | `main` / (legacy static) |
| `CNAME` file | `zerokfest.com` |
| HTTPS | Enforce after DNS + cert (same as 9k) |
| `2kfest.com/zerok/` | Meta + JS redirect to `https://zerokfest.com/` |
| Source of truth | this repo, not `hilltop/zerok/` |

## GitHub (done from this machine)

1. Copy `hilltop/zerok/` to a sibling `zerokfest/` repo with the site at the root.
2. Point canonical / OG / JSON-LD URLs at `https://zerokfest.com/`.
3. Replace hilltop-relative links (`../ijhf/`, `../spring26/`, `../photos/`) with `2kfest.com` URLs or a local favicon.
4. `git init` → `gh repo create zerokfest --public` → push `main`.
5. Enable Pages on `main` `/` and set custom domain `zerokfest.com`.

Until GoDaddy DNS lands, the site is also at `https://bgnipp.github.io/zerokfest/`. GitHub will prefer the CNAME once DNS verifies.

## GoDaddy DNS (the part that needs your account)

There is no GoDaddy API key in this environment, so DNS cannot be written from the CLI. Records to set on **zerokfest.com** (DNS → Records). Delete GoDaddy's default parked/A/`Parked` records first so they do not fight GitHub.

| Type | Name | Value | TTL |
|---|---|---|---|
| A | `@` | `185.199.108.153` | 600 |
| A | `@` | `185.199.109.153` | 600 |
| A | `@` | `185.199.110.153` | 600 |
| A | `@` | `185.199.111.153` | 600 |
| CNAME | `www` | `bgnipp.github.io` | 600 |

Optional IPv6 (GitHub also publishes these):

| Type | Name | Value |
|---|---|---|
| AAAA | `@` | `2606:50c0:8000::153` |
| AAAA | `@` | `2606:50c0:8001::153` |
| AAAA | `@` | `2606:50c0:8002::153` |
| AAAA | `@` | `2606:50c0:8003::153` |

Do **not** leave GoDaddy's forwarding / "forward to www" / parking page on. That steals the apex from Pages.

After save:

```bash
dig zerokfest.com +short A
dig www.zerokfest.com +short CNAME
```

You want the four `185.199.*` A records and `www` → `bgnipp.github.io`.

Then in the repo: **Settings → Pages → Enforce HTTPS** (greyed out until the cert issues, usually 15–60 min after DNS is right). Same checkbox 9k uses.

## What stays on 2kfest.com

- `/zerok/` becomes a redirect so old federation links and `2kfest.com/zerok` keep working.
- Sister-fest + IJHF cards should link to `https://zerokfest.com` (not the folder).
- Gallery “2k crew” tab still hotlinks `https://2kfest.com/spring26/…`.

## Not doing in this pass

- Review.md design fixes
- Apps Script interest form
- 9kfest.com sister card (separate site)
- Waiting out full DNS worldwide (can take minutes to a few hours)
