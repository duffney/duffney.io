---
author:
  name: "Josh Duffney"
date: 2016-11-06
linktitle: Doubling My Salary a PowerShell Story
type: post
type: posts
title: Doubling My Salary a PowerShell Story
aliases:
- /doubling-my-salary-a-powershell-story
tags: ["Career","Motivation"]
categories: ["essays"]
---

<p align="center">
   <a href="https://twitter.com/CianAllner/status/780390924635500544">
  <img width="584" height="302" src="/img/posts/doubling-my-salary-a-powershell-story/powershelltriplesalarytweet.png">
  </a>
</p>

I've seen tweets like this before and have even heard compelling stories about how a sys admin learned PowerShell and then got a 10% raise. Hearing stuff like this is always motivating to me, but it's not only motivating it's now a reality for me. The same day this tweet went out, I received an offer that has doubled my salary! Now, I'll admit the offer didn't double my current salary, but it has doubled since I started learning and using PowerShell (3 years ago). The rest of this blog post is my PowerShell story.


I'm not writing this to toot my own horn, but to show you that investing in PowerShell is both rewarding and worth your time and effort. I'd also like to use this as an opportunity to recognize all of those who have helped me along the way.

# The Math

The image below is a simple line graph where I plotted my starting salary from each job in my career on the Y axis and then started plotting my years of PowerShell experience on the X axis.
As you can see a trend emerged; there were of course several factors that feed into this, but one very powerful constant - PowerShell! Doing the math, my PowerShell multiplier is currently at
2.4x. The tweet I linked to above says PowerShell can triple your income, I didn't quite do that, but I'm pretty damn happy with 2.4x. Compensation is rarely talked about, but it's important
to know that this is real. I'm not the only one to accomplish this feat, it's a pretty common side effect of learning and becoming proficient in PowerShell. I personally know of at least five 
PowerShell success stories.

<p align="center">   
  <img width="600" height="360" src="/img/posts/doubling-my-salary-a-powershell-story/salary.png">
</p>

# The Beginning

Before I begin, let me just say I started in IT with the mindset that I don't want to code or program. I told myself I didn't want to stare at a screen of text all day and hack away. Well, turns out that’s what I LOVE doing! Wish I had gone to school for computer science... anyway on with the story. 

My PowerShell story begins with my manager at the time requesting that I log into a few machines each day to check their disk space. I was to do this every morning first thing when I got into the office. This wasn't bad for the first day or so, but after that it got really really annoying. I knew there had to be a better way!

Luckily, I had heard of PowerShell before and even used it a few times to create some mailboxes. So, at this point I knew the problem and I knew I could probably accomplish it with PowerShell, but how?

Like any sys admin since the invention of Google, that is where I looked first. I found a lot of related stuff, but it just left me with more questions. What's the $var mean? How does the pipeline work? What type of loop should I use? Those types of questions are harder to answer with Google, so I turned to the [Spiceworks Community](https://community.spiceworks.com). 

At the time I was a very active member in the spiceworks community, but had stayed away from the PowerShell category. It was time to change that and I posted some of my very first PowerShell related questions. 

Within a few hours I had a functioning script called
[Simple Disk Space Report](https://community.spiceworks.com/scripts/show/2106-simple-disk-space-report) that would get the diskspace and even email it! How AWESOME is that?! No more logging into servers! Needless to say that was the beginning of my addiction to PowerShell and automation.

![simplediskspacereport](/img/posts/doubling-my-salary-a-powershell-story/simplediskspacereport.png "simplediskspacereport")

# Becoming a Scripter

It's worth mentioning that at this point in the story, I wasn't quite sold on the whole sys admin scripter thing. At the time I dreamed of becoming a CCIE and was studying for my CCNA. Man, was that pain in the A$$. 

I started becoming a scripter when I worked on the service desk for a large enterprise. I was hired on as tier 2 support for network, Windows server and Citrix. I got a wide variety of requests, which I loved. It exposed me to a lot of different technologies and at scale. The tickets I didn't enjoy at first were the "Hey, we need these 300 computer objects disabled" or "please reclaim ownership of 6000 Citrix profiles". Almost everyone dreaded these tickets because they were so time consuming and mundane. 

Long story short, another guy on my team and I started using PowerShell to complete these tickets. At first everyone thought we were God's of efficiency and then we became known as the scripters.

I was only at the service desk for about 6 months before I accepted a position within the company to become their System Center Configuration Manager admin \ engineer. I didn't know it at the time, but this was a turning point in my career. I spent the next year building up their application store within SCCM and revamping their operating system deployment. Since my manager and I were the only two SCCM admins, automating wasn't an option, it was a necessity.

I used PowerShell for everything I could to automate SCCM driver imports, application creation, application deployment, maintenance, etc.. I relied heavily on the PowerShell communities and a few engineers within the company to improve my skills. It was at this point that I started reading everything I could on PowerShell. I started attending the local PowerShell user group every single month. I didn't understand half the presentations, but it was motivating to see the potential of PowerShell. I also got to network with some great people, more on that later.

![sccmprojects](/img/posts/doubling-my-salary-a-powershell-story/sccmprojects.png "sccmprojects")

# Advocating PowerShell

After honing my PowerShell skills I wanted to give back. I began advocating and teaching PowerShell to practically anyone who would listen to me. I started setting up meetings with the service desk and desktop support teams to help them learn PowerShell. I also started presenting at the local user group I was attending. Like most, I had a fear of public speaking, but I didn't let that stop me. _“Everything you want is on the other side of fear.” --Jack Canfield_.
It was at this point that I realized this is what I want to do with my career, not networking, even though I had just passed my CCNA 6 months earlier...  

There's a saying _"It's not what you know but who you know"_. Well, there is a bit of truth to that statement. Presenting and networking at the local user group got me my next gig. I was happy at my current job and enjoyed a lot of the benefits of working at a large company so I asked for a lot of money. I interviewed for the position and it went well, but it was the person I had met at the user group who got me the job. He was advocating for me within the company. He was a PowerShell scripter himself and was a very valued person at the company. 

Luckily, for me his opinion carried enough weight to get me the job. This was truly life changing for me. My wife and I were expecting our first child and we weren't sure how we could afford day care or if we should have her quit her job. This job allowed me to make up half of my wife's income. She was able to become a stay at home mom and I didn't have to worry about paying for the mortgage. Words can't express how grateful I am for that and it's because I started to learn PowerShell.

![presenting](/img/posts/doubling-my-salary-a-powershell-story/presenting.png "presenting")

# Embracing DevOps

Just before I started this new job, I had just finished a book called [The Phoenix Project](https://www.amazon.com/Phoenix-Project-DevOps-Helping-Business/dp/0988262592). This book was the start of a 2 year journey of understanding and embracing DevOps. 

Once you've been scripting or automating for awhile you start to wonder; How do I better manage this code? Is there a way to automatically execute code? How can I collaborate with others who write code? I found out that DevOps was the answer to these questions. Many still dismiss DevOps, they believe it's a buzzword. Well, it is and it isn't. 

At the core of DevOps, it means bringing Development and Operations together to create solutions. As I started learning and embracing DevOps, I started to become aware of software development practices. Things like using source control, writing tests, release pipelines, and CI\CD systems to name a few.

I took it upon myself to start leading my team in the DevOps direction. I was quickly labeled “Idealist”, but I didn't mind, I've owned that identity. My main source of DevOps news came from podcasts: DevOps Cafe, Food Fight Show, Arrested DevOps, and the Goat Farm were my favorites. I also started following a lot of the leaders of the DevOps movement on Twitter and somehow ended up in a few awesome chat rooms on slack. I asked lots of questions, watched lots of YouTube videos and read an endless stream of blog posts. Every time I learned something I applied what I could to my work and to the way my team did work. Slowly but surely it started to take hold.

Within the first 6 months or so I got most of the team sold on the idea of source control. I setup a single repository to store all of the scripts in and created training documents and videos to teach them how to use it. After that, I stood up a Jenkins instance we would use for a centralized location for all of our automation. I got help from the internal SCM team who had been using Jenkins for awhile for different development teams. This was a pivotal point for the transformation. The team really started to see the value in the vision I had been preaching for months and months. No more scheduled task servers running random non source controlled code! Within a matter of a few days we had all of our automation consolidated and running on from within Jenkins. There were jobs to automate symantec, active directory administration, vmware maintenance, and even some Linux administration!

In the meantime, I created some of my best PowerShell automation yet. One of my main projects was to consolidate several Active Directory domains. I quickly became aware of how hard it was to manually track all the migrations for groups, users, computers etc... The default would of been to use an excel spreadsheet. I refused this of course and created a solution with PowerShell and SQL to track all Active Directory domain migrations. The automation would scan the target domain and populate several SQL tables with the objects discovered. It would also email users and support teams of upcoming migrations so I didn't have to! Towards the end of the development for this, we had a Microsoft onsite review of the Active Directory environment and the Microsoft representative told me that if Microsoft were to have created it, it would've cost around half a million dollars! This isn't an accurate number, but faltering nonetheless.

It was also during this time that I started to learn Desired State Configuration. After taking a few months and watching every training video I could find on Pluralsight, Microsoft Virtual Academy, and Microsoft Premier I started using it at work to configure servers. I used DSC to promote domain controllers, build out Windows event forwarding environments and to build out the Jenkins windows slaves. I also saw an opportunity to fill a gap in Pluralsight's library and released my first course called [Practial Desired State Configuration](https://app.pluralsight.com/library/courses/practical-desired-state-configuration/table-of-contents). Producing the course wasn't easy, but it was a very rewarding process.

With all this talk about DevOps, you might be thinking that DevOps is still a buzzword and that it's unrelated to PowerShell. In my opinion, PowerShell is the technology that enables DevOps on Windows. DevOps on Windows is sometimes referred to as WinOps. Throughout all of this PowerShell was and still is the only language I use day in and out. There's so much new stuff in PowerShell, it's hard to even find the time to learn another language. However, C# is on my to-learn soon list. This is the point in the story where I started to become a developer. I still don't consider myself a true developer, but I'm somewhere in the middle of developer and scripter. Word of advice, don't be afraid to embrace these changes. Even if you're not in a software development company you can still apply these techniques and practices to improve the quality of your work. _"It is not the strongest of the species that survive, nor the most intelligent, but the one most responsive to change" Charles Darwin_

After about 18 months or so of research and experimentation I felt like I had a pretty good idea of what DevOps was and where PowerShell fit into that equation. I also obtained a clear sense of where I wanted my career to go.

In my mind, my next step would be to become a DevOps Engineer where I could continue to sharpen my PowerShell skills, but also start to become a true developer. My end goal is to basically to become an infrastructure developer working with configuration management, mainly DSC.

# I'm DevOps? Game Over?

I very recently accepted an opportunity to join the DevOps team at Paylocity! My official title is DevOps Engineer, so game over right? I made it, I'm DevOps. Well, as it turns out there is a lot more to this DevOps thing than meets the eye. As always with tech, there is a ton of new stuff to learn so I've got a long way to go. However, I think I'm on the right path. I've switched roles a little bit, the DevOps team I'm on deals mostly with the deployments of the company's applications. I'm actually within the Development department, which is very weird for me, I've only been in IT Operations prior to this. It's different, but good. Day 3 of the job and I was assigned a project to automate some SQL data masking using a [WSDL](https://en.wikipedia.org/wiki/Web_Services_Description_Language) with horrible documenation. I had never done that before so I love that I'm already being challenged to learn new stuff.

<p align="center">   
  <img width="480" height="272" src="/img/posts/doubling-my-salary-a-powershell-story/GameOver.jpg">
</p>


So, now you're caught up with my PowerShell story. I hope it was an enjoyable read, but more than that I hope it has given you some insight into how learning PowerShell has changed my career for the better. When I started in IT I never thought I would end up writing code all day and enjoying it so much. [Jeffrey Snover](https://twitter.com/jsnover) often talks about how PowerShell makes your work fun again, that's exactly what PowerShell has done for me. Work is no longer work, I don't dread getting up in the morning and getting to work. It's actually quite the opposite, I'm ready at 5:00am sometimes to start coding because I dreamt of something cool I could do in PowerShell.

## Credits

![credits](/img/posts/doubling-my-salary-a-powershell-story/credits.gif "credits")

