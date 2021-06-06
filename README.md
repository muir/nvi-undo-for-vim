# Nvi undo behavior for vim

This is a little bit of config you can put in your `.vimrc` that will
cause vim's undo behavior to be nvi-like.

Just cut-n-paste the following.

```vimscript
function Undo()
	if !exists('b:expectedtick') || b:changedtick != b:expectedtick 
		" this is a first typing of "u"
		normal! u
		let b:expectedtick = b:changedtick
		let b:undobackwards = 1
	else
		" this is a subsequent typing of "u"
		if b:undobackwards 
			normal! 
		else
			normal! u
		endif
		let b:undobackwards = !b:undobackwards
		let b:expectedtick = b:changedtick
	endif
endfunction

function Repeat()
	if !exists('b:expectedtick') || b:changedtick != b:expectedtick 
		" a normal typing of "."
		normal! .
	else
		" this is a typing of "." after undo/redo
		if b:undobackwards 
			normal! u
		else
			normal! 
		endif
		let b:expectedtick = b:changedtick
	endif
endfunction

nnoremap u :call Undo()
nnoremap . :call Repeat()
```
