# VSCode WSL (Windows)

### 1. Open VSCode -> Extensions -> Search for WSL -> Install Remote WSL (from Microsoft) Extension

### 2. Press (CTRL + SHIFT + P) on VSCode, type "Open WSL Window"

When prompted with an error "WSL not avaiable on your system", click on "documentation" button as shown in the video below.

### 3. Open Powershell with Administrator Priviledges, run

`wsl --install`

![Follow the steps in the video (part I)](../../../.gitbook/assets/WSL\_install\_1.gif)

{% hint style="warning" %}
Your PC will have to restart after following the steps in the video.
{% endhint %}

### 4. Let it install Ubuntu automatically after the restart

![](../../../.gitbook/assets/WSL\_install\_2.gif)

{% hint style="info" %}
You will have to provide username and the password during the installation of Ubuntu to enter into Ubuntu later.
{% endhint %}

### 5. Open VSCode after the installation of WSL and Ubuntu

* Follow the video and **open a folder in WSL**
* Install `gcc, gdb, valgrind, make` tools in the terminal as shown below:
* Now you can compile your code in WSL, Ubuntu on Windows ! :)

![](../../../.gitbook/assets/WSL\_install\_3.gif)
