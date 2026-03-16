# Project State: SpineBase Dynamic Explorer (v2.0)

## 1. Project Identity & Vision
* **Core Purpose**: SpineBase is a mobile-first, headless-CMS platform designed to centralize and standardize spine imaging measurements.
* **Mechanism**: It maps a GitHub repository folder structure into a professional clinical navigator for medical providers and students.

## 2. Technical Infrastructure
* **Repository**: `sshlyk/spinebase-public`.
* **Data Source**: GitHub REST API (for directory crawling) and GitHub Raw (for content/assets).
* **Initial Manifest**: `structure.json` defines the primary categories.
* **Scrolling Optimization**: Uses `overflow-y: auto` and `-webkit-overflow-scrolling: touch` on the `main` container to ensure fluid vertical navigation.
* **Global Assets**: 
    * Background: `assets/spine-background.png`.
    * Mission Image: `assets/about-us.png`.

## 3. UI/UX Architecture
### A. Persistent Dual-Action Header
* **SpineBase Banner (Left)**: Functions as a global reset button to return to the root level.
* **About Us (Right)**: Opens a static informational page featuring the mission statement by Philip Louie, MD.
* **Dynamic Breadcrumbs**: Located below the brand banner; displays the value proposition on the home screen and the navigation path during browsing.

### B. Navigation & Layout
* **Conditional Centering**: The `centered` class (Flexbox) is applied only to the root menu to center primary category buttons.
* **Scrolling Fix**: Item grids utilize `display: block` to prevent Flexbox from clipping overflowing content, allowing full access to all list items.
* **Vertical Menu Centering**: Category and Type buttons are 92% container width to maximize tap targets.
* **Proportional Scaling**: Background uses `background-size: contain` and `background-position: center`.
* **Interactive Feedback**: All clickable elements utilize `cursor: pointer` and active-state scaling (0.97).

### C. Content Display
* **Item Grid**: Displays cards with `thumbnail.png`. 
* **Card Design**: Thumbnails have `15px` top padding and cards have `flex-shrink: 0` to maintain dimensions during scroll.
* **Clinical Reading Mode**: When viewing a measurement document, the background image is removed (solid white) for maximum legibility.
* **Asset Normalization**: A JavaScript Regex engine automatically rewrites relative image paths (`src="images/..."`) within clinical docs to absolute GitHub Raw URLs.

## 4. State Management
* **StateStack**: An array-based navigation history that allows the "Back" button to traverse nested levels.
* **Scroll Reset**: The `render()` function resets `viewContainer.scrollTop = 0` on every state change to ensure new content starts at the top.
* **View Types**: 
    * `menu`: Vertical centered buttons.
    * `grid`: Top-aligned, scrollable item cards.
    * `content`: Full-screen document viewer.
    * `about`: Static informational view.

## 5. Folder Hierarchy Requirements
* **Leaf Folders**: Must contain an `index.html` (primary content) and a `thumbnail.png` (grid preview).
* **Images Folder**: Clinical images must be in a subfolder named `images/` relative to the `index.html`.
