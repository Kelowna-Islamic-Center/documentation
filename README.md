# Kelowna Islamic Center Documentation

This repository contains the full documentation for the Kelowna Islamic Center (KIC) ecosystem, including:

- **Server-Side Cloud Functions** – for prayer reminders, announcements, and notifications.  
- **Mobile App** – end-user app for prayer alerts and admin tools.  
- **Kiosk App** – display app deployed on Masjid kiosks to show prayer times and announcements.

The documentation is built with [MkDocs](https://www.mkdocs.org/) and the [Material for MkDocs](https://squidfunk.github.io/mkdocs-material/) theme.

---

## Getting Started

Before installing, ensure you have Python 3 with pip installed.

Start by installing the **MkDocs-Material** package:

```bash
pip install mkdocs-material
````

Once installed, use the MkDocs CLI to work with the documentation locally:

* `mkdocs serve` – Start a live-reloading local server.
* `mkdocs build` – Build the documentation site into a deployable HTML site.
* `mkdocs -h` – Show help message and exit.

---

## Project Layout

```text
mkdocs.yml        # Configuration file for MkDocs
docs/             # MkDocs generated site (deployed on GitHub Pages)
content/          # Markdown documentation
```

---

## Contributing

1. Fork the repository.
2. Create a new branch for your changes.
3. Add or edit Markdown documentation in the appropriate folder.
4. Run `mkdocs serve` locally to preview your changes.
5. Submit a pull request for review.

---

## Deployment

The documentation site is hosted on **GitHub Pages**.
Simply build the site for deployment:

```bash
mkdocs build
```

and commit your changes to the main branch or make a pull request to the main branch. GitHub Pages should automatically deploy the site.

---

## License

GPL-v3
