---
layout: project
title: "Dialogue System"
subtitle: "A node based approach to dialogue trees using the GraphView API."
cover: ../assets/projects/DialogueSystem/cover.PNG

intro: |
  This is a dialogue system that I created through following [this](https://www.youtube.com/watch?v=nvELzBYMK1U&list=PL0yxB6cCkoWK38XT4stSztcLueJ_kTx5f) tutorial series from Indie Wafflus. After finishing the tutorial, I iterated upon the base and added a ton of data features, as well as a front-end for actual use in my projects.
sections:
  - image: ../assets/projects/DialogueSystem/node.PNG
    text: |
      ## Nodes
      The entire project centers around nodes within Unity's GraphView API. These nodes serve as the individual dialogue boxes in a game. They originally just contained text, a node name (different than speaker name), and connections to other nodes. I added lots of data to actually make this usable, including: a sprite, speaker name, side of the screen, audio clips, and a callback feature which is used in the game for various events.
      
      Node's data had to be saved via a complex nested scriptable object (SO) system. The save system would look at each node in the graph, create a (SO) for that node, populate it with all of it's needed data, then associate it with the next node. All of those nodes had to be saved to a serialized dictionary which was referenced by the main dialogue graph SO to actually display the text in order. This entire process is encapsulated to possible grouped nodes within a graph, which adds another layer of complexity.
    reverse: false
  - image: ../assets/projects/DialogueSystem/CustomInspector.PNG
    text: |
      ## Custom Inspectors
      To actually make this system easy to use, outside of just the GraphView, there needed to be custom inspectors created to allow designers to just drag a container in, and select the dialogue that they want to play when the container is activated via the frontend manager. Luckily, Unity's system for this is quite easy to use and is mostly just a series of if's and error detection. This DS Dialogue component you see here is useful because that is the dialogue that actually gets triggered by the front end, so designers could create one graph for an entire level, separate by groups, and then just play whichever one they want within those groups.
    reverse: true
  - image: ../assets/projects/DialogueSystem/UI.PNG
    text: |
     ## Frontend
     The frontend is mostly UI, coroutines, and event handlers. Dialogues can be invoked via a static event from anywhere, assuming they have a valid DSDialogue object to pass. This event pauses the game, activates the dialogue UI, and begins playing. Playing is handled by a coroutine which displays text at a given text speed, can stop on inflection points (periods, exclamations, etc.), and plays audio. It can be skipped through at any time.
      
     The actual great thing is the dialogue callback functionality. Any node can have a callback boolean set on it, this will tell the manager front end to invoke an event when this dialogue is passed through, therefore anything can listen for that specific node's event and respond to it. This allows us to have someone say something, then the game can actually respond, this makes tutorials great!
    reverse: false
conclusion: |
 Before this project I was very anti-YouTube tutorials, I thought they were very bad at actually teaching you. This one proved me wrong, I learned so much about tools programming, Unity APIs, and weird saving algorithms. I also was really happy to work on this because while it was something players won't see, my teammates love me for it! This really ignited my interest in tools programming and I would love to keep making anything that can help my future games see their full potential.
---