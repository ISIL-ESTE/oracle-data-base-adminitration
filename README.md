Explore this GitHub repository that documents the administration of an Oracle Database 21c Express Edition. The provided script (init.ora.10142022163950) is utilized for tasks such as creating a server parameter file (spfile), connecting to the database, managing user privileges, creating tables and tablespaces, and demonstrating SQL operations. The script covers actions like starting and shutting down the database, managing sessions, creating users and roles, granting privileges, and handling errors. Additionally, it includes examples of creating tablespaces with both smallfile and bigfile configurations. This comprehensive repository serves as a practical guide for Oracle Database administration tasks, making it valuable for individuals learning and working with Oracle databases.



# Oracle Database Administration Script

## Initialization File: init.ora.10142022163950

### 1. Create SPFILE
```sql
SQL> create spfile='C:\app\Khalid\product\21c\admin\XE\pfile\isil.ora' from pfile='C:\app\Khalid\product\21c\admin\XE\pfile\init.ora.10142022163950';
File created.
```
## 2. Connect and Start Database

```sql
SQL> connect sys as sysdba
Enter password: 
Connected to an idle instance.

SQL> startup pfile='C:\app\Khalid\product\21c\admin\XE\pfile\init.ora.10142022163950'
ORACLE instance started.
Database mounted. Database opened.
```

## 3. Shutdown and Restart in Read-Only

```sql
SQL> shutdown
Database closed. Database dismounted. ORACLE instance shut down.

SQL> startup open read only
ORACLE instance started. Database mounted. Database opened.
```

## 4. User and Privilege Management

```sql
SQL> alter session set "_Oracle_Script"=True;
create user HR identified by hr;
grant CREATE SESSION, ALTER SESSION, CREATE DATABASE LINK,
      CREATE MATERIALIZED VIEW, CREATE PROCEDURE, CREATE PUBLIC SYNONYM,
      CREATE ROLE, CREATE SEQUENCE, CREATE SYNONYM, CREATE TABLE,
      CREATE TRIGGER, CREATE TYPE, CREATE VIEW, UNLIMITED TABLESPACE to chris;
```

## 5. Table Operations and Error Handling

```sql
SQL> insert into sys.regions values(99,'marocoo');
-- ERROR: ORA-16000: database or pluggable database open for read-only access

SQL> shutdown
Database closed. Database dismounted. ORACLE instance shut down.

SQL> startup open read write
ORACLE instance started. Database mounted. Database opened.
```

## 6. Rollback and Session Management

```sql
SQL> connect HR/hr
Connected.

SQL> insert into sys.regions values(99,'marocoo');
1 row created.

SQL> shutdown transactional
Database closed. Database dismounted. ORACLE instance shut down.

SQL> Rollback;
Rollback complete.

SQL> disconnect
Disconnected from Oracle Database 21c Express Edition Release 21.0.0.0.0 - Production Version 21.3.0.0.0 (with complications)
```

## 7. Session Alteration and Privilege Management

```sql
SQL> alter system enable restricted session;

SQL> connect HR/hr
-- ERROR: ORA-01035: ORACLE only available to users with RESTRICTED SESSION privilege

SQL> alter system disable restricted session;
```

## 8. User and Role Creation with Granting Privileges

```sql
-- User and Role Creation
create user user_isil identified by isil quota 100M on USERS default tablespace users password expire;
create role R_isil;

-- Grant Privileges
grant create session, create table to R_isil;
grant select on emp_isil to R_isil;
grant R_isil to user_isil;
```

## 9. Password Change and Role Usage

```sql
SQL> select * from all_users where lower(username) = 'user_isil';

SQL> select * from dba_roles where lower(role) = 'r_isil';

-- Changing Password and Connecting
Enter user-name: user_isil
Enter password: 
Password changed.

Connected to: Oracle Database 21c Express Edition Release 21.0.0.0.0 - Production Version 21.3.0.0.0
```

## 10. Table Creation, Update, and Privilege Management

```sql
create table emp_isil(id_emp number, nom varchar2(50), prenom varchar2(50), salaire number);

insert into emp_isil values(1,'amri','amjad', 5000);
insert into emp_isil values(2,'boussaroual','khalid', 15000);
insert into emp_isil values(3,'alla','ismail', 7000);

grant update(salaire) on sys.emp_isil to user_isil;

update sys.emp_isil set salaire = 3000 where id_emp = 1;
```

## 11. Tablespaces Management

```sql
-- Create Tablespace
create tablespace ts_isil datafile 'C:\app\Khalid\product\21c\oradata\XE\ISIL\ts_isil.dbt' size 2M, 'C:\app\Khalid\product\21c\oradata\XE\ISIL\ts_isil2.dbf' size 4M;

-- Create Temporary Tablespace
create temporary tablespace ts_isil_temp tempfile 'C:\app\Khalid\product\21c\oradata\XE\ISIL\ts_isil.temp' size 2M, 'C:\app\Khalid\product\21c\oradata\XE\ISIL\ts_isil2.temp' size 4M;
```

## 12. Additional Tablespace Operations

```sql
-- Smallfile Tablespaces
create smallfile tablespace ts_isil datafile 'path' size 2M , 'path1' size 3M;
alter tablespace ts_isil add datafile 'pathfile' size 2M;

-- Bigfile Tablespace
create bigfile tablespace ts_isil datafile 'path' size 3M;
alter tablespace ts_isil resize 4M;
