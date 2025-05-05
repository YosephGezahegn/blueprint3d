# Blueprint3D Documentation

## Overview
Blueprint3D is a customizable web application for designing interior spaces (such as homes or apartments). It allows users to:
- Draw and edit 2D floorplans
- Add and arrange furniture/items
- Visualize and edit their designs in 3D

The application is built on top of [three.js](https://threejs.org/) for 3D rendering, and is modular for easy extension and customization.

---

## Architecture

### Main Components
- **BP3D.Blueprint3d (src/blueprint3d.ts):** Entry point for the app. Initializes the Model, ThreeJS renderer, and Floorplanner.
- **Model (src/model/):** Manages the 2D floorplan (rooms, walls, corners) and the 3D scene (items, their positions, and geometry).
- **Floorplanner (src/floorplanner/):** Provides the interactive 2D drawing tool for creating and editing floorplans.
- **ThreeJS Integration (src/three/):** Handles 3D rendering, camera controls, and scene management.
- **Items (src/items/):** Defines furniture and objects that can be placed in the scene.
- **Core Utilities (src/core/):** Utility functions, configuration, and helpers.

---

## How It Works
1. **Initialization:** The `Blueprint3d` class is instantiated with configuration options (DOM elements, texture directory, etc.).
2. **2D Floorplan:** Users draw rooms by placing corners and walls in the Floorplanner. The Floorplanner updates the underlying Floorplan model.
3. **3D Scene:** The Model's Scene reflects the Floorplan and contains 3D items. The ThreeJS renderer visualizes this scene.
4. **Adding Items:** Furniture and other objects are added to the Scene and can be positioned, rotated, and scaled in 3D.
5. **Switching Views:** Users can switch between the 2D floorplanner and the 3D view for editing.

---

## Where Do the Models Come From?

The 3D models (furniture and other items) are managed in the `src/items/` directory. Here’s how the system works:

- **Abstract Base Class:**
  - `Item` (in `item.ts`) is an abstract class representing any object that can be placed in the scene (e.g., furniture, wall items).

- **Item Types and Factory:**
  - The `factory.ts` file defines a mapping from item type numbers to specific item classes (e.g., `FloorItem`, `WallItem`, etc.).
  - The `Factory.getClass(itemType)` method returns the appropriate class for a given item type.

- **Model Loading:**
  - When an item is added to the scene (see `Scene.addItem()` in `src/model/scene.ts`), the system uses the item type and a file name (typically a model URL) to load the 3D model data.
  - The loader (usually a Three.js loader like `THREE.JSONLoader`) loads the geometry and materials from the provided file.
  - The item is then instantiated with this geometry and added to the 3D scene.

- **Example:**
  ```js
  // Adding an item to the scene (simplified):
  var item = new (Items.Factory.getClass(itemType))(
    model,
    metadata, geometry,
    new THREE.MeshFaceMaterial(materials),
    position, rotation, scale
  );
  ```
  - The `itemType` determines which subclass of `Item` is used.
  - The `fileName` (model URL) determines which 3D model is loaded.

- **Custom Models:**
  - To add new models, you can extend the item classes and update the factory mapping.
  - Models are typically stored as JSON or other Three.js-compatible formats and referenced by URL.

---

## Serialization
- **Export:** Use `Model.exportSerialized()` to get a JSON representation of the floorplan and items.
- **Import:** Use `Model.loadSerialized(json)` to load a design.

---

## Running Locally
1. Install dependencies: `npm install`
2. Build the app: `grunt`
3. Start a local server in the `example` directory (e.g., `python -m http.server`)
4. Visit `http://localhost:8000` in your browser.

---

## Extending the App
- Add new item types by extending the `src/items/` module and updating the factory.
- Customize the UI by editing files in `example/` and the main JS/TS files.
- Extend logic by editing `src/model/`, `src/floorplanner/`, or `src/three/` as needed.

---

## Contribution & Further Work
- Contributions are welcome for documentation, tests, new features, and refactoring.
- See the README for a list of TODOs and areas for improvement.
