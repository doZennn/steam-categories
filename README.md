# steam-categories
Super basic Node.js library to help with modifying the new Steam categories

```
npm i steam-categories
```

Notes:
 - Launching Steam while the database is still in use will cause Steam to delete it. So use `close()` when you're done.
 - `read()` will throw an exception if you try to access the database whilst Steam is open.  
 Make sure to catch it or use `isClosed()` / `isOpen()`

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
  // Get collection "test"
  const testCollection = cats.get('test');
  console.info(testCollection);
  /*
  {
    key: 'user-collections.test',
    timestamp: 1573477978,
    value: {
      id: 'test',
      name: 'Test Collection',
      added: []
    }
  }
  */

  // Add DOOM to "test" collection
  testCollection.value.added.push(379720);

  // Change the name
  testCollection.value.name = '👀';

  // Create a new collection with Counter-Strike, DOOM, and Half-Life 2
  cats.add('someUniqueKey', {
    name: 'Super Cool Collection',
    added: [10, 220, 379720]
  });

  // Save collections
  cats.save().then(() => {
    console.info('yay!');
    // Close the database when you're done
    cats.close().then(() => {
      console.info('Database closed, safe to open Steam again.');
    });
  });
}).catch((err) => {
  // Error reading database (most likely locked)
  console.error(err.message);
});
```
