<img src="./uxttxodh.png" style="width:6.51042in" /><img src="./s0bpmkyn.png" style="width:6.51042in" />

**Technical** **Specification:** **Seniors** **App** **(Educational**
**Prototype)**

**1.** **Core** **Objective**

Create a functional Web App for elderly users with persistent state
management via **Supabase**. The app eliminates technical friction,
focusing on high-visibility controls for home automation and safety.

**2.** **Technical** **Stack**

> • **Design:** Figma (High Contrast, Big Buttons).
>
> • **Frontend:** HTML5 / Tailwind CSS (Responsive). • **Database**
> **&** **Auth:** **Supabase**.
>
> • **Logic:** JavaScript (ES6+).

**3.** **Database** **Schema** **(Supabase)**

The app must connect to a Supabase project with a table named
seniors_home_state:

||
||
||
||
||
||
||
||
||
||

**4.** **Interaction** **Logic** **by** **Screen**

**Screen** **1:** **Home** **&** **Security** **(The** **Unlock**
**Flow)**

> • **Visual** **State:** Fetch door_locked from Supabase on load. •
> **Primary** **Action:** "UNLOCK DOOR" button.
>
> • **Security** **Trigger:**
>
> 1\. Clicking "UNLOCK" opens a **Full-screen** **PIN** **Pad**
> **overlay**. 2. The PIN Pad features 10 massive buttons (0-9).

<img src="./ex2swksz.png" style="width:6.51042in" /><img src="./s0eegckk.png" style="width:6.51042in" />

> 3\. **Logic:** If input sequence matches access_pin (Default:
> **0000**): ! Update Supabase: door_locked = false.
>
> ! Show "DOOR OPEN" visual for 5 seconds, then auto-relock or stay
> open.
>
> 4\. **Logic:** If input is wrong, vibrate/shake and clear input.

**Screen** **2:** **Lights** **Control** **(State** **Persistence)**

> • **Initialization:** On mount, subscribe to Supabase Realtime changes
> for the seniors_home_state table.
>
> • **Individual** **Toggles:**
>
> o Clicking a room card (e.g., "KITCHEN") toggles the boolean value in
> Supabase.
>
> o **UI** **Feedback:** If light_kitchen is true, the card background
> becomes **Bright** **Amber** and the icon changes to "Glowing".
>
> • **Bulk** **Actions:**
>
> o TURN ALL OFF: Set all light booleans to false in one Supabase query.
> o TURN ALL ON: Set all light booleans to true.

**Screen** **3:** **Emergency** **(Direct** **Action)**

> • **Requirement:** **ZERO** **SECURITY.** Do not ask for a PIN on this
> screen. • **Staff/Nurse** **Button:** Sends a timestamped row to a
> Supabase table
>
> emergency_calls.
>
> • **Family** **Button:** "Press and Hold" for 3 seconds to trigger a
> browser-level tel: link.

**5.** **UI/UX** **Global** **Constraints** **(Antigravity**
**Instructions)**

> • **Language:** English only.
>
> • **Contrast:** Background \#FFFDF5, Text \#1A1A1A. • **Button**
> **Minimums:**
>
> o Height: 90px for primary actions.
>
> o Font-size: 28px (Labels), 48px (Icons).
>
> • **Interactive** **States:** Use :active CSS states to scale down
> buttons slightly (0.98) to give the user tactile feedback that the
> button was pressed.

**6.** **Default** **Credentials** **for** **Students**

> • **Master** **PIN:** 0000
>
> • **Deployment** **Target:** Netlify (via GitHub).
