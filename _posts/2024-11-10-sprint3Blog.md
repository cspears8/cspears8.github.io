---
layout: default
title: "Sprint 3 Blog"
data: 2024-11-10
categories: blog
---
## Introduction

This sprint we have been working on refining what we already have and adding in the remaining mechanics for our prototype.

## Week One

### Camera Control Improvements (1hr)
After getting feedback about our camera controls, menu closing, and a couple game breaking bugs regarding the UI, I dove back in to make some revisions. A nice touch I added was multiple ways to control the camera, you can now control the camera by not only clicking and dragging but also by using either the arrow keys or the WASD keys. This was quite simple, just adding a couple input maps (as you can see below) and modifying the CameraManager script. The script modification was as easy as checking to see if the player was in the none camera state, if that was the case then the arrow/WASD inputs were checked. This ensured that the camera could not be dragged and moved with the keys at the same time.

![Updated UI Inputs]({{ site.baseurl }}/files/updatedUiInputs.PNG){: .post-image}

### UI Architecture Updates (2hr 30min)
Another piece of feedback, and big bug, we got was the fact that the menus could not be closed with a single button. They had to be clicked from their exit button which was confusing to some users. In order to fix this I was going to implement a CloseAllUI function/input on the escape key. Luckily our UI Manager script already had an event declared for such a purpose, the only problem was how it was implemented. At the time, each UI screen's script would define a callback for this event which closed all the menus, this worked for the old system of using the close buttons on each UI menu. This did not work for using a key to close all the menus, instead I had to define a callback within the UI Manager itself to close all the UIs. Sounds great, except for one thing, the UIs had been manually defined in the UI Manager as references to the scripts attached to the prefab GameObjects. This felt wrong, as whenever a new UI would be defined you would have to manually add it to the UI Manager and check it in the CloseAllUI as well as the switch statement for activating the UI. I tried to create a Dictionary with a String and reference to the prefab that would serve as the respective key-value pairs, this logic works, however, Dictionaries are not serializable types within the Unity Editor so you cannot add the building prefabs in the editor. In order to fix this I had the clever idea of defining a UIDictionaryManager script, this would dynamically build a static Dictionary of String and GameObjects that other scripts can reference, but not change. This is a good example of the object orientated paradigm of encapsulation, as well as good code practices for designers as their use of an IDE is limited to alter the gameplay. Here is the dictionary manager code:
```csharp
[Serializable]
public class UIKeyValuePair
{
    public string key;
    public GameObject value;
}

public class UIDictionaryManager : MonoBehaviour
{
    [SerializeField] private List<UIKeyValuePair> uiKeyValuePairs = new List<UIKeyValuePair>();

    public static Dictionary<string, GameObject> uiDictionary = new Dictionary<string, GameObject>();

    private void Awake()
    {
        uiDictionary.Clear();
        
        foreach (var pair in uiKeyValuePairs)
        {
            if (!uiDictionary.ContainsKey(pair.key))
            {
                uiDictionary.Add(pair.key, pair.value);
            }
        }
    }
}
```

### Wood Production (2hr)
A big design problem we've had is how the player should produce wood in the game. We have ways of producing the other resources, magic from wizard towers, gold from smithy and adventuring, and stone from the contractor. We have trees in the game so the original idea was the player cuts trees down to get wood, but then a problem arises where the player could possibly run out of wood and be stuck, unable to progress. Another problem with this design was that the player could just chop down all the trees in the game immediately and get all the wood, which wouldn't be very fun. So we had to restrict the player's access to wood while not allowing them to get stuck. The solution we came up with as a team was to limit the player to cutting trees down within the range of town. This incentivizes players to build their town out, and towards trees as well, which could be a nice way to get towns to go in a certain direction if a fog of war mechanic of some kind was implemented. To fix the problem of having a finite amount of wood, we decided to have farms produce trees at a slow rate in the radius of town, we thought this was fitting in lore too as farms would be run by druids. By having there be a slow amount of wood constantly, players would not get stuck but still would be incentivized to build out in order to reach large caches of wood.
In order to implement this, we had to define the OnDestroy for environment tiles, which trees were a part of. In order for this OnDestroy to be called, Gavin implemented a quick DestroyEnvironment method within the BuildingManager script which simply checks if the clicked tile is within range of town and invokes the OnDestroy for that tile. Trees are an enum of the EnvironmentTile class and so in the OnDestroy we just check if the passed EnvironmentTile from the BuildingManager is of type Tree. If it is them we destroy the tree and call the refund of the ResourceManager. Because the refundRate might not be 1, I set the woodAmount to be 20 divided by the refundRate, this ensures that the refund will always give 20 wood, regardless of the refundRate. Another minor change I made was making the ResourceManager static as we don't ever change it during the gameplay and many scripts need reference to it's methods such as refund and charge. Here is that callback:
```csharp
void OnDestroyTile(EnvironmentTile environmentTile, Vector3Int position)
{
    switch (environmentTile.envType)
    {
        case EnvironmentType.Tree:
            //Set refund rate to 1 to ensure that you get the same amount of wood every time
            int woodAmount = (int)Math.Round(20 / ResourceManager.Instance.refundRate);
            ResourceManager.Instance.Refund(new Resources(0, woodAmount, 0, 0));
            break;
        default:
            Debug.LogError("envType: " + environmentTile.envType + " does not exist!");
            break;
    }
}
```

### Discord Meeting #5 (30 min)
We had a quick Discord meeting on 11/8 to discuss our progress, I showed off the work I did, Nadav showed off his art progress, his tile set is almost complete so art implementation will be happening soon, and Gavin showed off some of his bug fixes as well as his implementation of town ranges which helps a lot with getting the game functional. We all mutually decided to give it a rest for the weekend as we all had a lot of other homework to do.

### Weekly Meeting #7 (1hr 30min)
This meeting was pretty laid back, we did some play testing, and assigned tasks for the week. We wrote down most of what was left to do in these last three weeks (ah!) of development. Nadav was to finish the tile set, and work on some more dungeon concepts as well as environment art. Gavin was to refine the dungeon rewards system and implement some clouds in the game when you zoom out. I was to implement the farm, a quick warning UI when deleting structures, get the main tower to produce magic, and a simple upgrade menu UI that pops up for the contractor so you can upgrade buildings. The warning UI I think is particularly important for deleting wizard towers since if you delete a wizard tower all the other structures in the range of that wizard tower get deleted too. After assigning tasks we went back to the main room and worked on the project and discussed life and everything in between.

## Week Two

### Work in Progress!

### Total Time Spent: 7hr 30min + 1hr for Devlogs