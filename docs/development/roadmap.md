## Roadmap

Okay, the roadmap... I'm sure everyone wants to know what direction AO intends to go. These are mostly long-term goals; short-term goals are usually done on impulse and usually just involve putting things that already exist together.

### Finish the ui-files branch

This is a full revamp of the UI and network code. There will be some things that won't be refactored before release, such as asset loading, the character parser, and the viewport (which is essentially the core engine of the game).

This means that the UI will be much easier to customize and will scale well for large monitors, and it will be significantly easier to add new features.

Work was begun in December of 2018 and has been ongoing. Various UI portions are still in the process of being moved to the new code.

![Screenshot](https://cdn.discordapp.com/attachments/323377366997008394/698959173840535606/unknown.png)

As you may see, I am having trouble with color and layout choices - I'm a backend, not a frontend, developer. If you have designed a good theme before, please don't hesitate to reach out.

**In need of:** UX designer

### Add a real asset system

I've got plenty of specs for a unifying asset system with the following features:

- Allows for automatic asset downloading (both on-demand and before joining)
- Backwards compatible with older servers
- Doesn't prevent players from installing characters manually
- Doesn't pollute the hard drive with duplicate animations of similar characters

This will have to be implemented after the major refactoring is finished, however.

**In need of:** Cooperating server owners

### Redesign the core engine

The core engine is rather limited in a few ways:

- There are only a limited number of events:
  - Emotes, which are just preanimation + talking + idle, with a choice of side and sound effect.
  - Evidence display.
  - Interjections.
  - WT/CE.
- Only a fixed number of layers.

It has been overloaded, and events really need to be split into smaller sub-events using a timeline model.

Eventually, this will also require a major modification of the network protocol in order to be fully leveraged.

### Disconnect from the *Ace Attorney* franchise

Using Capcom assets is an immense legal liability. Distancing ourselves from the *Ace Attorney* franchise by pushing the legal burden of nonfree assets to server hosts and distributing the base game as 100% fan content opens business opportunities.

This is also an opportunity to publish the game on Steam as an original fan game. From my knowledge of marketing, I personally believe that adding a small price to the game may increase perceived value than if it were simply free, even if the game remained open-source. It also introduces a revenue stream that can be used to fund artists for original content. But as there are many moving parts to sales - and this implies the formation of a company, during a time when I would already be working a full-time software job - I would otherwise assume that it would enter Steam free.
