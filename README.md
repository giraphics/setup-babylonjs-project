### **Creating a Simple Babylon.js Project with TypeScript and npm**

This guide will walk you through setting up a **Babylon.js TypeScript project** using **npm** and **webpack** for a smooth development experience.

------

## **1. Initialize a New npm Project**

Create a new folder for your Babylon.js project:

```sh
mkdir babylon-ts-project
cd babylon-ts-project
```

Initialize an **npm project**:

```sh
npm init -y
```

This will create a **`package.json`** file.

------

## **2. Install Dependencies**

### **Install Babylon.js and TypeScript**

```sh
npm install --save @babylonjs/core @babylonjs/loaders @babylonjs/materials
npm install --save-dev typescript webpack webpack-cli webpack-dev-server ts-loader
npm install --save-dev html-webpack-plugin
```

- **`@babylonjs/core`** – Main Babylon.js engine.
- **`@babylonjs/loaders`** – Allows importing `.gltf`, `.glb` models.
- **`@babylonjs/materials`** – Additional materials like `SkyMaterial`, `GridMaterial`.
- **`typescript`** – TypeScript compiler.
- **`webpack`** – Bundles the project.
- **`ts-loader`** – Compiles TypeScript in webpack.
- **`webpack-dev-server`** – Serves the project locally.

------

## **3. Create the Project Structure**

```sh
mkdir src public
touch src/index.ts public/index.html webpack.config.js tsconfig.json
```

If you are on windows, Run this inside your project folder:

```sh
echo. > src\index.ts
echo. > public\index.html
echo. > webpack.config.js
echo. > tsconfig.json
```

Project structure:

```
babylon-ts-project
│── node_modules/
│── src/
│   ├── index.ts
│── public/
│   ├── index.html
│── package.json
│── tsconfig.json
│── webpack.config.js
```

------

## **4. Configure TypeScript (`tsconfig.json`)**

In **`tsconfig.json`**, define TypeScript settings:

```json
{
  "compilerOptions": {
    "target": "ES6",
    "module": "ES6",
    "moduleResolution": "node",
    "strict": true,
    "esModuleInterop": true,
    "outDir": "./dist"
  },
  "include": ["src"]
}
```

------

## **5. Configure Webpack (`webpack.config.js`)**

Add the following to **`webpack.config.js`**:

```js
const path = require("path");
const HtmlWebpackPlugin = require("html-webpack-plugin");

module.exports = {
  entry: "./src/index.ts",
  output: {
    filename: "bundle.js",
    path: path.resolve(__dirname, "dist"),
  },
  resolve: {
    extensions: [".ts", ".js"],
  },
  module: {
    rules: [
      {
        test: /\.ts$/,
        use: "ts-loader",
        exclude: /node_modules/,
      },
    ],
  },
  plugins: [
    new HtmlWebpackPlugin({
      template: "./public/index.html",
    }),
  ],
  devServer: {
    static: "./dist",
    open: true,
    hot: true,
  },
};

```

------

## **6. Create the HTML File (`public/index.html`)**

Create **`public/index.html`**:

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Babylon.js with TypeScript</title>
    <style>canvas { width: 100%; height: 100% }</style>
</head>
<body>
    <canvas id="renderCanvas"></canvas>
</body>
</html>
```

------

## **7. Create a Basic Babylon.js Scene (`src/index.ts`)**

Edit **`src/index.ts`**:

```ts
import { Engine, Scene, ArcRotateCamera, Vector3, HemisphericLight, MeshBuilder } from "@babylonjs/core";

// Get the canvas element
const canvas = document.getElementById("renderCanvas") as HTMLCanvasElement;
const engine = new Engine(canvas, true);

// Create the scene
const scene = new Scene(engine);

// Create a camera
const camera = new ArcRotateCamera("Camera", Math.PI / 2, Math.PI / 2, 10, Vector3.Zero(), scene);
camera.attachControl(canvas, true);

// Add lighting
const light = new HemisphericLight("light", new Vector3(0, 1, 0), scene);

// Create a basic sphere
const sphere = MeshBuilder.CreateSphere("sphere", { diameter: 2 }, scene);
sphere.position.y = 1;

// Ground
const ground = MeshBuilder.CreateGround("ground", { width: 6, height: 6 }, scene);

// Render loop
engine.runRenderLoop(() => {
    scene.render();
});

// Resize the engine on window resize
window.addEventListener("resize", () => {
    engine.resize();
});
```

------

## **8. Add Scripts to `package.json`**

Edit **`package.json`** to include the start and build scripts:

```json
"scripts": {
  "start": "webpack serve --mode development",
  "build": "webpack --mode production"
}
```

------

## **9. Run the Project**

### **Start Development Server**

```sh
npm start
```

This will launch a local server at **`http://localhost:8080/`** with **live reload**.

**[ONLY IN CASE A PROCESS IS RUNNITNG ON PORT]** 

If **Node.js** is running on port **8080**, you can **force kill** it using:

```sh
npx kill-port 8080
```

### **Build Production Version**

```sh
npm run build
```

This generates the **`dist/`** folder with the **final bundled files**.

------

## **10. Open in Browser**

Once the development server is running, **open the browser** and visit:

```
http://localhost:8080/
```

You should see a **Babylon.js scene** with a **sphere and a ground plane**.

------

## **Summary**

| Step   | Description                                    |
| ------ | ---------------------------------------------- |
| **1**  | Initialize npm project                         |
| **2**  | Install Babylon.js & TypeScript dependencies   |
| **3**  | Create project structure                       |
| **4**  | Configure TypeScript (`tsconfig.json`)         |
| **5**  | Configure Webpack (`webpack.config.js`)        |
| **6**  | Create `index.html`                            |
| **7**  | Write TypeScript Babylon.js scene (`index.ts`) |
| **8**  | Add npm scripts                                |
| **9**  | Run `npm start` to launch the project          |
| **10** | Open in browser                                |

------

### 🎯 **Now You Have a Fully Working Babylon.js TypeScript Project!**
