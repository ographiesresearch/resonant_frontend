# Resonant Frontend

[![Netlify Status](https://api.netlify.com/api/v1/badges/5656f08d-00e6-4eb6-96a3-c590a98ac876/deploy-status)](https://app.netlify.com/sites/elaborate-kitsune-f9e948/deploys) [![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT) [![Project Status: Inactive – The project has reached a stable, usable state but is no longer being actively developed; support/maintenance will be provided as time allows.](https://www.repostatus.org/badges/latest/inactive.svg)](https://www.repostatus.org/#inactive) [![DOI](https://zenodo.org/badge/678906871.svg)](https://doi.org/10.5281/zenodo.15366770)

<img width="1591" alt="Screenshot 2023-09-05 at 9 15 57 AM" src="https://github.com/ericrobskyhuntley/resonant_frontend/assets/10646361/5af1c24c-e47b-4e13-be42-7696b2e45b32">

## Netlify Deployment
https://elaborate-kitsune-f9e948.netlify.app/

This is the frot end for the Solar Low-Income Tax Credit Map, [currently deployed on Netlify](https://elaborate-kitsune-f9e948.netlify.app/). built using Svelte, SvelteKit, Node, Bulma, and Mapbox that can be easily customized using the fields in `src/config/instance.json`, as well as the layers and geomtries provided.

## Configuration

### Node
Assuming you have node installed, you can install all dependencies by running `npm install` from the root directory.

### MapBox Token
For the application to run, you'll need a MapBox token. The application expects it to be stored in a `.env` file in the root directory. E.g., this file should look like...

```
MAPBOX_TOKEN='your token here`
```

Important note: the token _will be exposed to the client_! As such, before deploying, you should make sure that your token is appropriately scoped and that it has URL restrictions in place. 

## Running
Run `npm run css-build && npm run dev` from the root folder. (On subsequent runs, you can simply run `npm run dev`. This starts a vite server (`vite dev`) and a node-sass watcher that watches for changes in `src/styles/style.scss` (the latter supports rapid Bulma development). 
