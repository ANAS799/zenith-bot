# 🛠️ Bug Fixes
### 🚀 All commands are now bug-free and operational!
Greetings everyone! 🌟 This marks the release of a beta version for our package. If you encounter any issues or need assistance, feel free to reach out. Direct message me on Discord at `elias79` (ID: 1130583393176920095).

# 🚫 Functions Removed and Deprecated

## ❌ Removed Functions

- The `initeHandler()` function is no longer available. To connect to Lavalink, include the required configuration in the `start()` function. Refer to the example and usage provided below.
- `processMessage()` has been deprecated and replaced with `pixelAI()`.

## ⚠️ Deprecated Function

- `mongoConnect()` is now deprecated and can be used in the `start()` function.

# 🚀 Getting Started
- If you're new and facing issues or unable to register and execute commands (slash / prefix).
- Use the code snippet below (Note: This may slightly increase bot latency by approximately 50ms).
```javascript
const { Client, GatewayIntentBits, Partials } = require("discord.js");
const { start } = require('zenith-bot'); 
const path = require('path');

const client = new Client({
  intents: Object.keys(GatewayIntentBits).map((a)=>{
    return GatewayIntentBits[a]
  }),
 partials: Object.values(Partials),
});

const token = process.env.token;
const eventsPath = path.join(__dirname, 'src', 'events');
const prefixCommandPath = path.join(__dirname, 'src', 'prefix');

const options = {
  status: 'online', // idle, online, dnd
  name: 'Playing with commands',
  type: 0, // type is for activity (Playing, Watching, Custom , Listening, etc.)
  botName: 'tools', // bot name
  botAvatar: './pfp.jpeg', // set your bot avatar as a URL or local file (GIF also works) 
  eventsPath,

  // Note: Don't provide guildId or put it in the options if you set global as true
  slashCommandPath: {
    baseDir: __dirname, // need to be provided to read your commands path
    path: './src/slash',
    global: true, // true for making slash commands global
    guildId: '', // guild if global is false
    logsId: '', // channel ID for commands log
  },
  prefixCommandPath: {
    path: prefixCommandPath, // your prefix commands path
    logsId: '', // channel ID for commands log
    prefix: '!', // prefix of the bot
  },
  lavalink: {
    // Lavalink configuration for music
    nodes: [],
    search: 'ytmsearch', // (ytmsearch) => YouTube musics || (spsearch) => Spotify musics
    version: 'v4', // your Lavalink version
  },
  mongo: {
    mognoURI: '', // Your MongoDB URI for connecting to MongoDB
    dataName: '', // Your Database name to create in MongoDB
  }
};

start(client, token, options);
```

# 🚀 What's New

## 🌟 Bot Enhancements

- Implemented a comprehensive bot handler for managing slash commands, prefix settings, MongoDB integration for database functionality, Lavalink for music streaming, and enhanced event handling for the client.

## 🎨 PaintImage Class Update

- Added a new option to the `PaintImage` class for background removal (`bgRemoval`).

## 🚀 Usage Guidelines 🤖

For a standard reply without AI images or voice messages, set both `draw` and `voice` to `false`. Activate voice replies and AI images by setting them to `true`. Trigger the bot to draw AI images with specific words or phrases using `drawTrigger`.

```javascript
// Use this for js files (cjs modules)
const { pixelAI } = require('zenith-bot');

// For es module (.mjs/(.js if "type": "module" in package.json)
import { pixelAI } from 'zenith-bot';

const allowedChannelId = 'Channel_ID';
client.on('messageCreate', async (message) => {
  if (message.author.bot || message.channel.id !== allowedChannelId) return;

  await pixelAI(message, {
    voice: false, // Enable voice replies
    draw: true, // Enable AI images
    drawTrigger: ['create', 'draw', 'imagine', 'ارسم'],
    imageModel: 'prodia', // Choose AI image generator
    chatModel: 'v3-32k', // Choose chat model
    keywords: ['bot name', 'your name', 'who are you'],
    keywordResponses: {
      'bot name': 'My name is Zenith, and I was developed by Elias79.',
      'your name': 'I am known as Zenith, created by Elias79.',
      'who are you': 'I am Zenith, an AI created by Elias79.',
    },
  });
});
```

## 📊 Database Connection 📁

To connect to MongoDB/Mongoose, provide your URL and name your database. Follow these steps:

```js
///for cjs module (.cjs/.js)
const { mongoConnect } = require('zenith-bot');
///for es module (.mjs/(.js if "type": "module" in package.json)
import { mongoConnect  } from 'zenith-bot';

// Connect to MongoDB
const mongoSuccess = await mongoConnect('mongoURI', 'dbName');

// Access connected databases using getDb functions
const mongoDb = getDb();

/*----------------------------------------*/
//if you wanted to use your database to save/edit/delete then use this example below

// Example usage:

const { save, updateData, find, remove, removeSpecific } = require('zenith-bot');

//example of saving a data (equivalent to insertOne)
  await save('collectionName', { key: 'value', key2: 'value2' });//save data into a collection

//example of updating an existing data and if not found insert it (equivalent to updateOne)
  await updateData('collectionName', { key: 'value' }, { $set: { key2: 'new-value' } }, updateData);//update the old key with new value or updating both

//example of finding a data (equivalent to findOne)
await find('collectionName', { key: 'value' });//search for the key that has this value

//example of remove a data (equivalent to deleteOne)
await remove('collectionName', { key: 'value' });//deletes the whole document with this current key and value

//example of remove specific data (equivalent to deleteOne but deleting a specific field from the data)
await removeSpecific('collectionName', { key: 'value' }, {key2: 'value'});

//example of remove many data (equivalent to deleteMany)
await removeMany('collectionName', { keyToMatch: 'someValue' });/// if u didnt put any key it will run with default (_id)

////removeMany while ignore specific documents has the value
const keysToDelete = 'yourField';
const keyToKeep = 'anotherField';

removeManyExcept('yourCollection', keysToDelete, keyToKeep);//removes whole data in collection except those with the key

//example of update many which updates many documents that has specifc key into a new value
const filter = { keyToMatch: 'someValue' }; // Replace with the appropriate filter
const update = { $set: { keyToUpdate: 'newValue' } }; // Replace with the update criteria

await updateMany('collectionName', filter, update);

//update many with exception to excule a key
const filter = { keyToMatch: 'someValue' }; // Replace with the appropriate filter
const update = { keyToUpdate: 'newValue' }; // Replace with the update criteria
const excludedKeys = ['keyToExclude1', 'keyToExclude2']; // Replace with keys to exclude

await updateManyExcept('collectionName', filter, update, excludedKeys);//update the whole documents in collection except those with specific key

//Example of transfer data from colection to another (functionality same as insertMany and find Array)
await transferData('sourceCollection', 'destinationCollection');

//Example of transfer data and delete ('sourceCollection')
await transferDataAndDelete('sourceCollection', 'destinationCollection');///after transferring data it deletes the source collection
```

## 📸 Manipulating Images Using Canvas 🎨

Add images, shapes, and effects to images with ease!

```js
//You can use import for es module/.mjs
const { PaintImage } = require('zenith-bot'); 


const paintImage = new PaintImage();


///Creating a background
const canvasOptions = {
  width: 1200,//background width
  height: 300,//background height
  backgroundColor: '#3498db', // Example color (blue) or use 'transparent' for no backgound color
  backgroundGradient: {////Adding  a gradient instead of constant color
    type: 'linear',/// change to 'radial' if you want gradient start from middle
    startX: 0,
    startY: 0,
    startRadius: 0,///add if radial only!  
    endRadius: 0,///add if radial only! 
    endX: 100,
    endY: 0,
    colors: [
      { stop: 0, color: 'red' },
      { stop: 0.5, color: 'green' },
      { stop: 1, color: 'blue' },
      ///add more colors if needed
    ],
  },
  borderRadius: 0,///adding a radius to create a curvy edges or use 'circular' to make it a circle
};


//// 
const images = [
  {
    source: 'square',///drawing  shapes as images. We only support those shapes ('square', 'triangle', 'circle', 'pentagon').
    x: 385,///positions on background horizontally.
    y: 225,///positions on background vertically.
    rotate: 0,//rotate only applyed for shapes.
    filled: true,///to fill the shape if true.
    width: 500,///shape width.
    height: 80,//shape height.
    borderRadius: 35,///adding a radius to create a curvy edges or use 'circular' to make it a circle.
    color: 'rgba(0, 0, 0, 0.4)',///only being applied for shapes (filling the shape with the provided color).
    gradient:, ////Adding  a gradient instead of constant color (used same way as in canvas (only works with shapes).
    stroke: {//Adding stroke
      color: '#498afc',//color of the stroke
      width: 5,//size of the stroke.
      borderRadius: 35,///adding a radius to create a curvy edges or use 'circular' to make it a circle.
    },
  
  },
    {
    source: 'square',
    x: 395,
    y: 235,
    rotate: 0,
    filled: true,
    width: 440,
    height: 60,
    borderRadius: 35,
   gradient: {
    type: 'linear',
    startX: 0,
    startY: 0,
    endX: 100,
    endY: 0,
    colors: [
      { stop: 0, color: 'red' },
      { stop: 0.5, color: 'green' },
      { stop: 1, color: 'blue' },
    ],
  },
  },
  {
    source: 'local/file/path || image url',//you can provide an image url or local path and for images styling you can use same as above in shapes except for rotate and gradient which won't work.
    x: 120,
    y: 120,
    width: 210,
    height: 210,
    borderRadius: 20,///adding a radius to create a curvy edges or use 'circular' to make it a circle.
    shadow: {
      color: null,//shadow color.
      offsetX: 0,///shadow position off from image horizontally.
      offsetY: 0,///shadow postion off  from image vertically.
      opacity: 0,//shadow opacity.
      blur: 0,//shadow blur.
      borderRadius: null,///adding a radius to create a curvy edges or use 'circular' to make it a circle.
    }
  }, 
];
////image already got buffered you can send it directly or add text option
  const image = await paintImage.drawImages(images, canvasOptions, __dirname);

const textOptionsArray = [
  {
    text: 'elias79. Rank',//adding a text.
    x: 440,//text horizontally positon .
    y: 145,//text vetically position.
    fontName: 'Wind',//name your font as you want or use default (Arial).
    fontPath: './wind.ttf',//font path for custom font.
    fontSize: 50,///size of the font.
    color: 'white',//font color.
    shadow: {
    offsetX: 3,
    offsetY: 3,
    color: "rgba(0, 0, 0, 0.5)",
    blur: 5
  },
    stroke: {
        color: '#deff08',
        width: 1.2,
      },
  },
   {
    text: '95/100 XP',
    x: 530,
    y: 250,
    fontName: 'Arial',
    fontSize: 35,
    color: 'white',
    stroke: {
        color: 'black',
        width: 0.5,
      },
  },
];
///Adding the text on the drawn image before
const buffer = await paintImage.addText(textOptionsArray, image, __dirname);

//sending the buffered image with text on it 
 await message.channel.send({files: [buffer] });
```
<details>
  <summary>Click to expand/collapse</summary>

  This is the content you want to hide or display.
</details>

## 📚 More Commands & Documentation 📖

Explore a detailed list of commands and their prefixes in our [Support Server](https://883733a4-7d26-4e57-8a01-3eed284b6519-00-3kn6npon8kc06.sisko.replit.dev/home/docs/intro/).

## 🚨 Important Notes 📌

1. **Voice Messages:** Currently in beta, occasional disruptions may occur.

2. **Commands:** Support for slash commands and database integration using MongoDB is coming soon.

3. **Music:** Utilizing Lavalink for audio streaming. Customize Lavalink node if needed.

4. **Database:** Still in beta. Aimed at simplicity and bug-free experience.

Keep experimenting, and feel free to contact me for assistance! Suggestions and adjustments are welcome. 🌟
