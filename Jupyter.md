# Getting Started:


## Basic Installation

### JupyterLab

* Clone the forked repositories: JupyterLab, Phosphor
* Follow the instructions on the [contribution page](https://github.com/jupyterlab/jupyterlab/blob/master/CONTRIBUTING.md).
* Run a server with
```jupyter lab --dev-mode```
* Connect to the server in your browser

### Phosphor

* Clone `phosphor`.
* Run `jlpm install` in the cloned directory. ___(Do not use `npm install`!)___
* If you want to make changes in Phosphor, make sure to link to your local copy with
  ```jlpm link```
* Make sure you go into the right `phosphor` sub package. E.g., do

```
cd phosphor/packages/messaging
jlpm link
cd -
jlpm link @phosphor/messaging
```

## Where to make changes:

Let's do some changes to the code to see if things are working.

### In Jupyterlab

* go to `jupyterlab/packages/launcher-extension/src/index.ts`
* Change the line
```
launcher.title.label = 'Launcher';
```
to
```
launcher.title.label = 'My Launcher';
```
* build again with `jlpm run build`
* (___not strictly necessary!___) relaunch the server with `jupyter lab --dev-mode`
* reload page in the browser

You should now see a launcher with `My Launcher` label.

### In Phosphor

* link `widgets` package from phosphor:

```
cd phosphor/packages/widgets
jlpm link
cd -
jlpm link @phosphor/widgets
```

* go to file `phosphor/packages/widgets/src/menubar.ts`
* find the class `MenuBar` (line 45).
* in the constructor after the super class call `super({ node: Private.createNode() });`
  add the following line:
  ```
  this.node.setAttribute('style', 'color:red; background-color:blue');
  ```
* rebuild `phosphor` with `jlpm run build`
* rebuild `jupyterlab` with `jlpm run build` _(Note you have to rebuild both for changes to take effect)_
* reload the page in your browser. You should now see the menubar in blue with
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

* `jlpm` syntax is pretty much the same as `npm` syntax. _But make sure to use
  the former and not the latter!_

* Your build is dependent on where it had been built. If you move directories
  you have to rebuild from scratch.
