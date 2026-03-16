# Project Plan: SpineBase Dynamic Explorer (v2.2)

## 1. Core Architecture
A single-page, mobile-first web application acting as a dynamic browser for a GitHub-hosted spine database. It uses a "Headless CMS" approach where the repository folder structure defines the app's content, allowing for zero-code updates via GitHub.

## 2. Data Sources & API Strategy
The app interacts with the `sshlyk/spinebase-public` repository via three distinct entry points:

* **Manifest (Static):** - URL: `https://raw.githubusercontent.com/sshlyk/spinebase-public/refs/heads/main/structure.json`
    - Role: Defines the initial hierarchy (Category > Modality) and provides the relative `path` for each section.
* **Directory Discovery (GitHub API):** - Base: `https://api.github.com/repos/sshlyk/spinebase-public/contents`
    - Role: Scans the directory provided by the manifest to find sub-folders. Each sub-folder is treated as a clinical "Item."
* **Raw Content (Assets):**
    - Base: `https://raw.githubusercontent.com/sshlyk/spinebase-public/main/`
    - Role: Fetches the final `index.html`, `thumbnail.png`, and `images/` assets.

## 3. UI/UX Selection Flow & Scrolling Logic
Designed for clinical environments and one-handed mobile use:

* **State 0: Category/Modality Menus**
    - Rendered as large, high-tap-target buttons (92% width) centered vertically using Flexbox.
* **State 1: Item Selection (Vertical Scroll)**
    - **Layout:** One item per row using large image cards.
    - **Scrolling Fix:** Uses `display: block` on the main container (removing Flexbox centering) to ensure the list is fully scrollable and not clipped at the top/bottom on mobile.
    - **Thumbnails:** Large cards (280px height) with `15px` top padding, using `object-fit: contain` to preserve clinical image integrity.
* **State 2: Content Reader**
    - **Clinical Mode:** Removes the app background for a clean white workspace to maximize legibility.

## 4. Content Processing Engine (The "Renderer")
When an item is selected, the following logic is executed:

1.  **Fetch:** Downloads the `index.html` from the Raw Content URL.
2.  **Dynamic Asset Relocation (Regex):** - Scans content for relative image links: `src="images/..."`.
    - Rewrites them to absolute GitHub Raw URLs to ensure images render correctly within the host page.
3.  **State Synchronization:** - Resets scroll position to `window.scrollTo(0,0)` on every view change.
    - Toggles background visibility based on view type (Menu/Grid vs. Content).

## 5. Technical Specifications
* **Viewport Management:** Uses `100dvh` (Dynamic Viewport Height) to ensure the UI remains responsive whether the mobile browser's address bar is expanded or collapsed.
* **iOS Safari Optimization:** The `body` element is **not** `fixed`. This allows Safari’s search bar to naturally minimize/hide when the user scrolls down through item lists or clinical documents.
* **History & Navigation:** - **Native Back Button:** Integrated with the **History API** (`pushState` and `popstate`). The hardware/browser back button and the UI "Back" button stay perfectly in sync.
    - **StateStack:** An internal array manages the deep-navigation levels (Category > Type > Item).
* **Breadcrumbs:** Dynamic text that replaces the "Welcome" tagline with the current navigation path (e.g., *Cervical > X-Ray*).

## 6. Folder Hierarchy Requirements
To maintain the "Headless" automation, the repository must follow this structure:
* **Leaf Folders:** Each measurement must contain an `index.html` and a `thumbnail.png`.
* **Assets:** All clinical images within the document must reside in a subfolder named `images/` relative to the `index.html`.
* **Global Assets:** App background and "About Us" images reside in a root-level `assets/` folder.

## 7. Development Milestones
1.  **Initialize Shell:** Flexbox layout with `100dvh` and History API listener.
2.  **Recursive Menus:** Map `structure.json` to centered button lists.
3.  **Vertical Grid:** Implement block-flow container for `thumbnail.png` cards to fix scrolling.
4.  **Content Engine:** Regex-based path rewriting and clinical reading mode toggle.
5.  **Browser Sync:** Finalize `onpopstate` logic to handle hardware back-button navigation.
