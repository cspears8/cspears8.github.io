---
layout: default
title: "Sprint 4 Blog"
data: 2024-11-23
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
### More to come!

### Total Time Spent: 5hr + 30min for Devlogs