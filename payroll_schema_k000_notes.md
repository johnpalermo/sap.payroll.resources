# Technical Notes for Payroll Schema K000 and Payroll Driver RPCALCK0

## Table of Contents

* [RPCALCK0 - Selection Screen](#rpcalck0---selection-screen)
* [RPCALCK0 - LOAD-OF-PROGRAM](#rpcalck0---load-of-program)
* [RPCALCK0 - INITIALIZATION](#rpcalck0---initialization)
* [RPCALCK0 - START-OF-SELECTION](#rpcalck0---start-of-selection)
    * [How the schema source gets read](#how-the-schema-source-gets-read)
* [K000 - Step by Step]

## RPCALCK0 - Selection Screen Used

* Log - Display log
* Test run (no update) - Leave unchecked

[:top:](#table-of-contents)

## RPCALCK0 - LOAD-OF-PROGRAM

* Initialise infotype reader factory to PNP mode.

[:top:](#table-of-contents)

## RPCALCK0 - INITIALIZATION

* Program: **RPCALCK0**
* Include: **RPCHRT09**
-----
* **Schema not read at this point**
* **PERFORM init_payroll_log (RPCALCK0/RPCHRT09/179)** - Initialize payroll log
    * Set ISO Code of the Country/Region to 'CA' (from table T500L) (251-256)
    * Refresh log tables (261)
        * LOG_REF[], LOG_TREE[] (**SAPLHRPL/LHRPLU04/FUNCTION HR_PL_REFRESH_LOG_TABLES**)
        * log_dat_international, log_dat_ca, etc. (**SAPLHRPL/LHRPLU29/FUNCTION hr_pl_clear_log_dat**)
    * Switch to CE Payroll if set in config
    * Get text and icon (pencil) for log variant (267-278)
    * If NOT CE Payroll, use PERNR-PERNR, else if CE, use PERSON-OBJID (281-289)
* **PERFORM check_special_pay USING special_pay_exists (RPCALCK0/RPCHRT09/180)** - Check for any special runs for the country
    * from table t52bx - for Canada:
        * A - Bonus payment
        * B - Correction accounting
        * C - Manual check
* Set references to tables (wpbp,rt,crt,bt,c0,it,ort,zl,etc) (183)
* Get offcycle info (185)
* Get BAdI **HR_PY_ENQUEUE** and class name **CL_EX_HR_PY_ENQUEUE**
    * Check for active implementation
* Check if **HRPY_LOAN_MSGTYP_BUKRS_CHANGE** BAdI has been implemented (203)

[:top:](#table-of-contents)

## RPCALCK0 - START-OF-SELECTION

* Program: **RPCALCK0**
* Include: **RPCHRT09**
-----
* If "Display log" selected on Selection Screen, then set:
    * **gv_detailed_log_requested = abap_true**
* Initialize process manager
    * Normally this gets skipped
* Set the name format (277)
    * If table T522F does not have an entry for REPID = 'RPCALCK0'
        * Set name format to '**01**':
            * Format **01** is used to edit names according to national rules in Personnel Administration (infotype/field: P0001-ENAME) and can be included in the infotype header as well as in HR standard reports.
        * **Note**: Table **T522F** is available in IMG (Personnel Management > Personnel Administration > Personal Data > Name Format > Assign Name Formatting to Programs)
* Lines 283 - 296 not yet documented.
* Line 297 - Get the schema - **PERFORM get_schema**(rpuscg00)
    * See next subsection

[:top:](#table-of-contents)

### How the schema source gets read

---
> **Note**: 
> * Schema source is read from cluster **PCL2, area PS**
> * See include RPCXPS00, lines 34-53, for an example.
>   * Macro **RP-IMP-C2-PS** (include RPCXPS00) reads the schema source
> * Internal table **AS-SOURCE** (include RPC2PS00) is where the schema source is read into
> * Text for the schema sources is read from cluster **PCL2, area PS**
> * See include RPCXPT00, lines 31-46, for an example.
>   * Macro **RP-IMP-C2-PT** (include RPCXPT00) reads the schema source text
> * Internal table **AS-SOURCE-TEXT** (include RPC2PT00) is where the schema source text is read into
---

* ... continuing with RPCALCK0 - **START-OF-SELECTION**
* **PERFORM get_schema** - RPCALCK0/RPCHRT09/**START-OF-SELECTION**/Line 297/**PERFORM get_schema**(rpuscg00)
    * **FORM GET_SCHEMA** - RPUSCG00/RPUSCG10/(Lines 174-213)
        * **perform check_schema_intern** using .... - Line 196
            * **FORM CHECK_SCHEMA_INTERN** - RPUSCG00/RPUSCG10/(Lines 229-309)
                * **perform check_time_stamps** using .... - Line 240
                    * **FORM CHECK_TIME_STAMPS** - RPUSCG00/RPUSCG10(Lines 529-628)
                        * **perform import_ps using sname ps-subrc** - Line 569
                            * **FORM IMPORT_PS USING SNAME PS-SUBRC.** - RPUSCG00/RPUSCG20(Lines 22-39)
                                * **AS-SOURCE** get filled here.
                                    * See note block above
                                * **FIELDS[]** gets filled with these rows:
                                    * Row 1 - FC-PGM_TYP = ABR
                                    * Row 2 - FC-SW_UPD = X
                                    * Row 3 - FC-SW_OPT_INFTY = X
                                    * Row 4 - FC-SW_READPZ = X
                                    * If in schema KIN0, CHECK ABR is commented out:
                                        * there will not be a row for FC-SW_CHECKPA03ABR
                                        * else:
                                            * FC-SW_CHECKPA03ABR = X
                                    * field FC-SW_CHECKPA03ABR affects the logic starting at line 76 of FORM INIT (RPCALCK0/RPCINI09), which is called in the START-OF-SELECTION.
                        * **perform import_pt using sy-langu sname pt-subrc** - Line 582
                            * **FORM IMPORT_PT USING SPRAS SNAME PT-SUBRC.** - RPUSCG00/RPUSCG20 (Lines 41-71)
                                * **AS-SOURCE-TEXT** get filled here.
                                    * See note block above
* **LOG_BASIC** stores the log and holds information from the schema, such as schema steps and step texts (more on this later)
    * Function module **HR_PL_MOVE_SCHEMA_TO_PLOG** handles this
        * Is called from PERFORM connect_as_with_schema_tree (332)


[:top:](#table-of-contents)
