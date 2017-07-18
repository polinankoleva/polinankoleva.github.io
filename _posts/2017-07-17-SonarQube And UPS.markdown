---
layout: post
title:  "SonarQube and UPS"
date:   2017-07-17 11:28:00
categories: gsoc2017
comments: true
---
Hey, reader :smiley: <br/>
Monday is here and winter has come :snowflake: :snowflake: :snowflake:. First things first - the new episode's already watched and the time for new article comes. <br/>
The previous week I shared a tutorial how to install SonarQube.
> If you have not installed it yet, you can read [my post](https://polinankoleva.github.io/education/2017/07/13/install-sonar-qube.html) how to do it. 

This week it is time to put everything into practice and analyse the SonarQube report for [Aerogear UPS](https://aerogear.org/push/#unifiedpush).

## Measures
<p>One of the first things you can see about the project you have analysed is a list of measures. </p>

<img style='height: 100%; width: 100%; object-fit: contain; border: 1px solid;' src="/web-dist/images/overall.png" />

<p>As you can see from the image, 16 bugs are found in UPS and the estimated effort to fix them is 2h 50 mins. You can easily explore them one by one and SonarQube gives you a reference to the code where a bug is found and how exactly you can fix it. For example, we can see that if a variable is not used, this is detected by SonarQube as a bug with an explanation why this is considered as bad practice. Moreover, it gives you estimation how much effort it will take to fix it in terms of time and how exactly to do it. </p>
<img style='height: 100%; width: 100%; object-fit: contain; border: 1px solid;' src="/web-dist/images/unusedVariables.png" />

<p>The UPS has only 5 security vulnerabilities, but on the other hand, 224 code smells which technical debt is 2 days and 6 hours. 
Additionally, it has 18k lines, 10k of which code lines. The tool gives you information for #classes, #files, #function and so on. Only 0.4% are the duplicated lines. And in total, the project has 245 open issues that have to be fixed. </p>
<img style='height: 100%; width: 100%; object-fit: contain; border: 1px solid;' src="/web-dist/images/size.png" />

## Issues
<p>In total, we have 245 issues. Some of them shouldn't be fixed because there is something more that a static rule can enforce. But the others need attention. After a quick look I found some that might be interesting. For sure, I will explore more of the issues in the future because I can learn a lot about the language even only by reading SonarQube rules. </p>

The first one that grabbed my attention was instead of using `Collection.size() >= 1` 
to use `!Collection.isEmpty()`, because the complexity of `size()` method can be `O(n)` depending of the collection. It seems logical, but let's face it sometimes we mistake even the simplest things.

<img style='height: 100%; width: 100%; object-fit: contain; border: 1px solid;' src="/web-dist/images/isEmptyStatement.png" />

<p> This one below is to rename a field. As you can see it can be really unintuitive to have a class and its class variable named the same way. Maybe the logic is wrong - investigate it :flashlight: </p>

<img style='height: 100%; width: 100%; object-fit: contain; border: 1px solid;' src="/web-dist/images/renameField.png" />

<p>Of course, I have found some trivial issues which we all postpone for another day. For sure everyone has written TODO comment like "this has to be fixed" or "think about this later" :smiling_imp: </p>

<img style='height: 100%; width: 100%; object-fit: contain; border: 1px solid;' src="/web-dist/images/TODO.png" />

<p>And it is good to have a tool to remind you about the depricated methods that you always postpone to find a fix for: </p>

<img style='height: 100%; width: 100%; object-fit: contain; border: 1px solid;' src="/web-dist/images/depricatedCodeRemove.png" />

## Conclusion
<p>Such tool can give you good estimation about the code quality of your project. It doesn't matter that some of the fixes are trivial, they can lead to improvement of readability and maintainability of your code base. Not to mention that even severe bugs can be fixed using SonarQube. It stores a history of a project, so you can easily keep track of the progress. Moreover, it can help you to be a better developer helping you noticing your mistakes and giving you examples and explanation why they need to be fixed. </p>

Share your feedback and stay tuned for my next post. <br/>
**Do not overthink, be happy and just keep smiling...**
<br /> Polina
