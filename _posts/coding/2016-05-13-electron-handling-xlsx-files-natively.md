---
layout: post-no-feature
title: "Handling XLSX Files Natively"
description: "Orbiting the Electron"
tags: [coding, electron, desktop apps, javascript]

---

I have found that writing this blog has become an excellent source of release or reflection. Since nobody reads it and I don't really have people around who want to talk about the things that I want to talk about, this blog will have to do. 

Today I want to reflect on a current project that I am working on, VLOOKER. After a few apps in Swift, I decided to create a set of tools that would help me do my job better at work. Something that I found quite a pain to deal with was doing VLOOKUPs in Excel on huge (1MB+ sized) sheets. I am not sure why this is the case, but I ended up writing a lot of individual scripts that would help do the job more quickly. Perhaps it has to do with the fact that there are less computer resources pushing GUI pixels. Whatever. 

Anyway, Electron is a brand new thing for me. On a broad, grand concept I understand kind of what it is. It is basically an inert web browser with more excellent native file system privileges. However actually tapping into those capabilities is something that I didn't know. 

The original goal of the project is to simply wrap some of the scripts that I have lying around into a GUI. The goal quickly has blown up in my face - it is surprising how complex translating a simple sentence "Just wrap it in a GUI" into actual working code. And I am cutting like a million corners too. 

### Starting with Boilerplate

I started with the [Awesome Electron](https://github.com/sindresorhus/awesome-electron) github repo and ran to a Boilerplate. The first version of Electron that I started with was the version with [React](https://github.com/chentsulin/electron-react-boilerplate). However I found that it was a pretty hard boilerplate for me to start with. For one thing, I don't understand React yet and wasn't really interested in taking the time to pick it up. Learning the native elements of Electron was starting to be a pain (see below) and picking up a framework would be just too much. 

I ended up with this simple [boilerplate](https://github.com/szwacz/electron-boilerplate). It is a little more than just the standard code but gives me enough of a blank slate to do what I want. The rest would be made with jQuery and a templating library. The one I chose was Underscore, from my BackBone.js days. 

### Starting with Drag and Drop 

Like I said, there is not a lot of good content about writing the type of Electron app that I wanted. Not to mention, I was not so sure about what I wanted. At first I envisioned a drag and drop interface. People can drag XLS and XLSX files onto a box and the app would scan/convert the file - present them with information. 

While parsing through the Electron api docs, I came across [this code snippet](http://electron.atom.io/docs/api/file-object/): 

    <div id="holder">
      Drag your file here
    </div>

    <script>
      var holder = document.getElementById('holder');
      holder.ondragover = function () {
        return false;
      };
      holder.ondragleave = holder.ondragend = function () {
        return false;
      };
      holder.ondrop = function (e) {
        e.preventDefault();
        var file = e.dataTransfer.files[0];
        console.log('File you dragged here is', file.path);
        return false;
      };
    </script>

This seems like a good place to start. I would be able to pass the file to a library and work from there. I incorporated the code snippet into the app. Before long someone could crudely drag and drop and it would present information about the file like when it was created, how large it is, and its name. 

I combined it with this [Excel JS parser library](https://github.com/SheetJS/js-xlsx) and got some interesting and promising results. But then ... 

### Yeah I Changed My Mind and Now It's All Hard

Drag and drop sucked. I am an Excel guy with roots in banking and accounting. I love the shortcuts. I realized while testing the D&D functionality that while it worked rather well, I hated dealing it all the time. Using the mouse and/or the touchpad to drag files is unreliable and gawky. I want a keyboard shortcut that I can hit to bring up a dialog. So I dumped all my D&D templating and behavior code and rewrote it. 

Well this is rather new ground. 

### Keyboard Shortcuts

The main challenge was how to create custom keyboard shortcuts. I had seen in the boilerplate code a few pre built ones and wanted to adapt them. Here is the code provided: 

    export var viewMenuTemplate = {
      label: 'View',
      submenu: [
          {
            label: 'Reload',
            accelerator: 'CmdOrCtrl+R',
            click: function(item, focusedWindow) {
              if (focusedWindow)
                focusedWindow.reload();
            }
          }
        ]
    }

This code corresponds to one of the menus on the toolbar that presents itself when the application is focused. You can build features within it that are accessible by an "Accelerator", a keyboard shortcut. I tried to replicate this in my own "Data Menu Template": 

      {
        label: 'Upload Sheet',
        accelerator: 'CmdOrCtrl+U',
        click: function(item, focusedWindow) {

This would create a native upload menu file that is familiar to many people. By the way, that particular feature was an hour of headache to find. Nothing on the docs is like, "HEY that file upload thing you want? It's this feature here." You gotta google around until [SOMEONE says](http://www.mylifeforthecode.com/getting-started-with-standard-dialogs-in-electron/) you are looking for the Dialog feature.

Well anyhoo, after I got the Dialog feature working and selecting files, it passes to me a bunch of file URLs rather than the File itself. The problem with this thing is that when I consoled.log the filenames the filenames appeared on the Terminal rather than in the Browser where I had initialized the app itself (represented here as the App.js file). Hmm, how can I have the library parse the File if it cannot even access the files itself? How can I pass the data from the Main Process (as the docs appear to call it) to something living in the BrowserWindow (initialized as part of the app launch process)?

I struggled over this for a day, eventually quitting and going for a run. While I was in bed staring at the ceiling that night I had an idea. I would initialize the Current Window and have it execute raw JavaScript, that JS coming from the App.js files that make up the app that the BrowserWindow is showing. This would hopefully allow me to pass the filenames to the App.js. 

This didn't work at all so I eventually Googled around. Lord Google brought me eventually to an [interesting code snippet](https://github.com/szwacz/electron-boilerplate/issues/71#issuecomment-215697886) in the Electron Boilerplate issues page. This eventually let me to a solution to the fix. I would grab the focusedWindow variable from Electron's BrowserWindow function and send to its webContents the name of a predefined function. Once safely inside the app.js environment, I would THEN call Dialog and upload the files there. This gets around the issue of passing the filenames variable through the "blood-brain barrier" between the main process and the WebBrowser by simply encompassing the entire process into the WebBrowser itself. Hopefully this decision won't have future architectural pain in the ass consequences. 

This took a week. Ugh. This is just ONE feature. I haven't even started working on the actual functionality of the product and I'm writing rant posts on the blog. Sheesh. 

### Creating a Feature vs. Creating a Product

I write a lot about this in these code blog posts, but there is a big difference and a big jump from "an idea of how the thing works" to "actually getting the thing to work". It is kind of like an epic action scene in the movie. The concept is simply explained in a single sentence: "Truck flips over". But getting from sentence to thing actually happening is long and complex and requires a lot more thought than you might at first expect.  

Maybe it testifies to how much of an idiot I am that I keep falling into this trap. The shock of creating a product out of a feature has been something I have hit upon repeatedly with HakkerJobs and CollegeScan. Wrapping those features with functionality has been one of the greatest challenges of making those apps - holding the user's hand and guiding them to the feature. The actual feature was relatively easily created. 

Okay enough writing in this blog. Back to the product. 