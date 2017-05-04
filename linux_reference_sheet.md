<center>
Linux Reference Sheet
</center>

# Customizing Terminal Colors

I haven't found a way to automate the background and foreground colors  

* Gruvbox background color (hard contrast): #1d2021 rgb(29,32,33)
* Gruvbox foreground color (dark) : #ebdbb2 rgb(235,219,178)

Remaining colors are set with the .dircolors file in dotfiles repository. Shell
file can call it.

* The following website has the ansii codes for 256 colors. Dirfiles and shell
prompt can be set according to taste with these colors.  
[256 Colors](misc.flogisoft.com/bash/tip_colors_and_formatting "256 Colors")

# Permissions
* Sticky bit: Prevents other users from deleting unowned files while still
allowing them to edit said files.
    * `chmod +t <filename>`


