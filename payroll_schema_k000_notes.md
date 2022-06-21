# Payroll Schema Notes for Schema K000 and Payroll Driver RPCALCK0

## Table of Contents

* [RPCALCK0 - Selection Screen](#rpcalck0---selection-screen)
* [RPCALCK0 - LOAD-OF-PROGRAM](#rpcalck0---load-of-program)
* [RPCALCK0 - INITIALIZATION](#rpcalck0---initialization)
* [RPCALCK0 - START-OF-SELECTION](#rpcalck0---start-of-selection)
    * [How the schema source gets read](#how-the-schema-source-gets-read)

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
* Check if HRPY_LOAN_MSGTYP_BUKRS_CHANGE BAdI has been implemented (203)

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

[:top:](#table-of-contents)

### How the schema source gets read

---
> **Note**: 
> * Schema source is read from cluster **PCL2, area PS**
> * See include RPCXPS00, lines 34-53, for an example.
>   * Macro **rp-imp-c2-ps** (include RPCXPS00) reads the schema source
> * Internal table **as-source** (include RPC2PS00) is where the schema source is read into
---

* RPCALCK0 - **START-OF-SELECTION**
* **PERFORM get_schema** - RPCALCK0/RPCHRT09/**START-OF-SELECTION**/Line 297/**PERFORM get_schema**(rpuscg00)
    * asdfasdf

[:top:](#table-of-contents)
