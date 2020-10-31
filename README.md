# Helpful points for easy to use Maven

 **`<dependency>`** vs  **`<dependency-managemant>`**

- In the parent POM, the main difference between the <dependencies> and <dependencyManagement> is 
Artifacts specified in the **`<dependencies>`** section will ALWAYS be included as a dependency of the child module(s).
  
- Whereas Artifacts specified in the **`<dependencyManagement>`** section, will only be included in the child module if
they were also specified in the **`<dependencies>`** section of the child module itself. Why is it good?
because you specify the version and/or scope in the parent, and you can leave them out when specifying the dependencies
in the child POM. This can help you use unified versions for dependencies for child modules, without specifying the version in each child module.

- What dependencyManagement does is simply move your dependency definitions (version, exclusions, etc) up to the parent pom, then in the child poms you just have to put the groupId and artifactId. That's it.

-  So if you had 2 projects (proj1 and proj2) which share a common dependency (betaShared) you could move that dependency up to the parent pom. While you are at it, you can also move up any other dependencies (alpha and charlie) but only if it makes sense for your project. So for the situation outlined in the prior sentences, here is the solution with dependencyManagement in the parent pom:


<!-- ParentProj pom -->
```<project>
  <dependencyManagement>
    <dependencies>
      <dependency> <!-- not much benefit defining alpha here, as we only use in 1 child, so optional -->
        <groupId>alpha</groupId>
        <artifactId>alpha</artifactId>
        <version>1.0</version>
        <exclusions>
          <exclusion>
            <groupId>zebra</groupId>
            <artifactId>zebra</artifactId>
          </exclusion>
        </exclusions>
      </dependency>
      <dependency>
        <groupId>charlie</groupId> <!-- not much benefit defining charlie here, so optional -->
        <artifactId>charlie</artifactId>
        <artifactId>charlie</artifactId>
        <version>1.0</version>
        <type>war</type>
        <scope>runtime</scope>
      </dependency>
      <dependency> <!-- defining betaShared here makes a lot of sense -->
        <groupId>betaShared</groupId>
        <artifactId>betaShared</artifactId>
        <version>1.0</version>
        <type>bar</type>
        <scope>runtime</scope>
      </dependency>
    </dependencies>
     </dependencyManagement>
</project>

<!-- Child Proj1 pom -->
<project>
  <dependencies>
    <dependency>
      <groupId>alpha</groupId>
      <artifactId>alpha</artifactId>  <!-- jar type IS DEFAULT, so no need to specify in child projects -->
    </dependency>
    <dependency>
      <groupId>betaShared</groupId>
      <artifactId>betaShared</artifactId>
      <type>bar</type> <!-- This is not a jar dependency, so we must specify type. -->
    </dependency>
  </dependencies>
</project>

<!-- Child Proj2 -->
<project>
  <dependencies>
    <dependency>
      <groupId>charlie</groupId>
      <artifactId>charlie</artifactId>
      <type>war</type> <!-- This is not a jar dependency, so we must specify type. -->
    </dependency>```
