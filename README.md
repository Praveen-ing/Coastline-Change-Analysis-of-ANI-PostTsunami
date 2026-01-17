# Coastline-Change-Analysis-of-ANI-PostTsunami
Post-Tsunami Coastline Change Analysis of the Andaman &amp; Nicobar Islands Using Multi-Temporal Satellite Imagery (2000–2023)




Downloading dataset from USGS Earth Explorer 

andaman and nicobar 

![image.png](attachment:2ec6cb23-753d-402f-af42-951fd327425f:image.png)

these are the co ordinates 

14.90  91.80
14.90  94.00
06.30  94.00
06.30  91.80
14.90  91.80

Installed QGIS 

## 🔍 What is a “scene” in Landsat?

A **scene** is:

- One satellite pass
- On **one date**
- For **one path–row tile**
- Covering ~**185 km × 185 km**

So when you see one item in the Results list, that is **one scene**.

---

## 📦 What does the *Level-2 Product Bundle* contain?

Even though it’s **one scene**, the bundle includes **many files**:

### Inside ONE bundle you get:

- Surface Reflectance bands
    
    (`SR_B1, SR_B2, SR_B3, SR_B4, …`)
    
- QA / cloud mask
    
    (`QA_PIXEL`)
    
- Metadata file
    
    (`MTL.txt`)
    
- Sometimes thermal products

👉 So it looks like “many images”, but they are **bands of the same scene**, not multiple scenes.

---

## 🧠 Important distinction (remember this)

| Term | Meaning |
| --- | --- |
| **Scene** | One satellite image on one date |
| **Bundle** | All bands + metadata of that one scene |
| **Multiple scenes** | You must download multiple bundles |

---

## 🟢 How many scenes do you need?

For your project:

### Minimum (recommended to start):

- **2–3 scenes per time period**

Example:

- 2000–2003 → 2–3 scenes
- 2005–2006 → 2–3 scenes
- 2015 → 2–3 scenes
- 2020–2023 → 2–3 scenes

That means:

👉 **You will download multiple bundles**, one per scene.

for the first time am taking this scene with time range of 2000 - 2003  

![image.png](attachment:c8c54d82-4359-4114-94f0-3f031c80f1e7:image.png)

we have to download the 

***LE07_L2SP_134051_20031029_20200915_02_T1***     

extract this downloaded dataset file of the scene we will so files like this  

![image.png](attachment:f7c58626-553e-46df-bc89-9d4465219e81:image.png)

now the actual analysis starts 

# 🚀 WHAT TO DO NEXT (NOW THAT DATA IS EXTRACTED)

You are holding a **valid Landsat Level-2 scene**.

From this, we will create your **first NDWI map**.

---

## ✅ STEP 0: Understand what you see (very important)

From your folder screenshot, these are the **key files**:

### 🔑 Files you WILL use

- `_SR_B2.TIF` → **Green band**
- `_SR_B4.TIF` → **Near Infrared (NIR) band**
- `_QA_PIXEL.TIF` → Cloud / shadow mask (later)

### 🚫 Files you IGNORE for now

- `ST_*` → Surface temperature (not needed)
- `SR_B1, SR_B3, SR_B5, SR_B7` → not needed for NDWI
- Thumbnails, ANG, MTL → metadata only

👉 For NDWI, **only B2 and B4** matter.

---

## 🧰 STEP 1: Install QGIS (if not already)

If already installed, skip to Step 2.

- Download: [https://qgis.org](https://qgis.org/)
- Install **Latest Long Term Release (LTR)**

---

## 🗺️ STEP 2: Open QGIS and load the bands

1. Open **QGIS**
2. Click **Layer → Add Layer → Add Raster Layer**
3. Browse to your folder and add:
    - `_SR_B2.TIF`
    - `_SR_B4.TIF`
4. Click **Add**

You will now see **two grayscale images**.

✔ This means QGIS is reading the data correctly.

---

## 🧮 STEP 3: Create NDWI (THIS IS THE CORE STEP)

### NDWI formula (remember this):

NDWI=Green−NIRGreen+NIRNDWI = \frac{Green - NIR}{Green + NIR}

NDWI=Green+NIRGreen−NIR

### In QGIS (click-by-click):

1. Go to **Raster → Raster Calculator**
2. In the expression box, type **exactly**:

```
("LE07_L2SP_..._SR_B2@1" -"LE07_L2SP_..._SR_B4@1") /
("LE07_L2SP_..._SR_B2@1" +"LE07_L2SP_..._SR_B4@1")

```

👉 (Double-click band names from the list to avoid typos)

1. Output file:
    - Name: `NDWI_2003_L7.tif`
    - Location: create a folder `ndwi/`
2. Click **OK**

⏳ Wait a few seconds.

---

## 🌊 STEP 4: Visualize NDWI correctly

Your NDWI layer will appear, but it may look dull. Fix it:

1. Right-click NDWI layer → **Properties**
2. Go to **Symbology**
3. Set:
    - Render type: **Singleband pseudocolor**
    - Color ramp: **Blue → White**
4. Click **Classify → Apply**

### Interpretation:

- **Bright / positive values** → Water
- **Dark / negative values** → Land

🎉 You are now literally seeing **land vs water separation**.

---

## ✂️ STEP 5: Extract water mask (simple threshold)

1. Open **Raster Calculator**
2. Expression:

```
"NDWI_2003_L7@1" >0

```

1. Save as:
    - `WaterMask_2003.tif`

This creates a **binary water map**.

---

## 🧭 STEP 6: What this gives you (important concept)

From just **one scene**, you already have:

- ✔ NDWI map
- ✔ Water mask
- ✔ Basis for shoreline extraction

Repeating this for multiple years → **shoreline change analysis**.

---

## 🛑 STOP HERE (important)

Do **NOT** rush to download more scenes yet.

First confirm:

- NDWI works
- You understand the process

![image.png](attachment:f888f46a-cde7-4591-8241-361453a71b8c:image.png)

# 🟢 STEP 1: Save your QGIS project (do this first)

1. Click **Project → Save As**
2. Create a folder:
    
    ```
    ANI_Project/
    └── qgis_project/
    
    ```
    
3. File name:
    
    ```
    ANI_Coastline.qgz
    
    ```
    
4. Click **Save**

👉 This avoids crashes and lost work.

---

# 🟢 STEP 2: Add your Landsat bands (most important step)

### What we will load

From your extracted folder, we need **ONLY these two files**:

- `_SR_B2.TIF` → Green band
- `_SR_B4.TIF` → NIR band

---

### How to load them

1. In the **top menu**, click
    
    **Layer → Add Layer → Add Raster Layer**
    
2. Click **Browse**
3. Go to your folder:
    
    ```
    BTP/Data/Landsat/2000-2003/LE07_L2SP_134051_20031029_...
    
    ```
    
4. Select:
    - `..._SR_B2.TIF`
    - `..._SR_B4.TIF`
        
        (hold **Ctrl** and click both)
        
5. Click **Add**
6. Click **Close**

✅ You should now see **two grey images** on the map

✅ In the **Layers panel (bottom-left)**, you’ll see both bands listed

If you see them → you’re doing great.

![image.png](attachment:7ddb1c12-046e-48ab-821b-f4b9132b7302:image.png)

we are here after loading the Band 2 and Band 4  

![image.png](attachment:348c4af7-4635-4b3c-92c7-0ffeecf0208c:image.png)

add the NDWI  Raster →  This formula by double click   

https://youtu.be/ArdEYrTe7Zs?si=Ga4QAJjcqQpZh0DH

this is the exact video we have to do  from here  

but am not getting the exact same thing as in the vifdeo 

![image.png](attachment:9743c667-efbf-4dad-ba5d-72a4bea25b89:image.png)

something  like this    

till here to seperate land and water next last step is not working in my  QGIS   so work on it later