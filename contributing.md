# Contributing to Atom

:+1::tada: First off, thanks for taking the time to contribute! :tada::+1:

The following is a set of guidelines for contributing to Atom and its packages, which are hosted in the [Atom Organization](https://github.com/atom) on GitHub. These are mostly guidelines, not rules. Use your best judgment, and feel free to propose changes to this document in a pull request.

#### Table Of Contents

[Code of Conduct](contributing.md#code-of-conduct)

[I don't want to read this whole thing, I just have a question!!!](contributing.md#i-dont-want-to-read-this-whole-thing-i-just-have-a-question)

[What should I know before I get started?](contributing.md#what-should-i-know-before-i-get-started)

* [Atom and Packages](contributing.md#atom-and-packages)
* [Atom Design Decisions](contributing.md#design-decisions)

[How Can I Contribute?](contributing.md#how-can-i-contribute)

* [Reporting Bugs](contributing.md#reporting-bugs)
* [Suggesting Enhancements](contributing.md#suggesting-enhancements)
* [Your First Code Contribution](contributing.md#your-first-code-contribution)
* [Pull Requests](contributing.md#pull-requests)

[Styleguides](contributing.md#styleguides)

* [Git Commit Messages](contributing.md#git-commit-messages)
* [JavaScript Styleguide](contributing.md#javascript-styleguide)
* [CoffeeScript Styleguide](contributing.md#coffeescript-styleguide)
* [Specs Styleguide](contributing.md#specs-styleguide)
* [Documentation Styleguide](contributing.md#documentation-styleguide)

[Additional Notes](contributing.md#additional-notes)

* [Issue and Pull Request Labels](contributing.md#issue-and-pull-request-labels)

## Code of Conduct

This project and everyone participating in it is governed by the [Atom Code of Conduct](code_of_conduct.md). By participating, you are expected to uphold this code. Please report unacceptable behavior to [atom@github.com](mailto:atom@github.com).

## I don't want to read this whole thing I just have a question!!!

> **Note:** [Please don't file an issue to ask a question.](https://blog.atom.io/2016/04/19/managing-the-deluge-of-atom-issues.html) You'll get faster results by using the resources below.

We have an official message board with a detailed FAQ and where the community chimes in with helpful advice if you have questions.

* [Discuss, the official Atom and Electron message board](https://discuss.atom.io)
* [Atom FAQ](https://discuss.atom.io/c/faq)

If chat is more your speed, you can join the Atom and Electron Slack team:

* [Join the Atom and Electron Slack Team](https://atom-slack.herokuapp.com/)
  * Even though Slack is a chat service, sometimes it takes several hours for community members to respond — please be patient!
  * Use the `#atom` channel for general questions or discussion about Atom
  * Use the `#electron` channel for questions about Electron
  * Use the `#packages` channel for questions or discussion about writing or contributing to Atom packages \(both core and community\)
  * Use the `#ui` channel for questions and discussion about Atom UI and themes
  * There are many other channels available, check the channel list

## What should I know before I get started?

### Atom and Packages

Atom is a large open source project — it's made up of over [200 repositories](https://github.com/atom). When you initially consider contributing to Atom, you might be unsure about which of those 200 repositories implements the functionality you want to change or report a bug for. This section should help you with that.

Atom is intentionally very modular. Nearly every non-editor UI element you interact with comes from a package, even fundamental things like tabs and the status-bar. These packages are packages in the same way that packages in the [Atom package repository](https://atom.io/packages) are packages, with one difference: they are bundled into the [default distribution](https://github.com/atom/atom/blob/10b8de6fc499a7def9b072739486e68530d67ab4/package.json#L58).

![atom-packages](https://cloud.githubusercontent.com/assets/69169/10472281/84fc9792-71d3-11e5-9fd1-19da717df079.png)

To get a sense for the packages that are bundled with Atom, you can go to `Settings` &gt; `Packages` within Atom and take a look at the Core Packages section.

Here's a list of the big ones:

* [atom/atom](https://github.com/atom/atom) - Atom Core! The core editor component is responsible for basic text editing \(e.g. cursors, selections, scrolling\), text indentation, wrapping, and folding, text rendering, editor rendering, file system operations \(e.g. saving\), and installation and auto-updating. You should also use this repository for feedback related to the [Atom API](https://atom.io/docs/api/latest) and for large, overarching design proposals.
* [tree-view](https://github.com/atom/tree-view) - file and directory listing on the left of the UI.
* [fuzzy-finder](https://github.com/atom/fuzzy-finder) - the quick file opener.
* [find-and-replace](https://github.com/atom/find-and-replace) - all search and replace functionality.
* [tabs](https://github.com/atom/tabs) - the tabs for open editors at the top of the UI.
* [status-bar](https://github.com/atom/status-bar) - the status bar at the bottom of the UI.
* [markdown-preview](https://github.com/atom/markdown-preview) - the rendered markdown pane item.
* [settings-view](https://github.com/atom/settings-view) - the settings UI pane item.
* [autocomplete-plus](https://github.com/atom/autocomplete-plus) - autocompletions shown while typing. Some languages have additional packages for autocompletion functionality, such as [autocomplete-html](https://github.com/atom/autocomplete-html).
* [git-diff](https://github.com/atom/git-diff) - Git change indicators shown in the editor's gutter.
* [language-javascript](https://github.com/atom/language-javascript) - all bundled languages are packages too, and each one has a separate package `language-[name]`. Use these for feedback on syntax highlighting issues that only appear for a specific language.
* [one-dark-ui](https://github.com/atom/one-dark-ui) - the default UI styling for anything but the text editor. UI theme packages \(i.e. packages with a `-ui` suffix\) provide only styling and it's possible that a bundled package is responsible for a UI issue. There are other bundled UI themes, such as [one-light-ui](https://github.com/atom/one-light-ui).
* [one-dark-syntax](https://github.com/atom/one-dark-syntax) - the default syntax highlighting styles applied for all languages. There are other bundled syntax themes, such as [solarized-dark-syntax](https://github.com/atom/solarized-dark-syntax). You should use these packages for reporting issues that appear in many languages, but disappear if you change to another syntax theme.
* [apm](https://github.com/atom/apm) - the `apm` command line tool \(Atom Package Manager\). You should use this repository for any contributions related to the `apm` tool and to publishing packages.
* [atom.io](https://github.com/atom/atom.io) - the repository for feedback on the [Atom.io website](https://atom.io) and the [Atom.io package API](https://github.com/atom/atom/blob/master/docs/apm-rest-api.md) used by [apm](https://github.com/atom/apm).

There are many more, but this list should be a good starting point. For more information on how to work with Atom's official packages, see [Contributing to Atom Packages](https://flight-manual.atom.io/hacking-atom/sections/contributing-to-official-atom-packages/).

Also, because Atom is so extensible, it's possible that a feature you've become accustomed to in Atom or an issue you're encountering isn't coming from a bundled package at all, but rather a [community package](https://atom.io/packages) you've installed. Each community package has its own repository too, the [Atom FAQ](https://discuss.atom.io/c/faq) has instructions on how to [contact the maintainers of any Atom community package or theme.](https://discuss.atom.io/t/i-have-a-question-about-a-specific-atom-community-package-where-is-the-best-place-to-ask-it/25581)

#### Package Conventions

There are a few conventions that have developed over time around packages:

* Packages that add one or more syntax highlighting grammars are named `language-[language-name]`
  * Language packages can add other things besides just a grammar. Many offer commonly-used snippets. Try not to add too much though.
* Theme packages are split into two categories: UI and Syntax themes
  * UI themes are named `[theme-name]-ui`
  * Syntax themes are named `[theme-name]-syntax`
  * Often themes that are designed to work together are given the same root name, for example: `one-dark-ui` and `one-dark-syntax`
  * UI themes style everything outside of the editor pane — all of the green areas in the [packages image above](contributing.md#atom-packages-image)
  * Syntax themes style just the items inside the editor pane, mostly syntax highlighting
* Packages that add [autocomplete providers](https://github.com/atom/autocomplete-plus/wiki/Autocomplete-Providers) are named `autocomplete-[what-they-autocomplete]` — ex: [autocomplete-css](https://github.com/atom/autocomplete-css)

### Design Decisions

When we make a significant decision in how we maintain the project and what we can or cannot support, we will document it in the [atom/design-decisions repository](https://github.com/atom/design-decisions). If you have a question around how we do things, check to see if it is documented there. If it is _not_ documented there, please open a new topic on [Discuss, the official Atom message board](https://discuss.atom.io) and ask your question.

## How Can I Contribute?

### Reporting Bugs

This section guides you through submitting a bug report for Atom. Following these guidelines helps maintainers and the community understand your report :pencil:, reproduce the behavior :computer: :computer:, and find related reports :mag\_right:.

Before creating bug reports, please check [this list](contributing.md#before-submitting-a-bug-report) as you might find out that you don't need to create one. When you are creating a bug report, please [include as many details as possible](contributing.md#how-do-i-submit-a-good-bug-report). Fill out [the required template](https://github.com/atom/.github/blob/master/.github/ISSUE_TEMPLATE/bug_report.md), the information it asks for helps us resolve issues faster.

> **Note:** If you find a **Closed** issue that seems like it is the same thing that you're experiencing, open a new issue and include a link to the original issue in the body of your new one.

#### Before Submitting A Bug Report

* **Check the** [**debugging guide**](https://flight-manual.atom.io/hacking-atom/sections/debugging/)**.** You might be able to find the cause of the problem and fix things yourself. Most importantly, check if you can reproduce the problem [in the latest version of Atom](https://flight-manual.atom.io/hacking-atom/sections/debugging/#update-to-the-latest-version), if the problem happens when you run Atom in [safe mode](https://flight-manual.atom.io/hacking-atom/sections/debugging/#check-if-the-problem-shows-up-in-safe-mode), and if you can get the desired behavior by changing [Atom's or packages' config settings](https://flight-manual.atom.io/hacking-atom/sections/debugging/#check-atom-and-package-settings).
* **Check the** [**FAQs on the forum**](https://discuss.atom.io/c/faq) for a list of common questions and problems.
* **Determine** [**which repository the problem should be reported in**](contributing.md#atom-and-packages).
* **Perform a** [**cursory search**](https://github.com/search?q=+is%3Aissue+user%3Aatom) to see if the problem has already been reported. If it has **and the issue is still open**, add a comment to the existing issue instead of opening a new one.

#### How Do I Submit A \(Good\) Bug Report?

Bugs are tracked as [GitHub issues](https://guides.github.com/features/issues/). After you've determined [which repository](contributing.md#atom-and-packages) your bug is related to, create an issue on that repository and provide the following information by filling in [the template](https://github.com/atom/.github/blob/master/.github/ISSUE_TEMPLATE/bug_report.md).

Explain the problem and include additional details to help maintainers reproduce the problem:

* **Use a clear and descriptive title** for the issue to identify the problem.
* **Describe the exact steps which reproduce the problem** in as many details as possible. For example, start by explaining how you started Atom, e.g. which command exactly you used in the terminal, or how you started Atom otherwise. When listing steps, **don't just say what you did, but explain how you did it**. For example, if you moved the cursor to the end of a line, explain if you used the mouse, or a keyboard shortcut or an Atom command, and if so which one?
* **Provide specific examples to demonstrate the steps**. Include links to files or GitHub projects, or copy/pasteable snippets, which you use in those examples. If you're providing snippets in the issue, use [Markdown code blocks](https://help.github.com/articles/markdown-basics/#multiple-lines).
* **Describe the behavior you observed after following the steps** and point out what exactly is the problem with that behavior.
* **Explain which behavior you expected to see instead and why.**
* **Include screenshots and animated GIFs** which show you following the described steps and clearly demonstrate the problem. If you use the keyboard while following the steps, **record the GIF with the** [**Keybinding Resolver**](https://github.com/atom/keybinding-resolver) **shown**. You can use [this tool](https://www.cockos.com/licecap/) to record GIFs on macOS and Windows, and [this tool](https://github.com/colinkeenan/silentcast) or [this tool](https://github.com/GNOME/byzanz) on Linux.
* **If you're reporting that Atom crashed**, include a crash report with a stack trace from the operating system. On macOS, the crash report will be available in `Console.app` under "Diagnostic and usage information" &gt; "User diagnostic reports". Include the crash report in the issue in a [code block](https://help.github.com/articles/markdown-basics/#multiple-lines), a [file attachment](https://help.github.com/articles/file-attachments-on-issues-and-pull-requests/), or put it in a [gist](https://gist.github.com/) and provide link to that gist.
* **If the problem is related to performance or memory**, include a [CPU profile capture](https://flight-manual.atom.io/hacking-atom/sections/debugging/#diagnose-runtime-performance) with your report.
* **If Chrome's developer tools pane is shown without you triggering it**, that normally means that you have a syntax error in one of your themes or in your `styles.less`. Try running in [Safe Mode](https://flight-manual.atom.io/hacking-atom/sections/debugging/#using-safe-mode) and using a different theme or comment out the contents of your `styles.less` to see if that fixes the problem.
* **If the problem wasn't triggered by a specific action**, describe what you were doing before the problem happened and share more information using the guidelines below.

Provide more context by answering these questions:

* **Can you reproduce the problem in** [**safe mode**](https://flight-manual.atom.io/hacking-atom/sections/debugging/#diagnose-runtime-performance-problems-with-the-dev-tools-cpu-profiler)**?**
* **Did the problem start happening recently** \(e.g. after updating to a new version of Atom\) or was this always a problem?
* If the problem started happening recently, **can you reproduce the problem in an older version of Atom?** What's the most recent version in which the problem doesn't happen? You can download older versions of Atom from [the releases page](https://github.com/atom/atom/releases).
* **Can you reliably reproduce the issue?** If not, provide details about how often the problem happens and under which conditions it normally happens.
* If the problem is related to working with files \(e.g. opening and editing files\), **does the problem happen for all files and projects or only some?** Does the problem happen only when working with local or remote files \(e.g. on network drives\), with files of a specific type \(e.g. only JavaScript or Python files\), with large files or files with very long lines, or with files in a specific encoding? Is there anything else special about the files you are using?

Include details about your configuration and environment:

* **Which version of Atom are you using?** You can get the exact version by running `atom -v` in your terminal, or by starting Atom and running the `Application: About` command from the [Command Palette](https://github.com/atom/command-palette).
* **What's the name and version of the OS you're using**?
* **Are you running Atom in a virtual machine?** If so, which VM software are you using and which operating systems and versions are used for the host and the guest?
* **Which** [**packages**](contributing.md#atom-and-packages) **do you have installed?** You can get that list by running `apm list --installed`.
* **Are you using** [**local configuration files**](https://flight-manual.atom.io/using-atom/sections/basic-customization/) `config.cson`, `keymap.cson`, `snippets.cson`, `styles.less` and `init.coffee` to customize Atom? If so, provide the contents of those files, preferably in a [code block](https://help.github.com/articles/markdown-basics/#multiple-lines) or with a link to a [gist](https://gist.github.com/).
* **Are you using Atom with multiple monitors?** If so, can you reproduce the problem when you use a single monitor?
* **Which keyboard layout are you using?** Are you using a US layout or some other layout?

### Suggesting Enhancements

This section guides you through submitting an enhancement suggestion for Atom, including completely new features and minor improvements to existing functionality. Following these guidelines helps maintainers and the community understand your suggestion :pencil: and find related suggestions :mag\_right:.

Before creating enhancement suggestions, please check [this list](contributing.md#before-submitting-an-enhancement-suggestion) as you might find out that you don't need to create one. When you are creating an enhancement suggestion, please [include as many details as possible](contributing.md#how-do-i-submit-a-good-enhancement-suggestion). Fill in [the template](https://github.com/atom/.github/blob/master/.github/ISSUE_TEMPLATE/feature_request.md), including the steps that you imagine you would take if the feature you're requesting existed.

#### Before Submitting An Enhancement Suggestion

* **Check the** [**debugging guide**](https://flight-manual.atom.io/hacking-atom/sections/debugging/) for tips — you might discover that the enhancement is already available. Most importantly, check if you're using [the latest version of Atom](https://flight-manual.atom.io/hacking-atom/sections/debugging/#update-to-the-latest-version) and if you can get the desired behavior by changing [Atom's or packages' config settings](https://flight-manual.atom.io/hacking-atom/sections/debugging/#check-atom-and-package-settings).
* **Check if there's already** [**a package**](https://atom.io/packages) **which provides that enhancement.**
* **Determine** [**which repository the enhancement should be suggested in**](contributing.md#atom-and-packages)**.**
* **Perform a** [**cursory search**](https://github.com/search?q=+is%3Aissue+user%3Aatom) to see if the enhancement has already been suggested. If it has, add a comment to the existing issue instead of opening a new one.

#### How Do I Submit A \(Good\) Enhancement Suggestion?

Enhancement suggestions are tracked as [GitHub issues](https://guides.github.com/features/issues/). After you've determined [which repository](contributing.md#atom-and-packages) your enhancement suggestion is related to, create an issue on that repository and provide the following information:

* **Use a clear and descriptive title** for the issue to identify the suggestion.
* **Provide a step-by-step description of the suggested enhancement** in as many details as possible.
* **Provide specific examples to demonstrate the steps**. Include copy/pasteable snippets which you use in those examples, as [Markdown code blocks](https://help.github.com/articles/markdown-basics/#multiple-lines).
* **Describe the current behavior** and **explain which behavior you expected to see instead** and why.
* **Include screenshots and animated GIFs** which help you demonstrate the steps or point out the part of Atom which the suggestion is related to. You can use [this tool](https://www.cockos.com/licecap/) to record GIFs on macOS and Windows, and [this tool](https://github.com/colinkeenan/silentcast) or [this tool](https://github.com/GNOME/byzanz) on Linux.
* **Explain why this enhancement would be useful** to most Atom users and isn't something that can or should be implemented as a [community package](contributing.md#atom-and-packages).
* **List some other text editors or applications where this enhancement exists.**
* **Specify which version of Atom you're using.** You can get the exact version by running `atom -v` in your terminal, or by starting Atom and running the `Application: About` command from the [Command Palette](https://github.com/atom/command-palette).
* **Specify the name and version of the OS you're using.**

### Your First Code Contribution

Unsure where to begin contributing to Atom? You can start by looking through these `beginner` and `help-wanted` issues:

* [Beginner issues](https://github.com/search?utf8=%E2%9C%93&q=is%3Aopen+is%3Aissue+label%3Abeginner+label%3Ahelp-wanted+user%3Aatom+sort%3Acomments-desc) - issues which should only require a few lines of code, and a test or two.
* [Help wanted issues](https://github.com/search?q=is%3Aopen+is%3Aissue+label%3Ahelp-wanted+user%3Aatom+sort%3Acomments-desc+-label%3Abeginner) - issues which should be a bit more involved than `beginner` issues.

Both issue lists are sorted by total number of comments. While not perfect, number of comments is a reasonable proxy for impact a given change will have.

If you want to read about using Atom or developing packages in Atom, the [Atom Flight Manual](https://flight-manual.atom.io) is free and available online. You can find the source to the manual in [atom/flight-manual.atom.io](https://github.com/atom/flight-manual.atom.io).

#### Local development

Atom Core and all packages can be developed locally. For instructions on how to do this, see the following sections in the [Atom Flight Manual](https://flight-manual.atom.io):

* [Hacking on Atom Core](https://flight-manual.atom.io/hacking-atom/sections/hacking-on-atom-core/)
* [Contributing to Official Atom Packages](https://flight-manual.atom.io/hacking-atom/sections/contributing-to-official-atom-packages/)

### Pull Requests

The process described here has several goals:

* Maintain Atom's quality
* Fix problems that are important to users
* Engage the community in working toward the best possible Atom
* Enable a sustainable system for Atom's maintainers to review contributions

Please follow these steps to have your contribution considered by the maintainers:

1. Follow all instructions in [the template](pull_request_template.md)
2. Follow the [styleguides](contributing.md#styleguides)
3. After you submit your pull request, verify that all [status checks](https://help.github.com/articles/about-status-checks/) are passing What if the status checks are failing?If a status check is failing, and you believe that the failure is unrelated to your change, please leave a comment on the pull request explaining why you believe the failure is unrelated. A maintainer will re-run the status check for you. If we conclude that the failure was a false positive, then we will open an issue to track that problem with our status check suite.

While the prerequisites above must be satisfied prior to having your pull request reviewed, the reviewer\(s\) may ask you to complete additional design work, tests, or other changes before your pull request can be ultimately accepted.

## Styleguides

### Git Commit Messages

* Use the present tense \("Add feature" not "Added feature"\)
* Use the imperative mood \("Move cursor to..." not "Moves cursor to..."\)
* Limit the first line to 72 characters or less
* Reference issues and pull requests liberally after the first line
* When only changing documentation, include `[ci skip]` in the commit title
* Consider starting the commit message with an applicable emoji:
  * :art: `:art:` when improving the format/structure of the code
  * :racehorse: `:racehorse:` when improving performance
  * :non-potable\_water: `:non-potable_water:` when plugging memory leaks
  * :memo: `:memo:` when writing docs
  * :penguin: `:penguin:` when fixing something on Linux
  * :apple: `:apple:` when fixing something on macOS
  * :checkered\_flag: `:checkered_flag:` when fixing something on Windows
  * :bug: `:bug:` when fixing a bug
  * :fire: `:fire:` when removing code or files
  * :green\_heart: `:green_heart:` when fixing the CI build
  * :white\_check\_mark: `:white_check_mark:` when adding tests
  * :lock: `:lock:` when dealing with security
  * :arrow\_up: `:arrow_up:` when upgrading dependencies
  * :arrow\_down: `:arrow_down:` when downgrading dependencies
  * :shirt: `:shirt:` when removing linter warnings

### JavaScript Styleguide

All JavaScript must adhere to [JavaScript Standard Style](https://standardjs.com/).

* Prefer the object spread operator \(`{...anotherObj}`\) to `Object.assign()`
* Inline `export`s with expressions whenever possible

  ```javascript
  // Use this:
  export default class ClassName {

  }

  // Instead of:
  class ClassName {

  }
  export default ClassName
  ```

* Place requires in the following order:
  * Built in Node Modules \(such as `path`\)
  * Built in Atom and Electron Modules \(such as `atom`, `remote`\)
  * Local Modules \(using relative paths\)
* Place class properties in the following order:
  * Class methods and properties \(methods starting with `static`\)
  * Instance methods and properties
* [Avoid platform-dependent code](https://flight-manual.atom.io/hacking-atom/sections/cross-platform-compatibility/)

### CoffeeScript Styleguide

* Set parameter defaults without spaces around the equal sign
  * `clear = (count=1) ->` instead of `clear = (count = 1) ->`
* Use spaces around operators
  * `count + 1` instead of `count+1`
* Use spaces after commas \(unless separated by newlines\)
* Use parentheses if it improves code clarity.
* Prefer alphabetic keywords to symbolic keywords:
  * `a is b` instead of `a == b`
* Avoid spaces inside the curly-braces of hash literals:
  * `{a: 1, b: 2}` instead of `{ a: 1, b: 2 }`
* Include a single line of whitespace between methods.
* Capitalize initialisms and acronyms in names, except for the first word, which

  should be lower-case:

  * `getURI` instead of `getUri`
  * `uriToOpen` instead of `URIToOpen`

* Use `slice()` to copy an array
* Add an explicit `return` when your function ends with a `for`/`while` loop and

  you don't want it to return a collected array.

* Use `this` instead of a standalone `@`
  * `return this` instead of `return @`
* Place requires in the following order:
  * Built in Node Modules \(such as `path`\)
  * Built in Atom and Electron Modules \(such as `atom`, `remote`\)
  * Local Modules \(using relative paths\)
* Place class properties in the following order:
  * Class methods and properties \(methods starting with a `@`\)
  * Instance methods and properties
* [Avoid platform-dependent code](https://flight-manual.atom.io/hacking-atom/sections/cross-platform-compatibility/)

### Specs Styleguide

* Include thoughtfully-worded, well-structured [Jasmine](https://jasmine.github.io/) specs in the `./spec` folder.
* Treat `describe` as a noun or situation.
* Treat `it` as a statement about state or how an operation changes state.

#### Example

```coffeescript
describe 'a dog', ->
 it 'barks', ->
 # spec here
 describe 'when the dog is happy', ->
  it 'wags its tail', ->
  # spec here
```

### Documentation Styleguide

* Use [AtomDoc](https://github.com/atom/atomdoc).
* Use [Markdown](https://daringfireball.net/projects/markdown).
* Reference methods and classes in markdown with the custom `{}` notation:
  * Reference classes with `{ClassName}`
  * Reference instance methods with `{ClassName::methodName}`
  * Reference class methods with `{ClassName.methodName}`

#### Example

```coffeescript
# Public: Disable the package with the given name.
#
# * `name`    The {String} name of the package to disable.
# * `options` (optional) The {Object} with disable options (default: {}):
#   * `trackTime`     A {Boolean}, `true` to track the amount of time taken.
#   * `ignoreErrors`  A {Boolean}, `true` to catch and ignore errors thrown.
# * `callback` The {Function} to call after the package has been disabled.
#
# Returns `undefined`.
disablePackage: (name, options, callback) ->
```

## Additional Notes

### Issue and Pull Request Labels

This section lists the labels we use to help us track and manage issues and pull requests. Most labels are used across all Atom repositories, but some are specific to `atom/atom`.

[GitHub search](https://help.github.com/articles/searching-issues/) makes it easy to use labels for finding groups of issues or pull requests you're interested in. For example, you might be interested in [open issues across `atom/atom` and all Atom-owned packages which are labeled as bugs, but still need to be reliably reproduced](https://github.com/search?utf8=%E2%9C%93&q=is%3Aopen+is%3Aissue+user%3Aatom+label%3Abug+label%3Aneeds-reproduction) or perhaps [open pull requests in `atom/atom` which haven't been reviewed yet](https://github.com/search?utf8=%E2%9C%93&q=is%3Aopen+is%3Apr+repo%3Aatom%2Fatom+comments%3A0). To help you find issues and pull requests, each label is listed with search links for finding open items with that label in `atom/atom` only and also across all Atom repositories. We encourage you to read about [other search filters](https://help.github.com/articles/searching-issues/) which will help you write more focused queries.

The labels are loosely grouped by their purpose, but it's not required that every issue have a label from every group or that an issue can't have more than one label from the same group.

Please open an issue on `atom/atom` if you have suggestions for new labels, and if you notice some labels are missing on some repositories, then please open an issue on that repository.

#### Type of Issue and Issue State

| Label name | `atom/atom` :mag\_right: | `atom`‑org :mag\_right: | Description |
| :--- | :--- | :--- | :--- |
| `enhancement` | [search](https://github.com/search?q=is%3Aopen+is%3Aissue+repo%3Aatom%2Fatom+label%3Aenhancement) | [search](https://github.com/search?q=is%3Aopen+is%3Aissue+user%3Aatom+label%3Aenhancement) | Feature requests. |
| `bug` | [search](https://github.com/search?q=is%3Aopen+is%3Aissue+repo%3Aatom%2Fatom+label%3Abug) | [search](https://github.com/search?q=is%3Aopen+is%3Aissue+user%3Aatom+label%3Abug) | Confirmed bugs or reports that are very likely to be bugs. |
| `question` | [search](https://github.com/search?q=is%3Aopen+is%3Aissue+repo%3Aatom%2Fatom+label%3Aquestion) | [search](https://github.com/search?q=is%3Aopen+is%3Aissue+user%3Aatom+label%3Aquestion) | Questions more than bug reports or feature requests \(e.g. how do I do X\). |
| `feedback` | [search](https://github.com/search?q=is%3Aopen+is%3Aissue+repo%3Aatom%2Fatom+label%3Afeedback) | [search](https://github.com/search?q=is%3Aopen+is%3Aissue+user%3Aatom+label%3Afeedback) | General feedback more than bug reports or feature requests. |
| `help-wanted` | [search](https://github.com/search?q=is%3Aopen+is%3Aissue+repo%3Aatom%2Fatom+label%3Ahelp-wanted) | [search](https://github.com/search?q=is%3Aopen+is%3Aissue+user%3Aatom+label%3Ahelp-wanted) | The Atom core team would appreciate help from the community in resolving these issues. |
| `beginner` | [search](https://github.com/search?q=is%3Aopen+is%3Aissue+repo%3Aatom%2Fatom+label%3Abeginner) | [search](https://github.com/search?q=is%3Aopen+is%3Aissue+user%3Aatom+label%3Abeginner) | Less complex issues which would be good first issues to work on for users who want to contribute to Atom. |
| `more-information-needed` | [search](https://github.com/search?q=is%3Aopen+is%3Aissue+repo%3Aatom%2Fatom+label%3Amore-information-needed) | [search](https://github.com/search?q=is%3Aopen+is%3Aissue+user%3Aatom+label%3Amore-information-needed) | More information needs to be collected about these problems or feature requests \(e.g. steps to reproduce\). |
| `needs-reproduction` | [search](https://github.com/search?q=is%3Aopen+is%3Aissue+repo%3Aatom%2Fatom+label%3Aneeds-reproduction) | [search](https://github.com/search?q=is%3Aopen+is%3Aissue+user%3Aatom+label%3Aneeds-reproduction) | Likely bugs, but haven't been reliably reproduced. |
| `blocked` | [search](https://github.com/search?q=is%3Aopen+is%3Aissue+repo%3Aatom%2Fatom+label%3Ablocked) | [search](https://github.com/search?q=is%3Aopen+is%3Aissue+user%3Aatom+label%3Ablocked) | Issues blocked on other issues. |
| `duplicate` | [search](https://github.com/search?q=is%3Aopen+is%3Aissue+repo%3Aatom%2Fatom+label%3Aduplicate) | [search](https://github.com/search?q=is%3Aopen+is%3Aissue+user%3Aatom+label%3Aduplicate) | Issues which are duplicates of other issues, i.e. they have been reported before. |
| `wontfix` | [search](https://github.com/search?q=is%3Aopen+is%3Aissue+repo%3Aatom%2Fatom+label%3Awontfix) | [search](https://github.com/search?q=is%3Aopen+is%3Aissue+user%3Aatom+label%3Awontfix) | The Atom core team has decided not to fix these issues for now, either because they're working as intended or for some other reason. |
| `invalid` | [search](https://github.com/search?q=is%3Aopen+is%3Aissue+repo%3Aatom%2Fatom+label%3Ainvalid) | [search](https://github.com/search?q=is%3Aopen+is%3Aissue+user%3Aatom+label%3Ainvalid) | Issues which aren't valid \(e.g. user errors\). |
| `package-idea` | [search](https://github.com/search?q=is%3Aopen+is%3Aissue+repo%3Aatom%2Fatom+label%3Apackage-idea) | [search](https://github.com/search?q=is%3Aopen+is%3Aissue+user%3Aatom+label%3Apackage-idea) | Feature request which might be good candidates for new packages, instead of extending Atom or core Atom packages. |
| `wrong-repo` | [search](https://github.com/search?q=is%3Aopen+is%3Aissue+repo%3Aatom%2Fatom+label%3Awrong-repo) | [search](https://github.com/search?q=is%3Aopen+is%3Aissue+user%3Aatom+label%3Awrong-repo) | Issues reported on the wrong repository \(e.g. a bug related to the [Settings View package](https://github.com/atom/settings-view) was reported on [Atom core](https://github.com/atom/atom)\). |

#### Topic Categories

| Label name | `atom/atom` :mag\_right: | `atom`‑org :mag\_right: | Description |
| :--- | :--- | :--- | :--- |
| `windows` | [search](https://github.com/search?q=is%3Aopen+is%3Aissue+repo%3Aatom%2Fatom+label%3Awindows) | [search](https://github.com/search?q=is%3Aopen+is%3Aissue+user%3Aatom+label%3Awindows) | Related to Atom running on Windows. |
| `linux` | [search](https://github.com/search?q=is%3Aopen+is%3Aissue+repo%3Aatom%2Fatom+label%3Alinux) | [search](https://github.com/search?q=is%3Aopen+is%3Aissue+user%3Aatom+label%3Alinux) | Related to Atom running on Linux. |
| `mac` | [search](https://github.com/search?q=is%3Aopen+is%3Aissue+repo%3Aatom%2Fatom+label%3Amac) | [search](https://github.com/search?q=is%3Aopen+is%3Aissue+user%3Aatom+label%3Amac) | Related to Atom running on macOS. |
| `documentation` | [search](https://github.com/search?q=is%3Aopen+is%3Aissue+repo%3Aatom%2Fatom+label%3Adocumentation) | [search](https://github.com/search?q=is%3Aopen+is%3Aissue+user%3Aatom+label%3Adocumentation) | Related to any type of documentation \(e.g. [API documentation](https://atom.io/docs/api/latest/) and the [flight manual](https://flight-manual.atom.io/)\). |
| `performance` | [search](https://github.com/search?q=is%3Aopen+is%3Aissue+repo%3Aatom%2Fatom+label%3Aperformance) | [search](https://github.com/search?q=is%3Aopen+is%3Aissue+user%3Aatom+label%3Aperformance) | Related to performance. |
| `security` | [search](https://github.com/search?q=is%3Aopen+is%3Aissue+repo%3Aatom%2Fatom+label%3Asecurity) | [search](https://github.com/search?q=is%3Aopen+is%3Aissue+user%3Aatom+label%3Asecurity) | Related to security. |
| `ui` | [search](https://github.com/search?q=is%3Aopen+is%3Aissue+repo%3Aatom%2Fatom+label%3Aui) | [search](https://github.com/search?q=is%3Aopen+is%3Aissue+user%3Aatom+label%3Aui) | Related to visual design. |
| `api` | [search](https://github.com/search?q=is%3Aopen+is%3Aissue+repo%3Aatom%2Fatom+label%3Aapi) | [search](https://github.com/search?q=is%3Aopen+is%3Aissue+user%3Aatom+label%3Aapi) | Related to Atom's public APIs. |
| `uncaught-exception` | [search](https://github.com/search?q=is%3Aopen+is%3Aissue+repo%3Aatom%2Fatom+label%3Auncaught-exception) | [search](https://github.com/search?q=is%3Aopen+is%3Aissue+user%3Aatom+label%3Auncaught-exception) | Issues about uncaught exceptions, normally created from the [Notifications package](https://github.com/atom/notifications). |
| `crash` | [search](https://github.com/search?q=is%3Aopen+is%3Aissue+repo%3Aatom%2Fatom+label%3Acrash) | [search](https://github.com/search?q=is%3Aopen+is%3Aissue+user%3Aatom+label%3Acrash) | Reports of Atom completely crashing. |
| `auto-indent` | [search](https://github.com/search?q=is%3Aopen+is%3Aissue+repo%3Aatom%2Fatom+label%3Aauto-indent) | [search](https://github.com/search?q=is%3Aopen+is%3Aissue+user%3Aatom+label%3Aauto-indent) | Related to auto-indenting text. |
| `encoding` | [search](https://github.com/search?q=is%3Aopen+is%3Aissue+repo%3Aatom%2Fatom+label%3Aencoding) | [search](https://github.com/search?q=is%3Aopen+is%3Aissue+user%3Aatom+label%3Aencoding) | Related to character encoding. |
| `network` | [search](https://github.com/search?q=is%3Aopen+is%3Aissue+repo%3Aatom%2Fatom+label%3Anetwork) | [search](https://github.com/search?q=is%3Aopen+is%3Aissue+user%3Aatom+label%3Anetwork) | Related to network problems or working with remote files \(e.g. on network drives\). |
| `git` | [search](https://github.com/search?q=is%3Aopen+is%3Aissue+repo%3Aatom%2Fatom+label%3Agit) | [search](https://github.com/search?q=is%3Aopen+is%3Aissue+user%3Aatom+label%3Agit) | Related to Git functionality \(e.g. problems with gitignore files or with showing the correct file status\). |

#### `atom/atom` Topic Categories

| Label name | `atom/atom` :mag\_right: | `atom`‑org :mag\_right: | Description |
| :--- | :--- | :--- | :--- |
| `editor-rendering` | [search](https://github.com/search?q=is%3Aopen+is%3Aissue+repo%3Aatom%2Fatom+label%3Aeditor-rendering) | [search](https://github.com/search?q=is%3Aopen+is%3Aissue+user%3Aatom+label%3Aeditor-rendering) | Related to language-independent aspects of rendering text \(e.g. scrolling, soft wrap, and font rendering\). |
| `build-error` | [search](https://github.com/search?q=is%3Aopen+is%3Aissue+repo%3Aatom%2Fatom+label%3Abuild-error) | [search](https://github.com/search?q=is%3Aopen+is%3Aissue+user%3Aatom+label%3Abuild-error) | Related to problems with building Atom from source. |
| `error-from-pathwatcher` | [search](https://github.com/search?q=is%3Aopen+is%3Aissue+repo%3Aatom%2Fatom+label%3Aerror-from-pathwatcher) | [search](https://github.com/search?q=is%3Aopen+is%3Aissue+user%3Aatom+label%3Aerror-from-pathwatcher) | Related to errors thrown by the [pathwatcher library](https://github.com/atom/node-pathwatcher). |
| `error-from-save` | [search](https://github.com/search?q=is%3Aopen+is%3Aissue+repo%3Aatom%2Fatom+label%3Aerror-from-save) | [search](https://github.com/search?q=is%3Aopen+is%3Aissue+user%3Aatom+label%3Aerror-from-save) | Related to errors thrown when saving files. |
| `error-from-open` | [search](https://github.com/search?q=is%3Aopen+is%3Aissue+repo%3Aatom%2Fatom+label%3Aerror-from-open) | [search](https://github.com/search?q=is%3Aopen+is%3Aissue+user%3Aatom+label%3Aerror-from-open) | Related to errors thrown when opening files. |
| `installer` | [search](https://github.com/search?q=is%3Aopen+is%3Aissue+repo%3Aatom%2Fatom+label%3Ainstaller) | [search](https://github.com/search?q=is%3Aopen+is%3Aissue+user%3Aatom+label%3Ainstaller) | Related to the Atom installers for different OSes. |
| `auto-updater` | [search](https://github.com/search?q=is%3Aopen+is%3Aissue+repo%3Aatom%2Fatom+label%3Aauto-updater) | [search](https://github.com/search?q=is%3Aopen+is%3Aissue+user%3Aatom+label%3Aauto-updater) | Related to the auto-updater for different OSes. |
| `deprecation-help` | [search](https://github.com/search?q=is%3Aopen+is%3Aissue+repo%3Aatom%2Fatom+label%3Adeprecation-help) | [search](https://github.com/search?q=is%3Aopen+is%3Aissue+user%3Aatom+label%3Adeprecation-help) | Issues for helping package authors remove usage of deprecated APIs in packages. |
| `electron` | [search](https://github.com/search?q=is%3Aissue+repo%3Aatom%2Fatom+is%3Aopen+label%3Aelectron) | [search](https://github.com/search?q=is%3Aopen+is%3Aissue+user%3Aatom+label%3Aelectron) | Issues that require changes to [Electron](https://electron.atom.io) to fix or implement. |

#### Pull Request Labels

| Label name | `atom/atom` :mag\_right: | `atom`‑org :mag\_right: | Description |
| :--- | :--- | :--- | :--- |
| `work-in-progress` | [search](https://github.com/search?q=is%3Aopen+is%3Apr+repo%3Aatom%2Fatom+label%3Awork-in-progress) | [search](https://github.com/search?q=is%3Aopen+is%3Apr+user%3Aatom+label%3Awork-in-progress) | Pull requests which are still being worked on, more changes will follow. |
| `needs-review` | [search](https://github.com/search?q=is%3Aopen+is%3Apr+repo%3Aatom%2Fatom+label%3Aneeds-review) | [search](https://github.com/search?q=is%3Aopen+is%3Apr+user%3Aatom+label%3Aneeds-review) | Pull requests which need code review, and approval from maintainers or Atom core team. |
| `under-review` | [search](https://github.com/search?q=is%3Aopen+is%3Apr+repo%3Aatom%2Fatom+label%3Aunder-review) | [search](https://github.com/search?q=is%3Aopen+is%3Apr+user%3Aatom+label%3Aunder-review) | Pull requests being reviewed by maintainers or Atom core team. |
| `requires-changes` | [search](https://github.com/search?q=is%3Aopen+is%3Apr+repo%3Aatom%2Fatom+label%3Arequires-changes) | [search](https://github.com/search?q=is%3Aopen+is%3Apr+user%3Aatom+label%3Arequires-changes) | Pull requests which need to be updated based on review comments and then reviewed again. |
| `needs-testing` | [search](https://github.com/search?q=is%3Aopen+is%3Apr+repo%3Aatom%2Fatom+label%3Aneeds-testing) | [search](https://github.com/search?q=is%3Aopen+is%3Apr+user%3Aatom+label%3Aneeds-testing) | Pull requests which need manual testing. |

