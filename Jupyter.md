# Getting Started with Jupyterlab

Basic steps on getting started with the Jupyterlab project. These instructions
are based on working on Linux, but should work similarly on a Mac.

## Basic Installation

### JupyterLab

* Clone the forked repository: [JupyterLab](https://github.com/diagram-codesprint/jupyterlab)
* Follow the instructions on the [contribution page](https://github.com/jupyterlab/jupyterlab/blob/master/CONTRIBUTING.md).
* Run a server with
  ```jupyter lab --dev-mode --watch```
  - `--dev-mode` makes sure that you are seeing the current codebase
  - `--watch` automatically recompiles JupyterLab as you make changes
* Connect to the server in your browser

### Phosphor

* Clone the forked repository: [Phosphor](https://github.com/diagram-codesprint/phosphor)
* Run `jlpm install` in the cloned directory. ___(Do not use `npm install`!)___
* Run `jlpm run build`
* If you want to make changes in Phosphor, make sure to link to your local copy with
  ```jlpm link```
* Make sure you go into the right `phosphor` sub package. E.g., do

```bash
cd phosphor/packages/messaging
jlpm link
cd back to root of JupyterLab 
jlpm link @phosphor/messaging
```

## Where to make changes:

Let's do some changes to the code to see if things are working.

### In Jupyterlab

* Go to `jupyterlab/packages/launcher-extension/src/index.ts`
* Change the line
  ```JavaScript
  launcher.title.label = 'Launcher';
  ```
  to
  ```JavaScript
  launcher.title.label = 'My Launcher';
  ```
  and save the file.
* If you launched JupyterLab with `--dev-mode --watch`, it should automatically rebuild. If not, you'll need to build JupyterLab manually with `jlpm run build`.
* Reload page in the browser

You should now see a launcher with a `My Launcher` label.

### In Phosphor

* Link `widgets` package from phosphor:

  ```bash
  cd phosphor/packages/widgets
  jlpm link
  cd -
  jlpm link @phosphor/widgets
  ```

* Go to file `phosphor/packages/widgets/src/menubar.ts`
* Find the class `MenuBar` (line 45).
* In the constructor after the super class call `super({ node: Private.createNode() });`
  add the following line:
  ```JavaScript
  this.node.setAttribute('style', 'color:red; background-color:blue');
  ```
* Rebuild `phosphor` with `jlpm run build`
* Rebuild `jupyterlab` with `jlpm run build` _(Note you have to rebuild both for changes to take effect)_
* Reload the page in your browser. You should now see the menubar in blue with
  red font.


## Some Observations

Some random observations that might help you during the hackathon

* The initial build can take quite long (several minutes). Subsequent builds are
  incremental and should be quite fast.

* If you are working on `phosphor` you have to rebuild both `phosphor` and
  `jupyterlab`. So you might want to keep two consoles open with the build
  commands or use a little script like this:

```bash
pushd phosphor
jlpm run build
popd
pushd jupyterlab
jlpm run build
popd
```

* `jlpm` syntax is pretty much the same as `npm` syntax (`jlpm` is actually just a packaged version of `yarn`). _Make sure to use `jlpm` instead of `npm`!_

* Your build is dependent on where it had been built. If you move directories
  you have to rebuild from scratch.

* Once your local 'jupyterlab' server is launched it should never be necessary
  to restart it if your changes are purely TypeScript. Reloading in the browser should be sufficient.
