Multicast/PIM puede ser algo complicado, pero a la vez interesante desde el punto de vista de Control Plane.

Cuando se enfrenten a hacer troubleshoot de PIM Multicast, sigan los siguientes pasos.
80% de las ocasiones, siguiendo estos pasos estarán cerca de encontrar por que el tráfico de Multicast no llega a los receivers.




    Step 0

-    IP address del Multicast Sender.
-    IP address del Multicast Group.
-    IP address del Multicast Receiver.

    First Hop Router

1. Ir al o los Nexus que son el Layer 3 Gateway del Servidor enviando el tráfico de Multicast.
2. Vemos en dichos Nexus el (S,G)?    show ip mroute <Multicast Group>

- Si no vemos el (S,G) es que el Source no se encuentra enviando el tráfico de Multicast o PIM no se encuentra habilitado en la interface VLAN.
- Verifiquen que el tráfico de Multicast no sea enviado con TTL=1 en el IP header.

3. En el mismo equipo corran show ip pim rp <Multicast Group> e identifiquen la IP del RP (Rendezvous Point).


    Rendezvous Point

4. Vayan al equipo que es el RP y corran show ip mroute <Multicast Group>, si ven el mismo (S,G) estamos bien.
5. Si no ven el (S,G) hay un problema entre el RP y el Sender.
6. En el mismo equipo corran show ip pim rp <Multicast Group> e identifiquen la IP del RP (Rendezvous Point) debe ser una IP local.


    Last Hop Router

7. Ir al o los Nexus que es el Layer 3 Gateway del o los equipos que espera recibir el tráfico de Multicast (‘Receivers’).
8. Vemos en dichos Nexus el (*,G)?    show ip mroute <Multicast Group>

- Si no vemos el (*,G) es que no hemos recibido el IGMP Membership Report de algún Receiver (show ip igmp group <Multicast Group>) o PIM se encuentra deshabilitado en la interface VLAN.

9. En el mismo equipo corran show ip pim rp <Multicast Group> e identifiquen la IP del RP (Rendezvous Point).

10. Vemos en dichos Nexus el (S,G)?

-Si vemos el (S,G)  en la mayoría de los casos estamos recibiendo el tráfico de Multicast y lo estamos enviando local via Layer 2 al Receiver.

11. En cualquier momento podemos correr show ip mroute <Multicast Group> summary para verificar que los contadores incrementen (tardan en refrescarse).
