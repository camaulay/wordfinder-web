# wordfinder-web
[Live Demo](https://wordfinder-web.netlify.app)

Finds whole word matches of a given string with letters in any order using a specific [wordlist](https://gist.githubusercontent.com/adamc00/a45beb8a0cb55593220f749838c534d0/raw/fd11dcce5df4098839e19f21c50cc2363b9b7863/words.txt).

Permutations algorithm adapted from an implementation by [noam sauer-utley](https://levelup.gitconnected.com/find-all-permutations-of-a-string-in-javascript-af41bfe072d2).

To make up for my laziness thinking up this algorithm from scratch I implemented the algorithm in a webworks so calculating large permutations doesn't block the UI. You get a cute (lazy) loading spinner instead.

Enjoy!

## Requirements
- node & npm

## Run instructions
```
yarn install
```
```
yarn serve
```
App will be accessible at http://localhost:8080