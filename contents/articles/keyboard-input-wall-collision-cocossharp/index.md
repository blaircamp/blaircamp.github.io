---
title: Keyboard Input and Wall Collisions in CocosSharp
author: blair campbell
date: 2015-07-23 21:30
template: article.html
---



I have been trying to make a simple game in CocosSharp and the next thing I wanted to tackle was moving 
something around the screen using the arrow keys.

I started by looking at the 2D Math with CocosSharp for the equations on how to use velocities to calculate
a new position.  That page can be found [here](http://developer.xamarin.com/guides/cross-platform/game_development/cocossharp/math/)

The equation is this

	
    PositionX += VelocityX * seconds
	PositionY += VelocityY * seconds

Whenever the update method in the GameLayer is called we can calculate the next position of the object 
using the above formulas.  If the Velocity is negative, that will cause the position to decrease, if it is positive it will increase.

The next thing we want to do is get the input form the keyboard.  In the AddedToScene method of the GameLayer
you must register the CCEventListenerKeyboard.


	protected override void AddedToScene(){
		var keyboard = new CCEventListenerKeyboard();
		keyboard.OnKeyPressed = OnKeyPressed;
		AddEventListener(keyboard,this);
	}

This will pipe all Keyboard events to the method OnKeyPressed

	private void OnKeyPressed(CCEventKeyboard e){
		...
	}


Now we want to modify this method to change the velocities of the object based on user inputs.  What I came up with was this.

	private float VelocityX {get; set;}
	private float VelocityY {get; set;}
	...
	private void OnKeyPressed(CCEventKeyboard e)
        {
            switch (e.Keys)
            {
                case CCKeys.Space:
                    break;
                case CCKeys.Up:
                    VelocityY = 1;
                    break;
                case CCKeys.Down:
                    VelocityY = -1;
                    break;
                case CCKeys.Left:
                    VelocityX = -1;
                    break;
                case CCKeys.Right:
                    VelocityX = 1;
                    break;
            }
        } 

Whenever you press the Up or Down keys on the keyboard you change the Y velocity.  Whenever you press the Left or Right keys you change the X velocity.

Now we can change the velocity of the object, now we have to change the position.  That is done in the update method.

	public override void Update(float time){

		var changeInPositionX = VelocityX * time;
		var changeInPositionY = VelocityY * time;

		circle.PositionX += changeInPositionX;
		circle.PositionY += changeInPositionY

	}

And that should move the the object circle whenever you press an arrow key! 

Hopefully this was of some help, stay Tuned for more code snippets from my travels through CocosSharp.


