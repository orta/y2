# y2 - An experiment in using Yarn 2 with Yarn 1 defaults

Building off some of my notes in [yarn-vs-npm](https://orta.io/notes/js/yarn-vs-npm), I wondered out loud what a version of Yarn 2 looks like where it's deployed like a normal npm package, and has defaults which align with traditional node projects.

This is the result of that. It's a fork of [berry](https://github.com/yarnpkg/berry) ([here](https://github.com/orta/berry/tree/y2)) forked from [2da8101](https://github.com/yarnpkg/berry/commit/2da810140af64e07a7c94d368ad5c937f7373cb0) which defaults to the loosest resolution mode, and uses node_modules.

Usage: 

```sh
npm install -g @orta/y2

y2 init
y2 add --dev typescript
y2 tsc
```

### Building

Get a CLI build created in berry, then copy it into the vendor folder.

```sh
cd berry
yarn build:cli

cd ..
cp berry/packages/yarnpkg-cli/bundles/yarn.js vendor
```

### Prod

Be careful with this, it's definitely safe to say `y2` is not battle tested. I have removed the built-in patches (they _should_ only be needed for PNP projects) 

### License

What little code in here I made it MIT. The vendored copy of [Yarn 2 is BSD 2-Clause](https://github.com/yarnpkg/yarn/blob/master/LICENSE).
