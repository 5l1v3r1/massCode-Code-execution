# massCode Code execution (CVE-2020-8548)

A few days back I was looking for a tool to maintain my notes and important code snippets and I came across a tool called massCode

### About massCode

massCode is one of the free and open-source code snippet manager tool build with the electron. Sometime back it was in trending on GitHub and also listed on electron website
https://www.electronjs.org/apps/masscode

![1](https://github.com/c0d3G33k/massCodeRCE/blob/master/Screenshot_1.png)

### massCode makrdown editor 

You can select different programming languages to render respecting code snippets but my interest was in markdown editor. Here is a quick image of how massCode markdown editor works

![2](https://github.com/c0d3G33k/massCodeRCE/blob/master/Screenshot_2.png)
![3](https://github.com/c0d3G33k/massCodeRCE/blob/master/Screenshot_3.png)

### XSS in massCode makrdown editor 

Next, As usual, I tried to inject the `script` tag to see if it gets executed 

![4](https://github.com/c0d3G33k/massCodeRCE/blob/master/Screenshot_4.png)

But nothing happened. 

![5](https://github.com/c0d3G33k/massCodeRCE/blob/master/Screenshot_5.png)

Again i tried to inject `<a>` tag as shown in below image
![6](https://github.com/c0d3G33k/massCodeRCE/blob/master/Screenshot_6.png)

and luckily it worked this time. easy-peasy

![7](https://github.com/c0d3G33k/massCodeRCE/blob/master/Screenshot_7.png)

### Code execution in massCode

Since massCode is built on electron and we have XSS vulnerability at the same time. I quickly navigate to the source code available on GitHub, and figured out that `nodeIntegration` flag is set to `true`.

![8](https://github.com/c0d3G33k/massCodeRCE/blob/master/Screenshot_8.png)
which means we can invoke node API's. Next I created a simple XSS payload to open a calculator on windows 
```
<a href="javascript:try{ const {shell} = require('electron'); shell.openExternal('file:C:/Windows/System32/calc.exe') }catch(e){alert(e)}">aaaaaaa</a>
```

![poc-gif](https://github.com/c0d3G33k/massCodeRCE/blob/master/Screen-Recording-_02-02-2020-11-06-09_.gif)

This issue has been fixed in latest relase of massCode
