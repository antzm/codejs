# JavaScript Tips

## Embeding a YouTube video on our personal site

First of all, it should be noted that only some videos are allowed to be embeded, as the owner of each YouTube channle has the option either to allow the embeding of their videos or not. In general though, the most famous and succefull YouTube channels, don't allow to embeding their videos.

This though, is something that is not obvious and only by emebding a video on a web page we can actually find out if that video can be embeded or not. 

The easiest way to check whether a video can be ebmeded or not, is to copy-paste the embed code, by right-clicking on the YouTube video and then to save that code as an HTML file. There's no need to add anything else in our file, besides the embed code provided by YouTube. 

Then, when opening this file on a web browser, we can click on the YouTube video and if it can be embeded, the video will start playing and so we can proceed further and incude it on our site. Otherwise, we will get a message that the video cannot be played and instead, we should watch it on YouTube.

Let's assume now that we have three Youtube videos (e.g. ViDeOiD_1, ViDeOiD_2, ViDeOiD_3), which can be embeded,  and we would like to embed those videos in our personal site. Actually, this is something very easy as we can right click on any Youtube video (while playing) and select "copy embed code" and then copy paste that code into our own web page, like the following example.

```javascript
<iframe width="916" height="515" src="https://www.youtube.com/embed/ViDeOiD_1" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>
```

But how are we going to embed three videos? Well, the obvious way is to embed the code for each video but this will create three iframe winfows on our site, like the following approach.

```javascript
<iframe width="916" height="515" src="https://www.youtube.com/embed/ViDeOiD_1" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>

<iframe width="916" height="515" src="https://www.youtube.com/embed/ViDeOiD_2" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>

<iframe width="916" height="515" src="https://www.youtube.com/embed/ViDeOiD_3" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>

```

But maybe we would rather avoid  an approach with three iframe windows.In such a case, the only way is to create just one iframe window and to include all three videos as a playlist by using the following approach.

```javascript
<iframe width="916" height="515" src="https://www.youtube.com/embed/?playlist=ViDeOiD_1,ViDeOiD_2,ViDeOiD_3" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>
```
