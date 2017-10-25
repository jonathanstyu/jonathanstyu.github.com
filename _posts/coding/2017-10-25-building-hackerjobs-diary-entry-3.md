---
layout: post-no-feature
title: "Building HackerJobs"
description: "A Diary, Day 3"
tags: [ios, react native]

---

I keep working on this app. I have the iOS Simulator open on my screen and it keeps winking at me, reminding me that I need to get to it. Blah. 

## Saving Jobs

This was done relatively easily as I had already worked on the majority of the code with the AsyncStorage library for the JobListView component. The main difference from the previous times I have implemented this code is the use of Async/Await, which simplifies the code and keeps me from descending into nested code hell. 

The below shows the original code from the JobListView component. It triggers when you pull to refresh the FlatList. JobHandler returns the fresh list of jobs. The code then does two things. It tucks the converted data into the State for display but it also stringifies and saves the data. This way whenever someone goes back into the app, you do not need to do a call to the API. I decided this should be done because the list of Job Threads changes about once a month - infrequent enough to keep around. 

```
  try {
      var response = await JobHandler.refresh();
      var jobsStringify = JSON.stringify(response)
      await AsyncStorage.setItem("@jobthreads", jobsStringify)
      this.setState({refreshing: false, allthreads: response})
    } catch (e) {
      console.log("Error saving: " + e);
    }
```

It was not much to modify it for saved jobs. I added 2 more class functions to the JobHandler class: for getting saved jobs and saving them. 

```
JobHandler.getSavedJobs = () => {
  return new Promise(async (resolve, reject) => {
    try {
      var jobthreads = await AsyncStorage.getItem('@savedJobs')
      if (jobthreads !== null) {
        var deserializedJobthreads = JSON.parse(jobthreads);
        resolve(deserializedJobthreads)
      }
    } catch (e) {
      reject([])
      console.log("Error saving: " + e);
    }
  });
}
```

```
JobHandler.saveJob = (job) => {
  return new Promise(async (resolve, reject) => {
    try {
      var jobthreads = await AsyncStorage.getItem('@savedJobs')
      if (jobthreads !== null) {
        var deserializedSavedThreads = JSON.parse(jobthreads);
        var savedThreadsStringified = JSON.stringify(deserializedSavedThreads.push(job));
        await AsyncStorage.setItem("@savedJobs", savedThreadsStringified);
      }
    } catch (e) {
      reject(false)
      console.log("Error saving: " + e);
    }
  });
}

```

I briefly considered nesting the getSavedJobs function within the saveJob function so that I can get a more succinct function but decided that it opened the door for a future where if I make a major change in one it would require me to make similar changes to the other. Better to keep it all encapsulated. 

## “Infinite” Scrolling 

The old HackerJobs had a button at the end whereupon you needed to tap it to handle refreshing and appending new jobs to the UITableViewDataSource. I hacked this within the UITableView by taking the length of the array of jobs and adding 1. I then customized the [cellForRowAt function](https://developer.apple.com/documentation/uikit/uitableviewdatasource/1614861-tableview). I made it such that if the indexPath numbered one higher than the length of the array then I would return a special cell that carried within it a button. 

This was an ugly, horrible, dangerous hack (there was the potential for crashing if I was not keeping close track of the order of the indexpaths) and it was not available for the React Native version. 

However I noticed during a Google search (for a different functionality) a reference to a “onEndReached” function. I am not familiar with this thing, because it did not show up in the React Native documents for the FlatList nor the ScrollView (on which the FlatList is based upon). I could dig into the repo itself but decided to Google around to see if I could find an example in the wild. I did and it was implemented as such. 

```
    var list = (<FlatList style={styles.list}
        data={this.state.jobstories}
        keyExtractor={this._keyExtractor}
        ItemSeparatorComponent={this._renderSeparator}
        onEndReachedThreshold={0.8}
        onEndReached={this._endReached}
        renderItem={this._renderItem}/>)
```

## Starting Work on Themes 

This is probably going to be one of the hardest parts of the app, ironically enough. I have been struggling to create a “dark mode” for a long time and each time I run up on some significant issues. How do I “save” the setting of the theme universally throughout the whole app? 

I started a little bit of the changes here but decided to leave the rest of the implementation for a time when I can apply my full attention to it. 
