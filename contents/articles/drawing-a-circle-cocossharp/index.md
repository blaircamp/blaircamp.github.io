---
title: How to draw a circle in Cocossharp
author: blair campbell
date: 2015-07-09 20:00
template: article.html
---


I have recently started playing around with the 2D game framework [CocosSharp](https://github.com/mono/CocosSharp). The first thing I tried to do with CocosSharp was to draw a circle to the screen.  This article will show you howI ended up figuring it out.

<span class="more"></span>

### How to draw shapes in CocosSharp (How to use the CCDrawNode)

The CCDrawNode is one of the ways you can draw things in CocosSharp.  You can check out the class documentation [here] (http://developer.xamarin.com/api/type/CocosSharp.CCDrawNode/)

The function we are most interested in is this function 

```cs
	public void DrawSolidCircle (CCPoint pos, float radius, CCColor4B color)
```

With this function we can initialize a CCDrawNode in our GameLayer and then draw a circle to the screen.  The code could look like this

```cs
	public class GameLayer : CCLayerColor
	{
		CCDrawNode drawingNode;
		const float CIRCLE_RADIUS = 100;
		public GameLayer() : base(CCColor4B.Black){
			drawingNode = new CCDrawNode();
			AddChild(drawingNode);
		}

		public override void AddedToScene(){
			drawingNode.Clear();
            drawingNode.DrawSolidCircle(VisibleBoundsWorldspace.Center, CIRCLE_RADIUS, CCColor4B.Blue);
		}

	}
```


Stay Tuned for more code snippets from my travels through CocosSharp.


