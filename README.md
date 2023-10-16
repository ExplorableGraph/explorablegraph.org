# Graph Origami documentation site

This repo is the source for the Graph Origami documentation at https://treeorigami.org.

Graph Origami is very suitable for creating documentation: documentation is typically structured into a conceptual hierarchy, and multiple steps are typically needed to convert the documentation from the source form written directly by the project's authors into the result form viewed by the project's users.

Graph Origami's general-purpose facilities for working with hierarchical trees allow you to quickly construct, debug, and maintain a documentation pipeline for your project. This repository serves as its own demonstration of such a documentation pipeline.

The documentation system has the following features:

1. **Expresses the high-level steps of the pipeline as formulas.** The [site.orit](./src/site.orit) file defines formulas in the Origami expression language. Each formula succinctly expresses a step in the pipeline or some other high-level output of the project. The end result of the pipeline is a virtual tree of HTML pages and the other resources (styles, etc.) representing the final site.
1. **Browsable immediately.** You can directly serve the virtual tree for the site — visiting a URL is translated into a traversal of the virtual tree, and you can immediately view the result in your browser.
1. **Intermediate steps are browsable too.** If anything looks incorrect in the final result, it's possible to browse the intermediate steps of the pipeline to diagnose where the problem occurred.
1. **Deployable as static files.** A one-line Origami CLI command copies the virtual tree for the site into a tree of real files. These are deployed to the Netlify CDN (Content Delivery Network) as static files, which is both: a) free, and b) extremely fast.
1. **Translates a tree of markdown documents to HTML.** The documentation itself is written in markdown. The entire tree of markdown content is mapped to a corresponding tree of HTML pages.
1. **Generates links for header and sidebar.** The markdown content is organized into folders representing the conceptual areas of the site, augmented with metadata files that indicate the order in which those folders and its pages should appear. Templates take care of mapping that tree structure to create navigation elements that appear in the final HTML pages.
1. **Dynamically runs examples, inlines output.** The markdown documents contain embedded Origami expressions that define and execute sample code. One step of the documentation pipeline executes these embedded expressions and inlines the results.
1. **Extracts tagged code samples and runs them as unit tests.** A code sample can include Origami expressions and the expected output. A test process can be invoked to evaluate those expressions and compare the actual results with the expected output. This makes it possible to treat the documentation as a unit test suite, and will hopefully help catch cases where changes in the Graph Origami project might otherwise result in documentation drift.
1. **Generates and inlines SVGs.** Many of the pages include tree visualizations. These are SVGs which are generated on demand and inlined into the page.
1. **Maps page routes to illustrations.** The site's visual design includes a collection of colorful illustrations depicting animals as origami artwork. A small script uses a tree operation to hash a page's route into an index, which is then used to index into the folder of artwork at [src/client/assets/origami](./src/client/assets/origami). This allows the selected illustration to be pseudo-random but stable across program executions.
1. **Extracts type information and documentation comments to generate API documentation.** The Graph Origami project's own source code is treated as a tree, which is processed by a script to extract relevant API documentation. This script uses the Microsoft TypeScript compiler to extract type information from comments and from the source code itself. Example: the page at [src/content/core/ObjectTree.md](./src/content/core/ObjectTree.md) includes an expression that inlines the `ObjectTree` class documentation, which can be seen in the [final result](https://treeorigami.org/core/objecttree.html).
1. **Incorporates examples directly from code sample repositories.** The site includes tutorials, some of which reference code samples stored in separate repositories. The source for those repositories is also treated as a tree, allowing relevant sample code to be referenced and inlined into the tutorial pages.
1. **Extracts top-level JavaScript functions for function-by-function walkthroughs.** Inlining JavaScript files as code samples works for short files, but longer files defining multiple JavaScript functions can be harder to explain. Such longer files can be decomposed into a tree of source for the constituent functions, allowing the markdown content to inline specific functions at designated points in the documentation. The page at [src/content/pattern/serve.md](./src/content/pattern/serve.md) does this to break down the sample code for a simple tree server; the [final result](https://treeorigami.org/pattern/serve.html) shows the referenced functions inline.

Graph Origami itself does not impose any particular structure on documentation projects — in fact, Graph Origami isn't specifically a documentation tool; it just happens to be quite good for that purpose. The organization of this repository and its pipeline are flexible, and many other arrangements are possible.
