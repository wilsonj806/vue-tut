# Starting a Vue project
## Making a Project
There's two ways to start a new project using the Vue CLI:
```
  vue create
```

Or using the GUI
```
  vue ui
```

And then navigating through the GUI.

Both of the above basically does what Create React App does, adds dependencies and whatever else you need to start building a Vue app.

## Files
File structure is pretty much the same as CRA, but the Babel config is exposed for customization and some of the stuff in the `src` directory is different.

First, note how every vue component/ file has the `.vue` extension on it and gets turned into JavaScript by whatever Vue compiler thing, they've got running.

And second, we have `main.js` which serves as the entrypoint for Webpack to bundle our app from. Basically `index.js` in purpose, just a different name

## Running the app
We can run the app in a couple of ways:
- through the GUI
- NPM scripts

Both do the same thing, but the GUI adds some extra metadata/ performance data that might be really nice.
