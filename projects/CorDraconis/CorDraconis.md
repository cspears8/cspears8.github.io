---
layout: project
title: "Cor Draconis"
subtitle: "A fantasy city-builder with RPG elements."
cover: ../assets/projects/CorDraconis/cover.png

intro: |
  Cor Draconis was my final project with WolverineSoft at the University of Michigan. It started as a nuclear power plant simulator, which I had thought of because I had recently been watching through The Simpsons. But once I realized that I didn't understand how nuclear physics worked we shifted to everyone favorite MacGuffin: Magic! This is where it shifted into a more city builder, RPG, fantasy type game. We ran with a preproduction prototype developed by 3 people which successfully pitched to full production.
sections:
  - image: ../assets/projects/CorDraconis/Dungeon.PNG
    text: |
      ## Dungeon Generation
      The biggest feature that I developed on this project was the dungeon generation system for our questing mechanic. The questing mechanic was how you obtained the main progression items in the game: Dragon Fossils, so it was pretty important to get this right. One of my teammates, Kennedy, had come up with a more barebones system, but at the time I was really interested in graphical tree structures as well as procedural generation, so I took it upon myself to grow it into something more random.
      
      The structure was basically a binary tree of rooms with a maximum depth as well as a maximum number of steps needed to reach the boss at the end. These rooms were populated with either treasure, traps, monsters, or nothing semi-randomly. Each room type had a weight and there was also a maximum number of monster rooms possible. Dungeons were different and more challenging throughout the game, so I could scale the difficulty as needed, but I created a test command that I could use to see if an average set of adventurers could get through the dungeon, using Monte Carlo.
      
      Once I had all the numbers figured out, it was just a matter of changing the art/text output per dungeon, and adding in the UI. My initial idea for the dungeons were that it would be like a classic dungeon crawler. I think we got close with the perspective and the generation, but we ran out of time to implement more decision making on the part of the player.
    reverse: false
  - image: ../assets/projects/CorDraconis/BuildMenu.PNG
    text: |
      ## Build Menu
      One of the very first tasks I had on this project was to create some UI and a backend for selecting, purchasing, and placing buildings down. I started with basic Unity buttons in week two, just to understand how the hex A* works that my teammate, Gavin, wrote. Once that was working I programmed a basic UI which allowed the player to select a specific building out of a few options that we had. Each building had all their own data encapsulated via Scriptable Objects that the button callback passed into it's UI function for displaying all the information (cost, description, name, image, etc.). 
      
        We ran with that system for quite awhile, I added a couple of features such as hooking it up with our main UI Manager for easy closing and opening. Once we got started in production though it was very obvious it needed a facelift, so we got new art and I had to refactor the menu to actually look pretty with scaling, layout groups, etc.
    reverse: true
  - image: ../assets/projects/CorDraconis/buildings.png
    text: |
     ## Buildings
     Being a city builder, there had to be multiple different types of buildings which served different purposes. So as designer, I took time to design 5 different buildings in the game all with 5 levels. Buildings included: a farm which boosted production of towers around it, wizard towers which increased the possible size of your town, and smithies which could craft gear for adventurers as well as produce gold. 
      
     To figure out costs and upgrade costs, I created a small exponential scaling function within Desmos. Each resource (magic, wood, stone) had it's base cost as well as a scaling cost which were all multiplied by the level of the upgrade. This allowed some upgrades to start out with heavy wood cost but then scale more towards another resource. This felt a lot better than just arbitrarily guessing on what felt right for costs, it also accounted for increased production the player would have throughout the game.  
    reverse: false
  - text: |
      ## Devlogs
      Here is a devlog I wrote for the pre-production part of the project during Fall 2024:
      
      [View the Cor Draconis Devlogs →](/projects/CorDraconis/devlog/)
    reverse: true
conclusion: |
 In the end, I don't think Cor Draconis turned out the way I had envisioned. We had a really talented group of people, but getting them familiar with and onboarded with the idea of what we wanted was very difficult. City builders and western RPGs are pretty niche. We also failed to account for lost time throughout the semester due to our group trip to GDC which ate into a lot of production time.

 With all that said, it was a ton of fun working on this project, there were a ton of really interesting systems that we created. I just think the end project was really fragmented as a result. I learned a lot about preproduction, pitching, leadership, and systems programming which I will always be able to take with me. I am also extremely grateful for my time at WolverineSoft, I would not have gone down the same road I am on if it was not for the experience I had there.
---