# kaktree

![kaktree](https://user-images.githubusercontent.com/19470159/59667890-2d397780-91c0-11e9-9214-e32b04539e7a.png)

This plugin displays the interactive filetree. It requires Tmux and Perl, as
well as `ls` command that supports at least `-l`, `-F`, and `-a` flags.

## Installation
You need latest Kakoune build from master in order to use
this plugin.

### With [plug.kak](https://github.com/andreyorst/plug.kak)
Add this to your `kakrc`:

```sh
plug "andreyorst/kaktree" config %{
    hook global WinSetOption filetype=kaktree %{
        remove-highlighter buffer/numbers
        remove-highlighter buffer/matching
        remove-highlighter buffer/wrap
        remove-highlighter buffer/show-whitespaces
    }
    kaktree-enable
}
```

Restart Kakoune or re-source your `kakrc` and call `plug-install` command.

### Without plugin manager
Clone this repo to your autoload directory, or source `kaktree.kak` file from your `kakrc`.

It's strongly recommended to disable line numbers and wrap highlighters as shown
in the plug.kak example above.

## Configuration
There are set of options that affect how `kaktree` works:

- `kaktreeclient` - the name of the client that will be used to display
  `kaktree` buffer.
- `kaktree_split` - how to split TMUX (horizontally or vertically).
- `kaktree_side` - the side where `kaktree` will be displayed in TMUX.
- `kaktree_size` - size of the split.
- `kaktree_dir_icon_close` - icon for closed directory.
- `kaktree_dir_icon_open` - icon for opened directory.
- `kaktree_file_icon` - icon for files.
- `kaktree_show_hidden` - whether to show hidden files
- `kaktree_hlline` - configures highlighting of current line in the tree.
- `kaktree_sort` - whether to sort items in the tree.
- `kaktree_double_click_duration` - amount of time Kakoune waits to register
  double clicks in the kaktree.
- `kaktree_show_help` - whether to display help box on first launch of kaktree.

For example, to have nice folder and file icons as on the screenshot add this to
your configuration (assuming that your font has these characters and your
terminal can handle wide Unicode symbols):

```
plug "andreyorst/kaktree" defer kaktree %{
    set-option global kaktree_double_click_duration '0.5'
    set-option global kaktree_indentation 1
    set-option global kaktree_dir_icon_open  '▾ 🗁 '
    set-option global kaktree_dir_icon_close '▸ 🗀 '
    set-option global kaktree_file_icon      '⠀⠀🖺'
} config %{...}
```

If you're not using plug.kak, replace `defer` with `hook global
ModuleLoaded`. Beware that `'⠀⠀🖺'` the first two characters here are not spaces,
but invisible Unicode symbols. Currently Kaktree handles tree structure based on
indentation, so in order to do alignment of icons you should use something that
doesn't match `\s` regexp atom.
