# TP4



 
 ## DEMO 

 - **Créer les nouveaux utilisateurs comme suit:**  
      A) Equipe Dev :

      * [ username: dev1, password: dev1 ]
      * [ username: dev2, password: dev2 ]
      
      B) Equipe Test :

      * [ username: tester1, password: tester1 ]
      * [ username: tester2, password: tester2 ]
     
     
     C) Equipe DevSecOps :      
      * [ username: devsecops1, password: devsecops1 ]
      * [ username: devsecops2, password: devsecops2 ]


```sql
  create user dev1 identified by dev1 ;
  create user dev2 identified by dev2 ;
  create user tester1 identified by tester1 ;
  create user tester2 identified by tester2 ;
  create user devsecops1 identified by devsecops1 ;
  create user devsecops2 identified by devsecops2 ;
---
```
  --->  **Une fois qu'un utilisateur est créé, le DBA peut octroyer des privilèges de système spécifiques à cet utilisateur.**
 

  - **Attribuer les privilèges ci-dessous à l'utilisateur dev1 :** 
 
     * Création de procédures stockées.
     * Création de vue.
     * Création de séquence.
     * Création de session.
     * Création,lecture, modification de structure et suppression de tables.

```sql
  grant 
  create procedure ,
  create view ,
  create sequence ,
  create session ,
  create any table ,
  select any table ,
  alter any table ,
  drop any table 
  to dev1 ;
---
```

¤   **Une fois qu'un utilisateur est créé, le DBA peut octroyer des privilèges de système spécifiques à cet utilisateur.**
 
 
   - **Révoquer tous les privilèges associès à l'utilisateur dev1 :** 

```sql
  revoke 
  create procedure ,
  create view ,
  create sequence ,
  create session ,
  create any table ,
  select any table ,
  alter any table ,
  drop any table 
  from dev1 ;
---
```

 
  - **Créer des rôles dédiés pour chaque Equipe d'utilisateurs en réspectant les critères suivants** 

      A) Le rôle de l'équipe Dev permet de:

      * Création de procédures stockées.
      * Création de vue.
      * Création de séquence.
      * Création de session.
      * Création,lecture, modification de structure et suppression de tables.
      
      B) Le rôle de l'équipe Test permet de:

      * Se connecter à la base de données.
      * Création de session.
      * Lecture données de toutes les tables.
     
     C) Le rôle de l'équipe DevSecOps permet d'avoir tous les privilèges avec mode administrateur de la base:  

```sql
  create role dev ;
  create role test ;
  create role devsecops ;
---
```
```sql
  grant 
  create procedure ,
  create view , 
  create sequence ,
  create session ,
  create any table ,
  select any table ,
  alter any table ,
  drop any table 
  to dev ;
---
```
```sql
  grant 
  connect ,
  create session ,
  select any table 
  to test ;
---
```
```sql
  grant 
  ALL PRIVILEGES 
  to devsecops with admin option ;
---
```


 
   - **Attribuer à chaque utilisateur, le rôle qui lui correspond:** 
  

```sql
    grant dev 
    to dev1,dev2 ;
---
```
```sql
    grant test 
    to tester1,tester2 ;
---
```
```sql
    grant devsecops 
    to devsecops1,devsecops2 ;
---
```

   - **Limiter l'accès pour les testeurs de sorte qu'ils n'accèdent qu'à la table des employés "EMP":** 
  

```sql
    revoke 
    select any table 
    from test ;
---
```

 ```sql
    grant 
    select on emp 
    to test ;
---
```
 
 
 
   - **Autoriser tous les utilisateurs sur le système pour interroger les données de la table EMP :** 
  

 ```sql
    grant 
    select on emp 
    to dev,test,devsecops ;
---
```

**Retirer les privilèges attribuées aux admins, ainsi que les utilisateurs qui ont reçu leurs privilèges sur la table EMP par un membre de l'équipe devsecops:**

 
 
```sql
    revoke 
    all privileges 
    ON emp 
    from devsecops ;
---
```


**Créer un profile de ressources dédié à l'équipe des développeurs avec les limitations suivantes:**
  * ***Nombre maximal de sessions permises par utilisateur:*** **illimité**
  * ***Nombre maximal de CPU par session:*** **10000**  
  * ***Nombre maximal de CPU par appel à la base :*** **1000**
  * ***Durée maximal en secondes d'une session:*** ***45*** 
  * ***Nombre maximal de lectures logiques par session utilisateur:*** ***Valeur par defaut***
  * ***Nombre maximal de lectures logiques par appel à la base:*** ***1000***
  * ***Taille maximale de l'SGA privée:*** ***25K***
  * ***Durée de vie en jours du mot de passe:*** ***60***
  * ***Nombre maximal de réutilisations de mot de passe:*** ***10***



```sql 
    create profile developeur 
    limit 
    SESSIONS_PER_USER UNLIMITED 
    CPU_PER_SESSION 10000
    CPU_PER_CALL 1000
    CONNECT_TIME 45
    LOGICAL_READS_PER_SESSION DEFAULT 
    LOGICAL_READS_PER_CALL 1000
    PRIVATE_SGA 25K 
    PASSWORD_LIFE_TIME 60
    PASSWORD_REUSE_TIME 10 ; 
---
```




**Créer un profile de ressources dédié à l'équipe de test avec les limitations suivantes:**
  * ***Nombre maximal de sessions permises par utilisateur:*** **5**
  * ***Nombre maximal de CPU par session:*** **illimité**  
  * ***Nombre maximal de CPU par appel à la base :*** **3000**
  * ***Durée maximal en secondes d'une session:*** ***45*** 
  * ***Nombre maximal de lectures logiques par session utilisateur:*** ***Valeur par defaut***
  * ***Nombre maximal de lectures logiques par appel à la base:*** ***1000***
  * ***Taille maximale de l'SGA privée:*** ***25K***
  * ***Durée de vie en jours du mot de passe:*** ***60***  
  * ***Nombre maximal de réutilisations de mot de passe:*** ***10***
```sql 
    create profile TESTEUR  
    limit 
    SESSIONS_PER_USER 5 
    CPU_PER_SESSION UNLIMITED 
    CPU_PER_CALL 3000
    CONNECT_TIME 45
    LOGICAL_READS_PER_SESSION DEFAULT 
    LOGICAL_READS_PER_CALL 1000
    PRIVATE_SGA 25K 
    PASSWORD_LIFE_TIME 60
    PASSWORD_REUSE_TIME 10 ; 
---
```

**Créer un profile de ressources dédié à l'équipe devsecops avec les limitations suivantes:**
  * ***Nombre maximal de sessions permises par utilisateur:*** **illimité**
  * ***Nombre maximal de CPU par session:*** **illimité**  
  * ***Nombre maximal de CPU par appel à la base :*** **3000**
  * ***Durée maximal en secondes d'une session:*** ***3600*** 
  * ***Nombre maximal de lectures logiques par session utilisateur:*** ***Valeur par defaut***
  * ***Nombre maximal de lectures logiques par appel à la base:*** ***5000***
  * ***Taille maximale de l'SGA privée:*** ***80K***
  * ***Durée de vie en jours du mot de passe:*** ***60***
  * ***Nombre maximal de réutilisations de mot de passe:*** ***10***

```sql 
    create profile DEVSECOPS 
    limit 
    SESSIONS_PER_USER UNLIMITED 
    CPU_PER_SESSION UNLIMITED
    CPU_PER_CALL 3000
    CONNECT_TIME 3600
    LOGICAL_READS_PER_SESSION DEFAULT 
    LOGICAL_READS_PER_CALL 5000
    PRIVATE_SGA 80K 
    PASSWORD_LIFE_TIME 60
    PASSWORD_REUSE_TIME 10 ; 
---
```

  - **Attribuer à l'utilisateur "dev1", le profile qui lui correspond:** 
```sql
    alter user dev1 PROFILE developeur ;
---
```
