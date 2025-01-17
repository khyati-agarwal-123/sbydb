## 17LOG_ARCHIVE_DEST_n Parameter Attributes {#GUID-B04FC22C-F0B0-440F-BAF4-182EE547FCC1}

The script content on this page is for navigation purposes only and does not alter the content in any way.

This is a list of the attributes for the `LOG_ARCHIVE_DEST_`*n*** initialization parameter, (where *n* is an integer between 1 and 31). 

* [AFFIRM and NOAFFIRM](LOG_ARCHIVE_DEST_n-parameter-attributes.md#GUID-559248E6-B153-4B7B-A8A9-9CEE102088F0)

* [ALTERNATE](LOG_ARCHIVE_DEST_n-parameter-attributes.md#GUID-88222D10-22FD-4F3B-BBE5-B59C8ABEC725)

* [COMPRESSION](LOG_ARCHIVE_DEST_n-parameter-attributes.md#GUID-DD58C7C2-EE9D-4632-94C9-F6C652718006)

* [DB_UNIQUE_NAME](LOG_ARCHIVE_DEST_n-parameter-attributes.md#GUID-59781B0B-B8EA-4676-9F93-A7A72E021D7A)

* [DELAY](LOG_ARCHIVE_DEST_n-parameter-attributes.md#GUID-C014D9BB-79D3-4E49-9477-A5CBF0E88167)

* [ENCRYPTION](LOG_ARCHIVE_DEST_n-parameter-attributes.md#GUID-72FD2420-BD1A-4121-94BE-3A3B8EFFA3A5)

* [GROUP](LOG_ARCHIVE_DEST_n-parameter-attributes.md#GUID-99D83167-AA3A-4311-B2A9-6E38A353E33C)

* [LOCATION and SERVICE](LOG_ARCHIVE_DEST_n-parameter-attributes.md#GUID-9ACDEC6C-C2E0-4EDA-B66D-B1F819D3368D) (`LOCATION` is not supported for `LOG_ARCHIVE_DEST_11` through `LOG_ARCHIVE_DEST_31)`

* [MANDATORY](LOG_ARCHIVE_DEST_n-parameter-attributes.md#GUID-C4AD4812-D4A6-41C9-8F03-4C87B64F1D4D) (not supported for `LOG_ARCHIVE_DEST_11` through `LOG_ARCHIVE_DEST_31`) 

* [MAX_FAILURE](LOG_ARCHIVE_DEST_n-parameter-attributes.md#GUID-3655C0B5-E669-4475-95B9-01A67AE6A144)

* [NET_TIMEOUT](LOG_ARCHIVE_DEST_n-parameter-attributes.md#GUID-5067F60F-780E-429E-A0FB-CB121B8DF321)

* [NOREGISTER](LOG_ARCHIVE_DEST_n-parameter-attributes.md#GUID-B23425C5-FED4-4084-9D82-A2B7888AE1E5)

* [PRIORITY](LOG_ARCHIVE_DEST_n-parameter-attributes.md#GUID-B5AC0CAF-92FB-4CE6-B164-CC4328E389F5)

* [REOPEN](LOG_ARCHIVE_DEST_n-parameter-attributes.md#GUID-AE01BD16-8FD7-4C4D-BBB6-156281C947A7)

* [SYNC and ASYNC](LOG_ARCHIVE_DEST_n-parameter-attributes.md#GUID-E3626025-2E4C-4C61-B5DF-2BB106F0A349) (`SYNC` is not supported for `LOG_ARCHIVE_DEST_11` through `LOG_ARCHIVE_DEST_31)`

* [TEMPLATE](LOG_ARCHIVE_DEST_n-parameter-attributes.md#GUID-9E06C1CD-DA57-44FE-8BC4-720A890EB657)

* [VALID_FOR](LOG_ARCHIVE_DEST_n-parameter-attributes.md#GUID-2E1845CC-B3CA-4409-BACB-6AB40D6D42E5)




Usage Notes

* Each database in an Oracle Data Guard configuration typically has one destination with the `LOCATION` attribute for the archival of the online and standby redo logs and one destination with the `REMOTE` attribute for every other database in the configuration. 

* If configured, each `LOG_ARCHIVE_DEST_1` through `LOG_ARCHIVE_DEST_10` destination must contain either a `LOCATION` or `SERVICE` attribute to specify a local disk directory or a remotely accessed database, respectively. Each `LOG_ARCHIVE_DEST_11` through `LOG_ARCHIVE_DEST_31` destination must contain a `SERVICE` attribute. <br>All other attributes are optional.

* `LOG_ARCHIVE_DEST_11` through `LOG_ARCHIVE_DEST_31` can only be used when the `COMPATIBLE` initialization parameter is set to 11.2.0.0 or later. 




> **note:** 

Several attributes of the `LOG_ARCHIVE_DEST_`*n* initialization parameter have been deprecated. These attributes are supported for backward compatibility only and are documented in the [*Oracle Database Reference*](https://docs.oracle.com/pls/topic/lookup?ctx=en/database/oracle/oracle-database/23/sbydb&id=REFRN10086). 

> **note:** See Also: 

[Redo Transport Services](oracle-data-guard-redo-transport-services.md#GUID-0EC71C7D-A80B-40AA-93E5-28BB7E24ED31) for more information about defining `LOG_ARCHIVE_DEST_*`n`*` destinations and setting up redo transport services 

### AFFIRM and NOAFFIRM {#GUID-559248E6-B153-4B7B-A8A9-9CEE102088F0}

The `AFFIRM` and `NOAFFIRM` attributes control whether a redo transport destination acknowledges received redo data before or after writing it to the standby redo log. 

Definitions of each option are as follows:

* `AFFIRM`—specifies that a redo transport destination acknowledges received redo data *after* writing it to the standby redo log. 

* `NOAFFIRM`—specifies that a redo transport destination acknowledges received redo data *before* writing it to the standby redo log. 




Category | AFFIRM | NOAFFIRM  
---|---|---  
Data type |  Keyword |  Keyword  
Valid values |  Not applicable |  Not applicable  
Default Value |  Not applicable |  Not applicable  
Requires attributes |  `SERVICE` |  `SERVICE`  
Conflicts with attributes |  `NOAFFIRM` |  `AFFIRM`  
Corresponds to |  `AFFIRM` column of the `V$ARCHIVE_DEST` view  |  `AFFIRM` column of the `V$ARCHIVE_DEST` view   

Usage Notes

* If neither the `AFFIRM` nor the `NOAFFIRM` attribute is specified, then the default is `AFFIRM` when the `SYNC` attribute is specified and `NOAFFIRM` when the `ASYNC` attribute is specified. 

* Specification of the `AFFIRM` attribute without the `SYNC` attribute is deprecated and will not be supported in future releases. 




> **note:** See also: 

[SYNC and ASYNC](LOG_ARCHIVE_DEST_n-parameter-attributes.md#GUID-E3626025-2E4C-4C61-B5DF-2BB106F0A349) attributes 

Example

The following example shows the `AFFIRM` attribute for a remote destination. 
```
LOG_ARCHIVE_DEST_3='SERVICE=stby1 SYNC AFFIRM'
LOG_ARCHIVE_DEST_STATE_3=ENABLE
```


### ALTERNATE {#GUID-88222D10-22FD-4F3B-BBE5-B59C8ABEC725}

The `ALTERNATE` attribute specifies an alternate archiving destination to be used when the original destination fails. 

> **note:** As of Oracle Database 12*c* Release 2 (12.2.0.1), you can expand the number of alternate archive destinations and functionality by using the `GROUP` and `PRIORITY` attributes in place of the `ALTERNATE` attribute for remote log archive destinations. This new method cannot be used in conjunction with the `ALTERNATE` attribute method. For more information, see [Alternate Destinations](creating-oracle-data-guard-far-sync-instance.md#GUID-5CA464E7-40B6-486C-AF95-2F04493EB2DD). 

The `ALTERNATE` attribute is reserved for configuring alternate local archiving destinations. For backwards compatibility, examples of using `ALTERNATE` for remote log archiving destination are provided in [Using the ALTERNATE Attribute to Configure Remote Alternate Destinations](using-ALTERNATE-attribute-to-configure-alternate-destinations.md#GUID-7055337B-A852-402F-8B2F-2E119CBE6437). 

Category | ALTERNATE=LOG_ARCHIVE_DEST_n  
---|---  
Data Type |  String  
Valid Value |  A `LOG_ARCHIVE_DEST_`*n* destination, where *n* is a value from 1 through 10.   
Default Value |  None. If an alternate destination is not specified, then redo transport services do not automatically change to another destination.  
Requires attributes |  NoneFoot 1  
Conflicts with attributes |  `GROUP` and `PRIORITY`Foot 2  
Corresponds to |  `ALTERNATE` and `STATUS` columns of the `V$ARCHIVE_DEST` view   

Footnote 1 

Although it is not mandatory that `MAX_FAILURE` be used with `ALTERNATE`, a nonzero `MAX_FAILURE` value is required for an alternate to become active. Using `ALTERNATE` with the default value of `MAX_FAILURE` (zero), does not result in any change in behavior. 

Footnote 2 

If the `REOPEN` attribute is specified with a nonzero value, then an alternate is not activated until the number of failures is greater than or equal to the value of `MAX_FAILURE`, with a minimum time period between attempts equal to the value of `REOPEN` (in seconds). 

Usage Notes

* The `ALTERNATE` attribute is optional. If an alternate destination is not specified, then archiving services do not automatically change to another destination if the original destination fails. 

* You can specify only one alternate destination for each local `LOG_ARCHIVE_DEST_n` parameter (`LOCATION=`…). 

* An alternate destination should specify a different disk location on the same local primary or standby database system, as shown in the examples below. 

* To configure an alternate destination, set its `LOG_ARCHIVE_DEST_STATE_n` parameter to `ALTERNATE`. 

* To manually enable an alternate destination, set its `LOG_ARCHIVE_DEST_STATE_n` parameter to `ENABLE`. 

* If no enabled destinations reference an alternate destination, then the alternate destination is assumed to be deferred, because there is no automatic method of enabling the alternate destination. However, you can enable (or defer) alternate destinations at runtime using the `ALTER SYSTEM` statement. 

* Any destination can be designated as an alternate destination, given the following restrictions:

* At least one local destination is enabled.

* The number of enabled destinations must meet the defined `LOG_ARCHIVE_MIN_SUCCEED_DEST` parameter value. 

* A destination cannot be its own alternate.

* When a destination fails, its alternate destination is enabled on the next archival operation. There is no support for enabling the alternate destination in the middle of the archival operation because that would require rereading already processed blocks. 

* If an alternate destination is not specified, or if `MAX_FAILURE` is set to zero (the default), then archiving services do not automatically change to another destination if the original destination fails. 




Examples

These examples are included to illustrate basic concepts and are not meant to be used exactly as shown. They will be different in your configuration depending on your local archiving setup.

The following example shows a sample initialization parameter file in which a local archiving destination `LOG_ARCHIVE_DEST_1` automatically fails over to the alternate destination `LOG_ARCHIVE_DEST_2` on the next archival operation if an error occurs, such as a write failure or an allocation failure if the device were to become full. 

Example 17-1 Automatically Failing Over to an Alternate Local Destination
```
LOG_ARCHIVE_DEST_1='LOCATION=/disk1 MANDATORY MAX_FAILURE=1
ALTERNATE=LOG_ARCHIVE_DEST_2'

LOG_ARCHIVE_DEST_STATE_1=ENABLE

LOG_ARCHIVE_DEST_STATE_2=ALTERNATE

LOG_ARCHIVE_DEST_2='LOCATION=/disk2 MANDATORY'
```


To resume archiving to the original destination, `LOG_ARCHIVE_DEST_1`, you must re-enable it manually. Then you must reset `LOG_ARCHIVE_DEST_2` to its former alternate state to avoid having two active local archiving destinations. To do this, set the `LOG_ARCHIVE_DEST_STATE_n` parameters back to their original values, as follows: 
```
ALTER SYSTEM SET LOG_ARCHIVE_DEST_STATE_1=ENABLE

ALTER SYSTEM SET LOG_ARCHIVE_DEST_STATE_2=ALTERNATE
```


You can automate this fallback mechanism. Pair the original destination and the alternate destination by specifying an `ALTERNATE` attribute that points back to the preferred destination, as shown in the sample initialization parameter file in [Example 17-2](LOG_ARCHIVE_DEST_n-parameter-attributes.md#GUID-88222D10-22FD-4F3B-BBE5-B59C8ABEC725__I77382). 

Example 17-2 Automatic Local Alternate Fallback
```
LOG_ARCHIVE_DEST_1='LOCATION=/disk1 MANDATORY MAX_FAILURE=1
ALTERNATE=LOG_ARCHIVE_DEST_2'

LOG_ARCHIVE_DEST_STATE_1=ENABLE

LOG_ARCHIVE_DEST_STATE_2=ALTERNATE

LOG_ARCHIVE_DEST_2='LOCATION=/disk2 MANDATORY
ALTERNATE=LOG_ARCHIVE_DEST_1'
```


If `LOG_ARCHIVE_DEST_1` becomes available again, then Oracle Data Guard automatically sets it to become the active local archiving destination and resets `LOG_ARCHIVE_DEST_2` as its alternate. 

### COMPRESSION {#GUID-DD58C7C2-EE9D-4632-94C9-F6C652718006}

The `COMPRESSION` attribute is used to specify whether redo data is compressed before transmission to a redo transport destination. 

> **note:** 

Redo transport compression is a feature of the Oracle Advanced Compression option. You must purchase a license for this option before using the redo transport compression feature.

Category | COMPRESSION=[ENABLE | DISABLE | ZLIB | LZO]  
---|---  
Data Type |  Boolean  
Valid values |  `ENABLE`, `DISABLE`, `ZLIB`, or `LZO`  
Default value |  `DISABLE`  
Requires attributes |  None  
Conflicts with attributes |  None  
Corresponds to |  `COMPRESSION` column of the `V$ARCHIVE_DEST` view   

Usage Notes

* The `ENABLE` option enables compression and uses the ZLIB compression algorithm by default. 

* The `COMPRESSION` attribute is optional. If it is not specified, the default compression behavior is `DISABLE`. 

* For Oracle Data Guard `SYNC` connection strings that also use the Oracle Data Guard `COMPRESSION` attribute, be sure the `SQLNET.COMPRESSION` configuration parameter is set to disabled (set to off) in the `sqlnet.ora` file. See [*Oracle Database Net Services Reference*](https://docs.oracle.com/pls/topic/lookup?ctx=en/database/oracle/oracle-database/23/sbydb&id=NETRF1972) for more information about the `SQLNET.COMPRESSION` parameter. 




Example

The following example shows the `COMPRESSION` attribute with the `LOG_ARCHIVE_DEST_`*n* parameter. Since the `ENABLE` option is specified, the ZLIB compression algorithm is used. 
```
LOG_ARCHIVE_DEST_3='SERVICE=denver SYNC COMPRESSION=ENABLE'
LOG_ARCHIVE_DEST_STATE_3=ENABLE
```


### DB_UNIQUE_NAME {#GUID-59781B0B-B8EA-4676-9F93-A7A72E021D7A}

The `DB_UNIQUE_NAME` attribute specifies a unique name for the database at this destination. 

Category | DB_UNIQUE_NAME=name  
---|---  
Data Type |  String  
Valid values |  The name must match the value that was defined for this database with the `DB_UNIQUE_NAME` parameter.   
Default value |  None  
Requires attributes |  None  
Conflicts with attributes |  None  
Corresponds to |  `DB_UNIQUE_NAME` column of the `V$ARCHIVE_DEST` view   

Usage Notes

* This attribute *is optional* if: 

* The `LOG_ARCHIVE_CONFIG=DG_CONFIG` initialization parameter is not specified. 

* This is a local destination (specified with the `LOCATION` attribute). 

* This attributes *is required* if the `LOG_ARCHIVE_CONFIG=DG_CONFIG` initialization parameter is specified and if this is a remote destination (specified with the `SERVICE` attribute). 

* Use the `DB_UNIQUE_NAME` attribute to clearly identify the relationship between a primary and standby databases. This attribute is particularly helpful if there are multiple standby databases in the Oracle Data Guard configuration. 

* The name specified by the `DB_UNIQUE_NAME` must match one of the `DB_UNIQUE_NAME` values in the `DG_CONFIG` list. Redo transport services validate that the `DB_UNIQUE_NAME` attribute of the database at the specified destination matches the `DB_UNIQUE_NAME` attribute or the connection to that destination is refused. 

* The name specified by the `DB_UNIQUE_NAME` attribute must match the name specified by the `DB_UNIQUE_NAME` initialization parameter of the database identified by the destination. 




Example

In the following example, the `DB_UNIQUE_NAME` parameter specifies `boston` (`DB_UNIQUE_NAME=boston`), which is also specified with the `DB_UNIQUE_NAME` attribute on the `LOG_ARCHIVE_DEST_1` parameter. The `DB_UNIQUE_NAME` attribute on the `LOG_ARCHIVE_DEST_2` parameter specifies the `chicago` destination. Both `boston` and `chicago` are listed in the `LOG_ARCHIVE_CONFIG=DG_CONFIG` parameter. 
```
DB_UNIQUE_NAME=boston
LOG_ARCHIVE_CONFIG='DG_CONFIG=(chicago,boston,denver)'
LOG_ARCHIVE_DEST_1='LOCATION=/arch1/
VALID_FOR=(ALL_LOGFILES,ALL_ROLES)
DB_UNIQUE_NAME=boston'
LOG_ARCHIVE_DEST_2='SERVICE=Sales_DR
VALID_FOR=(ONLINE_LOGFILES,PRIMARY_ROLE)
DB_UNIQUE_NAME=chicago'
```


### DELAY {#GUID-C014D9BB-79D3-4E49-9477-A5CBF0E88167}

The `DELAY` attribute specifies a minimum time lag between when redo data from the primary site is archived on a standby site and when the archived redo log file is applied to the standby database or any standbys cascaded from it. 

Category | DELAY[=minutes]  
---|---  
Data Type |  Numeric  
Valid values |  >=0 minutes   
Default Value |  30 minutes  
Requires attributes |  `SERVICE`  
Conflicts with attributes |  `LOCATION`, VALID_FOR=(*,STANDBY_ROLE)   
Corresponds to |  `DELAY_MINS` and `DESTINATION` columns of the `V$ARCHIVE_DEST` view   

Usage Notes

* The `DELAY` attribute is optional. By default there is no delay. 

* The `DELAY` attribute indicates the archived redo log files at the standby destination are not available for recovery until the specified time interval has expired. The time interval is expressed in minutes, and it starts when the redo data is successfully transmitted to, and archived at, the standby site. 

* The `DELAY` attribute may be used to protect a standby database from corrupted or erroneous primary data. However, there is a tradeoff because during failover it takes more time to apply all of the redo up to the point of corruption. 

* The `DELAY` attribute does not affect the transmittal of redo data to a standby destination. 

* If you have real-time apply enabled, then any delay that you set is ignored.

* Changes to the `DELAY` attribute take effect the next time redo data is archived (after a log switch). In-progress archiving is not affected. 

* You can override the specified delay interval at the standby site, as follows:

* For a physical standby database:
```
SQL> ALTER DATABASE RECOVER MANAGED STANDBY DATABASE NODELAY;
```


* For a logical standby database:
```
SQL> ALTER DATABASE START LOGICAL STANDBY APPLY NODELAY;
```


* The `DELAY` value that a cascaded standby uses is the value that was set for the `LOG_ARCHIVE_DEST_`*n* parameter on the primary that shipped the redo to the cascading standby. 




> **note:** See Also: 

[*Oracle Database SQL Language Reference*](https://docs.oracle.com/pls/topic/lookup?ctx=en/database/oracle/oracle-database/23/sbydb&id=SQLRF00802) for more information about these `ALTER DATABASE` statements 

Example

You can use the `DELAY` attribute to set up a configuration where multiple standby databases are maintained in varying degrees of synchronization with the primary database. However, this protection incurs some overhead during failover, because it takes Redo Apply more time to apply all the redo up to the corruption point. 

For example, assume primary database A has standby databases B and C. Standby database B is set up as the disaster recovery database and therefore has no time lag. Standby database C is set up with a 2-hour delay, which is enough time to allow user errors to be discovered before they are propagated to the standby database.

The following example shows how to specify the `DELAY` attribute for this configuration: 
```
LOG_ARCHIVE_DEST_1='LOCATION=/arch/dest MANDATORY'
LOG_ARCHIVE_DEST_STATE_1=ENABLE
LOG_ARCHIVE_DEST_2='SERVICE=stbyB SYNC AFFIRM'
LOG_ARCHIVE_DEST_STATE_2=ENABLE
LOG_ARCHIVE_DEST_3='SERVICE=stbyC DELAY=120'
LOG_ARCHIVE_DEST_STATE_3=ENABLE
```


> **note:** 

Alternatively, you can use Flashback Database to revert the database to a point-in-time or SCN in a different database incarnation as long as there is sufficient flashback log data. Using Flashback Database is described in [*Oracle Database Backup and Recovery User's Guide*](https://docs.oracle.com/pls/topic/lookup?ctx=en/database/oracle/oracle-database/23/sbydb&id=BRADV71000). 

### ENCRYPTION {#GUID-72FD2420-BD1A-4121-94BE-3A3B8EFFA3A5}

The `ENCRYPTION` attribute is used to specify whether redo data is encrypted before transmission to a Zero Data Loss Recovery Appliance (Recovery Appliance). 

> **note:** 

Redo transport encryption is allowed only for connections to a Recovery Appliance. Attempting to configure encryption on a log archive destination other than a Recovery Appliance results in an error. 

Category | ENCRYPTION=ENABLE or DISABLE  
---|---  
Data type |  Boolean  
Valid values |  `ENABLE` or `DISABLE`  
Default value |  `DISABLE`  
Requires attributes |  `SERVICE`  
Conflicts with attributes |  `COMPRESSION`, `SYNC`, and `LOCATION`  
Corresponds to |  `ENCRYPTION` column of the `V$ARCHIVE_DEST` view   

Usage Notes

* The `ENCRYPTION` attribute is optional. If it is not specified, then the default encryption behavior is `DISABLE`. 

* To use the `ENCRYPTION` attribute, you must set the `COMPATIBLE` initialization parameter to 11.2.0.4 or higher on the protected database. 




Example

The following example shows the `ENCRYPTION` attribute specified on the `LOG_ARCHIVE_DEST_`*n* parameter. 
```
LOG_ARCHIVE_DEST_3='SERVICE=denver ENCRYPTION=ENABLE'
LOG_ARCHIVE_DEST_STATE_3=ENABLE
```


> **note:** See Also: 

* [*Zero Data Loss Recovery Appliance Administrator's Guide*](https://docs.oracle.com/pls/topic/lookup?ctx=en/database/oracle/oracle-database/23/sbydb&id=AMAGD-GUID-328CB500-2AD3-4F42-A2C6-5657C0ABC568) for more information about redo encryption using the `LOG_ARCHIVE_DEST_*`n`*` parameter 

* [*Oracle Database Advanced Security Guide*](https://docs.oracle.com/pls/topic/lookup?ctx=en/database/oracle/oracle-database/23/sbydb&id=ASOAG-GUID-62AA9447-FDCD-4A4C-B563-32DE04D55952) for more information about transparent data encryption 




### GROUP {#GUID-99D83167-AA3A-4311-B2A9-6E38A353E33C}

The `GROUP` attribute is used to specify membership in a specific collection of log archive destinations. 

Groups are numbered 1 through 8. The default group (`GROUP=0`) is special in that it cannot be assigned. The default group is populated with all destinations that are not explicitly assigned to a group. 

Category | GROUP=integer  
---|---  
Data Type | Integer  
Valid Value | 1 through 8  
Default Value | 0  
Requires Attributes | SERVICE  
Conflicts with Attributes | `ALTERNATE`  
Corresponds to | Not applicable  

Usage Notes

* None




Examples

The following example is given to illustrate basic concepts and is not meant to be used exactly as shown. Depending on your configuration, there may be other parameters, such as `DB_UNIQUE_NAME`, that are required. 
```
LOG_ARCHIVE_DEST_1='SERVICE=FS1 GROUP=1'
LOG_ARCHIVE_DEST_2='SERVICE=FS2 GROUP=1'
LOG_ARCHIVE_DEST_3='SERVICE=FS3 GROUP=2'
LOG_ARCHIVE_DEST_4='SERVICE=FS4 GROUP=2'
```


> **note:** See Also: 

* [Assigning Log Archive Destinations to a Group](creating-oracle-data-guard-far-sync-instance.md#GUID-DB84EA69-8B64-4230-88AB-E83D5298C009)




### LOCATION and SERVICE {#GUID-9ACDEC6C-C2E0-4EDA-B66D-B1F819D3368D}

Each destination *must* specify either the `LOCATION` or the `SERVICE` attribute to identify either a local disk directory or a remote database destination where redo transport services can transmit redo data. 

`LOG_ARCHIVE_DEST_1` through `LOG_ARCHIVE_DEST_10` destinations can contain either a `LOCATION` attribute or a `SERVICE` attribute. 

`LOG_ARCHIVE_DEST_11` through `LOG_ARCHIVE_DEST_31` destinations can only contain a `SERVICE` attribute. 

Category | LOCATION=local_disk_directory or USE_DB_RECOVERY_FILE_DEST | SERVICE=net_service_name  
---|---|---  
Data type |  String value |  String value  
Valid values |  Not applicable |  Not applicable  
Default Value |  None |  None  
Requires attributes |  Not applicable |  Not applicable  
Conflicts with attributes |  `SERVICE`, `DELAY`, `NOREGISTER`, `SYNC`, `ASYNC`, `NET_TIMEOUT, AFFIRM,NOAFFIRM`, `COMPRESSION` |  `LOCATION`  
Corresponds to |  `DESTINATION` and `TARGET` columns of the `V$ARCHIVE_DEST` view  |  `DESTINATION` and `TARGET` columns of the `V$ARCHIVE_DEST` view   

Usage Notes

* Either the `LOCATION` or the `SERVICE` attribute must be specified. There is no default. 

* The `LOG_ARCHIVE_DEST_11` through `LOG_ARCHIVE_DEST_31` parameters do not support the `LOCATION` attribute. 

* If you are specifying multiple attributes, specify the `LOCATION` or `SERVICE` attribute first in the list of attributes. 

* You must specify at least one local disk directory with the `LOCATION` attribute. This ensures that local archived redo log files are accessible if media recovery of a database becomes necessary. You can specify up to thirty additional local or remote destinations. 

* For the `LOCATION` attribute, you can specify one of the following: 

* `LOCATION=` *`local_disk_directory`* <br>This specifies a unique directory path name for a disk directory on the system that hosts the database. This is the local destination for archived redo log files.

* `LOCATION=USE_DB_RECOVERY_FILE_DEST`<br>To configure a fast recovery area, specify the directory or Oracle Storage Manager disk group to serve as the fast recovery area using the `DB_RECOVERY_FILE_DEST` initialization parameter. 

* When you specify a `SERVICE` attribute: 

* You identify remote destinations by specifying the `SERVICE` attribute with a valid Oracle Net service name (`SERVICE=` *`net_service_name`* ) that identifies the remote Oracle database instance to which the redo data is sent. <br>The Oracle Net service name that you specify with the `SERVICE` attribute is translated into a connection descriptor that contains the information necessary for connecting to the remote database. 

> **note:** See Also: 

[*Oracle Database Net Services Administrator's Guide*](https://docs.oracle.com/pls/topic/lookup?ctx=en/database/oracle/oracle-database/23/sbydb&id=NETAG003) for details about setting up Oracle Net service names 

* Transmitting redo data to a remote destination requires a network connection and an Oracle database instance associated with the remote destination to receive the incoming redo data.

* To verify the current settings for `LOCATION` and `SERVICE` attributes, query the `V$ARCHIVE_DEST` fixed view: 

* The `TARGET` column identifies if the destination is local or remote to the primary database. 

* The `DESTINATION` column identifies the values that were specified for a destination. For example, the destination parameter value specifies the Oracle Net service name identifying the remote Oracle instance where the archived redo log files are located. 




Examples

The following example shows how to specify the LOCATION attribute:
```
LOG_ARCHIVE_DEST_2='LOCATION=/disk1/oracle/oradata/payroll/arch/'
LOG_ARCHIVE_DEST_STATE_2=ENABLE
```


The following example shows how to specify the `SERVICE` attribute: 
```
LOG_ARCHIVE_DEST_3='SERVICE=stby1'
LOG_ARCHIVE_DEST_STATE_3=ENABLE
```


### MANDATORY {#GUID-C4AD4812-D4A6-41C9-8F03-4C87B64F1D4D}

The `MANDATORY` attribute specifies that filled online log files must be successfully archived to the destination before they can be reused. 

Category | MANDATORY  
---|---  
Data type |  Keyword  
Valid values |  Not applicable  
Default value |  Not applicable  
Requires attributes |  Not applicable  
Conflicts with attributes |  Optional  
Corresponds to |  `BINDING` column of the `V$ARCHIVE_DEST` view   

Usage Notes

* The `LOG_ARCHIVE_DEST_11` through `LOG_ARCHIVE_DEST_31` parameters do not support the `MANDATORY` attribute. 

* If `MANDATORY` is not specified, then, by default, the destination is considered to be optional. <br>At least one destination must succeed, even if all destinations are optional. If archiving to an optional destination fails, the online redo log file is still available for reuse and may be overwritten eventually. However, if the archival operation of a mandatory destination fails, online redo log files cannot be overwritten.

* The `LOG_ARCHIVE_MIN_SUCCEED_DEST=`*`n`* parameter (where *`n`* is an integer from 1 to 10) specifies the number of destinations that must archive successfully before online redo log files can be overwritten. <br>All `MANDATORY` destinations and optional local destinations contribute to satisfying the `LOG_ARCHIVE_MIN_SUCCEED_DEST=`*n* count. If the value set for the `LOG_ARCHIVE_MIN_SUCCEED_DEST` parameter is met, the online redo log file is available for reuse. For example, you can set the parameter as follows: 
```
# Database must archive to at least two locations before
# overwriting the online redo log files.
LOG_ARCHIVE_MIN_SUCCEED_DEST = 2
```


* You must have at least one local destination, which you can declare `MANDATORY` or leave as optional. <br>At least one local destination is operationally treated as mandatory, because the minimum value for the `LOG_ARCHIVE_MIN_SUCCEED_DEST` parameter is 1. 

* The failure of any mandatory destination makes the `LOG_ARCHIVE_MIN_SUCCEED_DEST` parameter irrelevant. 

* The `LOG_ARCHIVE_MIN_SUCCEED_DEST` parameter value cannot be greater than the number of mandatory destinations plus the number of optional local destinations. 

* The `BINDING` column of the `V$ARCHIVE_DEST` fixed view specifies how failure affects the archival operation 




Example

The following example shows the `MANDATORY` attribute: 
```
LOG_ARCHIVE_DEST_1='LOCATION=/arch/dest MANDATORY'
LOG_ARCHIVE_DEST_STATE_1=ENABLE
LOG_ARCHIVE_DEST_3='SERVICE=denver MANDATORY'
LOG_ARCHIVE_DEST_STATE_3=ENABLE
```


### MAX_FAILURE {#GUID-3655C0B5-E669-4475-95B9-01A67AE6A144}

The `MAX_FAILURE` attribute controls the consecutive number of times at a log switch that redo transport services attempts to reestablish communication and transmit redo data to a failed destination before the primary database gives up on the destination. 

Category | MAX_FAILURE=count  
---|---  
Data type |  Numeric  
Valid value |  >=0  
Default value |  For default group destinations the default value is 0. For non-default log archive destination group destinations, the default value is 1.  
Requires attributes |  `REOPEN`  
Conflicts with attributes |  None  
Corresponds to |  `MAX_FAILURE`, `FAILURE_COUNT`, and `REOPEN_SECS` columns of the `V$ARCHIVE_DEST` view   

Usage Notes for MAX_FAILURE

At certain intervals, archive destinations will be checked to determine if they need to be activated. Following are the scenarios in which archive destinations are checked:

* Standalone archive destination, with no alternate location set<br>If an archive destination is configured using `LOG_ARCHIVE_DEST_``n`, and no `ALTERNATE` attribute is set: 

* If `MAX_FAILURE` is set to 1, no attempts are made to reactivate this archive destination when it fails. 
* If `MAX_FAILURE` > 1, reactivation attempts are made until the number of consecutive failures is equal to the value of `MAX_FAILURE`. After this number is reached, no further attempts are made to reactivate the archive destination. 
* If `MAX_FAILURE` is 0, reactivation attempts are continuous. 
* Pair of archive destinations, with each being the alternate location for the other<br>Consider two archive destinations configured using the `LOG_ARCHIVE_DESTINATION_1` and `LOG_ARCHIVE_DESTINATION_2` parameters. The `ALTERNATE` attribute for the `LOG_ARCHIVE_DESTINATION_1` parameter is set to `LOG_ARCHIVE_DESTINATION_2` and that for `LOG_ARCHIVE_DESTINATION_2` is set to `LOG_ARCHIVE_DESTINATION_1`. The state of the preferred destination is set to `ENABLE` and the other archive destination is set to `ALTERNATE`. 

* If the value of `MAX_FAILURE` for the preferred destination is set to zero, the alternate destination is never activated. <br>It is not recommended to set `MAX_FAILUE` to zero for the preferred destination because it disables the ability to activate the alternate destination. 

* If the value of `MAX_FAILURE` for the preferred destination is set to a non-zero value, the archive destination is activated after the preferred destination has `MAX_FAILURE` failures. At this time, the preferred destination is assigned as the alternate destination. While the preferred destination is in the `ALTERNATE` state, regardless of the value of `REOPEN`, attempts are made to reactivate the preferred destination. 

After a non-preferred alternate location is activated, its behavior is similar to a standalone archive location with the following exceptions: 

* If the preferred destination is activated during one of the reactivation attempts, the non-preferred destination will be designated to the `ALTERNATE` state. 
* If the non-preferred destination reaches `MAX_FAILURE`, then both archive destinations are disabled and no further reactivation attempts are made. 
* Chain of archive destinations<br>Consider an example of three archive destinations configured using the `LOG_ARCHIVE_DEST_1`, `LOG_ARCHIVE_DEST_2`, and `LOG_ARCHIVE_DEST_3` parameters. `LOG_ARCHIVE_DEST_2` is the alternate destination for `LOG_ARCHIVE_DEST_1` and `LOG_ARCHIVE_DEST_3` is the alternate location for `LOG_ARCHIVE_DEST_2`. `LOG_ARCHIVE_DEST_3` has no alternate location. <br>The value of the `MAX_FAILURE` attribute for `LOG_ARCHIVE_DEST_1` and `LOG_ARCHIVE_DEST_2` must be set to a non-zero value. Otherwise, if `LOG_ARCHIVE_DEST_1` or `LOG_ARCHIVE_DEST_2` fails, its alternate destination is never activated because with `MAX_FAILURE` being zero, the destination is continuously checked for reactivation. Once the `MAX_FAILURE` value is reached for `LOG_ARCHIVE_DEST_1`, `LOG_ARCHIVE_DEST_2` is activated. When `LOG_ARCHIVE_DEST_2` reaches `MAX_FAILURE`, then `LOG_ARCHIVE_DEST_3` is activated. The behavior for `LOG_ARCHIVE_DEST_3` is the same as that for standalone archive destinations. 

* Archive destinations in a group<br>Consider a log archive group containing multiple log archive destinations. Each archive destination has a priority between 1 and 8. All archive destinations in the group must have `MAX_FAILURE` attribute set to a non-zero value. The alternate destination with the highest priority is activated first. If there are multiple alternate destinations with the highest available priority, one of them is selected. <br>If priority 8 becomes the highest priority functioning archive destination, then all archive destinations with priority 8 are activated. <br>If the currently active destination fails, the destination with the highest priority among the remaining destinations is activated, with the special case if destinations with priority 8 are activated.<br>Regardless of the value set for the `REOPEN` attribute, all alternate destinations that are at a higher priority than the currently-active archive destination are checked for reactivation at regular intervals. If a higher priority archive destination becomes functional, that archive destination is activated and the currently active archive destination will be put in the `ALTERNATE` state. 




Usage Notes for MAX_FAILURE in Oracle Database 12c Release 2 (12.2)

* For redo destinations that use the new `GROUP` and `PRIORITY` attributes, if the error count reaches the value specified for the `MAX_FAILURE` attribute, then the destination enters the ERROR state where it remains until it is found to be accessible. It is checked periodically depending on the value specified for the `REOPEN` attribute. 

* For default destinations in log archive groups (those redo destinations that do not use the new `GROUP` and `PRIORITY` attributes), the behavior of the `MAX_FAILURE` attribute is the same as it is in Oracle Database 12*c* Release 1 (12.1.0.1) 




Example

The following example allows redo transport services to try reconnecting up to three consecutive times at log switch to the failed destination, as long as each log switch is more than 5 seconds apart. If the archival operation fails after the third attempt, then the destination is treated as if the `REOPEN` attribute was not specified and the destination is marked as permanently failed until reset. 
```
LOG_ARCHIVE_DEST_1='LOCATION=/arc_dest REOPEN=5 MAX_FAILURE=3'
LOG_ARCHIVE_DEST_STATE_1=ENABLE
```


### NET_TIMEOUT {#GUID-5067F60F-780E-429E-A0FB-CB121B8DF321}

The `NET_TIMEOUT` attribute specifies the number of seconds that the LGWR background process blocks waiting for a redo transport destination to acknowledge redo data sent to it. 

If an acknowledgement is not received within `NET_TIMEOUT` seconds, an error is logged and the redo transport session to that destination is terminated. 

Category | NET_TIMEOUT=seconds  
---|---  
Data type |  Numeric  
Valid values |  1Foot 3 to 1200   
Default value |  30 seconds  
Requires attributes |  `SYNC`  
Conflicts with attributes |  `ASYNC` (If you specify the `ASYNC` attribute, redo transport services ignores it; no error is returned.)   
Corresponds to |  `NET_TIMEOUT` column of the `V$ARCHIVE_DEST` view of the primary database   

Footnote 3 

Although a minimum value of 1 second is allowed, Oracle recommends a minimum value of 8 to 10 seconds to avoid disconnecting from the standby database due to transient network errors.

Usage Notes

* The `NET_TIMEOUT` attribute is optional. However, if you do not specify the `NET_TIMEOUT` attribute it is set to 30 seconds, but the primary database can potentially stall. To avoid this situation, specify a small, nonzero value for the `NET_TIMEOUT` attribute so the primary database can continue operation after the user-specified timeout interval expires when waiting for status from the network server. 

* As of Oracle Database 12*c* Release 12.2 (12.2.0.1), there is a new database initialization parameter,` DATA_GUARD_SYNC_LATENCY`, which is global for all synchronous standby destinations. It defines the maximum amount of time (in seconds) that the primary database may wait before disconnecting subsequent destinations after at least one synchronous standby has acknowledged receipt of the redo. See [*Oracle Database Reference*](https://docs.oracle.com/pls/topic/lookup?ctx=en/database/oracle/oracle-database/23/sbydb&id=REFRN-GUID-A0965F0B-608C-4B68-85D1-9F14EFC191CC). 




Example

The following example shows how to specify a 10-second network timeout value on the primary database with the `NET_TIMEOUT` attribute. 
```
LOG_ARCHIVE_DEST_2='SERVICE=stby1 SYNC NET_TIMEOUT=10'
LOG_ARCHIVE_DEST_STATE_2=ENABLE
```


### NOREGISTER {#GUID-B23425C5-FED4-4084-9D82-A2B7888AE1E5}

The `NOREGISTER` attribute indicates that the location of the archived redo log file should not be recorded at the corresponding destination. 

Category | NOREGISTER  
---|---  
Data type |  Keyword  
Valid values |  Not applicable  
Default value |  Not applicable  
Requires attributes |  `SERVICE`  
Conflicts with attributes |  `LOCATION`  
Corresponds to |  `DESTINATION` and `TARGET` columns of the `V$ARCHIVE_DEST` view   

Usage Notes

* The `NOREGISTER` attribute *is optional* if the standby database destination is a part of an Oracle Data Guard configuration. 

* The `NOREGISTER` attribute *is required* if the destination is not part of an Oracle Data Guard configuration. 

* This attribute pertains to remote destinations only. The location of each archived redo log file is always recorded in the primary database control file.

* <br>This attribute must not be used in an Oracle Data Guard configuration that has no downstream GoldenGate mining setup. Using `NOREGISTER` in this scenario may cause problems during switchover operations. 




Example

The following example shows the `NOREGISTER` attribute: 
```
LOG_ARCHIVE_DEST_5='NOREGISTER'
```


### PRIORITY {#GUID-B5AC0CAF-92FB-4CE6-B164-CC4328E389F5}

The `PRIORITY` attribute is used to specify preference within a collection of log archive destinations. 

Priorities are numbered 1 through 8. A lower value represents a higher priority. The lowest priority (`PRIORITY=8`) is special in the sense that if that priority is active then all destinations at that priority are made active. If any higher priority destination returns to service, then that destination is made active and all low priority destinations are made inactive. 

Category | Priority=*integer*  
---|---  
Data Type | Integer  
Valid Value | 1 through 8  
Default Value | 1  
Requires attributes | `SERVICE`  
Conflicts with attributes | `ALTERNATE`  
Corresponds to | Not applicable  

Usage Notes

* The `PRIORITY` attribute is always used in conjunction with the `GROUP` attribute to provide an orderly enabling and fallback of members (redo destinations) of the group. 




Example

The following example is given to illustrate basic concepts and is not meant to be used exactly as shown. Depending on your configuration, there may be other parameters, such as `DB_UNIQUE_NAME`, that are required. A sample log archive destination setup that defines priorities is as follows: 
```
LOG_ARCHIVE_DEST_1='SERVICE=FS1 SYNC GROUP=1 PRIORITY=1'
LOG_ARCHIVE_DEST_2='SERVICE=FS2 SYNC GROUP=1 PRIORITY=1'
LOG_ARCHIVE_DEST_3='SERVICE=FS3 ASYNC GROUP=1 PRIORITY=2'
LOG_ARCHIVE_DEST_4='SERVICE=TS ASYNC GROUP=1 PRIORITY=3'
```


This declaration results in the following behavior:

* The primary ships to either of two preferred far sync instances, `FS1` or `FS2`. 

* If both `FS1` and `FS2` become unavailable, then the primary ships to `FS3` (in this case via `ASYNC`). 

* If either `FS1` or `FS2` become available while the primary is shipping to `FS3`, then the primary fails back to the available preferred log archive destination. 

* If all three higher priority log archive destinations fail, the primary begins shipping to `TS` (Terminal Standby). While shipping to `TS`, if `FS1`, `FS2`, or `FS3` become available, then the primary switches to the newly available higher priority destination. 




> **note:** See Also: 

* [Assigning Priorities to Log Archive Destinations in a Group](creating-oracle-data-guard-far-sync-instance.md#GUID-872B6548-DEC9-4BD1-9997-E1A4E3D2B808)




### REOPEN {#GUID-AE01BD16-8FD7-4C4D-BBB6-156281C947A7}

The `REOPEN` attribute specifies the minimum number of seconds before redo transport services try to reopen a failed destination. 

Category | REOPEN [=seconds]  
---|---  
Data Type |  Numeric  
Valid values |  >=0 seconds  
Default Value |  300 seconds  
Requires attributes |  None  
Conflicts with attributes |  `Not applicable`  
Corresponds to |  `REOPEN_SECS` and `MAX_FAILURE` columns of the `V$ARCHIVE_DEST` view   

Usage Notes

* The `REOPEN` attribute is optional. 

* Redo transport services attempt to reopen failed destinations at log switch time.

* Redo transport services check if the time of the last error plus the `REOPEN` interval is less than the current time. If it is, redo transport services attempt to reopen the destination. 

* `REOPEN` applies to all errors, not just connection failures. These errors include, but are not limited to, network failures, disk errors, and quota exceptions. 

* If you specify `REOPEN` for an optional destination, then it is possible for the Oracle database to overwrite online redo log files if there is an error. If you specify `REOPEN` for a `MANDATORY` destination, then redo transport services stall the primary database when it is not possible to successfully transmit redo data. When this situation occurs, consider the following options: 

* Change the destination by deferring the destination, specifying the destination as optional, or changing the `SERVICE` attribute value. 

* Specify an alternate destination.

* Disable the destination.




Example

The following example shows the `REOPEN` attribute. 
```
LOG_ARCHIVE_DEST_3='SERVICE=stby1 MANDATORY REOPEN=60'
LOG_ARCHIVE_DEST_STATE_3=ENABLE
```


### SYNC and ASYNC {#GUID-E3626025-2E4C-4C61-B5DF-2BB106F0A349}

The `SYNC` and `ASYNC` attributes specify whether the synchronous (`SYNC`) or asynchronous (`ASYNC`) redo transport mode is to be used. 

Category | SYNC | ASYNC  
---|---|---  
Data type |  Keyword |  Keyword  
Valid values |  Not applicable |  Not applicable  
Default value |  Not applicable |  None  
Requires attributes |  None |  None  
Conflicts with attributes |  `ASYNC`, `LOCATION` |  `SYNC`, `LOCATION`  
Corresponds to |  `TRANSMIT_MODE` column of the `V$ARCHIVE_DEST` view  |  `TRANSMIT_MODE` column of the `V$ARCHIVE_DEST` view   

Usage Notes

* The `LOG_ARCHIVE_DEST_11` through `LOG_ARCHIVE_DEST_31` parameters do not support the `SYNC` attribute. 

* The redo data generated by a transaction must have been received by every enabled destination that has the `SYNC` attribute before that transaction can commit. 

* On primary databases and logical standbys, destinations 1 through 10 default to `ASYNC` (real-time cascading). <br>On physical standbys, snapshot standbys, and far sync instances, destinations 1 through 10 default to `ARCH` transport mode. (Note that the `ARCH` attribute is deprecated; the use of `ARCH` in this situation simply indicates non-real-time cascading.) <br>Destinations 11 through 31 always default to `ASYNC`. 

* When the LGWR attribute is specified but you do not specify either the SYNC or ASYNC attribute, the default is SYNC.



> **note:** See Also: 

* [*Oracle Database Reference*](https://docs.oracle.com/pls/topic/lookup?ctx=en/database/oracle/oracle-database/23/sbydb&id=REFRN10086) for more information about` LOG_ARCHIVE_DEST_`*n* deprecated attributes 




Example

The following example shows the `SYNC` attribute with the `LOG_ARCHIVE_DEST_`*n* parameter. 
```
LOG_ARCHIVE_DEST_3='SERVICE=stby1 SYNC'
LOG_ARCHIVE_DEST_STATE_3=ENABLE
```


### TEMPLATE {#GUID-9E06C1CD-DA57-44FE-8BC4-720A890EB657}

The `TEMPLATE` attribute defines a directory specification and format template for names of archived redo log files at the destination. 

The template is used to generate a filename that is different from the default filename format defined by the `LOG_ARCHIVE_FORMAT` initialization parameter at the redo destination. 

Category | TEMPLATE=filename_template_%t_%s_%r  
---|---  
Data Type |  String value  
Valid values |  Not applicable  
Default value |  None  
Requires attributes ... |  `SERVICE`  
Conflicts with attributes ... |  `LOCATION`  
Corresponds to ... |  `REMOTE_TEMPLATE` and `REGISTER` columns of the `V$ARCHIVE_DEST` view   

Usage Notes

* The `TEMPLATE` attribute is optional. If `TEMPLATE` is not specified, archived redo logs are named using the value of the `LOG_ARCHIVE_FORMAT` initialization parameter. 

* The `TEMPLATE` attribute overrides the `LOG_ARCHIVE_FORMAT` initialization parameter setting at the remote archival destination. 

* The `TEMPLATE` attribute is valid only with remote destinations (specified with the `SERVICE` attribute). 

* The value you specify for *`filename_template`*  must contain the %s, %t, and %r directives described in [Table 17-1](LOG_ARCHIVE_DEST_n-parameter-attributes.md#GUID-9E06C1CD-DA57-44FE-8BC4-720A890EB657__BABFAFID). 

**Table: Directives for the TEMPLATE Attribute**

Directive | Description  
---|---  
%t |  Substitute the instance thread number.  
%T |  Substitute the instance thread number, zero filled.  
%s |  Substitute the log file sequence number.  
%S |  Substitute the log file sequence number, zero filled.  
%r |  Substitute the resetlogs ID.  
%R |  Substitute the resetlogs ID, zero filled.  

* The filename_template value is transmitted to the destination, where it is translated and validated before creating the filename.




### VALID_FOR {#GUID-2E1845CC-B3CA-4409-BACB-6AB40D6D42E5}

The `VALID_FOR` attribute specifies whether redo data gets written to a destination. 

The following factors are considered:

* Whether the database is *currently* running in the primary or the standby role 

* Whether online redo log files, standby redo log files, or both are *currently* being archived on the database at this destination 




Category | VALID_FOR=(redo_log_type, database_role)  
---|---  
Data Type |  String value  
Valid values |  Not applicable  
Default Value |  `VALID_FOR=(ALL_LOGFILES, ALL_ROLES)`  
Requires attributes |  None  
Conflicts with attributes |  None  
Corresponds to |  `VALID_NOW`, `VALID_TYPE`, and `VALID_ROLE` columns in the `V$ARCHIVE_DEST` view   

Usage Notes

* The `VALID_FOR` attribute is optional. However, Oracle recommends that the `VALID_FOR` attribute be specified for each redo transport destination at each database in an Oracle Data Guard configuration so that redo transport continues after a role transition to any standby database in the configuration. 

* To configure these factors for each `LOG_ARCHIVE_DEST_`*n* destination, you specify this attribute with a pair of keywords: `VALID_FOR=( *`redo_log_type`* , *`database_role`* )`: 

* The *`redo_log_type`* keyword identifies the destination as valid for archiving one of the following: 

* `ONLINE_LOGFILE`—This destination is valid only when archiving online redo log files. 

* `STANDBY_LOGFILE`—This destination is valid only when archiving standby redo log files. 

* `ALL_LOGFILES`— This destination is valid when archiving either online redo log files or standby redo log files. 

* The *`database_role`* keyword identifies the role in which this destination is valid for archiving: 

* `PRIMARY_ROLE`—This destination is valid only when the database is running in the primary role. 

* `STANDBY_ROLE`—This destination is valid only when the database is running in the standby role. 

* `ALL_ROLES`—This destination is valid when the database is running in either the primary or the standby role. 

* If you do not specify the `VALID_FOR` attribute for a destination, by default, archiving online redo log files and standby redo log files is enabled at the destination, regardless of whether the database is running in the primary or the standby role. This default behavior is equivalent to setting the `(ALL_LOGFILES,ALL_ROLES)` keyword pair on the `VALID_FOR` attribute. 

* The `VALID_FOR` attribute enables you to use the same initialization parameter file for both the primary and standby roles. 




Example

The following example shows the default `VALID_FOR` keyword pair: 
```
LOG_ARCHIVE_DEST_1='LOCATION=/disk1/oracle/oradata VALID_FOR=(ALL_LOGFILES, ALL_ROLES)'
```


When this database is running in either the primary or standby role, destination 1 archives all log files to the `/disk1/oracle/oradata` local directory location. 