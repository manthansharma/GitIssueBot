GitIssueBot
-----------

A bot for managing Github issues. Currently only supports approving newly created issues, more ideas in the works.

## Approve

``gitissuebot-approve`` allows to check any newly added issues on the GitHub issue tracker whether the author included
a customizable phrase to be found in the contribution guidelines to check whether the user has read those guidelines.
The phrase may be included either in the body of the ticket or -- if it's not there -- be included as part of a comment
by the issue's **original author** to the issue. 

Issues not containing the phrase (checks are case-insensitive) will receive a configurable comment by the bot directing
the user to (re-)read the guidelines and the issue will be marked by a configurable label. Issues updated by their
author to include the phrase thereafter will be de-labeled again by the bot. If the issue is still lacking the phrase 
after a configurable grace period, the bot will automatically close the issue.

It's possible to configure phrases to be contained in the title of a newly created issue to make the bot ignore the
issue (e.g. you can instruct your users to prefix feature requests with "[Request]" and add that as an ignored title,
so that feature requests won't be processed by the bot). It's also possible to configure labels to ignore by the bot 
too.

### Configuration

Configuration of the bot can be done either completely via the command line (see `gitissue-approve --help` for a list
of available arguments) or by supplying a configuration file via the `--config` command line argument from which to 
take the configuration.

An example for such a configuration file can be found below:

    # The Github access token to use for accessing the Github API
    # => https://help.github.com/articles/creating-an-access-token-for-command-line-use
    token: someVeryLongToken
    
    # The repository on which to work, in the format <username>/<repositoryname>
    repo: myuser/myrepository
    
    # Any issues added since that date will be processed
    since: 2014-07-27 12:00:00+00:00
    
    # Phrase to search for
    phrase: I love cookies
    
    # Label to add to issue if it's invalid
    label: incomplete issue
    
    # Grace period after which to close issues that are still invalid
    grace_period: 14
    
    # Labels if issues to ignore
    ignored_labels:
    - request
    - support
    - question
    - misc
    - accepted
    
    # Parts of titles of issues to ignore
    ignored_titles:
    - '[Request]'
    - '[Support]'
    - '[Question]'
    - '[Misc]'
    
    # Reminder to add to incomplete issues
    reminder: 'Hi @{author},
    
    
      It looks like there is some information missing from your ticket that will be needed
      in order to diagnose and fix the problem at hand. Please take a look at the [Contribution
      Guidelines](https://github.com/myuser/myrepository/blob/master/CONTRIBUTING.md), which 
      will tell you **exactly** what your ticket has to contain in order to be processable.
    
    
      I''m marking this one now as needing some more information. Please understand that
      if you do not provide that information within the next two weeks (until {until})
      I''ll close this ticket so it doesn''t clutter the bug tracker.
    
    
      Best regards,
    
      ~ Your friendly GitIssueBot
    
    
      PS: I''m just an automated script, not a human being.
    
      '
    
    # Comment to add when closing an issue
    closing: 'Since apparently some of the required information is still missing, I''m
      closing this now, sorry. Feel free to reopen this or create a new issue once you
      can provide **all** required information.
    
      '
    
    # Whether to only perform a dry run, without any writing requests against the API
    dryrun: false

The [generated token](https://help.github.com/articles/creating-an-access-token-for-command-line-use) needs to grant 
access to repo and -- if the issue of a private repository are to be managed -- also access to private repos.