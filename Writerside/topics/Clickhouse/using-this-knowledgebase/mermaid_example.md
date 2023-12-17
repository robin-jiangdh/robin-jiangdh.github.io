# Mermaid Example
 
This Knowledge Base now supports [Mermaid](https://mermaid-js.github.io/mermaid/#/), a handy way to create charts from text.  The following example shows a very simple chart, and the code to use.

To add a Mermaid chart, encase the Mermaid code between {{</* mermaid */>}}, as follows:



```mermaid 
graph TD;
  A-->B;
  A-->C;
  B-->D;
  C-->D; 
```

And it renders as so:

<code-block lang="mermaid"> 
graph TD;
  A-->B;
  A-->C;
  B-->D;
  C-->D; 
</code-block>