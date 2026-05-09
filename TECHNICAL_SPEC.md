# Seniors App - Technical Design System

## 1. Design Tokens (From Figma Styles)

### Colors
The design uses a high-contrast palette tailored for visibility and clear affordances.

*   **Primary (Brand & Action):** `#002C98` (Deep Blue), `#1E3A8A`, `#1A43BF`
*   **Secondary/Active (Buttons):** `#FFBF00` (High-visibility Gold/Yellow)
*   **On-Secondary (Text on Yellow):** `#6D5000` (Dark Brown/Gold for contrast)
*   **Background (App & Cards):** `#F8F9FA` (Main App BG), `#FFFFFF` (Cards), `#F3F4F5`, `#E1E3E4` (Inactive elements)
*   **Emergency / Critical Action:** `#002C98` (Note: The emergency button uses the primary blue with white text `#FFFFFF`)
*   **Text (Dark):** `#191C1D`, `#001453`, `#444654` (Subtitles)

### Typography
The application strictly uses the **Lexend** typeface to ensure high legibility.

*   **Hero / Large Status Headers:** `56px` (e.g., "Your Home is Secure")
*   **Card Headers:** `36px` (Status Card), `30px` (Lights Cards)
*   **Action Buttons (Primary):** `30px` (e.g., "Tap to Unlock")
*   **PIN Pad Numbers:** `40px`
*   **Body / Subtitles:** `18px`, `20px`

---

## 2. Functional Logic (Bridge to Supabase)

### Authentication
The application does not use traditional username/password authentication to reduce friction.
*   **PIN Logic:** The app uses a default PIN of **`0000`**.
*   **Supabase Storage:** The access PIN should be stored in the `seniors_home_state` table (or similar config table) and validated client-side or via a Supabase Edge Function to prevent unauthorized state changes.

### State Management
The "Lights" section dynamically controls the home environment.
*   **Data Mapping:** Each light card (e.g., Living Room, Kitchen, Bedroom, Bathroom) maps to a corresponding boolean column in the `seniors_home_state` table (e.g., `light_living_room`, `light_kitchen`).
*   **Realtime Updates:** The application must subscribe to Supabase Realtime to update the UI (toggle states, background colors) instantly when a value changes in the database.

### Door Logic
*   **Unlock Flow:** The "Home Page" displays the current lock status. Clicking the primary gradient button ("Tap to Unlock") transitions the user to the "Security Pass Code" overlay/screen.
*   **Validation:** Upon entering the 4-digit PIN, if it matches the stored Supabase PIN, the `door_locked` boolean is set to `false`. If incorrect, the input is cleared (with a visual shake/vibrate feedback).

---

## 3. Detailed Screen Specs

### Home Page / Lock Status
*   **Layout:** Vertical stacking with a top App Bar, a Hero Status Section ("Your Home is Secure"), an Asymmetric Status Card, and a Bottom Navigation Bar.
*   **Primary Components:**
    *   **Status Card:** `440px` height. Displays an icon and the current door status ("Your door is locked").
    *   **Unlock Button:** Gradient background (`#795900` to `#FFBF00`), minimum `120px` height, large `30px` text.
*   **Accessibility Check:** Button height is `120px` (>= 80px). Font size is `30px` (>= 24pt).

### Security Pass Code
*   **Layout:** Focused transactional screen. Branding anchor at the top, a 4-dot PIN display area, and a 4x3 grid for the numpad.
*   **Primary Components:**
    *   **PIN Pad Buttons:** `110px` x `110px` circular buttons. High contrast Yellow (`#FFBF00`) with Dark text (`#6D5000`).
    *   **Emergency Button:** Positioned at the bottom, `80px` height, full width (`342px`), Blue (`#002C98`) with White text (`24px`).
*   **Accessibility Check:** Numpad buttons are `110px` (>= 80px) with `40px` font (>= 24pt). The Emergency button is `80px` high with `24px` font. High contrast ratios are maintained.

### Lights Section
*   **Layout:** Grid layout for individual room controls, followed by a Master Actions section and a decorative mood image.
*   **Primary Components:**
    *   **Active Light Card (e.g., Living Room):** `220px` height. Yellow background (`#FFBF00`) with an Oversized Toggle Switch (`96px` x `48px`).
    *   **Inactive Light Card:** Grey background (`#E1E3E4`) with a subdued toggle.
    *   **Master Actions:** "Turn All On" (Blue gradient, `96px` height) and "Turn All Off" (Outline style, `96px` height).
*   **Accessibility Check:** Cards provide large touch targets (`220px` height). Master action buttons are `96px` high. Fonts are `30px` and `24px` (>= 24pt).

---

## 4. Technical Roadmap

This roadmap outlines the steps to connect the Figma design to Project IDX and Supabase:

1.  **Project Initialization (Project IDX):**
    *   Initialize a new Vite project (HTML/JS or preferred framework).
    *   Install and configure Tailwind CSS.
    *   Configure `tailwind.config.js` with the extracted Design Tokens (Colors and Lexend font family).

2.  **Supabase Integration:**
    *   Set up a Supabase project and create the `seniors_home_state` table with boolean columns for lights (`light_kitchen`, etc.) and the door (`door_locked`), plus `access_pin`.
    *   Install `@supabase/supabase-js` and initialize the client.
    *   Implement data fetching and Realtime subscriptions on application mount.

3.  **Component Development:**
    *   Build the reusable UI components (Oversized Toggle, 110px PIN Pad Buttons, Bottom Navigation) using Tailwind classes matching the specs.
    *   Ensure all buttons use the `:active` state for tactile feedback (e.g., `active:scale-95`).

4.  **Screen Assembly & Routing:**
    *   Assemble the Home, PIN Pad, and Lights screens.
    *   Implement simple DOM manipulation (or framework routing) to transition between the Home and PIN Pad screens without full page reloads.

5.  **Logic Implementation:**
    *   Wire the Numpad buttons to update a local state and validate against the Supabase `access_pin`.
    *   Bind the Light Card toggles and Master Action buttons to execute Supabase `UPDATE` queries.
    *   Wire the Emergency button to insert a row into an `emergency_calls` table or trigger the `tel:` protocol depending on requirements.
