##  Preface {#GUID-B6209E95-9DA8-4D37-9BAD-3F000C7E3590} 

Oracle Data Guard is the most effective solution available today to protect the core asset of any enterprise—its data, and make it available on a 24x7 basis even in the face of disasters and other calamities. This guide describes Oracle Data Guard technology and concepts, and helps you configure and implement standby databases. 

###  Audience {#GUID-C5B240E0-11A7-4665-9088-8DF97B99304C} 

*Oracle Data Guard Concepts and Administration* is intended for database administrators (DBAs) who administer the backup, restoration, and recovery operations of an Oracle database system. 

To use this document, you should be familiar with relational database concepts and basic backup and recovery administration. You should also be familiar with the operating system environment under which you are running Oracle software. 

###  Documentation Accessibility {#GUID-E409CC44-9A8F-4043-82C8-6B95CD939296} 

For information about Oracle's commitment to accessibility, visit the Oracle Accessibility Program website at [ http://www.oracle.comhttps://docs.oracle.com/pls/topic/lookup?ctx=acc&id=docacc ](https://docs.oracle.com/pls/topic/lookup?ctx=acc&id=docacc) . 

Access to Oracle Support 

Oracle customers that have purchased support have access to electronic support through My Oracle Support. For information, visit [ http://www.oracle.comhttps://docs.oracle.com/pls/topic/lookup?ctx=acc&id=info ](https://docs.oracle.com/pls/topic/lookup?ctx=acc&id=info) or visit [ http://www.oracle.comhttps://docs.oracle.com/pls/topic/lookup?ctx=acc&id=trs ](https://docs.oracle.com/pls/topic/lookup?ctx=acc&id=trs) if you are hearing impaired. 

###  Related Documents {#GUID-99FDFF14-5960-4561-9E58-A936F6818827} 

Readers of *Oracle Data Guard Concepts and Administration* should also read: 

  * The beginning of [ *Oracle Database Concepts* ](https://docs.oracle.com/pls/topic/lookup?ctx=en/database/oracle/oracle-database/23/sbydb&id=CNCPT-GUID-A42A6EF0-20F8-4F4B-AFF7-09C100AE581E) , that provides an overview of the concepts and terminology related to the Oracle database and serves as a foundation for the more detailed information in this guide. 

  * The chapters in the [ *Oracle Database Administrator's Guide* ](https://docs.oracle.com/pls/topic/lookup?ctx=en/database/oracle/oracle-database/23/sbydb&id=ADMIN-GUID-EA8CC987-EF18-4434-B962-01312CD3A8AC) that deal with managing the control files, online redo log files, and archived redo log files. 

  * The chapter in the [ *Oracle Database Utilities* ](https://docs.oracle.com/pls/topic/lookup?ctx=en/database/oracle/oracle-database/23/sbydb&id=SUTIL-GUID-17FAE261-0972-4220-A2E4-44D479F519D4) that discusses LogMiner technology. 

  * [ *Oracle Data Guard Broker* ](https://docs.oracle.com/pls/topic/lookup?ctx=en/database/oracle/oracle-database/23/sbydb&id=DGBKR-GUID-723E5B73-A350-4B2E-AF3C-5EA4EFC83966) that describes the graphical user interface and command-line interface for automating and centralizing the creation, maintenance, and monitoring of Oracle Data Guard configurations. 

  * Oracle Enterprise Manager Cloud Control online Help system 




###  Conventions {#GUID-E4D25608-9AFE-4740-93B9-C3DCE99084D3} 

The following text conventions are used in this document: 

Convention  |  Meaning   
---|---  
boldface  |  Boldface type indicates graphical user interface elements associated with an action, or terms defined in text or the glossary.   
*italic*  |  Italic type indicates book titles, emphasis, or placeholder variables for which you supply particular values.   
` monospace ` |  Monospace type indicates commands within a paragraph, URLs, code in examples, text that appears on the screen, or text that you enter. 
