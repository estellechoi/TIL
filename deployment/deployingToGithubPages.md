# How to deploy a React app to Github Pages

> If you don't have your own Github Pages hosted, see [here](https://pages.github.com/).

<br>

## Install gh-pages

```
npm install gh-pages
```

<br>

## Set `homepage` url

```
'homepage': 'https://estellechoi.github.io/{project-name}'
```

Here, the `{project-name}` has to be the the Github repository name of the project you are deploying. Additionally, make sure that every letter written here is in lowercase.

<br>

## Build the project

```
npm run build
```

This may run the building script, `react-scripts build`. Just check it out in your `package.json`. As soon as building done, you get a folder named `build`, or something like this, in your project directory. Now, let's get it deployed using `gh-pages` installed before.

<br>

## Create another script to deploy the project

```
'scripts': {
    'deploy': 'gh-pages -d build'
}
```

Add a script for deployment like the above. `-d` option means "directory" and `build` is the directory name of the built project. Surely, typing `npm run delpoy` gonna work.

<br>

You might need another script, as following, which plays a role of handling the prerequisites for the `deploy` command to be executed eventually.

```
'predeploy': 'npm run build'
```

By naming the script `predeploy`, note the `pre` prefix, it will be executed previously, by default, before the execution of `deploy` command so you meet the prerequisites, like that the folder of built project must exist, spontaneously.

<br>

---

### References
