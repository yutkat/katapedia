# Autohotkey

~~~

MakeFasterBy := 50 ; how much to reduce from the delay of the keys, in ms
MakeFasterEvery := 100 ; interval between each time the delay is reduced, in ms
;MakeSlowerBy := 300 ; how much slower to make key entry every time there is a mistake, in ms
;MaxDelay := 500 ; maximum delay between keys
KeyDelay := 0 ; initial key delay, in ms
GraceKeys := 4 ; how many keys have to be typed correctly in a row, before the delay is cancelled
CorrectlyTyped := 0

SetKeyDelay, 0


; left alt+hjklでカーソルキー
<!h::
Send,{Left}
return
<!l::
Send,{Right}
return
<!j::
Send,{Down}
return
<!k::
Send,{Up}
return

<!backspace::
Send,{delete}
return
<!`::
Send,{delete}
return

; shift対応
<!+h::
Send,+{Left}
return
<!+l::
Send,+{Right}
return
<!+j::
Send,+{Down}
return
<!+k::
Send,+{Up}
return


>!a::
Send,{Home}
return
>!s::
Send,{PgDn}
return
>!d::
Send,{PgUp}
return
>!f::
Send,{End}
return


; ctrl+backspaceで文字
^BS::
Send, ^+{Left}{Delete}

; RWin::Send ^{Space}
RWin::Send {vkF3sc029}

Capslock::Ctrl
sc03a::Ctrl

~~~

