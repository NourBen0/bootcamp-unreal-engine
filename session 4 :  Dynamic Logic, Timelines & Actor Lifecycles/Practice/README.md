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
   - Inside the Blueprint, create and assign a Static Mesh for the door

3. **Add a Box Collider**  
   - Add a Box Collider component to define the detection zone  
   - This collider will detect when the player enters the zone and trigger the door opening logic

