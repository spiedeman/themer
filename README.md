# themer

`themer` takes a set of colors and generates editor themes, terminal themes, and desktop/device wallpapers.

`themer` is inspired by [trevordmiller/nova](http://www.trevordmiller.com/nova/) and [chriskempson/base16](http://chriskempson.com/projects/base16/).

## Installation

    mkdir my-dotfiles && cd my-dotfiles
    yarn add themer # or npm install --save themer

## Usage

    themer --colors < file / npm module > \
      --template < file / npm module > \
      [--template < file / npm module >...] \
      --out < directory >

`themer` can create themes from your custom color sets (see ["Create your own color set"](#create-your-own-color-set) below) or from color sets published on npm (see mjswensen/themer-colors-default). The same is true for templates.

### Example usage

Say you wanted to generate a vim theme and desktop background using `themer`'s default color set. First, install the color set and templates:

    cd my-dotfiles
    yarn add themer themer-colors-default themer-vim themer-wallpaper-block-wave

Then edit your `package.json`:

    ...
    "scripts: {
      "build": "themer -c themer-colors-default -t themer-vim -t themer-wallpaper-block-wave -o gen"
    },
    ...

Then run your new script:

    yarn build

Now check the `gen/` folder for your generated themes.

## Themer color sets

* mjswensen/themer-colors-default

### Create your own color set

    exports.colors = {
      dark: { // A color set can have both light and dark variants, but doesn't have to.

        accent0: '#FF4050', // accent0-7 should be the main accent colors of your theme.
        ...
        accent7: '#F553BF',

        shade0: '#282629', // shade0-7 should be shades of the same hue, with shade0 being the darkest and shade7 being the lightest.
        ...
        shade7: '#FFFCFF'

      },
      light: { ... },
    };

See mjswensen/themer-colors-default for an example.

I would recommend checking your color set into your dotfiles repo. Once you've fine-tuned it, you might consider publishing it to npm for others to use! (If you do, consider naming your repo starting with `themer-colors-` so that others can easily find it.)

## Themer templates

### Terminals

* mjswensen/hyper

### Editors/IDEs

* mjswensen/vim
* mjswensen/vim-lightline

### Wallpapers

* mjswensen/wallpaper-block-wave
* mjswensen/wallpaper-octagon

### Create your own template

    exports.render = function(colors, options) {

      // colors is an object that will have one or both keys: 'light' and
      // 'dark', each being an object with keys 'accent0' through 'accent7'
      // and 'shade0' through 'shade7'.

      // options is an object representing the original command-line args
      // passed to themer. This allows you to add special arguments that
      // will apply only to your template. An example of this is allowing
      // a themer user to specify custom resolutions for rending a wallpaper.

      // This function should return an array of Promises, each Promise
      // resolving to an object of the following structure:
      // {
      //   name: '<the name of the file to be written>',
      //   contents: <a Buffer of the contents of the file to be written>,
      // }

    };

As long as a template exports a `render` function as indicated, it can be passed to the `-t` (`--template`) argument of `themer`. Once you've developed your template, consider publishing it on npm (with repository name staring with `themer-`) so that others can use it!
