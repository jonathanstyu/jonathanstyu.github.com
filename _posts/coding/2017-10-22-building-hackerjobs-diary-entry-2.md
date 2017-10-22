---
layout: post-no-feature
title: "Building HackerJobs"
description: "A Diary"
tags: [ios, react native, coding]

---

Last time I left HackerJobs in React Native, I had built out the most crucial part - that allowed me to dive into a thread, make API calls that would find the details associated with threads that are seen as “job listings” and then display them. Virtually all of these has been done with just the React Native APIs that come out of the box. It goes to show how much the framework has improved since just a year ago, when I was doing my last dive into React Native. 

You can see a review of what was done up to this post in [this introductory YouTube](https://youtu.be/yupGoZknz2k). 

The only part that I did end up using an outside library for was React Native HTMLView, which handles the complicated part of rendering raw HTML into a nice Component. This is in contrast with my last project, an in-progress email client that uses Redux, DraftJS and a multitude of other complicated libraries with their own APIs. 

For this post, I want to talk more about some of the major structural changes that I have made. 

## Turn Job List Views from a FlatList to a SectionList 

I had been thinking about how to do this before. Previously I had a simple FlatList that showed all the stories individually. They were ordered chronologically and this would work fine (and in fact that was how I had done it in the first iteration of HackerJobs) but I wanted them to be categorized by the type of Story (i.e. Who is Hiring, Who wants to be Hired, etc). 

The API is clean. You can reuse a lot of the functions like RenderItems and RenderSeperator. You only need to pass through a Sections data object rather than a Data Array of Objects. However it was much more challenging to turn an Array of objects with variably named titles into the required organization. 

I applied the experiences that I brought from my work on the Email Client to this problem. Then, I stumbled upon a function within Lodash that would group things by a function or a property within an Array: 

```
  render() {
    var endIndex = this.state['shownthreads'];
    var organized = _.groupBy(this.state.allthreads, 'parsed_title');
    return (
      <SectionList style={styles.list}
        sections={[
          {data: organized["Who is hiring?"] || [], title: "Who is hiring?"},
          {data: organized["Who wants to be hired?"] || [], title: "Who wants to be hired"},
          {data: organized["Freelancer? Seeking freelancer?"] || [], title: "Freelancer? Seeking freelancer?"},
        ]}
        keyExtractor={this._keyExtractor}
        renderSectionHeader={({section}) => this._renderSectionHeader(section)}
        ItemSeparatorComponent={this._renderSeparator}
        refreshControl={<RefreshControl
          refreshing={this.state.refreshing}
          onRefresh={this._onRefresh.bind(this)}
          title="Refresh for Jobs"
          />}
        renderItem={this._renderItem}/>
    )
  }
```

The process, which I pondered on and off over the span of a few days, ended up just being done in a single line within the Render function of the React component. The Lodash groupby function is the second of the three steps that I needed to get the data to fit the SectionList API, creating this intermediary variable called Organized. The last part was completed by calling the 3 sections with hardcoded keys. I am loathe to do hardcoded anythings, but it worked and I did not see a need to future proof for something that had already worked for the past 6 months. 

So with that taken care of I needed to find the categories. This goes back to the function that takes care of the data pull. In the previous YouTube video I talked about collecting Promises into an Array and using Promise.all to execute them all. Within those Promises, I inserted a new property that used a big ugly hack of chained functions to parse the title from “Ask HN: Who is Hiring? (Aug 2017)” to just “Who is Hiring?”. 

```
var parsed = JSON.parse(response['_bodyText'])

          // Big ugly hack so that I can strip off the dates and the "Ask HN: " at the start
          parsed['parsed_title'] = parsed['title'].split(':')[1].split('(')[0].trim();
```

## Move to a Tabbed Interface 

This is a major one. In the past couple sessions with the app I was struggling with NavigatorIOS to figure out a way to present two necessary UI screens - that belonging to the saved Jobs as well as of settings. 

I had considered a Modal, but my own previous issues with a Modal had been painful. React Native offers an API out of the box, but the weird thing about it is that you have to already include it within the Render function. See the code that is below. 

``` 
  render() {
    var endIndex = this.state['shownthreads'];
    var organized = _.groupBy(this.state.allthreads, 'parsed_title');
    return (
      <SectionList style={styles.list}
        sections={[
          {data: organized["Who is hiring?"] || [], title: "Who is hiring?"},
          {data: organized["Who wants to be hired?"] || [], title: "Who wants to be hired"},
          {data: organized["Freelancer? Seeking freelancer?"] || [], title: "Freelancer? Seeking freelancer?"},
        ]}
        keyExtractor={this._keyExtractor}
        renderSectionHeader={({section}) => this._renderSectionHeader(section)}
        ItemSeparatorComponent={this._renderSeparator}
        refreshControl={<RefreshControl
          refreshing={this.state.refreshing}
          onRefresh={this._onRefresh.bind(this)}
          title="Refresh for Jobs"
          />}
        renderItem={this._renderItem}/>
    )
  }
}
```

I would have to just go and wrap the whole thing within a View that encloses the List as well as a Modal function that is rendered by React Native but kept invisible until some property switches it on (usually something within the component’s internal state). This would be fine except the problem emerged that I put the buttons to trigger these UI modals on the shoulders of the NavigatorIOS. 

```
export default class MainApp extends React.Component {
  render() {
    return (
      <NavigatorIOS
        initialRoute={{
          component: JobListView,
          title: "HackerJobs2",
          rightButtonTitle: "Settings",
          leftButtonTitle: "Saved",
        }}
        style={{flex: 1}}
      />
    );
  }
}
```

Sooo the problem became, how can I get the information about the button’s actions - which are right now living in the MainApp React Component - into the React Component where the Modal lives - which is the JobListView React Component? The React Native website examples show that it was done through … well just putting them in the same file and having them share the same context. 

I considered a few things. 

Previously I would be doing this with Redux but I was grimly determined against bringing that level of complexity into my app. It just calls an API, for godssakes. 

During one of my late night walks, it came into my head that I can pass down a callback function through the passProps function of the NavigatorIOS’s initialRoute object. But it felt bulky and awkward to me. 

Another idea that came to me was to set a modal presentation variable within Main’s state and then pass that down into the passProps as a prop into JobListView. I would then wire the modal directly to the prop a la Redux. I tried this but it did not work. I think I had confused myself because this sort of stuff works fine with Redux but not here. Something extra Redux does makes it possible but not here. Whomp whomp. 

Sitting in the coffee shop and watching everyone walk by, I considered new ways to get around this problem. Then it dawned on me that relatively few apps actually use a Modal on a single view to present an entirely new UI. Most others (such as OutLook and the HackerNews app I personally use) use a TabBar. I looked it up and that became the accepted solution. 

There are some other changes in the latest push but these two were the big ones. 
