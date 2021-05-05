# JavaScript Tips

## Embeding a YouTube video on our personal site

First of all, it should be noted that only some videos are allowed to be embeded, as the owner of each YouTube channle has the option either to allow the embeding of their videos or not. In general though, the most famous and succefull YouTube channels, don't allow to embeding their videos.

This though, is something that is not obvious and only by emebding a video on a web page we can actually find out if that video can be embeded or not. 

The easiest way to check whether a video can be ebmeded or not, is to copy-paste the embed code, by right-clicking on the YouTube video and then to save that code as an HTML file. There's no need to add anything else in our file, besides the embed code provided by YouTube. 

Then, when opening this file on a web browser, we can click on the YouTube video and if it can be embeded, the video will start playing and so we can proceed further and incude it on our site. Otherwise, we will get a message that the video cannot be played and instead, we should watch it on YouTube.

Let's assume now that we have three Youtube videos (e.g. ViDeOiD_1, ViDeOiD_2, ViDeOiD_3), which can be embeded,  and we would like to embed those videos in our personal site. Actually, this is something very easy as we can right click on any Youtube video (while playing) and select "copy embed code" and then copy paste that code into our own web page, like the following example.

```html
<iframe width="916" height="515" src="https://www.youtube.com/embed/ViDeOiD_1" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>
```

But how are we going to embed three videos? Well, the obvious way is to embed the code for each video but this will create three iframe winfows on our site, like the following approach.

```html
<iframe width="916" height="515" src="https://www.youtube.com/embed/ViDeOiD_1" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>

<iframe width="916" height="515" src="https://www.youtube.com/embed/ViDeOiD_2" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>

<iframe width="916" height="515" src="https://www.youtube.com/embed/ViDeOiD_3" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>

```

But maybe we would rather avoid  an approach with three iframe windows.In such a case, the only way is to create just one iframe window and to include all three videos as a playlist by using the following approach:

Changing the `...embed/ViDeOiD_1` with `embed/?playlist=ViDeOiD_1,ViDeOiD_2,ViDeOiD_3`

```html
<iframe width="916" height="515" src="https://www.youtube.com/embed/?playlist=ViDeOiD_1,ViDeOiD_2,ViDeOiD_3" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>
```
If though we would like to create a playlist with several videos, then an alternative way would be to create both the iframe and the playlist dynamically using JavaScript.


## Embeding individual YouTube videos dynamically

First of all, we will need to create an array containing the ID's of the videos that we would like to embed. This array should also contain the titles of the videos, as it's not easy to obtain dynamically the title of a video using it's ID. 

Thus, the array should be something like this:

```javascript
const videoList = [
	["Title 1", "ViDeOiD_1"],
	["Title 2", "ViDeOiD_2"],
	["Title 3", "ViDeOiD_3"]
];
```
In our HTML code, we should also include a `<section>` or a `<div>` where we plan to append the dynamically created content. For ecxample:

```HTML
<section class="embeded-videos"></section>
```
The name of the class used in this `<section>`, should be unique and it should only be used at this point in our code. Obviously, should we wish, we may add as many classes as we want in this `<section>`, but the class that will be used to append the dynamically created content should be unique. There's no need to use an ID in our codem as long as we remember that the class name should only be used once.

Then, in order to avoid multiple updates of the document object, we could create a temporary document fragment which we will use to stote the content we create. Only when the creation of our content has been completed, then we will update the document object with our new content.

**Creating a temporary document fragment**

```javascript
const contentFragment = document.createDocumentFragment();
```

To better understand the process, we create an iframe with our first video.

This will be done in two steps. First, we create a header with the title of the video

```javascript
		const videoHeader = document.createElement('h1');
		videoHeader.className = 'video-title';
		videoHeader.innerHTML = videoList[0][0];
````
And then, we create the iframe tag which will contain the Youtube video:

```javascript
		const videoFrame = document.createElement('iframe');
		videoFrame.className = 'video-frame';
		videoFrame.width = "916";
		videoFrame.height = "515";
		videoFrame.src = "https://www.youtube.com/embed/" + videoList[0][1];
		videoFrame.setAttribute( "frameborder", "0" );
		videoFrame.setAttribute( "allowfullscreen", "");	
```

And now, we use a for loop to create the content for all the video headers and all the video iframes. That content is added to the document fragment and when the process is completed, then we udate the document object with the new content:

```javascript
	
	const videoList = [
		["Title 1", "ViDeOiD_1"],
		["Title 2", "ViDeOiD_2"],
		["Title 3", "ViDeOiD_3"]
	];

	// First, we create a fragment to temporarily store our new html content
	// this way, we avoid to update the document object with every iteration
	const contentFragment = document.createDocumentFragment();


	for (let i = 0; i < videoList.length; i++) {

		// For each video we create a header tag
		const videoHeader = document.createElement('h1');
		videoHeader.className = 'video-title';
		videoHeader.innerHTML = videoList[i][0];

		// For each video we create an iframe tag
		const videoFrame = document.createElement('iframe');
		videoFrame.className = 'video-info';
		videoFrame.width = "916";
		videoFrame.height = "515";
		videoFrame.src = "https://www.youtube.com/embed/" + videoList[i][1];
		videoFrame.setAttribute( "frameborder", "0" );
		videoFrame.setAttribute( "allowfullscreen", "");		

		// For each video we append the tags to the fragment
		contentFragment.appendChild(videoHeader);
		contentFragment.appendChild(videoFrame);
	}

	// And finally, we update the html page with our new content.
	// (Obviously, there should be a <main class="main-content">
	// tag in our html code in order to apeend our content on it)
	const updatedContent = document.querySelector('.main-content');
	updatedContent.appendChild(contentFragment);

```

## Embeding YouTube videos, to a custom playlist, dynamically

Whenever we would like to embed several YouTube videos in a single page, the process will use too much resources due to the many `iframe` windows that will be created and also, it will take a longer time for all those iframes to load their initial content and become active.

A more efficient aproach though, is to use only one iframe in each page and to include a custom made playlist in that iframe. This way, the resources needed will be kept to a minimum and also, the time needed for the initial content of the iframe to be loaded, would be almost instantaneously.

Again, we start with an array that conntains the YouTube videoID's that we would like to embed in our web page.

```javascript
	const videoList = [
		["Title 1", "ViDeOiD_1"],
		["Title 2", "ViDeOiD_2"],
		["Title 3", "ViDeOiD_3"]
	];
```
Then, we need to create a function that will combine those video ids into a playlist.

```javascript

function createPlaylist() {
	// the format of the playlist would be
	// ?playlist=ViDeOiD_1,ViDeOiD_2,ViDeOiD_3
	let buildList = "?playlist="
	for (let i = 0; i < videoList.length; i++) {
		buildList += videoList[i][1] + ",";
	}
	// we remove the last comma from the playlist
	buildList = buildList.slice(0, -1);
	return buildList;
}

```

And finally, we create our content:

```javascript
	// Again, we create a fragment to temporarily store our new html content
	// this way, we avoid to update the document object with every iteration
	const contentFragment = document.createDocumentFragment();

	// We create a header for the iframe
	const playlistVideoHeader = document.createElement('h1');
	playlistVideoHeader.className = 'video-title';
	playlistVideoHeader.innerHTML = " YouTube Playlist of " + videoList.length + " videos)";

	// Then, we create the iframe tag
	const playlistVideoFrame = document.createElement('iframe');
	playlistVideoFrame.className = 'video-info';
	playlistVideoFrame.width = "916";
	playlistVideoFrame.height = "515";
	playlistVideoFrame.src = "https://www.youtube.com/embed/" + createPlaylist();
	playlistVideoFrame.setAttribute("frameborder", "0" );
	playlistVideoFrame.setAttribute("allowfullscreen", "");		

	// Then, we append the tags to the fragment
	contentFragment.appendChild(playlistVideoHeader);
	contentFragment.appendChild(playlistVideoFrame);

	// And finally, we update the html page with our new content.
	// (Obviously, there should be a <main class="main-content">
	// tag in our html code in order to apeend our content on it)
	const updatedContent = document.querySelector('.main-content');
	updatedContent.appendChild(contentFragment);

```

This code though, will always create the exact same playlist based on the order of the video ids in the original array.

Should we wish to randomize the playlist, so that every time the code runs a different random playlist will be created, then we need to create a function that will first randomize the intitial array before creating the playlist.

One approach to randomize an array, would be the following:

```javascript

function randomizeArray(randArray) {
	let firstElement = 0;
	for (let i = 0; i < randArray.length; i++) {
		firstElement = randArray[0];
		randArray.splice(0,1);
		randArray.splice(randomNum(randArray.length), 0, firstElement);
	};
	return randArray;
}


function randomNum(max) {
	let randNum = Math.floor(Math.random()*(max+1));
	return randNum;
}

````

