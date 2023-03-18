# DesignSystem

## Instalation

1. Install angular cli global
   `npm install -g @angular/cli`

2. Create library project with scss `ng new angular-storybook --create-application=false`

3. Enter in project `cd angular-storybook`

4. Generate library `ng generate library ui-design`

5. Install storybook on our library `npx sb init`

6. First it is necessary to make a fix in the tsconfig.json file in the .storybook folder.

```json
"extends": "../projects/ui-design/tsconfig.lib.json",
  "compilerOptions": {
    "types": ["node"],
    "allowSyntheticDefaultImports": true
  },
  "exclude": [
    "../src/test.ts",
    "../src/**/*.spec.ts",
    "../projects/**/*.spec.ts"
  ],
  "include": ["../stories/**/*", "../src/**/*", "../projects/**/*"],
  "files": ["./typings.d.ts"]
}
```

7. When creating a project Angular with the flag --create-application=false Storybook is not able to recognize the location of the components, so you have to do it manually. In the angular.json file of our project

   ```json
   {
     "$schema": "./node_modules/@angular/cli/lib/config/schema.json",
     "version": 1,
     "newProjectRoot": "projects",
     "projects": {
       "ui-design": {
         "projectType": "library",
         "root": "projects/ui-design",
         "sourceRoot": "projects/ui-design/src",
         "prefix": "ngx-ui",
         "architect": {
           "build": {
             "builder": "@angular-devkit/build-angular:ng-packagr",
             "options": {
               "project": "projects/ui-design/ng-package.json"
             },
             "configurations": {
               "production": {
                 "tsConfig": "projects/ui-design/tsconfig.lib.prod.json"
               },
               "development": {
                 "tsConfig": "projects/ui-design/tsconfig.lib.json"
               }
             },
             "defaultConfiguration": "production"
           },
           "test": {
             "builder": "@angular-devkit/build-angular:karma",
             "options": {
               "main": "projects/ui-design/src/test.ts",
               "tsConfig": "projects/ui-design/tsconfig.spec.json",
               "karmaConfig": "projects/ui-design/karma.conf.js"
             }
           }
         }
       },
       "storybook": {
         "projectType": "application",
         "root": "stories",
         "sourceRoot": "stories",
         "architect": {
           "build": {
             "options": {
               "tsConfig": "tsconfig.json",
               "styles": [],
               "scripts": []
             }
           }
         }
       }
     },
     "defaultProject": "ui-design"
   }
   ```

8. Change our prefix in design system

   ```json
   ...
   "prefix": "ngx-ui",
   ...
   ```

## Storybook

1. Remove all storybook files created by default in stories folder

2. Remove all the files from the lib folder except the ui-design.module.ts

3. Remove imports from public-api.ts except `export * from './lib/ui-design.module';`

4. Create components folder inside lib folder

5. Create our first component `ng g c components/button`

6. Add button component on exports

7. Create button stories.ts `Button.component.stories.ts`

8. Launch storybook `yarn storybook`

9. Optional

- Add info on package.json inside ui-design library

  ```json
  ...
  "license": "MIT",
  "author": {
      "name": "jadelma",
      "url": "https://github.com/jadelmag"
  }
  ...
  ```

## Build to test our library

1. Run `ng build` to build the project. The build artifacts will be stored in the `dist/` directory.

- **Important!!** Import necessary modules in `ui-design.module.ts` and all components in public-api.ts

```json
export * from './lib/ui-design.module';
export * from './lib/components/button/button.component';
```

2. Go inside dist folder after build and run `npm pack`.

- path: ...\dist\ui-design\ npm pack

3. Create other project and install our package. `npm i ...\dist\ui-design\ui-design-0.0.1.tgz`

## Build and Publish

1. Run `ng build` to build the project. The build artifacts will be stored in the `dist/` directory.

2. Go to dist folder `cd dist/ui-design`

3. And publish `npm publish`

**!!Important!!** You must be logged on npm
