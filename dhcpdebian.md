# CONFIGURAR UN SERVIDOR DHCP EN DEBIAN
### Ire realizando un ejemplo a par que el tutorial

1. [Maquinas virtuales necesarias](1--maquinas-virtuales-necesarias)
2. [Configuracion de las maquinas virtuales]()
3. [Configuracion de Pfsense](3--configuracio-de-pfsense)
4. [Arrancar y parar el servidor](4--arrancar-y-parar-el-servidor)
5. [Comprobacion emn equipo cliente](5--comprobacion-en-equipo-cliente)
 ## 1- Maquinas virtuales necesarias
 Para poder configurar un servidor DHCP en dbian necesitaremos una maquina virtual con Debian que funcionara como servidor, una con PFsense que funcionara como un router y otra con Windows 10 que funcionara como cliente.

 ## 2- Configuracion de las maquinas virtuales 
 - PfSense: Debera tener dos adaptadores de red uno en puente o NAT para tener salida a internet y otro en red interna para conectar con el resto de maquinas.


   ![image](https://github.com/ManuFdzDC/ManuelFernandezSRI/assets/144890528/a56a4bce-ca6e-4235-978c-981d6d72e7a0)
   ![image](https://github.com/ManuFdzDC/ManuelFernandezSRI/assets/144890528/26ee2784-898e-4ccc-8f6a-67755444548e)

 - Debian y Windows10: Tienen que tener un adaptador de red en red interna y tiene que ser la misma red que tenga el PFsense.

   
   ![image](https://github.com/ManuFdzDC/ManuelFernandezSRI/assets/144890528/0d5af25e-0eec-4b6d-801a-9e616b9a080d)
   ![image](https://github.com/ManuFdzDC/ManuelFernandezSRI/assets/144890528/b81fa5bb-b3ac-4916-877d-04d806b156a5)

 ## 3- Configuracion de PFsense
 Despues de arrancar el pfsense desde una de las otras maquinas accederemos a la pagina de configuracion desde el navegador escribriendo la IP del router que por defecto es 192.168.1.1, nos pedira una identificacion la cual sera usuario: admin y contraseña: pfsense.
 Una vez dentro de la pagina de configuracion buscaremos la pestaña services y dentro de services DHCP Server, la primera opcion debe estar desactivada.
 
   ![image](https://github.com/ManuFdzDC/ManuelFernandezSRI/assets/144890528/e2b6021f-29dc-4150-b10c-9fb89cdea98a)
   
  Volveremos a la pestaña principal y en la pestaña interfaces vamos a LAN y configuramos la IP

   ![image](https://github.com/ManuFdzDC/ManuelFernandezSRI/assets/144890528/830b803a-30af-42dc-b582-6e496ea74070)
   
  Con esto el PFsense ya quedaria totalmente configurado.

  ## 3- Configuracion Debian 
  Lo primero es configurar una ip estatica para la mquina yo pondre la 192.168.7.50 esto se hace desde la 
  configuracion grafica de red tambien pondremos la mascara de red (255.255.255.0), la puerta de enlace 
  (192.168.7.1) y el DNS (10.0.1.48). Es recomendable reiniciar y asegurarse de que la ip sea correcta.Esta 
  configuracion puede variar dependiendo de la red que se quiera utilizar.

  ![image](https://github.com/ManuFdzDC/ManuelFernandezSRI/assets/144890528/79c30e4f-0a71-43fa-b09e-dd63b146e439)


  Despues de tener configurada la ip estatatica podremos configurar el servidor.
  - Utilizaremos el coamndo `su -` para trabajacomo superusuario, nos pedira la contraseña y despues de 
    introducirla ya estaremos como superusuario.
  - Con el comando `apt install isc-dhcp-server` instalaremos el servicio para despues configurar y arrancar 
    el servidor dhcp.
    ![image](https://github.com/ManuFdzDC/ManuelFernandezSRI/assets/144890528/a7439309-c56f-4c17-9216-0025aefe06c8)
  - Una vez este instalado debemos especificar en que targeta de red escuchara el servidor, para ello            utilizaremos el comando `nano /etc/default/isc-dhcp-server` modificaremos el arvhivo y pondremos la          targeta de red correcta en INTERFACESv4 el nombre de la targeta debe ir entre comillas.
    
    ![image](https://github.com/ManuFdzDC/ManuelFernandezSRI/assets/144890528/e015ff24-af0d-4c49-8c14-1ffb19426f1a)
 
    Para comprobar el nombre de la targeta de red se puede utilizar le comando `ip a`

    ![image](https://github.com/ManuFdzDC/ManuelFernandezSRI/assets/144890528/d348ac27-1f99-475e-a782-5945d9347bed)

    Aparecen dos targetas nos fijaremos en la segunda que es la que tiene la ip que previamente habiamos 
    configurado. En este caso mi targeta es enp0s8.

  - Ahora tenemos que configurar los parametros del servidor modificando el archivo de configuracion, para 
    acceder a el usaremos `nano /etc/dhcp/dhcpd.conf`. Antes de modificar nada es recomendable hacer una 
    copia del archivo. Dentro del archivo localizaremos la parte que aparece en la siguiente captura y configuraremos los       parametros de IP, los rangos de las direcciones que repartiran a los clientes, el tiempo maximo y minimo que el            cliente tendra la IP, el DNS y el nombre de dominio.
    
    ![image](https://github.com/ManuFdzDC/ManuelFernandezSRI/assets/144890528/88ab70c2-9c71-41a8-a9ed-b27a7491992d)

En la captura se puede ver que hay dos rangos de IP esta hecho asi para excluir la 192.168.7.50 que es la que tiene el servidor.

## 4- Arrancar y parar el servidor
Para arrancar el servidor utilizaremos el comando `systemctl start isc-dhcp-server`, si todo funciona bien el comando no tendra respuesta.
Ahora comprobaremos que esta funcionando con el comando `systemctl status isc-dhcp-server`, en el apartado active pondra active en verde si todo esta funcionando correctamente. 

![image](https://github.com/ManuFdzDC/ManuelFernandezSRI/assets/144890528/1e44e0f8-157f-4b47-beb0-2670cc30f4a3)

Para parar el servicio DHCP se utilizara el comando `systemctl stop isc-dhcp-server`.

## 5. Comprobacion en equipo cliente

Para comporbar que funciona el servico DHCP uniremos un cliente Windows 10, para ello debe estar en la misma red interna que el servidor y servicio funcionando en Debian.
Con esto el cliente estara unido solo falta comprobarlo con `ipconfig /all` y el resultado sera similar a la captura de pantalla siguiente.

![image](https://github.com/ManuFdzDC/ManuelFernandezSRI/assets/144890528/a50d0a63-7cea-4c62-bf64-fa0a62452e16)










  
