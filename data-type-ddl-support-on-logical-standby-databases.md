## CData Type and DDL Support on a Logical Standby Database {#GUID-340BA6D6-74D5-4684-AF39-08890D5CD9D4}

The script content on this page is for navigation purposes only and does not alter the content in any way.

When setting up a logical standby database, you must ensure the logical standby database can maintain the datatypes and tables in your primary database.

> **note:** 

A multitenant container database is the only supported architecture in Oracle Database 21c and later releases. While the documentation is being revised, legacy terminology may persist. In most cases, "database" and "non-CDB" refer to a CDB or PDB, depending on context. In some contexts, such as upgrades, "non-CDB" refers to a non-CDB from a previous release.

The following topics describe the various database objects, storage types, and PL/SQL supplied packages that are supported and unsupported by logical standby databases:

* [Datatype Considerations](data-type-ddl-support-on-logical-standby-databases.md#GUID-54DD0323-EC06-46E7-A2EC-3078C653D142)

* [Support for Data Types That Lack Native Redo-Based Support](data-type-ddl-support-on-logical-standby-databases.md#GUID-0E3F7414-E338-4710-97C8-B2E99740015A)

* [Support for Transparent Data Encryption (TDE)](data-type-ddl-support-on-logical-standby-databases.md#GUID-CE6B667C-46EE-48C5-94AA-C96B98EB4504)

* [Support for Tablespace Encryption](data-type-ddl-support-on-logical-standby-databases.md#GUID-1D2E27FB-2978-4250-97A1-F65EBE984871)

* [Support For Row-level Security and Fine-Grained Auditing](data-type-ddl-support-on-logical-standby-databases.md#GUID-6E584765-ED9A-4FF3-BED4-2AE5043877B5)

* [Oracle Label Security](data-type-ddl-support-on-logical-standby-databases.md#GUID-65FEB2C3-FA7B-4E33-A099-2830772F88A6)

* [Oracle E-Business Suite](data-type-ddl-support-on-logical-standby-databases.md#GUID-6910C4C9-3CFF-444E-BC98-E239B63979F5)

* [Supported Table Storage Types](data-type-ddl-support-on-logical-standby-databases.md#GUID-BD53979B-3120-45CD-803F-2E6675A9DAFD)

* [Unsupported Table Storage Types](data-type-ddl-support-on-logical-standby-databases.md#GUID-55806A96-6216-41A2-AB52-20704DAAF5C7)

* [PL/SQL Supplied Packages Considerations](data-type-ddl-support-on-logical-standby-databases.md#GUID-7CE601D9-F67A-40F6-9226-6A6B90DE6CE5)

* [Unsupported Tables](data-type-ddl-support-on-logical-standby-databases.md#GUID-291FD74A-165A-4E80-9E1F-66996F1FC6CA)

* [Skipped SQL Statements on a Logical Standby Database](data-type-ddl-support-on-logical-standby-databases.md#GUID-9DBC35BD-43C9-469F-BADD-C91752E88225)

* [DDL Statements Supported by a Logical Standby Database](data-type-ddl-support-on-logical-standby-databases.md#GUID-1595A660-6790-436A-BB84-D74C9187D8AE)

* [Distributed Transactions and XA Support](data-type-ddl-support-on-logical-standby-databases.md#GUID-8BF91B38-CC5C-40A9-B062-683B4A6F5AB8)

* [Support for SecureFiles LOBs](data-type-ddl-support-on-logical-standby-databases.md#GUID-F10F2D96-B441-4465-810D-1A06112552C8)

* [Support for Database File System (DBFS)](data-type-ddl-support-on-logical-standby-databases.md#GUID-0309A2A5-E4DE-406C-9DD3-74DD86F419BB)

* [Character Set Considerations](data-type-ddl-support-on-logical-standby-databases.md#GUID-CC85C064-6115-423C-BF30-790B3D17FC43)

* [Additional PL/SQL Package Support Available Only in the Context of DBMS_ROLLING Upgrades](data-type-ddl-support-on-logical-standby-databases.md#GUID-F54904C7-F666-4CC3-B1CB-493525476D2F)




### Datatype Considerations {#GUID-54DD0323-EC06-46E7-A2EC-3078C653D142}

See these topics for information about supported and unsupported database objects.

* [Supported Datatypes in a Logical Standby Database](data-type-ddl-support-on-logical-standby-databases.md#GUID-8FED6166-FB83-42E4-8D2E-8A1F7DFCEB00)

* [Unsupported Datatypes in a Logical Standby Database](data-type-ddl-support-on-logical-standby-databases.md#GUID-76E70ADB-0709-4403-BA6A-379CEAFE758A)




#### Supported Datatypes in a Logical Standby Database {#GUID-8FED6166-FB83-42E4-8D2E-8A1F7DFCEB00}

These are the datatypes that logical standby databases support.

Logical standby databases support the following datatypes:

* Abstract Data Types (ADTs) and ADT tables

* ADTs cannot contain any data types that are not supported as a top-level column type (for example, nested tables, PKREFs, BFILE, unsupported opaque types).

* For a table with ADT columns to be supported there must be a primary key (or at least a unique constraint or unique index) that consists solely of scalar top-level columns (scalar ADT attributes cannot be part of such a candidate key).

* `BINARY_DOUBLE`

* `BINARY_FLOAT`

* `BLOB`, `CLOB`, and `NCLOB` stored as BasicFile and SecureFiles. SecureFiles can be compressed, encrypted, or deduplicated. See [Support for SecureFiles LOBs](data-type-ddl-support-on-logical-standby-databases.md#GUID-F10F2D96-B441-4465-810D-1A06112552C8)

* `CHAR`

* `DATE`

* `INTERVAL YEAR TO MONTH`

* `INTERVAL DAY TO SECOND`

* `LONG`

* `LONG RAW`

* `NCHAR`

* `NUMBER`

* `NVARCHAR2`

* Objects stored as `VARRAY`s (except for Collections) 

* Oracle Text

* `RAW`

* Multimedia (See exceptions listed in [Unsupported Datatypes in a Logical Standby Database](data-type-ddl-support-on-logical-standby-databases.md#GUID-76E70ADB-0709-4403-BA6A-379CEAFE758A).): 

* `ORDAudio`

* `ORDDataSource` (internal) 

* `ORDDicom`

* `ORDDoc`

* `ORDImage`

* `ORDSource` (internal) 

* `ORDVideo`

* Spatial (See exceptions listed in [Unsupported Datatypes in a Logical Standby Database](data-type-ddl-support-on-logical-standby-databases.md#GUID-76E70ADB-0709-4403-BA6A-379CEAFE758A).) 

* `TIMESTAMP`

* `TIMESTAMP WITH TIMEZONE`

* `TIMESTAMP WITH LOCAL TIMEZONE`

* `VARCHAR` and `VARCHAR2`

* `XMLType` data for all storage models, assuming the following primary database compatibility requirements: 

* `XMLType` stored as `CLOB` is deprecated as of Oracle Database 12*c* Release 1 (12.1). 




> **note:** 

SQL Apply does not support statements that have function calls that perform DML on ADT, `LOB`, or `XMLType` columns. 

> **note:** 

As of Oracle Database 12*c* Release 1 (12.1), the maximum size of the `VARCHAR2`, `NVARCHAR2`, and `RAW` datatypes has been increased to 32 KB when the `COMPATIBLE` initialization parameter is set to 12.0 or later and the `MAX_STRING_SIZE` initialization parameter is set to `EXTENDED`. Logical standby databases support this increased size in most cases. See "[Ensure Table Rows in the Primary Database Can Be Uniquely Identified](creating-oracle-data-guard-logical-standby.md#GUID-C238335E-8383-43F1-B5B1-709A2A619C30)" for known restrictions. 

##### Compatibility Requirements {#GUID-58CF57D0-3681-4108-B498-07CA4F99C57E}

SQL Apply support for theses features has compatibility requirements on the primary database:

* Multibyte `CLOB` support requires primary database to run at a compatibility of 10.1 or higher. 

* IOT support without `LOB`s and Overflows requires primary database to run at a compatibility of 10.1 or higher. 

* IOT support with `LOB` and Overflow requires primary database to run at a compatibility of 10.2 or higher. 

* TDE support requires primary database to run at a compatibility of 11.1 or higher.

* Basic compression and advanced row compression require the primary database to run at a compatibility of 11.1 or higher.

* Hybrid Columnar Compression support is dependent on the underlying storage system.




> **note:** See Also: 

* [*Oracle Database Concepts*](https://docs.oracle.com/pls/topic/lookup?ctx=en/database/oracle/oracle-database/23/sbydb&id=CNCPT89198) for more information about Hybrid Columnar Compression 




##### Opaque Type Restrictions {#GUID-55998F99-9598-4AD1-84FC-17889E82C717}

These are the restrictions regarding opaque types.

* `SYS.ANYDATA` is supported as long as the instance does not store user-defined opaque data types or `BFILE`s. 

* `SYS.ANYDATASET`, `SYS.ANYTYPE`, and user-defined opaque types are not supported. 




#### Unsupported Datatypes in a Logical Standby Database {#GUID-76E70ADB-0709-4403-BA6A-379CEAFE758A}

Some data types are not supported by logical standby databases. 

If a table contains columns having any of the following unsupported data types, then the entire table is ignored by SQL Apply. (See [Support for Data Types That Lack Native Redo-Based Support](data-type-ddl-support-on-logical-standby-databases.md#GUID-0E3F7414-E338-4710-97C8-B2E99740015A) for information about support for data types that lack native redo-based support.) 

* `ROWID`, `UROWID`<br>Note that only logical `UROWID` columns are supported. Non-logical `UROWID` are detected at run-time, the DML is skipped, and a trace message is written to the report file. 

* Nested tables

* Objects with nested tables

* Identity columns




### Support for Data Types That Lack Native Redo-Based Support {#GUID-0E3F7414-E338-4710-97C8-B2E99740015A}

The Extended Datatype Support (EDS) feature provides a mechanism for logical standbys to support certain data types that lack native redo-based support.

> **note:** As of Oracle Database 18c, Extended Datatype Support (EDS) is deprecated. All EDS-supported Oracle data types are now supported natively by logical standbys or Oracle GoldenGate. 

For example, tables with `SDO_GEOMETRY` columns can be replicated using EDS. (Source tables must have a primary key.) 

You can query the `DBA_LOGSTDBY_EDS_SUPPORTED` view to find out which tables are candidates for EDS. 

### Support for Transparent Data Encryption (TDE) {#GUID-CE6B667C-46EE-48C5-94AA-C96B98EB4504}

Oracle Data Guard SQL Apply can be used to provide data protection for a primary database with Transparent Data Encryption (TDE) enabled.

Consider the following when using a logical standby database to provide data protection for applications with advanced security requirements:

* Tables with Transparent Data Encryption using server held keys are replicated on a logical standby database.

* Transparent Data Encryption in the context of Hardware Security Modules is supported for logical standby databases.




You must consider the following restrictions when, in the context of a logical standby database, you want to replicate tables that have encrypted columns:

1. To translate encrypted redo records, SQL Apply must have access to an open wallet containing the Transparent Data Encryption keys. Therefore, you must copy the wallet containing the keys from the primary database to the standby database after it has been created.

2. The wallet must be copied from the primary database to the logical standby database every time the master key is changed. 

3. Oracle recommends that you not rekey the master key at the logical standby database while the logical standby database is replicating encrypted tables from the primary database. Doing so may cause SQL Apply to halt when it encounters an encrypted redo record.

4. You can rekey the encryption key of a replicated table at the logical standby database. This requires that you lower the guard setting to `NONE` before you issue the rekey command. 

5. Replicated encrypted tables can use a different encryption scheme for columns than the one used in the primary database. For example, if the `SALARY` column of the `HR.EMPLOYEES` table is encrypted at the primary database using the AES192 encryption algorithm, it can be encrypted at the logical standby using the AES256 encryption algorithm. Or, the `SALARY` column can remain unencrypted at the logical standby database. 




> **note:** See Also: 

* [*Zero Data Loss Recovery Appliance Administrator's Guide*](https://docs.oracle.com/pls/topic/lookup?ctx=en/database/oracle/oracle-database/23/sbydb&id=AMAGD-GUID-328CB500-2AD3-4F42-A2C6-5657C0ABC568) for more information about redo encryption using the `LOG_ARCHIVE_DEST_*`n`*` parameter 

* [*Oracle Database Advanced Security Guide*](https://docs.oracle.com/pls/topic/lookup?ctx=en/database/oracle/oracle-database/23/sbydb&id=ASOAG-GUID-62AA9447-FDCD-4A4C-B563-32DE04D55952) for more information about transparent data encryption 




### Hybrid Data Guard Configurations and Transparent Data Encryption (TDE) {#GUID-773790D5-B6F2-439B-8B85-D9A33D082C19}

One of Oracle's most important security features to protect data-at-rest is Transparent Data Encryption (TDE) with encrypted tablespaces. Implementation of TDE requires an Advanced Security Option (ASO) license, but Oracle Cloud Services make TDE available for free without ASO licenses. In fact encryption for all customer data hosted in Oracle Cloud Services is required.

Possible hybrid configurations for Oracle Cloud Services with TDE and Data Guard include:

* Migrating the primary database into the Cloud and keeping the standby database on-premise.
* Maintaing the primary databases on-premise, while moving the standby into Oracle Cloud Services.



A primary database on premise with no TDE encrypted data and a standby with everything encrypted by TDE in the Cloud, or a primary database in the Cloud with everything encrypted with TDE but nothing encrypted on the on-premise standby have both technical and licensing issues.

Starting with Oracle Database 23ai, it is now possible to decrypt redo in hybrid cloud Data Guard configurations where the cloud database is encrypted with TDE and the on-premises database is not. A new parameter, `TABLESPACE_ENCRYPTION`, accepts three values that give the control over the encryption and decryption of data on the on-premises databases. 

* `AUTO_ENABLE`: Always encrypt the data coming from the redo stream (whether it comes encrypted or not). This is the only accepted value for Cloud deployments. It is also the right one for sites having ASO license and willing to encrypt everything on-premises. 
* `DECRYPT_ONLY`: Decrypt the encrypted data coming from an encrypted redo stream and always store it unencrypted. This is what sites without ASO must set. 
* `MANUAL_ENABLE`: Fallback to previous behavior for the DBA wishing to decide which tablespaces must be encrypted. 



To setup a hybrid Cloud/On-premise Data Guard configuration, follow the steps below.

1. Set up the TDE wallet as usual, both on-premise and in the Cloud.
2. On the Cloud database set the `TABLESPACE_ENCRYPTION` parameter to `AUTO_ENABLE`:
```
SQL> alter system set TABLESPACE_ENCRYPTION  = AUTO_ENABLE;
```


3. On the on-premise database set the `TABLESPACE_ENCRYPTION` parameter to `DECRYPT_ONLY`:
```
SQL> alter system set TABLESPACE_ENCRYPTION  = DECRYPT_ONLY;
```


4. Continue Data Guard configuration as usual.



> **note:** The `TABLESPACE_ENCRYPTION` parameter is always set at the CDB level. 

### Support for Tablespace Encryption {#GUID-1D2E27FB-2978-4250-97A1-F65EBE984871}

Oracle Data Guard SQL Apply can be used to provide data protection for a primary database that has tablespace encryption enabled.

In such a case, restrictions 1, 2, and 3 listed in [Support for Transparent Data Encryption (TDE)](data-type-ddl-support-on-logical-standby-databases.md#GUID-CE6B667C-46EE-48C5-94AA-C96B98EB4504) apply. 

Encryption, re-keying, or decryption of a tablespace on a primary does not trigger the need for the same operation on a logical standby. However, a logical standby must have the capability of re-keying as well. 

> **note:** 

In some cases, when SQL Apply mines and applies redo records for changes made to tables in encrypted tablespaces, records of user data in unencrypted form may be kept for a long period of time. If this is not acceptable, then issue the following command to move all metadata tables pertaining to the mining component of SQL Apply to an encrypted tablespace:
```
SQL> DBMS_LOGMNR_D.SET_TABLESPACE(NEW_TABLESPACE => 'ENCRYPTED_LOGMNR_TS');
```


In previous releases, tablespaces in Data Guard environments were required to use the same encryption policy on the primary and standby databases. For example, if all tablespaces were encrypted on the primary database, then all tablespaces were required to be encrypted on the standby database. The `TABLESPACE_ENCRYPTION` parameter introduced in Oracle Database release 19c, version 19.16, eliminates this requirement and allows you to use different tablespace encryption policies on primary and standby databases. This new parameter is meant to replace the `ENCRYPT_NEW_TABLESPACES` parameter. For more information regarding the useage of the `TABLESPACE_ENCRYPTION` parameter, see the [Oracle Database Reference](https://docs.oracle.com/pls/topic/lookup?ctx=en/database/oracle/oracle-database/23/sbydb&id=REFRN-GUID-42408BFB-1A3F-4BAA-B0E9-1AA0CD48FA5A). 

### Support For Row-level Security and Fine-Grained Auditing {#GUID-6E584765-ED9A-4FF3-BED4-2AE5043877B5}

Logical standby can automatically replicate the security environment provided through the `DBMS_RLS` and `DBMS_FGA` PL/SQL packages. 

This support simplifies management of security considerations when a server fails over to the standby since the security environment is transparently maintained. It also ensures that access control policies applied to the primary data can be automatically forwarded to the standby, and the standby data transparently given the same level of protection.

Support for the replication of these PL/SQL packages requires that the table referenced be a Logical Standby maintained object. For example, a table with a rowid column does not have its data maintained by Logical Standby, so `DBMS_RLS` and `DBMS_FGA` calls referencing that table are not maintained. 

#### Row-level Security {#GUID-5768DF75-A955-4FAB-B928-C35F337330DC}

Row-Level Security, also known as Virtual Private Database (VPD), is a feature that enforces security at a fine level of granularity, when accessing tables, views, or synonyms.

When a user directly or indirectly accesses a table, view, or synonym protected with a VPD policy, the server dynamically modifies the SQL statement of the user. The modification creates a `WHERE` condition (known as a predicate) returned by a function implementing the security policy. The statement is modified dynamically, transparently to the user, using any condition that can be expressed in, or returned by, a function. VPD policies can be applied to `SELECT`, `INSERT`, `UPDATE`, `INDEX`, and `DELETE` statements. VPD is implemented by using the `DBMS_RLS` package to apply security policies. 

When a `DBMS_RLS` procedure is executed on the primary, additional information is captured in the redo that allows the procedure call to be logically reconstructed and executed on the standby. Logical Standby supports replication of ancillary objects for VPD such as Contexts, Database Logon Triggers, and their supporting packages. You must ensure that these objects are placed in maintained schemas and that no DDL skips have been configured that would stop their replication. 

#### Fine-Grained Auditing {#GUID-3493175A-5B62-412F-BF6E-C41D0BF6D498}

Fine-grained auditing provides a way to audit select statements.

The `DBMS_FGA` package enables all select statements that access a table to be captured, together with what data was accessed. An FGA policy may be applied to a particular column or even to only those select statements that return rows for which a specified predicate returns `TRUE`. 

When a `DBMS_FGA` procedure is executed on the primary, additional information is captured to the redo that allows the procedure call to be logically reconstructed and executed on the standby. 

#### Skipping and Enabling PL/SQL Replication {#GUID-5905EAC6-7FED-440B-AB43-82DA3A90FF1C}

PL/SQL can be configured with skip and skip_error rules exactly as DDL statements except that the use of wildcards on the package and procedure are not supported.

For example to skip all aspects of VPD, do the following:
```
DBMS_LOGSTDBY.Skip (
stmt => 'PL/SQL',
schema_name => 'SYS',
object_name =>'DBMS_RLS',
use_like => FALSE);
```


The schema specified is the schema in which the package is defined. To skip an individual procedure in a package, the syntax is as follows:
```
DBMS_LOGSTDBY.Skip (
stmt => 'PL/SQL',
schema_name => 'SYS',
object_name =>'DBMS_RLS.ADD_POLICY',
use_like => FALSE);
```


To skip VPD on certain schemas or tables, a skip procedure must be used. The skip procedure is passed the fully qualified PL/SQL statement that is to be executed, for example:
```
DBMS_RLS.DROP_POLICY(
object_schema => 'SCOTT,
object_name  => 'EMP',
policy_name => 'MYPOLICY');
```


The procedure could then parse the statement to decide whether to skip it, to apply it, or to stop apply and let the DBA take a compensating action.

Unlike DDL, skip procedures on PL/SQL do not support returning a replacement statement.

### Oracle Label Security {#GUID-65FEB2C3-FA7B-4E33-A099-2830772F88A6}

As of Oracle Database 12*c* Release 2 (12.2), you can upgrade databases that use Oracle Label Security (OLS) to new Oracle Database releases and patch sets using Oracle Data Guard database rolling upgrades with a transient logical standby database and the PL/SQL package, `DBMS_ROLLING`. 

### Oracle Database Vault {#GUID-DED77868-6048-42F8-BE96-41C8D2EF4057}

Oracle Data Guard rolling upgrades support databases that use Oracle Database Vault.

As of Oracle Database 12*c* Release 2 (12.2.0.1), you can upgrade databases that use Oracle Database Vault to new Oracle Database releases and patch sets by using Oracle Data Guard database rolling upgrades with a transient logical standby and the PL/SQL package, `DBMS_ROLLING`. 

### Oracle Embedded Database Firewall {#GUID-42D25040-E25D-4557-ADB7-FAE25B4C59FB}

Oracle Embedded Database Firewall support for Data Guard.

With the introduction of Oracle Database 23ai, database firewall functionalities, such as allow-listing, deny-listing, and object/command based access control, are implemented inside the Oracle database kernel so that Oracle DBAs can utilize these functionalities and achieve the goal of SQL injection detection and prevention. This new firewall implementation in the database kernel is called Oracle Embedded Database Firewall (EDBFW)

EDBFW will support DG physical standby by replicating the EDBFW state change and any policy metadata change from the primary. When a standby becomes the primary due to role transition, the deployed EDBFW policy will continuously be in effect in the new primary. Additionally, the deployed EDBFW policy will still be enforced in standbys; however, no log will be generated. The EDBFW logs generated in the primary is not be replicated to its standbys.

Currently, EDBFW does not fully support Data Guard logical standby. However, rolling upgrade is fully supported in that all the policy metadata in the standby will be synced with the primary via procedural replications.

### Oracle E-Business Suite {#GUID-6910C4C9-3CFF-444E-BC98-E239B63979F5}

Logical standby databases do not fully support an Oracle E-Business Suite implementation because there are tables that contain unsupported data types.

However, using `SKIP` rules, it is possible for you to replicate a subset of the E-Business Suite schemas and tables to offload applications to the logical standby. 

> **note:** See Also: 

The My Oracle Support note 851603.1 at [`http://support.oracle.com`](http://support.oracle.com) for additional information about using Logical standby with Oracle E-Business Suite 

### Supported Table Storage Types {#GUID-BD53979B-3120-45CD-803F-2E6675A9DAFD}

Logical standby databases support several table storage types.

* Cluster tables (including index clusters and heap clusters).

* Index-organized tables (partitioned and nonpartitioned, including overflow segments).

* Heap-organized tables (partitioned and nonpartitioned).

* Advanced row compression and basic table compression. Both of these options require that the compatibility setting of the primary database be set to 11.1.0 or higher.

* Tables containing LOB columns stored as SecureFiles, when compatibility is set to 11.2 or higher.

* Tables using Hybrid Columnar Compression, when compatibility is set to 11.2.0.2 or higher.

> **note:** See Also: 
* [*Oracle Database Concepts*](https://docs.oracle.com/pls/topic/lookup?ctx=en/database/oracle/oracle-database/23/sbydb&id=CNCPT89198) for more information about Hybrid Columnar Compression 

* Tables with virtual columns (provided the table has no other columns or properties not supported by logical standby)

* If there is no primary key and no non-null unique constraint or index, then all columns with a declared maximum length of 4000 bytes are logged as part of the `UPDATE` statement to help identify the modified row. For the purpose of row identification, logical standby requires that a table have at least one visible (not virtual) column of one of the following datatypes: 

* `CHAR`

* `VARCHAR`

* `VARCHAR2` (with a declared column length <= 4000 bytes) 

* `NVARCHAR`

* `NVARCHAR2` (with a declared column length <= 4000 bytes) 

* `NUMBER`

* `DATE`

* `RAW`

* `BINARY FLOAT`

* `BINARY DOUBLE`

* `TIMESTAMP`

* `TIMESTAMP WITH TIME ZONE`

* `TIMESTAMP WITH LOCAL TIME ZONE`

* `INTERVAL YEAR TO MONTH`

* `INTERVAL DAY TO SECOND`

> **note:** See Also: 
* [Ensure Table Rows in the Primary Database Can Be Uniquely Identified](creating-oracle-data-guard-logical-standby.md#GUID-C238335E-8383-43F1-B5B1-709A2A619C30)




### Unsupported Table Storage Types {#GUID-55806A96-6216-41A2-AB52-20704DAAF5C7}

If a table contains *only* these datatypes, then logical standby does not support it. 

* `LOB` (`CLOB`, `NCLOB`, `BLOB`) 

* `LONG`

* `LONG` `RAW`

* `OBJECT` `TYPE`

* `COLLECTIONS`

* `XML`

* `VARCHAR2` (with a declared column length > 4000 bytes) 

* `NVARCHAR2` (with a declared column length > 4000 bytes) 

* `RAW` (with a declared column length > 4000 bytes) 




> **note:** See Also: 

* [Support for SecureFiles LOBs](data-type-ddl-support-on-logical-standby-databases.md#GUID-F10F2D96-B441-4465-810D-1A06112552C8)

* [Ensure Table Rows in the Primary Database Can Be Uniquely Identified](creating-oracle-data-guard-logical-standby.md#GUID-C238335E-8383-43F1-B5B1-709A2A619C30)




#### Unsupported Tables as a Result of Partitioning {#GUID-1750C1DB-B3CC-4CE8-8860-FD3BC50E4299}

Logical standby does not support tables that use system partitioning or reference partitioning.

When possible, the `ATTRIBUTES` column of the `DBA_LOGSTDBY_UNSUPPORTED` view displays the reason why a table is not supported by SQL Apply. The `ATTRIBUTES` column may be NULL if the table structure itself is not supported by SQL Apply (for example, the table is system-partitioned). 

### PL/SQL Supplied Packages Considerations {#GUID-7CE601D9-F67A-40F6-9226-6A6B90DE6CE5}

Keep these considerations in mind regarding supported and unsupported PL/SQL supplied packages.

* [Supported PL/SQL Supplied Packages](data-type-ddl-support-on-logical-standby-databases.md#GUID-E89A196F-2C5D-45F9-94BA-7793FBF44440)

* [Unsupported PL/SQL Supplied Packages](data-type-ddl-support-on-logical-standby-databases.md#GUID-703C8569-4298-48ED-98F5-6146A6D46F3F)

* [Handling XML and XDB PL/SQL Packages in Logical Standby](data-type-ddl-support-on-logical-standby-databases.md#GUID-F413799B-DB5F-4C57-A3E2-E1C3F20E3FF4)




> **note:** See Also: 

[*Oracle Database PL/SQL Packages and Types Reference*](https://docs.oracle.com/pls/topic/lookup?ctx=en/database/oracle/oracle-database/23/sbydb&id=ARPLS001) for more information about Oracle PL/SQL supplied packages 

#### Supported PL/SQL Supplied Packages {#GUID-E89A196F-2C5D-45F9-94BA-7793FBF44440}

Oracle PL/SQL supplied packages that do not modify system metadata or user data leave no footprint in the archived redo log files, and hence are safe to use on the primary database.

Examples of such packages are `DBMS_OUTPUT`, `DBMS_RANDOM`, `DBMS_PIPE`, `DBMS_DESCRIBE`, `DBMS_TRACE`, `DBMS_METADATA```, `DBMS_CRYPTO`. 

Oracle PL/SQL supplied packages that do not modify system metadata but may modify user data are supported by SQL Apply, as long as the modified data belongs to the supported data types listed in [Supported Datatypes in a Logical Standby Database](data-type-ddl-support-on-logical-standby-databases.md#GUID-8FED6166-FB83-42E4-8D2E-8A1F7DFCEB00). Examples of such packages are `DBMS_LOB`, `DBMS_SQL`, and `DBMS_TRANSACTION`. 

Oracle Data Guard logical standby supports replication of actions performed through the following packages: `DBMS_DDL`, `DBMS_FGA`, `SDO_META`, `DBMS_REDACT`, `DBMS_REDEFINITION`, `DBMS_RLS`, `DBMS_SQL_TRANSLATOR`, `DBMS_XDS`, `DBMS_XMLINDEX` and `DBMS_XMLSCHEMA`. 

To identify which packages are supported in logical standby, you can query the `DBA_LOGSTDBY_PLSQL_SUPPORT` view. For example, you can run the following query to find out which packages are supported in a generic logical standby: 
```
SQL> SELECT OWNER, PKG_NAME FROM DBA_LOGSTDBY_PLSQL_SUPPORT -
> where support_level = 'ALWAYS';
```


To identify which packages are supported in the context of rolling upgrades done using the `DBMS_ROLLING` package, you can query the `DBA_LOGSTDBY_PLSQL_SUPPORT` view, as follows: 
```
SQL> SELECT OWNER, PKG_NAME FROM DBA_LOGSTDBY_PLSQL_SUPPORT -
> where support_level = 'DBMS_ROLLING';
```


#### Unsupported PL/SQL Supplied Packages {#GUID-703C8569-4298-48ED-98F5-6146A6D46F3F}

Oracle PL/SQL supplied packages that modify system metadata typically are not supported by SQL Apply, and therefore their effects are not visible on the logical standby database.

Examples of such packages are `DBMS_JAVA`, `DBMS_REGISTRY`, `DBMS_ALERT`, `DBMS_SPACE_ADMIN`, `DBMS_REFRESH`, ```and DBMS_AQ`. 

Additionally, the `DBMS_RESOURCE_MANAGER` package is not supported for physical standby rolling upgrades. 

##### Support for DBMS_JOB {#GUID-51E18D3F-C741-4627-B14E-C36DFE37F00A}

Specific support for `DBMS_JOB` has been provided. Jobs created on the primary database are replicated on the standby database, but are not run as long as the standby maintains its standby role. 

In the event of a switchover or failover, jobs scheduled on the original primary database automatically begin running on the new primary database.

You can also create jobs at the logical standby. These jobs only run as long as the logical standby maintains it standby role.

##### Support for DBMS_SCHEDULER {#GUID-182AE34F-BEAD-4B38-A9D0-323BE5E37B7E}

Specific support for `DBMS_SCHEDULER` has been provided to allow jobs to be run on a standby database. 

When a scheduler job is created, it defaults to the local role, so a job created on the standby defaults to a `database_role` of `LOGICAL STANDBY`. The job scheduler executes only jobs specific to the current role. On switchover or failover, the scheduler automatically switches to running jobs specific to the new role. 

Scheduler jobs are not replicated to the standby, except in the context of a rolling upgrade done using the `DBMS_ROLLING` PL/SQL package. However, existing jobs can be activated under the new role by using the `DBMS_SCHEDULER.Set_Attribute` procedure. Alternatively, jobs that should run in both roles can be cloned and the copy made specific to the other role. The `DBA_SCHEDULER_JOB_ROLES` view shows which jobs are specific to which role. 

Scheduler jobs obey the database guard when they run on a logical standby database. Thus, to run jobs that need to modify unmaintained tables, set the database guard to `STANDBY`. (It is not possible to use the `ALTER SESSION DISABLE GUARD `statement inside a PL/SQL block and have it take effect.) 

#### Handling XML and XDB PL/SQL Packages in Logical Standby {#GUID-F413799B-DB5F-4C57-A3E2-E1C3F20E3FF4}

Logical Standby supports `XMLType` data for all storage models, with some compatibility requirements. 

The requirements are as follows:

* `XMLType` stored as `CLOB` is deprecated as of Oracle Database 12*c* Release 1 (12.1). 




There are several PL/SQL packages used in conjunction with XML that are not fully supported. 

The PL/SQL packages and procedures that are supported by Logical Standby only modify in-memory structures; they do not modify data stored in the database. These packages do not generate redo and therefore are not replicated to a Logical Standby.

Certain PL/SQL packages and procedures related to XML and XDB that are not supported by Logical Standby, but that require corresponding invocations at the logical standby database for replication activities to continue, are instrumented such that invocations of these procedures at the primary database generate additional redo records indicating procedure invocation. When SQL Apply encounters such redo records, it stops and writes an error message in the `DBA_LOGSTDBY_EVENTS` table, indicating the procedure name. This allows the DBA to invoke the corresponding procedure at the logical standby database at the appropriate time so that subsequent redo records generated at the primary database can be applied successfully at the logical standby database. See [The DBMS_XMLSCHEMA Schema](data-type-ddl-support-on-logical-standby-databases.md#GUID-EA3D935A-F460-4985-B498-274F17F06EDB) through [Compensating for Ordering Sensitive Unsupported PL/SQL](data-type-ddl-support-on-logical-standby-databases.md#GUID-71AE3EBB-C4FE-4EB9-94E7-3F04FE1A0DB7) for more information about dealing with these unsupported procedures. 

The following packages contain unsupported procedures:

* `DBMS_XMLSCHEMA` (Supported if compatibility is set to 12.0.0 or higher.) 

* `DBMS_XMLINDEX`




In addition to these packages, Logical Standby does not support any modifications to the XDB schema. The objects within the XDB schema are considered to be system metadata and direct modifications to them are not replicated.

Tables managed by the Oracle XML DB Repository, also known as hierarchy-enabled tables, are not supported by Logical Standby. These tables are used to store XML data and can be accessed using the FTP and HTTP protocols, as well as the normal SQL access. For more information on these tables, refer to the [*Oracle XML DB Developer's Guide*](https://docs.oracle.com/pls/topic/lookup?ctx=en/database/oracle/oracle-database/23/sbydb&id=ADXDB5517). 

##### The DBMS_XMLSCHEMA Schema {#GUID-EA3D935A-F460-4985-B498-274F17F06EDB}

Certain procedures within the `DBMS_XMLSCHEMA` package are unsupported and cannot be replicated by Logical Standby. 

Logical Standby stops when it encounters calls to these procedures to provide the user an opportunity to take a compensating action for these calls. Sections [Dealing With Unsupported PL/SQL Procedures](data-type-ddl-support-on-logical-standby-databases.md#GUID-254F904E-C5DB-4A99-A4B9-1F5765CBF7DD) through [Compensating for Ordering Sensitive Unsupported PL/SQL](data-type-ddl-support-on-logical-standby-databases.md#GUID-71AE3EBB-C4FE-4EB9-94E7-3F04FE1A0DB7) provide more information on the alternatives available for dealing with these unsupported procedures: 

* `COPYEVOLVE`

* `INPLACEEVOLVE`

* `COMPILESCHEMA`




The XDB schema is an Oracle-managed schema. Any changes to this schema are automatically skipped by Logical Standby. The following procedure makes changes to the XDB schema which do not get replicated:

* `GENERATEBEAN`




##### The DBMS_XMLINDEX Package {#GUID-DD8598A7-8859-46C3-A0DD-01F6B42C845C}

All procedures in the `DBMS_XMLINDEX` package are supported except for these. 

* `DBMS_XMLINDEX.REGISTERPARAMETER`

* `DBMS_XMLINDEX.MODIFYPARAMETER`

* `DBMS_XMLINDEX.DROPPARAMETER`




##### Dealing With Unsupported PL/SQL Procedures {#GUID-254F904E-C5DB-4A99-A4B9-1F5765CBF7DD}

There are a couple options for dealing with unsupported PL/SQL procedures. 

The first option is to allow the Logical Standby apply process to stop and to manually perform some compensating action. The second option is to take a preemptive action and to skip the unsupported PL/SQL either by using Logical Standby skip procedures. Each of these options is discussed in the following sections.

##### Manually Compensating for Unsupported PL/SQL {#GUID-F23D42AA-97E6-4ADA-AD18-6B6D5A0C6A93}

When Logical Standby encounters something that is unsupported, it stops the apply process and records an error in the `DBA_LOGSTDBY_EVENTS` table. 

You can query this table to determine what action caused the standby to stop and what action, if any, needs to be taken to compensate.

The following example shows a sample of what this query and its output might look like:
```
select status, event from dba_logstdby_events
where commit_scn >= (select applied_scn from dba_logstdby_progress) and
status_code = 16265
order by commit_scn desc;

STATUS
--------------------------------------------------------------------------------
EVENT
--------------------------------------------------------------------------------
ORA-16265: Unsupported PL/SQL procedure encountered
begin
"XDB"."DBMS_XMLSCHEMA"."REGISTERPARAMETER" (
"NAME" => 'myIndexParam',
"PARAMETER" => 'PATH TABLE

ORA-16265: Unsupported PL/SQL procedure encountered
begin
"XDB"."DBMS_XMLSCHEMA"."REGISTERPARAMETER" (
"NAME" => 'myIndexParam',
"PARAMETER" => 'PATH TABLE

2 rows selected.
```


Two rows with the same information are returned because Logical Standby automatically retries the failed transaction. The results show that the standby was stopped when a call to `DBMS_XMLSCHEMA.REGISTERSCHEMA` was encountered for the `xmlplsqlsch2` schema. You can use this information to transfer any needed files from the primary and register the schema on the standby. 

Once the schema has been successfully registered on the standby, the apply process on the Logical Standby can be restarted. This must be performed using the `SKIP FAILED TRANSACTION `option, for example: 
```
alter database start logical standby apply skip failed transaction'
```


Logical Standby skips past the offending transaction and continues applying redo from the primary.

The general procedure for manually replicating unsupported PL/SQL follows these steps:

1. Some unsupported PL/SQL is executed on the primary database.

2. The standby database encounters the unsupported PL/SQL and stops Apply.

3. You examine the `DBA_LOGSTDBY_EVENTS` table to determine what caused Apply to stop. 

4. You execute some compensating actions on the standby for the unsupported PL/SQL.

5. You restart apply on the standby. 




##### Compensating for Ordering Sensitive Unsupported PL/SQL {#GUID-71AE3EBB-C4FE-4EB9-94E7-3F04FE1A0DB7}

Although the previous approach is useful, it cannot be used in all cases. It can only be safely used when the time that the PL/SQL is executed relative to other transactions is not critical. One case that this should not be used for is that of `DBMS_XMLSCHEMA.copyEvolve`. The `DBMS_XMLSCHEMA.copyEvolve` procedure evolves, or changes, a schema and can modify tables by adding and or removing columns and it can also change whether or not XML documents are valid.

The timing of when this procedure should be executed on the Logical Standby is critical. The only time guaranteed to be safe is when apply has stopped on the Logical Standby when it sees that this procedure was executed on the primary database.

Before evolving a schema, it is also important to quiesce any traffic on the primary that may be using the schema. Otherwise, a transaction that is executed close in time to the `evolveSchema` on the primary may be executed in a different order on the Logical Standby because the dependency between the two transactions is not apparent to the Logical Standby. Therefore, when ordering sensitive PL/SQL is involved, you should follow these steps: 

1. Quiesce changes to dependent tables on the primary.

2. Execute the `CopyEvolve` on the primary. 

3. Wait for the standby to stop on the `CopyEvolve` PL/SQL. 

4. Apply the compensating `CopyEvolve` on the standby. 

5. Restart apply on the standby.




[Example C-1](data-type-ddl-support-on-logical-standby-databases.md#GUID-71AE3EBB-C4FE-4EB9-94E7-3F04FE1A0DB7__BEHEAFGB) shows a sample of the procedures that could be used to determine how to handle `RegisterSchema` calls. 

Example C-1 PL/SQL Skip Procedure for RegisterSchema
```
-- Procedures to determine how to handle registerSchema calls

-- This procedure extracts the schema URL, or name, from the statement
-- string that is passed into the skip procedure.

Create or replace procedure sec_mgr.parse_schema_str(
statement             in varchar2,
schema_name      out varchar2)
Is
pos1 number;
pos2 number;
workingstr   varchar2(32767);
Begin

-- Find the correct argument
pos1 := instr(statement, '"SCHEMAURL" => ''');
workingstr := substr(statement, pos1 + 16);

-- Find the end of the schema name
pos1 := instr(workingstr, '''');

-- Get just the schema name
workingstr := substr(workingstr, 1, pos1 - 1);

schema_name := workingstr;

End parse_schema_str;
/
show errors


-- This procedure checks if a schema is already registered. If so,
-- it returns the value DBMS_LOGSTDBY.SKIP_ACTION_SKIP to indicate that
-- the PL/SQL should be skipped. Otherwise, the value
-- DBMS_LOGSTDBY.SKIP_ACTION_SKIP is returned and Logical Standby apply
-- will halt to allow the DBA to deal with the registerSchema call.

Create or replace procedure sec_mgr.skip_registerschema(
statement          	in varchar2,
package_owner            in varchar2,
package_name             in varchar2,
procedure_name          	in varchar2,
current_user              	in varchar2,
xidusn             	in number,
xidslt               	in number,
xidsqn             	in number,
exit_status           	in number,
skip_action      	out number)
Is
schema_exists number;
schemastr varchar2(2000);
Begin

skip_action := DBMS_LOGSTDBY.SKIP_ACTION_SKIP;

-- get the schame name from statement
parse_schema_str(statement, schemastr);

-- see if the schema is already registered
select count(*) into schema_exists from sys.all_xml_schemas s
where s.schema_url = schemastr and
s.owner = current_user;

IF schema_exists = 0 THEN
-- if the schema is not  registered, then we must stop apply
skip_action := DBMS_LOGSTDBY.SKIP_ACTION_APPLY;
ELSE
-- if the schema is already registered, then we can skip this statement
skip_action := DBMS_LOGSTDBY.SKIP_ACTION_SKIP;
END IF;

End skip_registerschema;
/
show errors

-- Register the skip procedure to deal with the unsupported registerSchema
-- PL/SQL.
Begin
sys.dbms_logstdby.skip(stmt => 'PL/SQL',
schema_name => 'XDB',
object_name   => 'DBMS_XMLSCHEMA.REGISTERSCHEMA',
proc_name     => 'SEC_MGR.SKIP_REGISTERSCHEMA',
use_like         => FALSE );
End;
/
show errors
```


### Unsupported Tables {#GUID-291FD74A-165A-4E80-9E1F-66996F1FC6CA}

It is important to identify unsupported database objects on the primary database before you create a logical standby database.

This is because changes made to unsupported data types and tables on the primary database are automatically skipped by SQL Apply on the logical standby database. Moreover, no error message is returned.

> **note:** 

Starting with Oracle Database Release 12.2, new types or features, including long identifiers, are only supported for logical replication using the `DBMS_ROLLING` package or Oracle Golden Gate. 

> **note:** 

Sorted hash cluster tables are not supported on logical standby databases.

There are three types of objects on a database, from the perspective of logical standby support:

* Objects that are explicitly maintained by SQL Apply

* Objects that are implicitly maintained by SQL Apply

* Objects that are not maintained by SQL Apply




Some schemas that ship with the Oracle database (for example, `SYSTEM`) contain objects that are implicitly maintained by SQL Apply. However, if you put a user-defined table in `SYSTEM`, then it is not maintained even if it has columns of supported data types. To discover which objects are not maintained by SQL Apply, you must run two queries. The first query is as follows: 
```
SQL> SELECT OWNER FROM DBA_LOGSTDBY_SKIP WHERE STATEMENT_OPT = 'INTERNAL SCHEMA';
```


This returns all schemas that are considered to be internal. User tables placed in these schemas are not replicated on a logical standby database and do not show up in the `DBA_LOGSTDBY_UNSUPPORTED` view. Tables in these schemas that are created by Oracle are maintained on a logical standby, if the feature implemented in the schema is supported in the context of logical standby. 

The second query you must run is as follows. It returns tables that do not belong to internal schemas and are not maintained by SQL Apply because of unsupported data types:
```
SQL> SELECT DISTINCT OWNER,TABLE_NAME FROM DBA_LOGSTDBY_UNSUPPORTED -
> ORDER BY OWNER,TABLE_NAME;

OWNER        TABLE_NAME
-----------  --------------------------
HR           COUNTRIES
OE           ORDERS
OE           CUSTOMERS
OE           WAREHOUSES
```


To view the column names and data types for one of the tables listed in the previous query, use a `SELECT` statement similar to the following: 
```
SQL> SELECT COLUMN_NAME,DATA_TYPE FROM DBA_LOGSTDBY_UNSUPPORTED -
> WHERE OWNER='OE' AND TABLE_NAME = 'CUSTOMERS';

COLUMN_NAME                      DATA_TYPE
-------------------------------  -------------------
CUST_ADDRESS                     CUST_ADDRESS_TYP
PHONE_NUMBERS                    PHONE_LIST_TYP
CUST_GEO_LOCATION                SDO_GEOMETRY
```


If the primary database contains unsupported tables, SQL Apply automatically excludes these tables when applying redo data to the logical standby database.

> **note:** 

For the queries shown in this section, if you are working in a multitenant container database (CDB) environment, then many DBA views have analogous CDB views that you should use instead. For example, you would query the `CDB_LOGSTDBY_SKIP` view instead of the `DBA_LOGSTDBY_SKIP` view. 

#### Unsupported Tables During Rolling Upgrades {#GUID-13DDE926-B175-488D-9037-F707A4D2256D}

Before you perform a rolling upgrade, determine whether any of the tables involved contain data types that are unsupported on logical standby databases.

To do this, you can query either the `DBA_LOGSTDBY_UNSUPPORTED` view or `DBA_ROLLING_UNSUPPORTED` view, depending on the type of rolling upgrade being performed. 

A rolling upgrade performed using `DBMS_ROLLING` supports more object types than a manual rolling upgrade operation. For example, only upgrades performed with `DBMS_ROLLING` support queue tables. Additionally, a rolling upgrade performed using `DBMS_ROLLING` also supports more PL/SQL packages. 

If you are performing a rolling upgrade using the `DBMS_ROLLING` PL/SQL package, as described in [Using DBMS_ROLLING to Perform a Rolling Upgrade](using-DBMS_ROLLING-to-perform-rolling-upgrade.md#GUID-70C09F5B-90BE-4C8C-96A5-45A52E05D380), then query the `DBA_ROLLING_UNSUPPORTED` or `DBA_ROLLING_SUPPORT_MODE` view. Starting with Oracle Database Release 21c, when the `COMPATIBLE` parameter is set to 21.0 or higher, the `ATTRIBUTES` column of the `DBA_ROLLING_UNSUPPORTED` view indicates why a table is not supported for rolling upgrade. The `EXPLANATION` column of the `DBA_ROLLING_SUPPORT_MODE` view contains the reason for limited support when the support mode is not FULL. 

If you are not using the `DBMS_ROLLING` package, but are instead following the manual process outlined in [ Using SQL Apply to Upgrade the Oracle Database](using-sql-apply-to-perform-rolling-upgrade.md#GUID-290F632F-5295-47F3-AEF1-2D37C69C00D7), then query the `DBA_LOGSTDBY_UNSUPPORTED` or `DBA_LOGSTDBY_SUPPORT_MODE` view. Starting with Oracle Database Release 21c, when the `COMPATIBLE` parameter is set to 21.0 or higher, the `ATTRIBUTES` column of the `DBA_LOGSTDBY_UNSUPPORTED` view indicates why a table is not supported for rolling upgrade. The `EXPLANATION` column of the `DBA_LOGSTDBY_SUPPORT_MODE` view contains the reason for limited support when the support mode is not FULL. 

> **note:** See Also: 

* [ Using SQL Apply to Upgrade the Oracle Database ](using-sql-apply-to-perform-rolling-upgrade.md#GUID-290F632F-5295-47F3-AEF1-2D37C69C00D7) for more information about performing manual rolling upgrades 

* [Using DBMS_ROLLING to Perform a Rolling Upgrade ](using-DBMS_ROLLING-to-perform-rolling-upgrade.md#GUID-70C09F5B-90BE-4C8C-96A5-45A52E05D380) for more information about performing rolling upgrades using the `DBMS_ROLLING` PL/SQL package 

* [Additional PL/SQL Package Support Available Only in the Context of DBMS_ROLLING Upgrades](data-type-ddl-support-on-logical-standby-databases.md#GUID-F54904C7-F666-4CC3-B1CB-493525476D2F) for information about PL/SQL package support available only in the context of `DBMS_ROLLING` upgrades 

* [*Oracle Database PL/SQL Packages and Types Reference*](https://docs.oracle.com/pls/topic/lookup?ctx=en/database/oracle/oracle-database/23/sbydb&id=ARPLS73534) for a description of the `DBMS_ROLLING` PL/SQL package 

* [*Oracle Database Reference*](https://docs.oracle.com/pls/topic/lookup?ctx=en/database/oracle/oracle-database/23/sbydb&id=REFRN002) for complete information about views 




#### Unsupported Tables As a Result of DML Performed In a PL/SQL Function {#GUID-8ED9EBC7-4F22-4693-84A7-897763E76A2F}

If, during an insert or update DML operation on a supported table, an out-of-line column (LOB, XMLType, or ADT) is modified through a PL/SQL function and that function in turn performs DML on another table in the course of its execution, then the redo patterns generated are unsupported by LogMiner.

As a result, redo for such a workload cannot be reliably mined using LogMiner.

### Skipped SQL Statements on a Logical Standby Database {#GUID-9DBC35BD-43C9-469F-BADD-C91752E88225}

By default, certain SQL statements are automatically skipped by SQL Apply.

The affected statements are as follows:

* `ALTER DATABASE`
* `ALTER MATERIALIZED VIEW`
* `ALTER MATERIALIZED VIEW LOG`
* `ALTER SESSION`
* `ALTER SYSTEM`
* `CREATE CONTROL FILE`
* `CREATE DATABASE`
* `CREATE DATABASE LINK`
* `CREATE PFILE FROM SPFILE`
* `CREATE MATERIALIZED VIEW`
* `CREATE MATERIALIZED VIEW LOG`
* `CREATE SCHEMA AUTHORIZATION`
* `CREATE SPFILE FROM PFILE`
* `DROP DATABASE LINK`
* `DROP MATERIALIZED VIEW`
* `DROP MATERIALIZED VIEW LOG`
* `EXPLAIN`
* `LOCK TABLE`
* `PURGE DBA_RECYCLEBIN`
* `PURGE INDEX`
* `SET CONSTRAINTS`
* `SET ROLE`
* `SET TRANSACTION`
* 


All other SQL statements executed on the primary database are applied to the logical standby database.

### DDL Statements Supported by a Logical Standby Database {#GUID-1595A660-6790-436A-BB84-D74C9187D8AE}

The `DBMS_LOGSTDBY.SKIP` procedure has several optional keywords. 

[Table C-1](data-type-ddl-support-on-logical-standby-databases.md#GUID-1595A660-6790-436A-BB84-D74C9187D8AE__BABGCFCA) lists the supported values for the `stmt` parameter of the `DBMS_LOGSTDBY.SKIP` procedure. The left column of the table lists the keywords that may be used to identify the set of SQL statements to the right of the keyword. In addition, any of the SQL statements listed in the `sys.audit_actions` table (shown in the right column of Table 1-13) are also valid values. Keywords are generally defined by database object. 

> **note:** See Also: 

[*Oracle Database PL/SQL Packages and Types Reference*](https://docs.oracle.com/pls/topic/lookup?ctx=en/database/oracle/oracle-database/23/sbydb&id=ARPLS363) for complete information about the [`DBMS_LOGSTDBY`](https://docs.oracle.com/pls/topic/lookup?ctx=en/database/oracle/oracle-database/23/sbydb&id=ARPLS363) package and [Setting up a Skip Handler for a DDL Statement](managing-oracle-data-guard-logical-standby-databases.md#GUID-28EB46A0-696D-426E-B596-225443867604)

**Table C-1 Values for stmt Parameter of the DBMS_LOGSTDBY.SKIP procedure**

Keyword | Associated SQL Statements  
---|---  
There is no keyword for this group of SQL statements. | 
```
GRANT
REVOKE
ANALYZE TABLE
ANALYZE INDEX
ANALYZE CLUSTER
```


`CLUSTER` | 
```
AUDIT CLUSTER
CREATE CLUSTER
DROP CLUSTER
TRUNCATE CLUSTER
```


`CONTEXT` | 
```
CREATE CONTEXT
DROP CONTEXT
```


`DATABASE LINK` | 
```
CREATE DATABASE LINK
CREATE PUBLIC DATABASE LINK
DROP DATABASE LINK
DROP PUBLIC DATABASE LINK
```


`DIMENSION` | 
```
ALTER DIMENSION
CREATE DIMENSION
DROP DIMENSION
```


`DIRECTORY` | 
```
CREATE DIRECTORY
DROP DIRECTORY
```


`DML` |  Includes DML statements on a table (for example: `INSERT`, `UPDATE`, and `DELETE`)   
`INDEX` | 
```
ALTER INDEX
CREATE INDEX
DROP INDEX
```


`NON_SCHEMA_DDL` |  *All DDL that does not pertain to a particular schema*<br>**Note:** `SCHEMA_NAME` and `OBJECT_NAME` must be null   
`PROCEDURE`Foot 1 | 
```
ALTER FUNCTION
ALTER PACKAGE
ALTER PACKAGE BODY
ALTER PROCEDURE
CREATE FUNCTION
CREATE LIBRARY
CREATE PACKAGE
CREATE PACKAGE BODY
CREATE PROCEDURE
DROP FUNCTION
DROP LIBRARY
DROP PACKAGE
DROP PACKAGE BODY
DROP PROCEDURE
```


`PROFILE` | 
```
ALTER PROFILE
CREATE PROFILE
DROP PROFILE
```


`PUBLIC DATABASE LINK` | 
```
CREATE PUBLIC DATABASE LINK
DROP PUBLIC DATABASE LINK
```


`PUBLIC SYNONYM` | 
```
CREATE PUBLIC SYNONYM
DROP PUBLIC SYNONYM
```


`ROLE` | 
```
ALTER ROLE
CREATE ROLE
DROP ROLE
SET ROLE
```


`ROLLBACK SEGMENT` | 
```
ALTER ROLLBACK SEGMENT
CREATE ROLLBACK SEGMENT
DROP ROLLBACK SEGMENT
```


`SCHEMA_DDL` |  *All DDL statements that create, modify, or drop schema objects (for example: tables, indexes, and columns)*<br>**Note:** `SCHEMA_NAME` and `OBJECT_NAME` must *not* be null   
`SEQUENCE` | 
```
ALTER SEQUENCE
CREATE SEQUENCE
DROP SEQUENCE
```


`SYNONYM` | 
```
CREATE PUBLIC SYNONYM
CREATE SYNONYM
DROP PUBLIC SYNONYM
DROP SYNONYM
```


`SYSTEM AUDIT` | 
```
AUDIT SQL_statements
NOAUDIT SQL_statements
```


`TABLE` | 
```
CREATE TABLE
ALTER TABLE
DROP TABLE
TRUNCATE TABLE
```


`TABLESPACE` | 
```
CREATE TABLESPACE
DROP TABLESPACE
ALTER TABLESPACE
```


`TRIGGER` | 
```
ALTER TRIGGER
CREATE TRIGGER
DISABLE ALL TRIGGERS
DISABLE TRIGGER
DROP TRIGGER
ENABLE ALL TRIGGERS
ENABLE TRIGGER
```


`TYPE` | 
```
ALTER TYPE
ALTER TYPE BODY
CREATE TYPE
CREATE TYPE BODY
DROP TYPE
DROP TYPE BODY
```


`USER` | 
```
ALTER USER
CREATE USER
DROP USER
```


`VIEW` | 
```
CREATE VIEW
DROP VIEW
```


Footnote 1 

Java schema objects (sources, classes, and resources) are considered the same as procedures for purposes of skipping (ignoring) SQL statements.

> **note:** See Also: 

The following sections that provide usage examples of the `SKIP` and `UNSKIP` options: 

* [Using DBMS_LOGSTDBY.SKIP to Prevent Changes to Specific Schema Objects](managing-oracle-data-guard-logical-standby-databases.md#GUID-B976F237-1888-46D3-A901-F28FE362ACBC)

* [Setting up a Skip Handler for a DDL Statement](managing-oracle-data-guard-logical-standby-databases.md#GUID-28EB46A0-696D-426E-B596-225443867604)

* [Modifying a Logical Standby Database](managing-oracle-data-guard-logical-standby-databases.md#GUID-43971CB9-3337-4575-B4EF-D591878982F1)

* [Adding or Re-Creating Tables On a Logical Standby Database](managing-oracle-data-guard-logical-standby-databases.md#GUID-DD88E3D6-D6DB-4175-BB59-F048B9A87BAA)




#### DDL Statements that Use DBLINKS {#GUID-84B0FD0B-2193-410B-BAAF-F11E9523803E}

SQL Apply may not correctly apply DDL statements that reference a database link.

An example of such a statement is as follows:
```
CREATE TABLE tablename AS SELECT * FROM bar@dblink
```


This is because the *dblink* at the logical standby database may not point to the same database as the primary database. If SQL Apply fails while executing such a DDL statement, then use the `DBMS_LOGSTDBY.INSTANTIATE_TABLE` procedure for the table being created, and then restart SQL APPLY operations. 

#### Replication of AUD$ and FGA_LOG$ on Logical Standbys {#GUID-04AE6CBF-F487-4EBC-B066-96F198971872}

Auditing and fine-grained auditing are supported on logical standbys.

Changes made to the `AUD$` and `FGA_AUD$` tables at the primary database are replicated at the logical standby. 

Both the `AUD$` table and the `FGA_AUD$` table have a DBID column. If the DBID value is that of the primary database, then the row was replicated to the logical standby based on activities at the primary. If the DBID value is that of the logical standby database, then the row was inserted as a result of local activities at the logical standby. 

After the logical standby database assumes the primary role as a result of a role transition (either a switchover or failover), the `AUD$` and `FGA_AUD$` tables at the *new primary* (originally the logical standby) and at the *new logical standby* (originally the primary) are not necessarily synchronized. Therefore, it is possible that not all rows in the `AUD$` or `FGA_AUD$` tables at the new primary database will be present in the new logical standby database. However, all rows in `AUD$` and `FGA_LOG$` that were inserted while the database was in a primary role are replicated and present in the logical standby database. 

### Distributed Transactions and XA Support {#GUID-8BF91B38-CC5C-40A9-B062-683B4A6F5AB8}

You can perform distributed transactions using a couple different methods.

* Modify tables in multiple databases in a coordinated manner using database links.

* Use the XA interface, as exposed by the `DBMS_XA` package in supplied PL/SQL packages or via OCI or JDBC libraries. The XA interface implements X/Open Distributed Transaction Processing (DTP) architecture. 




Changes made to the primary database during a distributed transaction using either of these two methods are replicated to the logical standby database.

However, the distributed transaction state is not replicated. The logical standby database does not inherit the in-doubt or prepared state of such a transaction, and it does not replicate the changes using the same global transaction identifier used at the primary database for the XA transactions. As a result, if you fail over to a logical standby database before committing a distributed transaction, the changes are rolled back at the logical standby. This rollback occurs even if the distributed transaction on the primary database is in a prepared state and has successfully completed the first phase of the two-phased commit protocol. Switchover operations wait for all active distributed transactions to complete, and are not affected by this restriction.

XA transactions can be performed in two ways:

* tightly coupled, where different XA branches share locks

* loosely coupled, where different XA branches do not share locks




Replication of changes made by loosely coupled XA branches is supported regardless of the `COMPATIBLE` parameter value. Replication of changes made by tightly coupled branches on an Oracle RAC primary is supported only with `COMPATIBLE=11.2` or higher. 

### Support for SecureFiles LOBs {#GUID-F10F2D96-B441-4465-810D-1A06112552C8}

SecureFiles LOBs are supported when the database compatibility level is set to 11.2 or higher. 

Transparent Data Encryption and data compression can be enabled on SecureFiles LOB columns at the primary database. 

Deduplication of SecureFiles LOB columns and SecureFiles Database File System (DBFS) operations are fully supported. 

If SQL Apply encounters redo generated by unsupported operations, it stops with an `ORA-16211: Unsupported record found in the archived redo log` error. To continue, add a skip rule for the affected table using `DBMS_LOGSTDBY.SKIP` and restart SQL Apply. 

### Support for Database File System (DBFS) {#GUID-0309A2A5-E4DE-406C-9DD3-74DD86F419BB}

The Database File System (DBFS) creates a standard file system interface on top of files and directories that are stored in database tables, which makes it easier for you to access and manage files stored in the database. 

Logical standby supports the Database File System (DBFS). See [*Oracle Database SecureFiles and Large Objects Developer's Guide*](https://docs.oracle.com/pls/topic/lookup?ctx=en/database/oracle/oracle-database/23/sbydb&id=ADLOB45943) for more information about DBFS. 

### Character Set Considerations {#GUID-CC85C064-6115-423C-BF30-790B3D17FC43}

There are considerations to keep in mind regarding character sets.

* It is not supported to have a Data Guard configuration in which the primary database and logical standby database have different character sets.

* Configurations in which a multitenant container database (CDB) has a mixed character set are only supported when using `DBMS_ROLLING` for rolling upgrades. A mixed character set means that the `CDB$ROOT` and one or more of the CDB’s pluggable databases (PDBs) have different character sets. 




### Additional PL/SQL Package Support Available Only in the Context of DBMS_ROLLING Upgrades {#GUID-F54904C7-F666-4CC3-B1CB-493525476D2F}

Replication of certain packages is available only in the context of rolling upgrades performed using the `DBMS_ROLLING` package. 

The affected packages are as follows:

* DBFS

* `DBMS_DBFS_CONTENT_ADMIN`

* `DBMS_DBFS_SFS`

* `DBMS_DBFS_SFS_ADMIN`

* Lightweight Security

* `XS_ACL`

* `XS_DATA_SECURITY`

* `XS_NAMESPACE`

* `XS_PRINCIPAL`

* `XS_ROLESET`

* `XS_SECURITY_CLASS`

* Oracle Streams Advanced Queuing (AQ)

* `DBMS_AQ`

* `DBMS_AQJMS`

* `DBMS_AQADM` (except for the following procedures: `SCHEDULE_PROPAGATION`, `RECOVER_PROPAGATION`, `UNSCHEDULE_PROPAGATION`, `ALTER_PROPAGATION_SCHEDULE`, `ENABLE_PROPAGATION_SCHEDULE`, and `DISABLE_PROPAGATION_SCHEDULE`) 

* Oracle Text

* `CTX_ADM`

* `CTX_ANL`

* `CTX_CLS`

* `CTX_DDL`

* `CTX_DOC`

* `CTX_ENTITY`

* `CTX_OUTPUT`

* `CTX_QUERY`

* `CTX_THES`

* `CTX_TREE`

* Scheduler 

* `DBMS_SCHEDULER`

* XDB-related 

* `DBMS_RESCONFIG`

* `DBMS_XDB_CONFIG` (Certain procedures are not supported. See [*Oracle XML DB Developer's Guide*](https://docs.oracle.com/pls/topic/lookup?ctx=en/database/oracle/oracle-database/23/sbydb&id=ADXDB5703) for more information.) 

* `DBMS_XDB_REPOS`

* `DBMS_XDBRESOURCE`

* `DBMS_XDB_VERSION`

* `DBMS_XDBZ` (Certain procedures are not supported. See [*Oracle XML DB Developer's Guide*](https://docs.oracle.com/pls/topic/lookup?ctx=en/database/oracle/oracle-database/23/sbydb&id=ADXDB5703) for more information.) 




> **note:** See Also: 

* [Using DBMS_ROLLING to Perform a Rolling Upgrade ](using-DBMS_ROLLING-to-perform-rolling-upgrade.md#GUID-70C09F5B-90BE-4C8C-96A5-45A52E05D380)

* [*Oracle Database Real Application Security Administrator's and Developer's Guide*](https://docs.oracle.com/pls/topic/lookup?ctx=en/database/oracle/oracle-database/23/sbydb&id=DBFSG80000) for more information about the Lightweight Security packages 

* [*Oracle Database PL/SQL Packages and Types Reference*](https://docs.oracle.com/pls/topic/lookup?ctx=en/database/oracle/oracle-database/23/sbydb&id=ARPLS72235) for more information about `DBMS_SCHEDULER`, XDB-related, and DBFS-related packages 


