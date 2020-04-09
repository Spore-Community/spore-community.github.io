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

### PNG Examples
#### Creature example
`spore00062b978c4640626200182183d70190b84d00000074a91fa01900000074a91fa0130000000ed18afdbf09DOGC_Kyle0000007476707e6505Kylon036From Planet Kylia. Part of the Spore Multiplayer Game.1aset:dogckylon, multiplayer0217e5ef84cfb01b93`

Name | Value | Description
--- | --- | ---
"spore" | "spore" |
Metadata version | 0006 | This asset is from the full game.
Type ID | `2b978c46` | This is a tribal creature.
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

The metadata has the following format for player assets:

Length | Name | Description
--- | --- | --- | ---
4 | Version? | Unknown. Could be a version, could be a length. If lower than 7 or greater than 13, game discards file. Maxis uses 10, player creations use 13.
8 | Asset ID | Uniquely identifies this asset on the server. May be null (-1) if offline.
4 | Type ID* | Type of asset.
4 | Group ID* | ID of asset's folder inside `editorSaves.package`. Corresponds to asset type.
4 | Instance ID | ID of asset inside `editorSaves.package`.
4 | Parent Type ID* | Type of parent asset.
4 | Parent Group ID* | ID of parent asset's folder inside `editorSaves.package`. Corresponds to asset type.
4 | Parent Instance ID | ID of parent asset inside `editorSaves.package`.
8 | Parent ID | Indicates the "parent" asset's ID, if this creation was edited. Used for lineage.
8 | Original Parent ID | Indicates the oldest, original "parent" asset's ID, if this creation was edited. Used for lineage.
8 | Timestamp | Time when asset was saved. Seconds since AD 1.
8 | Timestamp | Time when asset was saved. Seconds since AD 1. Unknown why there are two.
4 | Assembled Content? | Unknown. For player creations, all zeroes. For Maxis creations, -1.
8 | User ID | Uniquely identifies user on the server. May be null (-1) if offline.
4 | | Length of author's username.
\* | Username | Author's username. May be computer username if offline.
4 | | Length of asset's name.
\* | Name | Asset's name, as set in editor.
4 | | Length of description.
\* | Description | As set in editor. NOT updated if the description is changed on Spore.com.
4 | | Number of next data (usernames)?
\*4 | | Length of next data (username)?
\*\* | Username | Author's username. Purpose unknown.
4 | | Number of tags.
\*4 | | Length of tag.
\*\* | Tag | As set in editor. NOT updated if the tags are changed on Spore.com.
4 | | Unknown. -1 (null).
4 | | Number of consequence traits (stages completed). Will be 0 for non-creatures.
\*4 | Consequence Traits* | A hex ID for each consequence trait this creature has. Only present for creatures.

For Maxis creations included with the game (Assembled Content) in `Spore_Content.package`, the fields for User ID, Username, Name, and Description, are replaced with three fieldsL

Length | Name | Description
--- | --- | --- | ---
4 | Localization file | The ID of the localization file in `Text.package`.
4 | Author ID | Hash inside localization file. 1 for Maxis, 2 for Jessica, 3 for Micheal.
4 | Name ID | Hash inside localization file. Used to retrieve asset name.

\*These hex values are hashes. Use [SporeModder FX](https://emd4600.github.io/SporeModder-FX/)'s Utilities tab to convert hashes into names.

Most other hex values should be converted to decimal values. Windows Calculator (Programmer mode) or [SporeModder FX](https://emd4600.github.io/SporeModder-FX/) (Utilities tab) can do this.

Compared to the format used in PNGs, the data is in a different order, and the Machine ID is not present. Tags are split up (not a single string). Also, there is extra data for locating the parent asset in `editorSaves.package`, and also an asset ID for the oldest parent asset.

### Package File Examples
#### Creature example
`00 00 00 0D 00 00 00 74 A9 1F A0 19 2B 97 8C 46 40 62 62 00 18 21 83 D7 2B 97 8C 46 40 62 62 00 18 21 83 B5 00 00 00 74 A9 1F A0 13 00 00 00 74 A9 1F 20 28 00 00 00 0E D1 8A FD BF 00 00 00 0E D1 8A FD BF 00 00 00 00 00 00 00 74 76 70 7E 65 00 00 00 09 44 00 4F 00 47 00 43 00 5F 00 4B 00 79 00 6C 00 65 00 00 00 00 05 4B 00 79 00 6C 00 6F 00 6E 00 00 00 00 36 46 00 72 00 6F 00 6D 00 20 00 50 00 6C 00 61 00 6E 00 65 00 74 00 20 00 4B 00 79 00 6C 00 69 00 61 00 2E 00 20 00 50 00 61 00 72 00 74 00 20 00 6F 00 66 00 20 00 74 00 68 00 65 00 20 00 53 00 70 00 6F 00 72 00 65 00 20 00 4D 00 75 00 6C 00 74 00 69 00 70 00 6C 00 61 00 79 00 65 00 72 00 20 00 47 00 61 00 6D 00 65 00 2E 00 00 00 00 01 00 00 00 09 44 4F 47 43 5F 4B 79 6C 65 00 00 00 00 00 00 00 02 00 00 00 0D 73 00 65 00 74 00 3A 00 64 00 6F 00 67 00 63 00 6B 00 79 00 6C 00 6F 00 6E 00 00 00 00 0B 6D 00 75 00 6C 00 74 00 69 00 70 00 6C 00 61 00 79 00 65 00 72 00 FF FF FF FF 00 00 00 02 17 E5 EF 84 CF B0 1B 93`

Name | Value | Description
--- | --- | ---
Asset ID Length | `00 00 00 0D` | Converts to `13`.
Asset ID | `00 00 00 74 A9 1F A0 19` | Converts to `501,053,628,441`. Can be used to find this creation on the server: <http://www.spore.com/sporepedia#qry=sast-501053628441>
Type ID | `2B 97 8C 46` | This is a tribal creature.
Group ID | `40 62 62 00` | This asset is in the `creature_editorModel~` folder in `editorSaves.package`.
Instance ID | `18 21 83 D7` | This asset has an ID of `182183d7` inside the `creature_editorModel~` folder.
Parent Type ID | `2B 97 8C 46` | The parent asset is a tribal creature.
Parent Group ID | `40 62 62 00` | The parent asset is in the `creature_editorModel~` folder in `editorSaves.package`.
Parent Instance ID | `18 21 83 B5` | The parent asset has an ID of `182183d7` inside the `creature_editorModel~` folder.
Parent ID | `00 00 00 74 A9 1F A0 13` | Converts to `501,053,628,435`. Can be used to find parent creation on the server: <http://www.spore.com/sporepedia#qry=sast-501053628435>
Original Parent ID | `00 00 00 74 A9 1F 20 28` | Converts to `501,053,595,688`. Can be used to find parent creation on the server: <http://www.spore.com/sporepedia#qry=sast-501053595688>
Timestamp | `00 00 00 0E D1 8A FD BF` | Converts to `63,645,089,215‬`. This can be [converted](https://www.epochconverter.com/seconds-days-since-y0#s1) to a date and time: Tuesday, October 31, 2017 11:26:55 PM
Timestamp | `00 00 00 0E D1 8A fD BF` | Same as last.
? | `00 00 00 00` | Unknown.
User ID | `00 00 00 74 76 70 7E 65` | Identifies DOGC_Kyle on the server. Rarely used.
Username Length | `00 00 00 09` | Converts to `9`. The author's username is 9 characters long.
Username | `44 00 4F 00 47 00 43 00 5F 00 4B 00 79 00 6C 00 65 00` | This creation was made by *DOGC_Kyle*.
Name Length | `00 00 00 05` | Converts to `5`. The creation's name is 5 characters long.
Name | `4B 00 79 00 6C 00 6F 00 6E 00` | The creation's name is *Kylon*.
Description Length | `00 00 00 36` | Converts to `54`. The description is 54 characters long.
Description | `46 00 72 00 6F 00 6D 00 20 00 50 00 6C 00 61 00 6E 00 65 00 74 00 20 00 4B 00 79 00 6C 00 69 00 61 00 2E 00 20 00 50 00 61 00 72 00 74 00 20 00 6F 00 66 00 20 00 74 00 68 00 65 00 20 00 53 00 70 00 6F 00 72 00 65 00 20 00 4D 00 75 00 6C 00 74 00 69 00 70 00 6C 00 61 00 79 00 65 00 72 00 20 00 47 00 61 00 6D 00 65 00 2E 00` | The creation has the following description: *"From Planet Kylia. Part of the Spore Multiplayer Game."*
Username Count | `00 00 00 01` | Converts to `1`. Indicates 1 username?
Username Length | `00 00 00 09` | Converts to `9`. The following username is 9 characters long.
Username | `44 4F 47 43 5F 4B 79 6C 65` | This is the author's username (*DOGC_Kyle*). Possibly used for search?
? | `00 00 00 00` | Unknown.
Tags Count | `00 00 00 02` | Converts to `2`. There are two tags.
Tag Length | `00 00 00 0D` | Converts to `13`. The first tag is 13 characters long.
Tag | `73 00 65 00 74 00 3A 00 64 00 6F 00 67 00 63 00 6B 00 79 00 6C 00 6F 00 6E 00` | The first tag is: *"set:dogckylon"*
Tag Length | `00 00 00 0DB` | Converts to `11`. The second tag is 11 characters long.
Tag | `6D 00 75 00 6C 00 74 00 69 00 70 00 6C 00 61 00 79 00 65 00 72 00` | The second tag is: *"multiplayer"*
? | `FF FF FF FF` | Unknown.
Traits Length | `00 00 00 02` | This creature has completed two stages, and therefore has acquired two consequence traits.
Traits | `17 E5 EF 84` & `CF B0 1B 93` | These are IDs of the two consequence traits this creature has: `crg_attack` (Predator) and `clg_meat` (Carnivore).