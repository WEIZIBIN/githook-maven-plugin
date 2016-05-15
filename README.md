# githook-maven-plugin
Maven plugin to configure and install local git hooks

## Reasoning
It's always a good idea to check your changes before comitting: run unit tests, perform build, etc. However, when you rely on memory and 
conscience, there's a risk to simply forget, as you're human. The problem grows when we talk about a team or even about a huge project 
with a number of teams. The solution is to get rid of human factor. The best way is to implement such verification on the 
project infrastructure level: force merge requests, use git backend hooks to execute validation scripts, restrict merge if they failed. 
But sometimes there's no infrastructure or it doesn't allow to implement that. For the latter cases 



