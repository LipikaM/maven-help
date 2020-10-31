# Helpful points for easy to use Maven

 **`<dependency>`** vs  **`<dependency-managemant>`**

In the parent POM, the main difference between the <dependencies> and <dependencyManagement> is 
Artifacts specified in the **`<dependencies>`** section will ALWAYS be included as a dependency of the child module(s).
Whereas Artifacts specified in the **`<dependencyManagement>`** section, will only be included in the child module if
they were also specified in the **`<dependencies>`** section of the child module itself. Why is it good?
because you specify the version and/or scope in the parent, and you can leave them out when specifying the dependencies
in the child POM. This can help you use unified versions for dependencies for child modules, without specifying the version in each child module.
