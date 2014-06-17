These two projects were designed to show the issue with the removal of tests 
execution in the SonarQube java Plugin 2.2.0.

I hope I've missed an obvious workaround to that issue, if you find one, just let us know! :-).

Thanks.

Note
----
Note that this is by no mean a criticism towards SonarQube product or team, just 
a way to demonstrate a situation where getting code coverage actually suddenly 
got a lot harder. 

And as I guess we're not having some really uncommon settings, I hope that finding a 
good solution here could help others in the community.

How to reproduce the issue
--------------------------
That repository contains two projects:
* corporate-pom : you must mvn install it first
* sonar-notests-anymore : project to analyze in Sonar, unable to get code coverage starting with 2.2.0.

Because we have an "argLine" property put inside our "configuration" block of the maven-surefire-plugin,
the 'argLine' created by jacoco:prepare-agent won't get used by surefire.

What is so complicated? Just fix your poms!
-------------------------------------------
Well, yes, but not that simple. 

* Fix each project to analyse directly?
  * Last time I checked, we had 140 jobs set up inside Jenkins for SonarQube analysis. 
    That makes more than a hundred of commits *just to workaround that issue*. 

* Fix the corporate pom ?
  * Sure, yes, that's what we're planning to do in the long-term. 
   But then you're back anyway to the first issue: updating all the dependent projects 
   so that they get the new configuration. That will take time and energy to be sure 
   no-one still uses the old corporate pom...

What we're trying to do?
------------------------
Find a way to still get the code coverage *without touching any sources* (say pom.xml).
The SonarQube documentation says it's possible: http://docs.codehaus.org/display/SONAR/Analyzing+with+Maven :

    $ mvn clean org.jacoco:jacoco-maven-plugin:prepare-agent install
    $ mvn sonar:sonar

But indeed this test project is trying to show a situation where this is not usable that easily (see explanations above).

References
----------
* Related discussion: http://sonarqube.15.x6.nabble.com/Sonar-runner-doesn-t-execute-maven-tests-anymore-td5024882.html
