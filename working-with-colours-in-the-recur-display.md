the display is just a text widget from the Tkinter framework. every refresh reads the current states of the program, writes it all into a single Text() object (with new line chars also to form each line ) and renders it.

http://effbot.org/tkinterbook/text.htm - this page talks about the text widget in general.

## text widget

in recur, first [we create the object](https://github.com/langolierz/r_e_c_u_r/blob/master/display_centre/display.py#L30), settings font, size etc:
```
self.display_text = Text(tk, bg="black", fg="white", font=('Liberation Mono', 13), undo=False)
```

then we use the `insert` function to add text to it. for example when [adding the main title](https://github.com/langolierz/r_e_c_u_r/blob/master/display_centre/display.py#L62) : 

```
self.display_text.insert(END, self.TITLES[0] + ' \n')
```

we always insert starting from the _END_ which is like where the cursor is currently pointing.

next we want to add some colouring to this line. remember the main title looks like this : 
```
'{0} r_e_c_u_r {0}'.format('='*18)
```
or `================== r_e_c_u_r ==================` when rendered. i want just the r_e_c_u_r part to be red.


## tags

colour is added to the text widget using tags. first we have to [define the tags](https://github.com/langolierz/r_e_c_u_r/blob/master/display_centre/display.py#L34): 

```
self.display_text.tag_configure("TITLE", background="black", foreground="red")
```

this sets the tag called _TITLE_ to have background black and foreground red. next, we need to [add the tag](https://github.com/langolierz/r_e_c_u_r/blob/master/display_centre/display.py#L63) to the widget in the correct spot :

```
self.display_text.insert(END, self.TITLES[0] + ' \n')
self.display_text.tag_add("TITLE", 1.19, 1.28)
```

i try to do this in the same spot that the text is added. the first param in `tag_add` is the name of the tag we defined above, the next two are the start and end position - in this case the range we want coloured red.

i dont understand why it is like this (and it causes some problems i dont know how to solve..) but it seems like we need to define the start and end position as decimals, where the number before the `.` is the y coord and the number after is the x coord.

in this case the range we want coloured red is from the 19th char on the first line to 28th char on the first line. (problems occur if we wanted to say the 30th char - since 1.3 and 1.30 are the same decimal ?)

## other tag functions used

- in a few places i used `configure_tag` , like a bonus feature to update the colour of the NOW player display as the opacity of the NOW player changed..
- sometimes i would use the `tag_remove` to set the hierarchy if a certain spot may dynamically have multiple tags assigned to it 