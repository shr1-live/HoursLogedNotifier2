# Hours Logged — web dashboard

An interactive dashboard for the same attendance data the desktop app records:
ring gauges, a weekday comparison chart, office-hours tracking against the 95%
mark, and WFH days with editable hours.

It is a static site with no backend, and the app sits at the root of this
repository, so any host detects it as a Vite project without configuration.

## Running locally

```bash
npm install
npm run dev
```

## Building

```bash
npm run build     # outputs to dist/
npm run preview   # serves the built output
```

## Deploying

Connect the repository and deploy the `main` branch. Nothing else to set:

- **Vercel** — `vercel.json` pins the Vite preset and carries the security
  headers and the single-page rewrite.
- **Netlify / Cloudflare Pages** — `public/_headers` and `public/_redirects`
  are copied into `dist/` at build time and read from there.

Build command `npm run build`, output directory `dist`.

The security headers and the SPA rule are duplicated between `vercel.json` and
`public/_headers` because no host reads both formats. Change one and change the
other, or a host ends up serving the site without a content-security policy.

## How it relates to the desktop app

The desktop app lives in a separate repository. The two share a data format
rather than a server:

| | Desktop (.NET) | Web (React) |
|---|---|---|
| Desktop notifications and reminders | yes | no |
| Runs in the background | yes | no |
| Parses the pasted shift block | yes | yes |
| Office-hours target and 95% mark | yes | yes |
| WFH days with editable hours | yes | yes |
| Charts and ring gauges | basic (WinForms) | full |
| Storage | `shifts.json` | browser localStorage |

**Use both together:** the desktop app keeps sending notifications, and
`Export shifts.json` / `Import` moves history between them. The exported file
is the same shape the desktop app reads, so it can be copied straight over the
desktop `shifts.json`.

## Notes

- Data lives in `localStorage`, so it is per-browser and per-device. Export if
  you need it elsewhere.
- Notifications deliberately stay in the desktop app: a web page cannot send
  reliable reminders when the tab is closed.
