#!/bin/bash
# Ejercicio 1 del Boletin
Fecha=$(date +%d-%m-%Y)
# Bloque principal
informe ()
{
echo "-----------------------------------"
echo "  Informe de ocupación del disco."
echo "-----------------------------------"
echo "Fecha= $Fecha"
df -hT "/"
df -hT | grep "home"
}
informe>/home/usuario/Scripts/RECUPERACION/InformeDisco-$Fecha.inf

#!/bin/bash
# Ejercicio 2 del Boletin
# Variables
usuario=$1
# Funciones
Seleccion ()
{
echo "Vamos a ver el tamaño de un usuario"
read -p "Introduzca el usuario: " usuario
}
ComprobarUsuario ()
{
    tamano=$(getent passwd | grep -w "$usuario")
    echo "$tamano"
    if [ -z "$tamano" ]
    then
        echo "El usuario $usuario NO existe"
        exit 1
    else
        du -hs /home/"$usuario"
    fi
}
# Bloque principal
Seleccion
ComprobarUsuario

#!/bin/bash
# Ejercicio 3 del Boletin
# Variables
usuario=$1
# Funciones
ComprobarRoot ()
{
    if [ $(whoami) != "root" ]
    then
        echo "No eres el root"
        echo "No podemos seguir adelante"
    fi
}
Tamano ()
{
    echo "Estos son los tamaños de los usuarios"
    echo "-----------------------------------------------"
    usuario=$(getent passwd | awk -F: '$3>1000 && $3<2000 {print $1}')
    for i in $usuario
    do
        du -hs /home/"$i"
    done
}
# Bloque Principal
ComprobarRoot
Tamano

#!/bin/bash
# Ejercicio 4 del Boletin
# Variables
Fecha=$(date +%d-%m-%Y)
# Funciones
ComprobarRoot ()
{
    if [ $(whoami) != "root" ]
    then
        echo "No eres el root"
        echo "No podemos seguir adelante"
        exit 1
    fi    
}
ComprobarDirectorio ()
{
    if [ -f /home/usuario/Escritorio/Intermedio/servidores.txt ]
    then
        echo "El fichero indicado existe"
        echo "Puede continuar"
    else
        echo "El directorio indicado no existe"
        echo "No podemos continuar"
        exit
    fi
}
Cabecera ()
{
        echo "---------------------------------"
        echo "     INFORME DE SERVIDORES"
        echo "     Fecha: $Fecha"
        echo "---------------------------------"
}
ComprobarPing ()
{
    while read linea
    do
        IP=$(echo $linea | cut -d: -f2)
        respuesta=$(ping -w 1 "$IP" | grep -w "0 received" | cut -d, -f2)
        if [ -z "$respuesta" ]
        then
            echo "$IP - Esta IP tiene conexion"
        else
            echo "$IP - Esta IP NO tiene conexion"
        fi
    done </home/usuario/Escritorio/Intermedio/servidores.txt
}
# Bloque principal
ComprobarDirectorio
ComprobarRoot
Cabecera >InformeServidores-"$Fecha".inf
ComprobarPing >>InformeServidores-"$Fecha".inf
cat InformeServidores-"$Fecha".inf

#!/bin/bash
# Ejercicio 5 del boletin
# Variables
Fecha=$(date +%d-%m-%Y)
red="192.168.1"
# Funciones
Cabecera ()
{
    echo "----------------------"
    echo "  INFORME DE EQUIPOS  "
    echo "  Fecha: $Fecha"
    echo "----------------------"
}
ComprobarRoot ()
{
    if [ $(whoami) != "root" ]
    then
        echo "No eres el root"
        echo "No podemos continuar"
        exit 1
    fi
}
Conexion ()
{
    read -p "Introduce el valor inicial: " valor1
    read -p "Introduce el ultimo valor: " valor2
    for i in $(seq "$valor1" "$valor2")
    do
        ip=$red.$i
        #ping -w 1 192.168.1.$i | grep -w "192" | awk '{print $2'} | head -n 1
        respuesta=$(ping -w 1 192.168.1.$i | grep -w "0 received")
        if [ -z "$respuesta" ]
        then
            echo "$ip: Si hay conexion"
        else
            echo "$ip: No hay conexion"
        fi
    done
}
# Bloque principal
ComprobarRoot
Cabecera >/home/usuario/Escritorio/Intermedio/InformePing-$Fecha.inf
Conexion >>/home/usuario/Escritorio/Intermedio/InformePing-$Fecha.inf

#!/bin/bash
# Ejercicio 6 del boletin
# Variables
Fecha=$(date +%d-%m-%Y)
parametro=$1
# Funciones
ComprobarFichero ()
{
    if [ -f /home/usuario/Escritorio/Intermedio/dhcpd.leases ]
    then
        echo "El fichero existe, puede continuar"
    else
        echo "El fichero no existe"
        exit 1
    fi
}
ComprobarParametro ()
{
    if [ -n "$parametro" ]
    then
        echo "Existe"
    else
        echo "No existe"
    fi
}
Separador ()
{
    cat /home/usuario/Escritorio/Intermedio/dhcpd.leases | grep "^lease" | cut -d" " -f2 > /home/usuario/Escritorio/Intermedio/ip.tmp
    cat /home/usuario/Escritorio/Intermedio/dhcpd.leases | grep "hardware ethernet" | cut -d" " -f5 | tr -d ";" > /home/usuario/Escritorio/Intermedio/mac.tmp
    paste -d+ ip.tmp mac.tmp > /home/usuario/Escritorio/Intermedio/resultado.tmp
}
Informe ()
{
    echo "Concesiones de IP"
    echo "----------------------------------"
    cat resultado.tmp
}
# Bloque principal
ComprobarParametro
Separador
ComprobarFichero
Informe

#!/bin/bash
# Ejercicio 7 del Boletin
# Variables
Fecha=$(date +%d-%m-%Y)
# Funciones
ComprobarUsuario ()
{
    read -p "Introduzca un usuario valido: " usuario1
    getent passwd | grep -o "$usuario1" > /dev/null 2>&1
    if [ $? = 0 ]
    then
        echo "Usuario correcto, puede continuar"
    else
        echo "Usuario incorrecto, no puede continuar"
        exit 1
    fi
}
CopiaUsuario ()
{
    tar -zcvf /root/datos/$usuario1-$Fecha.tar.gz /home/$usuario1/Escritorio #> /dev/null 2>&1
}
ComprobarRoot ()
{
    if [ $(whoami) != "root" ]
    then
        echo "No eres el root, no podemos seguir"
        exit 1
    fi
}
# Bloque principal
ComprobarRoot
ComprobarUsuario
CopiaUsuario
ls /root/datos

#!/bin/bash
# Ejercicio 8 del boletin
# Variables
Fecha=$(date +%d-%m-%Y-%H-%M)
# Funciones
Menu ()
{
    echo "-------------------------------------------"
    echo "Copia y restauracion de copias de seguridad"
    echo "-------------------------------------------"
    echo "1.- Realizar copia de seguridad"
    echo "2.- Restaurar copia de seguridad"
    echo "3.- Salir"
    read -p "Elige una opcion: " opcion
    case $opcion in
    1)
    CopiaUsuario
    ;;
    2)
    RestauraCopia
    ;;
    3)
    exit 0
    ;;
    esac
}
ComprobarUsuario ()
{
    read -p "Introduzca un usuario: " usuario1
    getent passwd | grep -o "$usuario1" > /dev/null 2>&1
    if [ $? = 0 ]
    then 
    echo "Usuario conectado"
    else 
    echo "Usuario no conectado"
    echo "No podemos continuar"
    echo "El usuario $usuario1 no está conectado"
    exit 1
    fi
}
CopiaUsuario()
{
    ComprobarUsuario
    tar -zcvf /root/datos/"$usuario1"-"$Fecha".tar.gz /home/"$usuario1"/Escritorio
}
RestauraCopia () {
    echo "Restaurar Copia de Usuario"
    ComprobarUsuario "$usuario1"
    resultado=$?
    if [ "$resultado" -eq 0 ]
    then
        #copia=$(ls -t /root/datos/$usuario* | nl | grep -w "1" | cut -f2)
        ls -l /root/datos/* | nl
        echo "Introduzca la ruta completa de la copia"
        read -p "¿Que copia quieres restaurar?: " copia
        tar -zxvf "$copia" -C /home/usuario
    else
        echo "No se ha podido restaurar la copia"
    fi
}
# Bloque principal
Menu

#!/bin/bash
# Ejercicio 9 del Boletin
# Variables
Fecha=$(date +%d-%m-%Y)
# Funciones
Menu ()
{
    echo "-------------------"
    echo "1.- Restaurar copia"
    echo "2.- Salir"
    echo "-------------------"
    read -p "Selecciona que quieres realizar: " num
    case $num in
    1)
    RestauraCopia
    ;;
    2)
    exit 1
    ;;
    esac
}
RestauraCopia ()
{
    ls /root/datos/ | nl
    read -p "Elige que copia quieres restaurar: " copia
    if [ -z "$copia" ]
    then
        seguridad=$(ls -t /root/datos/* | nl | grep -w "1" | cut -f2)
        tar -zxvf $seguridad -C /home/usuario/Escritorio
    else
        seguridad2=$(ls -t /root/datos/* | nl | grep -w "$copia" | cut -f2)
        tar -zxvf $seguridad2 -C /home/usuario/Escritorio
    fi
}
# Bloque principal
Menu

#!/bin/bash
# Ejercicio 10 del Botelin
# Variables
Fecha=$(date +%d-%m-%Y-%H-%M)
parametro1=$1
parametro2=$2
# Funciones
CopiaUsuario ()
{
    echo "Copia realizada"
    tar -zcvf /root/datos/"$parametro2"-"$Fecha".tar.gz /home/"$parametro2"/Escritorio >/dev/null 2>&1
}
RestauraCopia ()
{
    echo "Copia restaurada"
    klk=$(ls -h /root/datos | nl -s ";" | cut -d";" -f2 | tail -n1)
    tar -zxvf /root/datos/"$klk" -C /home/usuario/Escritorio >/dev/null 2>&1
}
Parametro ()
{
if [ "$parametro1" = "c" ]
then
    CopiaUsuario
fi

if [ "$parametro1" = "r" ]
then
    RestauraCopia
else
    exit 1
fi
}
ComprobarUsuario ()
{
getent passwd | grep -w "$parametro2" >/dev/null 2>&1
if [ $? = 0  ]
then
    echo "El usuario $parametro2 existe"
else
    echo "El usuario $parametro2 NO existe"
    exit 1
fi
}
# Bloque principal
ComprobarUsuario
Parametro