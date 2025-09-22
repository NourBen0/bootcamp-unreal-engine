# Collectibles System – Part 1: **Scoring with Casting (via GameMode)**

A lightweight project that demonstrates **Blueprint-to-Blueprint communication using Casting** in Unreal Engine. You’ll build a small scene where:

* The player can collect **4 different types of objects**:

  * **Sphere** → `+10 points`
  * **Cube** → `+5 points`
  * **Cylinder** → `+25 points`
  * **Cone** → `-15 points`
* Each collectible updates the **Score** and the **Total Collected** count.
* When collected, the item is destroyed from the level.

---

## Scene Setup (Overview)

* **BP\_Sphere**: StaticMesh (sphere) + Sphere Collision.
* **BP\_Cube**: StaticMesh (cube) + Box Collision.
* **BP\_Cylinder**: StaticMesh (cylinder) + Capsule or Box Collision.
* **BP\_Cone**: StaticMesh (cone) + Sphere Collision.
* **BP\_ThirdPersonGameMode**: Stores score, exposes `UpdateScore()` and `UpdateTotalCollected()`.
* **Player**: ThirdPersonCharacter capable of overlapping with collectibles.

---

## STEP 1 — Create **BP\_Sphere (+10 Points)**

**Goal:** collectible sphere that adds `+10` to the score.

1. **Blueprint**

   * Add → **Blueprint Class → Actor** → name it `BP_Sphere`.

2. **Components**

   * Add **Static Mesh** → assign a Sphere mesh.
   * Add **Sphere Collision** → resize to cover the sphere mesh.

3. **Event Setup**

   * Select **Sphere Collision** → in **Details → Events** → click **+ OnComponentBeginOverlap**.

4. **Logic (Event Graph)**

   * `OnComponentBeginOverlap` → **Branch**.
   * **Condition**: `Other Actor == Get Player Character`.
   * If **True**:

     * **Get GameMode → Cast to BP\_ThirdPersonGameMode**.
     * From Cast success → call **UpdateScore(+10)**.
     * From Cast success → call \*\*UpdateTotalCollected()\`.
     * Finally → **Destroy Actor** (remove the sphere).

<img width="1637" height="392" alt="image" src="https://github.com/user-attachments/assets/81aedd3e-7468-4b5b-a6ff-b596efea2d10" />


---

## STEP 2 — Create **BP\_Cube (+5 Points)**

**Goal:** collectible cube that adds `+5` to the score.

1. **Blueprint**

   * Add → **Blueprint Class → Actor** → name it `BP_Cube`.

2. **Components**

   * Add **Static Mesh** → assign a Cube mesh.
   * Add **Box Collision** → resize to cover the cube mesh.

3. **Event Setup**

   * Select **Box Collision** → **+ OnComponentBeginOverlap**.

4. **Logic**

   * Same as Sphere, except:

     * Call **UpdateScore(+5)**.
     * Then call \*\*UpdateTotalCollected()\`.
     * Destroy Actor.
       
<img width="1641" height="398" alt="image" src="https://github.com/user-attachments/assets/50c37582-658b-48ff-ac0c-8d56a89a1462" />

---

## STEP 3 — Create **BP\_Cylinder (+25 Points)**

**Goal:** collectible cylinder that adds `+25` to the score.

1. **Blueprint**

   * Add → **Blueprint Class → Actor** → name it `BP_Cylinder`.

2. **Components**

   * Add **Static Mesh** → assign a Cylinder mesh.
   * Add **Box Collision** (or Capsule Collision) → resize to cover the mesh.

3. **Event Setup**

   * Select **Collision** → **+ OnComponentBeginOverlap**.

4. **Logic**

   * Same as Sphere, except:

     * Call **UpdateScore(+25)**.
     * Then call \*\*UpdateTotalCollected()\`.
     * Destroy Actor.

<img width="1670" height="377" alt="image" src="https://github.com/user-attachments/assets/8920bd8e-959d-4bd0-b104-468c1318c72d" />

---

## STEP 4 — Create **BP\_Cone (-15 Points)**

**Goal:** collectible cone that **subtracts -15** from the score.

1. **Blueprint**

   * Add → **Blueprint Class → Actor** → name it `BP_Cone`.

2. **Components**

   * Add **Static Mesh** → assign a Cone mesh.
   * Add **Sphere Collision** → resize to cover the cone.

3. **Event Setup**

   * Select **Sphere Collision** → **+ OnComponentBeginOverlap**.

4. **Logic**

   * Same as Sphere, except:

     * Call **UpdateScore(-15)**.
     * Then call \*\*UpdateTotalCollected()\`.
     * Destroy Actor.

---

## STEP 5 — Update **BP\_ThirdPersonGameMode**

**Goal:** manage scoring and tracking of total collectibles.

1. **Variables**

   * `Score` (Integer, default = 0).
   * `TotalCollected` (Integer, default = 0).

2. **Functions**

   * **UpdateScore(Delta: int)**

     * `Score += Delta`.
     * (Optional) Print Score to screen.
   * **UpdateTotalCollected()**

     * `TotalCollected += 1`.
     * (Optional) Print Total Collected.

---

## Test Flow

1. Place multiple `BP_Sphere`, `BP_Cube`, `BP_Cylinder`, and `BP_Cone` in the level.
2. Play as the player.
3. Collect each shape:

   * Score updates correctly depending on type.
   * Total Collected increments.
   * Collected actor is destroyed.


👉 Do you want me to continue this guide into **Part 2 (GameInstance version)** so the score persists across multiple levels, in the same style?
