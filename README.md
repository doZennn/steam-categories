# steam-categories
Super basic Node.js library to help with modifying the new Steam categories

```
npm i steam-categories
```

Usage:
```js
const SteamCat = require('steam-categories');
const path = require('path');

// %LOCALAPPDATA%/Steam/htmlcache/Local Storage/leveldb
const levelDBPath = path.join(process.env['localappdata'], 'Steam', 'htmlcache', 'Local Storage', 'leveldb');

// Supply leveldb path and Steam3 ID of user whose collections to edit
const cats = new SteamCat(levelDBPath, '88089999');

// Read user collections
cats.read().then((collections) => {
  // Add DOOM to existing "test" collection
  collections[1]['user-collections.test'].value.added.push(379720);

  // Create a new collection with Counter-Strike, DOOM, and Half-Life 2
  cats.add('someUniqueKey', {
    name: 'Super Cool Collection',
    added: [10, 220, 379720]
  });

  // Save collections
  cats.save().then(() => {
    console.info('yay!');
  });
});
```
