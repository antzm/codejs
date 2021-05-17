# JavaScript Tips

** Index **

* [Embeding a YouTube video on our personal site](#Embeding-a-YouTube-video-on-our-personal-site)
* [Embeding individual YouTube videos dynamically](#Embeding-individual-YouTube-videos-dynamically)
* [Embeding YouTube videos, into a custom playlist, dynamically](#Embeding-YouTube-videos,-into-a-custom-playlist,-dynamically)
* [Updating the content dynamically](#Updating-the-content-dynamically)
* [Styling the buttons](#Styling-the-buttons)


## Embeding a YouTube video on our personal site

First of all, it should be noted that only some videos are allowed to be embeded, as the owner of each YouTube channle has the option either to allow the embeding of their videos or not. In general though, the most famous and succefull YouTube channels, don't allow to embeding their videos.

This though, is something that is not obvious and only by emebding a video on a web page we can actually find out if that video can be embeded or not. 

The easiest way to check whether a video can be ebmeded or not, is to copy-paste the embed code, by right-clicking on the YouTube video and then to save that code as an HTML file. There's no need to add anything else in our file, besides the embed code provided by YouTube. 

Then, when opening this file on a web browser, we can click on the YouTube video and if it can be embeded, the video will start playing and so we can proceed further and incude it on our site. Otherwise, we will get a message that the video cannot be played and instead, we should watch it on YouTube.

Let's assume now that we have three Youtube videos (e.g. videoId_1, videoId_2, videoId_3), which can be embeded,  and we would like to embed those videos in our personal site. Actually, this is something very easy as we can right click on any Youtube video (while playing) and select "copy embed code" and then copy paste that code into our own web page, like the following example.

```html
<iframe width="916" height="515" src="https://www.youtube.com/embed/videoId_1" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>
```

But how are we going to embed three videos? Well, the obvious way is to embed the code for each video but this will create three iframe winfows on our site, like the following approach.

```html
<iframe width="916" height="515" src="https://www.youtube.com/embed/videoId_1" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>

<iframe width="916" height="515" src="https://www.youtube.com/embed/videoId_2" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>

<iframe width="916" height="515" src="https://www.youtube.com/embed/videoId_3" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>

```

But maybe we would rather avoid  an approach with three iframe windows.In such a case, the only way is to create just one iframe window and to include all three videos as a playlist by using the following approach:

Changing the `...embed/videoId_1` with `embed/?playlist=videoId_1,videoId_2,videoId_3`

```html
<iframe width="916" height="515" src="https://www.youtube.com/embed/?playlist=videoId_1,videoId_2,videoId_3" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>
```
If though we would like to create a playlist with several videos, then an alternative way would be to create both the iframe and the playlist dynamically using JavaScript.


## Embeding individual YouTube videos dynamically

First of all, we will need to create an array containing the ID's of the videos that we would like to embed. This array should also contain the titles of the videos, as it's not easy to obtain dynamically the title of a video using it's ID. 

Thus, the array should be something like this:

```javascript
const videoList = [
	["Title 1", "videoId_1"],
	["Title 2", "videoId_2"],
	["Title 3", "videoId_3"]
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
		["Title 1", "videoId_1"],
		["Title 2", "videoId_2"],
		["Title 3", "videoId_3"]
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

## Embeding YouTube videos, into a custom playlist, dynamically

Whenever we would like to embed several YouTube videos in a single page, the process will use too much resources due to the many `iframe` windows that will be created and also, it will take a longer time for all those iframes to load their initial content and become active.

A more efficient aproach though, is to use only one iframe in each page and to include a custom made playlist in that iframe. This way, the resources needed will be kept to a minimum and also, the time needed for the initial content of the iframe to be loaded, would be almost instantaneously.

Again, we start with an array that conntains the YouTube videoId's that we would like to embed in our web page.

```javascript
	const videoList = [
		["Title 1", "videoId_1"],
		["Title 2", "videoId_2"],
		["Title 3", "videoId_3"]
	];
```
Then, we need to create a function that will combine those video ids into a playlist.

```javascript

function createPlaylist() {
	// the format of the playlist would be
	// ?playlist=videoId_1,videoId_2,videoId_3
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

## Randomizing the video playlist

The above code though, will always create the exact same playlist based on the order of the video ids in the original array.

Should we wish to randomize the playlist, so that every time the code runs a different random playlist will be created, then we need to create a function that will first randomize the intitial array before creating the playlist.

One approach to randomize an array, would be the following:

```javascript

// In this approach, we remove the first item of the array
// and then, we place back the item in a ranodm position inside the array
// and for this process we use another function that returns
// a random position where we will place that item,
// and process is repaetad for every item of the array.
function randomizeArray(randArray) {
	let firstElement = 0;
	for (let i = 0; i < randArray.length; i++) {
		firstElement = randArray[0];
		randArray.splice(0,1);
		randArray.splice(randomNum(randArray.length), 0, firstElement);
	};
	return randArray;
}

// This function returns a random number between 0 and max (max included)
function randomNum(max) {
	let randNum = Math.floor(Math.random()*(max+1));
	return randNum;
}

````
So now, we only need to call the "randomizeArray" to randomize our array.

Thus, we only need to change the first lines of our code:

```javascript
const videoList = [
		["Title 1", "videoId_1"],
		["Title 2", "videoId_2"],
		["Title 3", "videoId_3"]
	];

videoList = randomizeArray(videoList);

```

## Updating the content dynamically

Another approach is to create dynamically, multiple videos or multiple playlists that will have some common content.

This can be accomplished by creating e.g., 5 button on our page and by clicking each button, another category of videos will appear.

To do this, we should first start by creating the button in HTML and then to style those buuton using CSS.

Thus, the HTML code could be like this:

```html

	<ul class='button-group'>
		<li><button class='buttons button-1' type='button'>Album 1</button></li>
		<li><button class='buttons button-2' type='button'>Album 2</button></li>
		<li><button class='buttons button-3' type='button'>Album 3</button></li>
		<li><button class='buttons button-4' type='button'>Album 4</button></li>
		<li><button class='buttons button-5' type='button'>Album 5</button></li>
	</ul>

```

And we also need to format the appearance of the buttons using CSS as the example below:

```css
		.button-group {
			margin-top: 25px;
			padding-top: 25px;
			text-align: center;
			list-style-type: none;
			display: flex;
			flex-direction: row;
			justify-content: center;
			flex-wrap: wrap;
		}

		.buttons {
			text-align: center;
			display: inline-block;
			background-color: #3498db;
			border: 1px solid #337fed;
			color: #ebf5fb;
			font-family: Arial;
			font-size: 15px;
			font-weight: bold;
			padding: 6px 24px;
			margin: 3px;
			outline: none;
			text-decoration: none;
			text-shadow: 0px 1px 0px #1570cd;
			border-radius: 4px;
		}
```
As we can see, we have created a flex container for the buttons while each button is displayed as an inline-block

Besides that though, we should also enable the buutons to perform the required actions.

The usual approach is to add one event listener to each button that will monitor if the button is pressed and when the button is pressed, the corresponding JavaScript code will be executed.

A simplified approach though is to add only one event listener for all the buttons and then to monitor which button was pressed. This can be acomplished by using the "evt" parameter which is fired every time a button is presed and based on the value of the "evt" parameter, we are able to detrmnine which buttons was pressed and then to run the corrsponding JavaScript code.

So, here's the code to add the event listener to the buttons:

```javascript

// This function adds the event listener to the buttons
function enableListener() {

	const buttonListener = document.querySelector('.button-group');
	buttonListener.addEventListener('click', activateButtons);
}

```

And this is the function, which performs the corresponding code depending on which button has been pressed

```javascript
const album1 = [
		["Title a", "videoId_a"],
		["Title b", "videoId_b"],
		["Title c", "videoId_c"]
	];

const album2 = [
		["Title x", "videoId_x"],
		["Title y", "videoId_y"],
		["Title z", "videoId_z"]
	];

	// the rest of the album arrays,
	// where each album contain a playlist

// This function uses the evt parameter,
// which is automatically transmitted whenever
// a button is clicked
// and depending on the evt value
// the corresponding array for each album
// is selected, the album button is highlighted
// and the page content is created
function activateButtons(evt) {

	if (evt.target.classList.contains('button-1')) {
		videoList = album1;
		selectButton('.button-1');
		createContent();
	}

		if (evt.target.classList.contains('button-2')) {
		videoList = album2;
		selectButton('.button-2');
		createContent();
	}

	// the code for the rest of the buttons...
}

```

## Styling the buttons

At the same time, we should highlight the color of the button that was pressed and this is done with the following code:

```javascript

// When a button is pressed, its color changes
function selectButton(button) {

	// First, we set all buttons to their default style.
	// This is done by selecting all the tags with the class "buttons"
	// and then we loop through all those button tags and we
	// remove the class "button-selected" from each one of them.
	const selectedButtons = document.querySelectorAll('.buttons');
	for (i = 0; i < selectedButtons.length; i++) {
		selectedButtons[i].classList.remove('button-selected');
	}

	// Then, we change the style of the newly selected button
	// by adding the class "button-selected" to the button
	// that was pressed.
	const pressedButton = document.querySelector(button);
	pressedButton.classList.add('button-selected');
}

```

The above highlighting of the buttons, uses this sample CSS code for the button classes:

```css

		.buttons:hover {
			background-color: #5dade2;
			color: #21618c;
		}

		.button-selected {
			background-color: #85c1e9;
			color: #21618c;
		}
```

The proper approach to handle the styling of the buttons, is to create a class for every style that our buttons could possible have, and then using our JavaScript code, we add or remove certain classes from the buttons in order to achive the appearance we would like to accomplish.
