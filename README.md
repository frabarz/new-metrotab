# The current status on new metroTab <img src="icon_128.png" align="right" alt="" width="128">

If you got here through the listing for new metroTab on the Chrome Web Store (or any other replica), you probably want to know how to fix it, or if there are any plans to do it soon.

> **TL;DR**: no. I can't do anything to fix the published extension, because the listing in the Chrome Web Store it's not mine anymore. But I can still help.

I transferred the extension at the end of 2013, and with that, the extension was removed from my dashboard. That agreement also made me forfeit all the additional development assets and private keys, so there's nothing I can do directly to get it back, and I can't make a copy and continue development either. I mean, I can, but the extension is not mine anymore and the counterpart can come one day, claim it, and we're back to the beginning.

However, the extension still keep most of its original code, so I think I can offer some support through the Issues of this repository.

I still have the intention to make a better new tab page extension (because I use Firefox, and its new tab page can do better), so you can keep an eye on this github account. This time I'll make it open source too, and try to set it with a better permission structure. Looking at the amount of permissions it requests, if I didn't knew what does it have inside, I'd probably not install it.

## How to backup my tiles?

The main concern in the reviews right now is this. new metroTab stores the tiles in the browser's indexedDB for the extension, so we can retrieve them from there.

> The following procedure is running a external script in the context of the page. Please don't do this kind of procedure in other sites like facebook or your email website if you don't **exactly, line per line**, know what the code you're using does.

Follow these steps to download the tile backup file:

1. Go to the options (Hover on the user badge > Options)
2. Open a context menu (Right click) anywhere on the options page, and select the "Inspect" option. It's usually the last one. The options screen should split and some code should appear.
3. In this new area, click on the "Console" tab.
4. Paste this code:

```js
MT.miniDB.cursor().then(tiles => {
  const blob = new Blob([JSON.stringify(tiles, null, 2)], {type: "application/json"});
  const anchor = document.createElement("a");
  anchor.href = window.URL.createObjectURL(blob);
  anchor.download = "metrotab_tiles.json";
  anchor.click();
});
```
5. Press enter to run the code. After pressing enter, the browser's download prompt should appear. Save that file.

## What about the other options?

Follow the same procedure, but run this other snippet:

```js
(function() {
  const config = {
    "instalado": localStorage.getItem("instalado"),
    "bloques_info": localStorage.getItem("bloques_info"),
    "apar_bgcolor": localStorage.getItem("apar_bgcolor"),
    "apar_bgimg": localStorage.getItem("apar_bgimg"),
    "livet_clima_ubic": localStorage.getItem("livet_clima_ubic"),
    "livet_da_include": localStorage.getItem("livet_da_include"),
    "livet_zonahora": localStorage.getItem("livet_zonahora"),
  };
  document.querySelectorAll('.autosave').forEach(node => {
    const value = localStorage.getItem(node.name);
    value && Object.defineProperty(config, node.name, {value});
  });

  const blob = new Blob([JSON.stringify(config, null, 2)], {type: "application/json"});
  const anchor = document.createElement("a");
  anchor.href = window.URL.createObjectURL(blob);
  anchor.download = "metrotab_config.json";
  anchor.click();
})();
```
