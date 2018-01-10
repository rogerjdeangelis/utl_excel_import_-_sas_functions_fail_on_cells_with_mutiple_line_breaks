# utl_excel_import_-_sas_functions_fail_on_cells_with_mutiple_line_breaks
Excel_import_:_sas_functions_fail_on_cells_with_mutiple_line_breaks  Keywords: sas sql join merge big data analytics macros oracle teradata mysql sas communities stackoverflow statistics artificial inteligence AI Python R Java Javascript WPS Matlab SPSS Scala Perl C C# Excel MS Access JSON graphics maps NLP natural language processing machine learning igraph DOSUBL DOW loop stackoverflow SAS community.
    Excel_import_:_sas_functions_fail_on_cells_with_mutiple_line_breaks

    Bug or feature?
    Same result in SAS and WPS

    Problem: Does the excel cell, CELL_WITH_BREAKS, contain the text VANILLA.
    Also fails with input SAS datasets with '0A'x separators

    github
    https://goo.gl/T7FF5q
    https://github.com/rogerjdeangelis/utl_excel_import_-_sas_functions_fail_on_cells_with_mutiple_line_breaks

    see
    https://goo.gl/82Fn7T
    https://communities.sas.com/t5/Base-SAS-Programming/Findw-does-not-find-after-line-breaks/m-p/426445


    INPUT

      d:/xls/utl_excel_handling_line_breaks_within_a_single_cell.xlsx

          +------------------------------------+
          |                A                   |
          +------------------------------------+
       1  |       CELL_WITH_BREAKS             |
          +------------------------------------+
          | CHOCOLATE  PRESENT MANY (>30):     |
       2  | VANILLA  PRESENT MANY (>30):       |
          | CARAMEL PRESENT MANY (>30):        |
          +------------------------------------+

    [HAVE]


    PROBLEM CODE
    ============

       libname xel "d:/xls/utl_excel_handling_line_breaks_within_a_single_cell.xlsx";
       data want;
         retain find_vanilla;
         set xel.have;
         find_vanilla=findw(CELL_WITH_BREAKS,'VANILLA');
       run;quit;
       libname xel clear;

       Also fails with a imput SAS dataset;

       data want_dataset;
          retain find_vanilla;
          set have;
          find_vanilla=findw(CELL_WITH_BREAKS,'VANILLA');
       run;quit;


    OUTPUT  FIND_VANILLA=0 means the text was not found
    ====================================================


    WORK.WANT_DATASET total obs=1

      FIND_
     VANILLA                                       CELL_WITH_BREAKS

        0       CHOCOLATE  PRESENT MANY (>30):
VANILLA  PRESENT MANY (>30):
CARAMEL PRESENT MANY (>30):

    *                _              _       _
     _ __ ___   __ _| | _____    __| | __ _| |_ __ _
    | '_ ` _ \ / _` | |/ / _ \  / _` |/ _` | __/ _` |
    | | | | | | (_| |   <  __/ | (_| | (_| | || (_| |
    |_| |_| |_|\__,_|_|\_\___|  \__,_|\__,_|\__\__,_|

    ;

    %utlfkil(d:/xls/utl_excel_handling_line_breaks_within_a_single_cell.xlsx);
    libname xel "d:/xls/utl_excel_handling_line_breaks_within_a_single_cell.xlsx";
    data xel.have have;

      cell_with_breaks=
         catx('0A'x,  "CHOCOLATE  PRESENT MANY (>30):"
                     ,"VANILLA  PRESENT MANY (>30):"
                     ,"CARAMEL PRESENT MANY (>30):");
    run;quit;
    libname xel clear;

    *          _       _   _
     ___  ___ | |_   _| |_(_) ___  _ __  ___
    / __|/ _ \| | | | | __| |/ _ \| '_ \/ __|
    \__ \ (_) | | |_| | |_| | (_) | | | \__ \
    |___/\___/|_|\__,_|\__|_|\___/|_| |_|___/

    ;

    libname xel "d:/xls/utl_excel_handling_line_breaks_within_a_single_cell.xlsx";
    data want;
      retain find_vanilla;
      set xel.have;
      find_vanilla=findw(CELL_WITH_BREAKS,'VANILLA');
    run;quit;
    libname xel clear;


    %utl_submit_wps64('
    libname wrk sas7bdat "%sysfunc(pathname(work))";
       data want_dataset;
          retain find_vanilla;
          set wrk.have;
          find_vanilla=findw(CELL_WITH_BREAKS,"VANILLA");
          put  find_vanilla=;
       run;quit;
    run;quit;
    ');
