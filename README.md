Pendent to find a a solarized color that works well with base16Shell
Try using plugin to convert from gui colors as it might work even better
Setup so its eady to switch
Try not putting it in bashrc and activate from command line
Try one standalone plugin and see how integrates -e.g jellybeans, gruv (compare gruv plugin with gruv16)

# Goal
To understand how colors are handled within the terminal and vim, so we create a system to change easily the terminal theme and the vim theme


The following are main factors when defining the color:
- Console profile colors
  - Border Color
  - Background color
  - Forground Color
- Dircolors
- vim colorscheme
- Teminal number colors

### Dircolors
The first part before the semicolon represent the text style.
From the colors explantion in this [page](https://askubuntu.com/questions/17299/what-do-the-different-colors-mean-in-ls):
00=none, 01=bold, 04=underscore, 05=blink, 07=reverse, 08=concealed.
The second and third part are the colour and the background color:
30=black, 31=red, 32=green, 33=yellow, 34=blue, 35=magenta, 36=cyan, 37=white.

A dircolors file defines for each type of file how its display in terms of text style, foreground and background color. 

#### dircolors.ansi-light
EXEC 01;31 # executable file displayes in bold and in red
.txt 32 # text file displayis normal and green
.7z   1;35 # bold and blue

### Palette
A 16 colors palette define the basic colors and the equivelent brigh colors 
- Termianl which don't support bold can be configured to show bold types using bright colors instead. The basic colors are in this:

 1. black, 2. red, 3. green, 4. yellow 5. blue, 6. magenta, 7. cyan, 8. white

For for example if we define 0 as white it will print blacks as whites. If we then defined 09 as red, it will show bold blacks as red:
```
printf "\33[0;30mHello World\33[m" # prints  Hello in white 
printf "\33[1;30mHello World\33[m" # prints  Hello in red
```


### Study
We will look at different projects which help setting terminal colors and vim colors. 
[shell_base16](https://github.com/chriskempson/base16-shell.git)(~/.config/base16-shell)
[base16-gnome-terminal](https://github.com/aaron-williamson/base16-gnome-terminal.git)  (~/.config/base16-gnome-terminal/)
[base16-gnome-terminal](https://github.com/seebi/dircolors-solarized): it provides the dircolors for the previous palette
[gnome solarized](https://github.com/Anthony25/gnome-terminal-colors-solarized.git): it defines the gnome palette for the ethanschoonver solarized project.
[gnome solarized dircolors](
[solarized and others](https://github.com/metalelf0/gnome-terminal-colors.git): fork of solarized repository which include a few more themes (dracula, gruvbox, gotham and hemisu)

They define the color paletter and the foreground and background. But they do not define the type of font. Use the terminal for that.

## bash16-shell
It uses printf with sequence of characters to set the palette.

printf '\033Ptmux;\033\033]4;%d;rgb:%s\033\033\\\033\\' 0 $color00

color00=1C/20/23

So it sets the color of the palette in hexadecimal and uses the printf script to convert it and set that color of the palette.

For setting foreground:
`printf '\033Ptmux;\033\033]%d;rgb:%s\033\033\\\033\\' 10 1C/20/23`
Background same but uses code 11:
`printf '\033Ptmux;\033\033]%d;rgb:%s\033\033\\\033\\' 11 1C/20/23`

When setting colors in this way is not compatible with using dconf tool. So need to close the terminal and come back.

### base16 + base16-vim
When runnig bas16-shell color, it sets the theme name under ~/.vimrc_background. Then this file is read by vim on entry and it uses that name to matches the them within colors.vim



###solarized
It uses gconftool

set foreground, background 
```
gconftool-2 -s -t string $profile_path/bold_color $(cat $bd_color_file)
gconftool-2 -s -t string $profile_path/background_color \
```


### Understangin base16 project
#### Setting profile colors
Run a scripts which set colors like this:
	`printf '\033]4;%d;rgb:%s\033\\'` where  `d` is the type e.g. 10 is forground and `s` is the color in rgb, e.g color_foreground="93/a1/a1" # Base 05

You have to set you Terminal emoulator's color setting to the Solarized palette
Testing the four different dircolors from https://github.com/seebi/dircolors-solarized looks very similar when profile is set with palette solarized light

dircolors
   Do not seem to be set,

### Understanding gnome-solidarized colors
Both dircolors and scheme needs to be setup. Currently seems that dircolors are not copy accross and they need to be copy manually when switching theme, e.g from dark to light 
### Understaning how impacts vim
So changin vim scheme colors is supported


### Test 16 vs 256

