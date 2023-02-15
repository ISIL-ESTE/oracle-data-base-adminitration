C:\app\Khalid\product\21c\admin\XE\pfile\init.ora.10142022163950

--- Administration des Bases de Données ORACLE ---
------- Atelier N° 1 ----------
4 )


```
SQL> create spfile='C:\app\Khalid\product\21c\admin\XE\pfile\isil.ora' from pfile='C:\app\Khalid\product\21c\admin\XE\pfile\init.ora.10142022163950';

File created.

```

SQL> disconnect
Disconnected from Oracle Database 21c Express Edition Release 21.0.0.0.0 - Production
Version 21.3.0.0.0

5 )
SQL> connect sys as sysdba
Enter password:
Connected to an idle instance.
SQL> startup pfile='C:\app\Khalid\product\21c\admin\XE\pfile\init.ora.10142022163950'
ORACLE instance started.

Total System Global Area 1610609432 bytes
Fixed Size 9855768 bytes
Variable Size 402653184 bytes
Database Buffers 1191182336 bytes
Redo Buffers 6918144 bytes
Database mounted.
Database opened.

6 )

SQL> shutdown
Database closed.
Database dismounted.
ORACLE instance shut down.

SQL> startup open read only
ORACLE instance started.

Total System Global Area 1610609432 bytes
Fixed Size 9855768 bytes
Variable Size 989855744 bytes
Database Buffers 603979776 bytes
Redo Buffers 6918144 bytes
Database mounted.
Database opened.

7 )
alter session set "\_Oracle_Script"=True;
create user HR identified by hr;

User created.
grant CREATE SESSION, ALTER SESSION, CREATE DATABASE LINK, -
CREATE MATERIALIZED VIEW, CREATE PROCEDURE, CREATE PUBLIC SYNONYM, -
CREATE ROLE, CREATE SEQUENCE, CREATE SYNONYM, CREATE TABLE, -
CREATE TRIGGER, CREATE TYPE, CREATE VIEW, UNLIMITED TABLESPACE -
to chris;

Grant succeeded.

SQL> grant select on regions to HR;

Grant succeeded.

SQL> grant insert on regions to HR;

Grant succeeded.

8 )
SQL> insert into sys.regions values(99,'marocoo');
insert into sys.regions values(99,'marocoo') \*
ERROR at line 1:
ORA-16000: database or pluggable database open for read-only access

9.

SQL> shutdown
Database closed.
Database dismounted.
ORACLE instance shut down.
SQL> startup open read write
ORACLE instance started.

Total System Global Area 1610609432 bytes
Fixed Size 9855768 bytes
Variable Size 989855744 bytes
Database Buffers 603979776 bytes
Redo Buffers 6918144 bytes
Database mounted.
Database opened.

10 )
SQL> connect HR/hr
Connected.
SQL> insert into sys.regions values(99,'marocoo');

1 row created

11 )

SYS = SQL> shutdown transactional
Database closed.
Database dismounted.
ORACLE instance shut down.

12 )
HR = SQL> Rollback;
Rollback complete.

SQL> disconnect
Session ID: 499 Serial number: 11185

Disconnected from Oracle Database 21c Express Edition Release 21.0.0.0.0 - Production
Version 21.3.0.0.0 (with complications)

13 )

SQL> alter system enable restricted session;

System altered.

n )

SQL> connect HR/hr
ERROR:
ORA-01035: ORACLE only available to users with RESTRICTED SESSION privilege

n+1 )

SQL> alter system disable restricted session;

System altered.

-------------- Gestion des utilisateurs, privilèges et rôles -------------------

1.  create table emp_isil(id_emp number, nom varchar2(50), prenom varchar2(50), salaire number);
    Table created.

2.  SQL> insert into emp_isil values(1,'amri','amjad', 5000);
    1 row created.
    SQL> insert into emp_isil values(2,'boussaroual','khalid', 15000);
    1 row created.
    SQL> insert into emp_isil values(3,'alla','ismail', 7000);
    1 row created.

3.

SQL> alter session set "\_oracle_script"=true;

Session altered.

SQL> create user user_isil identified by isil quota 100 M on USERS default tablespace users password expire;

User created.

select \* from all_users where lower(username) = 'user_isil';

## USERNAME

USER_ID CREATED COM O INH

---

## DEFAULT_COLLATION

IMP ALL EXT

---

USER_ISIL
111 14-FEB-23 YES Y NO
USING_NLS_COMP
NO NO NO

4.  SQL> create role R_isil;
    SQL> select \* from dba_roles where lower(role) = 'r_isil';

## ROLE

ROLE_ID PASSWORD AUTHENTICAT COM O INH IMP

---

## EXTERNAL_NAME

R_ISIL
112 NO NONE YES Y NO NO

5.

SQL> grant create session, create table to R_isil;

Grant succeeded.

SQL> grant select on emp_isil to R_isil;

Grant succeeded.

6.

SQL> grant R_isil to user_isil;

Grant succeeded. 7)

SQL> select GRANTEE from dba_role_privs where lower(GRANTEE) = 'user_isil';

## GRANTEE

USER_ISIL

8.

Enter user-name: user_isil
Enter password:
ERROR:
ORA-28001: the password has expired

Changing password for user_isil
New password:
Retype new password:
Password changed

Connected to:
Oracle Database 21c Express Edition Release 21.0.0.0.0 - Production
Version 21.3.0.0.0

SQL> show user
USER is "USER_ISIL"
SQL>

9.

SQL> select \* from user_tab_privs;

---

|GRANTEE | OWNER | TABLE_NAME | GRANTOR | PRIVILEGE | GRA | HIE | COM | TYPE | INT |

---

|PUBLIC | SYS | USER_ISIL | USER_ISIL | INHERIT PRIVILEGES | NO | NO | NO | USER | NO |

10.

SQL> select \* from user_sys_privs;

no rows selected.

11.

SQL> select \* from session_roles;

## ROLE

R_ISIL

12.

SQL> select \* from sys.emp_isil;

    ID_EMP NOM

---

PRENOM SALAIRE

---

         1 amri

amjad 5000

         2 boussaroual

khalid 15000

         3 alla

ismail 7000

13.

SQL> create table service (id number primary key, libelle varchar2(15));

Table created.

14.

SQL> revoke create table from R_isil;

Revoke succeeded.

15.

SQL> select \*
2 from role_sys_privs
3 where ROLE = 'R_ISIL';

## ROLE

PRIVILEGE ADM COM INH

---

R_ISIL
CREATE SESSION NO YES NO

16.

SQL> create table emp(id_emp number, nom varchar2(50), prenom varchar2(50), salaire number);
create table emp(id_emp number, nom varchar2(50), prenom varchar2(50), salaire number)

- ERROR at line 1:
  ORA-01031: insufficient privileges

17.

SQL> revoke r_isil from user_isil;

Revoke succeeded.

18.

SQL> select \* from session_roles;

## ROLE

R_ISIL

19.

SQL> grant r_isil to public;

Grant succeeded.

20.

SQL> select \* from session_roles;

## ROLE

R_ISIL

21.

SQL> revoke r_isil from user_isil;
revoke r_isil from user_isil

- ERROR at line 1:
  ORA-01951: ROLE 'R_ISIL' not granted to 'USER_ISIL'

22.

SQL> grant update(salaire) on sys.emp_isil to user_isil;

Grant succeeded.

23.

SQL> update sys.emp_isil set salaire = 3000 where id_emp = 1;

1 row updated.

24.

select \* from user_col_privs;

---

|GRANTEE | OWNER | TABLE_NAME | COLUMN_NAME | GRANTOR | PRIVILEGE |

---

|USER_ISIL | SYS | EMP_ISIL | SALAIRE | SYS | UPDATE |

**************\_\_\_************** cerate tablespace **************\_\_\_**************

-------- datafile ---------

SQL> create tablespace ts_isil datafile 'C:\app\Khalid\product\21c\oradata\XE\ISIL\ts_isil.dbt' size 2 m, 'C:\app\Khalid\product\21c\oradata\XE\ISIL\ts_isil2.dbf' size 4 m;

Tablespace created.

-------- temprary file ----------
SQL> create temporary tablespace ts_isil_temp tempfile 'C:\app\Khalid\product\21c\oradata\XE\ISIL\ts_isil.temp' size 2 m, 'C:\app\Khalid\product\21c\oradata\XE\ISIL\ts_isil2.temp' size 4 m;

Tablespace created.

[online/offline]

--------------- smallfile -------------
create smallfile tablespace ts_isil datafile 'path' size 2 m , 'path1' size 3 m ;
alter tablespace ts_isil add datafile 'pathfile' size 2 m;

--------------- bigfile ------------
create bigfile tablespace ts_isil datafile 'path' size 3 m;
alter tablespace ts_isil resize 4 m;

C:\app\Khalid\product\21c\admin\XE\pfile\init.ora.10142022163950

--- Administration des Bases de Données ORACLE ---
------- Atelier N° 1 ----------
4 )

SQL> create spfile='C:\app\Khalid\product\21c\admin\XE\pfile\isil.ora' from pfile='C:\app\Khalid\product\21c\admin\XE\pfile\init.ora.10142022163950';

File created.

SQL> disconnect
Disconnected from Oracle Database 21c Express Edition Release 21.0.0.0.0 - Production
Version 21.3.0.0.0

5 )
SQL> connect sys as sysdba
Enter password:
Connected to an idle instance.
SQL> startup pfile='C:\app\Khalid\product\21c\admin\XE\pfile\init.ora.10142022163950'
ORACLE instance started.

Total System Global Area 1610609432 bytes
Fixed Size 9855768 bytes
Variable Size 402653184 bytes
Database Buffers 1191182336 bytes
Redo Buffers 6918144 bytes
Database mounted.
Database opened.

6 )

SQL> shutdown
Database closed.
Database dismounted.
ORACLE instance shut down.

SQL> startup open read only
ORACLE instance started.

Total System Global Area 1610609432 bytes
Fixed Size 9855768 bytes
Variable Size 989855744 bytes
Database Buffers 603979776 bytes
Redo Buffers 6918144 bytes
Database mounted.
Database opened.

7 )
alter session set "\_Oracle_Script"=True;
create user HR identified by hr;

User created.
grant CREATE SESSION, ALTER SESSION, CREATE DATABASE LINK, -
CREATE MATERIALIZED VIEW, CREATE PROCEDURE, CREATE PUBLIC SYNONYM, -
CREATE ROLE, CREATE SEQUENCE, CREATE SYNONYM, CREATE TABLE, -
CREATE TRIGGER, CREATE TYPE, CREATE VIEW, UNLIMITED TABLESPACE -
to chris;

Grant succeeded.

SQL> grant select on regions to HR;

Grant succeeded.

SQL> grant insert on regions to HR;

Grant succeeded.

8 )
SQL> insert into sys.regions values(99,'marocoo');
insert into sys.regions values(99,'marocoo') \*
ERROR at line 1:
ORA-16000: database or pluggable database open for read-only access

9.

SQL> shutdown
Database closed.
Database dismounted.
ORACLE instance shut down.
SQL> startup open read write
ORACLE instance started.

Total System Global Area 1610609432 bytes
Fixed Size 9855768 bytes
Variable Size 989855744 bytes
Database Buffers 603979776 bytes
Redo Buffers 6918144 bytes
Database mounted.
Database opened.

10 )
SQL> connect HR/hr
Connected.
SQL> insert into sys.regions values(99,'marocoo');

1 row created

11 )

SYS = SQL> shutdown transactional
Database closed.
Database dismounted.
ORACLE instance shut down.

12 )
HR = SQL> Rollback;
Rollback complete.

SQL> disconnect
Session ID: 499 Serial number: 11185

Disconnected from Oracle Database 21c Express Edition Release 21.0.0.0.0 - Production
Version 21.3.0.0.0 (with complications)

13 )

SQL> alter system enable restricted session;

System altered.

n )

SQL> connect HR/hr
ERROR:
ORA-01035: ORACLE only available to users with RESTRICTED SESSION privilege

n+1 )

SQL> alter system disable restricted session;

System altered.

-------------- Gestion des utilisateurs, privilèges et rôles -------------------

1.  create table emp_isil(id_emp number, nom varchar2(50), prenom varchar2(50), salaire number);
    Table created.

2.  SQL> insert into emp_isil values(1,'amri','amjad', 5000);
    1 row created.
    SQL> insert into emp_isil values(2,'boussaroual','khalid', 15000);
    1 row created.
    SQL> insert into emp_isil values(3,'alla','ismail', 7000);
    1 row created.

3.

SQL> alter session set "\_oracle_script"=true;

Session altered.

SQL> create user user_isil identified by isil quota 100 M on USERS default tablespace users password expire;

User created.

select \* from all_users where lower(username) = 'user_isil';

## USERNAME

USER_ID CREATED COM O INH

---

## DEFAULT_COLLATION

IMP ALL EXT

---

USER_ISIL
111 14-FEB-23 YES Y NO
USING_NLS_COMP
NO NO NO

4.  SQL> create role R_isil;
    SQL> select \* from dba_roles where lower(role) = 'r_isil';

## ROLE

ROLE_ID PASSWORD AUTHENTICAT COM O INH IMP

---

## EXTERNAL_NAME

R_ISIL
112 NO NONE YES Y NO NO

5.

SQL> grant create session, create table to R_isil;

Grant succeeded.

SQL> grant select on emp_isil to R_isil;

Grant succeeded.

6.

SQL> grant R_isil to user_isil;

Grant succeeded. 7)

SQL> select GRANTEE from dba_role_privs where lower(GRANTEE) = 'user_isil';

## GRANTEE

USER_ISIL

8.

Enter user-name: user_isil
Enter password:
ERROR:
ORA-28001: the password has expired

Changing password for user_isil
New password:
Retype new password:
Password changed

Connected to:
Oracle Database 21c Express Edition Release 21.0.0.0.0 - Production
Version 21.3.0.0.0

SQL> show user
USER is "USER_ISIL"
SQL>

9.

SQL> select \* from user_tab_privs;

---

|GRANTEE | OWNER | TABLE_NAME | GRANTOR | PRIVILEGE | GRA | HIE | COM | TYPE | INT |

---

|PUBLIC | SYS | USER_ISIL | USER_ISIL | INHERIT PRIVILEGES | NO | NO | NO | USER | NO |

10.

SQL> select \* from user_sys_privs;

no rows selected.

11.

SQL> select \* from session_roles;

## ROLE

R_ISIL

12.

SQL> select \* from sys.emp_isil;

    ID_EMP NOM

---

PRENOM SALAIRE

---

         1 amri

amjad 5000

         2 boussaroual

khalid 15000

         3 alla

ismail 7000

13.

SQL> create table service (id number primary key, libelle varchar2(15));

Table created.

14.

SQL> revoke create table from R_isil;

Revoke succeeded.

15.

SQL> select \*
2 from role_sys_privs
3 where ROLE = 'R_ISIL';

## ROLE

PRIVILEGE ADM COM INH

---

R_ISIL
CREATE SESSION NO YES NO

16.

SQL> create table emp(id_emp number, nom varchar2(50), prenom varchar2(50), salaire number);
create table emp(id_emp number, nom varchar2(50), prenom varchar2(50), salaire number)

- ERROR at line 1:
  ORA-01031: insufficient privileges

17.

SQL> revoke r_isil from user_isil;

Revoke succeeded.

18.

SQL> select \* from session_roles;

## ROLE

R_ISIL

19.

SQL> grant r_isil to public;

Grant succeeded.

20.

SQL> select \* from session_roles;

## ROLE

R_ISIL

21.

SQL> revoke r_isil from user_isil;
revoke r_isil from user_isil

- ERROR at line 1:
  ORA-01951: ROLE 'R_ISIL' not granted to 'USER_ISIL'

22.

SQL> grant update(salaire) on sys.emp_isil to user_isil;

Grant succeeded.

23.

SQL> update sys.emp_isil set salaire = 3000 where id_emp = 1;

1 row updated.

24.

select \* from user_col_privs;

---

|GRANTEE | OWNER | TABLE_NAME | COLUMN_NAME | GRANTOR | PRIVILEGE |

---

|USER_ISIL | SYS | EMP_ISIL | SALAIRE | SYS | UPDATE |

**************\_\_\_************** cerate tablespace **************\_\_\_**************

-------- datafile ---------

SQL> create tablespace ts_isil datafile 'C:\app\Khalid\product\21c\oradata\XE\ISIL\ts_isil.dbt' size 2 m, 'C:\app\Khalid\product\21c\oradata\XE\ISIL\ts_isil2.dbf' size 4 m;

Tablespace created.

-------- temprary file ----------
SQL> create temporary tablespace ts_isil_temp tempfile 'C:\app\Khalid\product\21c\oradata\XE\ISIL\ts_isil.temp' size 2 m, 'C:\app\Khalid\product\21c\oradata\XE\ISIL\ts_isil2.temp' size 4 m;

Tablespace created.

[online/offline]

--------------- smallfile -------------
create smallfile tablespace ts_isil datafile 'path' size 2 m , 'path1' size 3 m ;
alter tablespace ts_isil add datafile 'pathfile' size 2 m;

--------------- bigfile ------------
create bigfile tablespace ts_isil datafile 'path' size 3 m;
alter tablespace ts_isil resize 4 m;

C:\app\Khalid\product\21c\admin\XE\pfile\init.ora.10142022163950

--- Administration des Bases de Données ORACLE ---
------- Atelier N° 1 ----------
4 )

SQL> create spfile='C:\app\Khalid\product\21c\admin\XE\pfile\isil.ora' from pfile='C:\app\Khalid\product\21c\admin\XE\pfile\init.ora.10142022163950';

File created.

SQL> disconnect
Disconnected from Oracle Database 21c Express Edition Release 21.0.0.0.0 - Production
Version 21.3.0.0.0

5 )
SQL> connect sys as sysdba
Enter password:
Connected to an idle instance.
SQL> startup pfile='C:\app\Khalid\product\21c\admin\XE\pfile\init.ora.10142022163950'
ORACLE instance started.

Total System Global Area 1610609432 bytes
Fixed Size 9855768 bytes
Variable Size 402653184 bytes
Database Buffers 1191182336 bytes
Redo Buffers 6918144 bytes
Database mounted.
Database opened.

6 )

SQL> shutdown
Database closed.
Database dismounted.
ORACLE instance shut down.

SQL> startup open read only
ORACLE instance started.

Total System Global Area 1610609432 bytes
Fixed Size 9855768 bytes
Variable Size 989855744 bytes
Database Buffers 603979776 bytes
Redo Buffers 6918144 bytes
Database mounted.
Database opened.

7 )
alter session set "\_Oracle_Script"=True;
create user HR identified by hr;

User created.
grant CREATE SESSION, ALTER SESSION, CREATE DATABASE LINK, -
CREATE MATERIALIZED VIEW, CREATE PROCEDURE, CREATE PUBLIC SYNONYM, -
CREATE ROLE, CREATE SEQUENCE, CREATE SYNONYM, CREATE TABLE, -
CREATE TRIGGER, CREATE TYPE, CREATE VIEW, UNLIMITED TABLESPACE -
to chris;

Grant succeeded.

SQL> grant select on regions to HR;

Grant succeeded.

SQL> grant insert on regions to HR;

Grant succeeded.

8 )
SQL> insert into sys.regions values(99,'marocoo');
insert into sys.regions values(99,'marocoo') \*
ERROR at line 1:
ORA-16000: database or pluggable database open for read-only access

9.

SQL> shutdown
Database closed.
Database dismounted.
ORACLE instance shut down.
SQL> startup open read write
ORACLE instance started.

Total System Global Area 1610609432 bytes
Fixed Size 9855768 bytes
Variable Size 989855744 bytes
Database Buffers 603979776 bytes
Redo Buffers 6918144 bytes
Database mounted.
Database opened.

10 )
SQL> connect HR/hr
Connected.
SQL> insert into sys.regions values(99,'marocoo');

1 row created

11 )

SYS = SQL> shutdown transactional
Database closed.
Database dismounted.
ORACLE instance shut down.

12 )
HR = SQL> Rollback;
Rollback complete.

SQL> disconnect
Session ID: 499 Serial number: 11185

Disconnected from Oracle Database 21c Express Edition Release 21.0.0.0.0 - Production
Version 21.3.0.0.0 (with complications)

13 )

SQL> alter system enable restricted session;

System altered.

n )

SQL> connect HR/hr
ERROR:
ORA-01035: ORACLE only available to users with RESTRICTED SESSION privilege

n+1 )

SQL> alter system disable restricted session;

System altered.

-------------- Gestion des utilisateurs, privilèges et rôles -------------------

1.  create table emp_isil(id_emp number, nom varchar2(50), prenom varchar2(50), salaire number);
    Table created.

2.  SQL> insert into emp_isil values(1,'amri','amjad', 5000);
    1 row created.
    SQL> insert into emp_isil values(2,'boussaroual','khalid', 15000);
    1 row created.
    SQL> insert into emp_isil values(3,'alla','ismail', 7000);
    1 row created.

3.

SQL> alter session set "\_oracle_script"=true;

Session altered.

SQL> create user user_isil identified by isil quota 100 M on USERS default tablespace users password expire;

User created.

select \* from all_users where lower(username) = 'user_isil';

## USERNAME

USER_ID CREATED COM O INH

---

## DEFAULT_COLLATION

IMP ALL EXT

---

USER_ISIL
111 14-FEB-23 YES Y NO
USING_NLS_COMP
NO NO NO

4.  SQL> create role R_isil;
    SQL> select \* from dba_roles where lower(role) = 'r_isil';

## ROLE

ROLE_ID PASSWORD AUTHENTICAT COM O INH IMP

---

## EXTERNAL_NAME

R_ISIL
112 NO NONE YES Y NO NO

5.

SQL> grant create session, create table to R_isil;

Grant succeeded.

SQL> grant select on emp_isil to R_isil;

Grant succeeded.

6.

SQL> grant R_isil to user_isil;

Grant succeeded. 7)

SQL> select GRANTEE from dba_role_privs where lower(GRANTEE) = 'user_isil';

## GRANTEE

USER_ISIL

8.

Enter user-name: user_isil
Enter password:
ERROR:
ORA-28001: the password has expired

Changing password for user_isil
New password:
Retype new password:
Password changed

Connected to:
Oracle Database 21c Express Edition Release 21.0.0.0.0 - Production
Version 21.3.0.0.0

SQL> show user
USER is "USER_ISIL"
SQL>

9.

SQL> select \* from user_tab_privs;

---

|GRANTEE | OWNER | TABLE_NAME | GRANTOR | PRIVILEGE | GRA | HIE | COM | TYPE | INT |

---

|PUBLIC | SYS | USER_ISIL | USER_ISIL | INHERIT PRIVILEGES | NO | NO | NO | USER | NO |

10.

SQL> select \* from user_sys_privs;

no rows selected.

11.

SQL> select \* from session_roles;

## ROLE

R_ISIL

12.

SQL> select \* from sys.emp_isil;

    ID_EMP NOM

---

PRENOM SALAIRE

---

         1 amri

amjad 5000

         2 boussaroual

khalid 15000

         3 alla

ismail 7000

13.

SQL> create table service (id number primary key, libelle varchar2(15));

Table created.

14.

SQL> revoke create table from R_isil;

Revoke succeeded.

15.

SQL> select \*
2 from role_sys_privs
3 where ROLE = 'R_ISIL';

## ROLE

PRIVILEGE ADM COM INH

---

R_ISIL
CREATE SESSION NO YES NO

16.

SQL> create table emp(id_emp number, nom varchar2(50), prenom varchar2(50), salaire number);
create table emp(id_emp number, nom varchar2(50), prenom varchar2(50), salaire number)

- ERROR at line 1:
  ORA-01031: insufficient privileges

17.

SQL> revoke r_isil from user_isil;

Revoke succeeded.

18.

SQL> select \* from session_roles;

## ROLE

R_ISIL

19.

SQL> grant r_isil to public;

Grant succeeded.

20.

SQL> select \* from session_roles;

## ROLE

R_ISIL

21.

SQL> revoke r_isil from user_isil;
revoke r_isil from user_isil

- ERROR at line 1:
  ORA-01951: ROLE 'R_ISIL' not granted to 'USER_ISIL'

22.

SQL> grant update(salaire) on sys.emp_isil to user_isil;

Grant succeeded.

23.

SQL> update sys.emp_isil set salaire = 3000 where id_emp = 1;

1 row updated.

24.

select \* from user_col_privs;

---

|GRANTEE | OWNER | TABLE_NAME | COLUMN_NAME | GRANTOR | PRIVILEGE |

---

|USER_ISIL | SYS | EMP_ISIL | SALAIRE | SYS | UPDATE |

**************\_\_\_************** cerate tablespace **************\_\_\_**************

-------- datafile ---------

SQL> create tablespace ts_isil datafile 'C:\app\Khalid\product\21c\oradata\XE\ISIL\ts_isil.dbt' size 2 m, 'C:\app\Khalid\product\21c\oradata\XE\ISIL\ts_isil2.dbf' size 4 m;

Tablespace created.

-------- temprary file ----------
SQL> create temporary tablespace ts_isil_temp tempfile 'C:\app\Khalid\product\21c\oradata\XE\ISIL\ts_isil.temp' size 2 m, 'C:\app\Khalid\product\21c\oradata\XE\ISIL\ts_isil2.temp' size 4 m;

Tablespace created.

[online/offline]

--------------- smallfile -------------
create smallfile tablespace ts_isil datafile 'path' size 2 m , 'path1' size 3 m ;
alter tablespace ts_isil add datafile 'pathfile' size 2 m;

--------------- bigfile ------------
create bigfile tablespace ts_isil datafile 'path' size 3 m;
alter tablespace ts_isil resize 4 m;
