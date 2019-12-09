# Lerna-Release-Prototype

## Intro
This is a prototype for testing out Lerna.js releases. It can be fully tested, by using a local **NPM Registry** (Docker-based Verdaccio).

## Setup
You need to have Node and Docker installed.

1. install deps `npm install`
1. run local test registry `npm run start:test-registry`
1. create a test user for publishing `npm adduser --registry http://localhost:4873`

## Release Flow 101
This prototype is for modelling the usage of a monorepo with the **Release Flow**[1] _branching model_. It basically boils down, to only using very _short-lived_ **topic** branches that are directly merged into **master**. The **master** branch is used for doing releases - in terms of NPM it will mean for publishing packages.

For implementing a feature you'd have to do the following steps:

1. _create_ a topic branch
1. actually implement a feature
1. (optionally) _release_ a **canary** version for _"others"_
1. merge-request it into master
1. upon approval it will get merged into master and your topic branch is deleted

Releases are always carried out from the **master** branch and involves in terms of a monorepo the following steps:

1. all _changed_ components, i.e. all components that have changed **since the last** release **must** be _bumped_ and then _published_
1. components that do **depend** on _other_ components (in the repo) need to be _bumped_ and _published_ as well

### Example: Modify a component that is not consumed by other internal components and release it
In this example we will modify a component, that is not depended on by other components. Its change should only impact itself.
1. create a topic branch: `npm run create-topic-branch topic/modify-b`
1. implement it (change it): `npm run modify:component-b`
1. merge-request and approve it: `npm run finish-topic-branch topic/modify-b && npm run delete-topic-branch topic/modify-b`
1. release it: `npm run publish-all`

Now you'd have to enter the version bump (major, minor, patch) and it will get _released_ (i.e. _published_)

### Example: Modify a component that is consumed by other internal components
In this example we will modify a component, that is depended on by other components. Its change should trigger new releases for other components as well.

1. create a topic branch: `npm run create-topic-branch topic/modify-a`
1. implement it (change it): `npm run modify:component-a`
1. merge-request and approve it: `npm run finish-topic-branch topic/modify-a && npm run delete-topic-branch topic/modify-a`
1. release it: `npm run publish-all`

Now you'd have to enter the version bump (major, minor, patch) for **both** components; **first** for the _modified_ component and **second** for the component, that is depending on it. **Both** will get _released_ (i.e. _published_).


[1]: https://docs.microsoft.com/en-us/azure/devops/learn/devops-at-microsoft/release-flow
