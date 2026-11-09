# Stealth Factory

A static landing page for https://stealthfactory.com with the supplied SF logo and “Make.”

## Preview

```sh
python3 -m http.server 8080 --bind 127.0.0.1
```

Open http://127.0.0.1:8080. The page has no dependencies or build step.

## Hosting

GitHub Pages publishes the root of `main` in `StealthFactory/landing`. `CNAME` declares `stealthfactory.com`. Future pushes to `main` publish the site automatically.

Add these Cloudflare DNS records, initially set to **DNS only** with automatic TTL so GitHub can verify the domain and provision its HTTPS certificate.

| Type | Name | Target |
| --- | --- | --- |
| A | @ | 185.199.108.153 |
| A | @ | 185.199.109.153 |
| A | @ | 185.199.110.153 |
| A | @ | 185.199.111.153 |

After GitHub Pages shows the certificate as ready, enable **Enforce HTTPS** in the repository's Pages settings. Set Cloudflare SSL/TLS to **Full (strict)** and switch these records to **Proxied**. Cloudflare's proxy is required for the max download Worker's path routes.

The landing repository serves the website. The max distribution Worker handles only `/max/download*` and `/max/appcast.xml*`, using its private R2 bucket. Deploy that Worker from the max source checkout after the proxied DNS records exist. The root `/` continues to use GitHub Pages. This page does not add a separate `/max` marketing page.

[GitHub Pages custom domain setup](https://docs.github.com/en/pages/configuring-a-custom-domain-for-your-github-pages-site/managing-a-custom-domain-for-your-github-pages-site)

[Cloudflare Worker routes](https://developers.cloudflare.com/workers/configuration/routing/routes/)
