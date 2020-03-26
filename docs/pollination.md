# Pollination
Pollination is Spore's system for sharing creations.

It includes a number of different systems, and one of the main goals of this project is to recreate most, if not all of them.

---

## Basic concepts
### Assets
Assets are the technical name for Spore creations. The purpose of Pollination is sharing assets.

[Read more...](assets)

### Pollen Metadata
Each asset has an associated Pollen Metadata, which stores basic information about the asset, such as its name, ID, author, description, tags, etc...

[Read more...](pollen_metadata)

### Spore Model
The model stores the actual parts and paints for an asset.

[Read more...](model)

### Asset ID
Each asset on the server has a unique 12-digit ID.

This is the most common type of ID to use. When using Spore.com or the Spore API, you will typically use Asset IDs to identify assets.

Player-made assets typically begin with 5, while Maxis creations may begin with 3. Asset IDs are created server-side, and downloaded into the game client as needed.

[Read more...](asset_id)

---

## Pollinator server
The Spore server is called the Pollinator.

For public Spore API documentation, see here: [http://www.spore.com/comm/samples]

### Pollinator
The game internally uses the Pollinator API to talk to the server. The game and server communicate over HTTP. The server typically provides either XML data, or Atom feeds.

Public data, such as Sporecasts, are sent over HTTP, and do not require login (although this is not enabled in the game). Other data, such as your own creations, are sent over HTTPS, and do require login.

Players do not interact with the Pollinator API directly. Instead, the public Spore API allows players to retrieve data from the Pollinator. Both XML data and Atom feeds can be returned.

### Static
Assets are not delivered directly by the Pollinator. Instead, the Pollinator provides a URL to `static.spore.com`, where asset PNGs and XML models can be retrieved, using asset IDs.

This API is publically accessible, as part of the public Spore API.

### Community
The public Spore.com website, and the webpages displayed in the in-game Sporepedia (MySpore page, Search online, user profiles, achievements), are accessed through `community.spore.com`.