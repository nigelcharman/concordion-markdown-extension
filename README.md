[![Build Status](https://travis-ci.org/concordion/concordion-markdown-extension.svg?branch=master)](https://travis-ci.org/concordion/concordion-markdown-extension)
[![Apache License 2.0](https://img.shields.io/badge/license-Apache%202.0-blue.svg)](http://www.apache.org/licenses/LICENSE-2.0.html)

# Concordion Markdown

__Note:__ This extension is in development, and the syntax may change as we continue to evolve it.

## Philosophy
[Markdown](https://daringfireball.net/projects/markdown/) provides an easy-to-read and easy-to-write syntax for converting plain text to structured XHTML.

This Concordion Markdown extension allows you to write your [Concordion](http://concordion.org/) specification in the Markdown format, converting the Markdown to XHTML at runtime and running the resultant XHTML as a Concordion specification.

The following assumes that you already understand Concordion. If not, please visit the [Concordion tutorial](http://concordion.org/Tutorial.html). 

### Use of inline links 

In order to keep the grammar readable, we have used Markdown's inline links to embed Concordion commands. This maintains a clean separation of Concordion commands from the original text.  

As an example:

> `When Jane logs on, she is greeted with Hello Jane!`

is marked up as:

> `When [Jane](- "#name") logs on, she is greeted with [Hello Jane!](- "?=greetingFor(#name)")`
 
where:

 * `[Jane](- '#name')` sets the `#name` variable to `Jane`.
 * `[Hello Jane!](- '?=greetingFor(#name)')` asserts that the `greetingFor(#name)` method returns `Hello Jane!`

When viewed on Github or in the preview pane of a Markdown editor, the original text is displayed as a link, and the Concordion command shown when you hover over the link:

> ![Github Markdown Example](img/IntroGithub.png)

When the specification is run with a fixture that implements the `greetingFor(String)` method, the links are converted to spans and the output is shown as:

> ![Markdown Example Output](img/IntroOutput.png)

## Installation
TODO

## Basic Grammar

Concordion commands are differentiated from other Markdown links by using the value `-` for the URL:

    [value](- "command")
or

    [value](- 'command')

### Commands
A shorthand syntax is provided for the set, assert equals and execute commands.

| Command        | Grammar                   | Example |
| -------------- | ------------------------  | ------- |
| Set            | `[value](- "#varname")`   | `[Jane](- "#name")` |
| Assert Equals  | `[value](- "?=#varname")` | `[Hello Jane!]`<br/>`(- "?=#greeting")` |
| Execute        | `[value](- "expression")` | `[The greeting is]`<br/>`(- "#greeting=greetingFor(#name)")` |
| Other commands | `[value](- "c:command")`  | `[is notified]`<br/>`(- "c:assertTrue=isNotified()")` |

#### Table Commands
The command to be run on the table is specified in the first table header row, with the commands for each column of the table specified in the second table header row.

The first table header row is not shown on the output HTML.

##### Execute on a table

    |[_add_](- "#z=add(#x, #y)")|
    |[Number 1](- "#x")|[Number 2](- "#y")|[Result](- "?=#z")|
    | ---------------: | ---------------: | ---------------: |
    |                 1|                 0|                 1|
    |                 1|                -3|                -2|

##### Verify Rows

    |[_check GST_](- "c:verifyRows=#detail:getInvoiceDetails()") |
    |[Sub Total](- "?=#detail.subTotal")|[GST](- "?=#detail.gst")|
    | --------------------------------- | ---------------------: |
    |                                100|                      15|
    |                                 20|                       2|


#### Example Command
Adding an inline link to a header changes the header into an example command. You can use either the Atx-style or Setext-style headers. For example:

    ## [Example 1](- "exampleName")

or 

    [Example 1](- "exampleName")
    ----------------------------------

will create an example named `exampleName` with the H2 heading `Example 1`.

#### Closing an example
An example is implicitly closed on any of these conditions:

* another example starts, or
* a header is encountered that is at a higher level than the example header (eg. the example is a `h3` and a `h2` header is encountered), or
* the end of file is reached.

To explicitly close an example, create a header with the example heading struck-through. For example:  

    ## ~~Example 1~~
    
will close the example with the heading `Example 1`    

__Note:__ the example command requires Concordion 2.0.0 or later.

#### Run Command
Adding a title of `c:run` to an inline link will add a run command to that link. For example:

    [Address](Address.html "c:run")

will run the `Address.html` specification.

---

For the full Grammar see the [Grammar Specification](http://concordion.github.io/concordion-markdown-extension/spec/concordion/markdown/Grammar.html).

## Configuration
The extension can also be [configured](http://concordion.github.io/concordion-markdown-extension/spec/concordion/markdown/Configuration.html) to output the source HTML that is generated, and to add extensions to the Markdown language.

## Editor Support
### IDEA
For IntelliJ IDEA, the official Markdown editor does not support tables. We recommend the [Markdown](https://plugins.jetbrains.com/plugin?id=5970) plugin, which uses the same underlying Pegdown library as this extension. After installing the plugin, you will need to configure the [settings](https://plugins.jetbrains.com/files/5970/screenshot_14568.png) to enable Tables and Strikethrough, plus any additional Markdown language extensions that you [configure](http://concordion.github.io/concordion-markdown-extension/spec/concordion/markdown/Configuration.html).

### Eclipse
Plugins include:

| Plugin | Has editor? | Has viewer? | Viewer supports tables and strikethrough |
|--------|:-----------:|:-----------:|:----------------------------------------:|
|Mylyn Wikitext Editor| Y | Y | N |
|[Markdown Text Editor](https://marketplace.eclipse.org/content/markdown-text-editor)| Y | N | - |
|[Github Flavored Markdown Viewer](https://marketplace.eclipse.org/content/github-flavored-markdown-viewer-plugin)| N | Y | Y |

In order to get editing features and the ability to view with tables and strikethrough, you may want to install either of the first 2 editor plugins listed along with the viewer plugin.