# The Russell Archive: A Data-Driven Visual Novel

![The Russell Archive Cover](The-Russell-Archive.png) 

![Ren'Py](https://img.shields.io/badge/Engine-Ren'Py-ff69b4)
![Python](https://img.shields.io/badge/Code-Python%20%7C%20SQL-blue)
![Status](https://img.shields.io/badge/Compliance-100%25-green)

**Course:** Information Structures and Implications (Professor Adalberto Simeone)

**Author:** YIN Renlong 

## Play Online (Web Version)

**[Launch in your browser: yin-renlong.github.io/RussellArchive-web/](https://yin-renlong.github.io/RussellArchive-web/)**

---

## Important Note: Official Submission

This GitHub repository is a **supplementary Web Build** created to allow immediate review in a browser. 
Per the course rules, the **Official ZIP Submission** (containing the Windows `.exe`, PDF Notebook, and SQL dump) has been submitted via the standard portal.

---

## Compliance Report (Mapped to Syllabus Instructions)

### Instruction 1: Group Formation

*   **Status:** Solo Project.
*   **Compliance:** Approved by Dr Adem Kikaj (referencing discussion on Dec 2nd regarding single-person scope).

### Instruction 2: Dataset Requirements

*   **Requirement:** At least 3 tables and minimum 1 Foreign Key relationship.
*   **Implemented:** I created a custom database `russell_db` with **4 tables**:
    1.  `Events` (Main timeline)
    2.  `Locations` (Geography)
    3.  `Works` (Bibliography)
    4.  `People` (Relationships)
*   **Compliance:** The `Events` table contains a **Foreign Key** (`location_id`) linking to the `Locations` table.

### Instruction 3: Story & Query Quantity

*   **Requirement:** Story described in 10 lines; minimum 10 SQL queries.
*   **Story:** The user acts as an Archivist reconstructing the fragmented memory of Bertrand Russell. Traversing a non-linear timeline (1910-1950), the user retrieves database snapshots exposing the tension between Russell’s pursuit of Logic and his chaotic Passion. The player must analyze these data points to render a final verdict on his legacy.
*   **Compliance:** I have implemented **10 distinct dynamic SQL query definitions** (encapsulated in Python functions). While the single-person allowance was 7 queries, I chose to implement the full 10 to meet the original standard. These are executed iteratively (totaling 18+ executions) to generate the final dataset.

### Instruction 4: Technical Workflow (Python to Ren'Py)

*   **4(a) & 4(b) English & Python:** All queries are documented in the Jupyter Notebook and written as Python functions (e.g., `def get_complex_snapshot(year)`).
*   **4(c) Dynamic Construction:** No queries are hard-coded. They use f-strings to inject parameters (e.g., `WHERE year = {year}`).
*   **4(d) & 4(e) JSON & Ren'Py:** The script exports a dictionary to `russell_data.json` using a custom `NpEncoder`. The Ren'Py app loads this JSON at startup to drive the game logic.

### Instruction 5: Ren'Py App Requirements

*   **5(a) Characters:** Features "Bertrand Russell" and "The Archivist".
*   **5(b) Dynamic Dialogue:** Dialogue is constructed via interpolation (e.g., `[location_text]`, `[context_text]`) fetched directly from the SQL/JSON data.
*   **5(c) Branching Paths:** The user makes choices that increment `logic_score` or `passion_score`, leading to **3 distinct endings** (The Logician, The Radical, The Paradox).
*   **5(d) Structure:** Clear start screen, a central interview loop, and a definitive conclusion screen.
*   **5(e) Functions (Calculation & Formatting):** I implemented two distinct functions to satisfy this requirement:    
    1. `get_russell_age(year)`: Calculates the character's biological age and vital status based on the selected year.    
    2. `emotion_color(text, tone)`: Dynamically formats dialogue color based on sentiment (e.g., "Sad" = Blue, "Proud" = Gold).
*   **5(f) New Element:** I implemented **two** features not covered in class:
    1. **Screen Language (UI):** Custom timeline_ui dashboard with vbox/hbox layouts and hover states.
    2. **Dynamic Asset Loading:** Used the scene expression statement to inject variable filenames (e.g., "bg_" + current_year) into the visual flow at runtime.
*   **5(g) Imagery:** Dynamic backgrounds change based on the year selected (1910 vs 1950).

### Instruction 6: Query Complexity

*   **Compliance with 6(a) (No Over-simplification):** Strict adherence to the restriction against trivial queries. `SELECT *` is never used. Every query serves a specific analytical purpose, utilizing constraints (`WHERE`, `LIMIT`), aggregations (`COUNT`, `AVG`), or calculations (e.g., `end_year - start_year` inside the SELECT clause).
*   **Compliance with 6(b) (No Over-complication):** Complexity is managed through modular design. All raw SQL logic is encapsulated within reusable Python functions (e.g., `get_war_status(year)`), ensuring the main data pipeline remains readable and efficient without unnecessary nesting.

### Instruction 7: Specific SQL Constraints

*   **7(a) Join with 3-4 Tables:** The main game mechanism uses a **4-Table Join** (Events + Locations + Works + People) to create a comprehensive snapshot of a specific year.
*   **7(b) Aggregation Functions:** I implemented multiple aggregation types:
    *   `COUNT` with `GROUP BY`: To determine the "Most Visited City" (`top_city`).
    *   `AVG`: To calculate the "Average Relationship Duration" (`avg_love`).
*   **7(c) Aggregation with HAVING:** The function `get_obsessions` uses `HAVING count > {min}` to identify genres Russell wrote about most frequently.
*   **7(d) Elements Not Covered in Class:**
    *   **Temporal Joins:** I used `JOIN ... ON year BETWEEN start_year AND end_year` to link People to Events based on time ranges rather than fixed IDs.
    *   **CASE WHEN:** Used to dynamically categorize "Peace" vs "War" status.
    *   **In-Query Calculation:** Performed math operations directly in the SELECT clause (`end_year - start_year`) to derive data for the "Longest Relationship" logic.

---

## How to Re-Run the Data Pipeline

1.  **Database:** Ensure MySQL is running.
2.  **Generate:** Open `Yin_Renlong_hw4_Russell_Final_Project.ipynb` and run all cells. This will:
    *   Drop and recreate the SQL Schema.
    *   Insert the historical data.
    *   Execute the 18 queries.
    *   Export the `russell_data.json` file used by the game.

## AI Declaration

*In accordance with university guidelines:*

*   **Tools:** Google Gemini 3.0 Pro Preview; Microsoft Azure OpenAI GPT-Image-1 API.
*   **Usage:** Gemini used as a coding assistant for Python/SQL syntax debugging and JSON formatting. Azure OpenAI used to generate background assets.
*   **Verification:** I personally verified all code logic, query results, and historical facts.
