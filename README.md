# Lerna-Release-Prototype

## Intro
This is a prototype for testing out Lerna.js releases. It can be fully tested, by using a local **NPM Registry** (Docker-based Verdaccio).

## Setup
You need to have Node and Docker installed.

1. install deps `npm install`
1. run local test registry `npm run start:registry`
1. create a test user for publishing `npm adduser --registry http://localhost:4873`

