# Niagara Trailing System for Chaos Destruction

This is a quick reference guide based on the tutorial by SARKAMARI:  
**[Chaos Destruction | Complete Guide Part 3 (Niagara Integration)](https://www.youtube.com/watch?v=u3hSFnBQpBM)**

This guide assumes that you already have a Geometry Collection placed in your level and configured for Chaos Destruction. The purpose is to create a Niagara particle system that reacts to debris motion (trailing) and spawns dust or visual trails.

---

## Step 1. Create a new Niagara System

- Choose the **Fountain** emitter preset and create a new Niagara System.

![Step 1](images/screen_001.jpg)

---

## Step 2. Remove unnecessary modules from the emitter

- Delete **Add Velocity** and **Gravity Force** modules. Select them and press Delete.

![Step 2](images/screen_002.jpg)

---

## Step 3. Change Simulation Target to GPU

- In the **Emitter Properties** module:
  - Set **Sim Target** to `GPUCompute Sim`.
  - Set **Calculate Bounds Mode** to `Fixed`.

![Step 3](images/screen_003.jpg)

---

## Step 4. Remove the Spawn Rate module

- Chaos events will control particle spawning, so remove the **Spawn Rate** module.

![Step 4](images/screen_004.jpg)

---

## Step 5. Add the Spawn From Chaos module

- Under **Emitter Update**, add **Spawn From Chaos**.
- If it doesn't show up, ensure the **Plugins** checkbox is enabled under Source Filtering.

![Step 5](images/screen_005.jpg)

- Set all inputs to "Read from new user parameter" by creating a new user parameter of type **Chaos Destruction Data Interface**.

![Step 5.1](images/screen_006.jpg)

---

## Step 6. Add Apply Chaos Data module

- Under **Particle Spawn**, add **Apply Chaos Data**.

![Step 6](images/screen_007.jpg)

- For input, use the previously created parameter (`New Chaos Destruction Data`).

![Step 6.1](images/screen_008.jpg)

---

## Step 7. Set up basic particle visuals for testing

- In the **Initialize Particle** module:
  - Assign a bright color (e.g. green).
  - Set size to around 200–300 units to make particles clearly visible during testing.

![Step 7](images/screen_009.jpg)

---

## Step 8. Create a particle material

- Create a new material:
  - Set **Blend Mode** to `Translucent`.
  - Set **Shading Model** to `Unlit`.

![Step 8](images/screen_010.jpg)

- Use a grayscale smoke texture or procedural noise.
- To rename inputs in the **Dynamic Parameter** node, open the **Details** panel and modify **Param Names**.

![Step 8.1](images/screen_011.jpg)

- To adjust the preview window:
  - Go to **Window → Preview Scene Settings**.
  - Disable **Background** under **Show**.
  - Disable **Post Processing Enabled**.
  - Change the mesh from sphere to cube or plane for easier previewing.

![Step 8.2](images/screen_012.jpg)

---

## Step 9. Assign the material to particles

- In the **Sprite Renderer** section of the emitter, assign the material you just created.

![Step 9](images/screen_013.jpg)

---

## Step 10. Place the Niagara System in the scene and configure it

- Place the Niagara System into the level.
- In the **Details** panel under **User Parameters**:
  - Set `New Chaos Destruction Data → Data Source` to `Trailing Data`.
  - Set **Max Number of Data to Spawn Particles** to a high value (e.g. `2000`).  
    This is the maximum number of spawn events handled per tick.
  - Set **Data Process Frequency** to `15–20`.  
    This controls how frequently chaos events are processed. Higher values increase responsiveness at the cost of performance.

![Step 10](images/screen_015.jpg)

- Select your **Geometry Collection** and in the **Details** panel under **Events**, enable:
  - `Notify Breaks`
  - `Notify Trailing`

![Step 10.1](images/screen_014.jpg)

- Make sure the following project setting is enabled:  
  **Edit → Project Settings → Engine → Physics → Chaos Physics → Solver Options → Generate Trailing Data**

![Step 10.2](images/screen_016.jpg)

---

## Step 11. Final particle tuning (optional)

This step follows the tutorial setup. You are free to modify parameters creatively.

- In the **Emitter State** module:
  - Set **Life Cycle Mode** to `Self`
  - Set **Loop Behavior** to `Once`
  - Set **Loop Duration Mode** to `Fixed`
  - Set **Loop Duration** to `5`

![Step 11](images/screen_017.jpg)

- Return to **Initialize Particle** and:
  - Adjust color to match the scene.
  - Set **Lifetime** and **Color Mode** to achieve fading and variation.

![Step 11.1](images/screen_018.jpg)

- Add **Dynamic Material Parameters** under **Particle Update** to control shader inputs like erosion and tiling.

![Step 11.2](images/screen_019.jpg)

- Add **Scale Sprite Size** module for dynamic particle scaling.

![Step 11.3](images/screen_021.jpg)

- Recommended curve:
  - `[0.8, 1.0]` to `[1.0, 4.0]`
  - Smooth the curve for better visual results.

![Step 11.4](images/screen_022.jpg)

- To control particle density, adjust **Spawn Percentage Fraction** in **Spawn From Chaos**.
  - A lower value reduces how many chaos events generate particles, improving performance.

![Step 11.5](images/screen_023.jpg)

- Add **Wind Force** in **Particle Update** to simulate ambient wind motion.

![Step 11.6](images/screen_024.jpg)

---

## Result

After completing these steps, your Niagara system will spawn particles dynamically when debris trails are generated by Chaos Destruction events.

If configured correctly, the system will visually enhance the realism of collapsing or breaking structures with dust or debris trails.
