---
title: "Goals"
slug: "goals"
excerpt: "The commands Pants runs."
hidden: false
createdAt: "2020-02-21T17:44:52.605Z"
updatedAt: "2022-04-11T21:31:11.557Z"
---
Pants commands are known as _goals_, such as `test` and `lint`.

To see the current list of goals, run:

```bash
❯ pants help goals
```

You'll see more goals activated as you activate more [backends](doc:enabling-backends).

Running goals
=============

For example:

```
❯ pants count-loc project/app_test.py
───────────────────────────────────────────────────────────────────────────────
Language                 Files     Lines   Blanks  Comments     Code Complexity
───────────────────────────────────────────────────────────────────────────────
Python                       1       374       16        19      339          6
───────────────────────────────────────────────────────────────────────────────
Total                        1       374       16        19      339          6
───────────────────────────────────────────────────────────────────────────────
```

You can also run multiple goals in a single run of Pants, in which case they will run sequentially:

```bash
# Format all code, and then test it:
❯ pants fmt test ::
```

Finally, Pants supports running goals in a `--loop`. In this mode, all goals specified will run  
sequentially, and then Pants will wait until a relevant file has changed to try running them again.

```bash
# Re-run linters and testing continuously as files or their dependencies change:
❯ pants --loop lint test project/app_test.py
```

Use `Ctrl+C` to exit the `--loop`.

Goal arguments
==============

Most goals require arguments to know what to work on. 

You can use several argument types:

| Argument type                   | Semantics                                   | Example                         |
| ------------------------------- | ------------------------------------------- | ------------------------------- |
| File path                       | Match the file                              | `pants test project/tests.py` |
| Directory path                  | Match everything in the directory           | `pants test project/utils`    |
| `::` globs                      | Match everything in the directory and below | `pants test project::`        |
| [Target addresses](doc:targets) | Match the target                            | `pants package project:tests` |

You can combine argument types, e.g. `pants fmt src/go:: src/py/app.py`.

To ignore something, prefix the argument with `-`. For example,  
`pants test :: -project/integration_tests` will run all your tests except for those in the  
directory `project/integration_tests`.

> 🚧 Set `[GLOBAL].use_deprecated_directory_cli_args_semantics = false` in `pants.toml`
> 
> This will become the default in Pants 2.14.

> 📘 Tip: advanced target selection, such as running over changed files
> 
> See [Advanced target selection](doc:advanced-target-selection) for alternative techniques to specify which files/targets to run on.

Goal options
------------

Many goals also have [options](doc:options) to change how they behave. Every option in Pants can be set via an environment variable, config file, and the command line.

To see if a goal has any options, run `pants help $goal` or `pants help-advanced $goal`. See [Command Line Help](doc:getting-help) for more information.

For example:

```
❯ pants help test
17:20:14.24 [INFO] Remote cache/execution options updated: reinitializing scheduler...
17:20:15.36 [INFO] Scheduler initialized.

`test` goal options
-------------------

Run tests.

Config section: [test]

  --[no-]test-debug
  PANTS_TEST_DEBUG
  debug
      default: False
      current value: False
      Run tests sequentially in an interactive process. This is necessary, for example, when you
      add breakpoints to your code.

...
```

You can then use the option by prefixing it with the goal name:

```bash
pants --test-debug test project/app_test.py
```

You can also put the option after the file/target arguments:

```bash
pants test project/app_test.py --test-debug
```

As a shorthand, if you put the option after the goal and before the file/target arguments, you can leave off the goal name in the flag:

```bash
pants test --debug project/app_test.py
```
