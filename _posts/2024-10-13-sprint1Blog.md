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

### Weekly Meeting #3 (1hr 30min)
This week's meeting was online due to the fall break, we instead had a Discord meeting to discuss our current progress. I asked Gavin questiosn about his building system and discussed my new UI. Nadav was running late, but we talked about art plans for him (getting building tiles). My next tasks are to add hotkey controls instead of UI elements, continue refining the UI, creating a preliminary building purchase menu, documentation work, as well as writing this very devlog. Gavin will work on getting buildings to do different things, at least have different variables and whatnot so we can implement some functionality before the end of the month.

### Devlog Madness (3hr)
After our meeting on Sunday, I spent a good portion of time in the afternoon trying to write my devlogs. I was inspired by Gavin who was using React to add stuff like interactive images and code files to his blogs. Having never used React and having this website already set up as a Jekyll site was a recipe for disaster. I spent 3 hours trying to understand the API for React while simultaneously trying to implement it to a project that wasn't built with it in mind. After failing to get code files to load as objects I simply gave up, thinking that React was far too complex for what I needed here, as well as a huge headache to alter the entire structure of the website.

## Week Two

### Input System Upgrade (3hr)
I was going to get started on the building purchase screen, when I noticed that how the inputs were being tracked (specifically for the camera movement) was sort of inefficient. The camera control design was supposed to be clicking and dragging the mouse to move the camera, Gavin initially implemented this. Looking at his code he was using the Event System and a state machine to set the mouse to 3 different states: None, Waiting, and Dragging. The waiting state was activated when the user held down the mouse button but didn't move it yet, this was simply there to transition into the dragging state. When the user dragged, the difference in where the pointer was to where it is now was calculated and used to move the camera. This worked, and had things like bounds and smoothness; however, when I looked I noticed that every function was being checked in the Update function which I thought was inefficient. To fix this I transitioned into using the new Unity Input System which allows us to name and set input events for anything we want, this allowed me to create events that happen when the user presses the mouse down and when the user releases the mouse. It should be noted that clicking the mouse is one action defined as a press down and a release of the mouse button, so I defined events for the down and up respectively, not the click. When the user presses down, the waiting state is initiated, which allows the user to drag the camera. When they release the mouse the none state is initiated. Changing the code structure and input system allows us to not have to check for these inputs every frame and only activate them when the user takes the action. Here is a snippet of that script:
```csharp
private void OnEnable()
{
    playerInput = GetComponent<PlayerInput>();

    clickAction = playerInput.actions["Select"];
    zoomAction = playerInput.actions["Zoom"];

    //Define functions for when each action is taken
    clickAction.started += _ => OnClickPerformed();
    clickAction.canceled += _ => OnClickReleased();
    zoomAction.performed += ctx => HandleScrollInput(ctx.ReadValue<Vector2>().y * zoomScale);
}

private void OnDisable()
{
    //remove functions for when each action is taken
    clickAction.started -= _ => OnClickPerformed();
    clickAction.canceled -= _ => OnClickReleased();
    zoomAction.performed -= ctx => HandleScrollInput(ctx.ReadValue<Vector2>().y * zoomScale);
}

private void Update()
{
    if (Application.isFocused)
    {
        //Changed from Input.MousePosition and calculate every frame since it's used a lot
        mousePos = Mouse.current.position.ReadValue();
        
        //If we are already dragging, or the criteria to start dragging is met then drag 
        if (state == MouseState.Waiting && (mousePos - clickPos).magnitude > dragThreshold)
        {
            state = MouseState.Dragging;
        }
        if (state == MouseState.Dragging)
        {
            DraggingState();
        }
        SmoothZoom();
        BoundCamera();

    }
}

//Sets waiting when user clicks
private void OnClickPerformed()
{
    clickPos = mousePos;
    state = MouseState.Waiting;
}

//Called only when user releases the mouse click
private void OnClickReleased()
{
    if (state==MouseState.Waiting)
    {
        mouseClick.Invoke();
    }
    state = MouseState.None;
}
```
A bug that we had regarding this was when the user clicked a tile when intending to drag the mouse, they may end up building or deleting the structure that the mouse was over when the mouse was released. This was caused because I had used clickAction.performed instead of clickAction.started, resulting in the waiting state overriding the dragging state every frame. When it was overridden the logic for OnClickReleased() did not work as intended, this was because the mouse state was always waiting when the mouse was released instead of only when the mouse was not moving (dragging).

I had to also make some minor changes to the BuildingManager and the ModeEdit scripts to work with the new input system. I made some extra minor adjustments to the ModeEdit to allow the user to switch modes using hot keys (Q, W, E) instead of requiring the use of the dropdown menu.
An interesting change in the BuildingManager was allowing the user to navigate the UI without the tile map thinking it was being selected. This was surprisingly solved by one line of code, we set a boolean every frame to check if the cursor is on a UI object, that boolean is then used in the interact script and immediately returns when true:
```csharp
isOverUI = EventSystem.current.IsPointerOverGameObject();
```
### Discord Meeting #3 (1hr)
This meeting was mostly spent working on a slideshow that we had to make to present to the rest of the studio during the next meeting. This slideshow contained info about our project from the design, art, story, and programming fields. It also contained requirements that we have for the prototype and where we are currently at in the process of building the pre-alpha for testing at the end of the month.

### Weekly Meeting #4 (2hr)
During this meeting we had some time to work and discuss where the project is at with not only each other but with the other R&D team who are working on a beat-em-up inspired mainly by Castle Crashers, but also older games in the genre like Final Fight. We showcased our project and took some questions, it went well. The main thing we have to figure out as a team about the design is if we are going to have adventurers or use a quarry for the main progression of finding Dragon Fossils. Afterwords we had a quick discussion with our Studio Director about the Game Developer's Conference (GDC) which is happening next March. I have always wanted to go and the studio is currently fundraising to get a group of students to go, but I am not a UofM student and will be graduating in December so it's complicated. I am currently in the process of applying to work for GDC which gives me an all access pass and pays me to work. Otherwise, we didn't have much else to discuss, just working on and discussing the project.

## Total Time Spent: 13hr 30min + 2hr 30min for Devlogs