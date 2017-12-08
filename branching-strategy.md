### Branching Strategy
It's always better to create local branches to work on a specific issue. Makes life easier for you if you are the kind who enjoys multiple things parallels.
These should also be created directly off of the `master` branch.
```
$ git checkout -b commit-message-type/subject -t upstream/master
```

an example would be:
```
$ git checkout -b feat/search -t upstream/master
```

#### Commit Message Format
Each commit message consists of type, scope and subject:
```
<type>(<scope>): <subject>
<BLANK LINE>
<footer>
```

Any line of the commit message **cannot be longer 100 characters!** This allows the message to be easier to read on github as well as in various git tools.

A commit message consists of a header and a footer, separated by a blank line.



##### Message header
The message header is a single line that contains succinct description of the change containing a ***type***, an *optional scope* and a ***subject***.


###### Example Types
Must be one of the following:
- **feat**: A new feature
- **fix**: A bug fix
- **docs**: Documentation only changes
- **style**: Changes that do not affect the meaning of the code (white-space, formatting, missing semi-colons, etc)
- **refactor**: A code change that neither fixes a bug or adds a feature
- **test**: Adding missing tests
- **chore**: Changes to the build process or auxiliary tools and libraries such as documentation generation


###### Scope
The scope could be anything specifying place of the commit change.

###### Subject
The subject contains succinct description of the change:
- use the imperative, present tense: *"change"* not *"changed"* nor *"changes"*
- don't capitalize first letter
- no dot (.) at the end


##### Message footer
###### Breaking changes
All breaking changes have to be mentioned as a breaking change block in the footer, which should start with the word **BREAKING CHANGE**: with a space or two newlines. The rest of the commit message is then the description of the change, justification and migration notes.

`BREAKING CHANGE: Breaks modal height because of improper calculation (use functionB instead)`


###### Referencing issues
Closed bugs should be listed on a separate line in the footer prefixed with **"Closes"** keyword like this:

`Closes #234`

or in case of multiple issues:

`Closes #123, #245, #992`


##### Example commits
- `feat(accordion): add an accordion component`
- `fix(button): change primary button styles`
- `docs(readme): add documentation for explaining the commit message`
- `style(panel): add couple of missing semi colons`
- `refactor: change other things`
- `test(eslint): add eslint to test all javascript for proper coding`
- `chore(changelog): change generation of CHANGELOG.md and output template structure`
