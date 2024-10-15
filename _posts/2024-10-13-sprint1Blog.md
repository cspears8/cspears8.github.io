---
layout: default
title: "Sprint 1 Blog"
data: 2024-10-13
categories: blog
---
## Introduction

This sprint was spent building out a very basic prototype for what a hexagonal tile map building simulator would look like. I have found my self working in my usual lane of UI programming and design, which I enjoy any ways and there is a lot of it to go about with this project.

## Week One

### Basic Control UI (3hr)
Once Gavin finished the prototype for the building system I looked over his code and was quite impressed with the system he had created. I spent some time messing around with it to learn how I can use it efficiently in my UI code. I noticed that there was no way to switch between building, destroying, or just clicking buildings. I added this in the form of a dropdown menu in Unity, I have never used dropdowns before, but they were quite simple to learn. Basically, each selection is an int whose value is represented by the spot in the dropdown list, you can simply use that integer to call specific functions, I used a simple switch statement as you can see here: 
```csharp
using System.Collections;
using System.Collections.Generic;
using System.Runtime.ConstrainedExecution;
using UnityEngine;
using UnityEngine.Serialization;

public class ModeEdit : MonoBehaviour
{
    [SerializeField] private Structure defaultBuilding;
    [SerializeField] private GameObject TypeList;
    
    public void ChangeMode(int modeIndex)
    {
        switch (modeIndex)
        {
            case 0:
                BuildingManager.DisableEditing.Invoke();
                TypeList.SetActive(false);
                break;
            case 1:
                BuildingManager.EnableBuilding.Invoke(defaultBuilding);
                TypeList.SetActive(true);
                break;
            case 2:
                BuildingManager.EnableDeleting.Invoke();
                TypeList.SetActive(false);
                break;
            default:
                Debug.LogError("Mode index at index " + modeIndex + " is out of range!");
                break;
        }
    }
}
```

The next task I had was to allow the player to switch between the current building the player wanted to build. This was quite easy as we can just use buttons which will switch the currentStructure variable in the BuildingSystem script to whatever prefab we want. I made sure to hide this UI unless the player was in build mode. This will be refactored later on in the sprint to instead be an entire menu that pops up with building information and costs for every building. Here is a screenshot of how it all looks:

![UI Screencap]({{ site.baseurl }}/files/PrelimUI.PNG){: .post-image}

## Weekly Meeting #3 (1h 30min)
This week's meeting was online due to the fall break, we instead had a Discord meeting to discuss our current progress. I asked Gavin questiosn about his building system and discussed my new UI. Nadav was running late, but we talked about art plans for him (getting building tiles). My next tasks are to add hotkey controls instead of UI elements, continue refining the UI, creating a preliminary building purchase menu, documentation work, as well as writing this very devlog. Gavin will work on getting buildings to do different things, at least have different variables and whatnot so we can implement some functionality before the end of the month.