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
  - image: ../assets/projects/CorDraconis/buildings.png
    text: |
     ## Tween Abuse
     Should put some stuff about building design here.
     blaksjdf;olkahsd
    reverse: false
  - image: ../assets/projects/ProjectPromotion/TaskBoard.PNG 
    text: |
      ## Project Management
      Here is a devlog I wrote for the pre-production part of the project during Fall 2024:
      
      [View the Cor Draconis Devlogs →](/projects/CorDraconis/devlog/)
    reverse: true
conclusion: |
 Some text about the game again to close it out, what I learned
 blhaslkdf;laksdj
---