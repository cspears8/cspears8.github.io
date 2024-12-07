---
layout: default
title: "Sprint 4 Blog"
data: 2024-12-01
categories: blog
---
## Introduction

Well, I suppose this is it. The last sprint. This sprint has revolved around a last minute push to get in the rest of the features we need as well as polish for our final presentation.

## Week One

### Farm Implementation (5hr)
Since we had discussed the system in our last Discord meeting, I had a pretty good understanding of how I wanted to implement the farm into the game. I largely did not run into any big snags, I augmented the RangeManager script to calculate and assign tiles to a separate FarmRangeTilemap. This was basically done in the same way as the RangeTilemap, the only thing that required changing was the logic for destruction, instead of destroying all the buildings in the range, I just removed their productionMultiplier, also when the farm was built I set the productionMultiplier for that building. Currently, because we don't have upgrades in the game I just manually set the multiplier to be 4. This will definitely change when we get upgrades for the farm in, which will likely be a big challenge due to how we have set the range up. Other than that, the only problem that exists is that buildings which are in the range of two farms do not work great. Their productionMultiplier is not stacked, which may be good, may not be. Also, if one farm is destroyed the productionMultiplier is reset when it shouldn't. That will have to be fixed at a later date. 
Farms also spawn trees at a set rate (45 seconds), this was somewhat easy to develop but was done sort of dirty. I had to get reference to the ObjectTilemap which held the tree tiles, the only way I could get access to that was through a tag when the farm was spawned. This is because the farm script held the logic for spawning trees, but they are just prefabs so they couldn't hold the reference to the ObjectTilemap, even if the tilemap was made to be a prefab, not sure why to be honest. I also had to add a method to the RangeManager to get all the tiles within the range of a given farm, here is that code:
```csharp
 public List<Vector3Int> GetFarmRangeTiles(Farm farm){
    List<Vector3Int> rangeTiles = new List<Vector3Int>();
    int range = farm.range;

    Vector3Int centerCubic = HexUtils.OffsetToCubic(farm.offsetCoord);

    for (int radius = 0; radius <= range; radius++)
    {
        Vector3Int[] directions = new Vector3Int[]
        {
            new Vector3Int(0, 1, -1),   // Top-right
            new Vector3Int(-1, 1, 0),   // Top-left
            new Vector3Int(-1, 0, 1),   // Left
            new Vector3Int(0, -1, 1),   // Bottom-left
            new Vector3Int(1, -1, 0),   // Bottom-right
            new Vector3Int(1, 0, -1)    // Right
        };
        Vector3Int currentCubic = new Vector3Int(radius, -radius, 0);

        for (int i = 0; i < 6; i++)
        {
            for (int j = 0; j < radius; j++)
            {
                Vector3Int offsetCoord = HexUtils.CubicToOffset(centerCubic + currentCubic);
                if (farmRangeMap.HasTile(offsetCoord))
                {
                    rangeTiles.Add(offsetCoord);
                }

                currentCubic += directions[i];
            }
        }
    }
```
### Weekly Meeting #8 (2hr)
This was our last meeting of production before documentation and pitch deck creation took center stage. We discussed what was left, which honestly wasn't much. During testing we found a couple of bugs related to the farm and its range implementation, I was tasked with fixing those up. I also was tasked with art implementation as we had gotten the new dungeon art asset in, with that all we needed was the wizard tower and a couple brush-ups on the environment art. Connor, our director, told us due dates for the final deliverable and what we needed with them. We have a rough draft of the documentation due on 12/7, our finished prototype on 12/9, and the pitch deck on 12/15. It was time to finish this game!

## Week Two

### Farm Bug Fixes (1hr)
There were a couple small bugs with the farm, namely the range rings weren't being deleted all the way and the multipliers for multiple farms on a single building was wrong. The first fix was easy, I simply forgot to add the calculations for finding the radii of other farms in range, this resulted in tiles getting set to null even if they are in range of another farm. The code was also cleaned up by nesting the farm logic and the wizard tower logic into an if-else statement so unnecessary code wasn't being run. The second problem was also easy, I just changed the production corroutine in the ResourceManager (which every producing building is subscribed to) to check for that building's production amount ever cycle using a get method. This meant that the RangeManager could set the production of that building and the ResourceManager would update automatically.

### Art Implementation (30min)
This was a really self-explanatory task, Nadav finished up the dungeon art, so I put it in the game. Since structures can take up multiple tiles, but that can be hard to design for as an artist, we have a set size for all buildings and then the first tile of the structure is the sprite while the rest are blank tiles which just act as representations of it actually being made up of bigger tiles. Here is how that works, below that is the dungeon in game:

![Dungeon Structure]({{ site.baseurl }}/files/dungeonStructure.png){: .post-image}

![Dungeon In-Game]({{ site.baseurl }}/files/dungeonInGame.png){: .post-image}

### Main Menu Design (3hr)
I figured we should probably have some sort of basic main menu for demonstration purposes. Mainly just to outline what we will need in the future for the next semester. This is mostly just editor work where I made some canvas objects and buttons, designed some assets in Figma like arrows. The navigation between menus was quite simple, but making it pretty is a different task. I used a lot of vertical and horizontal layout groups to make sure that every asset was evenly spaced out and well organized for if someone else needed to polish it later. We don't have any assets so this is purely functional. I had to create a lot of card type objects which used some of my web design knowledge. Here's a screenshot of the menu describing the buildings:

![Building Info Menu]({{ site.baseurl }}/files/buildingInfoMenu.png){: .post-image}

### Total Time Spent: 12hr + 1hr for Devlogs