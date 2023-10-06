# CONFIGURAR UN SERVIDOR DHCP EN DEBIAN

1. [Maquinas virtuales necesarias]()
2. [Configuracion de las maquinas virtuales]()

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

   ![image](https://github.com/ManuFdzDC/ManuelFernandezSRI/assets/144890528/8c971d6c-b682-4e5f-8ec0-4e09dedb3ea8)


 
