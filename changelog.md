# Changelog

All notable changes to this project will be documented in this file.

## [0.2.6] - 2025-06-07

### Fixed
-   **On-Page Link Catching:** Corrected a bug in `content_script.js` where settings for `catchfrompage` and `linksfoundindicator` were being compared to the string `"true"` instead of the boolean `true`, which prevented the feature from activating correctly.
-   **Tracker Rule Editing:** Fixed an issue in `options.js` where clearing the "Assign Label" or "Assign Directory" field for a Tracker URL Rule would not save the change. The empty value is now correctly saved, allowing users to remove a label or directory from a rule.

## [0.2.5] - 2025-06-06

### Fixed
-   **qBittorrent Directory Assignment:** Resolved `ReferenceError: downloadDir is not defined` in `qbittorrent_handler.js` and ensured the `savePath` parameter is correctly sent to qBittorrent's API for torrent additions.

### Changed
-   **Client-Specific UI Refinements (Options Page):**
    -   Dynamically show/hide Username and Password fields based on `clientType` (e.g., hidden for Kodi Elementum).
    -   Adjusted labels and placeholders for Server URL and Username fields based on `clientType` to provide more relevant guidance (e.g., "Username (optional for WebUI)" for Deluge, specific URL placeholders for uTorrent, Transmission, Synology/QNAP).

## [0.2.4] - 2025-06-06 
### Added
-   **Sound Notifications:**
    -   Implemented success/failure sound notifications for torrent additions.
    -   Uses the Offscreen API for reliable audio playback from the service worker.
    -   Resolved CSP issues by using an external script (`offscreen_audio.js`) for the offscreen document (`offscreen_audio.html`).
    -   Added an option in the settings page to enable/disable sound notifications.
    -   Audio files (`success.mp3`, `failure.mp3`) are expected in an `audio/` directory.
-   **Tracker URL-Based Label/Directory Assignment:**
    -   New feature to automatically assign labels and/or download directories based on tracker announce URLs found within `.torrent` files.
    -   Added a new section in the Options page to create, manage (edit/delete), and list these rules.
    -   Each rule consists of a "Tracker URL Pattern" (substring match), an optional "Assign Label", and an optional "Assign Directory".
    -   Logic implemented in `background.js` to:
        -   Refactor rule application into a helper function `applyTrackerRulesLogic`.
        -   For magnet links, extract tracker URLs from `&tr=` parameters (URI decoded) and pass to `applyTrackerRulesLogic`.
        -   For `.torrent` files, parse content (using `bencode` library), extract `announce` and `announce-list` URLs (using `TextDecoder`), and pass to `applyTrackerRulesLogic`.
        -   The helper function matches these URLs against configured `trackerUrlRules`.
        -   If a rule matches, the torrent's category (label) and/or download directory are overridden. The first matching rule applies.
    -   The `trackerUrlRules` are included in the settings export/import functionality.
    -   Success notifications now indicate if a tracker rule was applied.

### Changed
-   **Exported Settings Filename:** Corrected the default filename for exported settings from `RemoteTorrentAdder_settings.json` to `Add Remote Torrent_settings.json` to align with the new project name.
-   **qBittorrent API v5.1.0+ Compatibility:**
    -   Added `Referer` and `Origin` headers to all API requests.
    -   Changed `Content-Type` to `application/x-www-form-urlencoded` for login requests.
    -   Corrected the `getApiUrl` helper function to prevent double slashes.
    -   Updated the `testConnection` error message for authentication failures to suggest checking qBittorrent's "CSRF Protection" setting.
-   **Notification Icons:** Switched from `.svg` to `.png` (`icons/icon-48x48.png`) for potentially better reliability in `chrome.notifications.create` calls.
-   **Bencode Library:** Ensured consistent use of the `bencode` npm package for parsing torrent file metadata in `background.js` (for tracker rules) and `confirmAdd.js` (for file list).

### Fixed
-   **CSP for Theme Script:** Resolved Content Security Policy violations by moving the inline dark mode script to an external `js/theme.js` file.
-   **Sound Notification Errors:** Iteratively fixed "Audio is not defined" and "Receiving end does not exist" errors by implementing and refining the Offscreen API usage with a ready-handshake mechanism.

## [0.2.5] - 2025-06-06

### Fixed
-   **qBittorrent Directory Assignment:** Resolved `ReferenceError: downloadDir is not defined` in `qbittorrent_handler.js` and ensured the `savePath` parameter is correctly sent to qBittorrent's API for torrent additions.

### Changed
-   **Client-Specific UI Refinements (Options Page):**
    -   Dynamically show/hide Username and Password fields based on `clientType` (e.g., hidden for Kodi Elementum).
    -   Adjusted labels and placeholders for Server URL and Username fields based on `clientType` to provide more relevant guidance (e.g., "Username (optional for WebUI)" for Deluge, specific URL placeholders for uTorrent, Transmission, Synology/QNAP).

---
*(Previous versions/changes before 0.2.3 would be documented here if available)*
