# y2 - An experiment in using Yarn 2 with Yarn 1 defaults

Building off some of my notes in [yarn-vs-npm](https://orta.io/notes/js/yarn-vs-npm), I wondered out loud what a version of Yarn 2 looks like where it's deployed like a normal npm package, and has defaults which align with traditional node projects.

This is the result of that. It's a fork of [berry](https://github.com/yarnpkg/berry) ([here](https://github.com/orta/berry/tree/y2)) forked from [2da8101](https://github.com/yarnpkg/berry/commit/2da810140af64e07a7c94d368ad5c937f7373cb0) which defaults to the loosest resolution mode, and uses node_modules.

Usage: 

```sh
npm install -g y2

mkdir y2k
cd y2k

y2 init
y2 add --dev typescript
y2 tsc

tree . -L 1
.
├── README.md
├── node_modules
├── package.json
└── yarn.lock
```

### Note

Alternative take: For a single project you can use `yarn set version berry && yarn config set nodeLinker node-modules` to have the yarn command default to v2, and node_modules are default too. This is OK, but it means you have to keep opting in to use berry.

### Building

Get a CLI build created in berry, then copy it into the vendor folder.

```sh
cd berry
yarn build:cli

cd ..
cp berry/packages/yarnpkg-cli/bundles/yarn.js vendor
```

### Prod

Be careful with this, it's definitely safe to say `y2` is not battle tested. My specific changes:

- I switched `nodeLinker` to `"node_modules"` and `pnpMode` to `"loose"` by default, this means you will just get a `yarn.lock` and `node_modules`
- I switched all package names from `@yarnpkg/x` to `@orta/yarn-` (this probably does not matter, because `y2` runs with a bundled copy of yarn.js)
- I removed the default patches which are normally applied to `fsevents`, `resolve` & `typescript` ( I _believe_ these only exist for PNP support )
- I turned off the telemetry

### License

What little code in here I made it MIT. The vendored copy of [Yarn 2 is BSD 2-Clause](https://github.com/yarnpkg/yarn/blob/master/LICENSE).

### Note

Worth stressing, this isn't a _"Yarn 2 sucks"_ project. I'd like to use Yarn 2, this allows me to give it a shot at using it as my primary dependency manager for a while.
