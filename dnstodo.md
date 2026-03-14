# DNS & GitHub Pages Setup — cloudpathacademy.com

## Step 1 — Hover DNS Records

Log in to Hover and manage DNS for `cloudpathacademy.com`.

Delete any existing default/parking records for `@`, then add:

| Type  | Host | Value               |
|-------|------|---------------------|
| A     | @    | 185.199.108.153     |
| A     | @    | 185.199.109.153     |
| A     | @    | 185.199.110.153     |
| A     | @    | 185.199.111.153     |
| CNAME | www  | ka6wke.github.io    |

> The CNAME for `www` makes `www.cloudpathacademy.com` the canonical URL. GitHub Pages will redirect the apex to `www`.

## Step 2 — GitHub Pages Custom Domain

1. Go to [https://github.com/KA6WKE/ka6wke.github.io/settings/pages](https://github.com/KA6WKE/ka6wke.github.io/settings/pages)
2. Under **Custom domain**, enter `www.cloudpathacademy.com`
3. Click **Save**
4. Wait for DNS verification to complete (can take a few minutes)
5. Once verified, check **Enforce HTTPS**

## Step 3 — Deploy Updated Site

Merge the `s3-labs` branch into `main` to deploy the updated domain references:

```
git checkout main
git merge s3-labs
git push origin main
```

## Notes

- DNS propagation can take up to 24–48 hours, though usually much faster
- The `CNAME` file in the repo root contains `www.cloudpathacademy.com` — do not delete it
- `_config.yml` `url` is set to `https://www.cloudpathacademy.com`
