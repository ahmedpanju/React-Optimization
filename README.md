# React Optimization

## React is fast.

Like really fast. But when making large applications with tons of components and render methods, React is at risk of slowing down. Why does it slow down? How can we check if it's *really* slowing down? And what are some ways we can prevent this from happening? This document will serve as an explanation to answer all of these questions.

### How can I check how fast my React application is?

If you're already comfortable using Chrome Dev Tools, you probably know a bit about checking for performance. And while Chrome Dev Tools are awesome, one additional tool that we can use specifically for React components (alongside Chrome Dev Tools) is a package called *"React-Addons-Perf"*

In order to install it, open up your terminal, navigate to your project directory, and run: 

```yarn install --save react-addons-perf```

After installation, import the package and expose the tools to the window object by adding the following:

```import Perf from 'react-addons-perf```

```window.Perf = Perf;```

Great, now you're all set to start measuring your performance. The first step is to open up your application in your browser along with the console. To sum up how it all works, we start off by running a command that will start up our performance checker. Then we perform any action that we want to measure (ex. click something), and then we end the perfomrnace checker. Lastly, we will display our results.

So In order to start up our performance checker, run the following command in your console window: 

```Perf.start()```

Now perform some action. And then after you're done, run the following:

```Perf.end()```

Now, in order to display all the DOM manipulations made, we run the following: 

```Perf.printOperations()```

Boom! That was easy. Now that you have a visual representation of how many DOM manipulations were made on your component, you should have a better idea of how fast your react application actually is! It probably went one of two ways:

**"I performed a bunch of actions and only a few DOM manipulations were made."**
Great! Your application is likely running optimally

**"I clicked one button and over 20,000 DOM manipulations were made."**
Don't sweat it! We're gonna cover why things slow down in the next section! 

### Why do things usually slow down?

In order to answer this, let's think about how React works. Specifically, when we set the state of something.

Imagine for a moment that you had an application that displayed the score for a hockey game between Team Canada and Team USA. In order to do so we, we of course create a component that will display the score of the game, and give it an initial state of 0. We then make an AJAX request to some website every 10 seconds, where we will retrive the score of the game, and set our state to this value, which will rerender our component. 

Sounds pretty normal. But let's examine this a little closer, specifically the *rerender* part. If we think about a hockey game, the score usually doesn't change much in the span of 10 seconds, which is how often we are updating our state and rerendering our component. Meaning that even if the score didn't change, we are still rerendering our entire component.

**Doesn't this seem unnecessary?**

We are renrendering the component *every time,* even if there is no change in the score (ie. no change in state). Wouldn't it be helpful to only rerender our component if there was a change in score? 

**Spoiler Alert: Yes**, it would be very helpful! Let's find out how to do it! 

### How do I make things faster?

To sum up what we are trying to do, we are trying to rerender our component only when we need to, in order to avoid any unnecessary rerendering. 

In this case, we only want to rerender our score component when we see that there is a change in score. 

But how do I do this? 

Well, we are going to discuss 2 ways of doing so. The first way is slightly more complicated but it will serve to show you how things work under the hood. And the secone way will be as simple as importing something into our component file. Let's get started! 

**The first way** that we can rerender a component only when we need to, is by using a function called ```shouldComponentUpdate``` 
