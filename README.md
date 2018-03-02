# React Optimization

## React is fast.

Like really fast. But when making large applications with lots of components and render methods, React is at risk of slowing down. Why does it slow down? How can we check if it's *really* slowing down? And what are some ways that we can prevent this from happening? This section will serve as an explanation to answer all of these questions.

### How can I check how fast my React application is?

If you're already comfortable using Chrome Dev Tools, you probably know a bit about checking for performance. While Chrome Dev Tools are awesome, one additional tool that we can use specifically for React components (alongside Chrome Dev Tools) is a package called *"React-Addons-Perf"*

In order to install it, open up your terminal, navigate to your project directory, and run: 

```shell
yarn install --save react-addons-perf
```

After installation, import the package and expose the tools to the window object by adding the following:

```javascript
import Perf from 'react-addons-perf;'
```

```javascript
window.Perf = Perf;
```

Great, now you're all set to start measuring your application's performance. The first step is to open up your application in your browser, along with the console. To sum up what we're going to be doing, we start off by running a command that will start up our performance checker. Then we perform any action that we want to measure (ex. click something), and then we end the performance checker. Lastly, we will display our results.

So, In order to start up our performance checker, run the following command in your browsers console window: 

```shell
Perf.start()
```

Now, perform some action (ex. click something). After you're done, run the following:

```shell
Perf.end()
```

Lastly, in order to display all the DOM manipulations that were made, we run the following: 

```shell
Perf.printOperations()
```

Boom! That was easy. Now that you have a visual representation of how many DOM manipulations were made on your component, you should have a better idea of how fast your React application actually is! It probably went one of two ways:

**"I performed a bunch of actions and only a few DOM manipulations were made."**
Great! Your application is likely running optimally

**"I clicked one button and over 20,000 DOM manipulations were made."**
Don't sweat it! We're gonna cover why things slowed down in the next section! 

### Why do things usually slow down?

In order to answer this, let's think about how React works. Specifically, when we set the state of something.

Imagine for a moment that you had an application that displayed the score for a hockey game between Team Canada and Team USA. In order to do so, we create a component that will display the score of the game, and give it an initial state of 0. We then make an AJAX request to some website every 10 seconds, where we will retrieve the score of the game, and set our state to the value obtained from the AJAX request. After this, our component will rerender itself.

Sounds pretty normal. But let's examine this a little closer, specifically, the *rerender* part. If we think about a hockey game, the score usually doesn't change much in the span of 10 seconds, which is how often we are updating our state and rerendering our component. Meaning that even if the score didn't change, we are still rerendering our entire component.

**Doesn't this seem unnecessary?**

We are renrendering the component *every time,* even if there is no change in the score (ie. no change in state). Wouldn't it be helpful to only rerender our component if there was a change in score? 

**Spoiler Alert: Yes**, it would be very helpful! Let's find out how to do it! 

### How do I make things faster?

To sum up what we are trying to do, we are trying to rerender our component only when we need to, in order to avoid any unnecessary rerendering. 

In this case, we only want to rerender our score component when we see that there is a change in score. 

But how do we do this? 

**One way** to ensure that we only rerender a component only when we need to, is by using a method called

```javascript
shouldComponentUpdate()
``` 

Place this method inside of your component, and you'll notice that this method takes two arguments, *nextProps* and *nextState.* For now, we are not so concerned with the *nextProps* argument, but rather the *nextState* argument. The basic idea of this function is that it will compare our current state (*this.state*) with our new state (*nextState*) which we retrieve from the AJAX call every 10 seconds. If the current state and the new state stayed the same, we won't rerender our component. If it did, we will reredner our component! Easy, right? That's all there is to it! Here is what all of that looks like in React:

```javascript
shouldComponentUpdate(nextProps, nextState){
  return(this.state === nextState ? false: true)
}
