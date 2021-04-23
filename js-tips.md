# JavaScript Tips


Let's assume we have three Youtube videos (e.g. ViDeOiD_1, ViDeOiD_2, ViDeOiD_3) and we would like to embed those videos in our personal site. Actually, this is something very easy as we can right click on any Youtube video (while playing) and select "copy embed code" and then copy paste that code into our own web page, like the following example.

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
