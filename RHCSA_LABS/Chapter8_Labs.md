# Lab 8-1 : Nice and Renice a Process
![[Pasted image 20251118015108.png]]
So to start run this command on the first server
```bash
top
```
After switch to the second terminal and run this

```bash
pgrep top
```
It returns this result
![[Pasted image 20251118015209.png]]

Then stop top in the first terminal
```bash
nice -n 8 top
```

![[Pasted image 20251118015447.png]]

Then to renice the command run 
```bash
sudo renice -10 2667
```

![[Pasted image 20251118015641.png]]

# Lab 8-2 Configure a User Crontab File

![[Pasted image 20251118020241.png]]

to start run tty and date
```bash
tty
date
```

![[Pasted image 20251118020327.png]]

then we make the crontab
```bash
crontab -e

```

Enter this in
![[Pasted image 20251118020533.png]]

![[Pasted image 20251118021021.png]]
you can then exit then confirm
![[Pasted image 20251118021113.png]]