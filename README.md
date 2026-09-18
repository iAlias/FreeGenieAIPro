# FreeGenie AI Pro

**AI-generated content, images and SEO, built into WordPress.**

[![Platform](https://img.shields.io/badge/platform-WordPress-21759b?logo=wordpress&logoColor=white)](https://wordpress.org/)
[![PHP](https://img.shields.io/badge/PHP-7.4%2B-777bb4?logo=php&logoColor=white)](https://www.php.net/)
[![Version](https://img.shields.io/badge/version-1.4.2-orange)](freegenie-ai-pro.php)
[![License](https://img.shields.io/badge/license-MIT-green)](LICENSE)

🇮🇹 [Leggi in italiano](README.it.md)

FreeGenie AI Pro writes complete articles from a prompt, generates their
featured images, and optimizes their SEO meta tags — directly inside the
WordPress admin. You can publish by hand from the post editor, or let the
plugin publish on its own, on a schedule you control.

---

## Contents

- [Features](#features)
- [Supported providers](#supported-providers)
- [Installation](#installation)
- [Configuration](#configuration)
- [Usage](#usage)
- [How the automatic publisher works](#how-the-automatic-publisher-works)
- [SEO integration](#seo-integration)
- [Debug log](#debug-log)
- [Requirements](#requirements)
- [Project structure](#project-structure)
- [License](#license)

---

## Features

- ✍️ **Full articles from a prompt** — a compelling title plus complete body
  text, generated in one call from the post editor.
- 🖼️ **Image generation** across several providers, with a configurable
  priority order.
- 🔁 **Regeneration** — regenerate a single article or just its featured
  image without starting over.
- 🔎 **SEO** — meta description and title filters that integrate with
  **Yoast SEO** and **Rank Math**.
- 🗓️ **Scheduling** — choose which weekdays to publish on and how many
  articles per day; publication runs automatically through WP-Cron.
- 🧩 **Editor metabox** — generate article text and images directly from the
  post screen, with an optional separate image prompt.
- 🖼️ **Bulk image generator** — regenerate the featured images of every post
  published in a chosen date range, from the settings screen.
- 📋 **Debug log** viewable and clearable from the settings screen.

---

## Supported providers

| Scope | Providers |
|---|---|
| **Text** | OpenAI · Cohere · DeepAI · Hugging Face |
| **Images** | Pexels · Pixabay · Pollinations · Hugging Face · OpenAI |

The image provider priority order is configurable, and providers are tried
in turn until one succeeds (default order: Pexels → Pixabay → Pollinations →
Hugging Face → OpenAI → Cohere → DeepAI). Unsplash is also supported as an
image source when a key is configured.

---

## Installation

1. Copy the plugin folder into `wp-content/plugins/` (or upload the zip from
   **Plugins → Add New → Upload Plugin**).
2. Activate **FreeGenie AI Pro**.
3. Go to **FreeGenie AI Pro** in the admin menu and enter your API keys.

---

## Configuration

From the **FreeGenie AI Pro** settings screen you can set:

| Setting | Description |
|---|---|
| **API keys** | OpenAI, Cohere, DeepAI, Hugging Face, Unsplash, Pexels, Pixabay |
| **Publishing days** | Which weekdays to publish on |
| **Articles per day** | How many pieces of content to generate each day (0–24) |
| **Image provider order** | Drag to reorder which provider is tried first |

At least one text-provider key and one image-provider key are required for
generation to succeed; unconfigured providers are simply skipped in favor of
the next one in the priority order.

---

## Usage

- **Manual** — open a post, use the metabox to generate the article and its
  image, then regenerate individual parts if needed.
- **Automatic** — set publishing days and a daily quantity, and let WP-Cron
  take care of the rest.
- **Bulk images** — from the settings screen, pick a date range and
  regenerate the featured image of every published post in it.

---

## How the automatic publisher works

The scheduler (`includes/class-scheduler.php`) hooks an hourly WP-Cron event
(`fgp_hourly`). Each run it checks whether today is one of the configured
weekdays and whether today's publication count is still below the configured
per-day limit; if so, it:

1. Picks a topic at random from a built-in pool of categories (Technology,
   Artificial Intelligence, Smart Home, Blockchain, Mobile, Mobility,
   Science & Research).
2. Creates a draft post and assigns it to the matching category.
3. Generates the full article through `FreeGenie_AI_Pro_Generator`.
4. Publishes the post and increments a daily transient counter.

Changing the schedule settings resets the scheduled event automatically.

---

## SEO integration

`includes/class-seo.php` hooks into the SEO plugins already active on the
site:

- Fills `wpseo_metadesc` (Yoast SEO) and `rank_math/frontend/description`
  (Rank Math) with the first 155 characters of the post content when no meta
  description has been set manually.
- Falls back to the post title for `wpseo_title` when no SEO title has been
  set manually.

Existing SEO plugin values always take priority — FreeGenie only fills the
gaps.

---

## Debug log

The settings screen includes a **Debug Log** panel that reads
`wp-content/fgp-debug.log` for troubleshooting failed generations, and a
**Clear Log** button (behind a nonce-protected form) to empty it.

---

## Requirements

- WordPress
- PHP 7.4 or later
- At least one API key for a text provider and one for an image provider
- WP-Cron enabled (default in WordPress) for automatic publishing

---

## Project structure

```
freegenie-ai-pro.php        Plugin bootstrap: admin menu, settings, metabox, AJAX handlers
includes/
  class-generator.php       Calls the text/image providers and assembles the article
  class-scheduler.php       WP-Cron based auto-publisher
  class-seo.php             Yoast SEO / Rank Math meta integration
admin.js                    Admin screen behaviour (drag-to-reorder, bulk generator)
admin-styles.css            Admin screen styling
```

---

## License

MIT — see the [LICENSE](LICENSE) file for details.
