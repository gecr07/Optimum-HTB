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



































































