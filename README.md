# story-blocks
Interactive, dynamic, navigatable, freely sharable, single-file stories written with Markdown and ES6

Check out the sample stories at https://larsthorup.github.io/story-blocks/

Write your story as a sequence of named story blocks inside an .html file. The file can be named `index.html` or anything else you like. At any time you can open or reload the .html file in any modern browser to review your work. To share you work, simply send the .html file to someone, or upload it to a server somewhere.

Each story block must be wrapped between these two lines of HTML:
 
```
<script id="give the block a name" type="text/markdown">
```
and
```
</script>
```

There must be two blocks with specific names:

* `init` where you can assign initial values to variables you want to use
* `start` which is the first block that will be shown

Each block is written using friendly Markdown syntax, see this guide for details: https://github.com/showdownjs/showdown/wiki/Showdown's-Markdown-syntax

If you want to include a link to another story-block, you can use this syntax:
```
[text-of-link](#name-of-block)
```

You can show the value of a variable using JavaScript syntax, like this: `${`any javascript expression`}`, for example: `${strength}`.

You can change the value of a variable using JavaScript syntax, like this: `${`assignment expression`,''}`, for example: `${strength += 4, ''}`.

At the end of the file you should append this block of code which contains the interpreter for your script blocks:

```
<script src="https://cdnjs.cloudflare.com/ajax/libs/showdown/1.5.0/showdown.min.js"></script>
<script>
  var converter = new showdown.Converter();
  eval('`' + document.getElementById('init').textContent + '`'); // Note: eval is evil!
  function updatePage () {
    var page = window.location.hash.substr(1) || 'start';
    var template = document.getElementById(page).textContent;
    var text = eval('`' + template + '`'); // Note: eval is evil!
    var html = converter.makeHtml(text);
    document.body.innerHTML = html;
  }
  window.addEventListener('hashchange', updatePage, false);
  window.addEventListener('DOMContentLoaded', updatePage, false);
</script>
```

You can add any styling you'd like using CSS in a `<style>`-block.

If you need more features, you can modify this interpreter directly.

Enjoy!
/ Lars