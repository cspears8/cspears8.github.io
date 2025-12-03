---
layout: devlog
title: "Sprint 3 Blog"
data: 2024-11-17
categories: blog
---
## Introduction

This sprint we have been working on refining what we already have and adding in the remaining mechanics for our prototype.

## Week One

### Camera Control Improvements (1hr)
After getting feedback about our camera controls, menu closing, and a couple game breaking bugs regarding the UI, I dove back in to make some revisions. A nice touch I added was multiple ways to control the camera, you can now control the camera by not only clicking and dragging but also by using either the arrow keys or the WASD keys. This was quite simple, just adding a couple input maps (as you can see below) and modifying the CameraManager script. The script modification was as easy as checking to see if the player was in the none camera state, if that was the case then the arrow/WASD inputs were checked. This ensured that the camera could not be dragged and moved with the keys at the same time.

![Updated UI Inputs]({{ site.baseurl }}/files/updatedUiInputs.png){: .post-image}

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

### Warning UI (4hr)
I did not have much time this week to work on the project due to school work as the end of the semester looms. I did however build a warning UI and implement it into the game when a building was clicked to be destroyed. This was built to ensure that players knew that destroying a building was permanent and how much they would be refunded for doing so. There is also additional text for when the player tries to destroy a wizard tower, it mentions that structures in the range of only that tower will also be deleted. This whole addition was quite simple and fun with designing the UI and getting all the elements to work correctly. The most difficult part was actually dealing with some merge conflicts that resulted from me foolishly not working in a temporary scene. Here is what the finished product looked like:

![Destroy Warning UI]({{ site.baseurl }}/files/destroyWarningUI.png){: .post-image}

### Discord Meeting #6 (1hr)
That same night after finishing my warning UI we had a Discord meeting to discuss progress, make an update slideshow for the next meeting, and figure out some designs. I asked Gavin about how we should go about making the farms work, that was one of my tasks this week that I didn't have time to get around to. My original idea was to get reference to the nearest wizard tower and boost production of all the buildings within that tower's range. This was very complex as we didn't have a way to get the towers in that way, just checking if buildings are in range or out of range in general. Gavin had a great idea of just giving the farms their own range and adding that on top of the range system we already had, this was much simpler, although I still think my original idea was more interesting for gameplay purposes. I thought my idea incentivized players to place farms on the outskirts of town which is how real societies work. Gavin's idea is much more implementable with the time we have left though so I decided to go with that. Other than that we worked on the slides and got ready for the next meeting.

### Weekly Meeting #7 (2hr)
We had another production update presentation this week, Gavin implemented the art we had from Nadav at the last minute which looks amazing. Gavin showed off his work on the town range as well as dungeon levels, while I talked about the UI dictionary manager, wood production, and warning UI which I worked on. We only have a couple of weeks left so we are reaching the cutting room floor with some features. While we have designs in for the upgrades, I don't think we will get to the point where we can implement them. We also might not have time to get the smithy working, we will have to see. Other than that what is left is the farm implementation, a simple main menu, some music/SFX, a couple more art assets, a dungeon mini map for progress, and a more robust rewards system for the dungeons. With 2.5 weeks of development left I think we are in a good spot to get these tasks done and maybe a couple of others. The rest of our meeting was spent doling out tasks for the week and seeing what we can do with the rest of our development time. I was tasked with getting the farm to work (spawn trees, and increase the production rate of other buildings) and getting a simple main menu into the game. Below is an image of our updated art in game!

![Art Implementation]({{ site.baseurl }}/files/artImplementation1.png){: .post-image}

### Total Time Spent: 14hr 30min + 1hr 30min for Devlogs