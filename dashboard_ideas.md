# Dashboard Modules: Detailed Elaboration

## Module A: The "Commuter Heartbeat" (Directional Flow)
**Goal:** Prove the "Pulse" of the city. A true "Commuter Hub" must breathe *in* during the morning and *out* during the evening.

### 1. The Data Logic (Python Preparation)
You need to create a new dataset (or aggregated view) specifically for this.
*   **Input:** Raw `traffic_data_cleaned.csv`
*   **Filter 1 (Time of Day):**
    *   **Morning Peak:** 06:00 – 09:00
    *   **Evening Peak:** 16:00 – 19:00
*   **Filter 2 (Direction):**
    *   Group by `direction`: `inbound` vs `outbound`.
*   **Aggregation:** Sum `vehicle_count` for these specific windows per Quarter per Year.

### 2. The Narrative (What it tells you)
*   **Morning Inbound > Morning Outbound:** This is a **Work Destination** (people coming to work).
*   **Morning Outbound > Morning Inbound:** This is a **Bedroom Community** (people leaving home for work).
*   **Balanced:** Mixed-use neighborhood.

### 3. Tableau Visualization Concept
*   **Visual:** Map with **Flow Arrows** or **Split Semi-Circles**.
    *   **Left Half of Circle:** Inbound Volume (Color scaled by intensity).
    *   **Right Half of Circle:** Outbound Volume.
*   **Interactivity:** A "Time of Day" toggle button (Morning Peak / Evening Peak).
    *   *Effect:* When the user clicks "Morning", the map illuminates the Inbound arteries. When they click "Evening", the flow reverses.

---

## Module B: The "Urban Evolution" (Interactive Growth Time-Lapse)
**Goal:** Show the *velocity* of change. Static maps show status; this shows momentum.

### 1. The Data Logic
You already have this in your `stress_index_tableau_ready.csv`, but you need to leverage the `year` column effectively.
*   **Metrics:** `traffic_growth_pct` (Y-axis), `population_growth_pct` (X-axis), `total_population` (Size).
*   **Dimension:** `Quarter`.
*   **Animator:** `Year`.

### 2. The Narrative (Two Key Stories)
*   **The "Explosion":** Watch a specific quarter (like Zurich North) suddenly shoot up in population growth while traffic lags (Infrastructure Gap).
*   **The "Traffic Calming":** Watch a center district (like Rathaus) where population stays steady but traffic growth suddenly drops negative (Policy Success).

### 3. Tableau Visualization Concept
I recommend two coupled views for this dashboard page:

#### View 1: The "Hans Rosling" Bubble Chart (The Race)
*   **X-Axis:** Population Growth %
*   **Y-Axis:** Traffic Growth %
*   **Bubbles:** Quarters (Sized by Total Population).
*   **Animation:** Use the **Pages Shelf** in Tableau with `Year`.
*   **Action:** Press "Play". The bubbles move around the quadrant.
    *   *Top-Left Quadrant:* **Traffic Surge** (Bad?)
    *   *Bottom-Right Quadrant:* **Densification** (Good?)

#### View 2: The Time-Lapse Map
*   **Visual:** Choropleth Map of Zurich.
*   **Color:** `Stress Index`.
*   **Animation:** Synced with the Bubble Chart.
*   **Effect:** As the years progress (2012 -> 2025), you watch the districts change color physically on the map. You see the "Red" (Stress) migrate from the center to the outskirts (or vice versa).

---

## Module C: The "Stress Index" Core (The Diagnostic View)
**Goal:** This is your primary diagnostic tool. It must instantly answer: "Where is the problem?" and "How bad is it?"

### 1. The Data Logic
*   **Metric:** `stress_index` (Traffic Growth % - Population Growth %).
*   **Categories:** `Commuter Hub` (>5), `Residential Zone` (<-5), `Balanced` (-5 to 5).
*   **Granularity:** Annual resolution per Quarter.

### 2. The Narrative
*   This module is about **categorization and magnitude**. It allows specific targeting for urban planners.
*   It simplifies the complex "Growth vs Growth" logic into a single traffic-light signal.

### 3. Tableau Visualization Concept
This should be the **Landing Page** of your dashboard.

#### Component 1: The Diverging Choropleth Map
*   **Color Palette:** Use a **Red-Grey-Blue diverging palette**.
    *   **Red (Positive > 5):** Commuter Hubs (Traffic Overload).
    *   **Blue (Negative < -5):** Residential Zones (Population Overload).
    *   **Grey/White (0):** Balanced.
*   **Tooltip (Critical):** specific "Math" breakdown on hover:
    *   *“Stress Index: +12.5”*
    *   *“Because: Traffic (+15%) - Population (+2.5%)”*
    *   This demystifies the index for the user.

#### Component 2: The "Top 5" Punishment/Ranking Chart
*   **Visual:** Horizontal Bar Chart.
*   **Content:** Top 5 "Most Stressed" Districts (Highest Positive Index) and Top 5 "Most Densifying" Districts (Lowest Negative Index).
*   **Why:** A map requires searching; a ranked list gives immediate answers. "Where should I send my team tomorrow? -> The top district on this list."

#### Component 3: The Annual Trend Line
*   **Visual:** Line Chart (below the map).
*   **Action:** When a user clicks a district on the map, this chart updates to show the *history* of stress for that specific district from 2012-2025.
*   **Insight:** "Is this district *always* biased towards traffic, or was it a sudden spike in 2018?"
