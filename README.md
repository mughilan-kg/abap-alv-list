# abap-alv-list
abap _alv_list_display_program


REPORT ZEKKO.

TABLES : ekko,ekpo,ekbe.

types : begin of ty_n1,  
EBELN type EBELN,        
BUKRS type BUKRS,        
BSTYP type EBSTYP,        
BSART type EBSART,        
BSAKZ type BSAKZ,        
end of ty_n1.

data : itab1 type table of ty_n1,        
wa1 type ty_n1. 

types :begin of ty_n2,        
EBELN type EBELN,        
EBELP type EBELP,        
LOEKZ type ELOEK,       
STATU TYPE ASTAT,        
AEDAT TYPE PAEDT,       
MATNR TYPE MATNR,        
END OF TY_N2.

DATA : itab2 TYPE TABLE OF ty_n2,      
wa2 type ty_n2.

TYPES: begin of ty_n3,       
EBELN type EBELN,       
EBELP type EBELP,       
ZEKKN type DZEKKN,       
VGABE type VGABE,       
GJAHR type MJAHR,       
end of ty_n3.

data : itab3 type table of ty_n3,      
wa3 type ty_n3.

TYPES: BEGIN OF ty_n4,         
ebeln TYPE ebeln,         
bukrs TYPE bukrs,         
bstyp TYPE ebstyp,         
bsart TYPE ebsart,         
bsakz TYPE bsakz,         
ebelp TYPE ebelp,         
loekz TYPE eloek,         
statu TYPE astat,         
aedat TYPE paedt,         
matnr TYPE matnr,         
zekkn TYPE dzekkn,         
vgabe TYPE vgabe,         
gjahr TYPE mjahr,       
END OF ty_n4.

data : itab4 type table of ty_n4,        
wa4 type ty_n4.

SELECTION-SCREEN : begin of block e1 with frame.
SELECT-OPTIONS : s_p for ekko-ebeln,                
                s_p1 for ekbe-ebelp.  
PARAMETERs : p_ep TYPE matnr.  
SELECTION-SCREEN : end of block e1.

select EBELN BUKRS BSTYP BSART BSAKZ  from ekko  
into table itab1  
where ebeln in s_p .

select EBELN EBELP LOEKZ STATU AEDAT MATNR from ekpo
into table itab2   
for all ENTRIES IN itab1 
where ebeln = itab1-ebeln.

select EBELN EBELP ZEKKN VGABE GJAHR from ekbe
into table itab3 
for all ENTRIES IN itab1  
where ebeln = itab1-ebeln and ebelp in s_p1.  

loop at itab3 into wa3.    
read table itab2 into wa2 with key  ebeln = wa3-ebeln ebelp = wa3-ebelp.    
read table itab1 into wa1 with key  ebeln = wa3-ebeln.           

wa4-EBELN = wa1-EBELN.            
wa4-BUKRS = wa1-BUKRS.            
wa4-BSTYP = wa1-BSTYP.            
wa4-BSART = wa1-BSART.            
wa4-BSAKZ = wa1-BSAKZ.           
wa4-EBELP = wa2-EBELP.           
wa4-LOEKZ = wa2-LOEKZ.            
wa4-STATU = wa2-STATU.            
wa4-AEDAT = wa2-AEDAT.           
wa4-MATNR = wa2-MATNR.           
wa4-ZEKKN = wa3-ZEKKN.           
wa4-VGABE = wa3-VGABE.            
wa4-GJAHR = wa3-GJAHR.           
append wa4 to itab4. 

endloop.


data : it_ft type slis_t_fieldcat_alv,        
wa_ft type slis_fieldcat_alv.

wa_ft-col_pos = 1.
wa_ft-fieldname = 'EBELN'.
wa_ft-seltext_m = 'Purchasing Document Number'.
append wa_ft to it_ft.
clear wa_ft.

wa_ft-col_pos = 2.
wa_ft-fieldname = 'BUKRS'.
wa_ft-seltext_m = 'Company Code'.
append wa_ft to it_ft.
clear wa_ft.

wa_ft-col_pos = 3.
wa_ft-fieldname = 'BSTYP'.
a_ft-seltext_m = 'Purchasing Document Category'.
append wa_ft to it_ft.
clear wa_ft.

wa_ft-col_pos = 4.
wa_ft-fieldname = 'BSART'.
wa_ft-seltext_m = 'Purchasing Document Type'.
append wa_ft to it_ft.
clear wa_ft.

wa_ft-col_pos = 5.
wa_ft-fieldname = 'BSAKZ'.
wa_ft-seltext_m = 'Control indicator for purchasing document type'.
append wa_ft to it_ft.
clear wa_ft.

wa_ft-col_pos = 6.
wa_ft-fieldname = 'EBELP'.
wa_ft-seltext_m = 'Item Number of Purchasing Document'.
append wa_ft to it_ft.
clear wa_ft.

wa_ft-col_pos = 7.
wa_ft-fieldname = 'LOEKZ'.
wa_ft-seltext_m = 'Deletion indicator in purchasing document'.
append wa_ft to it_ft.
clear wa_ft.

wa_ft-col_pos = 8.
wa_ft-fieldname = 'STATU'.
wa_ft-seltext_m = 'RFQ status'.
append wa_ft to it_ft.
clear wa_ft.

wa_ft-col_pos = 9.
wa_ft-fieldname = 'AEDAT'.
wa_ft-seltext_m = 'Purchasing Document Item Change Date'.
append wa_ft to it_ft.
clear wa_ft.

wa_ft-col_pos = 10.
wa_ft-fieldname = 'MATNR'.
wa_ft-seltext_m = 'Material Number'.
append wa_ft to it_ft.
clear wa_ft.

wa_ft-col_pos = 11.
wa_ft-fieldname = 'ZEKKN'.
wa_ft-seltext_m = 'Sequential Number of Account Assignment'.
append wa_ft to it_ft.
clear wa_ft.

wa_ft-col_pos = 12.
wa_ft-fieldname = 'VGABE'.
wa_ft-seltext_m = 'Transaction/event type, purchase order history'.
append wa_ft to it_ft.
clear wa_ft.

wa_ft-col_pos = 13.
wa_ft-fieldname = 'GJAHR'.
wa_ft-seltext_m = 'Material Document Year'.
append wa_ft to it_ft.
clear wa_ft.  


CALL FUNCTION 'REUSE_ALV_LIST_DISPLAY'   
EXPORTING
*     I_INTERFACE_CHECK              = ' '
*     I_BYPASSING_BUFFER             =
*      I_BUFFER_ACTIVE                = ' '
   I_CALLBACK_PROGRAM            = 'ZEKKO'
*     I_CALLBACK_PF_STATUS_SET       = ' '
* *     I_CALLBACK_USER_COMMAND        = ' '
* *     I_STRUCTURE_NAME               =
* *     IS_LAYOUT                      =
 IT_FIELDCAT                    = it_ft
* *     IT_EXCLUDING                   =
* *     IT_SPECIAL_GROUPS              =
* *     IT_SORT                        =
* *     IT_FILTER                      =
* *     IS_SEL_HIDE                    =
* *     I_DEFAULT                      = 'X'
* *     I_SAVE                         = ' '
* *     IS_VARIANT                     =
* *     IT_EVENTS                      =
* *     IT_EVENT_EXIT                  =
* *     IS_PRINT                       =*
*      IS_REPREP_ID                   =
*      I_SCREEN_START_COLUMN          = 0
*      I_SCREEN_START_LINE            = 0
*     I_SCREEN_END_COLUMN            = 0
* *     I_SCREEN_END_LINE              = 0
  * *     IR_SALV_LIST_ADAPTER           =
    * *     IT_EXCEPT_QINFO                =
  *     I_SUPPRESS_EMPTY_DATA          = ABAP_FALSE
*   IMPORTING
*     E_EXIT_CAUSED_BY_CALLER        =
*     ES_EXIT_CAUSED_BY_USER         =    TABLES
 T_OUTTAB                       = itab4.
* EXCEPTIONS
* PROGRAM_ERROR                  = 1
* OTHERS                         = 2            .
 
 
    IF SY-SUBRC <> 0.
   message ' enter atleast one validation' type 'e'.
   ENDIF.

