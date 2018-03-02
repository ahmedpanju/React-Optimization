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
