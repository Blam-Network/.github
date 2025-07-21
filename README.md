# blf
This is a Rust based monorepo for handling "BLF" format files used by multiple versions of Halo. These files include user-generated content such as map variants, gametypes, screenshots and clips. It also includes over-the-wire data such as matchmaking playlists and statistics.
The blf repository consists of the following projects:
- blf_lib
  - Holds mappings for various blf chunks, the data units within a blf file. 
  - This library is built to handle multiple versions of Halo and provides a mechnism for registering chunk structures for a given game version.
- blf_cli
  - Holds functionality for managing blf_files via a command-line-interface. This is often used to build matchmaking playlist files. (see Blam-Title-Storage)
- blf_bnet
  - This is a web-assembly compiled package which can be used to provide blf file i/o functionality into a webpage, for example selecting a screenshot BLF file to see the JPEG image it contains within a web-page.

## blf_private
Similar to **blf**, this is another Rust monorepo for BLF files, though this one contains more sensitive functionality. Legacy halo games are lacking a lot of security practices expected of modern games, in lieu of a more secure platform, we rely partly on [security through obsurity](https://www.bcs.org/articles-opinion-and-research/security-through-obscurity/).
The blf_private repository consists of the following projects:
- blf_lib-private
  - Similar to blf_lib but with data structures relating to player statistics (eg. postgame stats).
- blf_lsp
  - Holds BLF oprations which are used by the **web_private** NodeJS web server.
- blf_lsp-npm
  - Configuration for building blf_lsp as a Node Native-API (NAPI) NPM package via [napi-rs](https://github.com/napi-rs/napi-rs) as of writing, a [fork of napi-rs](https://github.com/Blam-Network/napi-rs) is used soley to disable it's camel casing of generated bindings.

# web_private
Provides reimplemented web-features for legacy halo games with support for Halo 3, Halo 3: ODST, and Halo: Reach. Halo 3 support is very complete, Halo: Reach support is limited to Matchmaking.
This is a NodeJS web-api using the NestJS framework, written in TypeScript. Prisma ORM is used for database interactions and management, though the server can operate with a reduced feature set (matchmaking only) should the database become unavailable.
Reimplemented features include:
  - File Share
  - Matchmaking
  - Carnage Reports
  - User Data
  - Population Statistics & Map
The API includes OpenAPI documentation via Swagger which can be viewed by simply launching the server and navigating to /api.

# Blam-Title-Storage
Halo 3 & Reach matchmaking requires a lot of BLF configuration files including data like what matchmaking playlists are available, which gametypes do they include, the gametypes themselves etc. In addition, there's configuration for features like the message of the day, messages that appear should a user be banned, etc. There are many binary files to manage and many require hashes. We use **blf_cli** to build these configuration files from easier to read JSON, CSV or plain-text files, which are held in this repository.
Blam-Title-Storage holds configuration for Halo 3 TU2, Halo 3: ODST, and Halo: Reach TU1.

# Legacy Projects
- Content
  - An older version of Blam-Title-Storage which includes configuration for pre-release versions of Halo such as the Halo 3 Public Beta. This repo used to be built with the C# BlfTool, was created for research, and is unused.
- BlfTool
  - An older version of blf_lib / blf_cli which was written in C#. This project is mostly irrelevant, though it does hold some structures for pre-release halo versions such as the Halo 3 Public Beta which have not yet been added to blf_lib.
