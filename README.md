# textfile
test

DECLARE
  -- Declare variables for cursor and fetch
  l_sql_query VARCHAR2(4000);
  l_column_names VARCHAR2(4000);
BEGIN
  -- Loop through each table
  FOR t IN (SELECT table_name FROM user_tables) LOOP
    -- Generate SQL query dynamically for the current table
    l_sql_query := 'SELECT ';

    -- Reset column names for each table
    l_column_names := '';

    -- Retrieve column names for the current table
    FOR c IN (SELECT column_name FROM user_tab_columns WHERE table_name = t.table_name) LOOP
      -- Concatenate column names
      l_column_names := l_column_names || c.column_name || ', ';
    END LOOP;

    -- Remove trailing comma and space
    l_column_names := RTRIM(l_column_names, ', ');

    -- Concatenate column names into SQL query
    l_sql_query := l_sql_query || l_column_names || ' FROM ' || t.table_name;

    -- Execute dynamic SQL query
    EXECUTE IMMEDIATE l_sql_query;
  END LOOP;
END;
/
