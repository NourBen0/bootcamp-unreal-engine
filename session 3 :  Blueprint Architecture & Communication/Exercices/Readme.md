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

<img width="1657" height="437" alt="image" src="https://github.com/user-attachments/assets/d0519900-61fe-492e-8f05-3648d82cbd30" />

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
     <img width="1047" height="458" alt="image" src="https://github.com/user-attachments/assets/ebd7d19d-8dea-40fa-a7d2-4bb65d71d85c" />

   * **UpdateTotalCollected()**

     * `TotalCollected += 1`.
     * (Optional) Print Total Collected.
       <img width="1041" height="492" alt="image" src="https://github.com/user-attachments/assets/041ace24-036e-4f81-80e9-02d201555b93" />


---

## Test Flow

1. Place multiple `BP_Sphere`, `BP_Cube`, `BP_Cylinder`, and `BP_Cone` in the level.
2. Play as the player.
3. Collect each shape:

   * Score updates correctly depending on type.
   * Total Collected increments.
   * Collected actor is destroyed.


Perfect 👍 Thanks for clarifying.
So in **Part 2**, we build the **Key & Door system** **without Casting at all**.
That means we’ll use **direct references (editable variables)** between Blueprints instead of Casts or GameMode logic.

Here’s the guide in the same style:

---

# Collectibles System – Part 2: **Keys & Doors **

In this version, we introduce a **Key** and a **Door**.
The player must collect the **Key** before the **Door** can be opened.

Unlike other approaches, here we avoid **Casting** and **GameMode references**.
Instead, the **Door** holds a **direct reference to a Key** and checks if it was collected.

---

## STEP 1 — Create **BP\_Key**

**Goal:** an actor that can be picked up and sets itself as "collected".

1. **Blueprint**

   * Add → **Blueprint Class → Actor** → name it `BP_Key`.

2. **Components**

   * **Static Mesh** → assign a key mesh.
   * **Sphere Collision** → surrounds the mesh for pickup.

3. **Variables**

   * `bIsCollected` (Boolean, default = false).

4. **Logic (Event Graph)**

   * `OnComponentBeginOverlap`:

     * Branch → check `Other Actor == Player Character`.
     * If **true**:

       * Set `bIsCollected = true`.
       * Optionally Print `"Key collected!"`.
       * Hide the mesh or call `DestroyActor`.

---

## STEP 2 — Create **BP\_Door**

**Goal:** door only opens if the linked key has been collected.

1. **Blueprint**

   * Add → **Blueprint Class → Actor** → name it `BP_Door`.

2. **Components**

   * **Static Mesh** → assign a door mesh.
   * **Box Collision** → surrounds the doorway.

3. **Variables**

   * `LinkedKey` (BP\_Key Reference → Instance Editable).
   * `bIsOpen` (Boolean, default = false).

4. **Logic (Event Graph)**

   * `OnComponentBeginOverlap`:

     * Branch → check `Other Actor == Player Character`.
     * If **true**:

       * Check if `LinkedKey → bIsCollected == true`.

         * If true → Play Timeline to rotate/open door.
         * If false → Print `"Door is locked – find the key."`.
           
<img width="1382" height="400" alt="image" src="https://github.com/user-attachments/assets/b06da1c1-96f7-48a3-9ee2-3b94eaf1024c" />

---

## STEP 3 — Level Setup

1. Place a **BP\_Key** somewhere in the level.
2. Place a **BP\_Door** in front of an area.
3. In the **Details Panel** of BP\_Door → assign the placed **BP\_Key** to its `LinkedKey` variable.

Now the Door knows **which Key** it depends on.




