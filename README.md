# Package Management

## Intro
* in my [JS talks](https://github.com/numical/jsacademy), I noted that node is the [most productive environment I have worked in](./Why%20Node.odp)
* a major part, easy modularisation and sharing of code via packages
* no more than `npm import package-x` and `require('package-x')`
* npm **massive** library of packages
* *building on the shoulders of many, many midgets*
* having said all of this then why [this](https://www.youtube.com/watch?v=M3BM9TB-8yA&t=913s) ?
* this talk is about that 'afterthought' - a deep dive into node package management


## What is Package Management
1. access a library of them (and most normally copy)
2. make available to dynamic runtime mechanism (metadata)
3. handle dependencies (was going to say predictably, but see later...)

## npm
* a CLI tool
* a library
* a company

## Dependency Management a la Node
### Theory + History
* [Directed Acyclic Graphs](https://en.wikipedia.org/wiki/Directed_acyclic_graph
* cannot map to a tree structure
* rely on an algorithm  
* [maven](https://maven.apache.org/guides/introduction/introduction-to-dependency-mechanism.html) in java  
* npm 1 & 2 - [deep tree](http://npm.github.io/how-npm-works-docs/npm2/how-npm2-works.html)
* [require algorithm](https://nodejs.org/api/modules.html#modules_all_together)
* despite ['wonderfully symbiotic'](http://npm.github.io/how-npm-works-docs/theory-and-design/the-node-module-loader.html)
* actually a pigs ear
* race condition in implementation, and simply not work in windows
* npm 3 - [flatter tree](http://npm.github.io/how-npm-works-docs/npm3/how-npm3-works.html)
* [non-determinism](http://npm.github.io/how-npm-works-docs/npm3/non-determinism.html) not a problem! Just switch it off and on again.
* then came yarn 1.x in 2016
    * deterministic and [reliable](https://engineering.fb.com/2016/10/11/web/yarn-a-new-package-manager-for-javascript/)
    * local cache 
    * lockfile  (npm 5 - 2017 - previously `shrinkwrap`)
    * workspaces  (npm 7 - 2020/[21](https://github.com/npm/roadmap/projects/1)) < monorepos!
* back to theory:
  * additional, spurious edges on ACD
  * [phantom dependencies](https://rushjs.io/pages/advanced/phantom_deps/)
    * monorepo only - phantom node_modules
  * [doppelgangers](https://rushjs.io/pages/advanced/npm_doppelgangers/)
    * no longer singletons
    * even more frightening stuff for TypeScript  (who would want to use that anyway)
* attempts to solve DAC impedence mismatch by:
  * [ied](https://www.npmjs.com/package/ied) - experimental, db-like
  * yarn 2.x (2020)
    * [a trainwreck of biblical proportions...?](https://ilikekillnerds.com/2020/08/yarn-2-2-update-released-but-is-anyone-even-using-yarn-2-yet/)
    * [plug'n'play](https://next.yarnpkg.com/features/pnp) - git rid of `node_modules` completely
    * takes over the require algorithm  
    * oh dear ... [when compatibility tables are needed...](https://next.yarnpkg.com/getting-started/migration)
  * [pnpm](https://pnpm.js.org/)
      * a different approach to npm2's problems
      * uses cleverly structured symlinks

## Monorepos
* aka multi-package repositories
* religious wars
* [for](https://rushjs.io/pages/intro/why_mono/)
    * cascading publishing - [example](https://confluence.devops.lloydsbanking.com/pages/viewpage.action?spaceKey=CPJ&title=How+to+update+FPR-UI+Projects)
    * atomic commits
    * downstream victims (e.g. auth middleware)(CODEOWNERS process)
* [against](https://blog.nrwl.io/misconceptions-about-monorepos-monorepo-monolith-df1250d4b03c)
    * [trunk based development](https://trunkbaseddevelopment.com/)
        * _Monorepos and long-lived feature branches do not play together nicely_
    * CI

* it's not just about code collocation
* but definite advantage of package management.
* let define the problem - UI order


### MonoRepo Tools
* [Lerna](https://lerna.js.org/)
* [RushJS](https://rushjs.io/) - superb documentation, lot of this talk from there


## Future Directions
* deno
  * 'radically simplified' [https://blog.logrocket.com/deno-1-0-what-you-need-to-know/](./Deno.png)
  * its own package manager based on es6 modules ([... a new npm repo?](https://www.skypack.dev/))
  * `import ${URL}` - relative or absolute, local cache
* snowpack (front-end only)
  * not a package manager, rather an alternative/compliment to webpack, parcel etc.
  * [unbundled development](https://www.snowpack.dev/concepts/how-snowpack-works) uses natibe browser esm support 
  * [v3.x - streaming imports](https://www.snowpack.dev/posts/2021-01-13-snowpack-3-0) - (optionally) no npm at all!

## Practical
* add package.json to git: that is the ***** point!
    * [npm ci](https://docs.npmjs.com/cli/v6/commands/npm-ci)
    * yarn --frozen-lockfile
* minimize dependencies
    * array.flatten > tree-shaking great in theory but practice (webpack link..?)  **<< TBD**
    * [bundlephobia](https://bundlephobia.com/) - e.g: react vs react-model vs react-router-dom
* spot phantoms / potential doppelgangers
    * API project loads
    * [eslint import plugin](https://www.npmjs.com/package/eslint-plugin-import)
    * [depcheck](https://github.com/depcheck/depcheck)
* unix-philosophy
    * mashup small tools
    * avoid [jest](https://bundlephobia.com/result?p=jest@26.6.3) and go for [mocha](https://bundlephobia.com/result?p=mocha@8.3.2) + [testdouble](https://bundlephobia.com/result?p=testdouble@3.16.1)


