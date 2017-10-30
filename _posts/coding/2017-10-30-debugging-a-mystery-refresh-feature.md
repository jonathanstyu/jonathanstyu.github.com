---
layout: post-no-feature
title: "Debugging a Mystery Refresh Feature on Electron and React"
description: "DraftJS and more"
tags: [electron, react]
---

I have been working on a small Electron project. The stack is pretty simple — Electron, React, Redux, the goods. You know the deal. For the most part, it has been straightforward albeit a little messy. But one of the major challenges that I came across was with a part of the application that requires me to implement Draft.js, which is a Library based on react that offers a superior composing and writing experience.

I wanted to create a writing experience that I would like to write in. I evaluated a lot of different libraries and eventually settled on Draft.JS because it offered the most customizations, compatibility, and documentation. However, I had attempted using it before in another part of the application and found it to be a little more challenging than I thought to implement fully. So to tackle this part of the application made me hesitate.

For the past several weeks I've been working on implementing the composer feature which is called WordSmith.  I was able to get major parts of it implemented including the ability to read a saved serialized file and then being able to load it into an API feature of DraftJS called EditorState. I then wrote around a crashing bug that crashed a WordSmith Component when coming across an empty WordSmith serialized file. That took several days to figure out. 

The biggest problem though came when I wanted to switch between different instances of WordSmith.  I found that while the title and the metadata associated with the different files updated as expected,  the actual content and composer did not. So when I was clicking in between the different files WordSmith I would get a composer that is shows the data from the file I just clicked away from. 

This bug hung around my coding sessions for several weeks, annoying the shit out of me. I was evaluating the various parts that could go wrong and could create the specific situation. 

First, I looked at the data that was being passed from the database to the front end. Was it being passed over correctly? I am put a bunch of console logs and looked at the result as the component was being updated. This looked correct to me, the data was being passed over from the database without error. So it has to be in the way that React was updating. 

Then I tried to create a specific function within the react component that would force it to update by comparing the current props with the NextProps being passed to it. Just took a little while but I eventually got it to work, with the function returning a hard true or false value that would in theory cause the react component to reevaluate its entire stack and refresh when the data has changed.  But this did not cause the problem to go away.

Finally, I went back to the basics and thought about the situation in which this particular bug presents itself. The WordSmith composer properly renders its underlying data file when switching in BETWEEN components, so when React has to “start over” again it correctly shows the right file. So to explain. Here’s two sample data files: 

![ex1](https://i.imgur.com/N6az5BW.png)
![ex2](https://i.imgur.com/EWazsa5.png)





If I were to switch from WordSmith file 01H92 (‘what i am writing’) … to the Shopping List … and THEN to WordSmith file 4edAR (‘the great thing’) then DraftJS updates properly. However, if I were to go directly between them it does not update. 

I thought this through and realized that it only works when the component is created. And thus was the secret. The code that updates the EditorState of DraftJS with the underlying serialized file only exists within the constructor(props) function that creates the Component: 

```
try {
      var editorState = EditorState.createWithContent(convertFromRaw(props.documentFile.data))
    } catch (e) {
      var editorState = EditorState.createEmpty();
    }
    this.state = {
      editorState: editorState,
      titleEditMode: false,
      newLabel: ''
    };
```

I realized that when switching in BETWEEN the same component, React is not going to run this code again. So I called a part of the API that runs when a Component receives a new set of Props (called componentWillReceiveProps) and updated the code to then set a new EditorState via setState. This solved the problem. 

I feel like I would have ever known how to fix this problem unless I had some decent idea of how React worked. I am glad that I fixed it. Oh don’t get me wrong - the app still sucks and crashes, but at least it does not crash and suck for this specific reason. 
