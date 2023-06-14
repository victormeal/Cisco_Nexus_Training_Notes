# Custom CoPP

+ By default, every Nexus 9000 has a “default” CoPP already applied. This one is the “strict” policy.
+ We cannot customize it. What we can do is make a copy of it and in this new one we can edit as we want.
+ This policy has many things on it. The policy-map itself containing the class-map and the class-maps contain ACLs. Each of it has a name associated to it. So when making the copy everything will be renamed differently.
+ Here are the steps to do this…

```
Step 1. Make a copy form the default Strict policy.
 
Copp copy profile strict suffix TAC
 
Step 2. Start Editing
 
conf t
Policy-map type control-plane copp-policy-strict-TAC
   Class copp-class-management-TAC
      Police cir 36000 bc 64000
 
Step 3. Apply the new policy to the control-plane
 
Control-plane
   Service-policy input copp-policy-strict-TAC
```


+ From those steps, you can change the name to something else and change the CIR and BC values to what you want.
+ In case you want to go back to the default Strict policy you can use the command “copp profile strict.”
+ You can check, which policy-map is applied with the command “show policy-map interface control-plane”
