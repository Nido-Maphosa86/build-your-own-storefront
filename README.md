# Film Club

Film Club is an online store built using Shopify. It sells analog (film) photography products. The store is based on the Dawn theme and built using Shopify CLI 3.x with Git.

This document explains the decisions made before building the store.

---

## Store Idea

### Niche

The store sells **film photography supplies**, such as:

* Film rolls
* Developing chemicals
* Darkroom tools
* Used cameras and lenses
* Accessories (changing bags, reels, tanks, sleeves, etc.)

This niche is different from the example used in class.

The name **Film Club**:

* “Film” = what we sell
* “Club” = who it is for (people interested in film photography)

---

### Why this store is complex

1. **Film has many options**

   * ISO (160, 400, 800)
   * Format (35mm, 120, 4x5)
   * Packs (single, 5-pack, pro pack)

2. **Customers filter differently**
   They search by:

   * ISO
   * Format
   * Process (C-41, black & white, etc.)
   * Grain and quality

3. **Chemicals need correct matching**

   * Some chemicals only work with certain films
   * Requires structured data

4. **Used gear is unique**

   * Every item is different
   * Has condition, serial number, defects

5. **Photos matter**

   * Customers buy based on image results
   * Needs sample photos, not just descriptions

---

### Target Audience

* Main: Film photographers (age 22–40)

* They already own cameras and shoot often

* Some develop film at home

* Secondary: Beginners

* People who found an old camera and need guidance

---

## Pages to Build

### 1. Film Index

A page showing all film types with filters.

Includes:

* Film name
* Brand
* ISO
* Format
* Sample images

---

### 2. Develop At Home

A guide to help users develop film.

Shows:

* Which chemical to use
* Time, temperature, steps
* Equipment needed

---

### 3. The Used Counter

Page for second-hand cameras and lenses.

Shows:

* Condition (Mint, Good, etc.)
* Test results
* Defects
* Warranty

---

## Development Setup

* Store Name: Film Club
* Store URL: https://film-club-rsyvjo2d.myshopify.com
* Shopify CLI: Version 3.x
* Theme: Dawn
* GitHub Repo: https://github.com/Nido-Maphosa86/build-your-own-storefront

Hot reload was tested and working correctly.

---

## Git vs Theme Editor

### Using CLI (Recommended)

* Work locally
* Use Git commits
* Safe and controlled
* Can review changes

### Using Theme Editor

* Changes go directly to GitHub
* No review step
* Risky for live stores

---

### Key Points

1. Always pull before pushing (to avoid conflicts)
2. Use CLI for code (Liquid, CSS, JS)
3. Use Theme Editor for layout/settings
4. CLI is safer for production

---

## VS Code Setup

* Installed Shopify Liquid extension
* Enabled format on save (only for Liquid files)

This avoids breaking other files like CSS or JS.

---

## Checklist

✔ Niche selected
✔ Complexity explained
✔ Audience defined
✔ 3 pages planned
✔ Store created
✔ GitHub connected
✔ CLI working
✔ Hot reload tested

---

This project is ready for development.
