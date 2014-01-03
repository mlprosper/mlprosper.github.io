---
layout: post
title: "Want to find a good engineer? Look at their hands"
date: 2013-02-24 18:00
comments: true
categories: 
---

<div style="float:right; margin-left:10px;"><img alt="reloaded" src="/images/fasthands.png"/></div>
Finding a good engineer nowadays is harder than ever. If you can't compete with the companies flush with money at the high end of the salary scale, you're forced to take your chances and try to pluck a diamond from the rough. If you're wading in the shallow end of the talent pool, you need some good techniques to be able to weed out the wheat from the chaff. What's the first thing I look at in a technical interview? Their hands.

First, a little background. We coders do A LOT of repetitive tasks. Copy-pasting, building, running tests, refreshing, switching apps, and debugging are things that we do tens, if not hundreds of times a day. Good coders practice these actions until they turn into muscle memory.  Better coders find shortcuts that shave even the smallest fraction of time from these repetitive tasks.

In many interviews, I've found that a coder's keyboard and mouse speed are very good indicators of both their expertise in a particular area and, more importantly, how they approach coding. When a candidate comes in, I give them a few intentionally easy exercises. I tell them to talk me through as they work out the problem - how they approach the exercise, what options they are considering in their head, and how they chose the ultimate solution. If they don't know the answer, I tell them to do what they would normally do when they are looking for an answer.  Here's an example:

```html
<ol>
	<li>Editing only the "script.js" file, make the second paragraph blue when the page is loaded.</li>
	<li>Editing only the "script.js" file, make the third divider disappear when the first divider is clicked.</li>
</ol>

<div class="stuff">
		<p class="stuff">
			Lorem ipsum dolor sit amet, consectetur adipiscing elit. Duis ullamcorper eros justo, ac bibendum sem. Proin placerat iaculis quam, facilisis gravida ante condimentum eu. Nunc auctor est non tortor tempor vitae auctor quam dignissim. Nunc quis est at elit hendrerit consequat in sed ipsum. In lorem felis, interdum quis vehicula vitae, fermentum id urna.
		</p>
	</div>
	
	<div class="stuff">
		<p class="stuff">
			Lorem ipsum dolor sit amet, consectetur adipiscing elit. Duis ullamcorper eros justo, ac bibendum sem. Proin placerat iaculis quam, facilisis gravida ante condimentum eu. Nunc auctor est non tortor tempor vitae auctor quam dignissim. Nunc quis est at elit hendrerit consequat in sed ipsum. In lorem felis, interdum quis vehicula vitae, fermentum id urna. 
		</p>
	</div>
	
	<div class="stuff">
		<p class="stuff">
			Lorem ipsum dolor sit amet, consectetur adipiscing elit. Duis ullamcorper eros justo, ac bibendum sem. Proin placerat iaculis quam, facilisis gravida ante condimentum eu. Nunc auctor est non tortor tempor vitae auctor quam dignissim. Nunc quis est at elit hendrerit consequat in sed ipsum. In lorem felis, interdum quis vehicula vitae, fermentum id urna. 
		</p>
	</div>
```

I told you it was easy. It's intentionally bone-headed so that the actual exercise doesn't get in the way of what is most important - their hands.

If a coder comes into my office and says they know C# and Visual Studio, they'd better know the shortcut for debugging, finding a reference, and building.

If they list Ruby or Rails, they'd better know their way around a text editor like vim or SublimeText. RubyMine is another IDE with plenty of shortcuts to master. 

Javascript pros should be intimately familiar with a set of browser tools - Chrome Dev Tools or Firebug. (I immediately end the interview if the candidate starts with IE Dev tools).

One candidate I interviewed with about 2 years of C# experience started the exercise, but fumbled around with the keyboard for the first 30 seconds. "Sorry, the shortcuts on this keyboard are not the same as mine.", he said. He spent a few moments switching the Visual Studio hotkeys, then started blazing around the IDE to work on the solution. He clearly loved working with VS and put in the time to make the app work in the ways he felt most comfortable.

We hired him the next day.

Another candidate had very little experience - he had only really been coding JavaScript for 2 weeks. However, he knew his way around Chrome dev tools like a pro. His keyboard and mouse speed resembled someone with many more years of experience. It was clear to me that he had attention to detail and cared about optimizing the time he spent coding. 

<div style="float:right; margin-left:10px;"><img alt="reloaded" src="/images/reloading.png"/></div>
The converse is also true - I have never hired anyone with below average speed with a mouse and keyboard. I've had a couple of interviews where the candidate had a stellar resume, but insisted on pulling the View menu down and refreshing with a mouse-click. It was excruciating to watch him do it every time he made a UI change. 

Personally, I think the failure to optimize your workflow points to a larger problem - the inability to change or improve. If you're happy with spending the extra 2.3 seconds it takes to pull down the View menu and click "Reload" hundreds of times a day, how inefficient will you be when I give you a larger task? I ended that interview early.

Its important to remember that while keyboard and mouse speed are good indicators of competence, they don't tell you anything about a candidate's cultural fit, which is just as important. It's just what I notice first.


