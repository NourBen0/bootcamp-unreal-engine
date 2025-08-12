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

In Version 2, we improve the door interaction by enabling and disabling input based on whether the player is inside the collision box or not. This way, the player can press the E key only when close enough to the door, making the interaction more realistic.

==> **Select** the **Box Collision** component in the Components panel.  
==> In the **Details** panel, scroll down to **Events** and click **+** next to:
   - **On Component Begin Overlap** → creates event for entering the box.
   - **On Component End Overlap** → creates event for leaving the box.
==> Add Input Enable/Disable on Overlap
   - From On Component Begin Overlap event:
      - Drag a wire and Add Enable Input node.
      - Connect the Player Controller as the input target.
      ==> This means when the player enters the collision box, input for this Blueprint is enabled (so it can receive keyboard input).

   - From On Component End Overlap event:
      - Drag a wire and Add Disable Input node.
      - Connect the Player Controller as the input target.
      ==>This disables the input when the player leaves the box, so the door will no longer respond to key presses.

==> Add Input Action for ‘E’ Key

   - In the Blueprint Event Graph, add an Input Action for the E key
   - Connect the Pressed execution pin to a Print String "IN" node.
   - Connect the Released execution pin to a Print String "OUT" node.


<img width="1028" height="516" alt="Screenshot 2025-08-10 140116" src="https://github.com/user-attachments/assets/af2c038a-2996-469d-a2ef-07807bcbb028" />


========== Version 3 =============

In Version 3, we replace the simple Print String with actual door movement. When the player presses the E key, the door will open by rotating 90 degrees. When pressing E again, the door will close back to its original position.

What you’ll add:
==> Add a Timeline
   - In the Blueprint Event Graph, Right-click and search for Add Timeline, name it OpenCloseDoor.
   - Double-click the Timeline node to open the timeline editor.
   - Add a Float Track named opendoor.
   - Set the length to about 1 second.
   - Add two keyframes:
     
   ==> At time 0 → value 0
   ==> At time 1 → value 90
   
   This float will represent the door’s rotation in degrees.

<img width="1041" height="451" alt="image" src="https://github.com/user-attachments/assets/be7520bb-2d47-4c9c-b282-416a91651b3e" />


==> Use FlipFlop on Input Action
  - From the Input Action E (Pressed) node:
    
    ==> Add a FlipFlop node.
    
    ==> On A output → Play DoorTimeline forward (opens the door).
    
    ==> On B output → Play DoorTimeline in reverse (closes the door).
    
==> Set Door Rotation on Timeline Update
  - On the Timeline’s Update pin:
    ==> Once the Timeline starts playing (forward or reverse), it outputs values every frame along the opendoor float track (from our keyframes: 0 → 90 or 90 → 0).
  
  - From the Update pin on the Timeline:
    -  Get your Door Mesh (the part that should move).
    - Add a Set Relative Rotation node to rotate it.
    - Use a Lerp Rotator node
    - Plug the Lerp Rotator into Set Relative Rotation.
    - Why this works
      
      ==> Every frame while the Timeline is running, the opendoor value updates smoothly.
    
      ==> That value is applied directly to the door’s yaw rotation.
      
      ==> Playing forward makes yaw go 0 → 90 (open), playing reverse makes yaw go 90 → 0 (close).
      
      ==> Because it’s Set Relative Rotation, the door rotates relative to its hinge, not the world.


<img width="718" height="450" alt="Screenshot 2025-08-11 144513" src="https://github.com/user-attachments/assets/9c5d47af-c57f-4bd7-8c84-a37072deba9e" />

========== Version 4 =============

In Version 4, we add a lock mechanism to the door, controlled by a new Boolean variable called isLocked. This allows some doors to be locked (cannot open), and others unlocked (can open).

What you’ll add:

===> Add a Boolean Variable
   - In the Blueprint, create a new Boolean variable named isLocked.
   - Set the default value to false (unlocked).
   - Make this variable Instance Editable and Public:
     
     ==> Select the variable.
     
     ==> In the Details panel, check Instance Editable and Expose on Spawn.

<img width="282" height="96" alt="Screenshot 2025-08-12 220010" src="https://github.com/user-attachments/assets/b4918e82-45c0-424c-bbe1-f73ec4b2e1ae" />

<img width="452" height="171" alt="Screenshot 2025-08-12 220036" src="https://github.com/user-attachments/assets/0d61f5ef-22e1-4295-9d51-9902263aa5d4" />


==> Modify the Door Open Logic
   - Before opening the door (when E is pressed), add a branch (if condition).
   - Connect the branch condition to isLocked.
   - If isLocked is false, allow the door to open or close as usual.
   - If isLocked is true, do nothing or optionally print a message like "Door is locked".

<img width="803" height="513" alt="Screenshot 2025-08-12 220247" src="https://github.com/user-attachments/assets/0c2fa922-2b6b-4c57-9795-e8db9e4d300a" />


==> Use Multiple Instances
   - Place multiple instances of your door Blueprint in your level.
   - Select each door instance in the level editor.
   - In the Details panel, you will see the isLocked checkbox.
   - Set it to true or false per instance to control which doors are locked.

<img width="648" height="353" alt="Screenshot 2025-08-12 220353" src="https://github.com/user-attachments/assets/4d680fe0-fb17-40c8-b6c0-d2b3786bb348" />


========== Version 5 =============

In Version 5, we expand the system by adding a new Blueprint BP_Button that acts as a button or switch to unlock the door. This introduces interaction between Blueprints and demonstrates how to reference one Blueprint from another.

What you’ll add:

==> Create a new Blueprint: BP_Button

   - Add a new Blueprint Class → Actor → name it BP_Button.
     
   - Inside BP_Button, add:
     
   ==> A Static Mesh (for the button visual).
   
   ==> A Box Collision (to detect player proximity).

==> Add Input Activation to BP_Button

   - Just like the door, enable input only when the player is overlapping the collision box.
     
   - Add input action for the E key.
     
   - When the player presses E near the button, the button will activate.

==> Create a Variable of type BP_Door

   - Inside BP_Button, create a new variable of type BP_Door (Object Reference).
     
   - Make this variable Instance Editable so you can assign a door to this button in the editor.

==> Unlock Door from Button






