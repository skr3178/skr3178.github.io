# skr3178.github.io

Personal blog and portfolio site built with Jekyll.

## Local Development

**Prerequisites:** Install Jekyll ([guide](https://jekyllrb.com/docs/installation/))

```bash
# Start local server (site loads at http://127.0.0.1:4000)
jekyll serve --watch

# Build the site without serving
jekyll build
```

## Creating a Blog Post

1. Create a new `.md` file in `_posts/` named `YYYY-MM-DD-title.md`
2. Add front matter:
   ```yaml
   ---
   layout: post
   title: "Your Post Title"
   date: YYYY-MM-DD
   published: true   # set to false to hide
   ---
   ```
3. Add images to `assets/posts/<post-name>/` and reference them as:
   ```
   ![alt text](/assets/posts/<post-name>/image.png)
   ```
4. Push to `main` — GitHub Pages auto-deploys.

## Deploy

Pushing to `main` automatically triggers GitHub Pages deployment. No manual build needed.

---

Based on [Jon Barron's academic site template](https://jonbarron.info/).