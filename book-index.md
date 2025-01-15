[ Previous ](using-ALTERNATE-attribute-to-configure-alternate-destinations.md) Next  JavaScript must be enabled to correctly display this content 

##  Index 

A  B  C  D  E  F  G  H  I  K  L  M  N  O  P  Q  R  S  T  U  V  W 

* * *

##  A 

  * activating 
    * a logical standby database  [ 9.3.2 ](managing-oracle-data-guard-role-transitions.md#GUID-EBD0C21B-6356-446E-8BF5-7B7D96554713)
    * a physical standby database  [ 12.9.3 ](using-RMAN-in-oracle-data-guard-configurations.md#GUID-E8430B01-A852-4EAF-A711-10F3211F36A3)
  * Active Data Guard 
    * and physical standby databases  [ 2.1.1 ](getting-started-with-oracle-data-guard.md#GUID-88BBC338-BC93-454C-B5C6-23CA8F0C329E) , [ 10.2 ](managing-oracle-data-guard-physical-standby-databases.md#GUID-D5FB88EC-799D-40E7-80E1-19474E3167E4)
    * and the real-time query feature  [ 10.2.1 ](managing-oracle-data-guard-physical-standby-databases.md#GUID-07CB190C-C248-4FF5-AB64-EAA9C6D42677)
  * adding 
    * datafiles  [ 10.3.1 ](managing-oracle-data-guard-physical-standby-databases.md#GUID-C60E4DB4-F424-454D-B4DF-7A97B2F09540) , [ A.10.1.1 ](troubleshooting-oracle-data-guard.md#GUID-16CE2533-7190-4717-88E0-BDC53DBD887B)
    * indexes on logical standby databases  [ 11.5.4.1 ](managing-oracle-data-guard-logical-standby-databases.md#GUID-FB0ABA4B-4EA4-458E-9223-B3BEE00A581A)
    * new or existing standby databases  [ 1.4 ](introduction-to-oracle-data-guard-concepts.md#GUID-538B9DDD-1553-479D-8E1D-0B5C6848403E)
    * online redo log files  [ 10.3.5 ](managing-oracle-data-guard-physical-standby-databases.md#GUID-64E46731-490D-4228-AC87-A07CDE3C75FE)
    * tablespaces  [ 10.3.1 ](managing-oracle-data-guard-physical-standby-databases.md#GUID-C60E4DB4-F424-454D-B4DF-7A97B2F09540)
  * adjusting 
    * initialization parameter file 
      * for logical standby database  [ 4.3.4.2 ](creating-oracle-data-guard-logical-standby.md#GUID-31F89C7C-174C-49E4-9A5E-3B9F538BD739)
  * AFFIRM attribute  [ 17.1 ](LOG_ARCHIVE_DEST_n-parameter-attributes.md#GUID-559248E6-B153-4B7B-A8A9-9CEE102088F0)
  * ALTER DATABASE statement 
    * ACTIVATE STANDBY DATABASE clause  [ 9.3.2 ](managing-oracle-data-guard-role-transitions.md#GUID-EBD0C21B-6356-446E-8BF5-7B7D96554713) , [ 12.9.3 ](using-RMAN-in-oracle-data-guard-configurations.md#GUID-E8430B01-A852-4EAF-A711-10F3211F36A3)
    * ADD STANDBY LOGFILE clause  [ 18.1 ](sql-statements-used-by-oracle-data-guard.md#GUID-F690B602-6582-465B-8E86-0B0A52838D66)
    * ADD STANDBY LOGFILE MEMBER clause  [ 18.1 ](sql-statements-used-by-oracle-data-guard.md#GUID-F690B602-6582-465B-8E86-0B0A52838D66) , [ A.1.1 ](troubleshooting-oracle-data-guard.md#GUID-AFDF503B-600D-4B7E-A476-2E7E0CB489C0)
    * ADD SUPPLEMENTAL LOG DATA clause  [ 18.1 ](sql-statements-used-by-oracle-data-guard.md#GUID-F690B602-6582-465B-8E86-0B0A52838D66)
    * COMMIT TO SWITCHOVER clause  [ 9.3.1 ](managing-oracle-data-guard-role-transitions.md#GUID-42B8B448-90E0-4D7D-82A3-AAA273E6E49B) , [ 18.1 ](sql-statements-used-by-oracle-data-guard.md#GUID-F690B602-6582-465B-8E86-0B0A52838D66)
      * troubleshooting  [ G.3.2 ](performing-oracle-data-guard-role-transitions.md#GUID-A9CF13FE-3E89-4FB3-B2BF-6536DCAD3ADB)
    * CREATE STANDBY CONTROLFILE clause  [ 3.3.2 ](creating-oracle-data-guard-physical-standby.md#GUID-B471D788-AFD3-48B3-9709-D63C34DBC269)
      * REUSE clause  [ 18.1 ](sql-statements-used-by-oracle-data-guard.md#GUID-F690B602-6582-465B-8E86-0B0A52838D66)
    * DROP STANDBY LOGFILE MEMBER clause  [ 18.1 ](sql-statements-used-by-oracle-data-guard.md#GUID-F690B602-6582-465B-8E86-0B0A52838D66)
    * GUARD clause  [ 11.2 ](managing-oracle-data-guard-logical-standby-databases.md#GUID-572B9755-69F5-4BD7-9DFF-1F7EB1DE3771)
    * MOUNT STANDBY DATABASE clause  [ 18.1 ](sql-statements-used-by-oracle-data-guard.md#GUID-F690B602-6582-465B-8E86-0B0A52838D66)
    * OPEN READ ONLY clause  [ 18.1 ](sql-statements-used-by-oracle-data-guard.md#GUID-F690B602-6582-465B-8E86-0B0A52838D66)
    * PREPARE TO SWITCHOVER clause  [ 9.3.1 ](managing-oracle-data-guard-role-transitions.md#GUID-42B8B448-90E0-4D7D-82A3-AAA273E6E49B) , [ 18.1 ](sql-statements-used-by-oracle-data-guard.md#GUID-F690B602-6582-465B-8E86-0B0A52838D66)
    * RECOVER MANAGED STANDBY DATABASE clause  [ 3.3.6 ](creating-oracle-data-guard-physical-standby.md#GUID-3D65210D-9FE5-490E-938E-57FC2E07A569) , [ 4.3.5 ](creating-oracle-data-guard-logical-standby.md#GUID-68777C01-83F6-4CCF-9BA2-547ED180138D) , [ 18.1 ](sql-statements-used-by-oracle-data-guard.md#GUID-F690B602-6582-465B-8E86-0B0A52838D66)
      * canceling  [ 8.3.2 ](oracle-data-guard-redo-apply-services.md#GUID-1808DF72-C384-4BC2-A75F-D73C56C22A2B)
      * failover  [ 18.1 ](sql-statements-used-by-oracle-data-guard.md#GUID-F690B602-6582-465B-8E86-0B0A52838D66)
    * REGISTER LOGFILE clause  [ A.4.1 ](troubleshooting-oracle-data-guard.md#GUID-616EF287-25CE-4F6D-A0C3-9A4DEDA3A97C) , [ G.3.1 ](performing-oracle-data-guard-role-transitions.md#GUID-9D024B1F-54EF-4C95-A810-1FA09B7AE3E4)
    * RENAME FILE clause  [ A.1.1 ](troubleshooting-oracle-data-guard.md#GUID-AFDF503B-600D-4B7E-A476-2E7E0CB489C0)
    * SET STANDBY DATABASE clause 
      * TO MAXIMIZE AVAILABILITY clause  [ 18.1 ](sql-statements-used-by-oracle-data-guard.md#GUID-F690B602-6582-465B-8E86-0B0A52838D66)
      * TO MAXIMIZE PERFORMANCE clause  [ 9.1.4 ](managing-oracle-data-guard-role-transitions.md#GUID-3E6BE44A-3077-4E67-86E7-B641DBF30083)
      * TO MAXIMIZE PROTECTION clause  [ 18.1 ](sql-statements-used-by-oracle-data-guard.md#GUID-F690B602-6582-465B-8E86-0B0A52838D66)
    * START LOGICAL STANDBY APPLY clause  [ 8.4.1 ](oracle-data-guard-redo-apply-services.md#GUID-5E640AEE-12C7-4ED8-8C42-B989F7768E91) , [ 13.5 ](using-sql-apply-to-perform-rolling-upgrade.md#GUID-5995DF99-5268-43BE-BD63-D0DEAA8241EC) , [ A.6 ](troubleshooting-oracle-data-guard.md#GUID-E2C9EEB9-CDCE-46B4-A7ED-D60B7FCD657A)
      * IMMEDIATE keyword  [ 8.4.1 ](oracle-data-guard-redo-apply-services.md#GUID-5E640AEE-12C7-4ED8-8C42-B989F7768E91)
      * starting SQL Apply  [ 4.3.5 ](creating-oracle-data-guard-logical-standby.md#GUID-68777C01-83F6-4CCF-9BA2-547ED180138D)
    * STOP LOGICAL STANDBY APPLY clause  [ 8.4.2 ](oracle-data-guard-redo-apply-services.md#GUID-C7B53D2A-5D38-4BA5-8142-98A81691A415) , [ 9.3.2 ](managing-oracle-data-guard-role-transitions.md#GUID-EBD0C21B-6356-446E-8BF5-7B7D96554713)
  * alternate archive destinations 
    * setting up initialization parameters for  [ A.2 ](troubleshooting-oracle-data-guard.md#GUID-397ED7EA-1A1C-4224-82A3-746448BE6944)
  * ALTERNATE attribute  [ 17.2 ](LOG_ARCHIVE_DEST_n-parameter-attributes.md#GUID-88222D10-22FD-4F3B-BBE5-B59C8ABEC725)
    * LOG_ARCHIVE_DEST_n initialization parameter  [ A.2 ](troubleshooting-oracle-data-guard.md#GUID-397ED7EA-1A1C-4224-82A3-746448BE6944)
  * ALTER SESSION DISABLE GUARD statement 
    * overriding the database guard  [ 11.5.4 ](managing-oracle-data-guard-logical-standby-databases.md#GUID-43971CB9-3337-4575-B4EF-D591878982F1)
  * ALTER SESSION statement 
    * ENABLE GUARD clause  [ 18.2 ](sql-statements-used-by-oracle-data-guard.md#GUID-0C30DE80-F67E-4B27-9ABE-F6FA51358739)
  * ALTER TABLESPACE statement  [ 10.3.4 ](managing-oracle-data-guard-physical-standby-databases.md#GUID-2D3E5488-237C-467A-AB8B-7C09B6E43CE0) , [ A.10.1.1 ](troubleshooting-oracle-data-guard.md#GUID-16CE2533-7190-4717-88E0-BDC53DBD887B)
  * application continuity 
    * support in Oracle Data Guard  [ 1.6.1 ](introduction-to-oracle-data-guard-concepts.md#GUID-37CFE42A-D731-4A07-977B-09BE3D309E0C)
  * applying 
    * redo data immediately  [ 8.2.1 ](oracle-data-guard-redo-apply-services.md#GUID-9922F0E7-3E19-449E-8C35-57B7E5714C54)
    * redo data on standby database  [ 1.3.2 ](introduction-to-oracle-data-guard-concepts.md#GUID-D284E617-3F4F-4128-98F5-A3EB88772BF5) , [ 8 ](oracle-data-guard-redo-apply-services.md#GUID-B40DF83F-D4A0-4710-935B-AA5D4B2D9010)
    * SQL statements to logical standby databases  [ 8.4 ](oracle-data-guard-redo-apply-services.md#GUID-7D8A891C-2906-4CC0-9181-B29C4F3C2450)
  * applying state  [ 11.4.1 ](managing-oracle-data-guard-logical-standby-databases.md#GUID-5BD79860-B3B8-4E0D-9F55-2E9918B93292)
  * apply lag 
    * monitoring in a real-time query environment  [ 10.2.1.1 ](managing-oracle-data-guard-physical-standby-databases.md#GUID-C225DCBC-E9FB-4E86-9D19-A5D20C3A8AA5)
  * apply lag tolerance 
    * configuring in a real-time query environment  [ 10.2.1.2 ](managing-oracle-data-guard-physical-standby-databases.md#GUID-622B6596-54F7-4736-8A8B-0C5E308AC3F5)
  * apply services 
    * defined  [ 1.3.2 ](introduction-to-oracle-data-guard-concepts.md#GUID-D284E617-3F4F-4128-98F5-A3EB88772BF5) , [ 8.1 ](oracle-data-guard-redo-apply-services.md#GUID-74C8C1F3-70FC-4543-B958-83F309B6B01B)
    * delaying application of redo data  [ 8.2.2 ](oracle-data-guard-redo-apply-services.md#GUID-B891A164-1FDC-4AB3-838F-E6A91E7DA0B7) , [ 17.5 ](LOG_ARCHIVE_DEST_n-parameter-attributes.md#GUID-C014D9BB-79D3-4E49-9477-A5CBF0E88167)
    * real-time apply 
      * defined  [ 8.2.1 ](oracle-data-guard-redo-apply-services.md#GUID-9922F0E7-3E19-449E-8C35-57B7E5714C54)
    * Redo Apply 
      * defined  [ 8.1 ](oracle-data-guard-redo-apply-services.md#GUID-74C8C1F3-70FC-4543-B958-83F309B6B01B) , [ 8.3 ](oracle-data-guard-redo-apply-services.md#GUID-2B839172-947E-48A4-9FFD-33CC6907809F)
      * monitoring  [ 8.3.3 ](oracle-data-guard-redo-apply-services.md#GUID-F9964184-5E31-4373-A24E-318AD5C511F5)
      * starting  [ 8.3.1 ](oracle-data-guard-redo-apply-services.md#GUID-54FA44F8-1EDA-4CAF-8B04-90CC6108FED8)
      * stopping  [ 8.3.2 ](oracle-data-guard-redo-apply-services.md#GUID-1808DF72-C384-4BC2-A75F-D73C56C22A2B)
    * SQL Apply 
      * defined  [ 1.3.2 ](introduction-to-oracle-data-guard-concepts.md#GUID-D284E617-3F4F-4128-98F5-A3EB88772BF5) , [ 8.1 ](oracle-data-guard-redo-apply-services.md#GUID-74C8C1F3-70FC-4543-B958-83F309B6B01B)
      * monitoring  [ 8.4.3 ](oracle-data-guard-redo-apply-services.md#GUID-8A34CBE4-AD50-4918-AB5F-671CF9A66B20)
      * starting  [ 8.4.1 ](oracle-data-guard-redo-apply-services.md#GUID-5E640AEE-12C7-4ED8-8C42-B989F7768E91)
      * stopping  [ 8.4.2 ](oracle-data-guard-redo-apply-services.md#GUID-C7B53D2A-5D38-4BA5-8142-98A81691A415)
  * archive destinations 
    * alternate  [ A.2 ](troubleshooting-oracle-data-guard.md#GUID-397ED7EA-1A1C-4224-82A3-746448BE6944)
  * archived redo log files 
    * accessing information about  [ 10.6.1.4 ](managing-oracle-data-guard-physical-standby-databases.md#GUID-E97EB4D8-48E0-4355-A878-9CA3E80C0AE8)
    * applying 
      * Redo Apply technology  [ 1.3.2 ](introduction-to-oracle-data-guard-concepts.md#GUID-D284E617-3F4F-4128-98F5-A3EB88772BF5)
      * SQL Apply technology  [ 1.3.2 ](introduction-to-oracle-data-guard-concepts.md#GUID-D284E617-3F4F-4128-98F5-A3EB88772BF5)
    * delaying application  [ 17.5 ](LOG_ARCHIVE_DEST_n-parameter-attributes.md#GUID-C014D9BB-79D3-4E49-9477-A5CBF0E88167)
      * on the standby database  [ 8.2.2 ](oracle-data-guard-redo-apply-services.md#GUID-B891A164-1FDC-4AB3-838F-E6A91E7DA0B7)
    * deleting unneeded  [ 11.4.2 ](managing-oracle-data-guard-logical-standby-databases.md#GUID-893B304C-123F-4DCA-ACA2-96BB74E7A8A6)
    * destinations 
      * disabling  [ 7.2.2 ](oracle-data-guard-redo-transport-services.md#GUID-45DFFA7C-3967-4B46-8C2C-DBD22A4D6A74)
      * enabling  [ 7.2.2 ](oracle-data-guard-redo-transport-services.md#GUID-45DFFA7C-3967-4B46-8C2C-DBD22A4D6A74)
    * managing gaps  [ 1.9 ](introduction-to-oracle-data-guard-concepts.md#GUID-C65BF458-4BF7-4723-BAFC-5860B5B88A2B)
      * See also gap management  [ 1.9 ](introduction-to-oracle-data-guard-concepts.md#GUID-C65BF458-4BF7-4723-BAFC-5860B5B88A2B)
    * redo data transmitted  [ 1.3.2 ](introduction-to-oracle-data-guard-concepts.md#GUID-D284E617-3F4F-4128-98F5-A3EB88772BF5) , [ 8.1 ](oracle-data-guard-redo-apply-services.md#GUID-74C8C1F3-70FC-4543-B958-83F309B6B01B)
    * registering 
      * during failover  [ 9.3.2 ](managing-oracle-data-guard-role-transitions.md#GUID-EBD0C21B-6356-446E-8BF5-7B7D96554713)
    * standby databases and  [ 8.3.3 ](oracle-data-guard-redo-apply-services.md#GUID-F9964184-5E31-4373-A24E-318AD5C511F5) , [ 8.4.3 ](oracle-data-guard-redo-apply-services.md#GUID-8A34CBE4-AD50-4918-AB5F-671CF9A66B20) , [ 10.6.1 ](managing-oracle-data-guard-physical-standby-databases.md#GUID-567E9883-B1BE-431D-86E9-872BAB6A899F)
    * troubleshooting switchover problems  [ A.4.1 ](troubleshooting-oracle-data-guard.md#GUID-616EF287-25CE-4F6D-A0C3-9A4DEDA3A97C) , [ G.3.1 ](performing-oracle-data-guard-role-transitions.md#GUID-9D024B1F-54EF-4C95-A810-1FA09B7AE3E4)
  * ARCHIVELOG mode 
    * software requirements  [ 2.3.2 ](getting-started-with-oracle-data-guard.md#GUID-A9F7D87E-49EB-4CE9-A076-1F35F35C0EAB)
  * archiving 
    * real-time apply  [ 8.2.1 ](oracle-data-guard-redo-apply-services.md#GUID-9922F0E7-3E19-449E-8C35-57B7E5714C54)
    * specifying 
      * failure resolution policies for  [ 17.14 ](LOG_ARCHIVE_DEST_n-parameter-attributes.md#GUID-AE01BD16-8FD7-4C4D-BBB6-156281C947A7)
    * to failed destinations  [ 17.14 ](LOG_ARCHIVE_DEST_n-parameter-attributes.md#GUID-AE01BD16-8FD7-4C4D-BBB6-156281C947A7)
  * ASM 
    * See Automatic Storage Management (ASM)  [ 15.5 ](examples-of-using-oracle-data-guard.md#GUID-6FB574E5-2F3A-4C9E-A3F0-7C9154B0F540)
  * ASYNC attribute  [ 17.15 ](LOG_ARCHIVE_DEST_n-parameter-attributes.md#GUID-E3626025-2E4C-4C61-B5DF-2BB106F0A349)
  * attributes 
    * deprecated for the LOG_ARCHIVE_DEST_n initialization parameter  [ 17 ](LOG_ARCHIVE_DEST_n-parameter-attributes.md#GUID-B04FC22C-F0B0-440F-BAF4-182EE547FCC1)
  * AUD$ table 
    * replication on logical standbys  [ C.16.2 ](data-type-ddl-support-on-logical-standby-databases.md#GUID-04AE6CBF-F487-4EBC-B066-96F198971872)
  * automatic block repair  [ 10.2.1.5 ](managing-oracle-data-guard-physical-standby-databases.md#GUID-67B644A3-161B-4A83-A0B2-923BF51AD932)
  * automatic detection of missing log files  [ 1.3.1 ](introduction-to-oracle-data-guard-concepts.md#GUID-05D621EE-51AF-41E4-9CFC-45AEBAB08F57) , [ 1.9 ](introduction-to-oracle-data-guard-concepts.md#GUID-C65BF458-4BF7-4723-BAFC-5860B5B88A2B)
  * automatic failover  [ 1.3.3 ](introduction-to-oracle-data-guard-concepts.md#GUID-EFAC6E17-BCE5-417C-9124-1595DBF0ABFA)
  * Automatic Storage Management (ASM) 
    * creating a standby database that uses  [ 15.5 ](examples-of-using-oracle-data-guard.md#GUID-6FB574E5-2F3A-4C9E-A3F0-7C9154B0F540)
  * automatic switchover  [ 1.3.3 ](introduction-to-oracle-data-guard-concepts.md#GUID-EFAC6E17-BCE5-417C-9124-1595DBF0ABFA)
    * See also switchovers  [ 9.1 ](managing-oracle-data-guard-role-transitions.md#GUID-BEB883C9-0391-472A-A05E-9A5776C06F4C)



* * *

##  B 

  * BACKUP INCREMENTAL FROM SCN command 
    * scenarios using  [ 12.9.1 ](using-RMAN-in-oracle-data-guard-configurations.md#GUID-1F1D37FE-48A6-46D4-98DB-C0E3707E3BCF) , [ 12.11 ](using-RMAN-in-oracle-data-guard-configurations.md#GUID-F0EB4F8E-7D2F-4674-9A5B-AABC368D8F11)
  * backup operations 
    * after unrecoverable operations  [ 15.4.3 ](examples-of-using-oracle-data-guard.md#GUID-55881B7E-55C1-4342-94DA-A75739504980)
    * configuring on a physical standby database  [ 1.1.5 ](introduction-to-oracle-data-guard-concepts.md#GUID-0DA5AED1-4D4B-446D-ADD2-7F463A688AEF)
    * offloading on the standby database  [ 1.9 ](introduction-to-oracle-data-guard-concepts.md#GUID-C65BF458-4BF7-4723-BAFC-5860B5B88A2B)
    * primary databases  [ 1.1.2 ](introduction-to-oracle-data-guard-concepts.md#GUID-C49AC6F4-C89B-4487-BC18-428D65865B9A)
    * used by the broker  [ 1.4 ](introduction-to-oracle-data-guard-concepts.md#GUID-538B9DDD-1553-479D-8E1D-0B5C6848403E)
    * using RMAN  [ 12 ](using-RMAN-in-oracle-data-guard-configurations.md#GUID-516D9C60-F2EF-4822-BB87-5D3BE877B69B)
  * basic readable standby database See simulating a standby database environment  [ 2.3.2 ](getting-started-with-oracle-data-guard.md#GUID-A9F7D87E-49EB-4CE9-A076-1F35F35C0EAB)
  * batch processing 
    * on a logical standby database  [ 11.1.1.4 ](managing-oracle-data-guard-logical-standby-databases.md#GUID-97797605-D5BB-49B7-8E9D-D3E7C56A767C)
  * benefits 
    * Data Guard  [ 1.9 ](introduction-to-oracle-data-guard-concepts.md#GUID-C65BF458-4BF7-4723-BAFC-5860B5B88A2B)
    * logical standby database  [ 2.1.2 ](getting-started-with-oracle-data-guard.md#GUID-D6AB6955-C1E1-4FE9-A9D9-6F6275B8748A)
    * of a rolling upgrade  [ 13.1 ](using-sql-apply-to-perform-rolling-upgrade.md#GUID-701A35C5-B545-43DD-8854-F6948E0BB32A)
    * physical standby database  [ 2.1.1 ](getting-started-with-oracle-data-guard.md#GUID-88BBC338-BC93-454C-B5C6-23CA8F0C329E)
  * block repair, automatic  [ 10.2.1.5 ](managing-oracle-data-guard-physical-standby-databases.md#GUID-67B644A3-161B-4A83-A0B2-923BF51AD932)
  * broker 
    * command-line interface  [ 1.9 ](introduction-to-oracle-data-guard-concepts.md#GUID-C65BF458-4BF7-4723-BAFC-5860B5B88A2B)
    * defined  [ 1.4 ](introduction-to-oracle-data-guard-concepts.md#GUID-538B9DDD-1553-479D-8E1D-0B5C6848403E)
    * graphical user interface  [ 1.9 ](introduction-to-oracle-data-guard-concepts.md#GUID-C65BF458-4BF7-4723-BAFC-5860B5B88A2B)



* * *

##  C 

  * cascaded redo transport destinations  [ 7.3 ](oracle-data-guard-redo-transport-services.md#GUID-34BCB162-D996-4678-97F1-497805764950)
  * cascading redo 
    * non-real-time  [ 7.3 ](oracle-data-guard-redo-transport-services.md#GUID-34BCB162-D996-4678-97F1-497805764950)
    * real-time  [ 7.3 ](oracle-data-guard-redo-transport-services.md#GUID-34BCB162-D996-4678-97F1-497805764950)
  * cascading redo data  [ 7.3 ](oracle-data-guard-redo-transport-services.md#GUID-34BCB162-D996-4678-97F1-497805764950)
    * configuration requirements  [ 7.3 ](oracle-data-guard-redo-transport-services.md#GUID-34BCB162-D996-4678-97F1-497805764950)
    * restrictions  [ 7.3 ](oracle-data-guard-redo-transport-services.md#GUID-34BCB162-D996-4678-97F1-497805764950)
  * CDBs 
    * creating a logical standby of  [ 4.1 ](creating-oracle-data-guard-logical-standby.md#GUID-E2BB4836-32D1-4D57-9B32-75B1239170E2)
    * creating a physical standby of  [ 3.1 ](creating-oracle-data-guard-physical-standby.md#GUID-D5F0EB8F-8534-462A-911C-216E6DC7CE69)
  * character sets 
    * changing on primary databases  [ 15.9 ](examples-of-using-oracle-data-guard.md#GUID-FCDB44CE-FBFE-43BB-88E3-74267B38FE70)
    * configurations with differing  [ C.20 ](data-type-ddl-support-on-logical-standby-databases.md#GUID-CC85C064-6115-423C-BF30-790B3D17FC43)
  * checklist 
    * tasks for creating physical standby databases  [ 3.3 ](creating-oracle-data-guard-physical-standby.md#GUID-A86D403B-B3F5-4B05-9255-5BADD513F9CF)
    * tasks for creating standby databases  [ 4.3 ](creating-oracle-data-guard-logical-standby.md#GUID-2A3EDE02-DE51-4813-891C-248AA5F1C027)
  * checkpoints 
    * V$LOGSTDBY_PROGRESS view  [ 11.1.1.3 ](managing-oracle-data-guard-logical-standby-databases.md#GUID-F0FDEEF3-B4EA-40F3-BF7B-2167D33D91C8)
  * chunking 
    * transactions  [ 11.1.1.1 ](managing-oracle-data-guard-logical-standby-databases.md#GUID-344AF03D-88A6-4DE1-8650-D5F1B9781FC3)
  * command-line interface 
    * broker  [ 1.9 ](introduction-to-oracle-data-guard-concepts.md#GUID-C65BF458-4BF7-4723-BAFC-5860B5B88A2B)
  * COMMIT TO SWITCHOVER clause 
    * of ALTER DATABASE  [ 9.3.1 ](managing-oracle-data-guard-role-transitions.md#GUID-42B8B448-90E0-4D7D-82A3-AAA273E6E49B) , [ 18.1 ](sql-statements-used-by-oracle-data-guard.md#GUID-F690B602-6582-465B-8E86-0B0A52838D66)
      * troubleshooting  [ G.3.2 ](performing-oracle-data-guard-role-transitions.md#GUID-A9CF13FE-3E89-4FB3-B2BF-6536DCAD3ADB)
  * COMPATIBLE initialization parameter 
    * setting after upgrading Oracle Database software  [ B.5 ](upgrading-patching-downgrading-oracle-data-guard-configuration.md#GUID-253C94C1-8E40-4378-A0A5-2627C248D873)
    * setting for a rolling upgrade  [ 13.5 ](using-sql-apply-to-perform-rolling-upgrade.md#GUID-5995DF99-5268-43BE-BD63-D0DEAA8241EC)
  * complementary technologies  [ 1.7 ](introduction-to-oracle-data-guard-concepts.md#GUID-9EEFD8E5-901F-4092-85F4-2E0069AC5923)
  * COMPRESSION attribute  [ 17.3 ](LOG_ARCHIVE_DEST_n-parameter-attributes.md#GUID-DD58C7C2-EE9D-4632-94C9-F6C652718006)
  * configuration options 
    * creating with Data Guard broker  [ 1.4 ](introduction-to-oracle-data-guard-concepts.md#GUID-538B9DDD-1553-479D-8E1D-0B5C6848403E)
    * overview  [ 1.1 ](introduction-to-oracle-data-guard-concepts.md#GUID-AB9DF863-2C7E-4767-81F2-56AD0FA30B49)
    * physical standby databases 
      * location and directory structure  [ 2.4 ](getting-started-with-oracle-data-guard.md#GUID-B59DBCF7-13A7-4F49-97E9-BF1BEF803699)
    * standby databases 
      * delayed standby  [ 8.2.2 ](oracle-data-guard-redo-apply-services.md#GUID-B891A164-1FDC-4AB3-838F-E6A91E7DA0B7)
  * configuring 
    * backups on standby databases  [ 1.1.5 ](introduction-to-oracle-data-guard-concepts.md#GUID-0DA5AED1-4D4B-446D-ADD2-7F463A688AEF)
    * disaster recovery  [ 1.1.5 ](introduction-to-oracle-data-guard-concepts.md#GUID-0DA5AED1-4D4B-446D-ADD2-7F463A688AEF)
    * initialization parameters 
      * for alternate archive destinations  [ A.2 ](troubleshooting-oracle-data-guard.md#GUID-397ED7EA-1A1C-4224-82A3-746448BE6944)
    * listener for physical standby databases  [ 5.1.1 ](creating-oracle-data-guard-far-sync-instance.md#GUID-43FE645C-1383-44F1-9127-7EFB2A402DB3)
    * no data loss  [ 1.3.3 ](introduction-to-oracle-data-guard-concepts.md#GUID-EFAC6E17-BCE5-417C-9124-1595DBF0ABFA)
    * physical standby databases  [ 2.4 ](getting-started-with-oracle-data-guard.md#GUID-B59DBCF7-13A7-4F49-97E9-BF1BEF803699)
    * reporting operations on a logical standby database  [ 1.1.5 ](introduction-to-oracle-data-guard-concepts.md#GUID-0DA5AED1-4D4B-446D-ADD2-7F463A688AEF)
    * standby databases at remote locations  [ 1.1.5 ](introduction-to-oracle-data-guard-concepts.md#GUID-0DA5AED1-4D4B-446D-ADD2-7F463A688AEF)
  * constraints 
    * handled on a logical standby database  [ 11.6.3 ](managing-oracle-data-guard-logical-standby-databases.md#GUID-50903261-075E-44F4-80F2-E5ED7C165F0D)
  * control files 
    * copying  [ 3.3.4 ](creating-oracle-data-guard-physical-standby.md#GUID-4834C26E-6493-4C5A-91F7-680FD43FADF7)
    * creating for standby databases  [ 3.3.2 ](creating-oracle-data-guard-physical-standby.md#GUID-B471D788-AFD3-48B3-9709-D63C34DBC269)
  * converting 
    * a logical standby database to a physical standby database 
      * terminating  [ 4.3.4.1 ](creating-oracle-data-guard-logical-standby.md#GUID-715EBEC1-21B9-4DA9-80A5-ED58033F94C8)
    * a physical standby database to a logical standby database  [ 4.3.4.1 ](creating-oracle-data-guard-logical-standby.md#GUID-715EBEC1-21B9-4DA9-80A5-ED58033F94C8)
  * CONVERT TO SNAPSHOT STANDBY clause on the ALTER DATABASE statement  [ 18.1 ](sql-statements-used-by-oracle-data-guard.md#GUID-F690B602-6582-465B-8E86-0B0A52838D66)
  * COORDINATOR process 
    * LSP background process  [ 11.1 ](managing-oracle-data-guard-logical-standby-databases.md#GUID-735EED44-DFDB-43B8-ADBC-F1A3A62E7D93)
  * copying 
    * control files  [ 3.3.4 ](creating-oracle-data-guard-physical-standby.md#GUID-4834C26E-6493-4C5A-91F7-680FD43FADF7)
  * CREATE STANDBY CONTROLFILE clause 
    * of ALTER DATABASE  [ 3.3.2 ](creating-oracle-data-guard-physical-standby.md#GUID-B471D788-AFD3-48B3-9709-D63C34DBC269) , [ 18.1 ](sql-statements-used-by-oracle-data-guard.md#GUID-F690B602-6582-465B-8E86-0B0A52838D66)
  * CREATE TABLE AS SELECT (CTAS) statements 
    * applied on a logical standby database  [ 11.1.1.5 ](managing-oracle-data-guard-logical-standby-databases.md#GUID-1BBCF0C2-E189-47EF-B7D1-805BF552E4DB)
  * creating 
    * indexes on logical standby databases  [ 11.5.4.1 ](managing-oracle-data-guard-logical-standby-databases.md#GUID-FB0ABA4B-4EA4-458E-9223-B3BEE00A581A)



* * *

##  D 

  * data availability 
    * balancing against system performance requirements  [ 1.9 ](introduction-to-oracle-data-guard-concepts.md#GUID-C65BF458-4BF7-4723-BAFC-5860B5B88A2B)
  * database caching modes 
    * force full database caching 
      * status in Data Guard configurations  [ 10.2.1.7 ](managing-oracle-data-guard-physical-standby-databases.md#GUID-12950D5A-FE9F-4444-A6DE-26FCC5653225)
  * database guard  [ 11.5.4 ](managing-oracle-data-guard-logical-standby-databases.md#GUID-43971CB9-3337-4575-B4EF-D591878982F1)
    * overriding  [ 11.5.4 ](managing-oracle-data-guard-logical-standby-databases.md#GUID-43971CB9-3337-4575-B4EF-D591878982F1)
  * database incarnation 
    * changes with OPEN RESETLOGS  [ 10.4 ](managing-oracle-data-guard-physical-standby-databases.md#GUID-02218512-31A6-461E-96B4-0B9997DCFC4A)
  * database roles 
    * primary  [ 9.1 ](managing-oracle-data-guard-role-transitions.md#GUID-BEB883C9-0391-472A-A05E-9A5776C06F4C)
    * standby  [ 1.1.2 ](introduction-to-oracle-data-guard-concepts.md#GUID-C49AC6F4-C89B-4487-BC18-428D65865B9A) , [ 9.1 ](managing-oracle-data-guard-role-transitions.md#GUID-BEB883C9-0391-472A-A05E-9A5776C06F4C)
    * transitions  [ 1.3.3 ](introduction-to-oracle-data-guard-concepts.md#GUID-EFAC6E17-BCE5-417C-9124-1595DBF0ABFA)
  * databases 
    * failover and  [ 9.1.4 ](managing-oracle-data-guard-role-transitions.md#GUID-3E6BE44A-3077-4E67-86E7-B641DBF30083)
    * role transition and  [ 9.1 ](managing-oracle-data-guard-role-transitions.md#GUID-BEB883C9-0391-472A-A05E-9A5776C06F4C)
    * surviving disasters and data corruptions  [ 1 ](introduction-to-oracle-data-guard-concepts.md#GUID-5E73667D-4A56-445E-911F-1E99092DD8D7)
    * upgrading software versions  [ 13.1 ](using-sql-apply-to-perform-rolling-upgrade.md#GUID-701A35C5-B545-43DD-8854-F6948E0BB32A)
  * database schema 
    * physical standby databases  [ 1.1.2 ](introduction-to-oracle-data-guard-concepts.md#GUID-C49AC6F4-C89B-4487-BC18-428D65865B9A)
  * datafiles 
    * adding to primary database  [ 10.3.1 ](managing-oracle-data-guard-physical-standby-databases.md#GUID-C60E4DB4-F424-454D-B4DF-7A97B2F09540)
    * monitoring  [ 10.6 ](managing-oracle-data-guard-physical-standby-databases.md#GUID-0682EE24-B495-48E8-9E1F-52AC0E6FE486)
    * renaming on the primary database  [ 10.3.4 ](managing-oracle-data-guard-physical-standby-databases.md#GUID-2D3E5488-237C-467A-AB8B-7C09B6E43CE0)
  * Data Guard broker 
    * defined  [ 1.4 ](introduction-to-oracle-data-guard-concepts.md#GUID-538B9DDD-1553-479D-8E1D-0B5C6848403E)
    * distributed management framework  [ 9 ](managing-oracle-data-guard-role-transitions.md#GUID-44F26D5F-E036-4ACD-B8F0-224F5498FBCE)
    * failovers  [ 1.4 ](introduction-to-oracle-data-guard-concepts.md#GUID-538B9DDD-1553-479D-8E1D-0B5C6848403E)
      * fast-start  [ 9 ](managing-oracle-data-guard-role-transitions.md#GUID-44F26D5F-E036-4ACD-B8F0-224F5498FBCE)
      * manual  [ 1.4 ](introduction-to-oracle-data-guard-concepts.md#GUID-538B9DDD-1553-479D-8E1D-0B5C6848403E) , [ 9 ](managing-oracle-data-guard-role-transitions.md#GUID-44F26D5F-E036-4ACD-B8F0-224F5498FBCE)
    * fast-start failover  [ 1.4 ](introduction-to-oracle-data-guard-concepts.md#GUID-538B9DDD-1553-479D-8E1D-0B5C6848403E)
    * switchovers  [ 9 ](managing-oracle-data-guard-role-transitions.md#GUID-44F26D5F-E036-4ACD-B8F0-224F5498FBCE)
  * Data Guard configurations 
    * archiving to standby destinations using the log writer process  [ 8.2.1 ](oracle-data-guard-redo-apply-services.md#GUID-9922F0E7-3E19-449E-8C35-57B7E5714C54)
    * defined  [ 1.1 ](introduction-to-oracle-data-guard-concepts.md#GUID-AB9DF863-2C7E-4767-81F2-56AD0FA30B49)
    * protection modes  [ 1.5 ](introduction-to-oracle-data-guard-concepts.md#GUID-65DA4E2E-3666-4972-895C-EA45D043B127)
    * upgrading Oracle Database software  [ B ](upgrading-patching-downgrading-oracle-data-guard-configuration.md#GUID-A4FBE490-7E7E-4998-B80E-01C93C77AE2F)
  * data loss 
    * due to failover  [ 1.3.3 ](introduction-to-oracle-data-guard-concepts.md#GUID-EFAC6E17-BCE5-417C-9124-1595DBF0ABFA)
    * switchover and  [ 9.1 ](managing-oracle-data-guard-role-transitions.md#GUID-BEB883C9-0391-472A-A05E-9A5776C06F4C)
  * data protection 
    * balancing against performance  [ 1.9 ](introduction-to-oracle-data-guard-concepts.md#GUID-C65BF458-4BF7-4723-BAFC-5860B5B88A2B)
    * flexibility  [ 1.9 ](introduction-to-oracle-data-guard-concepts.md#GUID-C65BF458-4BF7-4723-BAFC-5860B5B88A2B)
    * provided by Data Guard  [ 1 ](introduction-to-oracle-data-guard-concepts.md#GUID-5E73667D-4A56-445E-911F-1E99092DD8D7)
  * data protection modes 
    * enforced by redo transport services  [ 1.3.1 ](introduction-to-oracle-data-guard-concepts.md#GUID-05D621EE-51AF-41E4-9CFC-45AEBAB08F57)
    * overview  [ 1.5 ](introduction-to-oracle-data-guard-concepts.md#GUID-65DA4E2E-3666-4972-895C-EA45D043B127)
  * Data Pump utility 
    * using transportable tablespaces with physical standby databases  [ 10.3.3 ](managing-oracle-data-guard-physical-standby-databases.md#GUID-88158C9B-51FB-4F13-93FC-97D737C86BF0)
  * DB_FILE_NAME_CONVERT initialization parameter 
    * setting at standby site after a switchover  [ A.4.3 ](troubleshooting-oracle-data-guard.md#GUID-ACDE1813-1DBD-48E1-8C42-0A2E4ED36631) , [ G.3.3 ](performing-oracle-data-guard-role-transitions.md#GUID-F394098F-8726-45AC-A66F-B91402719D6A)
    * setting on physical standby database  [ 3.3.3 ](creating-oracle-data-guard-physical-standby.md#GUID-06FDC24C-6E90-420B-BB63-3B6BAC4D8AFA)
    * when planning standby location and directory structure  [ 2.4 ](getting-started-with-oracle-data-guard.md#GUID-B59DBCF7-13A7-4F49-97E9-BF1BEF803699)
  * DB_ROLE_CHANGE system event  [ 9.1.5 ](managing-oracle-data-guard-role-transitions.md#GUID-18E84C50-79D7-4158-A5B8-FD0C3EEEB8B6)
  * DB_UNIQUE_NAME attribute  [ 17.4 ](LOG_ARCHIVE_DEST_n-parameter-attributes.md#GUID-59781B0B-B8EA-4676-9F93-A7A72E021D7A)
  * DB_UNIQUE_NAME initialization parameter  [ A.4.2 ](troubleshooting-oracle-data-guard.md#GUID-2BE05596-2E74-4FEE-84AB-81777B5B9E13) , [ G.3.2 ](performing-oracle-data-guard-role-transitions.md#GUID-A9CF13FE-3E89-4FB3-B2BF-6536DCAD3ADB)
    * required with LOG_ARCHIVE_CONFIG parameter  [ 16 ](oracle-initialization-parameters-used-by-oracle-data-guard.md#GUID-A1CF9B00-F0F5-4F3B-A0E0-9D6ABF775EC9)
    * setting database initialization parameters  [ 3.2.4 ](creating-oracle-data-guard-physical-standby.md#GUID-0FD317D6-1A49-4DB3-A141-D00CB3089E84)
  * DBA_LOGMNR_PURGED_LOG view 
    * list archived redo log files that can be deleted  [ 11.4.2 ](managing-oracle-data-guard-logical-standby-databases.md#GUID-893B304C-123F-4DCA-ACA2-96BB74E7A8A6)
  * DBA_LOGSTDBY_EVENTS view  [ 11.3.1 ](managing-oracle-data-guard-logical-standby-databases.md#GUID-BEF12BA7-9397-4156-9ED3-BAC7935D7D96) , [ A.6 ](troubleshooting-oracle-data-guard.md#GUID-E2C9EEB9-CDCE-46B4-A7ED-D60B7FCD657A)
  * DBA_LOGSTDBY_LOG view  [ 11.3.2 ](managing-oracle-data-guard-logical-standby-databases.md#GUID-646D305C-21C3-4590-8A6B-B2D02F90E8F2)
  * DBA_TABLESPACES view  [ 10.6 ](managing-oracle-data-guard-physical-standby-databases.md#GUID-0682EE24-B495-48E8-9E1F-52AC0E6FE486)
  * DBMS_LOGSTDBY.BUILD procedure 
    * building a dictionary in the redo data  [ 4.3.3.2 ](creating-oracle-data-guard-logical-standby.md#GUID-E5D227F2-C456-44F3-AEEA-2C8340439EF3)
  * DBMS_LOGSTDBY package 
    * INSTANTIATE_TABLE procedure  [ 11.5.5 ](managing-oracle-data-guard-logical-standby-databases.md#GUID-DD88E3D6-D6DB-4175-BB59-F048B9A87BAA)
    * SKIP_ERROR procedure  [ A.3 ](troubleshooting-oracle-data-guard.md#GUID-3BC98594-CFA0-4EDC-92B9-52F19088B224)
    * SKIP_TRANSACTION procedure  [ A.6 ](troubleshooting-oracle-data-guard.md#GUID-E2C9EEB9-CDCE-46B4-A7ED-D60B7FCD657A)
    * SKIP procedure  [ A.6 ](troubleshooting-oracle-data-guard.md#GUID-E2C9EEB9-CDCE-46B4-A7ED-D60B7FCD657A)
  * DBMS_ROLLING package 
    * used for rolling upgrades  [ 14 ](using-DBMS_ROLLING-to-perform-rolling-upgrade.md#GUID-70C09F5B-90BE-4C8C-96A5-45A52E05D380)
  * DDL statements 
    * supported by SQL Apply  [ C ](data-type-ddl-support-on-logical-standby-databases.md#GUID-340BA6D6-74D5-4684-AF39-08890D5CD9D4)
  * DDL Statements 
    * that use DBLINKS  [ C.16.1 ](data-type-ddl-support-on-logical-standby-databases.md#GUID-84B0FD0B-2193-410B-BAAF-F11E9523803E)
  * DDL transactions 
    * applied on a logical standby database  [ 11.1.1.5 ](managing-oracle-data-guard-logical-standby-databases.md#GUID-1BBCF0C2-E189-47EF-B7D1-805BF552E4DB)
    * applying to a logical standby database  [ 11.1.1.5 ](managing-oracle-data-guard-logical-standby-databases.md#GUID-1BBCF0C2-E189-47EF-B7D1-805BF552E4DB)
  * DELAY attribute  [ 17.5 ](LOG_ARCHIVE_DEST_n-parameter-attributes.md#GUID-C014D9BB-79D3-4E49-9477-A5CBF0E88167)
    * LOG_ARCHIVE_DEST_n initialization parameter  [ 8.2.2 ](oracle-data-guard-redo-apply-services.md#GUID-B891A164-1FDC-4AB3-838F-E6A91E7DA0B7)
  * delaying 
    * application of archived redo log files  [ 17.5 ](LOG_ARCHIVE_DEST_n-parameter-attributes.md#GUID-C014D9BB-79D3-4E49-9477-A5CBF0E88167)
    * application of redo log files  [ 8.2.2 ](oracle-data-guard-redo-apply-services.md#GUID-B891A164-1FDC-4AB3-838F-E6A91E7DA0B7)
  * deleting 
    * archived redo log files 
      * indicated by the DBA_LOGMNR_PURGED_LOG view  [ 11.4.2 ](managing-oracle-data-guard-logical-standby-databases.md#GUID-893B304C-123F-4DCA-ACA2-96BB74E7A8A6)
      * not needed by SQL Apply  [ 11.4.2 ](managing-oracle-data-guard-logical-standby-databases.md#GUID-893B304C-123F-4DCA-ACA2-96BB74E7A8A6)
  * deprecated attributes 
    * on the LOG_ARCHIVE_DEST_n initialization parameter  [ 17 ](LOG_ARCHIVE_DEST_n-parameter-attributes.md#GUID-B04FC22C-F0B0-440F-BAF4-182EE547FCC1)
  * destinations 
    * displaying with V$ARCHIVE_DEST view  [ 19 ](oracle-data-guard-in-oracle-database-views.md#GUID-FF72163C-78B0-475F-B4CE-135EF288A32A)
    * role-based definitions  [ 17.17 ](LOG_ARCHIVE_DEST_n-parameter-attributes.md#GUID-2E1845CC-B3CA-4409-BACB-6AB40D6D42E5)
  * detecting 
    * missing archived redo log files  [ 1.3.1 ](introduction-to-oracle-data-guard-concepts.md#GUID-05D621EE-51AF-41E4-9CFC-45AEBAB08F57) , [ 1.9 ](introduction-to-oracle-data-guard-concepts.md#GUID-C65BF458-4BF7-4723-BAFC-5860B5B88A2B)
  * DGMGRL command-line interface 
    * invoking failovers  [ 1.4 ](introduction-to-oracle-data-guard-concepts.md#GUID-538B9DDD-1553-479D-8E1D-0B5C6848403E) , [ 9 ](managing-oracle-data-guard-role-transitions.md#GUID-44F26D5F-E036-4ACD-B8F0-224F5498FBCE)
    * simplifying switchovers  [ 1.4 ](introduction-to-oracle-data-guard-concepts.md#GUID-538B9DDD-1553-479D-8E1D-0B5C6848403E) , [ 9 ](managing-oracle-data-guard-role-transitions.md#GUID-44F26D5F-E036-4ACD-B8F0-224F5498FBCE)
  * dictionary 
    * building a LogMiner  [ 4.3.3.2 ](creating-oracle-data-guard-logical-standby.md#GUID-E5D227F2-C456-44F3-AEEA-2C8340439EF3)
  * directory locations 
    * Optimal Flexible Architecture (OFA)  [ 2.4 ](getting-started-with-oracle-data-guard.md#GUID-B59DBCF7-13A7-4F49-97E9-BF1BEF803699)
    * structure on standby databases  [ 2.4 ](getting-started-with-oracle-data-guard.md#GUID-B59DBCF7-13A7-4F49-97E9-BF1BEF803699)
  * direct path inserts 
    * SQL Apply DML considerations  [ 11.1.1.4 ](managing-oracle-data-guard-logical-standby-databases.md#GUID-97797605-D5BB-49B7-8E9D-D3E7C56A767C)
  * disabling 
    * a destination for archived redo log files  [ 7.2.2 ](oracle-data-guard-redo-transport-services.md#GUID-45DFFA7C-3967-4B46-8C2C-DBD22A4D6A74)
  * disaster recovery 
    * configuring  [ 1.1.5 ](introduction-to-oracle-data-guard-concepts.md#GUID-0DA5AED1-4D4B-446D-ADD2-7F463A688AEF)
    * provided by Oracle Data Guard  [ 1 ](introduction-to-oracle-data-guard-concepts.md#GUID-5E73667D-4A56-445E-911F-1E99092DD8D7)
    * provided by standby databases  [ 1.1.5 ](introduction-to-oracle-data-guard-concepts.md#GUID-0DA5AED1-4D4B-446D-ADD2-7F463A688AEF)
  * disk I/O 
    * controlling with the AFFIRM and NOAFFIRM attributes  [ 17.1 ](LOG_ARCHIVE_DEST_n-parameter-attributes.md#GUID-559248E6-B153-4B7B-A8A9-9CEE102088F0)
  * distributed transactions  [ C.17 ](data-type-ddl-support-on-logical-standby-databases.md#GUID-8BF91B38-CC5C-40A9-B062-683B4A6F5AB8)
  * DML 
    * batch updates on a logical standby database  [ 11.1.1.4 ](managing-oracle-data-guard-logical-standby-databases.md#GUID-97797605-D5BB-49B7-8E9D-D3E7C56A767C)
  * DML operations 
    * on temporary tables  [ 10.2.3.1 ](managing-oracle-data-guard-physical-standby-databases.md#GUID-DAF341CA-69C9-497D-A50C-FDC89ED9C1D8)
  * DML transactions 
    * applying to a logical standby database  [ 11.1.1.4 ](managing-oracle-data-guard-logical-standby-databases.md#GUID-97797605-D5BB-49B7-8E9D-D3E7C56A767C)
  * downgrading 
    * Oracle Database software  [ B.7 ](upgrading-patching-downgrading-oracle-data-guard-configuration.md#GUID-FDB0FA2C-EA66-423C-AFD9-51F3FA4899F8)
  * dropping 
    * online redo log files  [ 10.3.5 ](managing-oracle-data-guard-physical-standby-databases.md#GUID-64E46731-490D-4228-AC87-A07CDE3C75FE)
  * DROP STANDBY LOGFILE MEMBER clause 
    * of ALTER DATABASE  [ 18.1 ](sql-statements-used-by-oracle-data-guard.md#GUID-F690B602-6582-465B-8E86-0B0A52838D66)



* * *

##  E 

  * ENABLE GUARD clause 
    * of ALTER SESSION  [ 18.2 ](sql-statements-used-by-oracle-data-guard.md#GUID-0C30DE80-F67E-4B27-9ABE-F6FA51358739)
  * enabling 
    * database guard on logical standby databases  [ 18.2 ](sql-statements-used-by-oracle-data-guard.md#GUID-0C30DE80-F67E-4B27-9ABE-F6FA51358739)
    * destinations for archived redo log files  [ 7.2.2 ](oracle-data-guard-redo-transport-services.md#GUID-45DFFA7C-3967-4B46-8C2C-DBD22A4D6A74)
    * real-time apply 
      * on logical standby databases  [ 8.4.1 ](oracle-data-guard-redo-apply-services.md#GUID-5E640AEE-12C7-4ED8-8C42-B989F7768E91)



* * *

##  F 

  * failovers  [ 1.3.3 ](introduction-to-oracle-data-guard-concepts.md#GUID-EFAC6E17-BCE5-417C-9124-1595DBF0ABFA)
    * Data Guard broker  [ 1.4 ](introduction-to-oracle-data-guard-concepts.md#GUID-538B9DDD-1553-479D-8E1D-0B5C6848403E) , [ 9 ](managing-oracle-data-guard-role-transitions.md#GUID-44F26D5F-E036-4ACD-B8F0-224F5498FBCE)
    * defined  [ 1.3.3 ](introduction-to-oracle-data-guard-concepts.md#GUID-EFAC6E17-BCE5-417C-9124-1595DBF0ABFA)
    * displaying history with DBA_LOGSTDBY_HISTORY  [ 19 ](oracle-data-guard-in-oracle-database-views.md#GUID-FF72163C-78B0-475F-B4CE-135EF288A32A)
    * fast-start failover  [ 9 ](managing-oracle-data-guard-role-transitions.md#GUID-44F26D5F-E036-4ACD-B8F0-224F5498FBCE)
    * flashing back databases after  [ 9.4 ](managing-oracle-data-guard-role-transitions.md#GUID-95E5629A-17F6-44B7-813F-00DD871359FA)
    * logical standby databases and  [ 9.3.2 ](managing-oracle-data-guard-role-transitions.md#GUID-EBD0C21B-6356-446E-8BF5-7B7D96554713)
    * manual versus automatic  [ 1.3.3 ](introduction-to-oracle-data-guard-concepts.md#GUID-EFAC6E17-BCE5-417C-9124-1595DBF0ABFA)
    * physical standby databases and  [ 18.1 ](sql-statements-used-by-oracle-data-guard.md#GUID-F690B602-6582-465B-8E86-0B0A52838D66)
    * preparing for  [ 9.1.4 ](managing-oracle-data-guard-role-transitions.md#GUID-3E6BE44A-3077-4E67-86E7-B641DBF30083)
    * simplifying with Data Guard broker  [ 9 ](managing-oracle-data-guard-role-transitions.md#GUID-44F26D5F-E036-4ACD-B8F0-224F5498FBCE)
    * transferring redo data before  [ 9.1.4 ](managing-oracle-data-guard-role-transitions.md#GUID-3E6BE44A-3077-4E67-86E7-B641DBF30083)
    * viewing characteristics for logical standby databases  [ 11.3.3 ](managing-oracle-data-guard-logical-standby-databases.md#GUID-76033857-52EC-41B2-B130-CC9083F3FA99)
    * with maximum performance mode  [ 9.1.4 ](managing-oracle-data-guard-role-transitions.md#GUID-3E6BE44A-3077-4E67-86E7-B641DBF30083)
    * with maximum protection mode  [ 9.1.4 ](managing-oracle-data-guard-role-transitions.md#GUID-3E6BE44A-3077-4E67-86E7-B641DBF30083)
  * failure resolution policies 
    * specifying for redo transport services  [ 17.14 ](LOG_ARCHIVE_DEST_n-parameter-attributes.md#GUID-AE01BD16-8FD7-4C4D-BBB6-156281C947A7)
  * far sync instances  [ 5 ](creating-oracle-data-guard-far-sync-instance.md#GUID-8AD7FBA2-42B0-46CF-852B-1AF0CB4A36E8)
    * creating  [ 5.1 ](creating-oracle-data-guard-far-sync-instance.md#GUID-3B796B03-DB88-4592-84CB-3E019C977CBA)
    * supported protection modes  [ 5.4 ](creating-oracle-data-guard-far-sync-instance.md#GUID-54F9001B-A5F2-4134-A6BA-521DC7E41E57)
  * fast-start failover 
    * automatic failover  [ 1.4 ](introduction-to-oracle-data-guard-concepts.md#GUID-538B9DDD-1553-479D-8E1D-0B5C6848403E) , [ 9 ](managing-oracle-data-guard-role-transitions.md#GUID-44F26D5F-E036-4ACD-B8F0-224F5498FBCE)
    * monitoring  [ 10.6 ](managing-oracle-data-guard-physical-standby-databases.md#GUID-0682EE24-B495-48E8-9E1F-52AC0E6FE486)
  * FastSync mode  [ 6.1 ](oracle-data-guard-protection-modes.md#GUID-7EF6EFEE-7E31-4F80-9C97-1B25DAA025F8)
  * FGA_LOG$ table 
    * replication on logical standbys  [ C.16.2 ](data-type-ddl-support-on-logical-standby-databases.md#GUID-04AE6CBF-F487-4EBC-B066-96F198971872)
  * file specifications 
    * renaming on the logical standby database  [ 11.5.3 ](managing-oracle-data-guard-logical-standby-databases.md#GUID-28EB46A0-696D-426E-B596-225443867604)
  * Flashback Database 
    * after a role transition  [ 9.4 ](managing-oracle-data-guard-role-transitions.md#GUID-95E5629A-17F6-44B7-813F-00DD871359FA)
    * after OPEN RESETLOGS  [ 15.3 ](examples-of-using-oracle-data-guard.md#GUID-BAA5ED38-29A0-4E8D-8435-AA083D19C13E)
    * after role transitions  [ 9.4 ](managing-oracle-data-guard-role-transitions.md#GUID-95E5629A-17F6-44B7-813F-00DD871359FA)
    * characteristics complementary to Data Guard  [ 1.7 ](introduction-to-oracle-data-guard-concepts.md#GUID-9EEFD8E5-901F-4092-85F4-2E0069AC5923)
    * physical standby database  [ 15.2.1 ](examples-of-using-oracle-data-guard.md#GUID-C7C39BEC-A841-48D1-BE4B-9BB49C65B9C9)
  * force full database caching mode 
    * status in Data Guard configurations  [ 10.2.1.7 ](managing-oracle-data-guard-physical-standby-databases.md#GUID-12950D5A-FE9F-4444-A6DE-26FCC5653225)



* * *

##  G 

  * gap management 
    * automatic detection and resolution  [ 1.3.1 ](introduction-to-oracle-data-guard-concepts.md#GUID-05D621EE-51AF-41E4-9CFC-45AEBAB08F57) , [ 1.9 ](introduction-to-oracle-data-guard-concepts.md#GUID-C65BF458-4BF7-4723-BAFC-5860B5B88A2B)
    * detecting missing log files  [ 1.9 ](introduction-to-oracle-data-guard-concepts.md#GUID-C65BF458-4BF7-4723-BAFC-5860B5B88A2B)
    * registering archived redo log files 
      * during failover  [ 9.3.2 ](managing-oracle-data-guard-role-transitions.md#GUID-EBD0C21B-6356-446E-8BF5-7B7D96554713)
  * Global Data Services  [ 1.7 ](introduction-to-oracle-data-guard-concepts.md#GUID-9EEFD8E5-901F-4092-85F4-2E0069AC5923)
  * global temporary tables 
    * DML operations  [ 10.2.3.1 ](managing-oracle-data-guard-physical-standby-databases.md#GUID-DAF341CA-69C9-497D-A50C-FDC89ED9C1D8)
    * using with Active Data Guard  [ 10.2.3.1 ](managing-oracle-data-guard-physical-standby-databases.md#GUID-DAF341CA-69C9-497D-A50C-FDC89ED9C1D8)



* * *

##  H 

  * high availability 
    * Oracle Real Application Clusters 
      * characteristics complementary to Data Guard  [ 1.7 ](introduction-to-oracle-data-guard-concepts.md#GUID-9EEFD8E5-901F-4092-85F4-2E0069AC5923)
    * provided by Oracle Data Guard  [ 1 ](introduction-to-oracle-data-guard-concepts.md#GUID-5E73667D-4A56-445E-911F-1E99092DD8D7)
    * provided by Oracle RAC and Data Guard  [ 1.7 ](introduction-to-oracle-data-guard-concepts.md#GUID-9EEFD8E5-901F-4092-85F4-2E0069AC5923)



* * *

##  I 

  * idle state  [ 11.4.1 ](managing-oracle-data-guard-logical-standby-databases.md#GUID-5BD79860-B3B8-4E0D-9F55-2E9918B93292)
  * incarnation of a database 
    * changed  [ 10.4 ](managing-oracle-data-guard-physical-standby-databases.md#GUID-02218512-31A6-461E-96B4-0B9997DCFC4A)
  * initialization parameters 
    * DB_UNIQUE_NAME  [ 3.2.4 ](creating-oracle-data-guard-physical-standby.md#GUID-0FD317D6-1A49-4DB3-A141-D00CB3089E84) , [ A.4.2 ](troubleshooting-oracle-data-guard.md#GUID-2BE05596-2E74-4FEE-84AB-81777B5B9E13) , [ G.3.2 ](performing-oracle-data-guard-role-transitions.md#GUID-A9CF13FE-3E89-4FB3-B2BF-6536DCAD3ADB)
    * LOG_ARCHIVE_MIN_SUCCEED_DEST  [ 17.9 ](LOG_ARCHIVE_DEST_n-parameter-attributes.md#GUID-C4AD4812-D4A6-41C9-8F03-4C87B64F1D4D)
    * LOG_FILE_NAME_CONVERT  [ E.2.2.3.1 ](creating-data-guard-standby-database-using-RMAN.md#GUID-730789B8-813C-433F-BF49-D34B4077DE77)
    * setting for both the primary and standby roles  [ 17.17 ](LOG_ARCHIVE_DEST_n-parameter-attributes.md#GUID-2E1845CC-B3CA-4409-BACB-6AB40D6D42E5)
  * INITIALIZING state  [ 11.4.1 ](managing-oracle-data-guard-logical-standby-databases.md#GUID-5BD79860-B3B8-4E0D-9F55-2E9918B93292)
  * INSTANTIATE_TABLE procedure 
    * of DBMS_LOGSTDBY  [ 11.5.5 ](managing-oracle-data-guard-logical-standby-databases.md#GUID-DD88E3D6-D6DB-4175-BB59-F048B9A87BAA)



* * *

##  K 

  * KEEP IDENTITY clause  [ 4.3.4.1 ](creating-oracle-data-guard-logical-standby.md#GUID-715EBEC1-21B9-4DA9-80A5-ED58033F94C8)



* * *

##  L 

  * latency 
    * on logical standby databases  [ 11.1.1.4 ](managing-oracle-data-guard-logical-standby-databases.md#GUID-97797605-D5BB-49B7-8E9D-D3E7C56A767C) , [ 11.1.1.5 ](managing-oracle-data-guard-logical-standby-databases.md#GUID-1BBCF0C2-E189-47EF-B7D1-805BF552E4DB)
  * listener.ora file 
    * redo transport services tuning and  [ A.7 ](troubleshooting-oracle-data-guard.md#GUID-5B879B01-2433-4B08-8D58-E91DBA58FEF7)
    * troubleshooting  [ A.1.2 ](troubleshooting-oracle-data-guard.md#GUID-E0F5B423-208F-4F7E-8CA4-0350D3A7CF0B) , [ A.7 ](troubleshooting-oracle-data-guard.md#GUID-5B879B01-2433-4B08-8D58-E91DBA58FEF7)
  * loading dictionary state  [ 11.4.1 ](managing-oracle-data-guard-logical-standby-databases.md#GUID-5BD79860-B3B8-4E0D-9F55-2E9918B93292)
  * LOCATION attribute  [ 17.8 ](LOG_ARCHIVE_DEST_n-parameter-attributes.md#GUID-9ACDEC6C-C2E0-4EDA-B66D-B1F819D3368D)
    * setting 
      * LOG_ARCHIVE_DEST_n initialization parameter  [ A.2 ](troubleshooting-oracle-data-guard.md#GUID-397ED7EA-1A1C-4224-82A3-746448BE6944)
  * LOG_ARCHIVE_CONFIG initialization parameter  [ 3.2.4 ](creating-oracle-data-guard-physical-standby.md#GUID-0FD317D6-1A49-4DB3-A141-D00CB3089E84) , [ 3.3.3 ](creating-oracle-data-guard-physical-standby.md#GUID-06FDC24C-6E90-420B-BB63-3B6BAC4D8AFA)
    * example  [ 17.4 ](LOG_ARCHIVE_DEST_n-parameter-attributes.md#GUID-59781B0B-B8EA-4676-9F93-A7A72E021D7A)
    * listing unique database names defined with  [ 19 ](oracle-data-guard-in-oracle-database-views.md#GUID-FF72163C-78B0-475F-B4CE-135EF288A32A)
    * relationship to DB_UNIQUE_NAME parameter  [ 16 ](oracle-initialization-parameters-used-by-oracle-data-guard.md#GUID-A1CF9B00-F0F5-4F3B-A0E0-9D6ABF775EC9)
  * LOG_ARCHIVE_DEST_n initialization parameter 
    * AFFIRM attribute  [ 17.1 ](LOG_ARCHIVE_DEST_n-parameter-attributes.md#GUID-559248E6-B153-4B7B-A8A9-9CEE102088F0)
    * ALTERNATE attribute  [ 17.2 ](LOG_ARCHIVE_DEST_n-parameter-attributes.md#GUID-88222D10-22FD-4F3B-BBE5-B59C8ABEC725) , [ A.2 ](troubleshooting-oracle-data-guard.md#GUID-397ED7EA-1A1C-4224-82A3-746448BE6944)
    * ASYNC attribute  [ 17.15 ](LOG_ARCHIVE_DEST_n-parameter-attributes.md#GUID-E3626025-2E4C-4C61-B5DF-2BB106F0A349)
    * COMPRESSION attribute  [ 17.3 ](LOG_ARCHIVE_DEST_n-parameter-attributes.md#GUID-DD58C7C2-EE9D-4632-94C9-F6C652718006)
    * DB_UNIQUE_NAME attribute  [ 17.4 ](LOG_ARCHIVE_DEST_n-parameter-attributes.md#GUID-59781B0B-B8EA-4676-9F93-A7A72E021D7A)
    * DELAY attribute  [ 8.2.2 ](oracle-data-guard-redo-apply-services.md#GUID-B891A164-1FDC-4AB3-838F-E6A91E7DA0B7) , [ 17.5 ](LOG_ARCHIVE_DEST_n-parameter-attributes.md#GUID-C014D9BB-79D3-4E49-9477-A5CBF0E88167)
    * deprecated attributes  [ 17 ](LOG_ARCHIVE_DEST_n-parameter-attributes.md#GUID-B04FC22C-F0B0-440F-BAF4-182EE547FCC1)
    * LOCATION attribute  [ 17.8 ](LOG_ARCHIVE_DEST_n-parameter-attributes.md#GUID-9ACDEC6C-C2E0-4EDA-B66D-B1F819D3368D) , [ A.2 ](troubleshooting-oracle-data-guard.md#GUID-397ED7EA-1A1C-4224-82A3-746448BE6944)
    * MANDATORY attribute  [ 17.9 ](LOG_ARCHIVE_DEST_n-parameter-attributes.md#GUID-C4AD4812-D4A6-41C9-8F03-4C87B64F1D4D)
    * MAX_FAILURE attribute  [ 17.10 ](LOG_ARCHIVE_DEST_n-parameter-attributes.md#GUID-3655C0B5-E669-4475-95B9-01A67AE6A144)
    * NET_TIMEOUT attribute  [ 17.11 ](LOG_ARCHIVE_DEST_n-parameter-attributes.md#GUID-5067F60F-780E-429E-A0FB-CB121B8DF321)
    * NOAFFIRM attribute  [ 17.1 ](LOG_ARCHIVE_DEST_n-parameter-attributes.md#GUID-559248E6-B153-4B7B-A8A9-9CEE102088F0)
    * NOALTERNATE attribute  [ A.2 ](troubleshooting-oracle-data-guard.md#GUID-397ED7EA-1A1C-4224-82A3-746448BE6944)
    * NODELAY attribute  [ 8.2.2 ](oracle-data-guard-redo-apply-services.md#GUID-B891A164-1FDC-4AB3-838F-E6A91E7DA0B7)
    * NOREGISTER attribute  [ 17.12 ](LOG_ARCHIVE_DEST_n-parameter-attributes.md#GUID-B23425C5-FED4-4084-9D82-A2B7888AE1E5)
    * REOPEN attribute  [ 17.14 ](LOG_ARCHIVE_DEST_n-parameter-attributes.md#GUID-AE01BD16-8FD7-4C4D-BBB6-156281C947A7)
    * SERVICE attribute  [ 17.8 ](LOG_ARCHIVE_DEST_n-parameter-attributes.md#GUID-9ACDEC6C-C2E0-4EDA-B66D-B1F819D3368D)
    * SYNC attribute  [ 17.15 ](LOG_ARCHIVE_DEST_n-parameter-attributes.md#GUID-E3626025-2E4C-4C61-B5DF-2BB106F0A349)
    * VALID_FOR attribute  [ 17.17 ](LOG_ARCHIVE_DEST_n-parameter-attributes.md#GUID-2E1845CC-B3CA-4409-BACB-6AB40D6D42E5)
  * LOG_ARCHIVE_MIN_SUCCEED_DEST initialization parameter  [ 17.9 ](LOG_ARCHIVE_DEST_n-parameter-attributes.md#GUID-C4AD4812-D4A6-41C9-8F03-4C87B64F1D4D)
  * LOG_FILE_NAME_CONVERT initialization parameter 
    * setting at standby site after a switchover  [ A.4.3 ](troubleshooting-oracle-data-guard.md#GUID-ACDE1813-1DBD-48E1-8C42-0A2E4ED36631) , [ G.3.3 ](performing-oracle-data-guard-role-transitions.md#GUID-F394098F-8726-45AC-A66F-B91402719D6A)
    * setting on physical standby databases  [ 3.3.3 ](creating-oracle-data-guard-physical-standby.md#GUID-06FDC24C-6E90-420B-BB63-3B6BAC4D8AFA)
    * when planning standby location and directory structure  [ 2.4 ](getting-started-with-oracle-data-guard.md#GUID-B59DBCF7-13A7-4F49-97E9-BF1BEF803699)
  * log apply services 
    * Redo Apply 
      * monitoring  [ 10.6.1 ](managing-oracle-data-guard-physical-standby-databases.md#GUID-567E9883-B1BE-431D-86E9-872BAB6A899F)
      * starting  [ 10.1.1 ](managing-oracle-data-guard-physical-standby-databases.md#GUID-76930886-6460-45AE-86CA-84DCB41E5BE5)
      * stopping  [ 10.1.2 ](managing-oracle-data-guard-physical-standby-databases.md#GUID-B51873F4-F50C-4757-9DF9-D43635BFAD7F)
    * tuning for Redo Apply  [ 10.8 ](managing-oracle-data-guard-physical-standby-databases.md#GUID-EEDAC95A-828F-4117-AF57-D8ABA702D42D)
  * logical change records (LCR) 
    * exhausted cache memory  [ 11.1.1.2 ](managing-oracle-data-guard-logical-standby-databases.md#GUID-E3ADF4B7-F08A-4F2B-A1CD-FB897ECCF534)
    * staged  [ 11.1 ](managing-oracle-data-guard-logical-standby-databases.md#GUID-735EED44-DFDB-43B8-ADBC-F1A3A62E7D93)
  * logical standby databases  [ 1.1.2 ](introduction-to-oracle-data-guard-concepts.md#GUID-C49AC6F4-C89B-4487-BC18-428D65865B9A)
    * adding 
      * datafiles  [ A.10.1.1 ](troubleshooting-oracle-data-guard.md#GUID-16CE2533-7190-4717-88E0-BDC53DBD887B)
      * indexes  [ 11.5.4.1 ](managing-oracle-data-guard-logical-standby-databases.md#GUID-FB0ABA4B-4EA4-458E-9223-B3BEE00A581A)
      * tables  [ 11.5.5 ](managing-oracle-data-guard-logical-standby-databases.md#GUID-DD88E3D6-D6DB-4175-BB59-F048B9A87BAA)
    * background processes  [ 11.1 ](managing-oracle-data-guard-logical-standby-databases.md#GUID-735EED44-DFDB-43B8-ADBC-F1A3A62E7D93)
    * benefits  [ 2.1.2 ](getting-started-with-oracle-data-guard.md#GUID-D6AB6955-C1E1-4FE9-A9D9-6F6275B8748A)
    * controlling user access to tables  [ 11.2 ](managing-oracle-data-guard-logical-standby-databases.md#GUID-572B9755-69F5-4BD7-9DFF-1F7EB1DE3771)
    * creating  [ 4 ](creating-oracle-data-guard-logical-standby.md#GUID-3666CA35-D993-44B6-8D70-A2B8B9EC8B2E)
      * converting from a physical standby database  [ 4.3.4.1 ](creating-oracle-data-guard-logical-standby.md#GUID-715EBEC1-21B9-4DA9-80A5-ED58033F94C8)
      * with Data Guard broker  [ 1.4 ](introduction-to-oracle-data-guard-concepts.md#GUID-538B9DDD-1553-479D-8E1D-0B5C6848403E)
    * database guard 
      * overriding  [ 11.5.4 ](managing-oracle-data-guard-logical-standby-databases.md#GUID-43971CB9-3337-4575-B4EF-D591878982F1)
    * data types 
      * supported  [ C ](data-type-ddl-support-on-logical-standby-databases.md#GUID-340BA6D6-74D5-4684-AF39-08890D5CD9D4) , [ C.1.1 ](data-type-ddl-support-on-logical-standby-databases.md#GUID-8FED6166-FB83-42E4-8D2E-8A1F7DFCEB00)
      * unsupported  [ C.1.2 ](data-type-ddl-support-on-logical-standby-databases.md#GUID-76E70ADB-0709-4403-BA6A-379CEAFE758A)
    * executing SQL statements on  [ 1.1.2 ](introduction-to-oracle-data-guard-concepts.md#GUID-C49AC6F4-C89B-4487-BC18-428D65865B9A)
    * failovers  [ 9.3.2 ](managing-oracle-data-guard-role-transitions.md#GUID-EBD0C21B-6356-446E-8BF5-7B7D96554713)
      * displaying history of  [ 19 ](oracle-data-guard-in-oracle-database-views.md#GUID-FF72163C-78B0-475F-B4CE-135EF288A32A)
      * handling failures  [ A.3 ](troubleshooting-oracle-data-guard.md#GUID-3BC98594-CFA0-4EDC-92B9-52F19088B224)
      * viewing characteristics with V$LOGSTDBY_STATS  [ 11.3.3 ](managing-oracle-data-guard-logical-standby-databases.md#GUID-76033857-52EC-41B2-B130-CC9083F3FA99)
    * logical standby process (LSP) and  [ 11.1 ](managing-oracle-data-guard-logical-standby-databases.md#GUID-735EED44-DFDB-43B8-ADBC-F1A3A62E7D93)
    * materialized views 
      * support for  [ C.15 ](data-type-ddl-support-on-logical-standby-databases.md#GUID-9DBC35BD-43C9-469F-BADD-C91752E88225)
    * monitoring  [ 8.4.3 ](oracle-data-guard-redo-apply-services.md#GUID-8A34CBE4-AD50-4918-AB5F-671CF9A66B20) , [ 19 ](oracle-data-guard-in-oracle-database-views.md#GUID-FF72163C-78B0-475F-B4CE-135EF288A32A)
    * renaming the file specification  [ 11.5.3 ](managing-oracle-data-guard-logical-standby-databases.md#GUID-28EB46A0-696D-426E-B596-225443867604)
    * setting up a skip handler  [ 11.5.3 ](managing-oracle-data-guard-logical-standby-databases.md#GUID-28EB46A0-696D-426E-B596-225443867604)
    * SQL Apply  [ 1.3.2 ](introduction-to-oracle-data-guard-concepts.md#GUID-D284E617-3F4F-4128-98F5-A3EB88772BF5)
      * resynchronizing with primary database branch of redo  [ 11.6.5 ](managing-oracle-data-guard-logical-standby-databases.md#GUID-44D7C275-A991-4B6F-84A2-D76085E00367)
      * skipping DDL statements  [ C.15 ](data-type-ddl-support-on-logical-standby-databases.md#GUID-9DBC35BD-43C9-469F-BADD-C91752E88225)
      * skipping SQL statements  [ C.15 ](data-type-ddl-support-on-logical-standby-databases.md#GUID-9DBC35BD-43C9-469F-BADD-C91752E88225)
      * starting real-time apply  [ 8.4.1 ](oracle-data-guard-redo-apply-services.md#GUID-5E640AEE-12C7-4ED8-8C42-B989F7768E91)
      * stopping  [ 8.4.2 ](oracle-data-guard-redo-apply-services.md#GUID-C7B53D2A-5D38-4BA5-8142-98A81691A415)
      * technology  [ 8.1 ](oracle-data-guard-redo-apply-services.md#GUID-74C8C1F3-70FC-4543-B958-83F309B6B01B)
      * transaction size considerations  [ 11.1.1.1 ](managing-oracle-data-guard-logical-standby-databases.md#GUID-344AF03D-88A6-4DE1-8650-D5F1B9781FC3)
    * starting 
      * real-time apply  [ 8.4.1 ](oracle-data-guard-redo-apply-services.md#GUID-5E640AEE-12C7-4ED8-8C42-B989F7768E91)
    * states 
      * applying  [ 11.4.1 ](managing-oracle-data-guard-logical-standby-databases.md#GUID-5BD79860-B3B8-4E0D-9F55-2E9918B93292)
      * idle  [ 11.4.1 ](managing-oracle-data-guard-logical-standby-databases.md#GUID-5BD79860-B3B8-4E0D-9F55-2E9918B93292)
      * initializing  [ 11.4.1 ](managing-oracle-data-guard-logical-standby-databases.md#GUID-5BD79860-B3B8-4E0D-9F55-2E9918B93292)
      * loading dictionary  [ 11.4.1 ](managing-oracle-data-guard-logical-standby-databases.md#GUID-5BD79860-B3B8-4E0D-9F55-2E9918B93292)
    * support for primary databases with Transparent Data Encryption  [ C.3 ](data-type-ddl-support-on-logical-standby-databases.md#GUID-CE6B667C-46EE-48C5-94AA-C96B98EB4504)
    * switchovers  [ 9.3.1 ](managing-oracle-data-guard-role-transitions.md#GUID-42B8B448-90E0-4D7D-82A3-AAA273E6E49B)
    * throughput and latency  [ 11.1.1.4 ](managing-oracle-data-guard-logical-standby-databases.md#GUID-97797605-D5BB-49B7-8E9D-D3E7C56A767C) , [ 11.1.1.5 ](managing-oracle-data-guard-logical-standby-databases.md#GUID-1BBCF0C2-E189-47EF-B7D1-805BF552E4DB)
    * upgrading  [ B.4 ](upgrading-patching-downgrading-oracle-data-guard-configuration.md#GUID-5AF5983E-E481-4599-88B9-311A43EB5C80)
      * rolling upgrades  [ 2.3.2 ](getting-started-with-oracle-data-guard.md#GUID-A9F7D87E-49EB-4CE9-A076-1F35F35C0EAB)
  * logical standby process (LSP) 
    * COORDINATOR process  [ 11.1 ](managing-oracle-data-guard-logical-standby-databases.md#GUID-735EED44-DFDB-43B8-ADBC-F1A3A62E7D93)
  * LogMiner dictionary 
    * using DBMS_LOGSTDBY.BUILD procedure to build  [ 4.3.3.2 ](creating-oracle-data-guard-logical-standby.md#GUID-E5D227F2-C456-44F3-AEEA-2C8340439EF3)
    * when creating a logical standby database  [ 4.3.4.1 ](creating-oracle-data-guard-logical-standby.md#GUID-715EBEC1-21B9-4DA9-80A5-ED58033F94C8)
  * log writer process (LGWR) 
    * ASYNC network transmission  [ 17.15 ](LOG_ARCHIVE_DEST_n-parameter-attributes.md#GUID-E3626025-2E4C-4C61-B5DF-2BB106F0A349)
    * NET_TIMEOUT attribute  [ 17.11 ](LOG_ARCHIVE_DEST_n-parameter-attributes.md#GUID-5067F60F-780E-429E-A0FB-CB121B8DF321)
    * SYNC network transmission  [ 17.15 ](LOG_ARCHIVE_DEST_n-parameter-attributes.md#GUID-E3626025-2E4C-4C61-B5DF-2BB106F0A349)



* * *

##  M 

  * managed recovery operations 
    * See:  Redo Apply 
  * MANDATORY attribute  [ 17.9 ](LOG_ARCHIVE_DEST_n-parameter-attributes.md#GUID-C4AD4812-D4A6-41C9-8F03-4C87B64F1D4D)
  * MAX_FAILURE attribute  [ 17.10 ](LOG_ARCHIVE_DEST_n-parameter-attributes.md#GUID-3655C0B5-E669-4475-95B9-01A67AE6A144)
  * maximum availability mode 
    * introduction  [ 1.5 ](introduction-to-oracle-data-guard-concepts.md#GUID-65DA4E2E-3666-4972-895C-EA45D043B127)
  * maximum performance mode  [ 9.1.4 ](managing-oracle-data-guard-role-transitions.md#GUID-3E6BE44A-3077-4E67-86E7-B641DBF30083)
    * introduction  [ 1.5 ](introduction-to-oracle-data-guard-concepts.md#GUID-65DA4E2E-3666-4972-895C-EA45D043B127)
  * maximum performance protection mode  [ 6.1 ](oracle-data-guard-protection-modes.md#GUID-7EF6EFEE-7E31-4F80-9C97-1B25DAA025F8)
  * maximum protection mode 
    * for Oracle Real Application Clusters  [ D.2.2 ](configuring-data-guard-standby-databases-in-oracle-RAC.md#GUID-7920893D-D3D2-44F4-A640-F68B6D516B1C)
    * introduction  [ 1.5 ](introduction-to-oracle-data-guard-concepts.md#GUID-65DA4E2E-3666-4972-895C-EA45D043B127)
    * standby databases and  [ 9.1.4 ](managing-oracle-data-guard-role-transitions.md#GUID-3E6BE44A-3077-4E67-86E7-B641DBF30083)
  * memory 
    * exhausted LCR cache  [ 11.1.1.2 ](managing-oracle-data-guard-logical-standby-databases.md#GUID-E3ADF4B7-F08A-4F2B-A1CD-FB897ECCF534)
  * missing log sequence 
    * detecting  [ 1.9 ](introduction-to-oracle-data-guard-concepts.md#GUID-C65BF458-4BF7-4723-BAFC-5860B5B88A2B)
    * See also gap management  [ 1.9 ](introduction-to-oracle-data-guard-concepts.md#GUID-C65BF458-4BF7-4723-BAFC-5860B5B88A2B)
  * modifying 
    * a logical standby database  [ 11.5.4 ](managing-oracle-data-guard-logical-standby-databases.md#GUID-43971CB9-3337-4575-B4EF-D591878982F1)
  * monitoring 
    * primary database events  [ 10.6 ](managing-oracle-data-guard-physical-standby-databases.md#GUID-0682EE24-B495-48E8-9E1F-52AC0E6FE486)
    * tablespace status  [ 10.6 ](managing-oracle-data-guard-physical-standby-databases.md#GUID-0682EE24-B495-48E8-9E1F-52AC0E6FE486)
  * MOUNT STANDBY DATABASE clause 
    * of ALTER DATABASE  [ 18.1 ](sql-statements-used-by-oracle-data-guard.md#GUID-F690B602-6582-465B-8E86-0B0A52838D66)
  * multitenant container databases 
    * See CDBs  [ 4.1 ](creating-oracle-data-guard-logical-standby.md#GUID-E2BB4836-32D1-4D57-9B32-75B1239170E2)



* * *

##  N 

  * NET_TIMEOUT attribute  [ 17.11 ](LOG_ARCHIVE_DEST_n-parameter-attributes.md#GUID-5067F60F-780E-429E-A0FB-CB121B8DF321)
  * network I/O operations 
    * network timers 
      * NET_TIMEOUT attribute  [ 17.11 ](LOG_ARCHIVE_DEST_n-parameter-attributes.md#GUID-5067F60F-780E-429E-A0FB-CB121B8DF321)
    * tuning 
      * redo transport services  [ A.7 ](troubleshooting-oracle-data-guard.md#GUID-5B879B01-2433-4B08-8D58-E91DBA58FEF7)
  * network timeouts 
    * acknowledging  [ 17.11 ](LOG_ARCHIVE_DEST_n-parameter-attributes.md#GUID-5067F60F-780E-429E-A0FB-CB121B8DF321)
  * NOAFFIRM attribute  [ 17.1 ](LOG_ARCHIVE_DEST_n-parameter-attributes.md#GUID-559248E6-B153-4B7B-A8A9-9CEE102088F0)
  * NOALTERNATE attribute 
    * LOG_ARCHIVE_DEST_n initialization parameter  [ A.2 ](troubleshooting-oracle-data-guard.md#GUID-397ED7EA-1A1C-4224-82A3-746448BE6944)
  * no data loss 
    * data protection modes overview  [ 1.5 ](introduction-to-oracle-data-guard-concepts.md#GUID-65DA4E2E-3666-4972-895C-EA45D043B127)
    * ensuring  [ 1.3.3 ](introduction-to-oracle-data-guard-concepts.md#GUID-EFAC6E17-BCE5-417C-9124-1595DBF0ABFA)
    * guaranteeing  [ 1.3.3 ](introduction-to-oracle-data-guard-concepts.md#GUID-EFAC6E17-BCE5-417C-9124-1595DBF0ABFA)
    * provided by maximum availability mode  [ 1.5 ](introduction-to-oracle-data-guard-concepts.md#GUID-65DA4E2E-3666-4972-895C-EA45D043B127)
    * provided by maximum protection mode  [ 1.5 ](introduction-to-oracle-data-guard-concepts.md#GUID-65DA4E2E-3666-4972-895C-EA45D043B127)
  * NODELAY attribute 
    * LOG_ARCHIVE_DEST_n initialization parameter  [ 8.2.2 ](oracle-data-guard-redo-apply-services.md#GUID-B891A164-1FDC-4AB3-838F-E6A91E7DA0B7)
  * NOREGISTER attribute  [ 17.12 ](LOG_ARCHIVE_DEST_n-parameter-attributes.md#GUID-B23425C5-FED4-4084-9D82-A2B7888AE1E5)



* * *

##  O 

  * OMF 
    * See Oracle Managed Files (OMF)  [ 15.5 ](examples-of-using-oracle-data-guard.md#GUID-6FB574E5-2F3A-4C9E-A3F0-7C9154B0F540)
  * on-disk database structures 
    * physical standby databases  [ 1.1.2 ](introduction-to-oracle-data-guard-concepts.md#GUID-C49AC6F4-C89B-4487-BC18-428D65865B9A)
  * online data files 
    * moving the location  [ 2.5 ](getting-started-with-oracle-data-guard.md#GUID-8DA7295A-53C7-4B52-9E0A-8A513640D265)
  * online redo log files 
    * adding  [ 10.3.5 ](managing-oracle-data-guard-physical-standby-databases.md#GUID-64E46731-490D-4228-AC87-A07CDE3C75FE)
    * dropping  [ 10.3.5 ](managing-oracle-data-guard-physical-standby-databases.md#GUID-64E46731-490D-4228-AC87-A07CDE3C75FE)
  * OPEN READ ONLY clause 
    * of ALTER DATABASE  [ 18.1 ](sql-statements-used-by-oracle-data-guard.md#GUID-F690B602-6582-465B-8E86-0B0A52838D66)
  * OPEN RESETLOGS 
    * flashing back after  [ 15.3 ](examples-of-using-oracle-data-guard.md#GUID-BAA5ED38-29A0-4E8D-8435-AA083D19C13E)
  * OPEN RESETLOGS clause 
    * database incarnation change  [ 10.4 ](managing-oracle-data-guard-physical-standby-databases.md#GUID-02218512-31A6-461E-96B4-0B9997DCFC4A)
    * recovery  [ 10.4 ](managing-oracle-data-guard-physical-standby-databases.md#GUID-02218512-31A6-461E-96B4-0B9997DCFC4A)
  * operational requirements  [ 2.3 ](getting-started-with-oracle-data-guard.md#GUID-62F35117-0A9C-47E9-82B6-7E3B214B28B7) , [ 2.3.2 ](getting-started-with-oracle-data-guard.md#GUID-A9F7D87E-49EB-4CE9-A076-1F35F35C0EAB)
  * Optimal Flexible Architecture (OFA) 
    * directory structure  [ 2.4 ](getting-started-with-oracle-data-guard.md#GUID-B59DBCF7-13A7-4F49-97E9-BF1BEF803699)
  * ORA-01102 message 
    * causing switchover failures  [ A.4.2 ](troubleshooting-oracle-data-guard.md#GUID-2BE05596-2E74-4FEE-84AB-81777B5B9E13) , [ G.3.2 ](performing-oracle-data-guard-role-transitions.md#GUID-A9CF13FE-3E89-4FB3-B2BF-6536DCAD3ADB)
  * Oracle Database software 
    * requirements for upgrading with SQL Apply  [ 13.2 ](using-sql-apply-to-perform-rolling-upgrade.md#GUID-2CF3E283-2EC3-4B55-A71B-9D25DDF52156)
    * upgrading  [ 2.3.2 ](getting-started-with-oracle-data-guard.md#GUID-A9F7D87E-49EB-4CE9-A076-1F35F35C0EAB) , [ B.1 ](upgrading-patching-downgrading-oracle-data-guard-configuration.md#GUID-784C06D8-9CFA-4244-BA55-D7003CAB3F1E)
    * upgrading with SQL Apply  [ 13.1 ](using-sql-apply-to-perform-rolling-upgrade.md#GUID-701A35C5-B545-43DD-8854-F6948E0BB32A)
  * Oracle Enterprise Manager 
    * invoking failovers  [ 1.4 ](introduction-to-oracle-data-guard-concepts.md#GUID-538B9DDD-1553-479D-8E1D-0B5C6848403E) , [ 9 ](managing-oracle-data-guard-role-transitions.md#GUID-44F26D5F-E036-4ACD-B8F0-224F5498FBCE)
    * invoking switchovers  [ 1.4 ](introduction-to-oracle-data-guard-concepts.md#GUID-538B9DDD-1553-479D-8E1D-0B5C6848403E) , [ 9 ](managing-oracle-data-guard-role-transitions.md#GUID-44F26D5F-E036-4ACD-B8F0-224F5498FBCE)
  * Oracle Managed Files (OMF) 
    * creating a standby database that uses  [ 15.5 ](examples-of-using-oracle-data-guard.md#GUID-6FB574E5-2F3A-4C9E-A3F0-7C9154B0F540)
  * Oracle RAC One Node 
    * supported by Oracle Data Guard  [ 1.7 ](introduction-to-oracle-data-guard-concepts.md#GUID-9EEFD8E5-901F-4092-85F4-2E0069AC5923)
  * Oracle Real Application Clusters 
    * primary databases and  [ 1.1.1 ](introduction-to-oracle-data-guard-concepts.md#GUID-0472F1B0-C2AA-4E9D-8773-1A7FBE27DC82) , [ D.1.2 ](configuring-data-guard-standby-databases-in-oracle-RAC.md#GUID-EBCDC4F9-B0C7-49C7-B04C-AD4DF60F3D26)
    * setting 
      * maximum data protection  [ D.2.2 ](configuring-data-guard-standby-databases-in-oracle-RAC.md#GUID-7920893D-D3D2-44F4-A640-F68B6D516B1C)
    * standby databases and  [ 1.1.2 ](introduction-to-oracle-data-guard-concepts.md#GUID-C49AC6F4-C89B-4487-BC18-428D65865B9A) , [ D.1 ](configuring-data-guard-standby-databases-in-oracle-RAC.md#GUID-3140A293-DDD8-4559-8493-B6C21646E90F)
  * Oracle Recovery Manager utility (RMAN) 
    * backing up files on a physical standby database  [ 12 ](using-RMAN-in-oracle-data-guard-configurations.md#GUID-516D9C60-F2EF-4822-BB87-5D3BE877B69B)
  * Oracle Sharding 
    * supported by Oracle Active Data Guard  [ 1.8 ](introduction-to-oracle-data-guard-concepts.md#GUID-858549EF-B72F-4545-A5D1-C632C8ECC961)
  * Oracle Standard Edition 
    * simulating a standby database environment  [ 2.3.2 ](getting-started-with-oracle-data-guard.md#GUID-A9F7D87E-49EB-4CE9-A076-1F35F35C0EAB)



* * *

##  P 

  * pageout considerations  [ 11.1.1.2 ](managing-oracle-data-guard-logical-standby-databases.md#GUID-E3ADF4B7-F08A-4F2B-A1CD-FB897ECCF534)
  * pageouts 
    * SQL Apply  [ 11.1.1.2 ](managing-oracle-data-guard-logical-standby-databases.md#GUID-E3ADF4B7-F08A-4F2B-A1CD-FB897ECCF534)
  * parallel DML (PDML) transactions 
    * SQL Apply  [ 11.1.1.3 ](managing-oracle-data-guard-logical-standby-databases.md#GUID-F0FDEEF3-B4EA-40F3-BF7B-2167D33D91C8) , [ 11.1.1.4 ](managing-oracle-data-guard-logical-standby-databases.md#GUID-97797605-D5BB-49B7-8E9D-D3E7C56A767C)
  * patch set releases 
    * upgrading  [ 2.3.2 ](getting-started-with-oracle-data-guard.md#GUID-A9F7D87E-49EB-4CE9-A076-1F35F35C0EAB)
  * performance 
    * balancing against data availability  [ 1.9 ](introduction-to-oracle-data-guard-concepts.md#GUID-C65BF458-4BF7-4723-BAFC-5860B5B88A2B)
    * balancing against data protection  [ 1.9 ](introduction-to-oracle-data-guard-concepts.md#GUID-C65BF458-4BF7-4723-BAFC-5860B5B88A2B)
  * physical standby databases 
    * and Oracle Active Data Guard  [ 2.1.1 ](getting-started-with-oracle-data-guard.md#GUID-88BBC338-BC93-454C-B5C6-23CA8F0C329E)
    * applying redo data  [ 8.1 ](oracle-data-guard-redo-apply-services.md#GUID-74C8C1F3-70FC-4543-B958-83F309B6B01B) , [ 8.3 ](oracle-data-guard-redo-apply-services.md#GUID-2B839172-947E-48A4-9FFD-33CC6907809F)
      * Redo Apply technology  [ 8.3 ](oracle-data-guard-redo-apply-services.md#GUID-2B839172-947E-48A4-9FFD-33CC6907809F)
    * applying redo log files 
      * starting  [ 8.3.1 ](oracle-data-guard-redo-apply-services.md#GUID-54FA44F8-1EDA-4CAF-8B04-90CC6108FED8)
    * benefits  [ 2.1.1 ](getting-started-with-oracle-data-guard.md#GUID-88BBC338-BC93-454C-B5C6-23CA8F0C329E)
    * configuration options  [ 2.4 ](getting-started-with-oracle-data-guard.md#GUID-B59DBCF7-13A7-4F49-97E9-BF1BEF803699)
    * converting datafile path names  [ 3.3.3 ](creating-oracle-data-guard-physical-standby.md#GUID-06FDC24C-6E90-420B-BB63-3B6BAC4D8AFA)
    * converting log file path names  [ 3.3.3 ](creating-oracle-data-guard-physical-standby.md#GUID-06FDC24C-6E90-420B-BB63-3B6BAC4D8AFA)
    * converting to a logical standby database  [ 4.3.4.1 ](creating-oracle-data-guard-logical-standby.md#GUID-715EBEC1-21B9-4DA9-80A5-ED58033F94C8)
    * creating 
      * checklist of tasks  [ 3.3 ](creating-oracle-data-guard-physical-standby.md#GUID-A86D403B-B3F5-4B05-9255-5BADD513F9CF)
      * configuring a listener  [ 5.1.1 ](creating-oracle-data-guard-far-sync-instance.md#GUID-43FE645C-1383-44F1-9127-7EFB2A402DB3)
      * directory structure  [ 2.4 ](getting-started-with-oracle-data-guard.md#GUID-B59DBCF7-13A7-4F49-97E9-BF1BEF803699)
      * with Data Guard broker  [ 1.4 ](introduction-to-oracle-data-guard-concepts.md#GUID-538B9DDD-1553-479D-8E1D-0B5C6848403E)
    * defined  [ 1.1.2 ](introduction-to-oracle-data-guard-concepts.md#GUID-C49AC6F4-C89B-4487-BC18-428D65865B9A)
    * failover 
      * checking for updates  [ 9.1.4 ](managing-oracle-data-guard-role-transitions.md#GUID-3E6BE44A-3077-4E67-86E7-B641DBF30083)
    * flashing back after failover  [ 15.2.1 ](examples-of-using-oracle-data-guard.md#GUID-C7C39BEC-A841-48D1-BE4B-9BB49C65B9C9)
    * monitoring  [ 8.3.3 ](oracle-data-guard-redo-apply-services.md#GUID-F9964184-5E31-4373-A24E-318AD5C511F5) , [ 10.6.1 ](managing-oracle-data-guard-physical-standby-databases.md#GUID-567E9883-B1BE-431D-86E9-872BAB6A899F) , [ 19 ](oracle-data-guard-in-oracle-database-views.md#GUID-FF72163C-78B0-475F-B4CE-135EF288A32A)
    * opening for read-only or read/write access  [ 10.2 ](managing-oracle-data-guard-physical-standby-databases.md#GUID-D5FB88EC-799D-40E7-80E1-19474E3167E4)
    * read-only  [ 10.2 ](managing-oracle-data-guard-physical-standby-databases.md#GUID-D5FB88EC-799D-40E7-80E1-19474E3167E4)
    * recovering through OPEN RESETLOGS  [ 10.4 ](managing-oracle-data-guard-physical-standby-databases.md#GUID-02218512-31A6-461E-96B4-0B9997DCFC4A)
    * Redo Apply  [ 1.3.2 ](introduction-to-oracle-data-guard-concepts.md#GUID-D284E617-3F4F-4128-98F5-A3EB88772BF5)
    * resynchronizing with primary database branch of redo  [ 10.4 ](managing-oracle-data-guard-physical-standby-databases.md#GUID-02218512-31A6-461E-96B4-0B9997DCFC4A)
    * role transition and  [ 9.2 ](managing-oracle-data-guard-role-transitions.md#GUID-857F6F45-DC1C-4345-BD39-F3BE7D79F1CD) , [ G ](performing-oracle-data-guard-role-transitions.md#GUID-6E78412B-8E55-4D8B-A318-9F0612376E86)
    * rolling forward with BACKUP INCREMENTAL FROM SCN command  [ 12.9.1 ](using-RMAN-in-oracle-data-guard-configurations.md#GUID-1F1D37FE-48A6-46D4-98DB-C0E3707E3BCF) , [ 12.11 ](using-RMAN-in-oracle-data-guard-configurations.md#GUID-F0EB4F8E-7D2F-4674-9A5B-AABC368D8F11)
    * shutting down  [ 10.1.2 ](managing-oracle-data-guard-physical-standby-databases.md#GUID-B51873F4-F50C-4757-9DF9-D43635BFAD7F)
    * starting 
      * apply services  [ 8.3.1 ](oracle-data-guard-redo-apply-services.md#GUID-54FA44F8-1EDA-4CAF-8B04-90CC6108FED8)
    * tuning the log apply rate  [ 10.8 ](managing-oracle-data-guard-physical-standby-databases.md#GUID-EEDAC95A-828F-4117-AF57-D8ABA702D42D)
    * upgrading  [ B.3 ](upgrading-patching-downgrading-oracle-data-guard-configuration.md#GUID-6431EFBF-6B00-4E7D-A106-63574225BDD9)
    * using transportable tablespaces  [ 10.3.3 ](managing-oracle-data-guard-physical-standby-databases.md#GUID-88158C9B-51FB-4F13-93FC-97D737C86BF0)
  * PL/SQL supplied packages 
    * supported  [ C.13.1 ](data-type-ddl-support-on-logical-standby-databases.md#GUID-E89A196F-2C5D-45F9-94BA-7793FBF44440)
    * unsupported  [ C.13.2 ](data-type-ddl-support-on-logical-standby-databases.md#GUID-703C8569-4298-48ED-98F5-6146A6D46F3F)
  * PREPARER process 
    * staging LCRs in SGA  [ 11.1 ](managing-oracle-data-guard-logical-standby-databases.md#GUID-735EED44-DFDB-43B8-ADBC-F1A3A62E7D93)
  * PREPARE TO SWITCHOVER clause 
    * of ALTER DATABASE  [ 9.3.1 ](managing-oracle-data-guard-role-transitions.md#GUID-42B8B448-90E0-4D7D-82A3-AAA273E6E49B) , [ 18.1 ](sql-statements-used-by-oracle-data-guard.md#GUID-F690B602-6582-465B-8E86-0B0A52838D66)
  * primary database 
    * configuring 
      * on Oracle Real Application Clusters  [ 1.1.1 ](introduction-to-oracle-data-guard-concepts.md#GUID-0472F1B0-C2AA-4E9D-8773-1A7FBE27DC82)
      * single-instance  [ 1.1.1 ](introduction-to-oracle-data-guard-concepts.md#GUID-0472F1B0-C2AA-4E9D-8773-1A7FBE27DC82)
    * datafiles 
      * adding  [ 10.3.1 ](managing-oracle-data-guard-physical-standby-databases.md#GUID-C60E4DB4-F424-454D-B4DF-7A97B2F09540)
    * defined  [ 1.1.1 ](introduction-to-oracle-data-guard-concepts.md#GUID-0472F1B0-C2AA-4E9D-8773-1A7FBE27DC82)
    * gap resolution  [ 1.9 ](introduction-to-oracle-data-guard-concepts.md#GUID-C65BF458-4BF7-4723-BAFC-5860B5B88A2B)
    * monitoring events on  [ 10.6 ](managing-oracle-data-guard-physical-standby-databases.md#GUID-0682EE24-B495-48E8-9E1F-52AC0E6FE486)
    * network connections 
      * avoiding network hangs  [ 17.11 ](LOG_ARCHIVE_DEST_n-parameter-attributes.md#GUID-5067F60F-780E-429E-A0FB-CB121B8DF321)
      * handling network timeouts  [ 17.11 ](LOG_ARCHIVE_DEST_n-parameter-attributes.md#GUID-5067F60F-780E-429E-A0FB-CB121B8DF321)
    * Oracle Real Application Clusters and 
      * setting up  [ D.1.2 ](configuring-data-guard-standby-databases-in-oracle-RAC.md#GUID-EBCDC4F9-B0C7-49C7-B04C-AD4DF60F3D26)
    * preparing for 
      * physical standby database creation  [ 3.2 ](creating-oracle-data-guard-physical-standby.md#GUID-3DA747A3-1976-4A70-A96A-56281872D4D9)
    * prerequisite conditions for 
      * logical standby database creation  [ 4.2 ](creating-oracle-data-guard-logical-standby.md#GUID-ACF65D75-F057-4C09-9D59-DFAE7D647579)
    * redo transport services on  [ 1.3.1 ](introduction-to-oracle-data-guard-concepts.md#GUID-05D621EE-51AF-41E4-9CFC-45AEBAB08F57)
    * reducing workload on  [ 1.9 ](introduction-to-oracle-data-guard-concepts.md#GUID-C65BF458-4BF7-4723-BAFC-5860B5B88A2B)
    * restore points 
      * replicating to standby  [ 10.7 ](managing-oracle-data-guard-physical-standby-databases.md#GUID-45D7D60E-ED0F-4B3E-9D32-EB5394196CAF)
    * switchover  [ 9.1.3 ](managing-oracle-data-guard-role-transitions.md#GUID-E86F0570-A207-4F14-AFE4-81B537B96AFD)
    * tablespaces 
      * adding  [ 10.3.1 ](managing-oracle-data-guard-physical-standby-databases.md#GUID-C60E4DB4-F424-454D-B4DF-7A97B2F09540)
  * primary databases 
    * ARCHIVELOG mode  [ 2.3.2 ](getting-started-with-oracle-data-guard.md#GUID-A9F7D87E-49EB-4CE9-A076-1F35F35C0EAB)
    * software requirements  [ 2.3.2 ](getting-started-with-oracle-data-guard.md#GUID-A9F7D87E-49EB-4CE9-A076-1F35F35C0EAB)
  * primary key columns 
    * logged with supplemental logging  [ 4.3.3.2 ](creating-oracle-data-guard-logical-standby.md#GUID-E5D227F2-C456-44F3-AEEA-2C8340439EF3) , [ 11.1.1.4 ](managing-oracle-data-guard-logical-standby-databases.md#GUID-97797605-D5BB-49B7-8E9D-D3E7C56A767C)
  * private temporary tables 
    * using with Active Data Guard  [ 10.2.3.2 ](managing-oracle-data-guard-physical-standby-databases.md#GUID-92A71AA4-BEEB-4C2B-B663-FC6641059E39)
  * processes 
    * SQL Apply architecture  [ 11.1 ](managing-oracle-data-guard-logical-standby-databases.md#GUID-735EED44-DFDB-43B8-ADBC-F1A3A62E7D93) , [ 11.4.1 ](managing-oracle-data-guard-logical-standby-databases.md#GUID-5BD79860-B3B8-4E0D-9F55-2E9918B93292)
  * protection modes 
    * maximum availability mode  [ 1.5 ](introduction-to-oracle-data-guard-concepts.md#GUID-65DA4E2E-3666-4972-895C-EA45D043B127)
    * maximum performance  [ 6.1 ](oracle-data-guard-protection-modes.md#GUID-7EF6EFEE-7E31-4F80-9C97-1B25DAA025F8)
    * maximum performance mode  [ 1.5 ](introduction-to-oracle-data-guard-concepts.md#GUID-65DA4E2E-3666-4972-895C-EA45D043B127)
    * maximum protection mode  [ 1.5 ](introduction-to-oracle-data-guard-concepts.md#GUID-65DA4E2E-3666-4972-895C-EA45D043B127)
    * monitoring  [ 10.6 ](managing-oracle-data-guard-physical-standby-databases.md#GUID-0682EE24-B495-48E8-9E1F-52AC0E6FE486)
    * setting on a primary database  [ 6.2 ](oracle-data-guard-protection-modes.md#GUID-5DB32C5F-3ABF-4AD4-AB41-208F1BF134BB)



* * *

##  Q 

  * queries 
    * offloading on the standby database  [ 1.9 ](introduction-to-oracle-data-guard-concepts.md#GUID-C65BF458-4BF7-4723-BAFC-5860B5B88A2B)



* * *

##  R 

  * read-only operations  [ 1.3.2 ](introduction-to-oracle-data-guard-concepts.md#GUID-D284E617-3F4F-4128-98F5-A3EB88772BF5)
    * physical standby databases and  [ 10.2 ](managing-oracle-data-guard-physical-standby-databases.md#GUID-D5FB88EC-799D-40E7-80E1-19474E3167E4)
  * real-time apply 
    * defined  [ 8.2.1 ](oracle-data-guard-redo-apply-services.md#GUID-9922F0E7-3E19-449E-8C35-57B7E5714C54)
    * starting 
      * on logical standby  [ 8.4.1 ](oracle-data-guard-redo-apply-services.md#GUID-5E640AEE-12C7-4ED8-8C42-B989F7768E91)
    * starting on logical standby databases  [ 8.4.1 ](oracle-data-guard-redo-apply-services.md#GUID-5E640AEE-12C7-4ED8-8C42-B989F7768E91)
    * stopping 
      * on logical standby  [ 8.4.2 ](oracle-data-guard-redo-apply-services.md#GUID-C7B53D2A-5D38-4BA5-8142-98A81691A415)
      * on physical standby databases  [ 10.1.2 ](managing-oracle-data-guard-physical-standby-databases.md#GUID-B51873F4-F50C-4757-9DF9-D43635BFAD7F)
  * real-time cascading  [ 7.3 ](oracle-data-guard-redo-transport-services.md#GUID-34BCB162-D996-4678-97F1-497805764950)
  * real-time query feature  [ 10.2 ](managing-oracle-data-guard-physical-standby-databases.md#GUID-D5FB88EC-799D-40E7-80E1-19474E3167E4)
    * and Oracle Active Data Guard  [ 10.2 ](managing-oracle-data-guard-physical-standby-databases.md#GUID-D5FB88EC-799D-40E7-80E1-19474E3167E4) , [ 10.2.1 ](managing-oracle-data-guard-physical-standby-databases.md#GUID-07CB190C-C248-4FF5-AB64-EAA9C6D42677)
    * configuring apply lag tolerance  [ 10.2.1.2 ](managing-oracle-data-guard-physical-standby-databases.md#GUID-622B6596-54F7-4736-8A8B-0C5E308AC3F5)
    * forcing Redo Apply synchronization  [ 10.2.1.3 ](managing-oracle-data-guard-physical-standby-databases.md#GUID-194B1448-84FB-4357-A6AD-72A77701F918)
    * monitoring apply lag  [ 10.2.1.1 ](managing-oracle-data-guard-physical-standby-databases.md#GUID-C225DCBC-E9FB-4E86-9D19-A5D20C3A8AA5)
    * restrictions  [ 10.2.1.4 ](managing-oracle-data-guard-physical-standby-databases.md#GUID-25EF4F01-3DF0-4F21-99B8-95178CA003BE)
    * using  [ 10.2.1 ](managing-oracle-data-guard-physical-standby-databases.md#GUID-07CB190C-C248-4FF5-AB64-EAA9C6D42677)
  * recovering 
    * from errors  [ A.10.1 ](troubleshooting-oracle-data-guard.md#GUID-1D92D18C-488C-4896-9592-33FBB4BEEAB2)
    * logical standby databases  [ 11.6.5 ](managing-oracle-data-guard-logical-standby-databases.md#GUID-44D7C275-A991-4B6F-84A2-D76085E00367)
    * physical standby databases 
      * after an OPEN RESETLOGS  [ 10.4 ](managing-oracle-data-guard-physical-standby-databases.md#GUID-02218512-31A6-461E-96B4-0B9997DCFC4A)
    * through resetlogs  [ 10.4 ](managing-oracle-data-guard-physical-standby-databases.md#GUID-02218512-31A6-461E-96B4-0B9997DCFC4A) , [ 11.6.5 ](managing-oracle-data-guard-logical-standby-databases.md#GUID-44D7C275-A991-4B6F-84A2-D76085E00367)
  * RECOVER MANAGED STANDBY DATABASE CANCEL clause 
    * terminating  [ 4.3.4.1 ](creating-oracle-data-guard-logical-standby.md#GUID-715EBEC1-21B9-4DA9-80A5-ED58033F94C8)
  * RECOVER MANAGED STANDBY DATABASE clause 
    * of ALTER DATABASE  [ 3.3.6 ](creating-oracle-data-guard-physical-standby.md#GUID-3D65210D-9FE5-490E-938E-57FC2E07A569) , [ 4.3.5 ](creating-oracle-data-guard-logical-standby.md#GUID-68777C01-83F6-4CCF-9BA2-547ED180138D) , [ 8.3.1 ](oracle-data-guard-redo-apply-services.md#GUID-54FA44F8-1EDA-4CAF-8B04-90CC6108FED8) , [ 18.1 ](sql-statements-used-by-oracle-data-guard.md#GUID-F690B602-6582-465B-8E86-0B0A52838D66)
  * RECOVER TO LOGICAL STANDBY clause 
    * converting a physical standby database to a logical standby database  [ 4.3.4.1 ](creating-oracle-data-guard-logical-standby.md#GUID-715EBEC1-21B9-4DA9-80A5-ED58033F94C8)
  * Recovery Manager 
    * characteristics complementary to Data Guard  [ 1.7 ](introduction-to-oracle-data-guard-concepts.md#GUID-9EEFD8E5-901F-4092-85F4-2E0069AC5923)
    * standby database 
      * creating  [ E.2.1 ](creating-data-guard-standby-database-using-RMAN.md#GUID-9864EED2-AE6E-4378-BA7C-1FB3971649E5)
      * LOG_FILE_NAME_CONVERT initialization parameter  [ E.2.2.3.1 ](creating-data-guard-standby-database-using-RMAN.md#GUID-730789B8-813C-433F-BF49-D34B4077DE77)
      * preparing using RMAN  [ E.2.2 ](creating-data-guard-standby-database-using-RMAN.md#GUID-C055CE69-9410-4B71-92B6-93C9B6F63197)
  * re-creating 
    * a table on a logical standby database  [ 11.5.5 ](managing-oracle-data-guard-logical-standby-databases.md#GUID-DD88E3D6-D6DB-4175-BB59-F048B9A87BAA)
  * redirecting DML operations 
    * standby databases  [ 10.2.2.1 ](managing-oracle-data-guard-physical-standby-databases.md#GUID-8AAD002C-ED06-4349-8BB5-EC8DB30B2628)
  * redirecting PL/SQL 
    * standby databases  [ 10.2.2.2 ](managing-oracle-data-guard-physical-standby-databases.md#GUID-143121BA-3F4C-4953-B0E7-68BE9EE700C9)
  * Redo Apply 
    * defined  [ 1.3.2 ](introduction-to-oracle-data-guard-concepts.md#GUID-D284E617-3F4F-4128-98F5-A3EB88772BF5) , [ 8.1 ](oracle-data-guard-redo-apply-services.md#GUID-74C8C1F3-70FC-4543-B958-83F309B6B01B)
    * flashing back after failover  [ 15.2.1 ](examples-of-using-oracle-data-guard.md#GUID-C7C39BEC-A841-48D1-BE4B-9BB49C65B9C9)
    * starting  [ 3.3.6 ](creating-oracle-data-guard-physical-standby.md#GUID-3D65210D-9FE5-490E-938E-57FC2E07A569)
    * stopping  [ 10.1.2 ](managing-oracle-data-guard-physical-standby-databases.md#GUID-B51873F4-F50C-4757-9DF9-D43635BFAD7F)
    * technology  [ 1.3.2 ](introduction-to-oracle-data-guard-concepts.md#GUID-D284E617-3F4F-4128-98F5-A3EB88772BF5)
    * tuning the log apply rate  [ 10.8 ](managing-oracle-data-guard-physical-standby-databases.md#GUID-EEDAC95A-828F-4117-AF57-D8ABA702D42D)
  * redo data 
    * applying 
      * through Redo Apply technology  [ 1.3.2 ](introduction-to-oracle-data-guard-concepts.md#GUID-D284E617-3F4F-4128-98F5-A3EB88772BF5)
      * through SQL Apply technology  [ 1.3.2 ](introduction-to-oracle-data-guard-concepts.md#GUID-D284E617-3F4F-4128-98F5-A3EB88772BF5)
      * to standby database  [ 8.1 ](oracle-data-guard-redo-apply-services.md#GUID-74C8C1F3-70FC-4543-B958-83F309B6B01B)
      * to standby databases  [ 1.1.2 ](introduction-to-oracle-data-guard-concepts.md#GUID-C49AC6F4-C89B-4487-BC18-428D65865B9A)
    * applying during conversion of a physical standby database to a logical standby database  [ 4.3.4.1 ](creating-oracle-data-guard-logical-standby.md#GUID-715EBEC1-21B9-4DA9-80A5-ED58033F94C8)
    * archiving on the standby system  [ 1.3.2 ](introduction-to-oracle-data-guard-concepts.md#GUID-D284E617-3F4F-4128-98F5-A3EB88772BF5) , [ 8.1 ](oracle-data-guard-redo-apply-services.md#GUID-74C8C1F3-70FC-4543-B958-83F309B6B01B)
    * building a dictionary in  [ 4.3.3.2 ](creating-oracle-data-guard-logical-standby.md#GUID-E5D227F2-C456-44F3-AEEA-2C8340439EF3)
    * cascading  [ 7.3 ](oracle-data-guard-redo-transport-services.md#GUID-34BCB162-D996-4678-97F1-497805764950)
    * transmitting  [ 1.1.2 ](introduction-to-oracle-data-guard-concepts.md#GUID-C49AC6F4-C89B-4487-BC18-428D65865B9A) , [ 1.3.1 ](introduction-to-oracle-data-guard-concepts.md#GUID-05D621EE-51AF-41E4-9CFC-45AEBAB08F57)
  * redo gaps  [ 7.6.3 ](oracle-data-guard-redo-transport-services.md#GUID-05CE1D49-3DEE-4AAC-A887-55ED436912AF)
    * manual resolution  [ 7.6.3.1 ](oracle-data-guard-redo-transport-services.md#GUID-97EB433B-4125-4082-9276-2454349BC4F8)
    * reducing resolution time  [ 7.6.3 ](oracle-data-guard-redo-transport-services.md#GUID-05CE1D49-3DEE-4AAC-A887-55ED436912AF)
  * redo log files 
    * delaying application  [ 8.2.2 ](oracle-data-guard-redo-apply-services.md#GUID-B891A164-1FDC-4AB3-838F-E6A91E7DA0B7)
  * redo logs 
    * automatic application on physical standby databases  [ 8.3.1 ](oracle-data-guard-redo-apply-services.md#GUID-54FA44F8-1EDA-4CAF-8B04-90CC6108FED8)
    * update standby database tables  [ 1.9 ](introduction-to-oracle-data-guard-concepts.md#GUID-C65BF458-4BF7-4723-BAFC-5860B5B88A2B)
  * redo transport services  [ 7 ](oracle-data-guard-redo-transport-services.md#GUID-0EC71C7D-A80B-40AA-93E5-28BB7E24ED31)
    * archive destinations 
      * alternate  [ A.2 ](troubleshooting-oracle-data-guard.md#GUID-397ED7EA-1A1C-4224-82A3-746448BE6944)
      * re-archiving to failed destinations  [ 17.14 ](LOG_ARCHIVE_DEST_n-parameter-attributes.md#GUID-AE01BD16-8FD7-4C4D-BBB6-156281C947A7)
    * authenticating sessions 
      * using SSL  [ 7.2.1.1 ](oracle-data-guard-redo-transport-services.md#GUID-DB2E46B2-2A4B-4DF0-B925-F07497F3C6A5)
    * configuring  [ 7.2 ](oracle-data-guard-redo-transport-services.md#GUID-8575AE63-9583-4149-9EDF-F4FD9E62B70A)
    * configuring security  [ 7.2.1 ](oracle-data-guard-redo-transport-services.md#GUID-FA3B203C-D28E-4A27-8585-9EBC19836F4C)
    * defined  [ 1.3.1 ](introduction-to-oracle-data-guard-concepts.md#GUID-05D621EE-51AF-41E4-9CFC-45AEBAB08F57)
    * gap detection  [ 7.6.3 ](oracle-data-guard-redo-transport-services.md#GUID-05CE1D49-3DEE-4AAC-A887-55ED436912AF)
    * handling archive failures  [ 17.14 ](LOG_ARCHIVE_DEST_n-parameter-attributes.md#GUID-AE01BD16-8FD7-4C4D-BBB6-156281C947A7)
    * monitoring status  [ 7.6.1 ](oracle-data-guard-redo-transport-services.md#GUID-A9239CAA-DCE9-4108-9516-980FF37A3AE5)
    * network 
      * tuning  [ A.7 ](troubleshooting-oracle-data-guard.md#GUID-5B879B01-2433-4B08-8D58-E91DBA58FEF7)
    * protection modes 
      * maximum availability mode  [ 1.5 ](introduction-to-oracle-data-guard-concepts.md#GUID-65DA4E2E-3666-4972-895C-EA45D043B127)
      * maximum performance mode  [ 1.5 ](introduction-to-oracle-data-guard-concepts.md#GUID-65DA4E2E-3666-4972-895C-EA45D043B127)
      * maximum protection mode  [ 1.5 ](introduction-to-oracle-data-guard-concepts.md#GUID-65DA4E2E-3666-4972-895C-EA45D043B127)
    * receiving redo data  [ 7.2.3 ](oracle-data-guard-redo-transport-services.md#GUID-AF64C4FE-2D16-4197-953A-25930F36CFA8)
    * sending redo data  [ 7.2.2 ](oracle-data-guard-redo-transport-services.md#GUID-45DFFA7C-3967-4B46-8C2C-DBD22A4D6A74)
    * synchronous and asynchronous disk I/O  [ 17.1 ](LOG_ARCHIVE_DEST_n-parameter-attributes.md#GUID-559248E6-B153-4B7B-A8A9-9CEE102088F0)
    * wait events  [ 7.6.4 ](oracle-data-guard-redo-transport-services.md#GUID-EB15DAF9-40A8-4185-B4C2-F8BB4FBD33CE)
  * registering 
    * archived redo log files 
      * during failover  [ 9.3.2 ](managing-oracle-data-guard-role-transitions.md#GUID-EBD0C21B-6356-446E-8BF5-7B7D96554713)
  * REGISTER LOGFILE clause 
    * of ALTER DATABASE  [ A.4.1 ](troubleshooting-oracle-data-guard.md#GUID-616EF287-25CE-4F6D-A0C3-9A4DEDA3A97C) , [ G.3.1 ](performing-oracle-data-guard-role-transitions.md#GUID-9D024B1F-54EF-4C95-A810-1FA09B7AE3E4)
  * REGISTER LOGICAL LOGFILE clause 
    * of ALTER DATABASE  [ 9.3.2 ](managing-oracle-data-guard-role-transitions.md#GUID-EBD0C21B-6356-446E-8BF5-7B7D96554713)
  * RELY constraint 
    * creating  [ 4.2.2 ](creating-oracle-data-guard-logical-standby.md#GUID-C238335E-8383-43F1-B5B1-709A2A619C30)
  * remote file server process (RFS) 
    * log writer process and  [ 8.2.1 ](oracle-data-guard-redo-apply-services.md#GUID-9922F0E7-3E19-449E-8C35-57B7E5714C54)
  * RENAME FILE clause 
    * of ALTER DATABASE  [ A.1.1 ](troubleshooting-oracle-data-guard.md#GUID-AFDF503B-600D-4B7E-A476-2E7E0CB489C0)
  * renaming 
    * datafiles 
      * on the primary database  [ 10.3.4 ](managing-oracle-data-guard-physical-standby-databases.md#GUID-2D3E5488-237C-467A-AB8B-7C09B6E43CE0)
      * setting the STANDBY_FILE_MANAGEMENT parameter  [ 10.3.4 ](managing-oracle-data-guard-physical-standby-databases.md#GUID-2D3E5488-237C-467A-AB8B-7C09B6E43CE0)
  * REOPEN attribute  [ 17.14 ](LOG_ARCHIVE_DEST_n-parameter-attributes.md#GUID-AE01BD16-8FD7-4C4D-BBB6-156281C947A7)
  * reporting operations 
    * configuring  [ 1.1.5 ](introduction-to-oracle-data-guard-concepts.md#GUID-0DA5AED1-4D4B-446D-ADD2-7F463A688AEF)
    * offloading on the standby database  [ 1.9 ](introduction-to-oracle-data-guard-concepts.md#GUID-C65BF458-4BF7-4723-BAFC-5860B5B88A2B)
  * requirements 
    * of a rolling upgrade  [ 13.2 ](using-sql-apply-to-perform-rolling-upgrade.md#GUID-2CF3E283-2EC3-4B55-A71B-9D25DDF52156)
  * restart considerations 
    * SQL Apply  [ 11.1.1.3 ](managing-oracle-data-guard-logical-standby-databases.md#GUID-F0FDEEF3-B4EA-40F3-BF7B-2167D33D91C8)
  * restore points 
    * replicating to standby  [ 10.7 ](managing-oracle-data-guard-physical-standby-databases.md#GUID-45D7D60E-ED0F-4B3E-9D32-EB5394196CAF)
  * resynchronizing 
    * logical standby databases with a new branch of redo  [ 11.6.5 ](managing-oracle-data-guard-logical-standby-databases.md#GUID-44D7C275-A991-4B6F-84A2-D76085E00367)
    * physical standby databases with a new branch of redo  [ 10.4 ](managing-oracle-data-guard-physical-standby-databases.md#GUID-02218512-31A6-461E-96B4-0B9997DCFC4A)
  * retrieving 
    * missing archived redo log files  [ 1.3.1 ](introduction-to-oracle-data-guard-concepts.md#GUID-05D621EE-51AF-41E4-9CFC-45AEBAB08F57) , [ 1.9 ](introduction-to-oracle-data-guard-concepts.md#GUID-C65BF458-4BF7-4723-BAFC-5860B5B88A2B)
  * RMAN BACKUP INCREMENTAL FROM SCN command  [ 12.9.1 ](using-RMAN-in-oracle-data-guard-configurations.md#GUID-1F1D37FE-48A6-46D4-98DB-C0E3707E3BCF) , [ 12.11 ](using-RMAN-in-oracle-data-guard-configurations.md#GUID-F0EB4F8E-7D2F-4674-9A5B-AABC368D8F11)
  * RMAN backups 
    * accessibility in Data Guard environment  [ 12.1.3 ](using-RMAN-in-oracle-data-guard-configurations.md#GUID-61ABB2C2-0813-4DD9-94F4-BB292CB21194)
    * association in Data Guard environment  [ 12.1.2 ](using-RMAN-in-oracle-data-guard-configurations.md#GUID-73A75929-1F78-46F1-9CBE-EC2611388A6B)
    * interchangeability in Data Guard environment  [ 12.1.1 ](using-RMAN-in-oracle-data-guard-configurations.md#GUID-075FF361-A398-4533-9E7B-9255A288A662)
  * role-based destinations 
    * setting  [ 17.17 ](LOG_ARCHIVE_DEST_n-parameter-attributes.md#GUID-2E1845CC-B3CA-4409-BACB-6AB40D6D42E5)
  * role management services 
    * defined  [ 9 ](managing-oracle-data-guard-role-transitions.md#GUID-44F26D5F-E036-4ACD-B8F0-224F5498FBCE)
  * role transitions  [ 1.3.3 ](introduction-to-oracle-data-guard-concepts.md#GUID-EFAC6E17-BCE5-417C-9124-1595DBF0ABFA) , [ 9.1 ](managing-oracle-data-guard-role-transitions.md#GUID-BEB883C9-0391-472A-A05E-9A5776C06F4C)
    * choosing a type of  [ 9.1.1 ](managing-oracle-data-guard-role-transitions.md#GUID-66282DCD-5E7B-43C2-ADA1-03342E2750A0)
    * defined  [ 1.3.3 ](introduction-to-oracle-data-guard-concepts.md#GUID-EFAC6E17-BCE5-417C-9124-1595DBF0ABFA)
    * flashing back the databases after  [ 9.4 ](managing-oracle-data-guard-role-transitions.md#GUID-95E5629A-17F6-44B7-813F-00DD871359FA)
    * logical standby database and  [ 9.3 ](managing-oracle-data-guard-role-transitions.md#GUID-CE785178-FD22-4A81-9248-DF5ED8E91635)
    * monitoring  [ 10.6 ](managing-oracle-data-guard-physical-standby-databases.md#GUID-0682EE24-B495-48E8-9E1F-52AC0E6FE486)
    * physical standby databases and  [ 9.2 ](managing-oracle-data-guard-role-transitions.md#GUID-857F6F45-DC1C-4345-BD39-F3BE7D79F1CD) , [ G ](performing-oracle-data-guard-role-transitions.md#GUID-6E78412B-8E55-4D8B-A318-9F0612376E86)
    * reversals  [ 1.3.3 ](introduction-to-oracle-data-guard-concepts.md#GUID-EFAC6E17-BCE5-417C-9124-1595DBF0ABFA) , [ 9.1 ](managing-oracle-data-guard-role-transitions.md#GUID-BEB883C9-0391-472A-A05E-9A5776C06F4C)
  * role transition triggers  [ 9.1.5 ](managing-oracle-data-guard-role-transitions.md#GUID-18E84C50-79D7-4158-A5B8-FD0C3EEEB8B6)
    * DB_ROLE_CHANGE system event  [ 9.1.5 ](managing-oracle-data-guard-role-transitions.md#GUID-18E84C50-79D7-4158-A5B8-FD0C3EEEB8B6)
  * rollback 
    * after switchover failures  [ A.4.4 ](troubleshooting-oracle-data-guard.md#GUID-97282862-4EA0-42D9-97EF-34E75A29F87F) , [ G.3.4 ](performing-oracle-data-guard-role-transitions.md#GUID-2CF7243A-24DA-42CB-86C7-D48E648D9F5D)
  * rolling upgrade 
    * software requirements  [ 2.3.2 ](getting-started-with-oracle-data-guard.md#GUID-A9F7D87E-49EB-4CE9-A076-1F35F35C0EAB)
  * rolling upgrades 
    * benefits  [ 13.1 ](using-sql-apply-to-perform-rolling-upgrade.md#GUID-701A35C5-B545-43DD-8854-F6948E0BB32A)
    * DBMS_ROLLING package  [ 14 ](using-DBMS_ROLLING-to-perform-rolling-upgrade.md#GUID-70C09F5B-90BE-4C8C-96A5-45A52E05D380)
    * patch set releases  [ 2.3.2 ](getting-started-with-oracle-data-guard.md#GUID-A9F7D87E-49EB-4CE9-A076-1F35F35C0EAB)
    * requirements  [ 13.2 ](using-sql-apply-to-perform-rolling-upgrade.md#GUID-2CF3E283-2EC3-4B55-A71B-9D25DDF52156)
    * setting the COMPATIBLE initialization parameter  [ 13.5 ](using-sql-apply-to-perform-rolling-upgrade.md#GUID-5995DF99-5268-43BE-BD63-D0DEAA8241EC)
    * use of KEEP IDENTITY clause  [ 4.3.4.1 ](creating-oracle-data-guard-logical-standby.md#GUID-715EBEC1-21B9-4DA9-80A5-ED58033F94C8)
    * using Active Data Guard  [ 14 ](using-DBMS_ROLLING-to-perform-rolling-upgrade.md#GUID-70C09F5B-90BE-4C8C-96A5-45A52E05D380)



* * *

##  S 

  * schemas 
    * identical to primary database  [ 1.1.2 ](introduction-to-oracle-data-guard-concepts.md#GUID-C49AC6F4-C89B-4487-BC18-428D65865B9A)
  * sequences 
    * unsupported on logical standby databases  [ C.14 ](data-type-ddl-support-on-logical-standby-databases.md#GUID-291FD74A-165A-4E80-9E1F-66996F1FC6CA)
    * using in Oracle Active Data Guard  [ 10.2.6 ](managing-oracle-data-guard-physical-standby-databases.md#GUID-DFDC929C-7446-413B-A25B-7F11284D900F)
  * SERVICE attribute  [ 17.8 ](LOG_ARCHIVE_DEST_n-parameter-attributes.md#GUID-9ACDEC6C-C2E0-4EDA-B66D-B1F819D3368D)
  * SET STANDBY DATABASE clause 
    * of ALTER DATA  [ 18.1 ](sql-statements-used-by-oracle-data-guard.md#GUID-F690B602-6582-465B-8E86-0B0A52838D66)
    * of ALTER DATABASE  [ 9.1.4 ](managing-oracle-data-guard-role-transitions.md#GUID-3E6BE44A-3077-4E67-86E7-B641DBF30083) , [ 18.1 ](sql-statements-used-by-oracle-data-guard.md#GUID-F690B602-6582-465B-8E86-0B0A52838D66)
  * shutting down 
    * physical standby database  [ 10.1.2 ](managing-oracle-data-guard-physical-standby-databases.md#GUID-B51873F4-F50C-4757-9DF9-D43635BFAD7F)
  * simulating 
    * standby database environment  [ 2.3.2 ](getting-started-with-oracle-data-guard.md#GUID-A9F7D87E-49EB-4CE9-A076-1F35F35C0EAB)
  * SKIP_ERROR procedure 
    * of the DBMS_LOGSTDBY package  [ A.3 ](troubleshooting-oracle-data-guard.md#GUID-3BC98594-CFA0-4EDC-92B9-52F19088B224)
  * SKIP_TRANSACTION procedure 
    * of DBMS_LOGSTDBY  [ A.6 ](troubleshooting-oracle-data-guard.md#GUID-E2C9EEB9-CDCE-46B4-A7ED-D60B7FCD657A)
  * skip handler 
    * setting up on a logical standby database  [ 11.5.3 ](managing-oracle-data-guard-logical-standby-databases.md#GUID-28EB46A0-696D-426E-B596-225443867604)
  * SKIP procedure 
    * of DBMS_LOGSTDBY  [ A.6 ](troubleshooting-oracle-data-guard.md#GUID-E2C9EEB9-CDCE-46B4-A7ED-D60B7FCD657A)
  * snapshot standby databases  [ 1.1.2 ](introduction-to-oracle-data-guard-concepts.md#GUID-C49AC6F4-C89B-4487-BC18-428D65865B9A)
    * managing  [ 10.11 ](managing-oracle-data-guard-physical-standby-databases.md#GUID-CE30E2D6-5B53-4389-8B02-FC0F341C8C1A)
  * software requirements  [ 2.3.2 ](getting-started-with-oracle-data-guard.md#GUID-A9F7D87E-49EB-4CE9-A076-1F35F35C0EAB)
    * rolling upgrades  [ 2.3.2 ](getting-started-with-oracle-data-guard.md#GUID-A9F7D87E-49EB-4CE9-A076-1F35F35C0EAB)
  * SQL Apply  [ 8.4.2 ](oracle-data-guard-redo-apply-services.md#GUID-C7B53D2A-5D38-4BA5-8142-98A81691A415) , [ 11.1.1.2 ](managing-oracle-data-guard-logical-standby-databases.md#GUID-E3ADF4B7-F08A-4F2B-A1CD-FB897ECCF534)
    * after an OPEN RESETLOGS  [ 11.6.5 ](managing-oracle-data-guard-logical-standby-databases.md#GUID-44D7C275-A991-4B6F-84A2-D76085E00367)
    * applying CREATE TABLE AS SELECT (CTAS) statements  [ 11.1.1.5 ](managing-oracle-data-guard-logical-standby-databases.md#GUID-1BBCF0C2-E189-47EF-B7D1-805BF552E4DB)
    * applying DDL transactions  [ 11.1.1.5 ](managing-oracle-data-guard-logical-standby-databases.md#GUID-1BBCF0C2-E189-47EF-B7D1-805BF552E4DB)
    * applying DML transactions  [ 11.1.1.4 ](managing-oracle-data-guard-logical-standby-databases.md#GUID-97797605-D5BB-49B7-8E9D-D3E7C56A767C)
    * architecture  [ 11.1 ](managing-oracle-data-guard-logical-standby-databases.md#GUID-735EED44-DFDB-43B8-ADBC-F1A3A62E7D93) , [ 11.4.1 ](managing-oracle-data-guard-logical-standby-databases.md#GUID-5BD79860-B3B8-4E0D-9F55-2E9918B93292)
    * defined  [ 1.3.2 ](introduction-to-oracle-data-guard-concepts.md#GUID-D284E617-3F4F-4128-98F5-A3EB88772BF5) , [ 8.1 ](oracle-data-guard-redo-apply-services.md#GUID-74C8C1F3-70FC-4543-B958-83F309B6B01B)
    * deleting archived redo log files  [ 11.4.2 ](managing-oracle-data-guard-logical-standby-databases.md#GUID-893B304C-123F-4DCA-ACA2-96BB74E7A8A6)
    * parallel DML (PDML) transactions  [ 11.1.1.3 ](managing-oracle-data-guard-logical-standby-databases.md#GUID-F0FDEEF3-B4EA-40F3-BF7B-2167D33D91C8) , [ 11.1.1.4 ](managing-oracle-data-guard-logical-standby-databases.md#GUID-97797605-D5BB-49B7-8E9D-D3E7C56A767C)
    * performing a rolling upgrade  [ 13.1 ](using-sql-apply-to-perform-rolling-upgrade.md#GUID-701A35C5-B545-43DD-8854-F6948E0BB32A)
    * requirements for rolling upgrades  [ 13.2 ](using-sql-apply-to-perform-rolling-upgrade.md#GUID-2CF3E283-2EC3-4B55-A71B-9D25DDF52156)
    * restart considerations  [ 11.1.1.3 ](managing-oracle-data-guard-logical-standby-databases.md#GUID-F0FDEEF3-B4EA-40F3-BF7B-2167D33D91C8)
    * rolling upgrades  [ 2.3.2 ](getting-started-with-oracle-data-guard.md#GUID-A9F7D87E-49EB-4CE9-A076-1F35F35C0EAB)
    * starting 
      * real-time apply  [ 8.4.1 ](oracle-data-guard-redo-apply-services.md#GUID-5E640AEE-12C7-4ED8-8C42-B989F7768E91)
    * stopping 
      * real-time apply  [ 8.4.2 ](oracle-data-guard-redo-apply-services.md#GUID-C7B53D2A-5D38-4BA5-8142-98A81691A415)
    * supported data types  [ C.1.1 ](data-type-ddl-support-on-logical-standby-databases.md#GUID-8FED6166-FB83-42E4-8D2E-8A1F7DFCEB00)
    * support for DDL statements  [ C ](data-type-ddl-support-on-logical-standby-databases.md#GUID-340BA6D6-74D5-4684-AF39-08890D5CD9D4)
    * support for PL/SQL supplied packages  [ C.13.1 ](data-type-ddl-support-on-logical-standby-databases.md#GUID-E89A196F-2C5D-45F9-94BA-7793FBF44440)
    * transaction size considerations  [ 11.1.1.1 ](managing-oracle-data-guard-logical-standby-databases.md#GUID-344AF03D-88A6-4DE1-8650-D5F1B9781FC3)
    * unsupported data types  [ C.1.2 ](data-type-ddl-support-on-logical-standby-databases.md#GUID-76E70ADB-0709-4403-BA6A-379CEAFE758A)
    * unsupported PL/SQL supplied packages  [ C.13.2 ](data-type-ddl-support-on-logical-standby-databases.md#GUID-703C8569-4298-48ED-98F5-6146A6D46F3F)
    * what to do if it stops  [ A.6 ](troubleshooting-oracle-data-guard.md#GUID-E2C9EEB9-CDCE-46B4-A7ED-D60B7FCD657A)
  * SQL statements 
    * executing on logical standby databases  [ 1.1.2 ](introduction-to-oracle-data-guard-concepts.md#GUID-C49AC6F4-C89B-4487-BC18-428D65865B9A) , [ 1.3.2 ](introduction-to-oracle-data-guard-concepts.md#GUID-D284E617-3F4F-4128-98F5-A3EB88772BF5)
    * skipping on logical standby databases  [ C.15 ](data-type-ddl-support-on-logical-standby-databases.md#GUID-9DBC35BD-43C9-469F-BADD-C91752E88225)
  * STANDBY_FILE_MANAGEMENT initialization parameter 
    * when renaming datafiles  [ 10.3.4 ](managing-oracle-data-guard-physical-standby-databases.md#GUID-2D3E5488-237C-467A-AB8B-7C09B6E43CE0)
  * standby database 
    * creating logical  [ 4 ](creating-oracle-data-guard-logical-standby.md#GUID-3666CA35-D993-44B6-8D70-A2B8B9EC8B2E)
  * standby databases 
    * about creating using RMAN  [ E.2.1 ](creating-data-guard-standby-database-using-RMAN.md#GUID-9864EED2-AE6E-4378-BA7C-1FB3971649E5)
    * applying redo data on  [ 8 ](oracle-data-guard-redo-apply-services.md#GUID-B40DF83F-D4A0-4710-935B-AA5D4B2D9010)
    * applying redo log files on  [ 1.3.2 ](introduction-to-oracle-data-guard-concepts.md#GUID-D284E617-3F4F-4128-98F5-A3EB88772BF5) , [ 1.9 ](introduction-to-oracle-data-guard-concepts.md#GUID-C65BF458-4BF7-4723-BAFC-5860B5B88A2B)
    * apply services on  [ 8.1 ](oracle-data-guard-redo-apply-services.md#GUID-74C8C1F3-70FC-4543-B958-83F309B6B01B)
    * configuring  [ 1.1 ](introduction-to-oracle-data-guard-concepts.md#GUID-AB9DF863-2C7E-4767-81F2-56AD0FA30B49)
      * maximum number of  [ 2 ](getting-started-with-oracle-data-guard.md#GUID-31EBC212-59E1-4536-A884-EFA9FE3DFD84)
      * on Oracle Real Application Clusters  [ 1.1.2 ](introduction-to-oracle-data-guard-concepts.md#GUID-C49AC6F4-C89B-4487-BC18-428D65865B9A) , [ D.1 ](configuring-data-guard-standby-databases-in-oracle-RAC.md#GUID-3140A293-DDD8-4559-8493-B6C21646E90F)
      * on remote locations  [ 1.1.5 ](introduction-to-oracle-data-guard-concepts.md#GUID-0DA5AED1-4D4B-446D-ADD2-7F463A688AEF)
      * single-instance  [ 1.1.2 ](introduction-to-oracle-data-guard-concepts.md#GUID-C49AC6F4-C89B-4487-BC18-428D65865B9A)
    * creating  [ 1.1.2 ](introduction-to-oracle-data-guard-concepts.md#GUID-C49AC6F4-C89B-4487-BC18-428D65865B9A)
      * checklist of tasks  [ 4.3 ](creating-oracle-data-guard-logical-standby.md#GUID-2A3EDE02-DE51-4813-891C-248AA5F1C027)
      * directory structure considerations  [ 2.4 ](getting-started-with-oracle-data-guard.md#GUID-B59DBCF7-13A7-4F49-97E9-BF1BEF803699)
      * if primary uses ASM or OMF  [ 15.5 ](examples-of-using-oracle-data-guard.md#GUID-6FB574E5-2F3A-4C9E-A3F0-7C9154B0F540)
      * on remote host with same directory structure  [ E.3 ](creating-data-guard-standby-database-using-RMAN.md#GUID-D1694762-4B53-4828-B094-874DB1A6FC81)
      * with a time lag  [ 8.2.2 ](oracle-data-guard-redo-apply-services.md#GUID-B891A164-1FDC-4AB3-838F-E6A91E7DA0B7)
    * defined  [ 2.1 ](getting-started-with-oracle-data-guard.md#GUID-4CAD29A7-28FD-40B4-A223-F69A302B1E00)
    * failover 
      * preparing for  [ 9.1.4 ](managing-oracle-data-guard-role-transitions.md#GUID-3E6BE44A-3077-4E67-86E7-B641DBF30083)
    * failover to  [ 9.1.4 ](managing-oracle-data-guard-role-transitions.md#GUID-3E6BE44A-3077-4E67-86E7-B641DBF30083)
    * LOG_FILE_NAME_CONVERT initialization parameter  [ E.2.2.3.1 ](creating-data-guard-standby-database-using-RMAN.md#GUID-730789B8-813C-433F-BF49-D34B4077DE77)
    * operational requirements  [ 2.3 ](getting-started-with-oracle-data-guard.md#GUID-62F35117-0A9C-47E9-82B6-7E3B214B28B7) , [ 2.3.2 ](getting-started-with-oracle-data-guard.md#GUID-A9F7D87E-49EB-4CE9-A076-1F35F35C0EAB)
    * preparing to use RMAN  [ E.2.2 ](creating-data-guard-standby-database-using-RMAN.md#GUID-C055CE69-9410-4B71-92B6-93C9B6F63197)
    * recovering through OPEN RESETLOGS  [ 10.4 ](managing-oracle-data-guard-physical-standby-databases.md#GUID-02218512-31A6-461E-96B4-0B9997DCFC4A)
    * redirecting DML operations  [ 10.2.2.1 ](managing-oracle-data-guard-physical-standby-databases.md#GUID-8AAD002C-ED06-4349-8BB5-EC8DB30B2628)
    * redirecting PL/SQL  [ 10.2.2.2 ](managing-oracle-data-guard-physical-standby-databases.md#GUID-143121BA-3F4C-4953-B0E7-68BE9EE700C9)
    * resynchronizing with the primary database  [ 1.9 ](introduction-to-oracle-data-guard-concepts.md#GUID-C65BF458-4BF7-4723-BAFC-5860B5B88A2B)
    * software requirements  [ 2.3.2 ](getting-started-with-oracle-data-guard.md#GUID-A9F7D87E-49EB-4CE9-A076-1F35F35C0EAB)
    * starting apply services on physical  [ 8.3.1 ](oracle-data-guard-redo-apply-services.md#GUID-54FA44F8-1EDA-4CAF-8B04-90CC6108FED8)
  * standby redo log files 
    * and real-time apply  [ 8.2.1 ](oracle-data-guard-redo-apply-services.md#GUID-9922F0E7-3E19-449E-8C35-57B7E5714C54)
  * standby redo logs 
    * creating and managing  [ 7.2.3.1 ](oracle-data-guard-redo-transport-services.md#GUID-E6EC6104-3C38-482D-B807-A0E84ECFB937)
  * standby role  [ 1.1.2 ](introduction-to-oracle-data-guard-concepts.md#GUID-C49AC6F4-C89B-4487-BC18-428D65865B9A)
  * starting 
    * logical standby databases  [ 4.3.5 ](creating-oracle-data-guard-logical-standby.md#GUID-68777C01-83F6-4CCF-9BA2-547ED180138D)
    * physical standby databases  [ 3.3.6 ](creating-oracle-data-guard-physical-standby.md#GUID-3D65210D-9FE5-490E-938E-57FC2E07A569)
    * real-time apply  [ 8.4.1 ](oracle-data-guard-redo-apply-services.md#GUID-5E640AEE-12C7-4ED8-8C42-B989F7768E91)
      * on logical standby databases  [ 8.4.1 ](oracle-data-guard-redo-apply-services.md#GUID-5E640AEE-12C7-4ED8-8C42-B989F7768E91)
    * Redo Apply  [ 10.1.1 ](managing-oracle-data-guard-physical-standby-databases.md#GUID-76930886-6460-45AE-86CA-84DCB41E5BE5)
    * SQL Apply  [ 4.3.5 ](creating-oracle-data-guard-logical-standby.md#GUID-68777C01-83F6-4CCF-9BA2-547ED180138D) , [ 8.4.1 ](oracle-data-guard-redo-apply-services.md#GUID-5E640AEE-12C7-4ED8-8C42-B989F7768E91)
  * START LOGICAL STANDBY APPLY clause 
    * IMMEDIATE keyword  [ 8.4.1 ](oracle-data-guard-redo-apply-services.md#GUID-5E640AEE-12C7-4ED8-8C42-B989F7768E91)
    * of ALTER DATABASE  [ 4.3.5 ](creating-oracle-data-guard-logical-standby.md#GUID-68777C01-83F6-4CCF-9BA2-547ED180138D) , [ 8.4.1 ](oracle-data-guard-redo-apply-services.md#GUID-5E640AEE-12C7-4ED8-8C42-B989F7768E91) , [ 13.5 ](using-sql-apply-to-perform-rolling-upgrade.md#GUID-5995DF99-5268-43BE-BD63-D0DEAA8241EC) , [ A.6 ](troubleshooting-oracle-data-guard.md#GUID-E2C9EEB9-CDCE-46B4-A7ED-D60B7FCD657A)
  * STOP LOGICAL STANDBY APPLY clause 
    * of ALTER DATABASE  [ 8.4.2 ](oracle-data-guard-redo-apply-services.md#GUID-C7B53D2A-5D38-4BA5-8142-98A81691A415) , [ 9.3.2 ](managing-oracle-data-guard-role-transitions.md#GUID-EBD0C21B-6356-446E-8BF5-7B7D96554713)
  * stopping 
    * real-time apply 
      * on logical standby databases  [ 8.4.2 ](oracle-data-guard-redo-apply-services.md#GUID-C7B53D2A-5D38-4BA5-8142-98A81691A415)
    * real-time apply on physical standby databases  [ 8.3.2 ](oracle-data-guard-redo-apply-services.md#GUID-1808DF72-C384-4BC2-A75F-D73C56C22A2B)
    * Redo Apply  [ 8.3.2 ](oracle-data-guard-redo-apply-services.md#GUID-1808DF72-C384-4BC2-A75F-D73C56C22A2B)
    * SQL Apply  [ 8.4.2 ](oracle-data-guard-redo-apply-services.md#GUID-C7B53D2A-5D38-4BA5-8142-98A81691A415)
  * streams capture 
    * running on a logical standby  [ 11.6.6 ](managing-oracle-data-guard-logical-standby-databases.md#GUID-DD1C8B1D-0F1A-448E-B193-124DD8FD65E9)
  * supplemental logging 
    * setting up to log primary key and unique-index columns  [ 4.3.3.2 ](creating-oracle-data-guard-logical-standby.md#GUID-E5D227F2-C456-44F3-AEEA-2C8340439EF3) , [ 11.1.1.4 ](managing-oracle-data-guard-logical-standby-databases.md#GUID-97797605-D5BB-49B7-8E9D-D3E7C56A767C)
  * supported data types 
    * for logical standby databases  [ C ](data-type-ddl-support-on-logical-standby-databases.md#GUID-340BA6D6-74D5-4684-AF39-08890D5CD9D4) , [ C.16 ](data-type-ddl-support-on-logical-standby-databases.md#GUID-1595A660-6790-436A-BB84-D74C9187D8AE)
  * supported PL/SQL supplied packages  [ C.13.1 ](data-type-ddl-support-on-logical-standby-databases.md#GUID-E89A196F-2C5D-45F9-94BA-7793FBF44440)
  * SWITCHOVER_STATUS column 
    * of V$DATABASE view  [ A.4.1 ](troubleshooting-oracle-data-guard.md#GUID-616EF287-25CE-4F6D-A0C3-9A4DEDA3A97C) , [ G.3.1 ](performing-oracle-data-guard-role-transitions.md#GUID-9D024B1F-54EF-4C95-A810-1FA09B7AE3E4)
  * switchovers  [ 1.3.3 ](introduction-to-oracle-data-guard-concepts.md#GUID-EFAC6E17-BCE5-417C-9124-1595DBF0ABFA)
    * choosing a target standby database  [ 9.1.2 ](managing-oracle-data-guard-role-transitions.md#GUID-60804EE7-88C0-4CB7-AEE7-073EBCAFD1AC)
    * defined  [ 1.3.3 ](introduction-to-oracle-data-guard-concepts.md#GUID-EFAC6E17-BCE5-417C-9124-1595DBF0ABFA)
    * displaying history with DBA_LOGSTDBY_HISTORY  [ 19 ](oracle-data-guard-in-oracle-database-views.md#GUID-FF72163C-78B0-475F-B4CE-135EF288A32A)
    * fails with ORA-01102  [ A.4.2 ](troubleshooting-oracle-data-guard.md#GUID-2BE05596-2E74-4FEE-84AB-81777B5B9E13) , [ G.3.2 ](performing-oracle-data-guard-role-transitions.md#GUID-A9CF13FE-3E89-4FB3-B2BF-6536DCAD3ADB)
    * flashing back databases after  [ 9.4 ](managing-oracle-data-guard-role-transitions.md#GUID-95E5629A-17F6-44B7-813F-00DD871359FA)
    * logical standby databases and  [ 9.3.1 ](managing-oracle-data-guard-role-transitions.md#GUID-42B8B448-90E0-4D7D-82A3-AAA273E6E49B)
    * manual versus automatic  [ 1.3.3 ](introduction-to-oracle-data-guard-concepts.md#GUID-EFAC6E17-BCE5-417C-9124-1595DBF0ABFA)
    * monitoring  [ 10.6 ](managing-oracle-data-guard-physical-standby-databases.md#GUID-0682EE24-B495-48E8-9E1F-52AC0E6FE486)
    * no data loss and  [ 9.1 ](managing-oracle-data-guard-role-transitions.md#GUID-BEB883C9-0391-472A-A05E-9A5776C06F4C)
    * preparing for  [ 9.1.3 ](managing-oracle-data-guard-role-transitions.md#GUID-E86F0570-A207-4F14-AFE4-81B537B96AFD)
    * seeing if the last archived redo log file was transmitted  [ A.4.1 ](troubleshooting-oracle-data-guard.md#GUID-616EF287-25CE-4F6D-A0C3-9A4DEDA3A97C) , [ G.3.1 ](performing-oracle-data-guard-role-transitions.md#GUID-9D024B1F-54EF-4C95-A810-1FA09B7AE3E4)
    * setting DB_FILE_NAME_CONVERT after  [ A.4.3 ](troubleshooting-oracle-data-guard.md#GUID-ACDE1813-1DBD-48E1-8C42-0A2E4ED36631) , [ G.3.3 ](performing-oracle-data-guard-role-transitions.md#GUID-F394098F-8726-45AC-A66F-B91402719D6A)
    * setting LOG_FILE_NAME_CONVERT after  [ A.4.3 ](troubleshooting-oracle-data-guard.md#GUID-ACDE1813-1DBD-48E1-8C42-0A2E4ED36631) , [ G.3.3 ](performing-oracle-data-guard-role-transitions.md#GUID-F394098F-8726-45AC-A66F-B91402719D6A)
    * simplifying with Data Guard broker  [ 1.4 ](introduction-to-oracle-data-guard-concepts.md#GUID-538B9DDD-1553-479D-8E1D-0B5C6848403E) , [ 9 ](managing-oracle-data-guard-role-transitions.md#GUID-44F26D5F-E036-4ACD-B8F0-224F5498FBCE)
    * starting over  [ A.4.4 ](troubleshooting-oracle-data-guard.md#GUID-97282862-4EA0-42D9-97EF-34E75A29F87F) , [ G.3.4 ](performing-oracle-data-guard-role-transitions.md#GUID-2CF7243A-24DA-42CB-86C7-D48E648D9F5D)
    * typical use for  [ 9.1.3 ](managing-oracle-data-guard-role-transitions.md#GUID-E86F0570-A207-4F14-AFE4-81B537B96AFD)
  * SYNC attribute  [ 17.15 ](LOG_ARCHIVE_DEST_n-parameter-attributes.md#GUID-E3626025-2E4C-4C61-B5DF-2BB106F0A349)
  * system events 
    * role transitions  [ 9.1.5 ](managing-oracle-data-guard-role-transitions.md#GUID-18E84C50-79D7-4158-A5B8-FD0C3EEEB8B6)
  * system global area (SGA) 
    * logical change records staged in  [ 11.1 ](managing-oracle-data-guard-logical-standby-databases.md#GUID-735EED44-DFDB-43B8-ADBC-F1A3A62E7D93)
  * system resources 
    * efficient utilization of  [ 1.9 ](introduction-to-oracle-data-guard-concepts.md#GUID-C65BF458-4BF7-4723-BAFC-5860B5B88A2B)



* * *

##  T 

  * tables 
    * logical standby databases 
      * adding on  [ 11.5.5 ](managing-oracle-data-guard-logical-standby-databases.md#GUID-DD88E3D6-D6DB-4175-BB59-F048B9A87BAA)
      * re-creating tables on  [ 11.5.5 ](managing-oracle-data-guard-logical-standby-databases.md#GUID-DD88E3D6-D6DB-4175-BB59-F048B9A87BAA)
      * unsupported on  [ C.14 ](data-type-ddl-support-on-logical-standby-databases.md#GUID-291FD74A-165A-4E80-9E1F-66996F1FC6CA)
  * tablespaces 
    * adding 
      * a new datafile  [ A.10.1.1 ](troubleshooting-oracle-data-guard.md#GUID-16CE2533-7190-4717-88E0-BDC53DBD887B)
      * to primary database  [ 10.3.1 ](managing-oracle-data-guard-physical-standby-databases.md#GUID-C60E4DB4-F424-454D-B4DF-7A97B2F09540)
    * monitoring status changes  [ 10.6 ](managing-oracle-data-guard-physical-standby-databases.md#GUID-0682EE24-B495-48E8-9E1F-52AC0E6FE486)
    * moving between databases  [ 10.3.3 ](managing-oracle-data-guard-physical-standby-databases.md#GUID-88158C9B-51FB-4F13-93FC-97D737C86BF0)
  * target standby database 
    * for switchover  [ 9.1.2 ](managing-oracle-data-guard-role-transitions.md#GUID-60804EE7-88C0-4CB7-AEE7-073EBCAFD1AC)
  * terminal destinations  [ 7.3 ](oracle-data-guard-redo-transport-services.md#GUID-34BCB162-D996-4678-97F1-497805764950)
    * configuring  [ 7.3.1 ](oracle-data-guard-redo-transport-services.md#GUID-10CFB766-F688-4CD8-80CC-7824A522D1DF)
  * terminating 
    * network connection  [ 17.11 ](LOG_ARCHIVE_DEST_n-parameter-attributes.md#GUID-5067F60F-780E-429E-A0FB-CB121B8DF321)
  * throughput 
    * on logical standby databases  [ 11.1.1.4 ](managing-oracle-data-guard-logical-standby-databases.md#GUID-97797605-D5BB-49B7-8E9D-D3E7C56A767C) , [ 11.1.1.5 ](managing-oracle-data-guard-logical-standby-databases.md#GUID-1BBCF0C2-E189-47EF-B7D1-805BF552E4DB)
  * TIME_COMPUTED column  [ 9.1.2 ](managing-oracle-data-guard-role-transitions.md#GUID-60804EE7-88C0-4CB7-AEE7-073EBCAFD1AC)
  * TIME_COMPUTED column of the V$DATAGUARD_STATS view  [ 9.1.2 ](managing-oracle-data-guard-role-transitions.md#GUID-60804EE7-88C0-4CB7-AEE7-073EBCAFD1AC)
  * time lag 
    * delaying application of archived redo log files  [ 8.2.2 ](oracle-data-guard-redo-apply-services.md#GUID-B891A164-1FDC-4AB3-838F-E6A91E7DA0B7) , [ 17.5 ](LOG_ARCHIVE_DEST_n-parameter-attributes.md#GUID-C014D9BB-79D3-4E49-9477-A5CBF0E88167)
    * in standby database  [ 8.2.2 ](oracle-data-guard-redo-apply-services.md#GUID-B891A164-1FDC-4AB3-838F-E6A91E7DA0B7)
  * tnsnames.ora file 
    * redo transport services tuning and  [ A.7 ](troubleshooting-oracle-data-guard.md#GUID-5B879B01-2433-4B08-8D58-E91DBA58FEF7)
    * troubleshooting  [ A.1.2 ](troubleshooting-oracle-data-guard.md#GUID-E0F5B423-208F-4F7E-8CA4-0350D3A7CF0B) , [ A.4.3 ](troubleshooting-oracle-data-guard.md#GUID-ACDE1813-1DBD-48E1-8C42-0A2E4ED36631) , [ A.7 ](troubleshooting-oracle-data-guard.md#GUID-5B879B01-2433-4B08-8D58-E91DBA58FEF7) , [ G.3.3 ](performing-oracle-data-guard-role-transitions.md#GUID-F394098F-8726-45AC-A66F-B91402719D6A)
  * transaction size considerations 
    * SQL Apply  [ 11.1.1.1 ](managing-oracle-data-guard-logical-standby-databases.md#GUID-344AF03D-88A6-4DE1-8650-D5F1B9781FC3)
  * Transparent Data Encryption 
    * support by SQL Apply  [ C.3 ](data-type-ddl-support-on-logical-standby-databases.md#GUID-CE6B667C-46EE-48C5-94AA-C96B98EB4504)
  * transportable tablespaces 
    * using with a physical standby database  [ 10.3.3 ](managing-oracle-data-guard-physical-standby-databases.md#GUID-88158C9B-51FB-4F13-93FC-97D737C86BF0)
  * triggers 
    * handled on a logical standby database  [ 11.6.3 ](managing-oracle-data-guard-logical-standby-databases.md#GUID-50903261-075E-44F4-80F2-E5ED7C165F0D)
    * role transitions  [ 9.1.5 ](managing-oracle-data-guard-role-transitions.md#GUID-18E84C50-79D7-4158-A5B8-FD0C3EEEB8B6)
  * troubleshooting 
    * if SQL Apply stops  [ A.6 ](troubleshooting-oracle-data-guard.md#GUID-E2C9EEB9-CDCE-46B4-A7ED-D60B7FCD657A)
    * last redo data was not transmitted  [ A.4.1 ](troubleshooting-oracle-data-guard.md#GUID-616EF287-25CE-4F6D-A0C3-9A4DEDA3A97C) , [ G.3.1 ](performing-oracle-data-guard-role-transitions.md#GUID-9D024B1F-54EF-4C95-A810-1FA09B7AE3E4)
    * listener.ora file  [ A.1.2 ](troubleshooting-oracle-data-guard.md#GUID-E0F5B423-208F-4F7E-8CA4-0350D3A7CF0B) , [ A.7 ](troubleshooting-oracle-data-guard.md#GUID-5B879B01-2433-4B08-8D58-E91DBA58FEF7)
    * logical standby database failures  [ A.3 ](troubleshooting-oracle-data-guard.md#GUID-3BC98594-CFA0-4EDC-92B9-52F19088B224)
    * SQL Apply  [ A.6 ](troubleshooting-oracle-data-guard.md#GUID-E2C9EEB9-CDCE-46B4-A7ED-D60B7FCD657A)
    * switchovers  [ A.4 ](troubleshooting-oracle-data-guard.md#GUID-05C15D55-8CD8-4CB3-ADD3-642AED0FB3A3)
      * ORA-01102 message  [ A.4.2 ](troubleshooting-oracle-data-guard.md#GUID-2BE05596-2E74-4FEE-84AB-81777B5B9E13) , [ G.3.2 ](performing-oracle-data-guard-role-transitions.md#GUID-A9CF13FE-3E89-4FB3-B2BF-6536DCAD3ADB)
      * roll back and start over  [ A.4.4 ](troubleshooting-oracle-data-guard.md#GUID-97282862-4EA0-42D9-97EF-34E75A29F87F) , [ G.3.4 ](performing-oracle-data-guard-role-transitions.md#GUID-2CF7243A-24DA-42CB-86C7-D48E648D9F5D)
    * tnsnames.ora file  [ A.1.2 ](troubleshooting-oracle-data-guard.md#GUID-E0F5B423-208F-4F7E-8CA4-0350D3A7CF0B) , [ A.4.3 ](troubleshooting-oracle-data-guard.md#GUID-ACDE1813-1DBD-48E1-8C42-0A2E4ED36631) , [ A.7 ](troubleshooting-oracle-data-guard.md#GUID-5B879B01-2433-4B08-8D58-E91DBA58FEF7) , [ G.3.3 ](performing-oracle-data-guard-role-transitions.md#GUID-F394098F-8726-45AC-A66F-B91402719D6A)
  * tuning 
    * log apply rate for Redo Apply  [ 10.8 ](managing-oracle-data-guard-physical-standby-databases.md#GUID-EEDAC95A-828F-4117-AF57-D8ABA702D42D)



* * *

##  U 

  * unique-index columns 
    * logged with supplemental logging  [ 4.3.3.2 ](creating-oracle-data-guard-logical-standby.md#GUID-E5D227F2-C456-44F3-AEEA-2C8340439EF3) , [ 11.1.1.4 ](managing-oracle-data-guard-logical-standby-databases.md#GUID-97797605-D5BB-49B7-8E9D-D3E7C56A767C)
  * unrecoverable operations 
    * backing up after  [ 15.4.3 ](examples-of-using-oracle-data-guard.md#GUID-55881B7E-55C1-4342-94DA-A75739504980)
  * unsupported PL/SQL supplied packages  [ C.13.2 ](data-type-ddl-support-on-logical-standby-databases.md#GUID-703C8569-4298-48ED-98F5-6146A6D46F3F)
  * upgrading 
    * Oracle Database software  [ 2.3.2 ](getting-started-with-oracle-data-guard.md#GUID-A9F7D87E-49EB-4CE9-A076-1F35F35C0EAB) , [ 13.1 ](using-sql-apply-to-perform-rolling-upgrade.md#GUID-701A35C5-B545-43DD-8854-F6948E0BB32A) , [ B ](upgrading-patching-downgrading-oracle-data-guard-configuration.md#GUID-A4FBE490-7E7E-4998-B80E-01C93C77AE2F) , [ B.1 ](upgrading-patching-downgrading-oracle-data-guard-configuration.md#GUID-784C06D8-9CFA-4244-BA55-D7003CAB3F1E)
      * setting the COMPATIBLE initialization parameter  [ B.5 ](upgrading-patching-downgrading-oracle-data-guard-configuration.md#GUID-253C94C1-8E40-4378-A0A5-2627C248D873)



* * *

##  V 

  * V$ARCHIVE_DEST view  [ A.1.2 ](troubleshooting-oracle-data-guard.md#GUID-E0F5B423-208F-4F7E-8CA4-0350D3A7CF0B)
    * displaying information for all destinations  [ 19 ](oracle-data-guard-in-oracle-database-views.md#GUID-FF72163C-78B0-475F-B4CE-135EF288A32A)
  * V$ARCHIVED_LOG view  [ 10.6.1.4 ](managing-oracle-data-guard-physical-standby-databases.md#GUID-E97EB4D8-48E0-4355-A878-9CA3E80C0AE8)
  * V$DATABASE view 
    * monitoring fast-start failover  [ 10.6 ](managing-oracle-data-guard-physical-standby-databases.md#GUID-0682EE24-B495-48E8-9E1F-52AC0E6FE486)
    * SWITCHOVER_STATUS column and  [ A.4.1 ](troubleshooting-oracle-data-guard.md#GUID-616EF287-25CE-4F6D-A0C3-9A4DEDA3A97C) , [ G.3.1 ](performing-oracle-data-guard-role-transitions.md#GUID-9D024B1F-54EF-4C95-A810-1FA09B7AE3E4)
  * V$DATAFILE view  [ 15.4.3 ](examples-of-using-oracle-data-guard.md#GUID-55881B7E-55C1-4342-94DA-A75739504980)
  * V$DATAGUARD_CONFIG view 
    * listing database names defined with LOG_ARCHIVE_CONFIG  [ 19 ](oracle-data-guard-in-oracle-database-views.md#GUID-FF72163C-78B0-475F-B4CE-135EF288A32A)
  * V$DATAGUARD_STATS view  [ 9.1.2 ](managing-oracle-data-guard-role-transitions.md#GUID-60804EE7-88C0-4CB7-AEE7-073EBCAFD1AC)
  * V$DATAGUARD_STATUS view  [ 10.6.1.6 ](managing-oracle-data-guard-physical-standby-databases.md#GUID-F60AAF04-10FE-4568-BA5E-792D150FEA28)
  * V$LOG_HISTORY view  [ 10.6.1.5 ](managing-oracle-data-guard-physical-standby-databases.md#GUID-6FF2CE1A-C0D8-4821-BF75-B219F9AA283A)
  * V$LOGSTDBY_PROCESS view  [ 11.3.4 ](managing-oracle-data-guard-logical-standby-databases.md#GUID-7E8E779A-E0EC-4C9D-8B77-1E80A60C077D) , [ 11.4.1 ](managing-oracle-data-guard-logical-standby-databases.md#GUID-5BD79860-B3B8-4E0D-9F55-2E9918B93292) , [ 11.7.3.1 ](managing-oracle-data-guard-logical-standby-databases.md#GUID-F105E232-364E-4F40-ADCE-E13EB3566A2E) , [ 11.7.3.2 ](managing-oracle-data-guard-logical-standby-databases.md#GUID-97300F48-66C4-4F9E-888D-DEFC0DDC10C8)
  * V$LOGSTDBY_PROGRESS view  [ 11.3.5 ](managing-oracle-data-guard-logical-standby-databases.md#GUID-DAF00126-1FDA-4980-AD24-1360472ADB49)
    * RESTART_SCN column  [ 11.1.1.3 ](managing-oracle-data-guard-logical-standby-databases.md#GUID-F0FDEEF3-B4EA-40F3-BF7B-2167D33D91C8)
  * V$LOGSTDBY_STATE view  [ 9.1.2 ](managing-oracle-data-guard-role-transitions.md#GUID-60804EE7-88C0-4CB7-AEE7-073EBCAFD1AC) , [ 11.3.6 ](managing-oracle-data-guard-logical-standby-databases.md#GUID-9E8AFFE3-E4C2-46EB-A733-B48793DB4F1F) , [ 11.4.1 ](managing-oracle-data-guard-logical-standby-databases.md#GUID-5BD79860-B3B8-4E0D-9F55-2E9918B93292)
  * V$LOGSTDBY_STATS view  [ 11.3.7 ](managing-oracle-data-guard-logical-standby-databases.md#GUID-C72DA5B7-7FDD-46C8-9242-CD7D88DCDDB7)
    * failover characteristics  [ 11.3.3 ](managing-oracle-data-guard-logical-standby-databases.md#GUID-76033857-52EC-41B2-B130-CC9083F3FA99)
  * V$MANAGED_STANDBY view  [ 10.6.1.3 ](managing-oracle-data-guard-physical-standby-databases.md#GUID-31B49D22-414F-4802-9A48-8D8C55CD8CB6)
  * V$REDO_DEST_RESP_HISTOGRAM 
    * using to monitor synchronous redo transport response time  [ 7.6.2 ](oracle-data-guard-redo-transport-services.md#GUID-666D0186-B639-4DB2-9993-D1947230C50F)
  * VALID_FOR attribute  [ 17.17 ](LOG_ARCHIVE_DEST_n-parameter-attributes.md#GUID-2E1845CC-B3CA-4409-BACB-6AB40D6D42E5)
  * verifying 
    * logical standby databases  [ 4.3.6 ](creating-oracle-data-guard-logical-standby.md#GUID-930CFC64-4EFD-4CF3-A5D0-062FBBD069E1)
    * physical standby databases  [ 3.3.7 ](creating-oracle-data-guard-physical-standby.md#GUID-AAA6D97B-A345-4825-A320-B662BB16E2ED)
  * versions 
    * upgrading Oracle Database software  [ 13.1 ](using-sql-apply-to-perform-rolling-upgrade.md#GUID-701A35C5-B545-43DD-8854-F6948E0BB32A)
  * views 
    * DBA_LOGSTDBY_EVENTS  [ 11.3.1 ](managing-oracle-data-guard-logical-standby-databases.md#GUID-BEF12BA7-9397-4156-9ED3-BAC7935D7D96) , [ A.6 ](troubleshooting-oracle-data-guard.md#GUID-E2C9EEB9-CDCE-46B4-A7ED-D60B7FCD657A)
    * DBA_LOGSTDBY_LOG  [ 11.3.2 ](managing-oracle-data-guard-logical-standby-databases.md#GUID-646D305C-21C3-4590-8A6B-B2D02F90E8F2)
    * V$ARCHIVE_DEST  [ A.1.2 ](troubleshooting-oracle-data-guard.md#GUID-E0F5B423-208F-4F7E-8CA4-0350D3A7CF0B)
    * V$ARCHIVED_LOG  [ 10.6.1.4 ](managing-oracle-data-guard-physical-standby-databases.md#GUID-E97EB4D8-48E0-4355-A878-9CA3E80C0AE8)
    * V$DATAFILE  [ 15.4.3 ](examples-of-using-oracle-data-guard.md#GUID-55881B7E-55C1-4342-94DA-A75739504980)
    * V$DATAGUARD_STATUS  [ 10.6.1.6 ](managing-oracle-data-guard-physical-standby-databases.md#GUID-F60AAF04-10FE-4568-BA5E-792D150FEA28)
    * V$LOG_HISTORY  [ 10.6.1.5 ](managing-oracle-data-guard-physical-standby-databases.md#GUID-6FF2CE1A-C0D8-4821-BF75-B219F9AA283A)
    * V$LOGSTDBY_PROCESS  [ 11.3.4 ](managing-oracle-data-guard-logical-standby-databases.md#GUID-7E8E779A-E0EC-4C9D-8B77-1E80A60C077D)
    * V$LOGSTDBY_PROGRESS  [ 11.3.5 ](managing-oracle-data-guard-logical-standby-databases.md#GUID-DAF00126-1FDA-4980-AD24-1360472ADB49)
    * V$LOGSTDBY_STATE  [ 11.3.6 ](managing-oracle-data-guard-logical-standby-databases.md#GUID-9E8AFFE3-E4C2-46EB-A733-B48793DB4F1F)
    * V$LOGSTDBY_STATS  [ 11.3.7 ](managing-oracle-data-guard-logical-standby-databases.md#GUID-C72DA5B7-7FDD-46C8-9242-CD7D88DCDDB7)
    * V$MANAGED_STANDBY  [ 10.6.1.3 ](managing-oracle-data-guard-physical-standby-databases.md#GUID-31B49D22-414F-4802-9A48-8D8C55CD8CB6)



* * *

##  W 

  * wait events 
    * for redo transport services  [ 7.6.4 ](oracle-data-guard-redo-transport-services.md#GUID-EB15DAF9-40A8-4185-B4C2-F8BB4FBD33CE)
  * WAITING FOR DICTIONARY LOGS state  [ 11.4.1 ](managing-oracle-data-guard-logical-standby-databases.md#GUID-5BD79860-B3B8-4E0D-9F55-2E9918B93292)


