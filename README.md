# windborne-server

A tiny static site served from my own hardware and exposed to the internet with a tunnel.

## Challenge

Host a web page directly from your personal machine and make it publicly accessible without touching your home router/firewall.

Success criteria:

- Serve this folder (at minimum `index.html` and `computer.JPG`) from your machine.
- Show a photo of the hardware hosting the page and a brief explanation of the setup.
- Expose the local server to the public internet using a tunneling service (ngrok).
- Produce a public URL that others can visit to see the page.

## What's in this folder

- `index.html` — The landing page describing the setup and how it’s exposed
- `computer.JPG` — Photo of the device hosting the site

## Prerequisites

- macOS with zsh (commands below assume this)
- One simple static server (pick one):
  - Python 3 built-in HTTP server
  - Node-based static server via `npx serve`
- ngrok (v3) installed and on your PATH

## Run locally

Option A — Python 3 built-in server:

```bash
cd server
python3 -m http.server 8080
```

Option B — Node static server (`serve`):

```bash
cd server
npx serve -l 8080 .
```

Then open http://localhost:8080 to verify the site loads.

## Expose to the internet (ngrok)

In a separate terminal:

```bash
ngrok http 8080
```

Copy the forwarding URL shown (e.g., `https://<random>.ngrok-free.app`) and share it. Keep both terminals running.

If you have a reserved ngrok domain and have authenticated (`ngrok config add-authtoken <token>`), you can request it explicitly:

```bash
ngrok http --domain=<your-subdomain>.ngrok-free.app 8080
```

Note: Free plans generally do not allow arbitrary custom domains; omit the domain flag unless you’ve reserved one in the ngrok dashboard.

## Troubleshooting

- ngrok exits with code 1 or errors when using a custom URL:
  - Do not include a path in the target (avoid `8080/server`); use just the port: `ngrok http 8080`.
  - The `--domain`/`--url` option requires a domain you own/reserved in ngrok; otherwise, omit it and use the random URL provided.
- Public URL not loading:
  - Ensure the local server is running on port 8080 and accessible at http://localhost:8080.
  - Keep the ngrok terminal open; closing it will drop the tunnel.
  - Check local firewall settings if connections are blocked.

## How it works

- `index.html` is served locally from your machine.
- ngrok creates a secure tunnel and forwards a public URL to `http://localhost:8080`, so anyone with the link can view your page.

## Notes

- Free ngrok tunnels are ephemeral; the URL changes every run.
- Treat the public URL as sensitive—anyone with the link can access the page.
