# githook-maven-plugin
Maven plugin to configure and install local git hooks

## Protect your VCS
It's always a good idea to check your changes before committing them: run unit tests, perform the build, etc. However, when you rely on memory and conscience, there's a risk to simply forget, as you're human. The problem grows when we talk about a team or even about a huge project with a number of teams. The solution is to get rid of the human factor. The best way is to implement such verification on the project infrastructure level: force merge requests, use git backend hooks to execute validation scripts, restrict merge if they failed. But sometimes there's no infrastructure or it doesn't allow to implement that. For the latter cases there are [git client hooks](https://git-scm.com/book/en/v2/Customizing-Git-Git-Hooks).

## Listen to your VCS
Besides pre-commit and pre-push hooks there are some others which allow reacting on local VCS events in any fashion.

## Drawbacks of local git hooks
The main disadvantage of this approach is that hooks are kept within .git directory, which shall never come to the remote repository. Therefore, each developer will have to install them manually in his local repository, and it's bad, as the human factor strikes back. Also, it will be nearly impossible to perform an action which requires something beyond the local development environment. 
Moreover, client hooks are common for every local branch.

## So why should I use this plugin?
Because it simply workarounds the problem of providing hook configuration to the repository, and automates their installation.

## The concept
The idea is simple: keep somewhere a mapping between the hook name and the script, for each hook name create a respective file in .git/hooks, containing that script when the project initializes. "Initializes" -- is quite a polymorphic term, but when it's a maven project, then it likely means initial [lifecycle phase](https://maven.apache.org/guides/introduction/introduction-to-the-lifecycle.html). In the majority of cases, it will be enough to map the plugin on "initialize" phase, but you can still [create any other custom execution](https://maven.apache.org/guides/mini/guide-configuring-plugins.html#Using_the_executions_Tag).

## Flaws
Obviously, nothing can restrain one from the cloning of repository, and interacting with it without initial build. Also, it's always possible to delete hook files.

## Usage
The plugin provides the only goal "install". It's mapped on "initialize" phase by default. To use the default flow add these lines to the plugin definition:
```
<executions>
    <execution>
        <goals>
            <goal>install</goal>
        </goals>
    </execution>
</executions>
```
To configure hooks provide the following configuration for the execution:
```
<hooks>
  <hook-name>script</hook-name>
  ...
</hooks>
```
NOTE: plugin rewrites hooks.

## Usage Example
```
<?xml version="1.0" encoding="UTF-8"?>
<project xmlns="http://maven.apache.org/POM/4.0.0"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
    <modelVersion>4.0.0</modelVersion>
    <groupId>org.sandbox</groupId>
    <artifactId>githook-test</artifactId>
    <version>1.0.0</version>
    <build>
        <plugins>
            <plugin>
                <groupId>org.sandbox</groupId>
                <artifactId>githook-maven-plugin</artifactId>
                <version>1.0.0</version>
                <executions>
                    <execution>
                        <goals>
                            <goal>install</goal>
                        </goals>
                        <configuration>
                            <hooks>
                                <pre-commit>
                                    echo running validation build
                                    exec mvn clean install
                                </pre-commit>
                            </hooks>
                        </configuration>
                    </execution>
                </executions>
            </plugin>
        </plugins>
    </build>
</project>
```
