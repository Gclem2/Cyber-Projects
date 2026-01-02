![[Pasted image 20251105141728.png]]

```bash
`PS1='$( [[ $PWD == "/etc" ]] && echo "<\u@\h in $PWD >: " || echo "\u@\h in \w >: ")'
source ~/.bashrc
```

![[Pasted image 20251105151815.png]]

```bash
ls /etc /dvd /var 2>/tmp/ioerrorj
```
![[Pasted image 20251105152217.png]]