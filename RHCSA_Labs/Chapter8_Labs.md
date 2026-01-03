# Lab 8-1 : Nice and Renice a Process
![](attachment/28db262db4426d4cfc6f0fa126b1eda8.png)
So to start run this command on the first server
```bash
top
```
After switch to the second terminal and run this

```bash
pgrep top
```
It returns this result
![](attachment/ebb6ea243048a7111ea61d76450a2757.png)

Then stop top in the first terminal
```bash
nice -n 8 top
```

![](attachment/8a79f12442b26908def3c8d69667f7e1.png)

Then to renice the command run 
```bash
sudo renice -10 2667
```

![](attachment/a55193e8d0854651ec2ab5d4c317e05e.png)

# Lab 8-2 Configure a User Crontab File

![](attachment/d69ed51c5eed365c74433399032e4b59.png)

to start run tty and date
```bash
tty
date
```

![](attachment/8ac207803f5c4e81dade200fab983fb2.png)

then we make the crontab
```bash
crontab -e

```

Enter this in
![](attachment/d25609dfb21faaeacc195c5acda8fca9.png)

![](attachment/11f667f4a70f458e0871ceaeb5de89fd.png)
you can then exit then confirm
![](attachment/8a687ccb59ce2e26980b1a4fb500b5f0.png)