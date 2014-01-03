---
layout: post
title: "5 Reasons Not to Hire a Contract Programmer"
date: 2013-01-27 22:21
comments: true
categories: 
---
<div style="float:right; margin-left:10px;"><img alt="christmas-bonus" src="/images/elbonia.png"/></div>
This is meant primarily for non-coders. I’ve had a lot of experience over the years in hiring contract programmers and decided to write up some of the most negative aspects of working with one. I admit - these are sweeping generalizations. However, finding a contract programmer who won’t do any of these things is tough. I’ll follow this up with a list of 5 positive things.

###1. They don’t live up to their resume.

Contract programmers, especially those that have done a lot of contract work, tend to list ALL their projects and “skills” on their resume. It usually includes an alphabet soup of acronyms that means little to nothing. There’s really no way to validate that they actually know that particular technology unless you know the tech and can ask them specific questions. Be especially wary of coders who call themselves “architects”. Software architecture is an art as well as a skill. Using the Strategy pattern in one project does not an architect make. Most of the good architects I know are already paid big money at established companies.

###2. They don’t care about your project's future.

Contract programmers have one job: complete the work by any means necessary.  They don’t have a vested interest in the long term success of your project, so they won't usually account for factors that might bite you hard in the future. Some examples:

<div style="margin:0px 15px 15px 0px;">
 <ul style="margin-left:25px;">
 	<li>They choose a fringe technology with little support or life expectancy (Coldfusion, anyone?)</li>
 	<li>They are unwilling to give you access to the source code from the start</li>
 	<li>They insist on including licensed components (charting, barcoding, etc.)</li>
</ul>
</div>

###3. They no write or speak english too good.

If a contract programmer can write code that is clear and expressive, then it really doesn’t matter if they can speak your language - the code speaks for itself most of the time. The problem is when they write mangled code, then try to explain it in mangled English. Here's an example where a poorly written comments can bloat code rather than clarify it:
```csharp
// We are assigning ExamCode Value here b'coz it's datatype is interger in database.
// So we can not assign without checking of integer
_examresults.CustomExamID = Convert.ToInt32(_resultId);
```

###4. They don’t have an incentive to code well.

Unless they 1) know how to code cleanly and 2) have an incredible sense of self pride about their work, they WILL take shortcuts. Why should they take the time to refactor when they probably wont be working on the code in 6 months anyway? Why write code that is clear and expressive when the owner can’t even read code? Here's some code from an Indian contractor:

```csharp
	columnTotal = new decimal[questions.Count - 1 + 1];
    columnTotalCorrect = new decimal[questions.Count - 1 + 1];
```

Even a non-coder should be able to pick out the LOLZ in there.

That said, it's almost impossible not to incur “technical debt” early in a project, contractor or no.  Just know that you'll probably need to rewrite the code in the future when your company hits the big time.

###5. They don’t understand your project.

It may take extra effort on your part to make a contractor understand your vertical. Clarifying specifications can be a struggle, especially if there's a language barrier. They sometimes think they understand what you say but do not realize that what they hear is not what you mean. Even the smallest difference in meaning can cause a roadblock. I once worked with a Chinese firm that just couldn't understand the difference between a "Grade Level" (my daughter is in eighth grade) and a "Grade" (my daughter's grade in Algebra was an A). I had to spend an inordinate amount of time explaining the difference. A contractor that worked on a US based education product would not have had this issue.

Sometimes hiring a contract programmer is the best business decision you can make. Just be aware of some of the pitfalls. These are a things I've learned along the way.  Have more tips?  Share them in the comments...
