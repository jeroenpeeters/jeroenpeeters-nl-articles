# Frequence of Java Dependencies

**Which projects are depended upon the most?**

As part of my thesis research I had to find out which Java projects have a high usage frequency. As more and more projects are hosted on Github, it is an important and vast source for data mining and software statistics.

To answer the question, _which projects are depended upon the most_, I queried GitHub for active mature projects and aggregated all available dependency data.

![image](https://31.media.tumblr.com/afe3b656bbcb8e9cecdcaa228aef06ed/tumblr_inline_njlyteR3271t9ks7b.png)

## The Approach

Github exposes an API through which you can search for projects with a certain language, rating, etc. Furthermore it is possible to query a project’s tree structure and obtain file data.

Because I needed a representative data set I choose to include mature and active projects only. For this purpose a project is considered active if it had at least one commit in the last year. Secondly a project is considered mature if it is older than at least one year.

The Java world has three major build and dependency management tools; Maven, Gradle and Ivy. I simply downloaded the according build files to obtain dependency related information.

For each Github project each dependency is only counted once. This means that if a project contains multiple modules each having its own dependency management, duplicated dependencies are counted as one occurrence. Furthermore, build tools specific dependencies (such as maven-compiler-plugin) are omitted from the results. This is because depending on these artifacts is a consequence of using the build tool. They would thus occur frequently and cloud the results.

## The Results

From the years 2008 to 2012 I was able to retrieve 3.029 projects with dependency management files (of which 2502 (82%) Maven projects, 430 (14%) Gradle projects and 97 (3%) Ant+Ivy projects).**
**

From these figures it is not difficult to conclude that the majority of Java projects use Maven as a build and dependency management tool.

These projects had a total of 26.235 unique dependencies. The following list details the top 5 projects on which others depend:

1.  junit – 1883
2.  slf4j-api - 764
3.  oss-parent – 700
4.  log4j - 671
5.  commons-io – 543

The following graphic shows the top 25 most depended on projects. We observe that JUnit is by far the most depended on artifact, followed by sfl4j-api and oss-parent.

We can see that a large portion of these top projects are related to testing (junit, mockito-all, spring-test, mockito-core) and logging (sfl4j-api, log4j, slf4j-log4j12, commong-logging, logback-classic).
![image](https://31.media.tumblr.com/4ff9b2726e6cea24751b18e9ed843459/tumblr_inline_njlyhirw5X1t9ks7b.png)

[](http://www.jeroenpeeters.nl/wp-content/uploads/2014/04/dependency_frequency.png)

The following graphic shows the top 100 projects as a word-cloud:[
](http://www.jeroenpeeters.nl/wp-content/uploads/2014/04/dependency-word-cloud.png)
![image](https://31.media.tumblr.com/9ed06b24873cedade64195f090e78424/tumblr_inline_njlyhucOuj1t9ks7b.png)

## The code

To obtain and analyze the Github data I had to implement two relatively small programs. Both of them are freely available under the GNU GPL from, off course, Github.

[https://github.com/jeroenpeeters/github-dependency-analyzer](https://github.com/jeroenpeeters/github-dependency-analyzer "https://github.com/jeroenpeeters/github-dependency-analyzer")

## The data

Raw and analyzed data can also be obtained from the above mentioned Github repository. I’ve also compiled an ODF spreadsheet which you can [download here directly](https://github.com/jeroenpeeters/github-dependency-analyzer/raw/master/results/dependency-frequency.ods).
