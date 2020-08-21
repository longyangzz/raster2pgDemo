//! 创建一个新表，导入tif到指定库中
raster2pgsql -s 4326 -C D:\testtaifeng\pgorhdftest\tongluwgs84_Clip11.tif rgbtifTable | psql -h localhost -p 5432 -U postgres -d postgis_30_sample -W

//！ 从pg中复制写出
# -*- coding: utf-8 -*-
 
import psycopg2
 
# Connect to an existing database
conn = psycopg2.connect('host=localhost port=5432 user=postgres password=123456 dbname=postgis_30_sample')
 
# Open a cursor to perform database operations
cur = conn.cursor()
 
# Execute SQL query
# cur.execute("SELECT ST_AsTIFF(rast, 'LZW') AS rasttiff FROM staging.wsiearth WHERE rid=1;")
cur.execute("SELECT ST_AsTIFF(ST_Union(rast), 'LZW') AS rasttiff FROM public.rgbtifTable;")
 
# Fetch data as Python objects
rasttiff = cur.fetchone()
 
# Write data to file
if rasttiff is not None:
    open('D:\\testtaifeng\\pgorhdftest\\tmean1.tif', 'wb').write((rasttiff[0]))
 
# Close communication with the database
cur.close()
conn.close()


select * from information_schema.columns
where table_name='rgbtifTable'

SELECT short_name 
 FROM ST_GDALDrivers();
