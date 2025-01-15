##  FSetting Archive Tracing {#GUID-515A27B2-5163-42E1-A78D-B6E979BA54EB} 

The Oracle database uses the ` LOG_ARCHIVE_TRACE ` initialization parameter to enable and control the generation of comprehensive trace information for log archiving and redo transport activity. 

The additional tracing that is output when ` LOG_ARCHIVE_TRACE ` is set to a nonzero value can appear in trace files for an archive process, ` RFS ` process, ` LGWR ` process, ` SYNC ` process, ` ASYNC ` process, foreground process, ` MRP ` process, recovery process, log apply process, startup process, shutdown process, and other processes that use redo transport services. Tracing information is written to the Automatic Diagnostic Repository. 

> **note:** See Also: 

  * [ *Oracle Database Reference* ](https://docs.oracle.com/pls/topic/lookup?ctx=en/database/oracle/oracle-database/23/sbydb&id=REFRN10093) for a complete description of the ` LOG_ARCHIVE_TRACE ` initialization parameter and its valid values 

  * [ *Oracle Database Administrator's Guide* ](https://docs.oracle.com/pls/topic/lookup?ctx=en/database/oracle/oracle-database/23/sbydb&id=ADMIN11261) for more information about the Automatic Diagnostic Repository 




###  Setting the LOG_ARCHIVE_TRACE Initialization Parameter {#GUID-E25745D3-B841-4F9B-87FF-4BB7D7415ED0} 

To enable, disable, or modify the ` LOG_ARCHIVE_TRACE ` parameter, you issue an ` ALTER SYSTEM ` SQL statement. 

The format for the ` LOG_ARCHIVE_TRACE ` parameter is as follows, where *`trace_level`* is an integer: 
    
    
    ```
    LOG_ARCHIVE_TRACE=trace_level
    
    ```

The following is an example of setting the ` LOG_ARCHIVE_TRACE ` parameter: 
    
    
    ```
    SQL> ALTER SYSTEM SET LOG_ARCHIVE_TRACE=15;
    
    ```

You can combine tracing levels by setting the value of the ` LOG_ARCHIVE_TRACE ` parameter to the sum of the individual levels. For instance, in the previous example, setting the ` LOG_ARCHIVE_TRACE ` parameter to a value of 15 sets trace levels 1, 2, 4, and 8. 

For a complete list and description of valid ` LOG_ARCHIVE_TRACE ` values, see [ *Oracle Database Reference* ](https://docs.oracle.com/pls/topic/lookup?ctx=en/database/oracle/oracle-database/23/sbydb&id=REFRN10093) . 

The following are examples of the ARC0 trace data generated on the primary site by the archiving of log file 387 to two different destinations: the service ` standby1 ` and the local directory ` /oracle/dbs ` . 

> **note:** 

The level numbers do not appear in the actual trace output; they are shown here for clarification only. 
    
    
    ```
    Level   Corresponding entry content (sample) 
    -----   -------------------------------- 
    ( 1)    ARC0: Begin archiving log# 1 seq# 387 thrd# 1 
    ( 4)    ARC0: VALIDATE 
    ( 4)    ARC0: PREPARE 
    ( 4)    ARC0: INITIALIZE 
    ( 4)    ARC0: SPOOL 
    ( 8)    ARC0: Creating archive destination 2 : 'standby1' 
    (16)    ARC0:  Issuing standby Create archive destination at 'standby1' 
    ( 8)    ARC0: Creating archive destination 1 : '/oracle/dbs/d1arc1_387.log' 
    (16)    ARC0:  Archiving block 1 count 1 to : 'standby1' 
    (16)    ARC0:  Issuing standby Archive of block 1 count 1 to 'standby1' 
    (16)    ARC0:  Archiving block 1 count 1 to :  '/oracle/dbs/d1arc1_387.log' 
    ( 8)    ARC0: Closing archive destination 2  : standby1 
    (16)    ARC0:  Issuing standby Close archive destination at 'standby1' 
    ( 8)    ARC0: Closing archive destination 1  :  /oracle/dbs/d1arc1_387.log 
    ( 4)    ARC0: FINISH 
    ( 2)    ARC0: Archival success destination 2 : 'standby1' 
    ( 2)    ARC0: Archival success destination 1 : '/oracle/dbs/d1arc1_387.log' 
    ( 4)    ARC0: COMPLETE, all destinations archived 
    (16)    ARC0: ArchivedLog entry added: /oracle/dbs/d1arc1_387.log 
    (16)    ARC0: ArchivedLog entry added: standby1 
    ( 4)    ARC0: ARCHIVED 
    ( 1)    ARC0: Completed archiving log# 1 seq# 387 thrd# 1 
     
    (32)  Propagating archive 0 destination version 0 to version 2 
             Propagating archive 0 state version 0 to version 2 
             Propagating archive 1 destination version 0 to version 2 
             Propagating archive 1 state version 0 to version 2 
             Propagating archive 2 destination version 0 to version 1 
             Propagating archive 2 state version 0 to version 1 
             Propagating archive 3 destination version 0 to version 1 
             Propagating archive 3 state version 0 to version 1 
             Propagating archive 4 destination version 0 to version 1 
             Propagating archive 4 state version 0 to version 1 
     
    (64) ARCH: changing ARC0 KCRRNOARCH->KCRRSCHED 
            ARCH: STARTING ARCH PROCESSES 
            ARCH: changing ARC0 KCRRSCHED->KCRRSTART 
            ARCH: invoking ARC0 
            ARC0: changing ARC0 KCRRSTART->KCRRACTIVE 
            ARCH: Initializing ARC0 
            ARCH: ARC0 invoked 
            ARCH: STARTING ARCH PROCESSES COMPLETE 
            ARC0 started with pid=8 
            ARC0: Archival started
    
    ```

The following is the trace data generated by the remote file server (RFS) process on the standby site as it receives archived redo log file 387 in directory ` /stby ` and applies it to the standby database: 
    
    
    ```
    level    trace output (sample) 
    ----    ------------------ 
    ( 4)      RFS: Startup received from ARCH pid 9272 
    ( 4)      RFS: Notifier 
    ( 4)      RFS: Attaching to standby instance 
    ( 1)      RFS: Begin archive log# 2 seq# 387 thrd# 1 
    (32)      Propagating archive 5 destination version 0 to version 2 
    (32)      Propagating archive 5 state version 0 to version 1 
    ( 8)      RFS: Creating archive destination file: /stby/parc1_387.log 
    (16)      RFS:  Archiving block 1 count 11 
    ( 1)      RFS: Completed archive log# 2 seq# 387 thrd# 1 
    ( 8)      RFS: Closing archive destination file: /stby/parc1_387.log 
    (16)      RFS: ArchivedLog entry added: /stby/parc1_387.log 
    ( 1)      RFS: Archivelog seq# 387 thrd# 1 available 04/02/99 09:40:53 
    ( 4)      RFS: Detaching from standby instance 
    ( 4)      RFS: Shutdown received from ARCH pid 9272
    ```