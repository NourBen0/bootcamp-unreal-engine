# Blueprint Communication – Part 1: **Direct Communication**

a lightweight project that demonstrates **Blueprint-to-Blueprint direct communication** in Unreal Engine. You’ll build a small scene where:

* 4 barriers block access to a zone with 3 collectible boxes.
* Only **one** barrier is active (`isOK = true`) and will open (rotate 90° on Z) when the player interacts.
* Each collectible box evaluates a simple equation `f(x) = ax + b = 0` using a function `calcul_x` and updates the **Score**.
* If a box’s result `x ≤ 0`, the game ends immediately.

> **Note**: The text says *quadratic*, but the formula shown is **linear** (`ax + b = 0`). This guide implements the **linear** case and computes `x = -b / a`. If you truly need a quadratic, replace `calcul_x` with a solver for `ax^2 + bx + c = 0` and adapt the rules accordingly.

---

## Scene Setup (Overview)

* **BP\_Barriere** × 4: StaticMesh + Box Collision, toggles open if `isOK`.
* **BP\_BoxCollectible** × 3: StaticMesh + Box Collision, evaluates `x` then updates score/visibility.
* **BP\_GameManager** (one actor): Stores score, exposes `AddScore()` and `EndGame()`.
* **Player**: Any Character/Controller capable of overlapping and pressing **E** to interact.

<img width="418" height="241" alt="Screenshot 2025-08-17 151748" src="https://github.com/user-attachments/assets/79c86bd2-02fa-4073-a526-cb5cf75ef181" />

---

## STEP 1 — Create **BP\_Barriere**

**Goal:** a barrier (static mesh) that opens only if `isOK = true`.

1. **Blueprint**

* Add Blueprint Class → **Actor** → name it `BP_barriere`.

2. **Components**

* Add **Static Mesh** → assign your barrier mesh.
* Add **Box Collision** → surrounds the approach area.

3. **Variables**

* `isOK` (Boolean) → **Instance Editable** (checked) → default **false**.

4. **Interaction **

* Select the **Box Collision** → add the **OnComponentBeginOverlap** event.
* **Branch** → `isOK`?
  
  * **True**: Toggle open state.

    * If `bIsOpen` is **false** → **Play** timeline forward (open) and set `bIsOpen = true` on finish.
    * Else → **Reverse** timeline (close) and set `bIsOpen = false` on finish.
  * **False**: Optional **Print** "Barrier inactive".

6. **Timeline (Rotation)**

* Add **Timeline** `Rotation` (Length = `1s`).
* **Float Track** `Alpha` with keys: (0,0) → (1,1).
* On **Update**:

  * **Lerp (Rotator)** from (0,0,`ClosedYaw`) to (0,0,`OpenYaw`) using `Alpha`.
  * **Set Relative Rotation** on the Static Mesh.
    
<img width="1300" height="657" alt="image" src="https://github.com/user-attachments/assets/41ce81a2-b30d-4166-b364-efc93a322d44" />


---

## STEP 2 — Place **4 barrier instances**

* Drag 4x `BP_barriere` into the Level.
* In the **Details** panel per instance:

  * Set **exactly one** instance `isOK = true`.
  * Others remain `false`.
* Test: Only the `true` barrier should respond to **E** and open.

<img width="787" height="335" alt="image" src="https://github.com/user-attachments/assets/c57d0ee8-c37e-4682-8aca-35285545d95b" />

---

## STEP 3 — Create **BP\_GameManager** (Score & Game Over)

**Goal:** centralize score and end-game logic that other BPs can call **directly**.

1. **Blueprint**

* Add Blueprint Class → **Actor** → name it `BP_GameManager`.

2. **Variables**

* `Score` (Integer) → default **0**.

3. **Functions / Events**

* **Function** `AddScore(Delta: int)` → `Score += Delta` → optional on-screen print with new score.
* **Custom Event** `EndGame()` → e.g., Print "Game Over", disable player input, or open a Game Over UI/level.

4. **Place** one instance in the Level.

<img width="1438" height="817" alt="image" src="https://github.com/user-attachments/assets/956f4997-dacb-4ada-86d9-457ad3702ca2" />

---

## STEP 4 — Create **BP\_BoxCollectible**

**Goal:** interact, compute `x = -b/a`, update score/visibility, and possibly end the game.

1. **Blueprint**

* Add Blueprint Class → **Actor** → name it `BP_BoxCollectible`.

2. **Components**

* **Static Mesh** (box mesh).
* **Box Collision** (interaction area).

3. **Variables**

* `a` (Float) → Instance Editable.
* `b` (Float) → Instance Editable.
* `GameManager` (Object Reference → **BP\_GameManager**) → **Instance Editable**.

4. **Function** `calcul_x()` → returns Float

* Guard: if `a == 0` → return a very negative value or 0 and **Print** "Invalid a".
* Else: `return -b / a`.

5. **Interaction **

* **Branch**: 

  * **True** → `x = calcul_x()`; set `xDebug = x` (optional display).

    * If `x > 0`:

      * **Set Hidden In Game = true** (mesh), **Set Collision Enabled = NoCollision**.
      * **GameManager.AddScore(+2)** (if valid).
      * (Optional) **Destroy Actor** after a short delay.
    * Else (`x ≤ 0`):

      * **GameManager.AddScore(-1)** (if valid).
      * `bInteractive = false` (box remains visible but no longer interacts).
      * **GameManager.EndGame()** (end immediately per rules).
  * **False** → do nothing (or Print "Box inactive").

> **Direct communication:** `BP_BoxCollectible` directly calls **functions** on `BP_GameManager` because `GameManager` is a **typed variable** (class = `BP_GameManager`). No casting, no interfaces — just a direct reference assigned in the level.

---

## STEP 5 — Level Wiring (Direct References)

* Place **1× BP\_GameManager** in the level.
* Place **3× BP\_BoxCollectible** in the level.

  * For each box instance:

    * Set `a`, `b` (choose values so you can test `x > 0` and `x ≤ 0`).
    * Assign **GameManager** by picking the placed `BP_GameManager` instance.
* Place **4× BP\_barriere** if not already.

  * Ensure only one has `isOK = true`.

<img width="1072" height="430" alt="image" src="https://github.com/user-attachments/assets/b9d22f82-14a7-4634-bec0-9675eb90d6ca" />

**Test flow:**

1. Approach barriers → only the active one opens with **E**.
2. Enter the zone → interact with boxes using **E**.
3. Each box computes `x` and updates the score.
4. On first `x ≤ 0`, **EndGame** fires immediately.

---

## Troubleshooting & Notes

* **a = 0** in `ax + b = 0` → undefined division. Protect against it.
* Always **Validate** references (e.g., `IsValid(GameManager)`) before calling.
* If you prefer **auto-open** barrier: move the `isOK` branch to **BeginOverlap** and call the timeline directly (no E key).
* Want randomness? Have `BP_GameManager` pick one barrier at **BeginPlay** and set its `isOK = true` (others false) by direct references.

---

## Why this is “Direct Communication”

* Blueprints store **explicit typed references** to other Blueprint classes.
* They call **variables/functions/events** on those references **directly**.
* No **Casting**, **Interfaces**, or **Dispatchers** yet.

> In the next parts, we’ll refactor the same scenario using: **Casting**, **Blueprint Interfaces**, and **Event Dispatchers** — showing when each pattern is cleaner, safer, or more scalable.

