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
    
    ==> Use the DoorRotation float track value to set the door’s rotation yaw.
    
    ==> Connect it to a Set Relative Rotation node targeting the door Static Mesh variable.
    
    ==> Use a Make Rotator node with yaw = DoorRotation and pitch/roll = 0.

<img width="718" height="450" alt="Screenshot 2025-08-11 144513" src="https://github.com/user-attachments/assets/9c5d47af-c57f-4bd7-8c84-a37072deba9e" />




