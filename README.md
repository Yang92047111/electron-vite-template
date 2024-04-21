# Electron-vite-tempalate

An Electron application with Vue.

This repository will help me memo what I learn about electron.

And I will build the point cloud viewer with this template. 

## Recommended IDE Setup

- [VSCode](https://code.visualstudio.com/) + [ESLint](https://marketplace.visualstudio.com/items?itemName=dbaeumer.vscode-eslint) + [Prettier](https://marketplace.visualstudio.com/items?itemName=esbenp.prettier-vscode) + [Volar](https://marketplace.visualstudio.com/items?itemName=Vue.volar)

## Project Setup

### Install

```bash
$ npm install
```

### Development

```bash
$ npm run dev
```

### Build

```bash
# For windows
$ npm run build:win

# For macOS
$ npm run build:mac

# For Linux
$ npm run build:linux
```

## Memo

### Lifecycle

```bash
# pre-process
will-finish-launching -> ready -> did-become-active

# main process
create-window -> browser-window-created -> ready-to-show
                                            whenReady()

# close
window-all-closed
quit()
```

`app.whenReady()`
Returns `Promise<void>` - fulfilled when Electron is initialized. May be used as a convenient alternative to checking `app.isReady()` and subscribing to the ready event if the app is not `ready` yet.

`app.quit()`
Try to close all windows. The `before-quit` event will be emitted first. If all windows are successfully closed, the `will-quit` event will be emitted and by default the application will terminate.

This method guarantees that all `beforeunload` and unload event handlers are correctly executed. It is possible that a window cancels the quitting by returning `false` in the `beforeunload` event handler.

### IPC main & IPC render

#### Pattern 1: render process to main process
```bash
# renderer -> main
electron.window.ipcRenderer.send() -> ipcMain.on()

# main -> renderer
ipcMainEvent.sender() -> window.ipcRenderer.on()
ipcMainEvent.reply()
```

electron.window.ipcRenderer.send(`channel`, ...args)
- `channel` string
- ...args any[]

ipcMain.on(`channel`, `listener`)
- `channel` string
- `listener` Function
    - event IpcMainEvent
    - ...args any[]

IpcMainEvent Object extends Event
- `processId Integer` - The internal ID of the renderer process that sent this message
- `frameId Integer` - The ID of the renderer frame that sent this message
- `returnValue any` - Set this to the value to be returned in a synchronous message
- `sender WebContents` - Returns the webContents that sent the message
- `senderFrame WebFrameMain Readonly` - The frame that sent this message
- `ports MessagePortMain[]` - A list of MessagePorts that were transferred with this message
- `reply Function` - A function that will send an IPC message to the renderer frame that sent the original message that you are currently handling. You should use this method to "reply" to the sent message in order to guarantee the reply will go to the correct process and frame.
    - channel string
    - ...args any[]

ipcRenderer.on(`channel`, `listener`)
- `channel` string
- `listener` Function
    - event IpcRendererEvent
    - ...args any[]

## Reference

- [Electron official](https://www.electronjs.org/docs/latest/api/app)

## TODO

- Point Cloud Viewer
