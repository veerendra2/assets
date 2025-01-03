# Assets

🎨 A curated collection of 🖼️ background wallpapers, 🌟 icons and 🔲 grafana dashboards.

## Example

You can keep the assets locally on the server by cloning them into a Docker volume while deploying the homepage.

```yaml
volumes:
  homepage_assets:
    name: homepage_assets

services:
  homepage_assets:
    image: alpine/git:latest
    container_name: homepage_assets
    entrypoint:
      - sh
      - -c
      - |
        rm -rf /app/* /app/.* &&
        git clone --depth 1 https://github.com/veerendra2/assets.git /app
    volumes:
      - homepage_assets:/app
  homepage:
    image: ghcr.io/gethomepage/homepage:latest
    hostname: homepage
    container_name: homepage
    restart: unless-stopped
    depends_on:
      homepage_assets:
        condition: service_completed_successfully
    volumes:
      - homepage_assets:/app/public
```

Assets are mounted into homepage

```bash
$ docker exec -it  homepage ls public
README.md  icons      images
```
