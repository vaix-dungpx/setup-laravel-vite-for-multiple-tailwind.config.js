
## Compile multiple tailwind css from different tailwind.config.js files using laravel-vite

If do you want to compile two or more tailwind css from different tailwind.config.js files, then you have to follow this guide.

#### SCENARIO:
I want to create two **tailwind.config.js** files one config for FRONTEND and second for BACKEND.

How can I do it with laravel-vite, because laravel is new tool for asset compilation but I am familiar with laravel webpack.mix package?


Yes, it is possible to compile separate tailwind.config.js files for each tailwind css assets.

just follow these steps with me to implement this in laravel project.

I have also created a detail youtube video tutorial for it.

watch now

[![Alt text](https://i.ytimg.com/vi/887_dkXohDU/hqdefault.jpg?sqp=-oaymwEcCPYBEIoBSFXyq4qpAw4IARUAAIhCGAFwAcABBg==&rs=AOn4CLCsQqvP8o5dFE-s8lt0CQ1wQOQMKw)](https://youtu.be/887_dkXohDU)

let's start!!!


### STEP 1:
Create two files of **vite.config.js**. One for frontend and second for backend

#### file 1: vite.frontend.config.js
```javascript
import { defineConfig } from 'vite';
import laravel, { refreshPaths } from 'laravel-vite-plugin';

export default defineConfig({
    plugins: [
        laravel({
            input: [
                'resources/frontend/frontend-tailwind.css',
            ],
            refresh: [
                ...refreshPaths,
            ],
            buildDirectory: '/frontendAssets',
        }),
    ],
    css: {
        postcss: {
            plugins: [
                require("tailwindcss/nesting"),
                require("tailwindcss")({
                    config: "./frontend-tailwind.config.js",
                }),
                require("autoprefixer"),
            ],
        },
    },
});
```
#### file 2: vite.backend.config.js
```javascript
import { defineConfig } from 'vite';
import laravel, { refreshPaths } from 'laravel-vite-plugin';

export default defineConfig({
    plugins: [
        laravel({
            input: [
                'resources/backend/dashboard/backend-tailwind.css',
            ],
            refresh: [
                ...refreshPaths,
            ],
            buildDirectory: '/backendAssets/dashboard',
        }),
    ],
    css: {
        postcss: {
            plugins: [
                require("tailwindcss/nesting"),
                require("tailwindcss")({
                    config: "./backend-tailwind.config.js",
                }),
                require("autoprefixer"),
            ],
        },
    },
});
```
### STEP 2: 
Create two **tailwind.config.js** files for frontend and backend.

#### file 1: frontend-tailwind.config.js
```javascript
/** @type {import('tailwindcss').Config} */
module.exports = {
    content: [
      "./resources/**/*.blade.php",
      "./resources/**/*.js",
      "./resources/**/*.vue",
    ],
    theme: {
      extend: {},
    },
    plugins: [],
  }
  ```
#### file 2: backend-tailwind.config.js
```javascript
/** @type {import('tailwindcss').Config} */
module.exports = {
    content: [
      "./resources/**/*.blade.php",
      "./resources/**/*.js",
      "./resources/**/*.vue",
    ],
    theme: {
      extend: {},
    },
    plugins: [],
  }
  ```
### STEP 3:
Now open **package.json** and add follwoing two scripts
```json
"scripts": {
    "dev": "vite",
    "build": "vite build",
    "build:backend": "vite build --config vite.backend.config.js",
    "build:frontend": "vite build --config vite.frontend.config.js"
},
```
### STEP 4:
Open **/routes/web.php** file and create follwoing two routes
```php
Route::get('/dashboard', function(){
    return view('backend-dashboard');
});
Route::get('/frontend', function(){
    return view('frontend-interface');
});
```
### STEP 5:
Create tow **blade.php** views template in **/resources/views**

#### file 1: frontend-interface.blade.php
```html
<!DOCTYPE html>
<html lang="en">
<head>
    {{
        Vite::useBuildDirectory('/frontendAssets')
    }}
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <meta http-equiv="X-UA-Compatible" content="ie=edge">
    <title>Document</title>
    @vite('resources/frontend/frontend-tailwind.css')

</head>
<body>
    <div style="width:50%; margin:100px auto">
        <h1>FrontEnd Interface</h1>
        <button class="backend" style="padding: 10px; font-size:30px">
            red button for frontend
        </button>
    </div>
</body>
</html>
```
#### file 2: backend-dashboard.blade.php
```html
<!DOCTYPE html>
<html lang="en">
<head>
    {{
        Vite::useBuildDirectory('/backendAssets/dashboard')
    }}
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <meta http-equiv="X-UA-Compatible" content="ie=edge">
    <title>Document</title>
    @vite('resources/backend/dashboard/backend-tailwind.css')

</head>
<body>
    <div style="width:50%; margin:100px auto">
        <h1>backend dashboard</h1>
        <button class="backend" style="padding: 10px;font-size:30px">
            blue button for backend
        </button>
    </div>
</body>
</html>
```
### STEP 6:
Now crate a **/resources/backend/dashboard/backend-tailwind.css** file and paste follwoing code.
```css
@tailwind base;
@tailwind components;
@tailwind utilities;

.backend{
    @apply bg-blue-500;
}
```
Crate one more **/resources/frontend/frontend-tailwind.css** file and paste follwoing code.
```css
@tailwind base;
@tailwind components;
@tailwind utilities;

.frontend{
    @apply bg-red-500;
}
```
### STEP 7:
so we have setup a demo laravel app, just run follwoing **command in terminal** and run you project.
```
npm run build:backend
npm run build:frontend
```

## License

The software licensed under the [MIT license](https://opensource.org/licenses/MIT).
