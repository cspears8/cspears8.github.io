---
layout: project
title: "Project Promotion"
subtitle: "A tactics game about beating up your boss."
cover: ../assets/projects/ProjectPromotion/cover.PNG

intro: |
  Some text about ProjectPromo should go here.
  Extra text to see a new line.

sections:
  - image: ../assets/projects/ProjectPromotion/CTB.PNG
    text: |
      ## CTB System
      The entire core of Project Promotion revolves around the long forgotten Conditional Turn Battle system pioneered by Final Fantasy X. CTB allows for dynamic gameplay where each action taken by a player will make them take their turn later in the turn order. Within the first 2 weeks of preproduction I had created a system for this using a MinHeap which used an integer to sort and compare units' positions in the turn order.

      This system was augmented throughout development to include not only characters, but also any generic Action which can be used by abilities or environments to invoke events at given spots in CTB. In terms of UI we switched around a few times from linear horizontal cards, to a line showing how far apart entries were which used a coroutine based tick system to move entries, then finally back to cards, but with vertical UI.

      Near the end of development I had noticed that the entire way that I had built the system using a MinHeap was wrong and that I was changing values of entries without taking them out of the queue. For this to work easier and for auto-sorting to be accomplished I switched the system to use a SortedSet and popped values and reinserted them each time I needed to change values. Shockingly this fixed a lot of the bugs in the game and felt no less efficient than using the MinHeap. The extra complexity of keeping up with the MinHeap was not worth the bugs it caused.
    reverse: false

  - image: ../assets/projects/ProjectPromotion/AStar.PNG
    text: |
      ## A* Pathfinding
      The very first thing I programmed on this project was an implementation of the A* pathfinding algorithm. I had a bit of experience with hex tile based A* on Cor Draconis, but I didn't actually program the core system, I just used it. For Project Promotion I implemented rect tile based A*. The program was quite simple at the beginning, but due to being at the core of our system there were a lot of augmentations to it throughout development.

      In week one I followed a basic tutorial which implemented the math behind it. This laid the ground work for me and resulted in a base tile script which generated ground tiles at runtime on a tilemap with a custom OverlayTile script to store A* information, a pathfinder which runs the A* according to each tile's G and H values (similar to Dijkstra's), a MapManager to actually spawn the OverlayTiles according to a tilemap, and a RangeFinder to calculate unit ranges.

      This core structure worked for the majority of our 2D prototype, but once we switched to 3D environments there was a lot that needed changing. Overlay tiles were being generated as sprites which had to be switched to quads. We started using a package called AutoTiles3D to build tilemaps out of 3D objects, this helped the MapManager to figure out where to place OverlayTiles. The part that gave me the most trouble though was switching from the 2D isometric which uses XZ but automatically parses it as XY, to 3D isometric which must be manually converted to XY from XZ. It took me a long time to figure that out, but once I did the system largely translated perfectly to 3D which was great news.
    reverse: true

  - image: ../assets/projects/ProjectPromotion/Tweens.PNG
    text: |
      ## Tween Abuse
      This project was my first real deep dive into using the DOTween package in Unity. I've used it before for small things, but in this project we really amped it up for giving clarity to all the different UI that we needed. Some examples include glowing image borders, bouncing UI elements, flashing different elements, running timers, and coolest of all: creating text effects.
      
      I created this script which makes any given text in the game grow and translate along a sine and cosine wave respectively. This creates this really cool wave effect, but I wasn't done with that. I also allowed for the text to change color within the tween, so it gives this great rainbow effect. This was all achieved using just basic trig. I even added some settings for speed, scale, and to turn the color effect off for text that we just wanted to scale. I really love making effects like these and can get caught up in it sometimes since it's so immediately rewarding.
    reverse: false

  - image: ../assets/projects/ProjectPromotion/TaskBoard.PNG
    text: |
      ## Project Management
      During preproduction as well as full production I served as the producer/project manager on top of my roles as a systems programmer. In preproduction we handled task assignments and documentation using a combination of tools, mainly Notion. Our professor required us to use Burndown Charts, which due to how agile our development was we thought was pretty inefficient. So we relied heavily on Notion for simple tasks and updates.
      
      However, during the end of our pitch cycles we ran into an issue with Notion, we ran out of free blocks! I had tried contacting Notion to get premium, but they would not budge. Since the premium was so expensive I had to spend the summer tooling around with different software that we could use for production materials. We considered Trello, old school Burndown Charts, Jira, and Obsidian. The issue I had with most of these was that they were very one use, I didn't want my team to have to have 4 different programs open to look at production materials. I also could not afford to pay for anything. So we settled for Obsidian with a bunch of plug-ins to allow me to make tasks, tags, and Kanban boards. Obsidian is very powerful, but it was hard to get others on board due to it's complexity.
      
      I ran production by having weekly and sometimes biweekly sprints, we would spend classes on Tuesday discussing the project as a team and I would write tasks accordingly. Then on Thursday I would check in with each team member individually, seeing where they were at, if they had any questions for me or anyone else, etc. Most of the work would get done over the weekend though. I think it went pretty well, but I realized that creating tasks was more for me than it was for anyone else, it was so I could know what other people SHOULD be working on generally.
      
      In terms of things that could've been better, I did a poor job of scheduling regular playtests, which resulted in a lot of bugs slipping through undetected. I also should have done a better job at scheduling meetings with my few team leads to get a general feel on timelines. Near the end of the semester I started running playtests on Tuesdays and that helped a lot for us to see where the game was at during a given week.
    reverse: true

conclusion: |
  Some text about the game again to close it out, what I learned
  blhaslkdf;laksdj
---
