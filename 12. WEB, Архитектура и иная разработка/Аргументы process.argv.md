При запуске приложения из консоли можно передавать ему параметры, которые сохраняются в `process.argv` и доступны в приложении.
```js
const [nodePath, filePath, ...args] = process.argv;
```
Первым параметром всегда передаётся полный путь к NodeJS, вторым - полный путь к исполняему js-файлу. 
```cmd
node index.js test
```
```js
const mode = process.argv[3];
console.log(mode); // test
```
