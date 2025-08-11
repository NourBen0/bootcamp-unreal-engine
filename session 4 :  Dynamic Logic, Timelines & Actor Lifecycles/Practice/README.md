# Practical Session: Open and Close the Door

In this session, we will practice key concepts through the exercise **Open and Close the Door**.  

The exercise is broken down into steps to help you fully understand the underlying logic, including timelines, events, and actor spawning.

## Step-by-Step Guide

1. **Create a new Blueprint**  
   - Add a new Blueprint Class       
<img width="413" height="236" alt="step1" src="https://github.com/user-attachments/assets/09e04f6c-6ac4-4366-a66a-6991ba0a2b99" />

   - Choose **Actor** as the parent class
<img width="275" height="225" alt="Screenshot 2025-08-10 145854" src="https://github.com/user-attachments/assets/e004c618-f272-48b9-8272-1b3b988e8279" />


2. **Add a Static Mesh**
   - Inside the Blueprint, create and assign a Static Mesh for the door.  
   <img width="331" height="356" alt="step2" src="https://github.com/user-attachments/assets/cdaa8fe5-9483-4be8-ba65-9c2321a3aca4" />
   
   - You should assign **SM_DoorFrame** as the main mesh, and inside it, add **SM_Door**.
<img width="433" height="111" alt="Screenshot 2025-08-10 151636" src="https://github.com/user-attachments/assets/5f99819f-635a-4032-8d31-06306f2d037e" /> <img width="440" height="101" alt="Screenshot 2025-08-10 151703" src="https://github.com/user-attachments/assets/f47df966-d760-4a87-a4b9-bc319d515962" />


   - If you can't find these assets, make sure to add the **StarterContent** to your project by following these steps:  
  ==> Click on **+ Add** in the Content Browser.  
  ==> Select **Add Feature or Content Pack**.  
  ==> Choose **Content** from the options.  
  ==> Select **Starter Content** and add it to your project.
   
   <img width="311" height="188" alt="Screenshot 2025-08-10 151853" src="https://github.com/user-attachments/assets/a5686348-88a3-4af8-8e3a-632e4918f549" />



3. **Add a Box Collision**  
   - Add a Box Collision component to define the detection zone  
   - This collision will detect when the player enters the zone and trigger the door opening logic
<img width="327" height="180" alt="Screenshot 2025-08-10 150611" src="https://github.com/user-attachments/assets/3c778405-b8aa-4841-917f-980fbd3491c8" />


4. **Basic Blueprint Programming**
   
========== Version 1 =============

==> **Select** the **Box Collision** component in the Components panel.  
==> In the **Details** panel, scroll down to **Events** and click **+** next to:
   - **On Component Begin Overlap** → creates event for entering the box.
   - **On Component End Overlap** → creates event for leaving the box.
     
==> In the Event Graph:
   - From **Begin Overlap** event → **Drag wire** → **Add “Print String” node** → set text to `"In"`.
   - From **End Overlap** event → **Drag wire** → **Add “Print String” node** → set text to `"Out"`.
     
==> **Compile** and **Save** the Blueprint.

<img width="595" height="511" alt="Screenshot 2025-08-10 135444" src="https://github.com/user-attachments/assets/2f1a51cc-26ea-4420-9aa8-13eb20946ef5" />

Great! You’ve set up the **Input Enable/Disable** on overlap, and the **E key press** prints “IN” and “OUT” based on key press state, which is a solid foundation.

To expand this into your **Version 2 door open/close interaction**, here’s what you need to add or modify based on your current setup:


========== Version 2 =============


==> **Add a `PlayerInZone` Boolean variable**

   * On **Begin Overlap**, set `PlayerInZone = true`
   * On **End Overlap**, set `PlayerInZone = false`

==> **Modify the E key input logic**

   * Before doing anything on the E key press, check if `PlayerInZone` is `true` (using a Branch node)
   * If true, execute the FlipFlop logic for opening/closing the door

==> **Add a FlipFlop node after the E key pressed event**

   * On **A**: Play the Timeline **forward** (door opens)
   * On **B**: Play the Timeline **reverse** (door closes)

==> **Create and add the Timeline node named `DoorTimeline`**

   * Timeline drives a float `Alpha` from 0 to 1 in 1 second
   * Use `Alpha` to Lerp between closed and open door rotations

==> **Use the Timeline update output to rotate the door**

   * Use `Lerp (Rotator)` node with `ClosedRotation` and `OpenRotation`
   * Set the door Static Mesh rotation accordingly every update tick from the Timeline


<img width="1028" height="516" alt="Screenshot 2025-08-10 140116" src="https://github.com/user-attachments/assets/af2c038a-2996-469d-a2ef-07807bcbb028" />







