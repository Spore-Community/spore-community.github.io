# Pollen Metadata
Pollen Metadata is a format used to store metadata for Spore assets (creations).

The following is included in Pollen Metadata:
- Name
    - The asset's name.
- Author
    - The username and user ID of the author.
- Asset ID
    - All Spore assets have a 12-digit ID, uniquely identifying them on the server. [Read more...](asset_id)
- Description
    - Asset's description, as set in the editor.
- Tags
    - Asset's tags, as set in the editor.
- Consequence traits
    - The "traits" assigned to a creature at the end of the first four stages, and at the start of the Space stage.
- Timestamp
    - The time the asset was saved, in seconds since AD 1.
- Type
    - The type of asset.
- Parent Asset ID
    - If the asset was edited, the ID of the "parent" asset is stored, to track lineage.
- Other IDs
    - Group ID and Instance ID uniquely identify the asset within your own game's files.
    - Machine ID, believed to identify you or your computer. Purpose unknown.

Note that parts and paints are not stored in this data. That data is stored in the [Spore Model](model) format.

---

## Use in Spore PNGs
This metadata is included inside Spore PNG data, as a header, prefixing the Spore Model XML.

The metadata has the following format:

Length | Type | Name | Description
--- | --- | --- | ---
5 | String | | Always "spore". Simply identifies the data as being from Spore.
4 | Int | Metadata version | 5 for Creature Creator. 6 for full game (adds support for Parent assets). No other versions have been seen.
8 | Hex | Type ID* | Type of asset.
8 | Hex | Group ID* | ID of asset's folder inside `editorSaves.package`. Corresponds to asset type.
8 | Hex | Instance ID | ID of asset inside `editorSaves.package`.
8 | Hex | Machine ID | Constant for user/machine? Purpose unknown.
16 | Hex | Asset ID | Uniquely identifies this asset on the server. May be null (-1) if offline.
16 | Hex | Parent ID | Indicates the "parent" asset's ID, if this creation was edited. Used for lineage. Only present if version is 6.
16 | Hex | Timestamp | Time when asset was saved. Seconds since AD 1.
2 | Hex | | Length of author's username.
\* | String | Username | Author's username. May be computer username if offline.
16 | Hex | User ID | Uniquely identifies user on the server. May be null (-1) if offline.
2 | Hex | | Length of asset's name.
\* | String | Name | Asset's name, as set in editor.
3 | Hex | | Length of description.
\* | String | Description | As set in editor. NOT updated if the description is changed on Spore.com.
2 | Hex | | Length of tags.
\* | String | Tags | As set in editor. NOT updated if the tags are changed on Spore.com.
2 | Hex | | Number of consequence traits (stages completed). Will be 0 for non-creatures.
\*8 | Hex | Consequence Traits* | An 8-length hex ID for each consequence trait this creature has. Only present for creatures.

\*These 8-length hex values are hashes. Use [SporeModder FX](https://emd4600.github.io/SporeModder-FX/)'s Utilities tab to convert hashes into names.

Most other hex values should be converted to decimal values. Windows Calculator (Programmer mode) or [SporeModder FX](https://emd4600.github.io/SporeModder-FX/) (Utilities tab) can do this.

### Examples
#### Creature example
```spore00062b978c4640626200182183d70190b84d00000074a91fa01900000074a91fa0130000000ed18afdbf09DOGC_Kyle0000007476707e6505Kylon036From Planet Kylia. Part of the Spore Multiplayer Game.1aset:dogckylon, multiplayer0217e5ef84cfb01b93```

Name | Value | Description
--- | --- | ---
"spore" | "spore" |
Metadata version | 0006 | This asset is from the full game.
Type ID | `2b978c46` | This is a creature.
Group ID | `40626200` | This asset is in the `creature_editorModel~` folder in `editorSaves.package`.
Instance ID | `182183d7` | This asset has an ID of `182183d7` inside the `creature_editorModel~` folder.
Machine ID | `0190b84d` | Purpose unknown.
Asset ID | `00000074a91fa019` | Converts to `501,053,628,441`. Can be used to find this creation on the server: <http://www.spore.com/sporepedia#qry=sast-501053628441>
Parent ID | `00000074a91fa013` | Converts to `501,053,628,435`. Can be used to find parent creation on the server: <http://www.spore.com/sporepedia#qry=sast-501053628435>
Timestamp | `0000000ed18afdbf` | Converts to `63,645,089,215‬`. This can be [converted](https://www.epochconverter.com/seconds-days-since-y0#s1) to a date and time: Tuesday, October 31, 2017 11:26:55 PM
Username Length | `09` | Converts to `9`. The author's username is 9 characters long.
Username | `DOGC_Kyle` | This creation was made by *DOGC_Kyle*.
User ID | `0000007476707e65` | Identifies DOGC_Kyle on the server. Rarely used.
Name Length | `05` | Converts to `5`. The creation's name is 5 characters long.
Name | `Kylon` | The creation's name is *Kylon*.
Description Length | `036` | Converts to `54`. The description is 54 characters long.
Description | | The creation has the following description: *"From Planet Kylia. Part of the Spore Multiplayer Game."*
Tags Length | `1a` | Converts to `26`. The tags are 26 characters long.
Tags | | The creation has the following tags: *"set:dogckylon, multiplayer"*
Traits Length | `02` | This creature has completed two stages, and therefore has acquired two consequence traits.
Traits | `17e5ef84` & `cfb01b93` | These are IDs of the two consequence traits this creature has: `crg_attack` (Predator) and `clg_meat` (Carnivore).

---

## Use in Package Files
A similar format is used inside of various Spore package files (DBPFs/database packed files), including `Spore_Content.package` (Maxis creations) and `editorSaves.package` (creations made locally).

The metadata for an asset is in its own file, with an extension of `.pollen_metadata`. `.pmet` is also believed to be a valid extension. A hex editor is needed to view and edit the data.

The metadata has the following format:

WIP
