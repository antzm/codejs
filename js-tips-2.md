# Organizing local MP4 videos

## Index

* [Creating a Python list with all the file names in a folder](#Creating-a-Python-list-with-all-the-file-names-in-a-folder)
* [Creating the page content](#Creating-the-page-content) 
* [Using several arrays in our code](#Using-several-arrays-in-our-code)
* [Randomizing the order of the videos](#Randomizing-the-order-of-the-videos)


There are various applications than can organize and play our locally stored MP4 videos but besides that, maybe we would like to organize them in a custom way.

To easily accomplish that, we could create the neccessary HTML code and access our videos in a quick, simple and fully customizable way.

Firstly, we will need to create Python lists with the filenames of our videos. Each Python list, will contain a few videos that we consider them as one group.

To simplify things, we assume that the videos we are interestd are stored in a single folder (but not inside any subfolders) and we will create five Python lists with those videos, each one containing the filenames that we consider as an individual group.

Thus, we will split our videos into five groups and we will use buttons to select which group will be displayed on our browser.


## Creating a Python list with all the file names in a folder

Firstly, we will need all the filenames of the videos that are included in the folder we are interested to use. Obviously, we could create this list manually but using python it will take just a few seconds. 

We will only need to run this python code inside the folder:

```python

import os
video_list = os.listdir()
print(video_list)

```

Running this code, a Python list with all the filenames of the folder will appear on screen and we can copy-paste that list and use it.

Alternatively, we could also run this code which will create a file with the list of the all the file names:

```python

import os
video_list = os.listdir()
my_file = open("output_file.txt","w")
print(video_list, file=my_file)


```

Now, this code will give us a Python list and just by placing a semicolon at the end of the list, we will have a JavaScript array which will be used in our code, as our code consists of HTML, CSS and JavaScript.

We used Python as this is the easiest way to create a list or an array with all the filenames in a folder.

Now, if we don't have Python installed in our computer, we could obviously use a simpler but more time consuming approach. Besides creating the JavaScript array by hand, we could also use some tricks in a computer with Windows 10. 

* In any folder, we can select the files we are interested and then, while pressing the "Shift" key, we can right-click our mouse and select the option "copy path" which will copy the complete path of every file we had selected.

* Then, in text editor we are familiar with, or even by using Word, we can select the column with the path of the files and delete it, while keeping only the names of the files. Selecting a column of text depends on the text editor we are using, while in Word we can select a column of text by pressing the "Alt" key and while pressing it, to use our mouse and select a column of text, which we will selete after, so to keep only the filenames.

Then, we use the filenames to create a JavaScript array which will be used in our code.

Let's assume that this is our array with all the videos:

```javascript

const videoList = [
	"video00.mp4",
	"video01.mp4",
	"video02.mp4",
	"video03.mp4",
	"video04.mp4",
	"video05.mp4",
	"video06.mp4",
	"video07.mp4",
	"video08.mp4",
	"video09.mp4"
];

```

[*Index*](#index)


## Creating the page content

So now, we can use this array and build our html page with all these videos:

```javascript

	// First, we create a fragment to temporarily store our new html content
	// this way, we avoid to update the document object with every iteration
	const contentFragment = document.createDocumentFragment();

	for (i = 0; i < videoList.length; i++) {

		// For each video we create a header tag
		const videoHeader = document.createElement('h1');
		videoHeader.className = 'video-title';
		videoHeader.innerHTML = videoList[i].slice(0, -4);

		// For each video we create a video tag
		const videoDetails = document.createElement('video');
		videoDetails.className = 'video-info';
		videoDetails.width = 986;  // As an example
		videoDetails.height = 553; // As an example
		videoDetails.controls = true;
		videoDetails.src = videoList[i];
		videoDetails.setAttribute("type", "video/mp4");

		// For each video we append the tags to the fragment
		contentFragment.appendChild(videoHeader);
		contentFragment.appendChild(videoDetails);
	}
```
And then, we append this document fragment to our existing html code:

```javascript

	// And finally, we update the html page with our new content
	const updatedContent = document.querySelector('.main-content');
	updatedContent.appendChild(contentFragment);

```
While our html code would be something like the following:

```html

<main class="main-content">

	<div class="video-title"></div>
	<div class="video-info"></div>

</main>

```

[*Index*](#index)


## Using several arrays in our code

If we would to use more than 1 array in our code, then we should be able to select which array we would like to use each time by pressing e.g., a button.

Let's consider the case where we have 5 arrays and so we should have 5 buttons in our code.

The html code for our buttons could be something like this:

```html

<nav id='top'>

	<ul class='button-group'>
		<li><button class='buttons button-1' type='button'>Videos 1</button></li>
		<li><button class='buttons button-2' type='button'>Videos 2</button></li>
		<li><button class='buttons button-3' type='button'>Videos 3</button></li>
		<li><button class='buttons button-4' type='button'>Videos 4</button></li>
		<li><button class='buttons button-5' type='button'>Videos 5</button></li>
	</ul>

</nav>

```

And here's the corresponding CSS code for those buttons:

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

		.buttons:hover {
			background-color: #5dade2;
			color: #21618c;
		}

		.button-selected {
			background-color: #85c1e9;
			color: #21618c;
		}

```

While, below is the JavaScript code for selecting the buttons:

```javascript

// This function adds the event listener to the buttons
function enableListener() {

	const buttonListener = document.querySelector('.button-group');
	buttonListener.addEventListener('click', activateButtons);
}


// This function uses the evt parameter,
// which is automatically transmitted whenever
// a button is clicked
// and depending on the evt value
// the corresponding array for each lesson
// is selected, the lesson button is highlighted
// and the page content is created
function activateButtons(evt) {

	if (evt.target.classList.contains('button-1')) {
		videoList = videos1;
		selectButton('.button-1');
		createContent();
	}

	if (evt.target.classList.contains('button-2')) {
		videoList = videos2;
		selectButton('.button-2');
		createContent();
	}

	if (evt.target.classList.contains('button-3')) {
		videoList = videos3;
		selectButton('.button-3');
		createContent();
	}

	if (evt.target.classList.contains('button-4')) {
		videoList = videos4;
		selectButton('.button-4');
		createContent();
	}

	if (evt.target.classList.contains('button-5')) {
		videoList = videos5;
		selectButton('.button-5');
		createContent();
	}
}

```

And this function, will change the style of the selected button:

```javascript

// When a button is pressed, its color changes
function selectButton(button) {

	// First, we set all buttons to their default style
	const selectedButtons = document.querySelectorAll('.buttons');
	for (i = 0; i < selectedButtons.length; i++) {
		selectedButtons[i].classList.remove('button-selected');
	}

	// Then, we change the style of the newly selected button
	const pressedButton = document.querySelector(button);
	pressedButton.classList.add('button-selected');
}

```

And finally, the function to create our html content with the selected videos:

```javascript

const videoWidth = 986;  // Example width
const videoHeight = 555; // Example height

function createContent() {

	// First, we remove any existing headers
	const existingHeaders = document.querySelectorAll('.video-title');

	for (i = 0; i < existingHeaders.length; i++) {
		const currentHeader = existingHeaders[i];
		currentHeader.remove();
	}

	// We remove any existing videos
	const existingVideos = document.querySelectorAll('.video-info');

	for (i = 0; i < existingVideos.length; i++) {
		const currentVideo = existingVideos[i];
		currentVideo.remove();
	}

	// And we also remove the back-to-top link
	const existingLink = document.querySelector('.back-to-top');
	existingLink.remove();

	// Then, we create a fragment to temporarily store our new html content
	// this way, we avoid to update the document object with every iteration
	const contentFragment = document.createDocumentFragment();

	for (i = 0; i < videoList.length; i++) {

		// For each video we create a header tag
		const videoHeader = document.createElement('h1');
		videoHeader.className = 'video-title';
		videoHeader.innerHTML = videoList[i].slice(0, -4);

		// For each video we create a video tag
		const videoDetails = document.createElement('video');
		videoDetails.className = 'video-info';
		videoDetails.width = videoWidth;
		videoDetails.height = videoHeight;
		videoDetails.controls = true;
		videoDetails.src = videoList[i];
		videoDetails.setAttribute("type", "video/mp4");

		// For each video we append the tags to the fragment
		contentFragment.appendChild(videoHeader);
		contentFragment.appendChild(videoDetails);
	}

	// We also add a back-to-top link
	const backToTop = document.createElement('a');
	backToTop.innerHTML = 'top of page';
	backToTop.className = 'back-to-top';
	backToTop.href = '#top';
	contentFragment.appendChild(backToTop);

	// And finally, we update the html page with our new content
	const updatedContent = document.querySelector('.main-content');
	updatedContent.appendChild(contentFragment);

}

```

And this is an example of the remaing CSS code:

```css

	body {
		margin: 0;
		padding: 0;
		background: #607d8b;
	}


	.main-content {
		text-align: center;
	}

	.video-title {
		margin-top: 36px;
		font-family: 'Palatino Linotype', 'Times New Roman', serif;
		color: #fff;
	}

	.video-info {
		margin-bottom: 63px;
		border: none;
		outline: none;
	}

	.back-to-top {
		display: block;
		color: #d6eaf8;
		font-size: 150%;
		margin-bottom: 36px;
		padding-bottom: 53px;
	}

```
[*Index*](#index)


## Randomizing the order of the videos

Using this function, we can randomize the arrays and the consquently the order of the videos:

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

[*Index*](#index)

