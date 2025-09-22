# Practical Session: Collectible Blueprints and Score System

In this session, we will practice key Blueprint concepts through the exercise **Collectible Blueprints and Score System**.

The exercise is broken down into steps to help you fully understand the underlying logic, including collision detection, variables, and inter-Blueprint communication.

# Step-by-Step Guide
## STEP 1 : Create a GameMode with a Score Variable  

==> Create a new Blueprint Class
==> Choose GameModeBase as the parent class
==> Name it BP_GameMode

Inside BP_GameMode:

Add a new variable: Score (type: Integer).

Set default value = 0.

Make it Public (Instance Editable) if needed for debugging.

📌 This variable will track the total player score whenever a collectible is picked up.



BP_Sphere (+10 Points)

==> Create a new Blueprint Class → Actor → BP_Sphere
==> Inside BP_Sphere:

Add Static Mesh → assign a Sphere mesh.

Add Sphere Collision → scale to cover the mesh.

==> Select Sphere Collision → In Details panel → Events → Click + On Component Begin Overlap

📌 Blueprint Logic:

From Begin Overlap event → Cast to BP_GameMode.

Get Score → Add 10 → Set Score.

Add a Print String to display “Score: X”.

Add Destroy Actor so the sphere disappears when collected.


BP_Cube (+5 Points)

==> Create Blueprint Actor → BP_Cube
==> Add Static Mesh → Cube
==> Add Box Collision

📌 Logic:

On Begin Overlap → Cast to BP_GameMode

Score = Score + 5

Print String → “+5 collected!”

Destroy Actor

Screenshot 2025-09-19 120355

BP_Cylinder (+25 Points)

==> Create Blueprint Actor → BP_Cylinder
==> Add Static Mesh → Cylinder
==> Add Capsule Collision

📌 Logic:

On Begin Overlap → Cast to BP_GameMode

Score = Score + 25

Print String → “+25 collected!”

Destroy Actor

Screenshot 2025-09-19 120422

BP_Cone (–15 Points)

==> Create Blueprint Actor → BP_Cone
==> Add Static Mesh → Cone
==> Add Collision

📌 Logic:

On Begin Overlap → Cast to BP_GameMode

Score = Score - 15

Print String → “–15 penalty!”

Destroy Actor

Screenshot 2025-09-19 120518

✅ At this stage, you can place multiple collectibles in your level and see the Score update in the Output Log using Print String.

========== Version 2 =============

In Version 2, we add feedback elements (sounds and particles) to make collecting items feel more interactive.

==> Inside each collectible Blueprint:

Add an Audio Component → assign a pickup sound.

Add a Particle System Component → e.g., P_Explosion or P_Sparks.

==> On Begin Overlap:

Play Sound at Location (or activate Audio Component).

Spawn Emitter at Location (or activate Particle Component).

Update Score.

Destroy Actor.

Screenshot 2025-09-19 120644

========== Version 3 =============

In Version 3, we display the Score on a UI Widget instead of just Print String.

==> Create a new Widget Blueprint → WBP_ScoreUI.

Add a Text Block → name it ScoreText.

Bind its text to the Score variable.

==> In BP_GameMode:

On BeginPlay → Create Widget (WBP_ScoreUI).

Add to Viewport.

Now the Score updates in real time on the HUD.

Screenshot 2025-09-19 120815

========== Version 4 =============

In Version 4, we expand the system with respawning collectibles instead of destroying them.

==> Instead of Destroy Actor:

Add a Set Actor Hidden in Game (true) node.

Add a Set Actor Enable Collision (false) node.

Add a Delay (e.g., 5 seconds).

After delay:

Set Actor Hidden in Game (false).

Set Actor Enable Collision (true).

📌 This makes collectibles disappear when collected and reappear later.

Screenshot 2025-09-19 121022

========== Version 5 =============

In Version 5, we introduce special items and more advanced logic:

Golden Sphere → gives bonus +50.

Poison Cube → removes 25 points and plays a red explosion.

Combo Mechanic → If player collects 3 cylinders in a row, grant an extra +100 bonus.

==> Add new Blueprints based on existing ones.
==> Use Boolean variables or Counters to track combo conditions.
==> Expand GameMode logic for bonuses/penalties.

Screenshot 2025-09-19 121223

Why this works

Each collectible updates a shared Score variable inside the GameMode.

Using Overlap events ensures the player must physically touch the object.

Destroying or hiding actors simulates collection.

UI binding allows real-time feedback.

Expanding with FX, respawn, and combo rules makes the system scalable.
