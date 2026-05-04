---
layout: project
title: "Pinblast Neon"
subtitle: "Platforming Meets Pinball."
cover: ../assets/projects/PinblastNeon/Well.jpg

intro: |
  Itch Page: [https://cookieinferno.itch.io/pinblastneon](https://cookieinferno.itch.io/pinblastneon)  

  Pinblast Neon is a 2D precision platformer with pinball elements. The game combines the chaos of pinball with the precision of platformers to create a dynamic and unique play experience. This project was created in a semester as my capstone in the games program at Michigan State University. We are currently still in development with plans for release in December 2026!
sections:
  - image: ../assets/projects/PinblastNeon/PinballFirstDemo.PNG
    text: |
      ## Ideation Process
      At the first team meeting we had decided that we all wanted to make the most polished student game possible, trying to break the convention of what a student game constituted with the limited time we had. With varying ideas of what we wanted as a team, I had the team spend 1-2 weeks just prototyping ideas, with design docs, prototypes, slide shows, whatever they wanted. This created a very positive and open environment to share ideas. 
      
      With all those ideas we knew we wanted something that was clean, fast, and had a lot of tech in it, then a platformer was suggested, but with the added twist of pinball. The chaotic nature, bright lights, and satisfying sounds of pinball were very interesting to the team, so off we went from there with our vision laid out: A fast-paced 2D precision platformer where most of your movement comes from interacting with the objects in the world. 
    reverse: false
  - image: ../assets/projects/PinblastNeon/cannon.gif
    text: |
      ## Polish
      The big push for the team was to make this game look good, we used a couple of plug-ins to make this easier: DOTween and Feel. DOTween I have used before and is amazing for creating sequences of events, especially for UI. I used it to create effects like a win screen which counts up your points to show your rank in a satisfying way. Other use cases for DOTween included vignette fade outs, color flashes, and shader value interpolations.
      
      Feel was a new package that I really fell in love with throughout the project. By creating sequences of small effects such as screen shake, bursts of chromatic aberration and bloom, or scale springs, we could get a multitude of really nice looking effects into the game. A key example is shooting out of a cannon. As you can see here. There are effects for entering and shooting out which convey vital information to them, as well as a bit of slowdown to subconsciously get them to judge where the cannon is about to shoot them. Effects such as these are littered throughout the entire game which really helps the game stand out amongst student projects.
    reverse: true
  - image: ../assets/projects/PinblastNeon/splines.gif
    text: |
     ## Design Philosophy
     The team tackled this project with a very experimental design philosophy which benefited us greatly, despite some early confusion and worry. We started with a massive toolbox of mechanics: a dash, aerial movement, a parry, and time slow, on top of our environment interactions. We gave those mechanics opportunities to thrive to see if anything useful would come out of them. Most of them were cut eventually except for a dash and aerial movement, for both ease of control, as well as to emphasize the pinball aspect of interacting with the board more. This "try it out!" pipeline allowed many people on the team to feel able to prototype quickly, resulting in a really solid structure for the game at the end.
      
     We carried this design philosophy through to our level designs, with everyone on the team creating bite-sized rooms which tackled a new idea, from portals, to our dash, to falling down or climbing up. With feedback on all of those mini-levels we could narrow down our actual levels and focus them better on individual "gimmicks" that our players enjoyed. We went from about 8 of these ideas to 4 complete levels and ~15 minute experience.
    reverse: false
conclusion: |
 Pinblast Neon has been an absolutely wonderful experience, it has been smooth, fun, and rewarding to develop so far. After working on these semester-long student projects with fluctuating teams and different time tables, if there is one thing I have learned it's that you need to scope small! All of my previous projects I had hoped that I would have "just a bit more time!" to work on them, but Pinblast Neon feels like we could put it away as it was at the end of the semester and be done with it.

 Of course that's not what is happening! Development is on-going as we have applied to IndieCade 2027 which means we have a lot of work to do including more levels, art, and general polish! So be on the lookout for future updates and a Steam release in December 2026!
---