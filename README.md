# Guide to Finding and Deleting Space-Consuming Media in Google Photos

This guide walks you through using the [Google Photos Toolkit (GPTK)](https://github.com/xob0t/Google-Photos-Toolkit) to find and delete space-consuming media from your Google Photos library. GPTK, developed by [xob0t](https://github.com/xob0t), utilizes Google Photos' web API for bulk organizing and managing media.

## Features

- **Find space-consuming media**: Easily identify large media files taking up space in your library.
- **Bulk delete media**: Quickly delete multiple items from your library to free up space.
- **API access**: Advanced users can use the toolkit's API for more powerful operations.

## Installation

To get started, you need a userscript manager for your browser. Here are some recommended options:

- [Violentmonkey](https://violentmonkey.github.io/)
- [Tampermonkey](https://www.tampermonkey.net/)

If you're using Android, you can install [Tampermonkey on Firefox](https://addons.mozilla.org/en-US/firefox/addon/tampermonkey/) as it supports the required functionality.

After installing a userscript manager, [click here to install the Google Photos Toolkit](https://github.com/xob0t/Google-Photos-Toolkit/releases/latest/download/google_photos_toolkit.user.js). Follow the prompts to complete the installation.

## Usage

### Starting the Toolkit

Once GPTK is installed, simply open [Google Photos](https://photos.google.com/), and the toolkit will be automatically injected at the top of the Google Photos webpage. You can now access the toolkit's features directly from the site.

### Finding Space-Consuming Media

Follow these steps to locate large media files in your Google Photos library:

1. In the GPTK interface, select **Library** as the source.
2. Use the **Space filter** and choose **SPACE-CONSUMING**.
3. To organize these items, select **Add to new album** to group all large media files in one album for easy management.

### Bulk Deleting Media

To delete multiple media items in your Google Photos library:

1. Select **Library** as the source in the GPTK interface.
2. Click **Move to trash**.
3. Finally, go to your trash and permanently delete the media to clear up space.

## Advanced Usage: Using the GPTK API

For those comfortable with JavaScript, GPTK exposes its API for more advanced functionality. You can access it through your browser’s console.

Here's an example to find and delete media shared by a specific user:

```javascript
let nextPageId = null;
const ownerName = "John";
do {
  const page = await gptkApi.getItemsByUploadedDate(nextPageId);
  for (const item of page.items) {
    if (item.isOwned) continue;
    const itemInfo = await gptkApi.getItemInfoExt(item.mediaKey);
    console.log(`${item.mediaKey} is shared by ${itemInfo.owner.name}`);
    if (itemInfo.owner.name == ownerName) {
      await gptkApi.moveItemsToTrash([itemInfo.dedupKey]);
      console.log(`${item.mediaKey} moved to trash`);
    }
  }
  nextPageId = page.nextPageId;
} while (nextPageId);
console.log("DONE");
```

This script allows you to scan the library for media shared by a specific user and move those items to the trash.

## Credits
This guide has been created for personal use to easily access instructions on using the Google Photos Toolkit, as it can be hard to search for this specific guide using traditional search engines. I have no intent of claiming ownership of the tool or project, and all credits go to the real owner, xob0t. For more advanced features or updates, please refer to the official repository.

```
Disclaimer: This tool leverages Google Photos' undocumented API, and usage is at your own risk. It is not affiliated with or endorsed by Google.
```
