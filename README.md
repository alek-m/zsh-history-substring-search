zsh-history-substring-search
==============================================================================

This is a clean-room implementation of the [Fish shell][1]'s history search
feature, where you can type in any part of any previously entered command
and press the UP and DOWN arrow keys to cycle through the matching commands.
You can also use K and J in VI mode or ^P and ^N in EMACS mode for the same.

[1]: http://fishshell.com
[2]: http://www.zsh.org/mla/users/2009/msg00818.html
[3]: http://sourceforge.net/projects/fizsh/
[4]: https://github.com/robbyrussell/oh-my-zsh/pull/215
[5]: https://github.com/zsh-users/zsh-history-substring-search
[6]: https://github.com/zsh-users/zsh-syntax-highlighting

------------------------------------------------------------------------------
Requirements
------------------------------------------------------------------------------

* [ZSH](http://zsh.sourceforge.net) 4.3 or newer

------------------------------------------------------------------------------
Usage
------------------------------------------------------------------------------

1.  Load this script into your interactive ZSH session:

        % source zsh-history-substring-search.zsh

    If you want to use [zsh-syntax-highlighting][6] along with this script,
    then make sure that you load it *before* you load this script:

        % source zsh-syntax-highlighting.zsh
        % source zsh-history-substring-search.zsh

2.  Bind keyboard shortcuts to this script's functions:

        ## Arrow Keys ###########################################

        # OPTION 1: for most systems
        zmodload zsh/terminfo
        bindkey "$terminfo[kcuu1]" history-substring-search-up
        bindkey "$terminfo[kcud1]" history-substring-search-down

        # OPTION 2: for iTerm2 running on Apple MacBook laptops
        zmodload zsh/terminfo
        bindkey "$terminfo[cuu1]" history-substring-search-up
        bindkey "$terminfo[cud1]" history-substring-search-down

        # OPTION 3: for Ubuntu 12.04, Fedora 21, and MacOSX 10.9
        bindkey '^[[A' history-substring-search-up
        bindkey '^[[B' history-substring-search-down

        ## EMACS mode ###########################################

        bindkey -M emacs '^P' history-substring-search-up
        bindkey -M emacs '^N' history-substring-search-down

        ## VI mode ##############################################

        bindkey -M vicmd 'k' history-substring-search-up
        bindkey -M vicmd 'j' history-substring-search-down

3.  Type any part of any previous command and then:

    * Press the UP arrow key to select the nearest command that (1) contains
      your query and (2) is older than the current command in the command
      history.

    * Press the DOWN arrow key to select the nearest command that (1)
      contains your query and (2) is newer than the current command in the
      command history.

    * Press ^U (the Control and U keys simultaneously) to abort the search.

4.  If a matching command spans more than one line of text, press the LEFT
    arrow key to move the cursor away from the end of the command, and then:

    * Press the UP arrow key to move the cursor to the line above.  When the
      cursor reaches the first line of the command, pressing the UP arrow
      key again will cause this script to perform another search.

    * Press the DOWN arrow key to move the cursor to the line below.  When
      the cursor reaches the last line of the command, pressing the DOWN
      arrow key again will cause this script to perform another search.

------------------------------------------------------------------------------
Configuration
------------------------------------------------------------------------------

This script defines the following global variables. You may override their
default values only after having loaded this script into your ZSH session.

* HISTORY_SUBSTRING_SEARCH_HIGHLIGHT_FOUND is a global variable that defines
  how the query should be highlighted inside a matching command. Its default
  value causes this script to highlight using bold, white text on a magenta
  background. See the "Character Highlighting" section in the zshzle(1) man
  page to learn about the kinds of values you may assign to this variable.

* HISTORY_SUBSTRING_SEARCH_HIGHLIGHT_NOT_FOUND is a global variable that
  defines how the query should be highlighted when no commands in the
  history match it. Its default value causes this script to highlight using
  bold, white text on a red background. See the "Character Highlighting"
  section in the zshzle(1) man page to learn about the kinds of values you
  may assign to this variable.

* HISTORY_SUBSTRING_SEARCH_GLOBBING_FLAGS is a global variable that defines
  how the command history will be searched for your query. Its default value
  causes this script to perform a case-insensitive search. See the "Globbing
  Flags" section in the zshexpn(1) man page to learn about the kinds of
  values you may assign to this variable.

To always receive _unique_ search results, use `setopt HIST_IGNORE_ALL_DUPS`.
Alternatively, use `setopt HIST_FIND_NO_DUPS` which makes this plugin skip
duplicate _adjacent_ search results as you cycle through them---however, this
does not guarantee that search results are unique: if your search results were
"Dog", "Dog", "HotDog", "Dog", then cycling them gives "Dog", "HotDog", "Dog".
Notice that the "Dog" search result appeared twice as you cycled through them!
If you wish to avoid this limitation, then use `setopt HIST_IGNORE_ALL_DUPS`.

------------------------------------------------------------------------------
History
------------------------------------------------------------------------------

This script was originally written by [Peter Stephenson][2], who published it
to the ZSH users mailing list (thereby making it public domain) in September
2009. It was later revised by Guido van Steen and released under the BSD
license (see below) as part of [the fizsh project][3] in January 2011.

It was later extracted from fizsh release 1.0.1, refactored heavily, and
repackaged as both an [oh-my-zsh plugin][4] and as an independently loadable
[ZSH script][5] by Suraj N. Kurapati in 2011.

It was [further developed][4] by Guido van Steen, Suraj N. Kurapati, Sorin
Ionescu, and Vincent Guerci in 2011.
