---
layout: default
title: "Sprint 2 Blog"
data: 2024-10-30
categories: blog
---
## Introduction

This sprint was spent doing more UI work as well as a lot of discussion and documentation of the building and adventuring systems

## Week One

### Building Purchase Menu (4hr)
The way we were doing building selection previously was just having a UI list that the user clicks when in build mode to select the structure they wanted to build, obviously this was temporary. My task for this week was to develop a menu that the user could use to select which building they wanted with descriptions such as cost, effect, name, and image. I created a small script which sets all the data fields in the UI to the building button that the player selected. It also had to tell the "build" button, which acts as the confirm selection button, which building it needs to set in the BuildingManager script. The debug message function was just defined for building buttons which may be present in the UI but not in game yet. Here is that code as well as a screenshot of what that UI looks like:
```csharp
public class BuildingMenu : MonoBehaviour
{
    private Building curBuilding;
    
    [SerializeField] private TextMeshProUGUI curBuildingText;

    [SerializeField] private TextMeshProUGUI curBuildingDescription;
    
    [SerializeField] private TextMeshProUGUI curBuildingCostMagic;
    [SerializeField] private TextMeshProUGUI curBuildingCostWood;
    [SerializeField] private TextMeshProUGUI curBuildingCostStone;
    
    //This needs some more looking at since the structures are created dynamically right now
    //[SerializeField] private Image curBuildingImage;

    [SerializeField] private Button buildButton;

    private void Awake()
    {
        buildButton.onClick.AddListener(OnBuildButtonClick);
    }

    public void OnClick(Building building)
    {
        curBuilding = building;
        
        curBuildingText.text = building.buildingName;

        curBuildingDescription.text = building.descriptionText;
        
        curBuildingCostMagic.text = building.buildCost.Magic.ToString();
        curBuildingCostWood.text = building.buildCost.Wood.ToString();
        curBuildingCostStone.text = building.buildCost.Stone.ToString();
        
        curBuildingText.transform.parent.gameObject.SetActive(true);
    }

    public void OnBuildButtonClick()
    {
        if (curBuilding != null)
        {
            BuildingManager.EnableBuilding.Invoke(curBuilding.currentStructure);
            gameObject.SetActive(false);
            curBuildingText.transform.parent.gameObject.SetActive(false);
        }
    }

    public void DebugMessage()
    {
        Debug.LogError("This building has not be implemented yet!");
    }
}
```

![Building UI Screencap]({{ site.baseurl }}/files/buildingUI.PNG){: .post-image}

### Discord Meeting #4 (1hr)
This was a quick meeting where Gavin and I discussed our progress throughout the week. I showed off my UI while he explained his work on the hex traversal system. The hex traversal system is super impressive, but I definitely don't understand all the math behind that. We figured out what comes next, Gavin will continue working on the pathfinding and getting characters to move across the paths, while I will make designs for each building (cost, effect, levels, etc.) in Notion.

### Building Design Doc (5hr)
I spent a long time thinking of and formatting different designs for each building. When I started the process I was largely guessing how level effects and costs would scale, basically putting in random numbers. This felt wrong, so I looked at other games like Cookie Clicker to see how they scaled costs, these were just multipliers which I was not a fan of. I came across more complex formulas and settled on exponential scaling with weights for each resource type. All I had to do is adjust the weights for each resource, the initial cost, and then check it for each level which got me a scaled output which I rounded. I mapped out the formulas in Desmos and here is what I ended up with:

![Upgrade Cost Scaling]({{ site.baseurl }}/files/costScalingFormula.png){: .post-image}
![Updated UI Inputs]({{ site.baseurl }}/files/updatedUiInputs.png){: .post-image}

The next step was to design what the buildings would do, I had a lot of fund doing this since it's something I really like doing: Systems design. I thought what would work best and entice the player to upgrade the structures. Each one does something very different for the player, but they all serve the main goal of obtaining the dragon fossils to beat the game. I thought of some designs for the questing system we are creating and used the tavern as its hub. The higher level the tavern the better adventurers you can get which increases your chances of success at beating dungeons and getting progression. The smithy also helps with the questing system by creating gear for adventurers and gold to hire them, higher the level the better the gear and the higher the gold production is. I thought of a bunch of different interactive systems like that. One of the big things we decided on was buildings only having effects within the wizard tower range that it is in. So for example farms will increase the production output of all the buildings within its range (the wizard tower). In this way you are enticed to build multiple of the same structure to get their benefits in a wider area. Here is an example of the design I had for the contractor building which produces stone and allows you to build higher level buildings in its range:

![Contractor Design Table]({{ site.baseurl }}/files/contractorDesignTable.png){: .post-image}

### Weekly Meeting #5 (2hr)
We didn't have a Discord meeting this week as we were all pretty set on what we were working on. During our studio meeting we discussed the next steps, I would program the different buildings, implementing their designs through script. Gavin would continue working on the questing system. If both of these things could be implemented by our next meeting we would have a fantastic alpha prototype build to get testing feedback on. Most of the main systems would be in the game in at least a prototype state. The whole studio got our pictures taken, and we ended up working more on the project. Later in the meeting our director Connor Chen discussed our progress with us and seemed really impressed by what we had so far, that put us in a good frame of mind for the rest of the week. We discussed our project with the other R&D team who seemed to be having a lot of struggles with their project and the direction it was going in which also made us feel better.

## Week Two

### Contractor Implementation and Upgrade Framework (6hr)
I was tasked with implementing the upgrades and putting their data into some sort of database. I started by creating child classes based on the main building class for each building in the game. I then defined an upgrade dictionary for each building, as well as an event handler called OnUpgrade that applies upgrades for a building when the UpgradeEvent is invoked. For buildings like the contractor which produce materials I had to register them in the ResourceManager, this calls a coroutine which produces that resource at a set interval. Here's the code for the contractor as an example:
```csharp
public class Contractor : Building
{
    private int stoneProduction = 1;
    private List<float> buildingLevelCosts = new List<float>{ -1f, -1f, -1f, -1f, -1f };

    void Start()
    {
        UpgradeEvent.AddListener(OnUpgrade);
        rm.RegisterBuilding(this, new Resources(0,0, stoneProduction, 0));
    }

    /* Holds all the data for each upgrade(-1 means that level is not unlocked yet)
     * Ideally this would be stored somewhere rather than just a dictionary, SOs don't work afaik
    */
    private Dictionary<int, (int stoneProduction, List<float> buildingLevelCosts)> upgradeData = new()
    {
        {1, (1, new List<float>{1.0f, -1.0f, -1.0f, -1.0f, -1.0f}) },
        {2, (2, new List<float>{0.8f, 1.0f, -1.0f, -1.0f, -1.0f}) },
        {3, (4, new List<float>{0.6f, 0.8f, 1.0f, -1.0f, -1.0f}) },
        {4, (8, new List<float>{0.6f, 0.6f, 0.8f, 1.0f, -1.0f}) },
        {5, (16, new List<float>{0.6f, 0.6f, 0.6f, 0.8f, 1.0f}) },
    };

    public override BuildingType type => BuildingType.Contractor;

    //Defines what to do when an upgrade is purchased
    private void OnUpgrade(Upgrade upgrade, int newLevel)
    {
        if (upgradeData.TryGetValue(newLevel, out var upgradeValues))
        {
            stoneProduction = upgradeValues.stoneProduction;
            buildingLevelCosts = upgradeValues.buildingLevelCosts;
            Debug.Log($"Contractor upgraded to level {level}: stoneProduction = {stoneProduction}");
        }
        else
        {
            Debug.LogError($"Invalid upgrade level {newLevel} for Contractor.");
        }
    }
}
```

### Weekly Meeting #6 (2hr)
This meeting was our big showcase date, we had a working build of the game which included building structures, deleting them, sending out adventurers, and producing resources. We got some good feedback for both our design and UI/UX adjustments which we will be working on in the next sprint. We will continue to get more feedback throughout the coming weeks.

## Total Time Spent: 20hr + 2hr for Devlogs