# Project State: SpineBase Dynamic Explorer (v1.9)

## 1. Project Identity & Vision
SpineBase is a mobile-first, headless-CMS platform designed to centralize and standardize spine imaging measurements. It maps a GitHub repository folder structure into a professional clinical navigator for medical providers and students.

## 2. Technical Infrastructure
* **Repository:** `sshlyk/spinebase-public`
* **Data Source:** GitHub REST API (for directory crawling) and GitHub Raw (for content/assets).
* **Initial Manifest:** `structure.json` defines the primary categories.
* **Global Assets:** * Background: `assets/spine-background.png`
    * Mission Image: `assets/about-us.png`

## 3. UI/UX Architecture
### A. Persistent Dual-Action Header
* **SpineBase Banner (Left):** Functions as a global reset button to return to the root level.
* **About Us (Right):** Opens a static informational page featuring the mission statement by Philip Louie, MD.
* **Dynamic Breadcrumbs:** Located below the brand banner; displays the value proposition on the home screen and the navigation path (e.g., `Cervical > X-Ray`) during browsing.

### B. Navigation & Layout
* **Vertical Menu Centering:** Category and Type buttons are vertically centered and wider (92% container width) to maximize tap targets and visual balance.
* **Proportional Scaling:** The background image uses `background-size: contain` and `background-position: center` to ensure the spine diagram remains proportional and visible on all screen sizes.
* **Interactive Feedback:** All clickable elements (banners, buttons, cards) utilize `cursor: pointer` and active-state scaling (0.97).

### C. Content Display
* **Item Grid:** Displays cards with `thumbnail.png`. Thumbnails have `15px` top padding to prevent the image from touching the card edge.
* **Clinical Reading Mode:** When viewing a measurement document (`index.html`), the background image is removed (solid white) to ensure zero distraction and maximum legibility.
* **Asset Normalization:** A JavaScript Regex engine automatically rewrites relative image paths (`src="images/..."`) within clinical docs to absolute GitHub Raw URLs.

## 4. State Management
* **StateStack:** An array-based navigation history that allows the "Back" button to traverse nested levels and handles the "Reset to Home" logic.
* **View Types:** * `menu`: Vertical centered buttons.
    * `grid`: Top-aligned item cards.
    * `content`: Full-screen document viewer.
    * `about`: Static informational view.

## 5. Folder Hierarchy Requirements
* **Leaf Folders:** Must contain an `index.html` (primary content) and a `thumbnail.png` (grid preview).
* **Images Folder:** Clinical images must be in a subfolder named `images/` relative to the `index.html`.
