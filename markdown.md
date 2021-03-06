# Markdown Cheet Sheet

## Contents
**[Headers](#headers)**  
**[Emphasis](#emphasis)**  
**[Tables](#tables)**  
**[Lists](#lists)**  
**[Links](#links)**  
**[Code](#code)**  
**[Block Quotes](#block-quotes)**  
**[HTML Tags](#html-tags)**  
**[Misc](#misc)**  
## Headers

```
# H1
## H2
### H3
```

## Emphasis
| Markdown | Interpreted |
| --- | --- |
|`*italic* or _italic_` | *italic* |
|`**bold** or __bold__` | **bold** |

## Tables
```
| Left_Aligned | Centered | Right_Aligned |
| :--- | :---: | ---: |
| *blah* | **blah** | blah |
```

| Left_Aligned | Centered | Right_Aligned |
| :--- | :---: | ---: |
| *blah* | **blah** | blah|

## Lists
```
1. First ordered list item
2. Another item
⋅⋅⋅⋅* Unordered sub-list.
1. Actual numbers don't matter, just that it's a number
⋅⋅⋅⋅1. Ordered sub-list
4. And another item.

⋅⋅⋅You can have properly indented paragraphs within list items. Notice the blank line above, and the leading spaces (at least one, but we'll use three here to also align the raw Markdown).

⋅⋅⋅To have a line break without a paragraph, you will need to use two trailing spaces.⋅⋅
⋅⋅⋅Note that this line is separate, but within the same paragraph.⋅⋅
⋅⋅⋅(This is contrary to the typical GFM line break behaviour, where trailing spaces are not required.)

* Unordered list can use asterisks
⋅⋅⋅⋅- Or minuses
⋅⋅⋅⋅⋅⋅⋅⋅+ Or pluses
```

1. First ordered list item
2. Another item
    * Unordered sub-list.
1. Actual numbers don't matter, just that it's a number
    1. Ordered sub-list
4. And another item.

   You can have properly indented paragraphs within list items. Notice the blank line above, and the leading spaces (at least one, but we'll use three here to also align the raw Markdown).

   To have a line break without a paragraph, you will need to use two trailing spaces.  
   Note that this line is separate, but within the same paragraph.  
   (This is contrary to the typical GFM line break behaviour, where trailing spaces are not required.)

* Unordered list can use asterisks
    - Or minuses
        + Or pluses

## Links
```
Website:
[Link](https://www.google.com "optional title for link")

Table of Contents:
**[Markdown Header](#markdown-header)**  
                     ^       ^^ lower-case and hyphen are necessary!

Images:
![Image](path.png "optional title")
```

## Code
````
```C
 #include <stdio.h>
 int main(){
   printf("Hello World\n");
   return 0;
 }
```
````

```C
#include <stdio.h>
int main(){
  printf("Hello World\n");
  return 0;
}
```

## Block Quotes
```
> Block **quotes** are made
> like this
```
> Block **quotes** are made
> like this

## HTML Tags
```
<center>Text</center>
<br> newline (like \n)
```

## Misc
* \ can be used before any character to stop it from being interpreted. `\*\*not bold or italic**`
