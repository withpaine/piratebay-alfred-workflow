Piratebay workflow, [download link](https://github.com/Sheraff/piratebay-alfred-workflow/blob/master/Piratebay.alfredworkflow?raw=true "Piratebay.alfredworkflow")
=========================

<form action="https://www.paypal.com/cgi-bin/webscr" method="post" target="_top">
<input type="hidden" name="cmd" value="_s-xclick">
<input type="hidden" name="hosted_button_id" value="NPLNPFS8GZ5LG">
<input type="image" src="https://www.paypalobjects.com/en_US/i/btn/btn_donate_SM.gif" border="0" name="submit" alt="PayPal - The safer, easier way to pay online!">
<img alt="" border="0" src="https://www.paypalobjects.com/fr_FR/i/scr/pixel.gif" width="1" height="1">
</form>

**New Workflow:** I made a new workflow that needs beta testers. Come and get a peek at the [*TV Shows Manager*](https://github.com/Sheraff/tvshows-alfred-workflow "TV Shows Manager on github") for alfred, a neat replacement for Netflix! It remembers the shows you watch, tells you what episode is next (or where you stopped in the previous one), when it's out, and let's you stream it right from alfred. [Get this workflow here...](https://github.com/Sheraff/tvshows-alfred-workflow "TV Shows Manager on github").

## summary
This is a workflow for [Alfred App](http://www.alfredapp.com/ "Alfred App official website") (Mac OSX) to crawl [piratebay](http://thepiratebay.se/ "the pirate bay") and return magnet links with proper information.

If you want to see what it looks like, here's a [link to a live demo](http://sheraff.github.io/alfred-workflow-mockup/ "live demo"), or simply stare at this lame gif below:
![gif demo](https://github.com/Sheraff/alfred-workflow-mockup/raw/gh-pages/screen_shot.gif)

## quick use
[<img src="https://raw.githubusercontent.com/Sheraff/piratebay-alfred-workflow/master/github%20ressources/cloud-download.png"> download the workflow](https://github.com/Sheraff/piratebay-alfred-workflow/blob/master/Piratebay.alfredworkflow?raw=true "Piratebay.alfredworkflow") and open with Alfred.

Alternatively, use `Packal` through [the website](http://www.packal.org/workflow/piratebay "link to the workflow’s Packal page") or through the Alfred workflow.

## my country blocks piratebay
*Step 1.* If you are accessing the internet through an internet provider that is currently blocking piratebay, you can still find a [list of proxies](http://proxybay.info/ "proxies list") that do have acces to the site *(my friend in China reported that this list might also be blocked)*. Choose one that works for you (possibly close to your country) or just use one that you know from a friend if you can't access proxybay.info ;-)

*Step 2.* Go to Alfred Preferences and within the Piratebay workflow, edit the first `script filter`. Somewhere at the top is the line where you define piratebay's address: `$pirate_url = "http://thepiratebay.se";`. Replace the official address with the one of your proxy. Save. *Enjoy.*

## overview of the workflow
This is a quick draft of a remodeled version of a piratebay workflow for Alfred App. APIfy is closing and we need an alternative. Don't hesitate to collaborate, there is a lot that could be improved.

### This project uses
- [David Ferguson's `workflows` php class](http://dferg.us/workflows-class/ "Workflows Class")
- [Mafintosh's `peerflix` node](https://github.com/mafintosh/peerflix "peerflix on github") for NodeJS

### Improvement ideas are
- only add the 'streaming' alternative if the user has peerflix and nodejs installed
- create a quick tutorial (or refer to one) on how to install nodejs and peerflix
- deal with deleting from cache / writing to cache (in `main_script.php`) in a separate thread to prevent slow downs

## detailed view of the various files
- `launcher.sh` is a bash script that launches a php server and a end-of-session `kill_script.sh` to handle multithreading.If the query is empty, it simply returns a static XML of the search categories, otherwise it passes the query to `main_script.php` through the server (if started) or through regular `php` command.
- `main_script.php` is the main script behind everything. It parses the query, goes through the cache (2 hours) & archive (2 days) and eventually fetches and crawls piratebay if necessary (and writes the result in cache).
- `clean_cache.php` is a script run after every validation (when the user presses enter with none or any of the modifier keys). It cleans the cache based on expiration date (set to 2 days). It is only run after the execution to prevent slowing down the display of results.
- `history_log.php` is the step that takes care of logging the piratebay ID of the torrent that was just downloaded (magnet or stream) in a file called `history.db` and that is just a serialized php array.
- `secondary_script_example.php` unserializes the argument passed by Alfred and sends it to the next script.
- `workflows.php` is David Ferguson’s
