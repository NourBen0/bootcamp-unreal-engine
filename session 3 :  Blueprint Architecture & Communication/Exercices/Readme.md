Practical Session: Collectible Blueprints and Score System

In this session, we will practice Unreal Engine Blueprint logic by creating a simple collectible system where different objects modify a Score variable when collected.

The exercise is broken down step by step to help you understand actor spawning, overlap detection, variable management, and interactivity.

Step-by-Step Guide
Create a Score Manager

Before creating collectibles, we need a way to store and update the player’s score.

Open your project and create a new Blueprint Class.

Choose GameModeBase as the parent class → name it BP_GameMode.

Inside BP_GameMode:

Add a new variable: Score (type: Integer).

Set default value to 0.

Make it Public/Instance Editable so it can be referenced from other Blueprints.

This Score variable will be updated whenever a collectible is picked up.

Create BP_Sphere (Collectible: +10 Points)

Create a new Blueprint Class → Actor → name it BP_Sphere.

Inside BP_Sphere, add:

Static Mesh (set to a Sphere mesh).

Sphere Collision (make sure it covers the mesh).

📌 Collision Setup

Select Sphere Collision.

In the Details panel → Events → Click + On Component Begin Overlap.

📌 Blueprint Logic

From Begin Overlap event → Cast to BP_GameMode.

Get Score variable → Add 10 → Set Score.

Destroy the actor (so the sphere disappears after collection).

💡 Tip: Add a Print String node after setting Score to display the updated value.

Create BP_Cube (Collectible: +5 Points)

Create new Blueprint Class → Actor → name it BP_Cube.

Add:

Static Mesh (set to Cube).

Box Collision (cover the cube).

📌 Logic

On Begin Overlap:

Cast to BP_GameMode.

Score = Score + 5.

Destroy Actor.

Create BP_Cylinder (Collectible: +25 Points)

Create Blueprint Actor → BP_Cylinder.

Add:

Static Mesh (Cylinder).

Capsule Collision (or Box, adjusted to size).

📌 Logic

On Begin Overlap:

Cast to BP_GameMode.

Score = Score + 25.

Destroy Actor.

Create BP_Cone (Collectible: -15 Points)

Create Blueprint Actor → BP_Cone.

Add:

Static Mesh (Cone).

Collision component.

📌 Logic

On Begin Overlap:

Cast to BP_GameMode.

Score = Score - 15.

Destroy Actor.

Test in the Level

Place BP_Sphere, BP_Cube, BP_Cylinder, and BP_Cone into your level.

Press Play → move the player over them.

Watch the Score update (check with Print String or add a UI Widget later for a proper HUD).

Versions / Expansions
Version 1: Basic Collectibles

Items update the score correctly when collected.

Version 2: Add Sound & FX

Add a Sound Cue (pickup sound) in each collectible.

Add a Particle System (spark effect).

Version 3: Add a HUD

Create a Widget Blueprint (BP_ScoreUI).

Bind the Score variable to a Text Block.

Add it to the viewport in the GameMode or PlayerController.

Version 4: Respawning Items

Instead of destroying items, hide them temporarily.

Use a Delay node to respawn them after X seconds.
