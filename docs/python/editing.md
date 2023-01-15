# Developer Tips

TODO

## Google is your friend

Before we get started, it's important to say that, when trying to learn new
programming tools and languages, **Google is your friend.** Experienced
programers often use Google to understand functions that they're using or error
messages that they receive, and you can too![^1]

[StackOverflow](https://en.wikipedia.org/wiki/Stack_Overflow) is a Q&A service
that you'll commonly see if you google programming questions. There's a good
chance that what you want to know has already been asked by someone, so
definitely click these links if they come up in your search!

Classes at MIT will generally let you look up programming questions you have or
error messages you run into as you debug your code, **as long as you don't look
up the problem or algorithm you're solving directly or copy someone else's
code.** Some classes will even let you copy-and-paste code snippets from the
internet if you cite them. Don't take this as official advice, though, and make
sure to read the collaboration policy for the class you're taking carefully!

[^1]:
    At Google, there's a _completely internal_ version of Google Search and
    StackOverflow, so that programmers working on non-public code can still ask
    and answer each other's questions! It's pretty cool!

## Play with your IDE

As a software developer (and yes, taking a 6.101 _does_ make you a developer!),
you'll probably use an
[Integrated Development Environment](https://en.wikipedia.org/wiki/Integrated_development_environment),
called an IDE, to write code. An IDE is like your software toolbox, and
understanding how to use your tools and how they can help you will make you a
more productive developer. So whether that's the
[Python IDLE](https://docs.python.org/3/library/idle.html),
[Spyder](https://www.spyder-ide.org/), [VSCode], or [PyCharm], you should play
around with your IDE a little to better understand how it works.

If you're unsure of what IDE to use, I'd personally recommend [VSCode] or
[PyCharm]. Both are popular, open-source IDEs with a large community of Python
developers around them. Both have helpful features like
[linting](<https://en.wikipedia.org/wiki/Lint_(software)>), automatic code
formatting, and debugging.

I would also recommend _against_ using Python's built-in IDLE editor. While IDLE
is helpful when getting started with Python, the kind of code you'll write in
6.101 will be complex enough that you will likely get a significant benefit from
using an IDE that can spot errors in your code before they happen and debug your
code for you.

[vscode]: https://code.visualstudio.com/
[pycharm]: https://www.jetbrains.com/pycharm/

### Find the Debugger

The **debugger** is a feature in many IDEs that allows you to step through your
code line-by-line and inspect all of the variables in the frame at each point.
Learning how to use your IDE's debugger can help you inspect the behavior of
your programs in a more precise way than print statements. While you don't have
to use it for 6.101 (and many students get by fine with print statements!), you
might want to learn about it to help you find issues in your code quicker. When
you're debugging, there's a few main things you'll want to do:

- **place breakpoints**: A breakpoint is a place in your code you tell the
  debugger to stop, so you can inspect what the code is doing _before_ it runs
  that line. You can usually create one by clicking next to a line number, and
  you'll see a red dot next to the line number where the debugger will stop.
- **look at the variables**: The variables when the code is frozen will be shown
  in the debugger panel. Check them out to see if they're what you expect them
  to be at that point.
- **stepping your code**: "Stepping" your code lets you run your code
  line-by-line and see how they change over time. You can usually also "resume"
  your code, and the debugger will resume running your code until it hits
  another breakpoint. This can be handy if you are trying to see if a bug occurs
  between iterations of a for loop, or calls to a function.

To learn about the debugger in the following IDEs, follow these guides:

- [VSCode](https://code.visualstudio.com/docs/introvideos/debugging)
- [PyCharm](https://www.jetbrains.com/help/pycharm/part-1-debugging-python-code.html)

## Play with the Python Console

Sometimes, you might want to see how a certain feature behaves in Python. Maybe
you want to create a simple function to test its behavior. I _highly encourage_
you to crack open the Python console to try these out. The Python console, also
known as a [REPL], lets you run one line of Python at a time. You can open it by
running `python3` in your terminal, and some IDEs have a Python console built
in, like [the one in PyCharm][pycconsole] or [in Spyder][spyconsole].

[repl]: https://en.wikipedia.org/wiki/Read%E2%80%93eval%E2%80%93print_loop
[pycconsole]: https://www.jetbrains.com/help/pycharm/interactive-console.html
[spyconsole]: https://docs.spyder-ide.org/current/panes/ipythonconsole.html

### IPython is handy!

[IPython], which stands for "Interactive Python", is a version of the Python
REPL that is a little more friendly to use. It contains [syntax highlighting]
and [code completion] that you might be used to in an IDE. To install it, just
run:

```shell
$ pip3 install ipython
```

Then you can run `ipython` in your terminal, and you should see something like
this:

```python
Python 3.11.1 (v3.11.1:a7a450f84a, Dec  6 2022, 15:24:06) [Clang 13.0.0 (clang-1300.0.29.30)]
Type 'copyright', 'credits' or 'license' for more information
IPython 8.5.0 -- An enhanced Interactive Python. Type '?' for help.

>>>
```

To learn more about IPython, you can follow
[this tutorial](https://ipython.readthedocs.io/en/stable/interactive/tutorial.html),
though you can mostly just use it as a better version of the Python REPL.

[syntax highlighting]: https://en.wikipedia.org/wiki/Syntax_highlighting
[code completion]:
  https://en.wikipedia.org/wiki/Autocomplete#In_source_code_editors
[ipython]: https://ipython.org/