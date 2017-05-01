<center>
# Markdown Cheet Sheet
</center>

## Contents
**[Headers](#headers)**  
**[Emphasis](#emphasis)**  
**[Tables](#tables)**  
**[Lists](#lists)**  
**[Links](#links)**  
**[Code](#code)**  
**[Block Quotes](#block-quotes)**  
**[HTML Tags](#html-tags)**   
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
[Link](https://www.google.com "optional title for link")
**[Markdown Header](#Markdown Header)**
![Image](path.png "optional title")
```
[Link](https://www.google.com "optional title for link")  
**[Markdown Header](#Markdown Header)**

## Code
```
```c
#include <stdio.h>
int main(){
  printf("Hello World\n");
  return 0;
}
  ```
```
```c
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
>like this

## HTML Tags
```
<center>Text</center>
<br> newline (like \n)
```
<center>Text</center>
