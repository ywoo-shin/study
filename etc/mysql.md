* mysql workbench dump 주의 사항

  * administration > Data Export
    * dump.sql 파일 생성 
  * administration > Data Import
    * dump.sql을 VI 편집기로 열고 다음을 주석 처리해야 함, (만약 하지 않고 Import 실행 시, access denied you need (at least one of) the SUPER privilege(s) for this operation 발생함)
      * 제일 상단
        * #SET @MYSQLDUMP_TEMP_LOG_BIN = @@SESSION.SQL_LOG_BIN;
        * #SET @@SESSION.SQL_LOG_BIN= 0;
        * #SET @@GLOBAL.GTID_PURGED=/*!80000 '+'*/ '';
    * 제일 하단 위치
        * #SET @@SESSION.SQL_LOG_BIN = @MYSQLDUMP_TEMP_LOG_BIN;
