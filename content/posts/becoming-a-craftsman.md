---
author:
  name: "Josh Duffney"
date: 2018-01-22
linktitle: Becoming a Craftsman
toc: true
type: post
type: posts
title: Becoming a Craftsman
aliases: 
- /PowerShell-Story-Continued-BecomingACraftsman
tags: ["Career","Motivation","DevOps"]
categories: ["essays"]
---

My journey started off by figuring out how to automate a daily disk space report on the mailserver, which ran most of the company, and emailing the report to my boss at the time. After PowerShell sent that first email, something clicked. I sat back in my chair and thought to myself, “Wow, I don’t have to do this anymore”. 

I can still feel how exciting and relieving that thought was. Fast forward a few years and I had made automation about 80% of my job. I had moved into a few new roles - Tier 2 Support to Systems Engineer, to Senior Systems Engineer. My last post left off when I left my role as a Senior Systems Engineer and landed a gig as a DevOps Engineer. 

At the time I thought this was the end of the road. I thought, “I’ll pick up a few new tricks and further improve my PowerShell skills”. I couldn’t have been more wrong. This post picks up at the beginning of my transition into the world of DevOps, where I learned no matter how much you know, you know nothing. Continue reading to hear the rest of the story…

## The Journal of a DevOps Engineer

A few weeks into the job I was assigned my first project. I was instructed to automate the process of masking production data that would then be copied to non-production environments for development teams to use for testing. The process currently took three days of babysitting. You had to make sure it didn’t fail and if it did you had to restart it. Some jobs took over twenty-four hours to complete. Needless to say, this was time consuming and for that reason it wasn’t done as frequently as it should have been. To automate this I would have to learn a few things. One, how to use a WSDL to start the jobs. I didn’t even know what a WSDL was… Two, I would have to create a database and tables to keep track of the jobs and their progress. Three, how to orchestrate the automation. I was in over my head… and I loved it!


Next on the docket was moving a development team from the current deployment tool to Octopus Deploy. This involved creating TeamCity and Octopus Deploy projects for them and working with the development team to match up all sorts of settings, variables, etc. I was doing the work traditionally done by release engineers. Throughout this process I learned about CI (Continuous Integration) and CD (Continuous Deployment) tools, which were concepts and tools that were completely new to me. I had used CI tools before, but only for simple things like running PowerShell code. I didn’t know how to compile code, run automated testing, build a pipeline, or how code is deployed. The most important takeaway I got from this project was that I now understood how release pipelines worked and gained confidence with the tool chain the company uses. This gave me the knowledge necessary to tackle my next project, a project I was destined for.... DSC! (Desired State Configuration)


At this point I had already released my first Pluralsight course, Practical Desired State Configuration (DSC), and had a decent amount of experience with DSC from my previous job. However, figuring out where to start was difficult. Not being on the infrastructure team presented a whole new set of challenges. For the first time in my career I wasn’t a domain admin! After many cycles and failed implementation designs, we finally discovered that the easiest entry point was IIS application provisioning.


When a new application was created, a ticket would be submitted requesting that IIS be provisioned for it. This included setting up the necessary folders, app pools, and the web app with various settings. To no one’s surprise this process was very inconsistent. Sometimes the app would deploy before the task was completed or there would be a miscommunication and a poor hand off resulting in the app being deployed to the default location and also being assigned to the default app pool. We had already provided one method of automating this through our deployment tool, but teams for one reason or another didn’t use it. We knew we needed a new approach.


The solution consisted of a custom PowerShell module called InvokeDSC. Its purpose is to abstract DSC to an easy-to-read, easy-to-interpret JSON document. This document would contain all the properties needed for DSC to provision the application in IIS. The JSON document would also live within the team code repository and be managed by them. We could, of course, submit pull requests to make changes but it shifted the ownership to the development teams. With this ownership, they gained control. A world where the development teams managed their server configuration was a farfetched dream until now. I submitted this work as a talk for the 2018 PowerShell and DevOps Summit and it was accepted! I will be presenting at the Summit! I’m excited, nervous, and still somewhat in shock.


My most recent project was to implement infrastructure from code or infrastructure as code, whichever buzzword you’d like to use for our new TeamCity environment. The bottom line was, every single step required to build the environment had to be in code or documented. Documented only if automation wasn’t at all possible. There was a problem though, we didn’t have a configuration management tool. But did that stop us?... It sure didn’t…


Not having an existing tool to handle, this project was certainly a challenge. I had previously done some proof of concepts with ansible and chef, but neither of them gained much traction. We knew we weren’t going to be able to adopt any of those tools within the time frame we had. After using ansible and chef I had a pretty good idea of how the tools worked and how they structured their code. I decided we could take what we needed from these tools and create one of our own, using our deployment tool Octopus Deploy as the runner and orchestrator. Four or so months later my co-worker and friend Nate Foreman and I finished this project. I’ve started calling the framework we built ansibleish, we’ll see if it sticks or not. The finished product was a idempotent repeatable process for configuring and standing up our new TeamCity environment.


## In the Background

In the background while all this project work was going on I authored three more Pluralsight courses; Introduction to Regex, Orchestration and Automation the Big Picture, and Debugging PowerShell with VSCode. I picked up more skills that augmented my PowerShellness; automated testing with pester, debugging with vscode, and invokebuild to name a few. More importantly my mindset has shifted from an operator of technology to a developer or software engineer of technology.


Each of my projects has helped me build upon that mindset. Notice I wasn’t able to just buy new things, I had to work with what I had and develop new systems. I’m still very new to this realm, but it’s a very rewarding and challenging realm and I couldn’t be happier. The book that changed my mindset was The Practice of Cloud System Administration Volume 2, recommended to me by Steven Murawski. I can’t recommend it enough, put this on your must read list for 2018. Word of advice while reading it, go slow, absorb every word of wisdom it has to offer.


It might sound like I have it all figured out and that I’ve mastered my craft, but I haven’t. I’m just getting started. I still have to figure out this “containers thing”, learn more programming languages, tackle the cloud and how to scale distributed systems. Truth be told, I couldn’t be more excited. If you read my previous post, you know it started off with my salary increases. Sorry to disappoint, but my salary has stayed the same. That being said, there is a time to earn and a time to learn. I am definitely in a phase of deep learning. Now I said my salary stayed the same, but that’s not my only source of income. My Pluralsight courses have really taken off! I’ll make an extra ten-ish thousand this year. A special thanks to everyone who watches those courses, you’ve helped me achieve some huge financial milestones and for that I’m extraordinarily grateful.


## Becoming a Craftsman


Up until 2017, I was following my passion. Advice that was shoved in my face from every angle in my early 20’s as I aspired to be successful. It did work for me, I followed them the best I could, discovering PowerShell was one of those discoveries and it took me to where I wanted to be. The problem was, it left me feeling empty and the only way to move forward in that model was to find new passions and work even harder which seemed exhausting and not worth it. This lead me to search out a new way of life. I wanted to enjoy life as I progressed through it, not work hard and enjoy only the end.


If I had to summarize this past year (2017), I would say it’s the year realized I wanted to pursue the life of a craftsman. In 2017, I achieved every goal I set for myself when I was 20. I reached my salary goal, my financial goal of being debt free with the exception of the mortgage, and I had the title I wanted. The first half of the year was really strange and I didn’t know where to set my sights. Sadly, I also didn’t know how to enjoy my accomplishments. The gratitude and sense of accomplishment I felt quickly faded as I searched for the next thing.


Throughout the year I read several books that started to put some puzzle pieces together for me. I started to understand that while goals are important, enjoying the path to obtaining the goal is more important. I also learned that rushing through things just to check the box isn’t the most effective use of time. It results in really shallow learning, which thus results in a shallow skill you’ll have to relearn. I also learned how to examine my own habits. I was then able to identify several non-essential activities that added little to no value. I’m now in the process of removing them and replacing them. This might surprise some, I’m not replacing them with more productivity but more down time.


I left 2017 with a new blueprint for my life. Cal Newport’s The Career Craftsman Manifesto summarizes it nicely. Doing the math, I’m only a few months into this journey, but already I’m starting to feel the positive effects of it. I’m much more calm and I’m also okay with not having everything figured out. I don’t have my sights set on that next job title, I don’t have my “have to make by thirty” salary number, or feel “I need to learn this right now!” and it is kind of nice. I am not saying I don’t think about it or have ideas of those things, but the time requirement and sense of urgency is gone, which used to stress me out.

## What's Next?

So where does this leave me and what’s next for me? As I said I’m not sure, but here are some ideas. I recently discovered that I really enjoy designing systems and teaching people how to use them and seeing how they can improve what I’ve built. I enjoy leading and lifting people up. I enjoy teaching and mentoring. I’m seriously addicted to containers and want to figure them out. I can’t wait for DSC core to be released as I think it will be my Dark Portal (yes, a Warcraft reference) into Linux. You can probably expect some more Pluralsight courses from me as well. The money from them is nice, but getting feedback on them is what makes it worth it. As I moved from traditional IT to DevOps, I realized there isn’t a clear path for sysadmins. I want to help fill that gap somehow, but am not sure how. I’m considering trying to put it into a book or maybe a series of blog posts.


I also do a lot of things people would label me as crazy for. Such as putting butter in my coffee, removing all social media from my phone, waking up at 4:30 am everyday, etc. I’m toying around with a few ideas to help share some of that stuff. Perhaps vlogging, a podcast, or some blog posts. If this type of stuff interests you, please say so in the comments. It would help motivate me to do it.


## Ending

That’s it, that’s the past year of my life and what I’ve been up to. I hope you found some wisdom in the words that will help you along your journey. If not, I hope it was at least an enjoyable read. I also want to congratulate you for finishing the blog post, it’s longer than I expected it to be. It would be remiss of me to not give credit where it’s due. I’ve got to thank my boss Brett McCarty. If he hadn’t taken a chance on me I wouldn’t be where I am today. I would also like to thank my team. They are all very talented and each have unique interests and strengths. I don’t think I’ll ever stop learning from them. Lastly, but certainly not least, I have to thank the PowerShell community which has now expanded into the larger DevOps community. To everyone on Twitter, everyone in the DevOps Library community, and anyone else who has interacted with me through my courses and blog, I’m very grateful for the support, feedback, and advice. I would also like to thank my beautiful wife, whom without I would be lost.
<br>

---

I write a weekly email, Tuesday’s Thoughts where I share how I work in technology without being consumed by it.

Join 700+ subscribers and enter your email below.

<script async data-uid="a1e537562f" src="https://unique-writer-1890.ck.page/a1e537562f/index.js"></script>
